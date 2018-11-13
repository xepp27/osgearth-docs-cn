# Simple Sky
根据Sam O'Neil GPU Gems的文章，实现大气散射和照明的天空模型。

用法示例：
```XML
<map>
    <options>
        <sky driver               = "simple"
                         hours                = "0.0"
             ambient              = "0.05"
                             atmospheric_lighting = "true"
                             exposure             = "3.0"  />
```

属性：

**atmospheric_lighting：** 是否将大气散射模型应用于Sky节点下的场景，如果将它设置为false，就会得到基本的Phong照明代替。

**exposure：** 应用于散射模型的曝光水平，模拟通过大气观察地形的冲刷效果。

常用属性：

**hours：** 一天中的时间；UTC时钟[0..24]

**ambient：** 用于地形中黑暗区域的最小环境照明等级[0..1]
