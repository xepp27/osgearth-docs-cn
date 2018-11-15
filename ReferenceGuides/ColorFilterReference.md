# 滤色器参考（Color Filter Reference）
*滤色器*是图像图层的内联GLSL处理器。osgEarth地形引擎在GPU上渲染时，通过其图层的滤色器运行每个图像瓦片。您可以将滤色器连接在一起以形成图像处理管道。

osgEarth附带几个已有过滤器; 您可以通过实现`osgEarth::ColorFilter`接口来创建自己的。

这里是如何在一个地球文件中使用滤色器：
```XML
<image driver="gdal" name="world">
    <color_filters>
        <chroma_key r="1" g="1" b="1" distance=".1"/>
    </color_filters>
</image>
```
已有滤色器：
* [BrightnessContrast](./ColorFilterReference.md#BrightnessContrast)
* [ChromaKey](./ColorFilterReference.md#ChromaKey)
* [CMYK](./ColorFilterReference.md#CMYK)
* [Gamma](./ColorFilterReference.md#Gamma)
* [GLSL](./ColorFilterReference.md#GLSL)
* [HSL](./ColorFilterReference.md#HSL)
* [RGB](./ColorFilterReference.md#RGB)

## BrightnessContrast
这个滤色器调整图像的亮度和对比度：
```XML
<brightness_contrast b="0.7" c="1.2"/>
```
`b`和`c`属性是传入值的*百分比*。例如，`c =“1.2”`表示将对比度增加20％。

## ChromaKey
这个过滤器匹配颜色值以将片段透明，提供一种“绿屏”效果：
```XML
<chroma_key r="1.0" g="0.0" b="0.0" distance="0.1"/>
```
在此示例中，我们找到所有红色像素并将它们变为透明。`distance`属性搜索接近指定颜色的颜色，设置为0仅精确匹配。

## CMYK
这个滤色器可以偏移CMYK（青色，品红色，黄色，黑色）颜色级别：
```XML
<cmyk y="-0.1"/>
```
在这里，我们将碎片的“黄度”降低0.1。`c`，`m`，`y`和`k`中的每一个的有效范围是[-1..1]。

## Gamma
这个滤色器执行伽玛校正。您可以为`r`，`g`或`b`中的每一个指定gamma值，也可以将它们全部调整：
```XML
<gamma rgb="1.3"/>
```

## GLSL
GLSL滤波器允许您嵌入自定义GLSL代码，以便您可以以任何方式调整颜色值。只需编写一个GLSL代码块，该代码块对`inout vec4 color`的RGBA颜色变量进行操作：
```XML
<glsl>
    color.rgb *= pow(color.rgb, 1.0/vec3(1.3));
</glsl>
```
此示例与Gamma过滤器完全相同，但直接使用GLSL代码。

## HSL
这个滤色器可以偏移HSL（色调，饱和度，亮度）级别：
```XML
<hsl s="0.1" l="0.1"/>
```
这个例子增加了一点色彩饱和度并且稍微提亮了片段。对于`h`，`s`和`l`中的每一个，有效范围是[-1..1]。

## RGB
这个滤色器可偏移RGB（红色，绿色，蓝色）色彩级别：
```XML
<rgb r="0.1" b="-0.5"/>
```
此示例添加一点红色并减少蓝色通道。对于`r`，`g`和`b`中的每一个，有效范围是[-1..1]。
