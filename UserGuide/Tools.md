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
|--tms| make a TMS repo |
|--out path| root output folder of the TMS repo (required) |
|--bounds| xmin ymin xmax ymax| bounds to package (in map coordinates; default=entire map) You can provide multiple bounds| 
|--max-level level| max LOD level for tiles (all layers; default=5). Note: you can set this to a large number to get all available data (e.g., 99). This works fine for files (like a GeoTIFF). But some data sources do not report (or have) a maximum data level, so it’s better to specify a specific maximum. |
|--out-earth earthfile| export an earth file referencing the new repo |
|--ext extension| overrides the image file extension (e.g. jpg) |
|--overwrite| overwrite existing tiles |
|--keep-empties| writes out fully transparent image tiles (normally discarded) |
|--continue-single-color| continues to subdivide single color tiles, subdivision typicall stops on single color images |
|--db-options| db options string to pass to the image writer in quotes (e.g., “JPEG_QUALITY 60”) |
|--mp| Use multiprocessing to process the tiles. Useful for GDAL sources as this avoids the global GDAL lock |
|--mt| Use multithreading to process the tiles. |
|--concurrency| The number of threads or processes to use if –mp or –mt are provided |
|--alpha-mask| Mask out imagery that isn’t in the provided extents. |
|--verbose| Displays progress of the operation |


