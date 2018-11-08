# Cesium Ion
Cesium Ion插件从[Cesium Ion](https://cesium.com/)服务器中读取图像瓦片，通过提供您自己的access_token，获得对图层的访问权限。

Cesium Ion需要被SSL解译的CURL函数库来支持https链接。

用法示例：
```XML
<image name="cesiumion bluemarble" driver="cesiumion">
    <asset_id>3845</asset_id>
    <token>eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NDViM2NkNi0xYTE2LTRlZTUtODBlNy05M2Q4ODg4M2NmMTQiLCJpZCI6MjU5LCJpYXQiOjE1MTgxOTc4MDh9.sld5jPORDf_lWavMEsugh6vHPnjR6j3qd1aBkQTswNM</token>
</image>
```
属性：

  **server：** 要访问的Cesium Ion服务器。默认为https://api.cesium.com/
  
  **asset_id：** 要访问的资产的ID，目前仅支持图像图层。
  
  **token：** 您对Cesium Ion服务的访问令牌。
  
  **format：** 图层格式，默认为png。
  
也可以参考：
  
  `tests`文件夹中的`cesium_ion.earth`。
