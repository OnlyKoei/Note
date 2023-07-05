# 3.8 spine-wasm 编译安装环境
1. 下载emsdk工具包，熟悉git的可以直接使用下列命令行在本地克隆emsdk库：
git clone https://github.com/juj/emsdk.git
或者直接访问https://github.com/juj/emsdk，然后通过页面右上方的“Clone or download”下载emsdk库并解压到本地；
2. 对MacOS或Linux用户，在控制台切换至emsdk所在目录，执行以下命令：
./emsdk update
./emsdk install latest
emsdk将联网下载并安装Emscripten最新版的各个组件。安装完毕后，执行以下命令配置并激活已安装的Emscripten：

./emsdk activate latest
在新建的控制台中，切换至emsdk所在的目录，执行以下命令：

source ./emsdk_env.sh
将为当前控制台配置Emscripten各个组件的PATH等环境变量。

在Windows环境下的安装及激活执行：

emsdk.bat update
emsdk.bat install latest
emsdk.bat activate latest
设置环境变量执行：

emsdk_env.bat
ps： 安装及激活只需要执行一次；以后在新建的控制台中设置一次环境变量后，即可使用Emscripten核心命令emcc。在Windows环境下，如果想把Emscripten的环境变量注册为全局变量，可以以管理员身份运行emsdk.bat activate latest --global，该命令将更改系统的环境变量，使得以后无需再运行emsdk_env.bat，该方法有潜在的副作用：它将环境变量指向了Emscripten内置的Node.js、Python、Java，若系统中安装了这些组件的其他版本，可能引发冲突。比如原先配置的 JAVA_HOME 路径会指向 emsdk 路径下自带的 jdk,如果版本和原先配置的不一致，可能会影响其他程序的使用；

3. 需要本地有安装 cmake 和 mingw, 安装完成后记得配置环境变量，即在 path 上添加相应路径，比如：
cmake:D:\android\Sdk\cmake\3.18.1\bin;
mingw:C:\mingw64\bin

4. 环境都安装完后，进入engine/native/cocos/editor-support/spine-wasm 目录下，新建 build 文件夹；
   
5. 在 build 目录分别执行：
emcmake cmake ..
emmake make
编译成功会生成 spine.js,spine.js.mem
等待编译完成即可在 build 文件夹中获取编译产物。
编译选项和调试。CMakeLists.txt 中的重要配置如下：
set(CMAKE_BUILD_TYPE "Release")

set(EMS_LINK_FLAGS "-O3 -s WASM=0 -s INITIAL_MEMORY=33554432 -s ALLOW_MEMORY_GROWTH=1 -s DYNAMIC_EXECUTION=0 -s ERROR_ON_UNDEFINED_SYMBOLS=0 \
        -flto --no-entry --bind -s USE_ES6_IMPORT_META=0 -s EXPORT_ES6=1 -s MODULARIZE=1 -s EXPORT_NAME='spineWasm' \
        -s ENVIRONMENT=web -s FILESYSTEM=0 -s NO_EXIT_RUNTIME=1 -s LLD_REPORT_UNDEFINED \
        -s MIN_SAFARI_VERSION=110000 \
        --js-library ../library_spine.js")

若需编译 Debug 版，将 BUILD_TYPE 设置为 Debug，取消 -O3 优化选项，增加 -g 编译选项。
WASM=1 打包生成 wasm 版本。
编译wasm =1时， 产物是spine.js 以及 spine.wasm。这个你开关选项时，需要把之前的编译产物取走，否则会被覆盖掉。
