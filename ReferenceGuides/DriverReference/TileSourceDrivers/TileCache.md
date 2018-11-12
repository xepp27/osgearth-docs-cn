# TileCache
[TileCache](http://tilecache.org/)（MetaCarta Labs）是一个网络地图瓦片缓存系统，它对于瓦片层次结构的编码有自己的布局。这个插件将从这个文件布局中读取瓦片。

用法示例：
```XML
<image driver="tilecache">
    <url>http://server/tiles/root</url>
    <layer>landuse</layer>
    <format>jpg</format>
</image>
```
属性：

**url：** 瓦片缓存（tilecache）存储器的根URL（或者路径名称）

**layer：** 所使用的TileCache图层

**format：** 独立瓦片的格式（如jpg，png）
