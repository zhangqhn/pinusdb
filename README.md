# 松果时序数据库(pinusdb)
松果时序数据库是一个开源的时间序列数据库。以简单、易用、高性能为目标，解决中小规模物联网场景设备数据存储，查询。
松果时序数据库服务仅包含大约3万行C++代码，虽然代码量少但也提供了丰富的功能、较高的性能。

# 性能
在i3-7100， 8G 内存，1TB HDD windows server 2016 环境下，每条数据8个字段，达到每秒20万条数据写入。
内存中数据扫描达到1500万条。
历史数据整理后压缩，每个设备的数据顺序存放，极大提供数据查询性能。

# 压缩
松果时序数据库先将整数、浮点数按照差值压缩，然后将数据块以zlib压缩，极大提高压缩率。
不仅如此，我们还提供将浮点数按倍数放大后存储为整数，从而提高浮点数的压缩率。用户使用时以浮点数使用即可。
real2 -> 倍数100，     取值范围[-999,999,999.99     ~ +999,999,999.99] 
real3 -> 倍数1000，    取值范围[-999,999,999.999    ~ +999,999,999.999]
real4 -> 倍数10000，   取值范围[-999,999,999.9999   ~ +999,999,999.9999]
real6 -> 倍数1000000， 取值范围[-999,999,999.999999 ~ +999,999,999.999999]

# 容量
在松果时序数据库中，每个表每天的数据存储为一个文件，超过写入时间窗口的文件会被压缩。
所以，数据容量仅限于服务器存储的容量，并且在大容量下还能保持极高的数据读取性能。

# 数据安全性
数据写入松果时序数据库中，首先会写commit日志，commit日志每3秒或写满1MB会刷一次磁盘，所以意外宕机，或服务器断电后只会丢失较少的数据。
松果时序数据库写数据文件时使用doublewrite，保证写入数据页时发送断电数据文件和数据页也不会损坏。

# 编译
目前松果时序数据库仅支持Windows平台，未来会支持Linux，代码中已包含vs 2015的项目文件，下载后需要配置boost库的包含目录和库目录即可编译成功。
若您需要已编译好的程序，请在 http://www.pinusdb.cn 网站相应的链接下载。

# 运行
松果时序数据库以windows服务的方式运行，运行前请先配置好服务配置文件config.ini 具体运行配置细节请参考文档： http://www.pinusdb.cn/doc/v1.3/doc_pdb_windows_install.html

# 二次开发
提供c/c++ SDK, .Net SDK, jdbc 驱动，未来还会支持restful及更多的二次开发接口，具体使用细节请参考：http://www.pinusdb.cn

