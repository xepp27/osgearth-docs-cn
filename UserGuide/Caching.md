# 缓存
由于源数据的性质，osgEarth在它成为地形图之前不得不对其进行一些处理，包括下载、二次投影、裁剪，修复以及合成等等。这些操作成本较高。通过设置缓存，您可以指示osgEarth存储处理结果，以便下次需要相同的贴片时不需要重复执行。

注意！osgEarth的缓存使用的是内部数据存储表示，不应通过任何公共API访问。它仅用作瞬态缓存而不是数据发布格式，结构可随时更改。如果要发布数据存储库，请考虑使用osgearth_package！

## 设置缓存
您可以在地球文件中设置缓存。以下设置将自动激活所有图像和高程图层上的缓存：
```C++
<map>
    <options>
        <cache type="filesystem">
            <path>folder_name</path>
        </cache>
```
在代码中，这将是这样的：
```c++
FileSystemCacheOptions cacheOptions;
cacheOptions.path() = ...;

MapOptions mapOptions;
mapOptions.cache() = cacheOptions;
```
或者，您可以使用适用于所有地球文件的环境变量。请记住，这将覆盖地球文件中的缓存设置：
```C++
set OSGEARTH_CACHE_DRIVER=leveldb
set OSGEARTH_CACHE_PATH=folder_name
```
在代码中，您可以在osgEarth注册表中设置全局缓存：
```C++
osgEarth::Registry::instance()->setCache(...);
osgEarth::Registry::instance()->setDefaultCachePolicy(...);
```
## 缓存策略
一旦设置了缓存，osgEarth将默认使用它来处理所有图像和高程图层。如果要覆盖此行为，可以使用缓存策略。缓存策略告诉osgEarth某个对象是否可以以及如何使用缓存。

在地球文件中，您可以使用`cache_policy`块执行此操作。这里我们将它应用于整幅地图：
```C++
<map>
    <options>
        <cache_policy usage="cache_only"/>
```
也可以在单个图层中应用策略：
```C++
<image>
    <cache_policy usage="no_cache"/>
    ...
```
缓存策略使用的值为：
  
  ** READ_WRITE： **默认值。如果配置了缓存，则使用缓存。

  ** no_cache： **  即使有缓存，也不使用。只能直接从数据源读取。

  ** cache_only： **如果设置了缓存，则只使用缓存中的数据; 永远不使用数据源。
  
  您还可以将缓存使对象设为过期。默认情况下，缓存数据永不过期，但您可以使用max_age属性设置对象的有效时间：
  ```C++
  <cache_policy max_age="3600"/>
  ```
  指定最大年龄（以秒为单位）。上面的示例将使超过一个小时的对象失效。
  
  ## 环境变量
  
  
  
  
  
  
  
  
  
  
  
