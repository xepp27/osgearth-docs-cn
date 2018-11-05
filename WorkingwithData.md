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
  通常GeoTiff等格式将其数据保存在扫描行中，但是使用一个瓦片数据集对于osgEarth将会更高效因为它们在内部使用瓦片的方法。
  
  要使用gdal_translate来构建一个瓦片GeoTiff，可以发出以下命令：
  ```C++
  gdal_translate -of GTiff -co TILED=YES input.tif output.tif
  ```
  Take是更进一步使用压缩来节省空间。如果数据不包含透明度，则可以使用内部JPEG压缩：
  ```C++
  gdal_translate -of GTiff -co TILED=YES -co COMPRESS=JPG input.tif output.tif
  ```
  **构建视图**
  添加视图（也称为“金字塔”（pyramids)或“rsets”）有时可以提高osgEarth中大数据源的性能，你可以使用gdaladdo功能来向数据集中添加视图：
  ```C++
  gdaladdo -r average myimage.tif 2 4 8 16
  ```
### 使用osgearth_conv来构建瓦片集
 预先平铺(pre-tiling)你的影像可以显著减少加载时间，尤其是在网络上。事实上，如果你想在网络上处理你的数据，这是唯一的方法。
 
 *osgearth_conv*是osgEarth中的一个低级转换工具，一个有用的应用是以瓦片格式平铺大量GeoTiff（或其它输入）。注意：这个方法仅对于支持写入的驱动器（MBTiles，TMS）有用。
 
 构建一个便携MBTiles文件：
 ```C++
 osgearth_conv --in driver gdal --in url myLargeFile.tif
              --out driver mbtiles --out filename myData.mbtiles
              --out format jpg
 ```
 如果你想从网络服务器提供瓦片，可使用TMS：
 ```C++
 osgearth_conv --in driver gdal --in url myLargeData.tif
              --out driver tms --out url myLargeData/tms.xml
              --out format jpg
 ```
 这样将创建一个文件夹（这里叫做“myLargeData”），你可以在任何标准网络服务器中对它进行配置（如Apache）。
 
 **提示：** 如果你要平铺高程数据，需要添加`--elevation`选项。
 
 **提示：** `jpg`格式不支持透明度，若你的数据是alpha通道，请使用`png`。
 
 只需要输入*osgearth_conv* 可获取选项的完整列表，`--in`和`--out`选项直接对应于地球文件中通常包含的一些属性。
 
### 使用包装程序（packager）来构建瓦片集
 另一个加速在osgEarth中加载影像和高程的方法是构建瓦片集。

 这个方法获取源数据并将其切割为离散瓦片的四叉树层次结构，osgEarth可以快速加载。通常来说，比如你加载一个GeoTiff，osgEarth需要在运行时创建瓦片来构建球形，事先这样做意味着在你运行程序时可以为osgEarth减少工作量。

 **osgearth_package**

 *osgearth_package*是一个用于为osgEarth准备需要用到的源数据的实用程序，时可选的——你可以针对原始数据来运行osgEarth并且它可以正常工作——但是你可以使用*osgearth_package*来构建更加优化的瓦片集，在大部分情况下能获得最优性能。使用方法：
 ```C++
 osgearth_package file.earth --tms --out output_folder
 ```
 这样将加载地球文件（这里是file.earth）中每个数据源，并在`output_folder`文件夹下为每个数据源生成一个TMS存储库。你还可以指定选项：

  --out *path* ：TMS存储库的根输出文件夹
  
  --ext *extension* ：输出文件扩展名
  
  --max-level *level* ：细节的最大级别
  
  -bounds xmin ymin xmax ymas：包装程序的边界（在地图坐标系中；默认-整幅地图）
  
  -out-earth：根据新存储器生成一个输出地图文件
  
  -overwrite：对现有文件强制覆盖
  
  -keep-empties：写入完全透明的影像瓦片（通常丢弃）
  
  -db-options：一个可选OSG选项字符串
  
  -verbos：显示操作进度
  
### 特征数据的空间索引
大矢量特征数据集（如形状文件）将从空间索引中收益。使用*ogrinfo*工具（包括在GDAL/OEG二进制分发中）可以为矢量数据创建一个空间索引，比如：
 ```C++
 ogrinfo -sql "CREATE SPATIAL INDEX ON myfile" myfile.shp
 ```
 对于形状文件，这样将生成一个“.qix”文件存储空间索引信息。
 


