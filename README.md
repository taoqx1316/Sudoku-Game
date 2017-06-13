# Sudoku-Game
untitled4.pro
#-------------------------------------------------
#
# Project created by QtCreator 2017-06-08T19:10:56
#
#-------------------------------------------------

QT       += core gui
QT += multimedia multimediawidgets
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

TARGET = untitled4
TEMPLATE = app

# The following define makes your compiler emit warnings if you use
# any feature of Qt which as been marked as deprecated (the exact warnings
# depend on your compiler). Please consult the documentation of the
# deprecated API in order to know how to port your code away from it.
DEFINES += QT_DEPRECATED_WARNINGS

# You can also make your code fail to compile if you use deprecated APIs.
# In order to do so, uncomment the following line.
# You can also select to disable deprecated APIs only up to a certain version of Qt.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0


SOURCES += main.cpp\
        mainwindow.cpp \
    game.cpp

HEADERS  += mainwindow.h \
    game.h

FORMS    += mainwindow.ui \
    game.ui

RESOURCES += \
    image/picture.qrc


game.h
#ifndef GAME_H
#define GAME_H
//#include<qdialog.h>
#include<iomanip>
#include<process.h>
#include<stdio.h>
#include<iostream>
#include<stdlib.h>
#include<windows.h>
#include<fstream>
#include <QWidget>
#include <QApplication>
#include <QTime>
#include <QSplashScreen>
#include <QDebug>
#include <QElapsedTimer>
#include <QDateTime>
#include<QPushButton>
#include <qicon.h>
#include<qsize.h>
#include<qevent.h>
#include<qpoint.h>
#include<iostream>
#include<qpixmap.h>
#include <QMediaPlayer>
#include<QMediaPlaylist>
#include<QUrl>
#include<QVideoWidget>
#include<ctime>
#include<Qpainter>
#include<QComboBox>
#include <QtGui>
#include <QMessageBox>
using namespace std ;
namespace Ui {
class game;
}

class game : public QWidget
{
    Q_OBJECT

public:
    explicit game(QWidget *parent = 0);
    ~game();
    QPushButton *shudu[9][9] ;
    void mousePressEvent(QMouseEvent* placelocation);
    void removenumber(int Math[9][9],int number) ;
    void exchangenumber(int m[9][9],int x,int y) ;
    bool check(int a[9][9]) ;
    bool full(int h[9][9]) ;
   void game::paintEvent(QPaintEvent *event);
   void boxChange(int j) ;

public slots:
   void boxChange();
private:
    Ui::game *ui;
    int SudoKu[9][9] ;
    int boxnumber ;
    QComboBox* Box ;
    int doingx1 ;
    int doingy1 ;
    int flag[9][9] ;

};

#endif // GAME_H

mainwindow.h

#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include"game.h"
namespace Ui {
class MainWindow;
}

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = 0);
    ~MainWindow();

private slots://菜单按钮


    void on_pushButton_3_clicked();

    void on_pushButton_clicked();

private:
    Ui::MainWindow *ui;
    QMediaPlaylist*  playlist;
    QMediaPlayer* player;

};

#endif // MAINWINDOW_H

game.cpp
#include "game.h"
#include "ui_game.h"

game::game(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::game)
{

    ui->setupUi(this);
    QPixmap pix(":/one.png") ;

     this->setWindowTitle("shuDO Game");
     //this->setWindowIcon(QIcon(pixmap,100,100));
    this->setFixedWidth(1000);//画窗口
    this->setFixedHeight(900);

   /* for(int i=0;i<=8;i++)
    {
        for(int j=0 ;j<=8;j++)
        {
        shudu[i][j]=new QPushButton(this) ;
        shudu[i][j]->setIcon(QIcon(pix));

        shudu[i][j]->setIconSize(*buttonsize);

        shudu[i][j]->setGeometry((i*100),(j*100),100,100);

        }

     }*/
    ifstream read("C:\\Users\\27254\\Desktop\\ShuDu.txt")	;
    srand((unsigned)time(0));
        int i=rand()%5 ; //随机选取种子矩阵
        int line=0 ;
        if(i>0)
        {
            line=i*9 ;
        }
        //cout<<i<<endl ;
        int count=1 ;
        char tempstring[20] ;
        char storenumber[20] ;
        while(count<=line)
        {
            read.getline(tempstring,20) ;
            count=count+1 ;
        }
        for(int m=0;m<=8;m++)
        {
            read.getline(storenumber,20) ;//读入一行
            //cout<<storenumber<<endl ;
            for(int j=0;j<=8;j++)
            {
            this->SudoKu[m][j]=int(storenumber[j*2]-'0') ;
            }

        }
        srand((unsigned)time(0));

            exchangenumber(this->SudoKu,(rand()%8)+1,(rand()%8)+1) ;
            exchangenumber(this->SudoKu,(rand()%8)+1,(rand()%8)+1)  ;
            exchangenumber(this->SudoKu,(rand()%8)+1,(rand()%8)+1)  ;
            exchangenumber(this->SudoKu,(rand()%8)+1,(rand()%8)+1)   ;
            exchangenumber(this->SudoKu,(rand()%8)+1,(rand()%8)+1)    ;
            for(int i=0;i<=8;i++)
            {
                for(int j=0;j<=8;j++)
                    {
                    cout<<SudoKu[i][j]<<" " ;

                    }
                   cout<<endl ;

            }
            this->removenumber(this->SudoKu,67);//挖空
            for(int m=0;m<=8;m++)
            {
                for(int n=0;n<=8;n++)
                {
                    if(SudoKu[m][n]!=10)
                    {
                        this->flag[m][n]=1 ;//一开始有的设置标志位为1
                    }
                }
            }
            //
                Box=new QComboBox(this) ;//下拉菜单
            Box->addItem(QWidget::tr("1"));
            Box->addItem(QWidget::tr("2"));
            Box->addItem(QWidget::tr("3"));
            Box->addItem(QWidget::tr("4"));
            Box->addItem(QWidget::tr("5"));
            Box->addItem(QWidget::tr("6"));
            Box->addItem(QWidget::tr("7"));
            Box->addItem(QWidget::tr("8"));
            Box->addItem(QWidget::tr("9"));
            boxnumber=0 ;
            Box->setVisible(false);
            doingx1=0 ;
            doingy1=0 ;
            connect(Box, SIGNAL(currentIndexChanged(int)), this, SLOT(boxChange()));


}

bool game::check(int a[9][9])//检查是否正确
{
        bool result = true;
        for (int j1 = 0; j1 <= 8; j1++)
        {
            for (int m1 = 0; m1 <= 8; m1++)
            {
                int temp = 0;
                temp = a[j1][m1];
                //
                if (temp >= 1 && temp <= 9)
                {
                    for (int u1 = 0; u1 <=m1-1; u1++)
                    {
                        if (temp == a[j1][u1])
                        {
                            result = false;
                            return result;
                        }
                    }
                    for (int u2 =m1+1; u2 <=8; u2++)
                    {
                        if (temp == a[j1][u2])
                        {
                            result = false;
                            return result;
                        }
                    }
                    for (int y1 = 0; y1 <= j1-1; y1++)
                    {
                        if (temp == a[y1][m1])
                        {
                            result = false;
                            return result;
                        }
                    }
                    for (int y2 = j1+1; y2 <= 8; y2++)
                    {
                        if (temp == a[y2][m1])
                        {
                            result = false;
                            return result;
                        }
                    }
                }
            }

        }
        return result;
}

void game::exchangenumber(int m[9][9],int x,int y)
{
    int place1=0 ;
    int place2=0 ;
    for(int i=0;i<=8;i++)
    {
        for(int j=0;j<=8;j++)
        {
            if(m[i][j]==x)
            {
                place1=j ;
            }
            if(m[i][j]==y)
            {
                place2=j ;
            }
        }
        int temp=m[i][place1] ;
        m[i][place1]=m[i][place2] ;
        m[i][place2]=temp ;
    }
}
void game::removenumber(int Math[9][9],int number)
{
    srand((unsigned)time(0));
    int beginnumber[200] ;
        for (int i = 0; i <= (number*2)-1; i++)
        {

            beginnumber[i]=rand()%9;

        }

        int p1=0 ;
        for (int t = 1; t <= number; t++)
        {

            Math[beginnumber[p1]][beginnumber[p1 + 1]]=10;
            p1 = p1 + 2;

        }
}
void game::paintEvent(QPaintEvent *event)//把数改成图
{
    Q_UNUSED(event);


       QPainter painter(this);
       for (int i=0; i<9; i++)
       {
           for (int j=0;j<9;j++)
         {
         if(this->SudoKu[i][j]==1)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/one.png"));
         }
         if(this->SudoKu[i][j]==2)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/two.png"));
         }
         if(this->SudoKu[i][j]==3)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/three.png"));
         }
         if(this->SudoKu[i][j]==4)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/four.png"));
         }
         if(this->SudoKu[i][j]==5)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/five.png"));
         }
         if(this->SudoKu[i][j]==6)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/six.png"));
         }
         if(this->SudoKu[i][j]==7)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/seven.png"));
         }
         if(this->SudoKu[i][j]==8)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/eight.png"));
         }
         if(this->SudoKu[i][j]==9)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/nine.png"));
         }
         if(this->SudoKu[i][j]==10)
         {
          painter.drawPixmap(0+100*j,0+100*i,100,100,QPixmap(":/zero.png"));
         }
         }
       }

}

game::~game()
{
    delete ui;
}

void game::boxChange()//下拉菜单
{
    if(flag[doingy1][doingx1]==1)
    {
      Box->setVisible(false);
     }
    else
     {
     int temp=SudoKu[doingy1][doingx1] ;//获取当前选择的数字
     this->SudoKu[doingy1][doingx1]=(Box->currentIndex()+1) ;
     if(check(SudoKu)==false)
     {
         QMessageBox::information(this, "Wrong warning", "Your behaviour is wrong!", QMessageBox::Ok);
         SudoKu[doingy1][doingx1]=temp ;
          this->update();
     }
     if(check(SudoKu)==true)
     {
         this->update();
     }
     this->Box->setVisible(false);
     }

}

void game::mousePressEvent(QMouseEvent* placelocation)//鼠标点击事件
{
     QPoint *point=new QPoint() ;
     *point=placelocation->pos() ;
      int x1=point->x() ;
      int y1=point->y(); //获取鼠标点击时鼠标的坐标
      doingx1=(x1/100) ;
      doingy1=(y1/100) ;//得到数独数组x,y的坐标，即suduku[y1][x1]

        this->Box->setGeometry(100*(doingx1+1),50+100*(doingy1),100,20);
        this->Box->setVisible(true);
}


main.cpp
#include "mainwindow.h"
#include <QApplication>
#include <QTime>
#include <QSplashScreen>
#include <QDebug>
#include <QElapsedTimer>
#include <QDateTime>
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QPixmap pix(":/new/prefix1/huhaomingzhenshuai.png") ;
   QSplashScreen splash(pix);
    splash.resize(pix.size());
   splash.show();
    app.processEvents();

       MainWindow w;
       w.show();

       //splash.finish(&w);
       return app.exec();
}

mainwindow.cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    player=new QMediaPlayer(this);

    playlist = new QMediaPlaylist;
    playlist->setPlaybackMode(QMediaPlaylist::CurrentItemInLoop);
    playlist->addMedia(QUrl("C://Music//14.mp3"));
    playlist->addMedia(QUrl("C://Music//13.mp3"));
    playlist->setCurrentIndex(2);
    player = new QMediaPlayer(this);
    player->setPlaylist(playlist);
    player->play();
}

MainWindow::~MainWindow()
{
    delete ui;
}


void MainWindow::on_pushButton_3_clicked()
{
    this->close() ;
}

void MainWindow::on_pushButton_clicked()
{

    game* shudo=new game ;


    shudo->show();


}
