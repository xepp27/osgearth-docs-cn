# 特征和符号
## 了解特征
**特征**是向量几何。与图像和高程数据（光栅）不同，功能没有离散显示的分辨率。osgEarth可以在任何细节级别渲染特征。

**特征**是三个组件的组合：

- 向量几何（点、线或多边形的集合）
- 属性（对应的名称/值的集合）
- 空间参考（描述几何坐标）

## 创建特征图层
osgEarth有两种方法渲染特征：
- 作为*图像图层*栅格化
- 作为*模型图层*进行细化

### 栅格化
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
### 细化
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

## 风格


## 地形跟踪


## 渲染大数据集
