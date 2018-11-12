# TMS (Tile Map Service)
这个插件读取根据广泛使用的OSGeo瓦片地图服务（[Tile Map Service](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification)）规格存储的数据。

用法示例：
```XML
<image driver="tms">
    <url>http://readymap.org:8080/readymap/tiles/1.0.0/79/</url>
</image>
```
属性：

**url：** TMS存储器的根URL（或路径名称）

**tms_type：** 设置为`google`来反转瓦片索引的Y轴

**format：** 覆盖服务反馈的格式（如jpg，png）
