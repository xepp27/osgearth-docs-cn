# 工具
osgEarth附带了许多工具，可以帮助你处理地球文件和地理空间数据。
## osgearth_viewer
osgearth_viewer可以从命令行加载并显示地图。 osgEarth EarthManipulator（地球控制器）用于控制摄像机，并对地理空间数据的显示进行了优化。

**样例**

`osgearth_viewer earthfile.earth [options]`

|选项|描述|
|--------------------|--------------------|
|--sky|安置SkyNode（太阳，月亮，星星和大气......仅限球状物）|
|--kml\[file.kml]|加载KML或KMZ文件|
|--kmlui|显示用于切换KML地标和文件夹的有限用户界面|
|--coords|显示鼠标位置的地图坐标|
|--ortho|安置正交相机投影|
|--logdepth|在高速模式下激活对数深度缓冲区。|
|--logdepth2|在高精度模式下激活对数深度缓冲区。|
|--uniform \[name]\[min]\[max]|安置统一变量并显示屏幕滑块来控制它的数值，便于调试。|
|--ico|激活OSG的IncrementalCompileOperation（增量编译操作），它将在一系列帧上编译页面对象（减少帧中断）。这实际上是一个OpenSceneGraph选项，但对osgEarth很有用|

## osgearth_version
osgearth_version显示osgEarth的当前版本。

|参数|描述|
|--------------------|--------------------|
|--caps|显示系统功能|
|--major-number|仅显示主要版本号|
|--minor-number|仅显示次要版本号|
|--patch-number|仅显示补丁版本号|
|--so-number|仅显示共享对象版本号|
|--version-number|仅显示版本号|

## osgearth_cache
osgearth_cache可用于管理osgEarth的缓存。有关缓存的更多信息，请参阅[缓存](.\Caching.md)。osgearth_cache最常见的用法是使用--seed参数以非交互方式填充缓存。

**样例**

`osgearth_cache --seed file.earth`

|参数|描述|
|--------------------|--------------------|
|--list|列出.earth文件中有关缓存的信息|
|--seed|在.earth文件中填充缓存|
|--estimate|显示对执行此填充操作所需的拼贴数量、磁盘空间和时间的估计|
|--mp|使用多处理来处理贴片。由于可以避免全局GDAL锁定，因此对于GDAL源有用|
|--mt|使用多线程处理贴片|
|--concurrency|提供-mp或-mt时使用的线程或进程数|
|--min-level level|填充的最低LOD级别（默认=0）|
|- max-level level|填充的最高LOD级别（默认=最高可得）|
|--bounds xmin ymin xmax ymax|填充的地理空间边界框（在地图坐标中；默认=整个地图）|
|--index shapefile|加载形状文件（.shp）并使用功能扩展区设置缓存填充边界框。对于形状文件中的每个特征，添加一个边界框（类似于--bounds）来约束要缓存的区域|
|--cache-path path|覆盖.earth文件中的缓存路径|
|--cache-type type|覆盖.earth文件中的缓存类型|
|--purge|清除.earth文件中的图层缓存|

## osgearth_conv

osgearth_conv将一个贴片源的内容复制到另一个。所有参数都是配置名称/值对，因此需要查看每个驱动程序的选项结构头文件中的选项。 当然，驱动程序必须支持写入输出（通过实现ReadWriteTileSource接口）。 “输入”属性来自GDALOptions getConfig，“输出”属性来自MBTilesOptions getConfig。

**样例**

`osgearth_conv --in driver gdal --in url world.tif --out driver mbtiles `

|参数|描述|
|--------------------|--------------------|
|--in \[name]\[value]|设置输入属性的值|
|--out \[name]\[value]|设置输出属性的值|
|--elevation|转换为高程数据（而不是影像数据）|
|--profile \[profile]|重新投影到目标配置文件，如WGS84|
|--min-level \[int]|可复制的最小细节级别|
|--max-level \[int]|可复制的最大细节级别|
|--threads \[n]|使用的线程（注意可能会崩溃，对于GDAL输入无效）|
|--extents \[minLat]\[minLong]\[maxLat]\[maxLong]|纬度/经度扩展|

## osgearth_package
osgearth_package从地球文件中创建可再分发的基于TMS的包。

**样例**

`osgearth_package --tms file.earth --out package`

|参数|描述|
|--------------------|--------------------|
|--tms|制作一个TMS报告|
|--out path|TMS报告输出的根目录（必填）|
|--bounds| xmin ymin xmax ymax|包的边界（在地图坐标系中；默认：整个地图）你可以提供多个边界| 
|--max-level level|贴片的最大LOD等级（所有图层；默认=5）。注意：你可以将其设置为较大的数字以获取所有可用数据（例如，99），适用于文件（如GeoTIFF）。但是一些数据源不报告（或具有）最大数据级别，因此最好指定特定的最大值|
|--out-earth earthfile|导出引用新报告的地球文件|
|--ext extension|覆盖图像文件扩展名（例如jpg）|
|--overwrite|覆盖现有贴片|
|--keep-empties|写出完全透明的图像贴片（通常是被废弃的）|
|--continue-single-color|继续细分单色贴片，细分一般停留在单色图像上|
|--db-options|db选项串以引用传递给图像写入器（例如，“JPEG_QUALITY 60”）|
|--mp|使用多处理来处理贴片，这对GDAL源有用，因为可以避免全局GDAL锁定|
|--mt|使用多线程处理贴片|
|--concurrency|提供-mp或-mt时使用的线程或进程数|
|--alpha-mask|屏蔽不在提供范围内的图像|
|--verbose|显示操作的进度|

## osgearth_tfs

osgearth_tfs从特征源（如形状文件）生成TFS数据集。将特征预处理为TFS提供的网格结构，可以显著提高大数据集的性能。此外，生成的TFS包可以由任何标准网络服务器提供，网络启用数据集。

**样例**

`osgearth_tfs filename`

|参数|描述|
|--------------------|--------------------|
|filename|形状文件（或其它特征源数据文件）|
|--first-level level|特征被添加到四分树的第一层|
|--max-level level|特征四分树的最大层|
|--max-features|每片贴片中特征的最大数目|
|--grid|生成具有指定分辨率的单层网格。默认单位是米（如50,100km,200mi）|
|--out|目标目录|
|--layer|要写入元数据文档的图层的名称
|--description|要写入元数据文档的图层的抽象/描述|
|--expression|运行特征源所用的表达，具体到特征源|
|--order-by|如果尚未包含于表达中，对特征进行排序。附带DESC按降序排列！|
|--crop|裁剪特征，而不是进行中心检查。启用裁剪时可以用于多个贴片|
|--dest-srs|osgEarth可以理解的任何格式的目标SRS串（wkt，proj4，epsg）。如果没有特定的，则将使用源数据SRS|

## osgearth_backfill
osgearth_backfill是一种专用工具，用于后处理TMS数据集。一些网络地图服务在不同的缩放级别使用完全不同的数据集。例如，他们可能会使用NASA BlueMarble图像，达到4级后突然切换到LANDSAT数据。这适用于2D滑动地图可视化，但在3D中查看时可能会在视觉上分散注意力，因为不同LOD相邻贴片看起来完全不同。

osgearth_backfill允许您像通常那样生成TMS数据集（使用osgearth_package或其他工具），然后从指定的更高级别的详细信息中“回填”较低级别的详细信息。例如，您可以指定最大级别10，并且将根据级别10中获得的数据重新生成0-9级。

**样例**

`osgearth_backfill tms.xml`

|参数|描述|
|--------------------|--------------------|
|--bounds xmin ymin xmax ymax|回填的边界（在地图坐标中；默认=整个地图）|
|--min-level level|停止回填的最低级别（默认=0）|
|--max-level level|开始回填的级别（默认=inf）|
|--db-options|db选项串以引用传递给图像写入器（例如，“JPEG_QUALITY 60”）|

## osgearth_boundarygen
osgearth_boundarygen生成可以与osgEarth <mask>图层一起使用的边界几何体，以便将外部模型固定到地形中。

**样例**

`osgearth_boundarygen model_file \[options]`

|参数|描述|
|--------------------|--------------------|
|--out file_name|边界几何体的输出文件（默认为boundary.txt）|
|--no-geocentric|跳过地心重投影（适用于平面数据库）|
|--convex-hull|计算凸壳，而不是完整边界|
|--verbose|在控制台显示进度|
|--view|在3D窗口中查看结果|
|- tolerance N|小于此距离的顶点将合并（0.005）|
|--precision N|输出坐标将有这么多有效数字（12）|

## osgearth_overlayviewer
osgearth_overlayviewer是一个用于调试osgEarth覆盖装饰器功能的实用程序。它显示了两个窗口，一个窗口是显示地图的普通视图，另一个窗口显示用于覆盖计算的边界视锥。
