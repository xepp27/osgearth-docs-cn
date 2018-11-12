# WorldWind TileService
这个插件读取存储于NASA WorldWind [TileService](https://www.worldwindcentral.com/wiki/index.php?title=TileService)布局中的瓦片。

用法示例：
```XML
<image driver="tileservice">
    <url>http://server/tileservice/tiles</url>
    <dataset>weather</dataset>
    <format>png</format>
</image>
```
属性：

**url：** 瓦片服务存储器（TileService repository）的根URL（或路径名称）

**dataset：** 所使用的WW数据库（图层）

**format：** 独立瓦片的格式（如jpg，png）
