先安裝ti-processor-sdk-linux-am57xx-evm-03.01.00.06-Linux-x86-Install.bin
註：建議把安裝位置名稱縮短

舉例
專案位置：/home/test/MyWorkspace/ti-process-project/
SDK安裝位置：/opt/ti-processor-sdk-linux-am57xx-evm-03.01.00.06/

開啟
/opt/ti-processor-sdk-linux-am57xx-evm-03.01.00.06/linux-devkit/sysroots/armv7ahf-neon-linux-gnueabi/usr/lib/qt5/mkspecs/linux-oe-g++
編輯文件qmake.conf
把include(../oe-device-extra.pri)註解掉(用#號)
因為這個檔案不存在

===============================
使用Termainal：
source ~/ti-processor-sdk-linux-am57xx-evm-03.01.00.06/linux-devkit/environment-setup
cd ~/MyWorkspace/ti-process-project/
make clean && make distclean
qmake -o ../build-project-aph/
make -C ../build-project-aph/ 2> ../project-aph-Warnings

註：
make distclean用於清除qmake的Makefile
建置後的檔案在build-project-aph資料夾中
錯誤及警告訊息寫入project-aph-Warnings文件

===============================
使用Qt Creator：
------------------
編輯此說明的qt.conf和kit-environment，把家目錄和SDK安裝路徑改成你的路徑

開啟
/opt/ti-sdk-03.02/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/qt5/
將qt.conf重新命名，如qt.conf.original，把改好的qt.conf放入資料夾

開啟Qt Creator
Tools > Options > Build & Run >

Qt Versions > Add
/opt/ti-sdk-03.02/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/qt5/qmake
Version name: 把括號內改成ti-sdk %{Qt:Version} (qt5)

Compilers >
Add > GCC > C
    Name: 可以取名為ti-gcc
    Compiler path: /opt/ti-sdk-03.02/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/arm-linux-gnueabihf-gcc
    ABI: arm-linux-generic-elf-32bit
Add > GCC > C++
    Name: 可以取名為ti-g++
    Compiler path: /opt/ti-sdk-03.02/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/arm-linux-gnueabihf-g++
    ABI: arm-linux-generic-elf-32bit
    
Debuggers > Add
    Name: 可以取名為ti-gdb
    Path: /opt/ti-sdk-03.02/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/arm-linux-gnueabihf-gdb
    
Kits > Add
    Name: 可以取名為ti-processor
    Device Type: Generic Linux Device
    Compiler: 選擇剛才自訂的Compiler(ti-gcc & ti-g++)
    Environment: 把改好的kit-environment內容全部貼到此項目中
    Debugger: 選擇剛才自訂的Debugger(ti-gdb)
    Qt version: 選擇剛才自訂的Qt(ti-sdk)
    Qt mkspec: /opt/ti-sdk-03.02/linux-devkit/sysroots/armv7ahf-neon-linux-gnueabi/usr/lib/qt5/mkspecs/linux-oe-g++


開啟專案(aphcontroller)
若為首次開啟，Kit勾選剛才建立的Kit(ti-processor)，取消勾選Desktop Qt > Configure
若否，點左方欄板手(Projects) > Build & Run > 右鍵點Desktop Qt選disable > 左鍵點選啟用剛才建立的Kit(ti-processor)

回到Edit視窗，點擊鐵鎚就可以Build(因沒設定Device所以不能Run)


註：
qt.conf用於自訂qmake的sysroot persistent property
    =>source後用qmake -query查，參考http://doc.qt.io/qt-5/qt-conf.html
kit-environment就是"/SDK安裝路徑/linux-devkit/environment-setup"的內容
    =>因不能用變數，source後用printenv查找出對應的
    =>其實很多變數用不到的樣子，反正通通貼進去也沒差
因無法Run所以Debugger也沒用，用系統預設的似乎也就沒差了





arm-linux-gnueabihf-objcopy --only-keep-debug aphcontroller aphcontroller.debug
arm-linux-gnueabihf-objcopy --strip-debug aphcontroller
arm-linux-gnueabihf-objcopy --add-gnu-debuglink=aphcontroller.debug aphcontroller
chmod -x aphcontroller.debug





