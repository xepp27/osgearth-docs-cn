# WCS (OGC Web Coverage Service)
这个插件基于OGC网络覆盖数据服务（[Web Converage Service](https://en.wikipedia.org/wiki/Web_Coverage_Service)）规格以有限的方式读取栅格覆盖数据。在osgEarth中它仅对于获取高程格网数据瓦片有用。我们支持WCS 1.1中的一个子集。

用法示例：
```XML
<elevation driver="wcs">
    <url>http://server</url>
    <identifier>elevation</identifier>
    <format>image/GeoTIFF<format>
</elevation>
```
属性：

**url：** WCS资源的位置

**identifier：** WCS标识符（如所读取的图层）

**format：** 返回的数据的格式（通常是`tif`）

**elevation_unit：** 解译高程格网高程值时使用的单位（默认为米）

**range_subset：** WCS范围子集字符串（参阅WCS文档）
