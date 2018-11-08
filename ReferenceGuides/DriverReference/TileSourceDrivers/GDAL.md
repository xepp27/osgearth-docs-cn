# GDAL（地理空间数据抽象函数库，Geospatial Data Abstraction Library）
GDAL插件可以读取大部分地理空间文件类型，是你在本地文件系统中读取数据时将使用到的最常见的驱动器。

[GDAL](https://www.gdal.org/)函数库支持大量的[格式](https://www.gdal.org/formats_list.html)，包括最常见的GeoTIFF、JPEG和ECW，如果配置正确，也可以从数据库和网络服务器中读取数据。

用法示例：
```XML
<image driver="gdal">
    <url>data/world.tif</url>
</image>
```
从文件夹中加载不同的文件（必须有相同的投影）：
```XML
<image driver="gdal">
    <url>data</url>
    <extensions>tif</extensions>
</image>
```
属性：

**url：** 加载的文件位置，或是需要加载的处于相同投影的多个文件的文件夹位置。

**connection：** 如果数据源是一个数据库（如PostGIS），连接字符串（connection string）用来打开数据库表。

**extensions：** 一个或多个文件扩展名，用分号分隔，在`url`指向一个文件夹并且你尝试加载多个文件时使用。

**black_extensions：** 所忽视的文件扩展名集合（与`extensions`相反）。

**interpolation：** 在重采样源数据时使用的插值方法，可选项为`nearest`、`average`和`bilinear`。仅作用于高程数据，除非interp_imagery也设为true。

**max_data_level：** 可用数据的最大细节级别。

**subdataset：** 一些GDAL所支持的格式支持子数据集，使用这个属性来指定数据源。

**interp_imagery：** 设置为true以使用“插值”指定的方法对图像进行采样。默认情况下，使用nearest方法对图像进行采样。它利用在源文件中的概视图或小波压缩所构建的任何内容，但是会导致相邻瓦片上的伪像。内插图像可以使其看起来更好但是速度更慢。

**warp_profile：** “warp_profile”是告诉GDAL驱动器保存原始SRS及源数据地理转换，但使用Warped VRT使得数据看起来符合给定配置文件的方法。对于合并可能使用复合驱动器在不同投影中的多个文件非常有用。

也可以参考：

  `tests`文件夹中的`gdal_tiff.earth`示例。
