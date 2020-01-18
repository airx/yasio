# yasio - *Y*et *A*nother *S*ocket *IO* service
[![Build Status](https://travis-ci.com/simdsoft/yasio.svg?branch=master)](https://travis-ci.com/simdsoft/yasio)
[![Windows Build Status](https://ci.appveyor.com/api/projects/status/fnd3fji4dss7ppsd/branch/master?svg=true)](https://ci.appveyor.com/project/halx99/yasio)
[![Release](https://img.shields.io/badge/release-v3.30.9-blue.svg)](https://github.com/simdsoft/yasio/releases)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)
[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/simdsoft/yasio/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/simdsoft/yasio.svg?label=Stars)](https://github.com/simdsoft/yasio)
[![GitHub forks](https://img.shields.io/github/forks/simdsoft/yasio.svg?label=Fork)](https://github.com/simdsoft/yasio)
[![Language grade: C/C++](https://img.shields.io/lgtm/grade/cpp/g/simdsoft/yasio.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/simdsoft/yasio/context:cpp)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/simdsoft/yasio.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/simdsoft/yasio/alerts/)

yasio is a multi-platform support library with focus on asynchronous TCP socket I/O for any client application, support win32 & linux & apple & android & wp8 & wp8.1-universal & win10-universal.  
The core design is reference from [asio](https://github.com/chriskohlhoff/asio) but very small.  
**This lib has been used by project [RedAlert OL/红警OL项目](http://hongjing.qq.com/) and run at millions of devices.**

## Clone at github: [yasio](https://github.com/simdsoft/yasio)

## Core framework
![image](https://github.com/simdsoft/yasio/blob/master/framework.png)  

## Features: 
* support IPv6-only network.  
* support multi-connections at one thread.  
* support deadline timer.  
* support unpack tcp packet internal, user do not need to care it.  
* support lua bindings  
* support cocos2d-x jsb  
* support CocosCreator jsb2.0  
* support Unity3D
* support multicast
  
## Intergation guides:  
### cpp intergation: 
Compile yasio\xxsocket.cpp yasio\yasio.cpp with your project  
please see: [tcptest](https://github.com/simdsoft/yasio/blob/master/tests/tcp/main.cpp)  
  
    
### Lua intergation: 
#### common lua application integration:
1. Compile ```yasio\xxsocket.cpp``` ```yasio\yasio.cpp``` ```yasio\ibstream.cpp``` ```yasio\obstream.cpp``` ```yasio\bindings\lyasio.cpp``` with your project  
2. call luaopen_yasio after LUA VM initialized.  
build example_lua & debug it(windows user): build ```Debug``` version with cmake, open ```example_lua.xsxproj```  by [x-studio ide](https://x-studio.net/) at the same directory of example_lua.exe,  
then you can toggle breakpoint and debug the example_lua.exe
  
#### cocos2d-x lua integration:
1. Copy the folder ```yasio``` to cocos2d-x engine's folder ```external```, ```yasio``` as sub directory of ```external```  

2. Add ```yasio\xxsocket.cpp``` ```yasio\yasio.cpp``` ```yasio\ibstream.cpp``` ```yasio\obstream.cpp``` ```yasio\bindings\lyasio.cpp```
```yasio\bindings\yasio_cclua.cpp``` to your compile system.
    + For Win32 & Apple platform:
Add ```yasio\xxsocket.cpp``` ```yasio\yasio.cpp``` ```yasio\ibstream.cpp``` ```yasio\obstream.cpp``` ```yasio\bindings\lyasio.cpp```
```yasio\bindings\yasio_cclua.cpp``` to **libluacocos2d** project of your Visual Studio or xcode solution.  
    + For Android:
Add yasio to your application Android.mk files, such as:  
```
LOCAL_STATIC_LIBRARIES += yasio_static
$(call import-module, external/yasio/jni/cocos-lua)
```

3. Call luaopen_yasio_cclua(L) at your AppDelegate.cpp, please remember ```#include "yasio/bindings/yasio_cclua.h"``` firstly.  

### JSB integration  
#### cocos2d-x jsb integration:
1. Copy the folder ```yasio``` to cocos2d-x engine's folder ```external```, ```yasio``` as sub directory of ```external``` 
2. Add ```yasio/bindings/yasio_jsb.cpp``` to your compile system.
    + For Win32 & Apple platform:
Add ```yasio\bindings\yasio_jsb.cpp``` to **libjscocos2d** project of your Visual Studio or xcode solution. 
    + For Android: Add yasio to your application Android.mk files, such as: 
```
LOCAL_STATIC_LIBRARIES += yasio_static
$(call import-module, external/yasio/jni/cocos-jsb)
```
3. Add register code to your AppDelegate.cpp: ```sc->addRegisterCallback(jsb_register_yasio);```, please include ```yasio/bindings/yasio_jsb.h``` firstly.
#### CocosCreator jsb2.0 integration:
1. Copy the folder ```yasio``` to cocos2d-x engine's folder ```external/sources```, ```yasio``` as sub directory of ```external/sources```  
2. Add obstream.cpp, ibstream.cpp, xxsocket.cpp, yasio.cpp, bindings/yasio_jsb20.cpp to your compile system.
    + For Win32 & Apple platform: Add ```yasio\bindings\yasio_jsb20.cpp``` to **libcocos2d** project of your Visual Studio or xcode solution.  
    + For Andorid: Add follow 1 lines to cocos/Android.mk file:
```
../external/sources/yasio/bindings/yasio_jsb20.cpp \
```
3. Add register code to your jsb_module_register.cpp: ```se->addRegisterCallback(jsb_register_yasio);``` , please include ```yasio/bindings/yasio_jsb20.h``` firstly.  


#### JS demo:
see: [example.js](https://github.com/simdsoft/yasio/blob/master/examples/js/example.js)  
  
  
## Quick test at linux platform with gcc compiler:  
```g++ yasio/xxsocket.cpp yasio/yasio.cpp yasio/ibstream.cpp yasio/obstream.cpp tests/tcp/main.cpp --std=c++11 -lpthread -I./ -o tcptest && ./tcptest``` or ```g++ tests/tcp/main.cpp --std=c++11 -DYASIO_HEADER_ONLY -lpthread -I./ -o tcptest && ./tcptest``` 
  
## pitfall: 
1. For Microsoft Visual Studio, if your project has specific precompiled header, you should include it at head of xxsocket.cpp or specific the compile option: ```C/C++ --> Advance -->'Forced Include File'``` to it(such as pch.h).  
2. For Microsoft Visual Studio 2019 MSVC C++17, you must set compile option: ```C/C++ --> Language --> Conformance mode``` to **No**, otherwise, you will got compiling error: ```sol.hpp(8060): error C3779: 'sol::stack::get': a function that returns 'decltype(auto)' cannot be used before it is defined```
  
## Unity3D integration guide
Tencent xLua integration, see: [https://github.com/simdsoft/xLua](https://github.com/simdsoft/xLua)

## For more detail usage: see [wiki](https://github.com/simdsoft/yasio/wiki)

