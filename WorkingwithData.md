# 使用数据
## 在哪里找到数据
帮助我们在列表中添加有用的免费数据源。

**栅格数据**
* [ReadyMap.org](http://web.pelicanmapping.com/readymap-tiles/)——为osgEarth开发者提供免费15米影像、高程和街景瓦片
* [USGS National Map](https://www.usgs.gov/core-science-systems/national-geospatial-program/national-map)——美国的高程、正射影像、海道测量、地理名称、边界、交通、建筑和土地覆盖产品
* [NASA BlueMarble](https://visibleearth.nasa.gov/view_cat.php?categoryID=1484)——NASA的全球影像（包括地形和水深地图）
* [Natural Earth](http://www.naturalearthdata.com/)—免费多尺度矢量和栅格地图数据
* [Virtual Terrain Project](http://vterrain.org/Imagery/WholeEarth/)——多源全球影像
* [Bing Maps](https://www.microsoft.com/en-us/maps/choose-your-bing-maps-api)——微软的全球影像和地图数据

**高程数据**
* [CGIAR](http://srtm.csi.cgiar.org/)——SRTM和ETOPO获取的全球90米高程数据([CGIAR European mirror](ftp://xftp.jrc.it/pub/srtmV4/))
* [SRTM30+](ftp://topex.ucsd.edu/pub/srtm30_plus/)——全球范围高程覆盖（包括水深）
* [GLCF](http://glcf.umiacs.umd.edu/data/srtm/)——UMD的全球土地覆盖设施（Global Land Cover Facility），（镶嵌有LANDSAT数据）
* [GEBCO](https://www.gebco.net/)——（一些海洋水深图表）

**特征数据**
* [OpenStreetMap](https://www.openstreetmap.org/#map=4/36.96/104.17)——全球范围社会来源的街道和土地使用数据（矢量和栅格瓦片）
* [Natural Earth](http://www.naturalearthdata.com/)——免费多尺度矢量和栅格地图数据
* [DIVA-GIS](http://www.diva-gis.org/gData)——任意国家的免费低分辨率矢量数据

## 准备你自己的数据时的一些提示
### 处理本地源数据
如果你有地理数据并且希望它在osgEarth中显示，一般可以使用GDAL驱动器。如果你准备这样做，先试着把它安装上。如果你觉得这样太慢了，有一些对瓦片接口（tiled access）数据进行优化的方法。

  **对你的数据进行重新投影**
  如果没有必需的坐标系统，osgEarth将迅速对你的数据进行重新投影，比如如果你想在一个测量地球（epsg：4326）上显示一张UTM影像。然而，如果你的数据已经有了正确的坐标系统，osgEarth将更加迅速地完成这项工作。你可以使用任意工具来重新投影你的数据，如GDAL、全球地图（Global Mapper）或ArcGIS。
  
  比如，使用gdal_warp来重新投影一张UTM影像到测量地球：
  ```C++
  gdalwarp -t_srs epsg:4326 my_utm_image.tif my_gd_image.tif
  ```
  **构建内部瓦片**
  
  
  **构建视图**
