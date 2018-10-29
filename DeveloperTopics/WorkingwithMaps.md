# 使用地图
**地图**是osgEarth的中心数据模型，是影像、高程和特征图层的存储器。
## 从地球文件中载入地图
渲染一张地图最简单的方法是从一个地球文件（earth file）中载入。由于osgEarth使用OpenSceneGraph插件，你可以使用这行代码：
```C++
osg::Node* globe = osgDB::readNodeFile("myglobe.earth");
```
现在你有一个可以添加到场景图形中显示的`osg::Node`，这真的很简单。

这种载入地图的方法通常是一个应用程序需要做的所有事情，如果你想使用API创建地图，请往下读。

## 程序化创建地图
osgEarth提供一个在运行时创建地图的API。

使用API创建一幅地图的基本步骤有：

1. 创建一个地图对象
 
2. 根据你的需要在地图中添加影像和高程图层

3. 创建一个`MapNode`（地图节点）来渲染地图对象

4. 在场景图形中添加你的`MapNode`

你可以随时在地图中添加图层：
```C++
using namespace osgEarth;
using namespace osgEarth::Drivers;

#include <osgEarth/Map>
#include <osgEarth/MapNode>
#include <osgEarthDrivers/tms/TMSOptions>
#include <osgEarthDrivers/gdal/GDALOptions>

using namespace osgEarth;
using namespace osgEarth::Drivers;
...

// Create a Map and set it to Geocentric to display a globe
// 创建一幅地图并将其设置为以地球为中心来显示一个球形
Map* map = new Map();

// Add an imagery layer (blue marble from a TMS source)
// 添加一个影像图层（TMS源中的蓝色纹理）
{
    TMSOptions tms;
    tms.url() = "http://labs.metacarta.com/wms-c/Basic.py/1.0.0/satellite/";
    ImageLayer* layer = new ImageLayer( "NASA", tms );
    map->addImageLayer( layer );
}

// Add an elevationlayer (SRTM from a local GeoTiff file)
// 添加高程图层（本地GeoTiff文件中的SRTM）
{
    GDALOptions gdal;
    gdal.url() = "c:/data/srtm.tif";
    ElevationLayer* layer = new ElevationLayer( "SRTM", gdal );
    map->addElevationLayer( layer );
}

// Create a MapNode to render this map:
//创建一个地图节点来渲染这个地图
MapNode* mapNode = new MapNode( map );
...

viewer->setSceneData( mapNode );

```
## 在运行时使用地图节点（MapNode）
`MapNode`是用来渲染`Map`的场景图形节点。不论你是从地球文件中载入地图或是使用API创建，你都可以在运行时得到地图以及它的`MapNode`来做一些改变。如果你没有准确地使用API来创建`MapNode`，你首先需要对`MapNode`进行引用才能使用。使用静态函数`get`：
```C++
// Load the map
// 载入地图
osg::Node* loadedModel = osgDB::readNodeFile("mymap.earth");

// Find the MapNode
// 寻找地图节点
osgEarth::MapNode* mapNode = MapNode::get( loadedModel );
```
一旦对`MapNode`有了一个引用，就可以得到地图：
```C++
// Add an OpenStreetMap image source
// 添加一个开放街景影像源
TMSOptions driverOpt;
driverOpt.url() = "http://tile.openstreetmap.org/";
driverOpt.tmsType() = "google";

ImageLayerOptions layerOpt( "OSM", driverOpt );
layerOpt.profile() = ProfileOptions( "global-mercator" );

ImageLayer* osmLayer = new ImageLayer( layerOpt );
mapNode->getMap()->addImageLayer( osmLayer );
```
你也可以移除或重新排列图层：
```C++
// Remove a layer from the map.  All other layers are repositioned accordingly
// 从地图中移除一个图层。其它所有图层将重新定位
mapNode->getMap()->removeImageLayer( layer );

// Move a layer to position 1 in the image stack
// 将图层移动到影像堆栈中的位置1
mapNode->getMap()->moveImageLayer( layer, 1 );
```
## 使用图层
`Map`存储了`ImageLayer`（影像图层）和`ElevationLayer`（高程图层）对象，它们有一些特性可以在运行时进行调整。例如，你可以使用API来开启或关闭一个图层或者调整`ImageLayer`的不透明度：
```C++
ImageLayer* layer;
...
layer->setOpacity( 0.5 );  // makes the layer partially transparent使图层部分透明
```




