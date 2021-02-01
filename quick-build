cd base
docker build -t qt_base .
cd ..

cd qt-image
docker build -t qt .
cd..

docker run -p 10022:22 \
    -v ./project:/home/test/project \
    qt
