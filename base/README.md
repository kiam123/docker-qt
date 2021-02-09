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
$ docker run -ti --rm -e DISPLAY=host.docker.internal:0.0 qt_installer
# 開始安裝

# 開啟 new tab terminal
$ docker commit qt_installer/finished
```

4. 安裝ti-process-sdk
```
$ docker build -f Dockerfile-qt-sdk-installer -t qt_sdk .
$ docker run -ti --rm -e DISPLAY=host.docker.internal:0.0 qt_sdk
# 開始安裝
# 在qt-creator設定ti-sdk

# 開啟 new tab terminal
$ docker commit qt-creator
```

5. 執行qt-creator
```
$ docker run -ti --rm -e DISPLAY=host.docker.internal:0.0 qt-creator
```

## 參考網址
* https://nicroland.wordpress.com/2015/12/06/running-qtcreator-in-docker/