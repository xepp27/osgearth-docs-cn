# Mapnik Vector Tiles
这个插件从一个包含`vector tiles<https://github.com/mapbox/vector-tile-spec>`_的[MBTiles](https://www.mapbox.com/developers/mbtiles/)文件读取矢量数据。

注意：这个驱动器目前不支持多级别mbtiles瓦片，仅加载数据库中最大级别。在osgEarth可以更好地支持非附加特征数据时将会改变。

这个驱动器需要你在SQLite3或Protobuf的支持下构建osgEarth。

用法示例：
```XML
<model driver="feature_geom">
    <features name="osm" driver="mapnikvectortiles">
        <url>../data/osm.mbtiles</url>
    </features>
    ...
```

属性：

**url：** mbtiles文件的位置。
