# Noise
noise插件基于一个名为[libnoise](http://libnoise.sourceforge.net/)的Perlin噪声发生器在程序上生成分形地形。我们将解释它在这里如何工作，但你也可以参考libniose文档来获取有关下列属性的含义和应用。

有许多方法来使用`noise`驱动器，在属性表后面有一些示例。

基本属性：

**resolution（分辨率）：** 生成噪声数据一个周期的线性距离（通常以米为单位）。

**scale（尺度）：** 一个周期内作用于噪声值的偏移量。默认为1.0，即你讲得到/[-1...1]范围内的噪声数据。

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
