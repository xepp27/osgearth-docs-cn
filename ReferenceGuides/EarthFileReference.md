# 地球文件参考
## 地图
*地图*是地球文件中最高级别要素。
```XML
<map name    = "my map"
     type    = "geocentric"
     version = "2" >

    <options>
    <image>
    <elevation>
    <model>
    <mask>
    <libraries>
```
|属性|描述|
|----|-----|
|名称(name)|可读的名称，对于渲染没有影响。|
|类型(type)|坐标系统类型。|
||**地心(geocentric)：** 渲染一个椭球体。|
||**投影(projected)：** 渲染一个“平面”(flat)，投影地图。|
|版本(version|地球文件版本。默认=“2”，设置为较旧版本地球文件格式。|
## 地图选项（Map Options）
这些选项控制地图模型（Map Model）和与整幅地图相关联的渲染属性。
```xml
<map>
    <options lighting                 = "true"
             elevation_interpolation  = "bilinear"
             overlay_texture_size     = "4096"
             overlay_blending         = "true"
             overlay_resolution_ratio = "3.0" >

        <profile>
        <proxy>
        <cache>
        <cache_policy>
        <terrain>
```
|属性|描述|
|---|----|
|光照（lighting)|是否允许光照着色器影响地图|
|高程内插(elevation_interpolation)|重采样高程源数据使用的方法|
||**最近值（nearest：）** 最临近值|
||**平均值（average：）** 相邻值的平均值|
||**双线性（bilinear：）** 两个轴上线性内插|
||**三角形（triangulate）：** 内插遵循三角斜率|
|覆盖纹理尺寸（overlay_texture_size）|设置拼贴的纹理尺寸（投影纹理）|
|覆盖拉伸（overlay_blending）|在拼贴时覆盖几何体是否随着地形拉伸（投影纹理）|
|覆盖分辨率比例（overlay_resolution_ratio）|对于拼贴的几何体，相机附近投影纹理的分辨率与远离相机的分辨率之比。增加该值以改善靠近相机的外观，同时牺牲更远的几何体的外观。 注意：如果您使用的是支持滚动的相机操纵器，则可能需要将其设置为1.0; 否则你会得到悬垂的文物！ 这是一个已知的问题。|
## 地形选项
这些选项控制地形表面的渲染
```XML
<map>
    <options>
        <terrain driver                = "rex"
                 lighting              = "true"
                 min_tile_range_factor = "6"
                 first_lod             = "0"
                 blending              = "false"
                 color                 = "#ffffffff"
                 tile_size             = "17"
                 normalize_edges       = "false"
                 elevation_smoothing   = "false"
                 normal_maps           = "true">
```
|属性|描述|
|---|----|
|驱动器（driver)|加载地形的引擎插件。默认=“mp”。有关每个插件的特定属性，请参阅驱动器参考指南。|
|光照（lighting)|是否在地形上启用GL_LIGHTING。默认情况下，这是未设置的，这意味着它将继承场景的照明模式。|
|最小瓦片范围（min_tile_range_factor）|确定显示瓦片时需要的最接近的距离。该值是瓦片的范围与其的比率。例如，如果瓦片具有10km半径，并且MTRF = 7，则瓦片将在约70km的范围内可见。|
|最低细节级别（first_lod)|地形将显示瓦片的最低细节级别。即地形将永远不会显示比此更低的LOD。|
|拉伸（blending）|将此设置为`true`以在地形的基础几何体上启用GL混合。这使您可以使地球部分透明。这对于看到地下物体很方便。|
|瓦片尺寸（tile_size)|每个地形瓦片的尺寸。每个地形图块都将具有`tile_size` X `tile_size`顶点。|
|正则化化边缘（normalize_edges）|沿着地形瓦片的边缘计算法线向量，使得从一个瓦片到下一个瓦片的光照看起来更平滑。|
|高程平滑（elevation_smoothing）|是否对高程数据插入时的变化进行平滑。这样做可以使不同的高度场数据更加平滑，但是高程不会那么准确。默认= false|
|法线贴图（normal_maps）|是否生成和使用法线贴图代替几何法线。法线贴图与光照一起使用，以创建比单独用三角形表示的更高分辨率地形的外观。默认值取决于引擎。|
|压缩法线贴图（compress_normal_maps）|是否在将法线贴图发送到GPU之前压缩它们。您必须在OpenSceneGraph构建中内置nvidia纹理工具图像处理器插件。默认值为false|
|最小过期帧数（min_expiry_frames）|在到期之前尚未看到地形瓦片的帧数。默认= 0|
|最小过期时间（min_expiry_time）|地形瓦片在到期之前未被剔除的秒数。默认= 0|
## 图像图层
*图像图层*是一个叠加在地图几何体上的栅格化图像。
```XML
<map>
    <image name              = "my image layer"
           driver            = "gdal"
           nodata_image      = "http://readymap.org/nodata.png"
           opacity           = "1.0"
           min_range         = "0"
           max_range         = "100000000"
           attenuation_range = "0"
           min_level         = "0"
           max_level         = "23"
           min_resolution    = "100.0"
           max_resolution    = "0.0"
           max_data_level    = "23"
           enabled           = "true"
           visible           = "true"
           shared            = "false"
           shared_sampler    = "string"
           shared_matrix     = "string"
           coverage          = "false"
           min_filter        = "LINEAR"
           mag_filter        = "LINEAR"
           blend             = "interpolate"
           texture_compression = "auto" >

        <cache_policy>
        <color_filters>
        <proxy>
```
|属性|描述|
|---|----|
|名称（name）|可读图层名称，在引擎中不可用。|
|驱动器（driver）|用于为此图层创建瓦片的插件。有关每个插件的特定属性，请参阅驱动器参考指南。|
|无数据图像（nodata_image）|源代码中表示“无数据”的图像的URI。如果osgEarth将瓦片与此图像匹配，则表明它在该位置找不到任何数据，并且不会渲染瓦片。|
|透明度（opacity）|图层的不透明度，\[0..1]。|
|最小范围（min_range）|最小可见范围，相对于相机，以米为单位。相机更接近时，瓦片将不可见。|
|最大范围（max_range）|最大可见范围，相对于相机，以米为单位。瓦片不会超出此范围。|
|衰减范围（attenuation_range）|与min_range或max_range拉伸的距离。（不支持文本或图标，仅支持几何）|
|最低级别（min_level）|最小可见细节级别。|
|最高级别（max_level）|最大可见细节级别。|
|最低分辨率（min_resolution）|绘制瓦片的最小源数据分辨率。以像素为单位，在源数据的原始单位表示。|
|最高分辨率（max_resolution）|绘制瓦片的最大源数据分辨率。以像素为单位，以源数据的原始单位表示。|
|最大数据级别（max_data_level）|此图像图层可用的新源数据的最大细节级别。通常，驱动器将报告此信息。但您可能希望自己限制它。对于某些没有分辨率限制的驱动器尤其如此，例如光栅化驱动器（agglite）。|
|可用（enabled）|是否在地图中包含此图层。你只能在加载时设置它，它只是一种在地球文件中“注释掉”图层的简单方法。|
|可见（vision）|是否绘制图层。|
|共享（shared）|为此层生成辅助专用采样器，以便可以通过自定义着色器全局访问它。|
|共享图层采样器（shared_sampler）|共享图层在GLSL代码中可用的采样器的统一名称。|
|共享图层矩阵（shared_matrix）|共享图层在GLSL代码中可用的纹理矩阵的统一名称，您可以使用该名称访问上面的`shared_sampler`的正确纹理坐标。|
|覆盖范围（converage）|表示这是一个覆盖层，即传递具有特定语义的离散值的图层。一个例子是“土地利用”层，其中每个像素保持一个值，该值指示该区域是草地，沙漠等。将图层标记为覆盖禁用任何插值、过滤或压缩，因为这些将破坏采样数据GPU上的值。|
|缩小滤波器（min_filter）|用于此层的OpenGL纹理缩小过滤器。选项是NEAREST，LINEAR，NEAREST_MIPMAP_NEAREST，NEAREST_MIPMIP_LINEAR，LINEAR_MIPMAP_NEAREST，LINEAR_MIPMAP_LINEAR|
|放大滤波器（mag_filter）|用于此图层的OpenGL纹理放大滤镜。选项与上面的`min_filter`相同。|
|纹理压缩（texure_compression）|“auto”压缩GPU上的纹理，“none”禁用，“fastdxt”使用FastDXT实时DXT压缩器。|
|拉伸（blend）|“modulate（调制）”将像素与帧缓冲相乘，“interpolate（内插）”基于alpha（def）的帧缓冲区拉伸。|

## 高程图层
*高程图层*为地形引擎提供高程地图格网，osgEarth引擎将所用高程数据合成为一个高程地图，并使用它构建地形瓦片。
```XML
<map>
    <elevation name            = "text"
               driver          = "gdal"
               min_level       = "0"
               max_level       = "23"
               min_resolution  = "100.0"
               max_resolution  = "0.0"
               enabled         = "true"
               offset          = "false"
               nodata_value    = "-32768"
               min_valid_value = "-32768"
               max_valid_value = "32768"
               nodata_policy   = "interpolate" >
```
|属性|描述|
|---|----|
|名称（name）|可读图层名称，未在引擎中使用。|
|驱动器（driver）|用于为此图层创建瓦片的插件。有关每个插件的特定属性，请参阅驱动器参考指南。|
|最低级别（min_level）|最小可见细节级别。|
|最高级别（max_level）|最大可见细节级别。|
|最低分辨率（min_resolution）|绘制瓦片的最小源数据分辨率。以像素为单位，在源数据的原始单位表示。|
|最高分辨率（max_resolution）|绘制瓦片的最大源数据分辨率。以像素为单位，以源数据的原始单位表示。|
|可用（enabled）|是否在地图中包含此图层。你只能在加载时设置它，它只是一种在地球文件中“注释掉”图层的简单方法。|
|偏移（offset）|指示此图层中的高程值是相对偏移而不是真实的地形高程样本。|
|无数据政策（nodata_policy）|如何处理“无数据”值。默认为“interpolate（插值）”，它将插入相邻值来填充。将其设置为“msl”，将“无数据”样本替换为当前海平面值。|
|无数据值（nodata_value）|将此值视为“无数据”。|
|最小可用数据（min_valid_value）|将小于此值的任何内容视为“无数据”。|
|最大可用数据（max_valid_value）|将大于此值的任何内容视为“无数据”。|

## 模型图层
*模型图层*渲染无地形数据，如矢量特征或外部三维模型。
```XML
<map>
    <model name    = "my model layer"
           driver  = "feature_geom"
           enabled = "true"
           visible = "true" >
```
|属性|描述|
|---|----|
|名称（name）|可读图层名称，未在引擎中使用。|
|驱动器（driver）|用于为此图层创建瓦片的插件。有关每个插件的特定属性，请参阅驱动器参考指南。|
|可用（enabled）|是否在地图中包含此图层。你只能在加载时设置它，它只是一种在地球文件中“注释掉”图层的简单方法。|
|可见（vision）|是否绘制图层。|

模型图层还允许你定义一个剪切蒙板（cut-out mask）。地形引擎将在地形表面剪切一个与你提供的*边界几何体*（boundary geometry）匹配的洞，你可以使用*osgearth_boundarygen*工具来创建这样的几何体。

如果你有一个外部地形模型并且想将它插入osgEarth地形中，这很实用。模型必须与地形在同一坐标系统中。
```XML
<map>
    <model ...>
        <mask driver="feature">
            <features driver="ogr">
                ...
```
蒙板（Mask）可以使用任意面特征作为输入，你可以使用内部几何体指定蒙板几何体：
```XML
<features ...>
    <geometry>POLYGON((120 42 0, 121 41 0, 121 40 0))</geometry>
```
或者你可以使用一个形状文件或者其它特征源，这时osgEarth将使用*第一个*源中的特征。

有关示例，请参阅mask.earth。

## 配置文件（Profile）
配置文件告诉osgEarth空间参考系统、地理空间范围以及它应该用于渲染地图瓦片的平铺方案。
```XML
<profile srs    = "+proj=utm +zone=17 +ellps=GRS80 +datum=NAD83 +units=m +no_defs"
         vdatum = "egm96"
         xmin   = "560725.500"
         xmax   = "573866.500"
         ymin   = "4385762.500"
         ymax   = "4400705.500"
         num_tiles_wide_at_lod_0 = "1"
         num_tiles_high_at_lod_0 = "1">
```
|属性|描述|
|---|----|
|srs|地图的空间参照系,可以是WKT字符串、EPSG代码、PROJ4初始化字符串或配置文件名称。有关详细信息，请参阅[空间参考](osgearth-docs-cn/UserGuide/SpatialReferences.md)。|
|vdatum|配置文件的垂直基准，描述如何处理Z值。有关详细信息，请参阅[空间参考](osgearth-docs-cn/UserGuide/SpatialReferences.md)。|
|xmin, xmax, ymin, ymax|地图的地理空间范围。单位是由上面的SRS定义的单位（通常投影地图是米数，地心地图是度数）。|
|num_tiles_\*_at_lod_0|切片层次结构的最高级别的大小。两个方向的默认值均为“1”。（可选）|

## 缓存
为瓦片数据配置缓存。
```XML
<cache driver = "filesystem"
       path   = "c:/osgearth_cache" >
```
|属性|描述|
|---|----|
|驱动器（driver）|缓存中使用的插件，`filesystem`或`leveldb`。|
|路径（path）|路径（相对和绝对）或缓存文件夹或文件。|

## 缓存策略
决定所给的要素如何使用配置好的缓存进行插入的策略。
```XML
<cache_policy usage="no_cache">
```
|属性|描述|
|---|----|
|使用方法（usage）|针对缓存的策略。|
||**read_write：** 使用一个已经配置好的缓存，默认。|
||**cache_only：** 仅从缓存中读取数据，忽略实际数据源，对于离线渲染很有用。|
||**no_cache：** 忽略缓存，始终从源数据中读取。|
|max_age|将早于此值的缓存（以秒为单位）视为已过期。|

## 代理设置（Proxy Settings）
*Proxy settings*帮你为远程数据源设置一个网络代理。
```XML
<proxy host     = "hostname"
       port     = "8080"
       username = "jason"
       password = "helloworld" >
```
这些属性是容易理解的。

## 滤色器（Color Filters）
*滤色器*可以在osgEarth将彩色图层中的颜色数据插入地形前改变其外观的一种可插入着色器。
```XML
<image>
    <color_filters>
        <gamma rgb="1.3">
        ...
```
您可以将多个滤色器链接在一起。 有关滤色器的详细信息，请参阅[滤色器参考](/ReferenceGuides/ColorFilterReference.md)。

## 函数库（Libraries）
预加载任意函数库。
```XML
<libraries>a</libraries>
```
使用“;”作为分隔符列出多个函数库名称。

<libraries>a;b;c;d;e</libraries>

函数库在osg函数库路径中被搜素，函数库名称需要遵循osg节点（nodekit）库名称协议（后缀为osg函数库版本）。
