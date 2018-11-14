# 符号系统参考（Symbology Reference）
osgEarth使用*样式表（Stylesheets）* 渲染*特征（features)* 和*注解（annotations）* 。这个文档列出了样式表中可使用的所有符号属性。不是所有的符号都适用于所有情况，这只是一个主要列表。

跳转到符号：
* [几何体](./SymbologyReference.md#几何体)
* [海拔](./SymbologyReference.md#海拔)
* [拉伸](./SymbologyReference.md#拉伸)
* [图标](./SymbologyReference.md#图标)
* [模型](./SymbologyReference.md#模型)
* [渲染](./SymbologyReference.md#渲染)
* [表面](./SymbologyReference.md#表面)
* [文本](./SymbologyReference.md#文本)
* [覆盖](./SymbologyReference.md#覆盖)

**开发者提示：**

在SDK中，符号系统处于`osgEarth::Symbology`命名空间中，例如，任意符号类的格式都是`AltitudeSymbol`。下述属性与在地球文件中显示的相同；在SDK中，属性可通过访问者以LineSymbol::strokeWidth()等形式获得。


## 数值类型
这些事基本数值类型（value types）。在本页的符号系统列表中，每个属性在其描述中的括号内包含数值类型。

|||
|---|---|
|**float** |浮点数|
|**float with units** |带有单位指示符的浮点数，例如20px（20像素）或10m（10米）|
|**HTML_Color** |十六进制格式的颜色字符串，用于HTML；格式为#RRGGBB或#RRGGBBAA。（例如：#FFCC007F）|
|**integer** |整数|
|**numeric_expr** |数字表达式（简单或JavaScript）|
|**string** |简单文本字符串|
|**boolean** |true或false|
|**string_expr** |文本字符串表达式（简单或JavaScript）|
|**uri_string** |表示资源位置的字符串（如URL或文件路径）。URI可以是绝对的或相对的；相对URI始终相对于引用者的位置，即请求资源的实体。（例如，地球文件中的相对URI将相对于地球文件本身的位置。）|


## 几何体
基本*几何体符号（geometry symbols）* （SDK：`LineSYMBOL`,`PolygonSymbol`,`PointSymbol`）控制矢量数据的颜色和样式。


## 海拔
*海拔符号（altitude symbols）* （SDK：`AltitudeSymbol`）控制特征与其所在位置地形的交互。

|**属性** |**描述** |
|---|----|
|altitude-clamping|控制地形跟随行为|
||**none：** 无固定（clamping）|
||**terrain：** 固定地形并丢弃Z值|
||**relative：** 固定地形并保留Z值|
||**absolute：** 特征的Z值包含其绝对Z值|
|altitude-technique|当`altitude-clamping`设置为`terrain`，选择一种地形跟随方式|
||**map：** 将几何体固定于地图的高程数据瓦片中|
||**drape：** 使用投影纹理固定几何体|
||**gpu：** 将几何体固定在GPU上的地形上|
||**scene：** 将几何体重新固定到新的平铺瓦片（仅限注释）|
|altitude-bindling|当`altitude-technique`是`map`时地图采样的间隔尺寸|
||**vertex：** 固定每个定点|
||**centroid：** 对每个特征仅固定质心|
|altitude-resolution|当`altitude-technique`是`map`（浮点数）时，地形高度采样的高程数据分辨率|
|altitude-offset|应用于几何体Z值的垂直偏移|
|altitude-scale|应用于几何体Z值的尺度因子|

提示：你也可以使用快捷方式激活覆盖或GPU固定，将`altitude-clamping`设置为`terrain-drape`或`terrain-gpu`。


## 拉伸
*拉伸符号（extrusion symbol）* （SDK：`ExtrusionSymbol`）指示osgEarth从源矢量数据创建拉伸几何体；拉伸将2D矢量转换为3D形状。注意：拉伸属性的简单存在将启用拉伸。

|**属性** |**描述** |
|---|----|
|extrusion-height|拉伸矢量数据的距离（numeric-expr）|
|extrusion-flatten|是否将所有拉伸顶点强制为相同的Z值（bool）。例如，如果要拉伸多边形来制作三维建筑物，将此设置为true将强制屋顶平坦，即使底层地形不是（boolean）|
|extrusion-wall-gradient|在三维形状基础上乘以拉伸几何体的填充颜色的因子。这导致三维形状在底部比在顶部更暗，效果很好（float [0..1]；尝试0.75）|
|extrusion-wall-style|osgEarth应该应用于拉伸形状的墙的同一样式表中的另一个样式的名称（string）|	
|extrusion-roof-style|osgEarth应该应用于拉伸形状屋顶的同一样式表中的另一个样式的名称（string）|	

## 表面
在适用的情况下，*表面符号（skin symbol）* （SDK：SkinSymbol）将纹理地图应用于几何体。（目前这只适用于拉伸几何体。）

|**属性** |**描述** |
|---|----|
|skin-library|包含表面资源库的名称|
|skin-tags|字符串集（由空格分隔，包含一个或多个资源标记。当选择要应用的纹理表面时，osgEarth将选择限制为使用其中一个标记的表面。如果省略此属性，则会考虑所有外观。例如，如果要拉伸建筑物，则可能只考虑使用`建筑物`标签纹理（string）|
|skin-tiled|当设置为`true`时，osgEarth将仅考虑选择有`tiled`属性的表面设置为`true`。`tiled`属性表示表面可以用作重复纹理（boolean）|
|skin-object-height|特征真实世界高度的数字表达式（以米为单位）。osgEarth使用此值将选择范围缩小到适合该高度的表面（即，值落在表面的最小/最大对象高度范围之间的表面。（numeric-expr）|
|skin-min-object-height|告诉osgEarth仅考虑最小对象高度大于或等于此值的表面（数字表达式）|
|skin-max-object-height|告诉osgEarth仅考虑最大对象高度小于或等于此值的表面（数字表达式）|
|skin-random-seed|一旦完成滤波（根据上面的属性，osgEarth确定从中选择的最小合适表面集合并随机选择一个。通过设置此种子值，您可以确保相同的“随机”选择每次运行应用程序时都会发生。（整数）|

## 图标



## 模型



## 渲染


## 文本


## 覆盖

















