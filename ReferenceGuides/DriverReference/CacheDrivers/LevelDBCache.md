# LevelDB Cache
这个插件使用Google [leveldb](https://github.com/pelicanmapping/leveldb)嵌入式键/值存储库将地形贴片、特征矢量和其它数据缓存到本地文件系统。

用法示例：
```XML
<map>
        <options>
        <cache driver      = "leveldb"
               path        = "c:/osgearth_cache"
               max_size_mb = "500" />
        </cache>
                    ...
```
`leveldb`缓存将每类数据存储在自己的bin中。所有bin都存储在同一目录中，位于同一数据库中。这样做可以对整个数据库施加大小限制。每条记录都有时间戳；当缓存达到最大大小时，它首先开始删除最旧的记录以腾出空间。

高速缓存访问是异步和多线程的，但您一次只能从一个进程访问高速缓存。

缓存数据文件的实际格式为“黑匣子”，如有更改，恕不另行通知。我们不打算将缓存文件直接用于其他目的。


属性：

**path：** 存储所用缓存bins和数据的根目录位置

**max_size_mb：** 缓存的最大大小（以兆字节为单位）。大小被视为目标；无法保证缓存的大小始终小于此值，但驱动器将尽力遵守。
