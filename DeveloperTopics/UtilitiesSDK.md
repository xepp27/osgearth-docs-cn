# 使用SDK
osgEarth *Utils*命名空间包含多种多样有用的类来与地图进行交互。这些都不是osgEarth所必需的，但是它们确实使执行一些常见操作变得容易。

## 数据扫描装置DataScanner
`DataScanner`对本地文件系统的目录进行递归搜索来寻找可以加载`ImageLayer`对象的文件，是加载影像图层完整目录快速又简单的方法。

**注意** 只有*MP地形引擎（MP Terrain Engine）*支持无线数量的影像图层，所以最好将这个引擎与DataScanner一同使用。

像这样使用DataScanner：
```C++
DataScanner scanner;
ImageLayerVector imageLayers;
scanner.findImageLayers( rootFolder, extensions, imageLayers );
```
接下来捏可以在`Map`对象在添加影像图层。

`extensions`参数允许您按拓展名筛选文件。例如，输入“tif,ecw”仅关注这些拓展名的文件，用逗号分隔多个拓展名。

## 细节纹理DetailTexture
`DetailTexture`是一种地形控制器，在地形中应用一种非地理纹理。这是一种可以用来生成“噪声”得到低分辨率地形显示的传统方法：
```C++
DetailTexture* detail = new DetailTexture();
detail->setImage( osgDB::readImageFile("mytexture.jpg") );
detail->setIntensity( 0.5f );
detail->setImageUnit( 4 );
mapnode->getTerrainEngine()->addEffect( detail );
```
试试这个例子，放大到接近地形来查看效果：
```C++
osgearth_detailtex readymap.earth
```
## 对数深度缓冲区Logarithmic Depth Buffer
在全地球应用程序中，您通常希望近距离观察某些物体（如高空飞行器），同时在远处看到地球及其地平线。这给现代图形硬件带来了问题，因为标准深度缓冲精度非常有利于靠近相机的物体，并且观看这样大范围的目标会导致“深度冲突（z-fighting）”问题。

`LogarithmicDepthBuffer`是解决此问题的一种方法，它使用着色器重新投影GPU的深度缓冲区数值，以便在此类场景中更好地使用它们。

它设置起来很简单：
```C++
LogarithmicDepthBuffer logdepth;
logdepth->install( view->getCamera() );
```
你也可以从`osgearth_viewer`或其它例子中启动它：
```C++
osgearth_viewer --logdepth ...
```
由于它确实会在绘制时改变几何体的投影空间坐标，因此需要注意，在自定义着色器中的剪辑空间中没有做任何与此冲突的其它事。

（2014年7月10日：一些osgEarth功能与日志深度缓冲区不兼容，即GPU置入和阴影。虽然深度偏差（Depth Offset）很正常。）

## 格式器Formatters
使用*Formatters*将地理空间坐标格式化为字符串。有两种格式器`LatLongFormatter`和`MGRSFormatter`。格式器使用一个`GeoPoint`并返回一个`std::string`，如下所示：
```C++
LatLongFormatter formatter;
GeoPoint point;
....
std::string = formatter.format( point );
```
### 经纬度格式器（LatLongFormatter）
`LatLongFormatter`将坐标转换为一个字符串。它支持以下格式：

**FORMAT_DECIMAL_DEGREES:**

  34.04582 

**FORMAT_DEGREES_DECIMAL_MINUTES:**

  34.20:30 

**FORMAT_DEGREES_MINUTES_SECONDS:**

  34:14:30 

您还可以为输出字符串指定选项：

**USE_SYMBOLS:**使用度分秒符号

**USE_COLONS:**在各部分间使用冒号

**USE_SPACES:**在各部分间使用空格



## 鼠标坐标工具MouseCoords Tool




