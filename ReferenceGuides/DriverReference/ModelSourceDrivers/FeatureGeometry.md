# 特征几何体（Feature Geometry）
这个插件使用样式表在OSG几何体中渲染矢量特征数据。

用法示例：
```XML
<model driver="feature_geom">
    <features driver="ogr">
        <url>world.shp</url>
    </features>
    <styles>
        <style type="text/css">
            default {
                stroke:       #ffff00;
                stroke-width: 2;
            }
        </style>
    </styles>
    <fading duration="1.0"/>
</model>
```
属性：

**geo_interolation：** 如何插入地理线；选项是`great_circle`或`rumb_line`

**instancing：** 对于点模型代替，是否使用GL拖曳（draw-instanced）（默认是`flase`）

共享属性：

所有特征渲染驱动器共享下列属性（上述的附加）：

**styles：** 渲染特征时使用的样式表（参阅[符号系统参考](osgearth-docs-cn/ReferenceGuides/SymbologyReference.md)）

**layout：** 页面数据的布局（参阅[特征和符号](osgearth-docs-cn/UserGuide/FeaturesandSymbology.md)）

**cache_policy：** 缓存策略（参阅[缓存](/UserGuide/Caching.md)）

**fading：** 褪色行为（参阅[褪色](./FeatureGeometry.md#褪色)）

**feature_name：** 用于评估包含特征名称的属性名称的表达

**feature_indexing：** 是否为查询开通索引功能（默认为`false`）

**lighting：** 是否在这个图层上覆盖并设置光照模式（t/f）

**max_granularity：** 用于细分地球上的线的角度阈值（度）

**shader_policy：** 着色器产生的选项（参阅[着色器策略](./FeatureGeometry.md#着色器策略)）

**use_texture_arrays：** 若你的显卡支持，对于墙和屋顶外观是否使用纹理数组（默认为true）


也可以参阅：

`feature_rasterize.earth`示例


## 褪色
若一个模型图层支持褪色，你可以像这样控制它：
```XML
<model ...
    <fading duration  = "1.0"
            max_range = "6000"
            attenuation_distance = "1000" />
```
属性：

**duration:** 淡入的时间（秒）

**max_range：** 开始淡入的距离

**attenuation_distance：** 淡入的距离


## 着色器策略

一些驱动器支持*着色器策略（shader policy）* 允许你控制如何（或是否）为外部几何体创建着色器。例如，如果你想要从一个样式表中加载一个外部模型但是不想osgEarth为它创建着色器：
```XML
<model ...
    <shader_policy>disable</shader_policy>
```
