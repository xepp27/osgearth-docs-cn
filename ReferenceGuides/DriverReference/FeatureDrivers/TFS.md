# TFS(Tiled Feature Service)
这个插件从一个*瓦片特征服务（Tiled Feature Service）* 存储器中读取矢量数据。TFS是一个瓦片化布局，与[TMS（Tile Map Service）](osgearth-docs-cn/ReferenceGuides/DriverReference/TileSourceDrivers/TMS.md)相似，但是是针对获取的特征数据。

用法示例：
```XML
<model driver="feature_geom">
    <features driver="tfs">
        <url>http://readymap.org/features/1/tfs/</url>
        <format>json</format>
    </features>
    ...
```

属性：

**url：** 加载特征数据的位置

**format：** TFS数据的格式；选项有`json`（默认）或`gml`
