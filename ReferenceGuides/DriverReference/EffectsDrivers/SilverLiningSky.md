# SilverLining Sky
使用SunDog Software的SilverLining SDK的天空模型。

SilverLining SDK需要有效的许可证代码。如果没有用户名和许可证代码，SDK将以“演示模式”运行，并且每五分钟会显示一个对话框。

用法示例：
```XML
<map>
    <options>
        <sky driver = "silverlining"
             hours               = "0.0"
             ambient             = "0.05"
             user                = "myname"
             license_code        = "mycode"
             clouds              = "false"
             clouds_max_altitude = "0.0 />
```

属性：

**user：** SilveLining SDK许可证的用户名

**license_code：** SilverLining SDK许可证编号

**clouds：** 是否渲染一个本地云图层

**clouds_max_altitude：** 开始渲染云图层的最大相机高度

常用属性：

**hours：** 一天中的时间；UTC时钟[0..24]

**ambient：** 用于地形中黑暗区域的最小环境照明等级[0..1]
