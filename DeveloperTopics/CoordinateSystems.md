# 坐标系统
OpenGL、OSG和osgEarth中使用了几种不同的坐标系统和参考体系，有时会很困惑哪个是哪个。

## OpenSceneGraph/OpenGL坐标空间
这里对OpenGL和OSG中使用的各种坐标系统有一个简要的说明。如果需要更多详细的说明（附图），可以直接阅读有关该主题的优秀教程：
[OpenGL转换](http://www.songho.ca/opengl/gl_transform.html)

### 模型坐标（Model Coordinates）
模型（或对象）空间指的是几何体（如地形贴片、飞机模型等）的实际坐标。在OSG中，模型坐标可能是绝对的或者它们可能经过一个OSG`Transform`转换。

我们经常使用两种类型的模型坐标：*世界（world）*和*本地（Local）*。

*世界坐标*表示为绝对位置，它们没有经过转换。*本地坐标*经过了转换而相对于某些参考点（在世界坐标中）。

为什么使用本地坐标？因为OpenGL硬件只能为顶点位置处理32位的数值，但是在一些如osgEarth的系统中，我们需要使用大数值来表示位置，无法在不超出32位精度限制的情况下完成。解决方法就是使用本地坐标。OSG使用一个双精度`MatrixTransform`来创建一个本地原点(0,0,0)，然后我们可以相对于它表示我们的数据。

### 视图坐标（View Coordinates）
视图空间（有时称为相机或眼空间）表示了几何体相对于相机本身的位置。相机位于原点(0,0,0)，坐标轴是：
```XML
+X : Right
+Y : Up
-Z : Forward (direction the camera is looking)
```
在osgEarth中，视图空间在顶点着色器（vertex shaders）中使用得很多——它们在限制于32位精度的GPU上操作，视图空间在相机处有一个本地原点。

### 裁剪坐标（Clip Coordinates）
*裁剪坐标*是在应用视景体（view volume，也称相机视锥体（camera frustum））后所得到的。视锥体确定了的视点所能看到的范围，得到的坐标在这个系统中：
```XML
+X : Right
+Y : Up
+Z : Forward
```
裁剪空间使用四维齐次坐标，数值范围包含相机视锥体，可表示为：
```XML
X : [-w..w] (-w = left,   +w = right)
Y : [-w..w] (-w = bottom, +w = top)
Z : [-w..w] (-w = near,   +w = far)
W : perspective divisor
```
注意，表示深度的Z值是非线性的，靠近近平面的精度要高得多。

当你需要在场景中采样或操纵深度信息时，着色器中的裁剪空间非常有用。
