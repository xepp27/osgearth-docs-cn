# 着色器组成
osgEarth其几种渲染模式中使用GLSL着色器。默认情况下，osgEarth将监测你的图形硬件性能来自动选择适当的模式来使用。

由于osgEarth依赖于着色器，你作为一个开发者可能会希望自定义或者在GLSL肿添加自己的效果和特征。任何使用着色器的人都遇到了相同的问题：
* 着色器程序是整天的，添加新的着色器代码需要复制、修改或替换已存在的代码以免丢失其功能。
* 使更改的内容与原始代码着色器的更改保持同步是一项维护噩梦。
* 维护着色器main()的多个版本是麻烦且复杂的。
* 随着GLSL代码库的复杂性增加并且添加了更多功能，维护可怕的“超级着色器”变得难以管理。
着色器组合通过*模块化*着色器管线解决了这些问题。您可以在程序中的任何位置添加和删除功能，而无需复制，粘贴或黑客攻击其他人的GLSL代码。

接下来我们将讨论osgEarth的着色器组合框架的结构。

## 框架基础
着色器组成框架自动提供main()函数，你不需要把它们写出来，只需写出模块函数并且告诉框架什么时候、在哪儿执行它们。

接下来你将看到osgEath创建的main()函数，`LOCATION_*`指示符允许你在着色器执行管线上各点处插入函数。

这是osgEarth内置着色器main的伪代码：
```C++
// VERTEX SHADER:

void main(void)
{
    vec4 vertex = gl_Vertex;

    // "LOCATION_VERTEX_MODEL" user functions are called here:
    model_func_1(vertex);
    model_func_2(vertex);
    ...

    vertex = gl_ModelViewMatrix * vertex;

    // "LOCATION_VERTEX_VIEW" user functions are called here:
    view_func_1(vertex);
    ...

    vertex = gl_ProjectionMatrix * vertex;

    // "LOCATION_VERTEX_CLIP" user functions are called last:
    clip_func_1(vertex);
    ...

    gl_Position = vertex;
}


// FRAGMENT SHADER:

void main(void)
{
    vec4 color = gl_Color;
    ...

    // "LOCATION_FRAGMENT_COLORING" user functions are called here:
    coloring_func_1(color);
    ...

    // "LOCATION_FRAGMENT_LIGHTING" user functions are called here:
    lighting_func_1(color);
    ...

    gl_FragColor = color;
}
```
如你所示，我们已决定了对大部分应用程序有意义的函数插入点，这不是说它们在所有情况下都是最好的，而是我们认为这种方法使框架便于使用并且不会太“低级”。

*重要：*着色器组成框架这时仅支持VERTEX（顶点）和FRAGMENT（碎片）着色器，目前并不支持GEOMETRY（几何体）或者TESSELLATION（曲面细分）着色器，我们计划在以后添加他们。

## 虚拟程序VirtualProgram
osgEarth引入了一种名为`VirtalProgram`的新的OSG状态属性来执行运行时的着色器组成。由于`VirtualProgram`是一种`osg::StateAttribute`,你可以将其添加到场景图形中的任一节点。属于`VirtualProgram`的着色器可以覆盖场景图形中较高的着色器。用这种方法你可以添加、合并以及覆盖osgEarth中独立的着色器函数。

在运行时，一个`VirtualProgram`将查看当前状态并组装一个完整的`osg::Program`，它使用内置的main()并调用通过`VirtualProgram`插入的所有函数。

### 添加函数
从生成的main函数中不难发现osgEarth调用了用户函数，它们并不存在于osgEarth生成的默认着色器中，而是代表你作为开发者可以“插入”着色器管线各个位置的代码。

例如，让我们使用用户函数来创建一个简单的“模糊”效果：
```C++
// haze_vertex:
out vec3 v_pos;
void setup_haze(inout vec4 vertexView)
{
    v_pos = vertexView.xyz;
}

// haze_fragment:
in vec3 v_pos;
void apply_haze(inout vec4 color)
{
    float dist = clamp( length(v_pos)/10000000.0, 0, 0.75 );
    color = mix(color, vec4(0.5, 0.5, 0.5, 1.0), dist);
}

// C++:
VirtualProgram* vp = VirtualProgram::getOrCreate( stateSet );

vp->setFunction( "setup_haze", haze_vertex,   ShaderComp::LOCATION_VERTEX_VIEW);
vp->setFunction( "apply_haze", haze_fragment, ShaderComp::LOCATION_FRAGMENT_LIGHTING);
```
在这个例子中，`setup_haze`函数在内置顶点函数后被内置顶点着色器main()函数调用，`apply_haze`函数在内置碎片函数之后被核心碎片着色器main()函数调用。

以下是六个插入点：

|位置|着色器类型|说明|
|----|---------|---|
|ShaderComp::LOCATION_VERTEX_MODEL|VERTEX|void func(inout vec4 vertex)|
|ShaderComp::LOCATION_VERTEX_VIEW|VERTEX|void func(inout vec4 vertex)|
|ShaderComp::LOCATION_VERTEX_CLIP|VERTEX|void func(inout vec4 vertex)|
|ShaderComp::LOCATION_FRAGMENT_COLORING|FRAGMENT|void func(inout vec4 color)|
|ShaderComp::LOCATION_FRAGMENT_LIGHTING|FRAGMENT|void func(inout vec4 color)|
|ShaderComp::LOCATION_FRAGMENT_OUTPUT|FRAGMENT|void func(inout vec4 color|

每个顶点位置都允许你在指定坐标空间中对顶点进行操作。你可以更改顶点，但是必须将其置于同一空间中。

**模型（MODEL）：** 顶点是几何体中得到的原始未转换数值。

**视图（VIEW）：** 顶点与视点相关，位于原点（0,0,0）并指向-Z轴。在视图空间，原始顶点已经过`gl_ModelViewMatrix`转换。

**剪裁（CLIP）：** 投影后的裁剪空间。裁剪空间位于三个坐标轴的[-W,W]范围内，是原始视点经`gl_ModelViewProjectionMatrix`转换的结果。

以下是碎片位置：

**着色（COLORING）：** 这里的函数在应用光照前解析碎片颜色时被调用。纹理或颜色调整通常在这一阶段进行。

**光照（LIGHTING）：** 这里的函数影响着应用于碎片颜色的光照，通常是如太阳光照、凹凸投影或发现投影等进行的地方。

**输出（OUTPUT）：** 这里是设置gl_FragColor的地方。默认情况下，内置碎片main()函数将为你设置，但是你可以设置一个输出着色器来进行替代。通常做这个的原因是实现MRT渲染（参考osgearth_mrt示例）、

## 着色器封装ShaderPackages
前面我们给你展示了如何使用`VirtualProgram`来插入函数，着色器组成框架还提供了`ShaderPackage`的概念支持更多的着色器管理方法。我们现在来介绍它们。

### 虚拟程序元数据（VirtualProgram Metadata）
如刚才所见，当你使用`VirtualProgram`往管线中添加一个着色器函数时你需要告诉osgEarth要调用的GLSL函数的名字，以及管线中所调用的位置，如：
```C++
VirtualProgram* vp;
....
vp->setFunction( "color_it_red", shaderSource, ShaderComp::LOCATION_FRAGMENT_COLORING );
```
这是有效的，但是如果函数名或者插入位置变化了，你需要用`setFunction()`参数使GLSL代码保持同步。

一次性指定这些更加便捷，`ShaderPackage`帮你来做这个。这是一个例子：
```C++
#version 110

#pragma vp_entryPoint  color_it_red
#pragma vp_location    fragment_coloring
#pragma vp_order       1.0

void color_it_red(inout vec4 color)
{
    color.r = 1.0;
}
```
这时你不用调用`VirtualProgram::setFunction()`，只需要创建一个`ShaderPackage`，添加你的代码并且在`VirtualProgram`中载入创建函数：
```C++
ShaderPackage package;
package.add( shaderFileName, shaderSource );
package.load( virtualProgram, shaderFileName );
```
它用到了“文件名”因为着色器可以再外部文件中，但不是必需的。继续阅读更多细节。

`vp_location`的数值基于代码的数值，如下所示：
```C++
vertex_model
vertex_view
vertex_clip
fragment_coloring
fragment_lighting
fragment_output
```

### 外部GLSL文件
`ShaderPackage`允许您从文件或字符串加载GLSL代码。如上所示调用`add`方法时会告诉封装（a）首先按该名称查找文件并从该文件加载；（b）如果文件不存在，请使用源字符串中的代码。

那么让我们来看看这个例子：
```C++
ShaderPackage package;
package.add( "myshader.frag.glsl", backupSourceCode );
...
package.load( virtualProgram, "myshader.frag.glsl" );
```
该封装将尝试从GLSL文件加载着色器，它将在`OSG_FILE_PATH`中搜索它。如果找不到该文件，它将从与封装中的着色器关联的备份源代码加载着色器。

osgEarth在内部使用这种技术来“内联”其着色器代码，这使您可以选择使用您的应用程序部署GLSL文件或保持它们内联——不管怎样应用程序仍将运行。

## 包含文件（Include Files）

`ShaderPackage`支持包含文件的概念。您的GLSL代码可以通过引用其文件名包含同一封装中的任何其他着色器。使用自定义`#pragma`包含另一个文件：
```C++
#pragma include myCode.vertex.glsl
```
就像在C++中一样，include将直接内联加载其他文件（或源代码）。因此，您所包含的文件必须结构化，就好像您已将其放在包含文件中一样。（例如这意味着它不能拥有自己的`#version`字符串。）

同样：包含者和被包含者必须使用相同的`ShaderPackage`注册。

## 特定于osgEarth的概念
尽管虚拟程序框架包含在osgEarth SDK中，但它实际上与地图渲染无关。在本节中，我们将介绍osgEarth使用着色器组合执行的一些操作。

### 地形变量
osgEarth地形引擎使用了一些内置的着色器`uniforms`和`variables`，开发人员可以使用这些变量。

*重要提示：以前缀“oe_”或“osgearth_”开头的着色器变量保留给osgEarth内部使用。*

Uniforms：

**oe_tile_key:** （vec4）元素0-2保存x，y和LOD（细节级别）贴片键值; 元素3保存贴片的边界球半径（以米为单位）

**oe_layer_tex:** （sampler2D）纹理应用于当前贴片的当前图层

**oe_layer_texc:** （vec4）当前贴片的纹理坐标

**oe_layer_tilec:** （vec4）当前贴片的单位坐标（x和y中0..1）

**oe_layer_uid:** （int）活动图层的唯一ID

**oe_layer_order:** （int）渲染活动图层的顺序

**oe_layer_opacity:** （浮点）活动层的不透明度[0..1]


顶点属性：

**oe_terrain_attr:** （vec4）元素0-2保存地形顶点的单位高度向量，元素3保存原始地形高程值

**oe_terrain_attr2:** （vec4）元素0保存父贴片的高程值; 元素1-3目前尚未使用。

### 共享影像图层
有时你想要同时访问多个影像图层，如，你可能有一个表示陆地或水源的掩蔽层（masking layer），您可能实际上不想绘制此图层，但您希望使用它来调整另一个可见图层。

您可以使用共享影像图层执行此操作，在`Map`中，将影像图层标记为共享（使用ImageLayerOptions::shared（）），渲染器将使其可用于辅助采样器中的所有其他图层。

有关用法示例，请参阅`osgearth_sharedlayer.cpp`！
