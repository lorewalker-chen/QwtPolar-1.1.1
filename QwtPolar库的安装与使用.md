# QwtPolar库的安装与使用
QwtPolar库用于在极坐标系统上显示值。  

安装前请查看官网<https://qwtpolar.sourceforge.io/>上版本对qt版本的要求。  

以在Qt5.12.10环境下安装qwtpolar-1.1.1为例。

## 下载
从[官网](https://qwtpolar.sourceforge.io/)下载：  
1. 源码压缩文件`qwtpolar-1.1.1.zip`或`qwtpolar-1.1.1.tar.bz2`
2. 官方文档`qwtpolar-1.1.1.pdf`
3. Qt帮助手册`qwtpolar-1.1.1.qch`

## Linux下编译
1. __安装Qwt库__。
2. __将源码解压__。例如解压到`/home/qwtpolar-1.1.1/`
3. __修改生成路径(可选)__。打开文件`qwtpolarconfig.prf`，找到并更改`QWT_POLAR_INSTALL_PREFIX`项。  
这里不做修改，使用默认路径`/usr/local/qwtpolar-1.1.1/`
4. __终端进入源码文件夹，依次执行以下命令__。编译QwtPolar需要Qwt库，所以需要将Qwt库添加到环境变量QMAKEFEATURES  
```
export QMAKEFEATURES=/usr/local/qwt-6.1.6/features:$QMAKEFEATURES
qmake qwtpolar.pro
make
sudo make install
```  
完成后可在生成路径下找到编译生成的文件。

## Linux下安装
1. 将生成目录`/usr/local/qwtpolar-1.1.1/plugins/designer/`中的文件`libqwt_polar_designer_plugin.so`拷入`/Qt5.12.10/5.12.10/Tools/QtCreator/lib/Qt/plugins/designer/`
2. 将生成目录`/usr/local/qwtpolar-1.1.1/lib/`中的所有文件`libqwtpolar.so*`拷入`/Qt5.12.10/5.12.10/gcc_64/lib/`
3. 在`/Qt5.12.10/5.12.10/gcc_64/include/`中新建文件夹`QwtPolar`，将生成目录`/usr/local/qwtpolar-1.1.1/include/`中的所有文件拷入其中  

重新启动Qt Creator，打开任意ui界面，在控件列表最下方看到QwtPolar Widgets则安装成功。

## Windows下编译
1. __安装Qwt库__。
2. __添加环境变量__。在系统环境变量中添加变量QMAKEFEATURES，其值为Qwt生成目录下`C:/Qwt-6.1.6/features`，在系统环境变量Path中添加`C:/Qwt-6.1.6/include/`
3. __将源码解压__。例如解压到`F:/home/qwtpolar-1.1.1/`
4. __修改生成路径(可选)__。打开文件`qwtpolarconfig.prf`，找到并更改QWT_INSTALL_PREFIX项。  
这里不做修改，使用默认路径`C:/QwtPolar-1.1.1/`
5. __用Qt自带终端进入源码文件夹，依次执行以下命令__。  
```
qmake qwtpolar.pro
//使用MinGW编译器
mingw32-make
mingw32-make install
//使用MSVC编译器
nmake
nmake install
```  
完成后可在生成路径下找到编译生成的文件。

## Windows下安装
以使用64位MinGW编译器为例，使用MSVC编译器请拷贝到对应文件夹。
1. 将生成目录`C:/QwtPolar-1.1.1/plugins/designer/`中的文件`qwt_polar_designer_plugin.dll`拷入`/Qt/Qt5.12.10/5.12.10/mingw73_64/plugins/designer/`
2. 将生成目录`C:/QwtPolar-1.1.1/plugins/lib/`中两个`.a`文件拷入`/Qt/Qt5.12.10/5.12.10/mingw73_64/lib/`，两个`.dll`文件拷入`/Qt/Qt5.12.10/5.12.10/mingw73_64/bin/`
3. 在`/Qt/Qt5.12.10/5.12.10/mingw73_64/include/`中新建文件夹`QwtPolar`，将生成目录`C:/QwtPolar-1.1.1/plugins/include/`中的所有文件拷入其中  

在Qt Creator中，右键任意ui界面，选择`用...打开->Qt Designer`，在控件列表最下方看到Qwt Widgets则安装成功。
> Windows下Qt Creator用MSVC编译，所以在Qt Creator中无法直接使用MinGW编译的Qwt库，必须用Qt Designer打开才能看到。

## 安装帮助文档
将下载的`qwtpolar-1.1.1.qch`拷入`/Qt5.12.10/Docs/Qt-5.12.10/`

## 使用Qwt库
### 方法一
在工程中的`.pro`文件中添加以下代码  
```
win32:CONFIG(debug,debug|release){
    LIBS += -L$$[QT_INSTALL_PREFIX]/lib -lqwtpolard
}
else {
    LIBS += -L$$[QT_INSTALL_PREFIX]/lib -lqwtpolar
}

INCLUDEPATH += $$[QT_INSTALL_PREFIX]/include/QwtPolar
```  
即可编译通过。
> Windows下debug模式需要调用`libqwtpolard.dll`，而release模式需要调用`libqwtpolar.dll`  
> Linux下调用的是同一个库

### 方法二
将生成目录下的features文件夹拷入工程目录  
在工程中的`.pro`文件中添加`include(./features/qwtpolar.prf)`  
即可编译通过
> 若两台电脑上Qwt编译生成路径不同，该方法将导致工程编译时不通过