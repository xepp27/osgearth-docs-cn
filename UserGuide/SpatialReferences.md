# 空间参考
我们使用坐标在地球上确定位置，即以某种精度精确定位地图上特定位置的一组数字。但是只知道坐标是不够的；你需要知道如何理解它们。

**空间参考**（SRS）将一组坐标映射到地球上的相应实际位置。
例如，给定地球上某个位置的坐标：
```C++
(-121.5, 36.8, 2000.0)
```
若你不知道如何使用它们，这些数字是没有意义的。因此需要使用一些参考信息与之结合：
```C++
Coordinate System Type: Geographic
Units:                  Degrees
Horizontal datum:       WGS84
Vertical datum:         EGM96
```
现在就可以在地球上精确确定点的位置、它与其它点的关系，以及如何转换为其它表现形式。

## SRS的组成
一个*空间参考*，即SRS，包含：
* [坐标系类型](./SpatialReferences.md#坐标系类型)
* [水平基准面](./SpatialReferences.md#水平基准面)
* [垂直基准面](./SpatialReferences.md#垂直基准面)
* [投影](./SpatialReferences.md#投影)

### 坐标系类型
osgEarth支持三种基本坐标系类型：
- **地理**——一种全地球椭球模型。坐标是以度为单位的球面角度（经度和纬度），包括WGS84和NAD83。（[了解更多](https://en.wikipedia.org/wiki/Geographic_coordinate_system)）
- **投影**——一种在地球有限区域内的局部坐标系，并将其“投影”到二位笛卡尔（X,Y）平面，包括UTM、US State Plane和Mercator。 （[了解更多](https://en.wikipedia.org/wiki/Map_projection)）
- **ECEF**——一种在整个地球的笛卡尔系统。ECEF=Earth Centered Earth Fixed（以地球为中心的地球固定），是一个三维笛卡尔系统（X,Y,Z），地球中心的原点（0,0,0）；X轴与纬度/经度（0,0）相交，Y轴与纬度/经度（0，-90）相交，Z轴与北极相交。 ECEF是osgEarth呈现图形的原生系统。（[了解更多](https://en.wikipedia.org/wiki/ECEF)）

### 水平基准面
基准是用于进行地理空间测量的一个参考点（或一组点）。地球上相同位置可以根据使用的数据而具有不同的坐标，有两类基准：

**水平基准**测量地球上的位置。由于地球不是一个完美的球体，甚至不是一个完美的椭圆体，因此指定的基准面通常设计成接近于指定区域的地球形状。常见基准包括北美的**WGS84**和**NAD83**，以及欧洲的**ETR89**。

### 垂直基准面
**垂直基准**测量高程。有几类垂直基准；osgEarth支持大地测量（基于椭圆体）和大地水准面（基于行星周围的一组高程点）。

osgEarth内置了以下垂直基准：
- 大地坐标系——默认; osgEarth使用水平基准椭球作为参考
- EGM84大地水准面
- EGM96大地水准面——通常称为MSL；用于DTED和KML
- EGM2008大地水准面

默认情况下，osgEarth中的SRS使用大地垂直基准，即高度被测量为“基于椭圆体（HAE）的高度”。

### 投影
SRS也具有投影，即将椭圆体上的点转换为二维平面（和背面）的数学公式。

osgEarth支持数千个已知投影（通过GDAL/OGR工具包）。主要包括：
- UTM（通用横轴墨卡托）
- 球极平面
- LCC（兰伯特等角圆锥）

每个都具有特定的特征，使其适用于某些类型的应用。请参阅维基百科上的[地图投影](https://en.wikipedia.org/wiki/Map_projection)以了解更多信息。

## SRS的表示
有许多方法可以定义SRS。osgEarth支持以下内容。
### WKT（众所周知的文字）
WKT是用于描述坐标系的OGC标准，通常保存于“.prj”文件中，并携带一些地理空间数据，如形状文件或图像。

以下是*UTM Zone 15N*投影的WKT表示：
```C++
PROJCS["NAD_1983_UTM_Zone_15N",
    GEOGCS["GCS_North_American_1983",
        DATUM["D_North_American_1983",
            SPHEROID["GRS_1980",6378137.0,298.257222101]],
        PRIMEM["Greenwich",0.0],
        UNIT["Degree",0.0174532925199433]],
    PROJECTION["Transverse_Mercator"],
    PARAMETER["False_Easting",500000.0],
    PARAMETER["False_Northing",0.0],
    PARAMETER["Central_Meridian",-93.0],
    PARAMETER["Scale_Factor",0.9996],
    PARAMETER["Latitude_Of_Origin",0.0],
    UNIT["Meter",1.0]]
```
### PROJ4
PROJ4是osgEarth和数百个其他地理空间应用程序和工具包使用的地图投影工具包，它有描述SRS的简易表示。以下是与上文SRS相同的PROJ4格式：
```C++
+proj=utm +zone=15 +ellps=GRS80 +units=m +no_defs
```
PROJ4对于所有常见组件（如UTM区域和基准）都有数据表，因此您不必像使用WKT那样明确定义所有内容。

### EPSG代码
EPSG（现已解散的欧洲石油勘探组）建立了一个数字代码表，用于引用常用投影。你可以在[这里](http://spatialreference.org/ref/epsg)浏览其列表。 osgEarth接受EPSG代码；为上面的例子可以表示为：
```C++
epsg:26915
```
EPFG是一种非常简易的表达方法，osgEarth所需要的OGR/PROJ4包含大量EPSG代码的表格。

### 别名
最后一类是命名SRS。有一些SRS非常常见，因此我们给它们简易表达。包括：

**WGS84：**世界地理测量1984地理系统

**球形墨卡托：**球形墨卡托（常用于网络制图系统）

**plate-carre：**WGS84投影平面（X=经度，Y=纬度）

## 在osgEarth中使用空间参考
有几种方法可以在osgEarth中使用SRS，最简单的方法是使用`GeoPoint`类。我们先看看如何创建一个SRS，然后再继续学习。

### 空间参考API
`SpatialReference`类表示SRS。osgEarth中的许多类和函数都需要SRS。以下是在代码中创建的方式：
```C++
const SpatialReference* srs = SpatialReference::get("epsg:4326");
```
信这将会创建一个人SRS。`get()`函数可以接受我们上述所提到的任何一种SRS表达形式：WKT、PROJ4、EPSG或者Alases。

如果你需要一个垂直基准面的SRS，使用第二个参数来表达。osgEarth支持egm84、egm96以及egm2008。这样使用：
```C++
srs = SpatialReference::get("epsg:4326", "egm96");
```
能够访问SRS的组件类型有时也很有用。例如，每个投影的SRS都具有基于它的基础地理SRS。可以通过以下方式得到：
```C++
geoSRS = srs->getGeographicSRS();
```
如果你要将投影点转换为纬度/经度，将会输出你想要的SRS。

你也可以获取与任何SRS相对应的地心（ECEF）SRS，如下所示：
```C++
geocentricSRS = srs->getGeocentricSRS();
```
`SpatialReference`有许多函数来实现转换等功能，可以查询它们的头文件来了解。但是在实际中最好直接使用`GeoPoint`等类而不是`SpatialRefence`。

### GeoPoint API
`GeoPoint`是二维或三维点的地理参考。（“地理参考”意味着坐标值与SRS相对应——也就是在地图上绘制该点所需要的所有信息都是自包含的。）还有其它“Geo”类，包括`GeoExtent`（一个边界框）和`GeoCircle`（一个边界圆）。

创建一个二维GeoPoint的方法如下：
```C++
GeoPoint point(srs, x, y);
```
也可以利用高程来创建一个三维GeoPoint：
```C++
GeoPoint point(srs, x, y, z, ALTMODE_ABSOLUTE);
```
`ALTMODE_ABSOLUTE`是*高程模式*，当你指定三维坐标时需要它：

`ALTMODE_ABSOLUTE`：

  Z相对于SRS的垂直基准面，如基于椭球面的高度或基于大地水准面的高度。

`ALTMODE_RELATIVE`：

  Z相对于点位之下地形的高度。

现在有了`GeoPoint`可以进行转换了。将其转换到其它SRS：
```C++
GeoPoint point(srs, x, y);
GeoPoint newPoint = point.transform(newSRS);
```
这里有更加具体的例子。将基于纬度/经度（WGS84）的点用UTM Zone 15N表示：
```C++
const SpatialReference* wgs84 = SpatialReference::get("wgs84");
const SpatialReference* utm15 = SpatialReference::get("+proj=utm +zone=15 +ellps=GRS80 +units=m");
...
GeoPoint wgsPoint( wgs84, -93.0, 34.0 );
GeoPoint utmPoint = wgsPoint.transform( utm15 );

if ( utmPoint.isValid() )
   // do something
```
由于并不是某一个SRS中的每一个点都可以转换到另一个SRS中，所以要经常检查`isvalid()`。例如，UTM Zone 15仅对经度的6度带有定义——数值超过这个范围太大可能会失败。



