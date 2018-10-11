# 使用vcpkg构建适用于Windows的osgEarth
vcpkg是一个非常有用的C++包管理器。它适用于Windows，Linux和MacOS，但对于本节，我们主要关注Windows。

首先，按照页面上的说明下载并安装vcpkg。

接下来安装构建功能齐全的osgearth所需的依赖项

`vcpkg install osg:x64-windows sqlite3:x64-windows protobuf:x64-windows poco:x64-windows`

第一次运行它时会减少许多依赖项，将耗费一段时间，可以去喝一杯咖啡。

一旦构建了所有依赖项，就需要实际构建osgEarth。

**获取源代码**

`git clone https://github.com/gwaldron/osgearth.git`

**为源代码构建创建一个目录**
```
cd osgearth
mkdir build
cd build
```
**配置Cmake**

vcpkg提供了一个Cmake工具链文件，可帮助osgEarth找到所有依赖项。
您需要为Release和Debug指定不同的构建目录，并使用-DCMAKE_BUILD_TYPE指定构建类型，
这是因为osgEarth的某些依赖项在没有指定构建类型的情况下不会同时获取调试版本和发行版本。
这应该会将来的cmake版本中修复。这是一个release版本：
```
cmake .. -G "Visual Studio 15 2017 Win64" \
-DCMAKE_BUILD_TYPE=Release \
-DWIN32_USE_MP=ON \
-DCMAKE_TOOLCHAIN_FILE=[vcpkg root]\scripts\buildsystems\vcpkg.cmake
```
**构建并安装osgEarth**

您可以使用cmake在命令行上构建和安装osgEarth，也可以打开Visual Studio的解决方案并从那里构建它。

`cmake --build . --target INSTALL --config Release`

**设置运行时环境**

你需要确保vcpkg依赖项和osgEarth在您的路径中，然后：
```C++
set PATH=%PATH%;c:\vcpkg\installed\x64-windows\bin
set PATH=%PATH%;c:\vcpkg\installed\x64-windows\tools\osg
set PATH=%PATH%;c:\Program Files\osgEarth\bin
```
注意：如果不想自己为应用程序构建osgEarth，也可以使用vcpkg实际安装它，只需要

`vcpkg install osgearth:x64-windows`
