@[TOC](目录)
# 一.简介
## 1、简介
这是本人大二学期专业课程设计所设计的一个很简易的音乐播放器（功能有限，好多地方还有不足），如今大四即将毕业，难忘那时为了课程设计而苦学Qt的激情，心中不禁回忆满满，“大学才四年，疫情占三年”，希望同学们都能珍惜大学校园生活，珍惜青春。在此特将源码和课程设计报告书分享出来，供需要的同学参考。
## 2、界面部分截图
（1）从本地添加音乐界面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffc0d4246de34a81b483d2687e9bc454.png#pic_center)
（2）在线搜索界面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b0eab6bb2aed4b67bd067810b24bebae.png#pic_center)
（3）历史播放界面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffb408c4027740fbbe3c0359023ca08b.png#pic_center)
## 3、功能描述
实现的功能有：
（1）自定义背景图片
（2）在线搜索
（3）播放本地音乐
（4）播放历史
（5）基本播放控制功能
（6）数据库的记录
# 二.代码
## 1、工程目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/573d437950ad462388644191eb3f97b8.png#pic_center)
## 2、代码展示
（1）mainwidget.h代码：

```cpp
#ifndef MAINWIDGET_H
#define MAINWIDGET_H

#include <QWidget>
#include <QPainter>
#include<QMenu>
#include<QtMultimedia/QMediaPlayer>
#include<QtMultimedia/QMediaPlaylist>
#include<QListWidgetItem>
#include <QNetworkAccessManager>

QT_BEGIN_NAMESPACE
namespace Ui { class mainWidget; }
QT_END_NAMESPACE

class mainWidget : public QWidget
{
    Q_OBJECT

public:
    mainWidget(QWidget *parent = nullptr);
    ~mainWidget();

private slots:
    void on_btn_Title_clicked();

    void on_btn_About_clicked();

    void on_btn_Skin_clicked();

    void on_btn_Min_clicked();

    void on_btn_Close_clicked();

    void on_btn_CurrentMusic_clicked();

    void on_btn_LocalMusic_clicked();

    void on_btn_FavorMusic_clicked();

    void on_btn_Pre_clicked();

    void on_btn_play_clicked();

    void on_btn_Next_clicked();

    void on_btn_PlayMode_clicked();

    void on_btn_AddMusic_clicked();//添加音频文件

    void on_btn_Voice_clicked();

    void background_to_default();//切换到默认皮肤
    void background_setting();//切换到自定义皮肤

    void on_listWidget_localMusic_itemDoubleClicked(QListWidgetItem *item);//播放列表双击来播放音乐
    void onDurationChanged(qint64 duration); //文件时长变化，更新当前播放文件名显示；duration为播放文件数据总大小的信号， 它可以获得文件时间长度。
    void onPositionChanged(qint64 position); //当前文件播放位置变化，更新进度显示；position为播放到什么位置的信号， 参数是以毫秒来计算的。
    void on_PositionSlider_valueChanged(int value);

    void on_VolumeSlider_valueChanged(int value);

    void on_PositionSlider_sliderReleased();
    void initSearch();//初始化在线搜索
    void search();//在线搜索
    void on_tableWidget_cellDoubleClicked(int row, int column);//双击播放在线搜索中歌曲播放歌曲

private:
    Ui::mainWidget *ui;
    QMenu *menu_changeSkin;     //更换皮肤的菜单
    QMediaPlayer * player;
    QMediaPlaylist * playlist;
    QString durationTime;
    QString positionTime;
    int M_Amount=0;//用来记录文件总数
    int M_Value=0;//当前播放的文件标记
    void init_Actions();//初始化菜单项
    void init_Settings();//初始化配置
    void init_Sql();//数据库初始化
    void insertSql(const QString& name);//存入数据库
    void read_fromSQL();//从数据中恢复本歌单
    void paintEvent(QPaintEvent *event) override;//绘制重写事件
    QNetworkAccessManager *NAM;

    //歌曲资源地址
    QUrl url;
    //歌手
    QString author;
    //歌曲名
    QString title;
    //时长
    qint64 duration;
    //唱片集
    QString albumTitle;
    //比特率
    int audioBitRate;
};
#endif // MAINWIDGET_H

```
（2）myQss.h代码：

```cpp
#ifndef MYQSS_H
#define MYQSS_H
#include<QString>
#endif // MYQSS_H

//该文件是一些控件的样式表
inline QString LoopStyle()
{
return "QPushButton"
" {"
"background-image:url(:/image/image/loop.png);"
"   background-repeat:no-repeat;"
"  background-position:center center;"
"   border:none;"

"    }"

"QPushButton:hover{"
"  background-repeat:no-repeat;"
"   background-position:center center;"
"background-image:url(:/image/image-hover/loop-hover.png);"

" }"
" QPushButton:pressed{"
"background-repeat:no-repeat;"
" background-position:center center;"
" background-image:url(:/image/image/loop.png);"

 "}";
}

inline QString LoopOneStyle()
{


    return "QPushButton"
   " {"
   "background-image:url(:/image/image/loop-one.png);"
 "   background-repeat:no-repeat;"
  "  background-position:center center;"
 "   border:none;"

"    }"

    "QPushButton:hover{"
  "  background-repeat:no-repeat;"
 "   background-position:center center;"
 "background-image:url(:/image/image-hover/loop-one-hover.png);"

   " }"
   " QPushButton:pressed{"
    "background-repeat:no-repeat;"
   " background-position:center center;"
   " background-image:url(:/image/image/loop-one.png);"

     "}";

}

inline QString RandomStyle()
{

    return "QPushButton"
   " {"
   "background-image:url(:/image/image/play-random.png);"
 "   background-repeat:no-repeat;"
  "  background-position:center center;"
 "   border:none;"

"    }"

    "QPushButton:hover{"
  "  background-repeat:no-repeat;"
 "   background-position:center center;"
 "background-image:url(:/image/image-hover/play-random-hover.png);"

   " }"
   " QPushButton:pressed{"
    "background-repeat:no-repeat;"
   " background-position:center center;"
   " background-image:url(:/image/image/play-random.png);"

     "}";

}

```
（3）main.cpp代码:

```cpp
#include "mainwidget.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    mainWidget w;
    w.show();
    return a.exec();
}

```
（4）mainwidget.cpp代码:

```cpp
#include "mainwidget.h"
#include "ui_mainwidget.h"
#include <QStyleOption>
#include<QFileDialog>
#include <QMessageBox>
#include<QAction>
#include<QStandardPaths>
#include<QSettings>
#include<QDebug>
#include<QUrl>
#include"myQss.h"
#include<QNetworkReply>
#include <QNetworkRequest>
#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonArray>
#include<QTableWidgetItem>
#include <QEventLoop>
#include<QByteArray>
#include<QSqlDatabase>
#include<QSqlQuery>
#include<QtSql>

mainWidget::mainWidget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::mainWidget)
{
    ui->setupUi(this);
    this->setWindowTitle("小何音乐播放器");
    init_Actions();
    init_Settings();
}

mainWidget::~mainWidget()
{
    delete ui;
}

void mainWidget::init_Actions()//初始化
{
    //“换肤”的菜单项
    QAction *action_backgroud_to_default = new QAction(QIcon(":/image/image/image/default.png"),u8"更换到默认背景");
    connect(action_backgroud_to_default,&QAction::triggered,this,&mainWidget::background_to_default);
    QAction *action_backgroud_setting=new QAction(QIcon(":/image/image/image/setting.png"),u8"自定义背景");
    connect(action_backgroud_setting,&QAction::triggered,this,&mainWidget::background_setting);
    menu_changeSkin=new QMenu(this);
    menu_changeSkin->addAction(action_backgroud_to_default);
    menu_changeSkin->addAction(action_backgroud_setting);

}

void mainWidget::init_Settings()//初始化相关设置
{
    //1.皮肤初始化
    QSettings mysettings("./LightMusicPlayer.ini",QSettings::IniFormat);
    mysettings.setIniCodec("UTF8");
    QString fileName = mysettings.value("background/image-url").toString();
    QImage testImage(fileName);
     qDebug() << fileName;
    if(!fileName.isEmpty()&&!testImage.isNull())
    {
         setStyleSheet(QString("QWidget#mainWidget{"
                               "border-radius:10px;"
                               "border-image: url(%1);}").arg(fileName));
    }else{
        fileName=":/image/background/MenuSceneBg.png";
        QSettings mysettings("./LightMusicPlayer.ini",QSettings::IniFormat);
        mysettings.setIniCodec("UTF8");
        mysettings.setValue("background/image-url",fileName);
        setStyleSheet(QString("QWidget#mainWidget{"
                              "border-radius:10px;"
                              "border-image: url(%1);}").arg(fileName));
    }
    //2.ui中stackedwidget默认开启页面设置为1
    ui->stackedWidget->setCurrentIndex(1);
    //3.播放器初始化
      player = new QMediaPlayer(this);
      playlist = new QMediaPlaylist(this);
      playlist->setPlaybackMode(QMediaPlaylist::Loop);//设置循环模式
      player->setPlaylist(playlist);//获取将播放列表要播放的文件
     //4.进度条和歌曲时间显示变化
      connect(player,SIGNAL(positionChanged(qint64)),this,SLOT(onPositionChanged(qint64)));
      connect(player,SIGNAL(durationChanged(qint64)),this,SLOT(onDurationChanged(qint64)));
      //5.主界面音量滑动条默认显示为隐藏
      ui->VolumeSlider->hide();
      //6.窗口设置圆角后，会出现留白，需要添加以下代码
      setAttribute(Qt::WA_TranslucentBackground, true);
      //7.在线搜索
       //ui->lineEdit_page->setValidator(new QIntValidator(1,50));
       connect(ui->lineEdit,SIGNAL(returnPressed()),this,SLOT(initSearch()));
       //8.初始化数据库
       init_Sql();
       //9.
       read_fromSQL();
       //10


}

void mainWidget::init_Sql()//初始化数据库
{
    QSqlDatabase database;
    if (QSqlDatabase::contains("qt_sql_default_connection"))
    {
        database = QSqlDatabase::database("qt_sql_default_connection");
    }
    else
    {
        database = QSqlDatabase::addDatabase("QSQLITE");
        database.setDatabaseName("Xiaohe.db");
        database.setUserName("Xiaohe");
        database.setPassword("Xiaohe");
        if (!database.open())
        {
            QMessageBox::critical(this,"无法打开数据库文件：Xiaohe.db",database.lastError().databaseText());
            exit(-1);
        }
    }

       //检查数据库是否存在,不存在则创建不存在的表
       QSqlQuery query;
       query.exec(QString("select count(*) from sqlite_master where type='table' and name='%1'").arg("MusicList"));//执行qsl语句
       if(query.next())
          {
            if(query.value(0).toInt()==0)
               {
                 QSqlQuery sql_query;
                 QString create_sql = "create table Musiclist (name varchar(30))";
                 sql_query.prepare(create_sql);
                 sql_query.exec();
               }
          }
}

void mainWidget::insertSql(const QString &name)//插入数据库
{
    QSqlQuery sql_query;
    QString insert_sql = "insert into Musiclist values (?)";
    sql_query.prepare(insert_sql);
    sql_query.addBindValue(name);
    //sql_query.exec();
    if(!sql_query.exec())
           {
             qDebug() << "插入失败: "<<sql_query.lastError();
           }
    else
           {
              qDebug() << "插入成功!";
    }
}

void mainWidget::read_fromSQL()//从数据库中读取
{
    QSqlQuery sql_query;
    QString select_sql = "select *from Musiclist";
    sql_query.prepare(select_sql);
    qDebug()<<"读取成功1";
    if(sql_query.exec())
    {
        while(sql_query.next())
        {
            QString resource= (sql_query.value(0).toString());
            qDebug()<<"读取成功1后"<<resource;
            ui->listWidget_localMusic->addItem(resource);
            qDebug()<<"读取成功2";
        }
    }
}

void mainWidget::paintEvent(QPaintEvent *event)//重写事件，为了是背景图在主界面显示
{
    QStyleOption opt;
    opt.init(this);
    QPainter p(this);
    style()->drawPrimitive(QStyle::PE_Widget, &opt, &p, this);
    QWidget::paintEvent(event);
}



void mainWidget::on_btn_Title_clicked()
{
    QMessageBox::about(this,"关于",
                       "小何播放器 | 一款精致小巧又清新的音乐播放器\n"
                       "著作人：小何同学\n"
                       "版本号：1.0\n"
                       "希望小何播放器能给您带来享受音乐的快乐\n"
                       );
}

void mainWidget::on_btn_About_clicked()
{
    QMessageBox::about(this,"关于",
                       "小何播放器 | 一款精致小巧又清新的音乐播放器\n"
                       "著作人：小何同学\n"
                       "版本号：1.0\n"
                       "希望小何播放器能给您带来享受音乐的快乐\n"
                       );
}

void mainWidget::on_btn_Skin_clicked()
{
    menu_changeSkin->exec(QCursor::pos());
}

void mainWidget::on_btn_Min_clicked()
{
    showMinimized();
}

void mainWidget::on_btn_Close_clicked()
{
    this->close();
}

void mainWidget::on_btn_CurrentMusic_clicked()
{
    ui->stackedWidget->setCurrentIndex(0);
}

void mainWidget::on_btn_LocalMusic_clicked()
{
     ui->stackedWidget->setCurrentIndex(1);
}

void mainWidget::on_btn_FavorMusic_clicked()
{
     ui->stackedWidget->setCurrentIndex(2);
}

void mainWidget::on_btn_Pre_clicked()
{
    if(M_Value == 0){
           M_Value =playlist->mediaCount()-1;
       }
       else{
           M_Value--;
       }
       QListWidgetItem *item = ui->listWidget_localMusic->item(M_Value);
       item->setSelected(true);
       player->stop();
       player->setMedia(QUrl::fromLocalFile(item->text()));
       if(player->state() == QMediaPlayer::StoppedState)
            {
                 ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));
            }
       player->play();
       ui->label_Position->setText(item->text());//显示歌曲信息
       ui->label_Motto->setText(item->text());//显示歌曲信息
}

void mainWidget::on_btn_play_clicked()
{
    if(player->state() == QMediaPlayer::PlayingState)
    {
        player->pause();
        ui->btn_play->setIcon(QIcon(":/image/image-hover/pase-hover.png"));
    }
    else if(player->state() == QMediaPlayer::PausedState)
    {
        player->play();

         ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));

    }

}

void mainWidget::on_btn_Next_clicked()
{
       if(M_Value ==playlist->mediaCount()-1)
           {
             M_Value =0;
            }
       else{
             M_Value++;
            }
       QListWidgetItem *item = ui->listWidget_localMusic->item(M_Value);
       item->setSelected(true);
       player->stop();
       player->setMedia(QUrl::fromLocalFile(item->text()));
       if(player->state() == QMediaPlayer::StoppedState)
            {
                 ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));
            }
       player->play();
       ui->label_Position->setText(item->text());//显示歌曲信息
       ui->label_Motto->setText(item->text());//显示歌曲信息
}

void mainWidget::on_btn_PlayMode_clicked()
{
    QAction *random=new QAction(QIcon(":/image/image/random2.png"),"随机播放");
    QAction *itemloop=new QAction(QIcon(":/image/image/loop-one2.png"),"单曲循环");
    QAction *loop=new QAction(QIcon(":/image/image/loop2.png"),"循环播放");

//随机播放
connect(random,&QAction::triggered,this,[=](){
             ui->btn_PlayMode->setStyleSheet(RandomStyle());
             ui->btn_PlayMode->setIcon(QIcon(":/image/image/play-random.png"));
             ui->btn_PlayMode->setToolTip(u8"随机播放");
             playlist->setPlaybackMode(QMediaPlaylist::Random);

});


//单曲循环
connect(itemloop,&QAction::triggered,this,[=](){
             qDebug()<<"ok1";
             ui->btn_PlayMode->setStyleSheet(LoopOneStyle());
             ui->btn_PlayMode->setIcon(QIcon(":/image/image/loop-one.png"));
             ui->btn_PlayMode->setToolTip(u8"单曲循环");
             playlist->setPlaybackMode(QMediaPlaylist::CurrentItemInLoop);

});


//循环播放
connect(loop,&QAction::triggered,this,[=](){
             ui->btn_PlayMode->setStyleSheet(LoopStyle());
             ui->btn_PlayMode->setIcon(QIcon(":/image/image/loop.png"));
             ui->btn_PlayMode->setToolTip(u8"循环播放");
             playlist->setPlaybackMode(QMediaPlaylist::Loop);

});
QMenu *mode=new QMenu(this);
mode->addAction(random);
mode->addAction(itemloop);
mode->addAction(loop);
mode->exec(QCursor::pos());
}

void mainWidget::on_btn_AddMusic_clicked()//添加本地音乐
{
       QString filter="音频文件(*.mp3 *.wav *.wma)mp3文件(*.mp3);;wav文件(*.wav);;wma文件(*.wma);;所有文件(*.*)";
       QStringList fileList = QFileDialog::getOpenFileNames(this,"选择音频文件","D:",filter);  
          for(auto i=0; i<fileList.count(); ++i)
           {

              QString path=fileList.at(i);
              playlist->addMedia(QUrl::fromLocalFile(path));
              //QString fileName=path.split("/").last();
              ui->listWidget_localMusic->addItem(path);
              //insertSql(fileList.at(i));
              insertSql(path);

           }
}

void mainWidget::on_btn_Voice_clicked()//调节音量
{
    if(ui->VolumeSlider->isHidden())
    {
        ui->VolumeSlider->show();
    }else
    {
         ui->VolumeSlider->hide();
    }
}

void mainWidget::background_to_default()//默认背景
{
    QString fileName=":/image/background/MenuSceneBg.png";
    QSettings mysettings("./LightMusicPlayer.ini",QSettings::IniFormat);
    mysettings.setIniCodec("UTF8");
    mysettings.setValue("background/image-url",fileName);
    setStyleSheet(QString("QWidget#mainWidget{"
                          "border-radius:10px;"
                          "border-image: url(%1);}").arg(fileName));
}

void mainWidget::background_setting()//设置背景
{
    //从默认图片位置打开文件选择框
    QString filter = "All file(*.*);;Text file(*.png);;Xml File(*.ipg)";
    QString fileName=QFileDialog::getOpenFileName(this,("选择自定义背景图片"),"/home/hezexi",filter);
    if(!fileName.isEmpty())
    {
        QImage testImage(fileName);
        if(!testImage.isNull()){
            QSettings mysettings("./LightMusicPlayer.ini",QSettings::IniFormat);
            mysettings.setIniCodec("UTF8");
            mysettings.setValue("background/image-url",fileName);
            setStyleSheet(QString("QWidget#mainWidget{"
                                  "border-radius:10px;"
                                  "border-image: url(%1);}").arg(fileName));
        }
    }
}

void mainWidget::on_listWidget_localMusic_itemDoubleClicked(QListWidgetItem *item)//双击音乐以播放
{
             player->stop();
             player->setMedia(QUrl::fromLocalFile(item->text()));
             player->setVolume(50);
             player->play();
             ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));
             ui->label_Motto->setText(item->text());//显示歌曲信息
             M_Value = ui->listWidget_localMusic->row(item);
             ui->listWidget_HistorytMusic->addItem(item->text());
}

void mainWidget::onDurationChanged(qint64 duration)
{
        ui->PositionSlider ->setMaximum(duration); //设置进度条最大值 也就是歌曲时长 ms
       int secs = duration/1000; //全部秒数
       int mins = secs/60;//分
       secs = secs % 60;//秒
       durationTime = QString::asprintf("%d:%d",mins,secs);
       ui->label_Position->setText(positionTime+"/"+durationTime);


}

void mainWidget::onPositionChanged(qint64 position)
{
    if(ui->PositionSlider->isSliderDown())
            return;//如果手动调整进度条，则不处理
        ui->PositionSlider->setSliderPosition(position);
        int secs = position/1000;
        int mins = secs/60;
        secs = secs % 60;
        positionTime = QString::asprintf("%d:%d",mins,secs);
        ui->label_Position->setText(positionTime+"/"+durationTime);
        if(positionTime == durationTime)
        {
            if(M_Value ==playlist->mediaCount()-1)
                {
                  M_Value =0;
                 }
            else{
                  M_Value++;
                 }
            QListWidgetItem *item = ui->listWidget_localMusic->item(M_Value);
            item->setSelected(true);
            player->stop();
            player->setMedia(QUrl::fromLocalFile(item->text()));
            if(player->state() == QMediaPlayer::StoppedState || player->state() == QMediaPlayer::PausedState)
                 {
                      ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));
                 }
              player->play();
              ui->label_Position->setText(positionTime+"/"+durationTime);//显示歌曲信息
            ui->label_Motto->setText(item->text());//显示歌曲信息
        }



}

void mainWidget::on_PositionSlider_valueChanged(int value)
{
    //player->setPosition(value);
}


void mainWidget::on_VolumeSlider_valueChanged(int value)//进度条声音调节
{
    player->setVolume(value);
}

void mainWidget::on_PositionSlider_sliderReleased()//音乐进度条变化
{
    player->setPosition(ui->PositionSlider->value());
}

void mainWidget::initSearch()
{
    //ui->lineEdit_page->setText(QString::number(1));
    search();
}

void mainWidget::search()//在线搜索
{
    QString word = ui->lineEdit->text();
    NAM =new QNetworkAccessManager();
    if (word != "") {
        qDebug() << word;
        QString surl = "http://c.y.qq.com/soso/fcgi-bin/client_search_cp?t=0&aggr=1&lossless=1&cr=1&catZhida=1&format=json&p=1&n=40&w=" + word;
        qDebug() <<  surl;
        QUrl url = QString(surl);
        QNetworkRequest request(url);
        QNetworkReply *reply = reply= NAM->get(request);
        QEventLoop loop;
        connect(reply, SIGNAL(finished()), &loop, SLOT(quit()));
        loop.exec();
        QByteArray BA = reply->readAll();
        qDebug() << BA;
        // 调试：写log
        /*
        QFile file("search.log");
        if (file.open(QFile::WriteOnly)) {
            file.write(QString(BA).toUtf8());
            file.close();
        }
        */

        QJsonDocument json = QJsonDocument::fromJson(BA);
        QJsonArray list = json.object().value("data").toObject().value("song").toObject().value("list").toArray();
        ui->tableWidget->setRowCount(0);
        QStringList header;
        header<<"歌名"<<"歌手";
         ui->tableWidget->setHorizontalHeaderLabels(header);
        for (int i=0; i<list.size(); i++) {
            ui->tableWidget->insertRow(i);
            ui->tableWidget->setItem(i,0,new QTableWidgetItem(list[i].toObject().value("songname").toString()));
            ui->tableWidget->setItem(i,1,new QTableWidgetItem(list[i].toObject().value("singer").toArray()[0].toObject().value("name").toString()));
            ui->tableWidget->setItem(i,2,new QTableWidgetItem(list[i].toObject().value("songmid").toString()));
            ui->tableWidget->setItem(i,3,new QTableWidgetItem(list[i].toObject().value("albummid").toString()));
        }
        ui->tableWidget->resizeColumnsToContents();
        ui->tableWidget->scrollToTop();
    }
}


void mainWidget::on_tableWidget_cellDoubleClicked(int row, int column)
{
        Q_UNUSED(column);
        ui->label_Motto->setText(ui->tableWidget->item(row,0)->text() + " - " + ui->tableWidget->item(row,1)->text());
        ui->listWidget_HistorytMusic->addItem(ui->tableWidget->item(row,0)->text() + " - " + ui->tableWidget->item(row,1)->text());
        QString songmid = ui->tableWidget->item(row,2)->text();
        QString surl = "https://u.y.qq.com/cgi-bin/musicu.fcg?-=getplaysongvkey2682247447678878&g_tk=5381&loginUin=956581739&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq.json&needNewCode=0&data={\"req_0\":{\"module\":\"vkey.GetVkeyServer\",\"method\":\"CgiGetVkey\",\"param\":{\"guid\":\"2796982635\",\"songmid\":[\"" + songmid + "\"],\"songtype\":[0],\"uin\":\"956581739\",\"loginflag\":1,\"platform\":\"20\"}},\"comm\":{\"uin\":956581739,\"format\":\"json\",\"ct\":24,\"cv\":0}}";
        QNetworkRequest request;
        request.setUrl(QUrl(surl));
        QNetworkReply *reply = NAM->get(request);
        QEventLoop loop;
        connect(reply, SIGNAL(finished()), &loop, SLOT(quit()));
        loop.exec();
        QByteArray BA = reply->readAll();
        QJsonDocument JD = QJsonDocument::fromJson(BA);
        QString purl = JD.object().value("req_0").toObject().value("data").toObject().value("midurlinfo").toArray()[0].toObject().value("purl").toString();
        surl = "http://ws.stream.qqmusic.qq.com/" + purl;
        qDebug() << surl;
        player->stop();
        player->setMedia(QUrl(surl));
        player->play();
        ui->btn_play->setIcon(QIcon(":/image/image-hover/play-hover.png"));
        ui->PositionSlider->setMaximum(player->duration());


}

```
（5）mainwidget.ui截图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ff7b7bc62cf4b958e428644dbdf2a97.png#pic_center)
（6）.pro配置文件截图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4a75ef5a5ed4c2192d5784a30cf7288.png#pic_center)

## 3、工程地址
（1）[工程源码提取码：u2x9](https://pan.baidu.com/s/1gG7bjZOgj4Gm1xEsXRm73Q)
（2）报告书部分页面截图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1b9143fb0d64af793aa41068ba46027.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b32dc858dca74a30ae2870fa91b17028.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d02bed0e6666460d82c98ecf44cfd1c1.png#pic_center)
[报告书提取码9veq](https://pan.baidu.com/s/1k1mZc5oK9ktIE03JfoqUFQ)
（3）因为qt是跨平台的，因此在windows平台、linux平台下也可运行，但是可能也会因qt版本不同或者其他问题而造成运行失败问题。


