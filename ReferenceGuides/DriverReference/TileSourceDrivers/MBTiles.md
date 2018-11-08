# MBTiles
这个插件从[MBTiles](https://www.mapbox.com/help/how-mapbox-works/)文件中读取数据，是一种在单表中保存所有瓦片数据的SQLite3数据库。这个驱动器需要在SQLite3的支持下构建osgEarth。

用法示例：
```XML
<image name="haiti" driver="mbtiles">
    <filename>../data/haiti-terrain-grey.mbtiles</filename>
            <format>jpg</format>
</image>
```
属性：

**filename：** MBTiles文件的文件名

**format：** MBTiles文件（如jpeg，png等）中的图像格式

**compute_levels：** 是否自动计算MBTiles文件的有效级别。默认为true并且会扫描表格确定最小河最大值。在第一次加载文件时会耗费一些时间，所以如果你事先知道你的文件的级别，可以将其设为false然后使用瓦片源的min_level和max_level设置。

也可以参考：

  `tests`文件夹中的`mb_tiles.earth`示例。
