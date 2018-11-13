# 简单模型（Simple Model）
这个插件简单地加载一个外部三维模型并且可选地在地图坐标系中放置它。

用法示例：
```XML
<model name ="model" driver="simple">
    <url>../data/red_flag.osg.100,100,100.scale</url>
    <location>-74.018 40.717 10</location>
</model>
```

属性：

**url：** 所加载的外部模型

**location：** 放置模型的地图坐标系。SRS是包含地图的其中之一。

**paged：** 若为true，则当摄像机位于该位置的最大范围内时，将对该模型进行分页；若为false，则立即加载模型。

也可以参阅：

`simple_model.earth`示例。
