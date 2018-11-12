# WMS (OGC Web Map Service)
这个插件从一个OGC网络地图服务（[Web Map Service](https://en.wikipedia.org/wiki/Web_Map_Service)）资源中读取图像数据。

用法示例：
```XML
<image name="Landsat" driver="wms">
    <url>http://onearth.jpl.nasa.gov/wms.cgi</url>
    <srs>EPSG:4326</srs>
    <tile_size>512</tile_size>
    <layers>global_mosaic</layers>
    <styles>visual</styles>
    <format>jpeg</format>
</image>
```
属性：

**url：** WMS资源的位置

**srs：** 返回瓦片的空间参考

**tile_size：** 覆盖默认瓦片尺寸（默认=256）

**layers：** 要合并并返回的WMS图层列表

**styles：** 要渲染的WMS样式

**format：** 返回的图像格式
