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
|名称（name）||
|驱动器（driver）||
|无数据图像（nodata_image）||
|透明度（opacity）||
|最小范围（min_range）||
|最大范围（max_range）||
|||
## 高程图层



## 模型图层



