
```XML
<!--
osgEarth Sample : Layer Opacity
This example tests setting a layers initial opacity in the .earth file 
设置一个不透明图层
-->

<!--地图名为hi-res inset，地心坐标系，地球文件版本2（默认） -->
<map name="hi-res inset" type="geocentric" version="2">
    
<!--底层影像名为world-tiff，驱动器为gdal，不使用缓存 -->
    <image driver="gdal" name="world-tiff" cache_enabled="false">
        <url>../data/world.tif</url>
        <caching_policy usage="no_cache"/>
    </image>  
    
<!--添加影像名为boston_inset，驱动器为gdal，不透明度为0.3 -->
<!--影像带有经纬度信息-->
    <image name="boston_inset" driver="gdal" opacity="0.3">
        <url>../data/boston-inset-wgs84.tif</url>
    </image>
   
</map>
```
