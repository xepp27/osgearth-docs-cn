
# 建立osgEarth 
osgEarth是一个跨平台的库，使用CMake构建系统。您需要2.8或更高版本。（与OSG使用的构建系统相同。）

**平台特性指引**
- [使用vcpkg构建适用于Windows的osgEarth](.\vcpkg.md)
- [构使用于iOS的osgEarth（和OSG）](.\ios.md)

 ## 获取源代码
 
选项1：使用GIT

osgEarth托管在GitHub上，你需要一个git客户端来访问它。我们向Windows用户推荐TortoiseGit。

使用客户端来复制存储器：
http://github.com/gwaldron/osgearth.git

选项2：下载标记的版本

要下载源代码的tarball或ZIP文件，请访问osgEarth标签并选择所需的文件。官方的最新发布在顶部。

## 获取依赖项

**必需的依赖项**
- OpenSceneGraph 3.4或更高版本
- GDAL 2.0或更高版本——地理空间数据抽象层
- CURL——HTTP传输库（OpenSceneGraph第三方库发行版附带）

**推荐的预构建依赖项**
- AlphaPixel为各种架构预先构建了OSG和第三方依赖关系。
- 针对各种体系结构的预构建GDAL二进制文件。
- 使用vcpkg安装所需的依赖项。

**可选的依赖项：** osgEarth将在没有它们的情况下编译。看看并决定你需要什么：
* GEOS 3.2.0或更高版本——用于拓扑操作的C++库。osgEarth使用GEOS执行各种几何操作，如缓冲和交叉。如果您打算在osgEarth中使用矢量要素数据，您可能需要这样做。
  * SQLite——自包含，无服务器，零配置，事务性SQL数据库引擎。用于访问sqlite/mbtiles数据集。您可能需要这些提示来从Windows二进制文件中包含的.def和.dll文件创建必要的.lib文件：http://eli.thegreenplace.net/2009/09/23/compiling-sqlite-on-windows
* QT_ 5.4或更高版本——跨平台UI框架。用于构建osgEarth的Qt支持库，这对于构建我们osgEarth的Qt应用程序很有用（尽管不是必需的）。将QT_QMAKE_EXECUTABLE CMake变量指向要使用的qmake.exe，CMake将填充所有其他QT变量。

## 建立
确保首先构建OSG和所有依赖项。

osgEarth使用CMake，2.8或更高版本。由于OSG也使用CMake，因此若构建过OSG，该过程应该是熟悉的。

这里有一些提示。
- 始终使用CMake进行“源外”构建。也就是使用与源代码分开的构建目录。这样可以更轻松地维护单独的版本并保持GIT更新整洁。
- 对于可选的依赖项（如GEOS），如果不使用它，只需将CMake字段留空。
- 对于OSG依赖项，只需输入OSG_DIR变量，生成CMake时将自动查找所有其他OSG目录。
- 与往常一样，如果遇到问题，请查看论坛！

祝你好运！
