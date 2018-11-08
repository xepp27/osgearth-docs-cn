# Color Ramp
Color Ramp插件除了使用颜色灰阶文件外，还是用基础高程场从单波段数据集（如高程或温度）中生成RGBA图像。

用法示例：
```XML
<image name="color ramp" driver="colorramp">
    <elevation name="readymap_elevation" driver="tms">
        <url>http://readymap.org/readymap/tiles/1.0.0/9/</url>
    </elevation>
    <ramp>..\data\colorramps\elevation.clr</ramp>
</image>
```

灰阶文件（Ramp files）：

定义数值如何对应于颜色的文件，每条线上都存储了一个数值和对应的RGB颜色，其值在0-255范围内。

示例：
```XML
0 255 0 0
1000 255 255 0
5000 0 0 255
```
属性

**elevation：** 所采样的高程图层的定义。

**ramp：** 对图层着色时使用的灰阶文件路径。

也可以参考：
  `tests`文件夹中的`colorramp.earth`示例。
