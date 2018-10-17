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

  **read_write：** 默认值。如果配置了缓存，则使用缓存。

  **no_cache：**   即使有缓存，也不使用。只能直接从数据源读取。
 
  **cache_only：** 如果设置了缓存，则只使用缓存中的数据; 永远不使用数据源。
  
您还可以将缓存使对象设为过期。默认情况下，缓存数据永不过期，但您可以使用`max_age`属性设置对象的有效时间：
```C++
<cache_policy max_age="3600"/>
```
指定最大年龄（以秒为单位）。上面的示例将使超过一个小时的对象失效。
  
## 环境变量
有时在环境中控制缓存更方便，尤其在开发中。
  
这些变量将覆盖缓存的策略属性：

  **OSGEARTH_NO_CACHE:**      在任意osgEarth地图中应用`no_cache`策略。（设为1）
  
  **OSGEARTH_CACHE_ONLY:**    在任意osgEarth地图中应用`cache_only`策略。（设为1）
  
  **OSGEARTH_CACHE_MAX_AGE:** 将缓存设置为使用对象的时间超过此秒数。
  
以下不包含于缓存策略，而是控制特定的缓存实现。
  
  **OSGEARTH_CACHE_PATH:**   缓存的根目录。设置此选项将为任何使用中的缓存驱动器用缓存。
  
  **OSGEARTH_CACHE_DRIVER:** 设置要使用的缓存驱器的名称，例如`filesystem`或`leveldb`。
  
**注意：** 环境变量会覆盖*地球文件*中的缓存设置！见下文。

## 缓存策略设置中的优先级
由于您可以在各个地方设置缓存策略，因此我们需要建立优先级。以下为规则：

- **地图设置** 这是在地球文件的`<map>` `<options>`块中`Map`对象中设置的缓存策略，将为地图中的每个图层设置默认缓存策略。这是最低级的政策设置;它可以被以下任何设置覆盖。
    
- **图层设置** 这是在`ImageLayer`（图像图层）或`ElevationLayer`（高程图层）对象（或地球文件中的`<map> <image>`或`<map> <elevation>`块）中设置的缓存策略。这将覆盖`Map`中的顶级设置，但不会覆盖环境设置的缓存策略（参见下文）。（这也是覆盖驱动器策略提示的唯一方法（见下文），但您很少需要这样做。）
    
- **环境变量** 它们被读取并存储在Registry的overrideCachePolicy（覆盖缓存策略）中，它们将覆盖地图或图层中的设置。但是，它们不会覆盖驱动器策略提示。

- **驱动器策略提示** 有时驱动器会告诉osgEarth永远不缓存它提供的数据，并且osgEarth将服从。覆盖它的唯一方法是在层本身上明确设置缓存策略。（你很少会担心这个。）

## 填充缓存
有时，为特定感兴趣的区域预先设置缓存是很有用的。osgEarth提供了一个名为`osgearth_cache`的功能来完成此任务。`osgearth_cache`将获取一个Earth文件并填充它找到的任何缓存。

在命令行上键入`osgearth_cache --help`以获取使用信息。

**注意：** 缓存是一个瞬态的“黑匣子”，旨在提高某些情况下的性能。它不是一个可分发的数据存储库。在许多情况下，您可以将缓存文件夹从一个环境移动到另一个环境，它可以工作，但osgEarth不保证这样的行为。
