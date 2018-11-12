# VPB (VirtualPlanerBuilder)
[VirtualPlanetBuilder](http://www.openscenegraph.com/index.php/documentation/tools/virtual-planet-builder)（VPB）是一个用于生成页面地形模型的OSG应用。这个应用尝试从VPB模型中“刮出”图像和高程格网瓦片，并为osgEarth引擎渲染提供这个数据。

**注意：** 我们把这个驱动器作为一个临时方案来解决那些有遗留VPB模型但是无法访问源数据的情况。由于VPB模型格式不会传达模型建立时所使用的所有参数，所以配置这个驱动会很棘手。

用法示例：
```XML
<image driver="vpb">
    <url>http://www.openscenegraph.org/data/earth_bayarea/earth.ive</url>
    <profile>global-geodetic</profile>
    <primary_split_level>5</primary_split_level>
    <secondary_split_level>11</secondary_split_level>
    <directory_structure>nested</directory_structure>
</image>
```
属性：

**url：** VPB模型的根文件

**primary_split_level：** VPB运行时所设置的，详见VPB文档

**secondary_split_level：** VPB运行时所设置的，详见VPB文档

**directory_structure：** 默认是`nested`；选项是`nested`，`flat`和`task`
