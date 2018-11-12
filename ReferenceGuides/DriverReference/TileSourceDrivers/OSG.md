# OSG (OpenSceneGraph Loader)
这个加载器将使用一个OSG图像插件来加载图像，然后基于这幅图像来返回瓦片。由于图像没有它自己的SRS信息，你需要指定地理空间配置。

这个插件不是很常用，GDAL驱动器包含了大部分文件类型。

用法示例：
```XML
<image driver="osg">
    <url>images/world.png</url>
    <profile>global-geodetic</profile>
</image>
```
属性：

**url：** 加载文件的位置。

**profile：** 图像的地理空间配置。
