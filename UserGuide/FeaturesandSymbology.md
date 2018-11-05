# 特征和符号（Features & Symbology）
## 了解特征（Understanding Features）
**特征**是向量几何。与图像和高程数据（光栅）不同，功能没有离散显示的分辨率。osgEarth可以在任何细节级别渲染特征。

**特征**是三个组件的组合：

- 向量几何（点、线或多边形的集合）
- 属性（对应的名称/值的集合）
- 空间参考（描述几何坐标）

## 创建特征图层（Creating a Feature Layer）
osgEarth有两种方法渲染特征：
- 作为*图像图层*栅格化
- 作为*模型图层*进行细化

### 栅格化（Rasterization）
栅格化特征是最简单的方法——osgEarth将在图像贴片上“画出”向量然后在普通图像图层中使用这个图像贴片。

osgEarth有一个特征栅格化驱动器：`agglite`驱动器。以下是将一个ESRI形状文件渲染成图像图层的例子：
```XML
<image name="my layer" driver="agglite">
    <features name="states" driver="ogr">
        <url>states.shp</url>
    </features>
    <styles>
        <style type="text/css">
            states {
                stroke:       #ffff00;
                stroke-width: 2.0;
            }
        </style>
    </styles>
</image>
```
### 细化（Tessellation）
特征细化通过一系列处理将输入的向量转换为OSG几何（点、线、三角形或替代三维模型）。主要的特征细分插件为`feature_geom`驱动器——你会发现在大多数展示特征数据用法的osgEarth地球文件中都用到了它。

以下是用一些黄线将一个ESRI形状文件渲染的一个模型图层，将其渲染为OSG线几何。
```XML
<feature_model name="boundaries">
    <features name="states" driver="ogr">
        <url>states.shp</url>
    </features>
    <styles>
        <style type="text/css">
            states {
                stroke:       #ffff00;
                stroke-width: 2.0;
            }
        </style>
    </styles>
</feature_model>
```
你也可以将特征数据作为单独的图层引用。在有多个使用相同数据集的特征图层时非常有用。
```XML
<feature_source name="data_layer" driver="ogr">
    <url>states.shp</url>
</feature_source>

<feature_model name="boundaries" feature_source="data_layer">
    <styles>
        <style type="text/css">
            states {
                stroke:       #ffff00;
                stroke-width: 2.0;
            }
        </style>
    </styles>
</feature_model>
```
### 特征图层的组成（Components of a Feature Layer）
从上述例子中可以看出，对于任意特征图层有一些必要的组成部分。
- `<features>`模块描述实际特征源，比如osgEarth在哪里获取输入数据。另外，`<feature_source>`引用另一个指定特征数据的图层。
- `<styles>`模块描述osgEarth如何渲染特征，比如他们在屏幕上的形态，我们称其为*样式表*或*符号*。样式表的构成可以从根本上改变特征数据的外观。
这两个要素都是必选的。

## 样式（Styling）
对于地球文件，你可能会看到类似如下的 `<styles>`模块：
```XML
<styles>
    <style type="text/css">
        buildings {
            altitude-clamping: terrain;
            extrusion-height:  15;
            extrusion-flatten: true;
            fill:              #ff7f2f;
        }
    </style>
</styles>
```
这是一个样式表模块。你可以在正在渲染特征数据的`<model>`图层发现它，对应于`<features>`模块。（`<features>`模块确定了实际内容的来源。）

在这种情况下，`<style>`要素包含CSS格式数据。一个CSS样式模块可以包含多个样式，每个样式都有一个名称。在这种情况下，我们只有一种样式：`建筑物`，这个样式告诉几何引擎执行以下操作：
- 将特征几何体存在地形高程数据中；
- 将形状拉伸到地形以上15米的高度；
- 展平拉伸形状的顶部；
- 将形状着色为橙色。
osgEarth采用“模型/视图”方法来渲染特征。它将内容和样式的概念分开，就像网络应用程序使用CSS来设置网络内容样式一样。

osgEarth提取每个输入特征并对其进行样式加工。输出将完全取决于样式表中符号的结合。这包括：
- 填充和描边（Fill and Stroke）——将数据画成线条或多边形
- 拉伸（Extrusion）——将二维几何体拉伸成三维形状
- 替换（Substitution）——用外部三维模型（如树木）或图标替代几何体
- 高度（Altitude）——几何体如何与地图地形进行交互
- 文本（Text）——控制标签
- 渲染（Rendering）——照明、混色和深度测试的应用

### 样式表（Stylesheets）
每一个特征图层都需要一个*样式表*。样式表在地图文件中的以`<styles>`模块出现。以下是一个示例：
```XML
<feature_model name="test">
    <features driver="ogr">
        <geometry>POLYGON( (0 0, 1 0, 1 1, 0 1) )</geometry>
        <profile>global-geodetic</profile>
    </features>
    <styles>
        <style type="text/css">
            default {
                fill:               #ff7f009f;
                stroke:             #ffffff;
                stroke-width:       2.0;
                altitude-clamping:  terrain;
                altitude-technique: drape;
                render-lighting:    false;
            }
        </style>
    </styles>
</feature_model>
```
样式表包含一个称为`default`（默认）的样式。由于只有一种样式，osgEarth将其用于所有输入特征。（若要将不同样式应用于不同特征，要使用*选择器*——以下列出更多信息）

样式包含一系列符号来描述osgEarth应如何渲染特征几何体。在这种情况下：

   **fill：** 用指定的HTML样式颜色填充多边形（此处用到橘色）。
  
   **stroke：** 用白色对多边形描边。
  
   **stroke-width：** 用2像素宽描边。
  
   **altitude-clamping：** 将多边形置于地形。
  
   **altitude-technique：** 使用“覆盖（draping）”技术放置多边形（投影纹理）。
  
   **render-lighting：** 禁用多边形中的OpenGL光照。
  
这只是可用符号的一小部分示例。可以在[符号系统参考](osgearth-docs-cn/ReferenceGuides/SymbologyReference.md)获取完整表格。

### 表达式（Expressions）
有些符号特性支持*表达式*。一个表达式是一个简单的内联计算，它使用特征属性值来动态地计算特性 。

在一个表达式中，可以通过将其名称括在方括号中来访问特征属性值，如下所示：`\[name]`

示例：
```XML
mystyle {
    extrusion-height:  [hgt]*0.3048;           - 读取“hgt”属性，将其单位从英尺转换为米
    altitude-offset:   max([base_offset], 1);  - 使用“base_offset”中较大的数值和1.0
    text-content:      "Name: [name]";         - 将文本标签设置为文字和属性值的串联
}
```
数值表达式计算器支持基本算术（+，-，\*，/％）一些使用函数（min，max）和括号分组。它也适用于字符串值，没有运算符仍然可以嵌入属性。

如果简单表达式不够，可以使用Javascript：
```Javascript
<styles>
    <script language="javascript">
        function getOffset() {
            return feature.properties.base_offset + 1.0;
        }
    </script>

    <style type="text/css">
        mystyle {
            extrusion-height: feature.properties.hgt * 0.3048;
            altitude-offset:  getOffset();
        }
    </style>
</styles>
```

## 地形跟踪（Terrain Following）
特征以某种方式与地形交互是相当常见的。对此的要求包括：
- 沿着地形轮廓的街道
- 种植在地面上的树木
- 专题制图，例如根据人口对国家/地区进行着色

osgEarth提供各种地形跟踪方法，因为没有一种方法最适合所有情况。

### Map Clamping
Map Clamping是最简单的方法。在编译要显示的特征时，osgEarth将对地图中的高程图层进行采样，找到图像的高度，并将其应用于生成的特征几何图形。它将对几何体上的每个点进行测试。

Map Clamping可实现高质量渲染。
- 根据选择的分辨率，可以对地图中的高程数据进行慢速采样。对于大量功能，它可能是CPU密集型且耗时的。
- 采样是准确的，并且对几何中的每个点都进行了采样。可以选择在每个功能的质心处进行采样，以提高编译速度。
- 根据特征几何的分辨率，可能需要对数据进行细分以获得更好的质量。
- 与其他方法相比，渲染质量良好。

您可以在样式表中激活map clamping，如下所示：
```XML
altitude-clamping:   terrain;        // 开启地形跟踪
altitude-technique:  map;            // 将特征置于地图数据
altitude-resolution: 0.005;          // [可选]所放置的地图数据的分辨率
```

### 覆盖（Draping）
*覆盖*是在地形表面上覆盖编译几何体的过程，就像在不平坦表面上“覆盖”毯子一样。 osgEarth将特征渲染到纹理（RTT），然后将该纹理投射到地形上。

覆盖有其优点和缺点：
- 覆盖将完美地适应地形；不用担心分辨率或细分。
- 渲染线条或多边形边缘时，可能会出现锯齿状。投影纹理的大小有限，并且必须覆盖的区域越大，投影图像的分辨率越低。这意味着在实践中，覆盖对多边形比对线条更有用。
- 意料之外的混色可能是由于在彼此顶部覆盖许多透明几何图形而导致的。

您可以像这样激活覆盖：
```XML
altitude-clamping:   terrain;        // 开启地形追踪
altitude-technique:  drape;          // 用投影纹理覆盖特征
```

### GPU Clamping
*GPU Clamping*使用GPU着色器来实现近似地形跟踪。分为两个阶段：首先利用其深度场采样将每个顶点置于顶点着色器中的地形表面；然后在碎片着色器中应用深度偏移算法以缓和深度冲突（z-fighting）。

GPU clamping也有其优缺点：
- 它非常适合于线条（甚至是三角形线条），但对于多边形则不太适合，因为它需要对多边形的内部进行细分以便进行良好的近似放置。
- 速度快，完全在运行时进行，并利用GPU的并行处理。
- 在覆盖中没有锯齿状边缘影响。

这样设置GPU clamping：
```XML
altitude-clamping:   terrain;        // 开启地形追踪
altitude-technique:  gpu;            // 在GPU上放置、补偿特征数据
```

## 渲染大数据集（Rendering Large Datasets）
以下是在osgEarth中加载特征数据最简单的方法：
```XML
<feature_model name="shapes">
   <features name="data" driver="ogr">
      <url>data.shp</url>
   </features>
   <styles>
      data {
          fill: #ffff00;
      }
   </styles>
</feature_model>
```
我们刚刚加载了形状文件中的每个功能，并将它们全部涂成黄色。

这在某种程度上可以正常工作——osgEarth（和OSG）因过多的几何体而过载。 即使使用osgEarth的几何编译器所采用的优化，足够大的数据集也会耗尽系统资源。

解决方案是特征的平铺和分页。以下是配置方法。

### 特征显示布局（Feature display layouts）
特征显示布局激活特征数据的分页和切片。修改一下前面的例子：
```XML
<feature_model name="shapes">
   <features name="data" driver="ogr">
      <url>data.shp</url>
   </features>

   <layout>
       <tile_size>250000</tile_size>
       <level name="data" max_range="100000"/>
   </layout>

   <styles>
      data {
          fill: #ffff00;
      }
   </styles>
</feature_model>
```
仅仅存在`<layout>`要素就会激活分页。 这意味着，一旦应用程序启动，特征数据将在后台加载和编译，而不是在加载时加载和编译。在特征数据出现在场景中之前可能存在延迟，具体取决于其复杂性。

布局中`<level>`要素的存在激活了平铺和细节层次。如果您的遗漏级别，数据仍将在后台加载，但它将立即加载。通过一个或多个级别，osgEarth会将特征数据分解为一个或多个细节级别的切片，并单独分页。以下是更多信息。

分页将数据分割为贴片。`tile_size`是每个分页贴片的宽度（以米为单位）。

### 裁剪特征（Cropping features）
默认情况下，如果特征与贴片相交，即使它延伸到贴片的范围之外，也会包含该贴片。这对于像拉伸建筑物这样的很有用，因为你不想看到半个建筑物页面，所以试图将它们切割成恰好适合于贴片是没有意义的。建筑物通常很小，所以它们延伸到贴片之外的距离相对也很小。

对于具有线性特征的道路或国家边界，裁剪它们来准确地贴片更有意义。视觉上来说，如果只看到一部分线条不会觉得不适应。您可以通过在布局上将`crop_features`特性设置为`true`来在布局上启用特征裁剪。

示例：
```XML
<feature_model name="roads">
      <features name="roads" driver="ogr" build_spatial_index="true">
            <url>roads.shp</url>
      </features>

      <layout crop_features="true" tile_size="1000">
          <level max_range="5000"/>
      </layout>

      <styles>
          <style type="text/css">
              roads {
                  stroke:  #ffff7f7f;
                }
          </style>
      </styles>
</feature_model>
```
### 级别（Levels）
每个级别都描述了细节程度，是在这个级别上每个被渲染的贴片上的镜头范围（介于`min_range`和`max_range`之间）。但是每个贴片有多大呢？这是基于*贴片范围要素*（tile range factor）计算的。

`tiel_size`设置了贴片的尺寸（以米为单位）。

为什么你要关心贴片尺寸呢？因为数据的密集程度将影响每个贴片中有多少个几何体。同时由于OSG（OpenGL）可以从向显卡发送大批量相似几何体中获益，因此调整贴片大小有助于提高性能和吞吐量。不幸的是，osgEarth无法准确得知“最佳”尺寸，因此你有机会使用此设置来调整。

### 布局设置（Layout Settings）
   **tile_size：** 布局中每个特征贴片在最大范围内的大小（在一个维度上）。必须设置最大范围才能生效。
  
   **max_range：** 预先平铺的特征源（如TFS）的所需最大范围。
   
   **min_range：** 布局中所有贴片的最小可见范围。
   
   **crop_features：** 在将特征级别切割为网格单元格时是否裁剪几何体以适合单元格范围。默认情况下是false，即将包含其质心落在单元格内的特征。将此设置为true意味着如果要素的任何部分包含于工作单元格，则会将其裁剪为单元格范围并使用。
   
   **priority_offset：** 设置将应用于此布局中计算的贴片分页优先级的偏移量。调整此值可能会影响此数据相对于场景中其他分页数据（如地形或其他特征图层）的优先级。默认值=0.0。
   
   **priority_scale：** 设置要应用于此布局中计算的贴片分页优先级的比例因子。调整此值可能会影响此数据相对于场景中其他分页数据（如地形或其他要素图层）的优先级。默认值= 1.0。
   
   **min_expiry_time：** 在特征贴片有资格进行分页之前的最短时间（秒）。将其设置为负数以完全禁用（即，铁盘将永远不会分页）。
