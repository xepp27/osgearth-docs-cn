# OGR
这个插件从OGR简单特征库（[OGR Simple Feature Library](https://www.gdal.org/)）支持（有相当多的）的任意格式中读取矢量数据。其中最常见的包括ESRI形状文件、GML和PostGIS。

用法示例：
```XML
<model driver="feature_geom">
    <features driver="ogr">
        <url>data/world_boundaries.shp</url>
    </features>
    ...
```
属性：

**url：** 加载特征数据的位置

**connection：** 如果特征数据在一个数据库中，使用这个来指定DB连接字符串来取代使用`url`。

**geometry：** 在\`OGC WKT format\`_中指定*内联* 几何体来取代`url`或`connection`。

**geometry_url：** 同`geometry`除了WKT字符串在一个文件中。

**ogr_driver：** 使用的``OGR driver``_。（默认=“ERSI形状文件”）

**build_spatial_index：** 设为true来为特征数据创建一个空间索引，将大大加快对大数据集的访问速度。

**layer：** 一些数据集需要子数据集的附加图层标识符，在这里设置（整数）。

*PostGIS使用中的特别提示：**
PostGIS使用一个`connection`字符串而不是`url`来创建它的数据库连接。通常包括表格参考，例如`table = something`。然而，在这个驱动器中，这会导致问题，因此在`layer`属性中指定您的表格。例如：
```XML
<features driver="ogr">
    <connection>PG:dbname=mydb host=127.0.0.1 ...</connection>
    <layer>myTableName</layer>
</features>
```
