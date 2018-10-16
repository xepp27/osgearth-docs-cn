# 使用地球文件
地球文件是地图的XML描述。创建一个地球文件是配置一幅地图并快速启动和运行它最简单的方法。在osgEarth存储库中，您将在`tests`文件夹中找到许多地球文件样例，涵盖各种主题并演示各种功能。我们鼓励探索并试用它们！

也可以参阅：[地球文件相关](osgearth-docs-cn/ReferenceGuide/EarthFileReference.md)

## 地球文件内容
osgEarth使用一种基于XML的称为地球文件的文件格式来指定源数据如何变成OSG场景图。地球文件有`.earth`的扩展名，但它是XML。

基本来说地球文件可以帮助您指定：
- 创造的地图的类型（地心或投影）
- 用到的影像、高程、矢量和模型源
- 数据缓存的位置

## 一个简单的地球文件
这是一个非常简单的示例，它从本地文件系统上的GeoTIFF文件中读取数据，并将其呈现为地心圆形地球场景：
```C++
<map name="MyMap">
    <image name="bluemarble" driver="gdal">
        <url>world.tif</url>
    </image>
</map>
```
此地球文件使用名为`bluemarble`的单个GeoTIFF图像源创建名为`MyMap`的地心地图。`driver`属性告诉osgEarth用于加载图像的哪个插件。（osgEarth使用插件框架从不同的源加载不同类型的数据。） 

某些子元素（在`图像`中）特定于所选择的驱动程序。要了解有关驱动程序以及如何配置驱动程序的更多信息，请参阅[驱动器相关](osgearth-docs-cn/ReferenceGuide/DriverReference.md)。

## 多个图像层
osgEarth支持具有多个图像源的地图。这允许您创建带有交通覆盖的基础图层的地图，或为位于较低分辨率基础地图上的特定区域提供高分辨率区域。

要在地球文件中添加多幅图像，可简单地将多个“图像”块添加到地球文件中。
`C++
<map name="Transportation">

    <!--Add a base map of the blue marble data-->
    <image name="bluemarble" driver="gdal">
        <url>c:/data/bluemarble.tif</url>
    </image>

    <!--Add a high resolution inset of Washington, DC-->
    <image name="dc" driver="gdal">
        <url>c:/data/dc_high_res.tif</url>
    </image>

</map>
`
上面的地图使用GDAL驱动程序从本地数据源提供两个图像。定义多个图像源时，顺序很重要：osgEarth按照它们在地球文件中出现的顺序显示它们。 

*提示：地球文件中的相对路径被理解为相对于地球文件本身。*

## 添加高程数据
在地球文件中添加高程数据（有时被称为“地形数据”）和添加图像很相似。使用一个如下`高程`块：
```C++
<map name="Elevation">

    <!--Add a base map of the blue marble data-->
    <image name="bluemarble" driver="gdal">
        <url>c:/data/bluemarble.tif</url>
    </image>

    <!--Add SRTM data-->
    <elevation name="srtm" driver="gdal">
        <url>c:/data/SRTM.tif</url>
    </elevation>

</map>
```
此地球文件包含一个基本`bluemarble`图像以及从本地GeoTIFF文件中加载的高程网格。你可以根据需要添加多个高程图层；osgEarth将它们合成一个单目网格。

与图像一样，顺序很重要——例如，如果您有整个世界的低分辨率的基本高程数据源和一个高分辨率城市区域，则需要**先**指定基础数据，然后是高分辨率区域。

*注意：对于高程图层，osgEarth仅支持单通道16位整数或32位浮点数据。*

## 缓存
由于osgEarth按需呈现数据，因此在准备用于显示的贴片时需要做一些工作。缓存功能使osgEarth可以在下一次保存这次工作的结果，而不是每次都重新处理贴片。这样可以提高性能并避免多次下载相同的数据。

缓存设置的示例：
```C++
<map name="TMS Example">

    <image name="metacarta blue marble" driver="tms">
        <url>http://readymap.org/readymap/tiles/1.0.0/7/</url>
    </image>

    <options>
        <!--Specify where to cache the data-->
        <cache type="filesystem">
            <path>c:/osgearth_cache</path>
        </cache>
    </options>

</map>
```
这是地球文件为osgEarth显示指定缓存的最基本方法。它告诉osgEarth启用缓存并缓存到文件夹`c:/osgearth_cache`。缓存路径可以是相对的或绝对的；相对路径相对于地球文件本身。

有许多方法可以配置缓存；更多详细信息请参阅[缓存](.\Caching.md)部分。
