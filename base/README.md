# docker-qt

1. 建立base image安裝前期需要的tool
```
docker build -f Dockerfile-qt-base -t qt_base .
```

2. 下載qt-creator5.14.2版或更高的版本
```
docker build -f Dockerfile-qt-installer -t qt_installer .
```

3. 用界面的方式開啟
```
### windows
$ docker run -ti --name=qt_installer  -e DISPLAY=host.docker.internal:0.0 qt_installer

### linux
#授予Docker訪問X-Server的權限
$ xhost +local:docker

# 啟動qt-creator的install
# 開始安裝
$ docker run -it --name=qt_installer -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt_installer

# 將container commit成一個image
$ docker commit qt_installer qt_installer/finished

# 測試qt-creator是否安裝成功
$ docker run -it --rm --name=qt_installer_finished -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt_installer/finished /home/test/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh
```

4. 安裝ti-process-sdk
```
$ docker build -f Dockerfile-ti-sdk-cp -t ti_sdk_cp .

# windows
# 啟動ti-sdk的install
# 開始安裝
$ docker run -ti --name=ti_sdk_cp -e DISPLAY=host.docker.internal:0.0 ti_sdk_cp

# linux
# 啟動ti-sdk的install
# 開始安裝
$ docker run -it --name=ti_sdk_cp -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix ti_sdk_cp /home/test/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh

# 開始設定ti-sdk...

# 將container commit成一個image
$ docker commit ti_sdk_cp qt-creator
```

5. 啟動qt-creator 
```
# windows
# 啟動qt-creator 
$ docker run --rm -ti --name=qt-creator -e DISPLAY=host.docker.internal:0.0 qt-creator /home/test/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh

# linux
# 啟動qt-creator 
$ docker run --rm -it --name=qt-creator -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt-creator /home/test/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh
```


## 參考網址
* https://nicroland.wordpress.com/2015/12/06/running-qtcreator-in-docker/

## 問題
* https://forums.docker.com/t/start-a-gui-application-as-root-in-a-ubuntu-container/17069
* http://xhyumiracle.com/error-while-loading-shared-libraries-libgthread-2-0-so-0/