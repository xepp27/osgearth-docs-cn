# AGGLite Rasterizer
这个插件使用*agglite*函数库对图像瓦片的特征数据进行栅格化，是现有的一种在地图上渲染矢量几何体的简单且有效方法。

用法实例：
```XML
<image driver="agglite">
    <features driver="ogr">
        <url>world.shp</url>
    </features>
    <styles>
        <style type="text/css">
            default {
                stroke:       #ffff00;
                stroke-width: 500m;
            }
        </style>
    </styles>
</image>
```
属性：

**optimize_line_sampling（优化线采样）:**

  对线数据进行下采样，使其分辨率不高于我们打算对其进行栅格化的图像。如果不这样做，则存在缓冲操作在非常高分辨率的输入数据上永远存在的风险。（可选）
  
也可以参考：
  `feature_rasterize.earth`示例
