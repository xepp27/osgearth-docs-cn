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
在适用的情况下，*表面符号（skin symbol）* （SDK：`SkinSymbol`）将纹理地图应用于几何体。（目前这只适用于拉伸几何体。）

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
*图标符号（icon symbol）*（SDK：`IconSymbol`）描述了二维图标的外观。 图标用于不同的事物，最常见的是：
* 代替点模型——用图标代替几何体
* 放置注释

|**属性** |**描述** |
|---|----|
|icon|图标图像的URI（uri-string）|
|icon-library|包含图标的资源库名称（可选）|
|icon-placement|对于模型替换，描述了osgEarth如何用图标代替几何体：|
||**vertex：** 用图标替换几何体中的每个顶点|
||**interval：** 根据`icon-density`属性，沿几何体以固定间隔放置图标|
||**random：** 根据图`icon-density`属性，在几何体中随机放置图标|
||**centroid：** 在几何体的质心处放置一个图标|
|icon-density|对于`interval(间隔)`或`random(随机)`的`icon-placement`设置，此属性提示osgEarth应放置多少个实体。该单位约为“每公里的单位”（对于线性数据），或对于多边形数据为“每平方公里单位”。（浮点数）|
|icon-scale|按此数量缩放图标（浮点数）|
|icon-heading|沿中心轴旋转图标（浮点数，度）|
|icon-declutter|激活此图标的清理（decluttering）。osgEarth将尝试自动显示或隐藏事物，以便它们不会在屏幕上重叠。（bool）|
|icon-align|设置图标相对于其锚点的位置。有效值的格式为“水平-垂直”，包括：|
||* `left-top`|
||* `left-center`|
||* `left-bottom`|
||* `center-top`|
||* `center-center`|
||* `center-bottom`|
||* `right-top`|
||* `right-center`|
||* `right-bottom`|
|icon-random-seed|对于随机放置操作，设置此种子，以便每次运行应用程序时可以重复随机化（整数）|
|icon-occlusion-cull|是否剔除遮挡文本，以便在视线受到地形阻挡时不显示|
|icon-occlusion-cull-altitude|当视线被地形阻挡时，观察者高度（MSL）开始剔除遮挡|

## 模型
*模型符号（model symbol）*（SDK：`ModelSymbol`）描述了外部三维模型。与图标一样，模型通常用于：
* 代替点模型——用三维模型代替几何体
* 模型注释

|**属性** |**描述** |
|---|----|
|model|三维模型的URI（uri-string）。使用这个**或** `model-library`属性，但不能同时使用两者。|
|model-library|包含模型的资源库名称。使用此**或** `model`属性，但不能同时使用两者。|
|model-placement|对于模型替换，描述osgEarth应如何用模型代替几何体：|
||**vertex：** 用模型替换几何体中的每个顶点|
||**interval：** 根据`icon-density`属性，沿几何体以固定间隔放置模型|
||**random：** 根据图`icon-density`属性，在几何体中随机放置模型|
||**centroid：** 在几何体的质心处放置一个模型|
|model-density|对于`interval(间隔)`或`random(随机)`的`model-placement`设置，此属性提示osgEarth应放置多少个实体。该单位约为“每公里的单位”（对于线性数据），或对于多边形数据为“每平方公里单位”。（浮点数）|
|model-scale|沿所有轴按此数量缩放图标（浮点数）|
|model-heading|旋转其+Z轴（浮点数，度）|
|icon-random-seed|对于随机放置操作，设置此种子，以便每次运行应用程序时可以重复随机化（整数）|

## 渲染
*渲染符号（render symbol）*（SDK：`RenderSymbol`）应用常规OpenGL渲染设置以及一些特定于osgEarth的设置，这些设置并非特定于任何其他符号类型。

|**属性** |**描述** |
|---|----|
|render-depth-test|启用或禁用GL深度测试(boolean)|
|render-lighting|启用或禁用GL照明(boolean)|
|render-transparent|透明度（深度排序）bin中渲染的提示（boolean）|
|render-bin|渲染bin用于排序（字符串）|
|render-depth-offset|启用或禁用深度偏移。深度偏移是一种GPU技术，它修改片段的深度值，模拟该对象的渲染距离观察者比实际更近或更远。这是一种减轻深度冲突（z-fighting）的机制（boolean）|
|render-depth-offset-min-bias|设置深度偏移的最小偏差（距观察者偏移）。如果此属性通常设置得充足，其他的所有将自动设置（浮点数，米）|
|render-depth-offset-max-bias|设置深度偏移的最小偏差（距离观察者偏移）|
|render-depth-offset-min-range|设置应用最小深度偏移偏差的范围（距观察者的距离）。偏差在指定范围内的最小值和最大值之间逐渐变化|
|render-depth-offset-max-range|设置应用最大深度偏移偏差的范围（距观察者的距离）。偏差在指定范围内的最小值和最大值之间逐渐变化|

## 文本
*文本符号（text symbol）* （SDK：`TextSymbol`）控制文本标签的存在和显示。

|**属性** |**描述** |
|---|----|
|text-fill|文本的前景色（HTML颜色）|
|text-size|文本大小（浮点数，像素）|
|text-font|要使用的字体的名称（取决于系统）。例如，对于Arial Bold在Windows上使用“arialbd”|
|text-halo|文本的轮廓颜色；没有轮廓时完全忽略这个属性（HTML颜色）|
|text-halo-offset|轮廓粗细（浮点数，字形宽度的百分比，默认值0.0625）|
|text-offset-x|文本的x偏移量，以字形宽度的百分比表示，默认为0.0625|
|text-offset-y|文本的y偏移量，以字形宽度的百分比表示，默认为0.0625|
|text-align|文本字符串相对于其中心点（anchor point）的对齐方式|
||* `left-top`|
||* `left-center`|
||* `left-bottom`|
||* `left-base-line`|
||* `left-bottom-base-line`|
||* `center-top`|
||* `center-center`|
||* `center-bottom`|
||* `center-base-line`|
||* `center-bottom-base-line`|
||* `right-top`|
||* `right-center`|
||* `right-bottom`|
||* `right-base-line`|
||* `right-bottom-base-line`|
||* `base-line`|
|text-layout|文本布局|
||* `ltr`|
||* `rtl`|
||* `vertical`|
|text-content|要显示的实际文本字符串（string-expr）|
|text-encoding|文本内容的字符编码：|
||* `utf-8`|
||* `utf-16`|
||* `utf-32`|
||* `ascii`|
|text-declutter|激活此图标的清理（decluttering）。osgEarth将尝试自动显示或隐藏事物，以便它们不会在屏幕上重叠。（bool）|
|text-occlusion-cull|是否剔除遮挡文本，以便在视线被地形阻挡时不显示|
|text-occlusion-cull-altitude|当视线被地形阻挡时，观察者高度（MSL）开始剔除遮挡|


## 覆盖
*覆盖符号（coverage symbol）* （SDK：`CoverageSymbol`）控制如何使用离散值将要素栅格化为覆盖数据。

|**属性** |**描述** |
|----|----|
|coverage-value|要编码的浮点值表达式|
