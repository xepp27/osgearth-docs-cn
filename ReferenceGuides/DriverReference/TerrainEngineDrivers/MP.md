# MP
osgEarth的默认地形引擎使用瓦片级别多通道混合技术来渲染无限数量的图像图层。

用法示例：
```XML
<map>
    <options>
        <terrain driver                   = "mp"
                 skirt_ratio              = "0.05"
                 color                    = "#ffffffff"
                 normalize_edges          = "false"
                 incremental_update       = "false"
                 quick_release_gl_objects = "true"
                 min_tile_range_factor    = "7.0"
                 cluster_culling          = "true" />
```

属性：

**skirt_ratio：** “边缘”（skirt）是一个隐藏了相邻不同细节级别瓦片间缝隙的垂直几何体。这个属性设置了边缘高度与瓦片宽度之间的比例。

**color：** HTML格式的底层地形（无图像）的颜色。默认=“ffffffff”（不透明的白色）。你可以调整alpha来改变透明度。

**normalize_edges：** 瓦片边界上法线矢量的后处理，使得它们在瓦片上平滑，不使用图像时使得瓦片边界变得不可见。

**incremental_update：** 启用时，当地图模型改变后（如图层添加或移除）仅可见的瓦片更新。不可见地形瓦片（如那些处于低LOD的）不会更新，直到它们可见。

**quick_release_gl_objects：**  如果为true，安装一个模块，该模块会在瓦片输出时立即释放GL资源。这可以防止在高速穿越平铺地形时的内存启动。禁用快速释放可能有助于实现更一致的帧速率。

通用属性：

**min_tile_range_factor：** 所有瓦片的“最大可见距离”比例。最大可见距离=瓦片半径*这个数值（默认7.0）

**cluster_culling：**  默认情况下，聚集筛选（cluster culling）会丢弃后向切片。您可以将其设置为`false`来禁用它，如您想要进入地下并查看表面。
