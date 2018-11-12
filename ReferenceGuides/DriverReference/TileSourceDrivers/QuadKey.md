# QuadKey
QuadKey插件是读取网络地图瓦片存储的有效方法，遵从于[Bing](https://msdn.microsoft.com/en-us/library/bb259689.aspx)地图瓦片系统。它假定数据库处于球面墨卡托投影，与Bing一样在根处有2X2瓦片。

用法示例：
```XML
<image name="imagery" driver="quadkey">
    <url>http://[1234].server.com/tiles/{key}.png</url>
</image>
```
创建URL模板（URL template）：

   方括号[]表示osgEarth应该“遍历”其中的字符，从而产生循环服务器请求。有些服务要求这样做。
   
   你需要在URL中提供{key}模板，URL是osgEarth为所需要的瓦片插入quadkey的地方。
   
属性：
   
   **url：** 存储器的位置（URL模板——参阅前文）
   
   **profile：** 存储器的空间配置
   
   **format：** 如果format不是URL的一部分，你可以在这里指定。
