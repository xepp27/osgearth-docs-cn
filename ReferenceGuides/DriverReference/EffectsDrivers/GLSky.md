# GL Sky
实现OpenGL Phong着色的天空模型。

用法示例：
```XML
<map>
    <options>
        <sky driver  = "gl"
                         hours   = "0.0"
             ambient = "0.05" />
```

常用属性：

**hours：** 一天中的时间；UTC时钟[0..24]

**ambient：** 用于地形中黑暗区域的最小环境照明等级[0..1]
