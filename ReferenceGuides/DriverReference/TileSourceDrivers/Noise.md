# Noise
noise插件基于一个名为[libnoise](http://libnoise.sourceforge.net/)的Perlin噪声发生器在程序上生成分形地形。我们将解释它在这里如何工作，但你也可以参考libniose文档来获取有关下列属性的含义和应用。

有许多方法来使用`noise`驱动器，在属性表后面有一些示例。

基本属性：

**resolution（分辨率）：** 生成噪声数据一个周期的线性距离（通常以米为单位）。

**scale（尺度）：** 一个周期内作用于噪声值的偏移量。默认为1.0，即你讲得到\[-1...1]范围内的噪声数据。

**octaves** 通过添加细节级别来改善噪声数据的次数，即噪声发生器在分辨率范围内递归的深度。较高的数值将在你放大时产生出更多细节，默认值为4。

**offset（偏移）：** 对于高程场，将其设为true来生成偏移量而不是绝对高程，它们将从另一个绝对高程图层中添加高程值。


高级属性：

**frequency（频率）：** 上述*分辨率（resolution）*的倒数。（由于osgEarth是一个投影SDK，因此通常更直观地指定分辨率并将其留空。）

**persistence（持久度）：** 当噪声函数遍历每个较高octave时*尺度（scale）*降低的速率。Scale（octave N + 1）=Scale（octave N）* Persistence。

**lacunarity（空隙率）** 当噪声函数遍历每一个较高细节octave时*频率（frequency）*增长的速率。Freq（octave N+1）=Freq（octave N）* Lacunarity。

**seed：** 随机数生成器，噪声驱动器是“连贯的”，即（除其他外）在给定相同随机种子的情况下生成相同的值。通过改变这个来改变模式。

**min_elevation：** 在构建高程场时所生成的最小高程值，使用高程数据来创造一个“楼层（floor）”。

**max_elevation：** 在构建高程场时所生成的最大高程值，使用高程数据来创造一个“楼顶(ceiling）”。

**normal_map：** 将此设为true（对于一个图像图层）来创建一个`NormalMap`地形效果一起使用的凹凸贴图法线纹理。


也可以参考：

  `tests`文件夹中的`noise.earth`、`fractal_detail.earth`和`normalmap.earth`示例。
  
## 示例
创建一个世界范围程序高程图层：
```XML
<elevation driver="noise">
    <resolution>3185500</resolution>   <!-- 1/4 earth's diameter -->
    <scale>5000</scale>                <!-- vary heights by +/- 5000m over the resolution -->
    <octaves>12</octaves>              <!-- detail recursion level -->
</elevation>
```
通过调整递归属性使其更有趣：
```XML
<elevation driver="noise">
    <resolution>3185500</resolution>   <!-- 1/4 earth's diameter -->
    <scale>5000</scale>                <!-- vary heights by +/- 5000m over the resolution -->
    <octaves>12</octaves>              <!-- detail recursion level -->
    <persistence>0.49</persistence>    <!-- don't reduce the scale as quickly = noisier -->
    <lacunarity>3.0</lacunarity>       <!-- increase the frequency faster = lumpier -->
</elevation>
```
通过创建图像层来查看噪声本身。看起来像云：
```XML
<image driver="noise">
    <resolution>3185500</resolution>   <!-- 1/4 earth's diameter -->
    <octaves>12</octaves>              <!-- detail recursion level -->
</image>
```
使用`noise`创建一个偏移图层来向真实高程数据中添加细节：
```XML
<!-- Real elevation data -->
<elevation name="readymap_elevation" driver="tms" enabled="true">
    <url>http://readymap.org/readymap/tiles/1.0.0/9/</url>
</elevation>

<elevation driver="noise" name="detail">
    <offset>true</offset>             <!-- treat this as offset data -->
    <tile_size>31</tile_size>         <!-- size of the tiles to create -->
    <resolution>250</resolution>      <!-- not far from the resolution of our real data -->
    <scale>20</scale>                 <!-- vary heights by 20m over 250m -->
    <octaves>4</octaves>              <!-- add some additional detail -->
</elevation>
```
对于创建偏移高程数据，我们可以使用*法线地图（normal map）* 来伪装它。法线地图是一种不可见纹理，它模拟了使用真实高程数据时得到的法线向量：
```XML
<image name="normalmap" driver="noise">
    <shared>true</shared>             <!-- share this layer so our effect can find it -->
    <visible>false</visible>          <!-- we don't want to see the actual texture -->
    <normal_map>true</normal_map>     <!-- create a normal map please -->
    <tile_size>128</tile_size>        <!-- 128x128 texture -->
    <resolution>250</resolution>      <!-- resolution of the noise function -->
    <scale>20</scale>                 <!-- maximum height offset -->
    <octaves>4</octaves>              <!-- level of detail -->
</image>

...
<external>
   <normal_map layer="normalmap"/>    <!-- Install the terrain effect so we can see it -->
   <sky hours="17"/>                  <!-- Must have lighting as well -->
</external>
```
