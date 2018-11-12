# XYZ
XYZ插件对于读取带有标准X/Y/LOD设置但不会明确显示任何元数据的网络地图瓦片存储器很有用。很多常用网络地图服务（如[MapQuest](https://business.mapquest.com/products/)）都属于这一类。在使用这个驱动器时你需要提供带有`profile`的osgEarth。

用法示例：
```XML
<image name="mapquest_open_aerial" driver="xyz">
    <url>http://oatile[1234].mqcdn.com/tiles/1.0.0/sat/{z}/{x}/{y}.jpg</url>
    <profile>spherical-mercator</profile>
</image>
```
创建URL模板：

 方括号[]表示osgEarth应该“遍历”其中的字符，从而产生循环服务器请求。有些服务要求这样做。

 花括号{}是osgEarth为所需要的瓦片插入正确x，y和z值的模板。

属性：

**url：** 瓦片存储器的位置（URL模板——详见上述）

**profile：** 存储器的空间配置

**invert_y：** 设置为true以反转Y轴进行瓦片索引

**format：** 如果格式不是URL本身的一部分，你可以在这里指定

也可以参阅：

`tests`文件夹中的`mapquest_open_aerial.earth`和`openstreetmap.earth`示例。
