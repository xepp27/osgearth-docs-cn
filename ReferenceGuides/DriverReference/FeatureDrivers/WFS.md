# WFS(OGC Web Feature Service)
这个插件从一个OGC网络特征服务（[Web Feature Service](https://en.wikipedia.org/wiki/Web_Feature_Service)）资源中读取矢量数据。

用法示例：
```XML
<model driver="feature_geom">
    <features name="states" driver="wfs">
        <url> http://demo.opengeo.org/geoserver/wfs</url>
        <typename>states</typename>
        <outputformat>json</outputformat>
    </features>
    ...
```

属性：

**url：** 加载特征数据的位置

**typename：** 获取的WFS类型名称（如图层）

**outputformat：** 从服务中返回的类型；`json`或`gml`

**maxfeatures：** 请求中返回的特征最大数量

**request_buffer：** 用于缓冲边框请求以保证返回足够数据的地图单元的数量。在使用AGGLite驱动器渲染缓冲线时很有用。
