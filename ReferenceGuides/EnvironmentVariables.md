# 环境变量（Environment Variables）
这是osgEarth支持的环境变量列表。


缓存：

**OSGEARTH_CACHE_PATH：** 在指定的文件夹（路径）设置缓存

**OSGEARTH_CACHE_ONLY：** 指示osgEarth仅使用缓存而没有数据源（设置为1）

**OSGEARTH_NO_CACHE：** 指示osgEarth永远不要使用缓存（设置为1）

**OSGEARTH_CACHE_DRIVER：** 设置用于缓存的插件的名称（默认为“filesystem”）


线程/性能：

**OSG_NUM_DATABASE_THREADS：** 设置OSG DatabasePager用于加载地形瓦片和特征数据瓦片的线程总数。

**OSG_NUM_HTTP_DATABASE_THREADS：** 设置Pager的线程池（见上文）中应该用于“高延迟”操作的线程数。（通常这意味着不从缓存中读取数据的操作，或预计需要比平均时间更长的操作。


调试：

**OSGEARTH_NOTIFY_LEVEL：** 与`OSG_NOTIFY_LEVEL`类似，设置控制台输出的详细程度。值为`DEBUG`，`INFO`，`NOTICE`和`WARN`。默认为`NOTICE`。（这与OSG的通知级别不同。）

**OSGEARTH_MP_PROFILE：** 将关于地形引擎的瓦片生成器的详细分析和定时数据转储到控制台。设置为1以获得详细的每瓦时间；设置为2以进行平均瓦片加载时间计算

**OSGEARTH_MP_DEBUG：* *在地图上绘制瓦片边界框和瓦片键（tilekey）标签

**OSGEARTH_MERGE_SHADERS：** 在单个着色器程序中合并所有着色器；这是GLES（移动设备）所必需的，因此可用于测试。（设为1）

**OSGEARTH_DUMP_SHADERS：** 组合着色器程序打显示控制台（设置为1）。


渲染：

**OSGEARTH_DEFAULT_FONT：** 用于文本符号系统的默认字体的名称

**OSGEARTH_MIN_STAR_MAGNITUDE：** 在SkyNode中使用的最小星数


联网：

**OSGEARTH_HTTP_DEBUG：** 显示HTTP调试消息（设置为1）

**OSGEARTH_HTTP_TIMEOUT：** 设置HTTP超时（秒）

**OSG_CURL_PROXY：** 为HTTP请求设置代理服务器（字符串）

**OSG_CURL_PROXYPORT：** 设置HTTP代理服务器的代理端口（整数）

**OSGEARTH_CURL_PROXYAUTH：** 设置代理验证信息（用户名：密码）

**OSGEARTH_SIMULATE_HTTP_RESPONSE_CODE：** 模拟HTTP错误（用于调试；设置为HTTP响应代码）


其他：

**OSGEARTH_USE_PBUFFER_TEST：** 指示osgEarth平台功能分析器创建基于PBUFFER的瓦片内容，以收集GL支持信息。（设为1）
