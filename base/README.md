# docker-qt

1. 建立base image安裝前期需要的tool
```
docker build -t qt_base .
```

2. 下載qt-creator5.14.2版或更高的版本
```
docker build -t qt_base .
```

3. 用界面的方式開啟
```
### windows
$ docker run -ti --rm -e DISPLAY=host.docker.internal:0.0 qt_installer

### linux
#授予Docker訪問X-Server的權限
$ xhost +local:docker
#啟動qt-creator的install
# 開始安裝
$ docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt_installer 

# 開啟 new tab terminal，然後將container commit成一個image
$ docker commit e4ffd6e45ed8 qt_installer/finished
```

4. 安裝ti-process-sdk
```
$ docker build -f Dockerfile-qt-sdk-installer -t qt_sdk_install .

# windows
#啟動ti-sdk的install
# 開始安裝
$ docker run -ti -e DISPLAY=host.docker.internal:0.0 qt_sdk_install

# linux
#啟動ti-sdk的install
# 開始安裝
$ docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt_sdk_install

# 開啟 new tab terminal
$ docker commit 244068a816a2 ti_sdk_setup
```

5. 設定ti-sdk
```
# windows
#啟動ti-sdk的install
# 開始安裝
$ docker run -ti -e DISPLAY=host.docker.internal:0.0 qt_sdk_setup /opt/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh

# linux
#啟動ti-sdk的install
# 開始安裝
$ docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix qt_sdk_setup /opt/Qt5.14.2/Tools/QtCreator/bin/qtcreator.sh
```

6. 執行qt-creator
```
$ docker run -ti --rm -e DISPLAY=host.docker.internal:0.0 qt-creator
```

## 參考網址
* https://nicroland.wordpress.com/2015/12/06/running-qtcreator-in-docker/

## 問題
* https://forums.docker.com/t/start-a-gui-application-as-root-in-a-ubuntu-container/17069