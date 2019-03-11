## <a name='memory'>内存监控</a>
<a href='index.html' target="_blank">Index</a>
**相关脚本**

```
sm_buffer_obj.sql	sm_memory.sql		sm_sga.sql
sm_db_buffer.sql	sm_pga.sql		sm_share_pool.sql
11g/sm_result_cache.sql
```

### <a name='sm_sga'>SGA</a>

> `sm_sga.sql`

```
17:57:37 study>@sm_sga

sga相关参数

   INST_ID 参数名称                      参数值
---------- ------------------------------ ------------------------------
         1 db_cache_size                  0
         1 lock_sga                       FALSE
         1 log_buffer                     4956K
         1 memory_max_target              820M
         1 memory_target                  820M
         1 result_cache_max_size          2112K
         1 sga_max_size                   820M
         1 sga_target                     0
         1 shared_pool_reserved_size      6501171
         1 shared_pool_size               0
         1 streams_pool_size              0

sga内存使用情况

   INST_ID POOL                      内存大小(M) 空闲内存(M)
---------- -------------------------- ----------- -----------
         1 buffer_cache                       216           0
         1 fixed_sga                         2.91           0
         1 java pool                            4           4
         1 large pool                        7.53           8
         1 log_buffer                        5.09           0
         1 shared pool                      77.97          48
         1 shared_io_pool                      20           0
         1 Total                            333.5          60
please more [E:\oradba_output\study-sm_sga.txt]。
```



### <a name='sm_pga'>PGA</a>

> `sm_pga.sql`

```
17:57:43 study>@sm_pga

pga相关参数

   INST_ID 参数名称                      参数值
---------- ------------------------------ ------------------------------
         1 hash_area_size                 131072
         1 workarea_size_policy           AUTO
         1 pga_aggregate_target           0
         1 sort_area_size                 65536

当workarea_size_policy设置为manual时,PGA的内存分配使用sort_area_size、hash_area_size参数来分配；
设置为auto时，sort_area_size等参数将被忽略.
如果PGA_AGGREGATE_TARGET 被设置了，那么workarea_size_policy默认识AUTO的。


pga整体情况

   INST_ID     TARGET      USED
---------- ---------- ----------
         1        284      87.97

pga详细使用情况

   INST_ID 指标名称                                                              大小 单位
---------- ---------------------------------------------------------------- ---------- -----------
         1 bytes processed                                                       17665 M
         1 PGA memory freed back to OS                                            7794 M
         1 maximum PGA allocated                                                   341 M
         1 aggregate PGA target parameter                                          284 M
         1 extra bytes read/written                                                193 M
         1 aggregate PGA auto target                                               177 M
         1 total PGA allocated                                                     151 M
         1 total PGA inuse                                                          88 M
         1 maximum PGA used for auto workareas                                      60 M
         1 global memory bound                                                      57 M
         1 total freeable PGA memory                                                41 M
         1 maximum PGA used for manual workareas                                    10 M
         1 total PGA used for auto workareas                                         0 M
         1 total PGA used for manual workareas                                       0 M
         1 cache hit percentage                                                  98.91 percent
         1 recompute count (total)                                               33048
         1 max processes count                                                      73
         1 process count                                                            56
         1 over allocation count                                                     0

pga advice (仅显示ESTD_PGA_CACHE_HIT_PERCENTAGE>=80%的记录)

   INST_ID PGA_TARGET PGA_TARGET_FACTOR ESTD_PGA_CACHE_HIT_PERCENTAGE ESTD_OVERALLOC_COUNT ADV
---------- ---------- ----------------- ----------------------------- -------------------- ---
         1         36              .125                            96                  162 ON
         1         71               .25                            97                  129 ON
         1        142                .5                            99                   11 ON
         1        213               .75                           100                    3 ON
         1        284                 1                           100                    0 ON
         1        341               1.2                           100                    0 ON
         1        398               1.4                           100                    0 ON
         1        454               1.6                           100                    0 ON
         1        511               1.8                           100                    0 ON
         1        568                 2                           100                    0 ON
         1        852                 3                           100                    0 ON
         1       1136                 4                           100                    0 ON
         1       1704                 6                           100                    0 ON
         1       2272                 8                           100                    0 ON

SQL(Sort或hash join)分配内存(work area)方式统计(仅统计分配64k以上内存)：
Optimal:所有的操作在内存中进行
Onepass:使用最小写磁盘操作，大部分在内存中进行
Multipass:当workarea太小的话将会发生大量磁盘操作，性能急剧下降.

   INST_ID TOTAL_COUNT OPTIMAL_COUNT OPTIMAL_PERC ONEPASS_COUNT ONEPASS_PERC MULTIPASS_COUNT MULTIPASS_PERC
---------- ----------- ------------- ------------ ------------- ------------ --------------- --------------
         1       12092         12088        99.97             4          .03               0              0
please more [E:\oradba_output\study-sm_pga.txt]。
```



### <a name='sm_db_buffer'>数据库缓冲区(db_buffer)</a>

> `sm_db_buffer.sql`

```
17:58:35 study>@sm_db_buffer

数据缓区(database buffer)相关信息


db buffer 相关参数:

   INST_ID 参数名称                      参数值
---------- ------------------------------ ------------------------------
         1 db_cache_advice                ON
         1 db_cache_size                  0
         1 db_keep_cache_size             0
         1 db_recycle_cache_size          0

db buffer 使用情况:
   INST_ID 数据缓冲区大小(M) 数据缓冲区命中率
---------- ----------------- -----------------------------------------
         1               204 95.45%
仅能看当前instance
内存状态说明：
free - Not currently in use
xcur - Exclusive
scur - Shared current
cr   - Consistent read
read - Being read from disk
mrec - In media recovery mode
irec - In instance recovery mode

   INST_ID 内存状态   内存大小(M)
---------- ---------- -----------
         1 xcur                70
         1 cr                  41

   INST_ID   LRU_FLAG 内存状态  内存大小(M)  BLOCK_NUM
---------- ---------- ---------- ----------- ----------
         1          0 xcur                19       2371
         1          0 cr                   9       1105
         1          4 xcur                 1         82
         1          4 cr                  32       4144
         1          6 cr                   0          7
         1          8 xcur                50       6447
         1          8 cr                   0         10

db buffer advice (仅显示estd_physical_read_factor<=1的记录)

   INST_ID SIZE_FOR_ESTIMATE SIZE_FACTOR ESTD_PHYSICAL_READ_FACTOR ESTD_PHYSICAL_READS
---------- ----------------- ----------- ------------------------- -------------------
         1               204           1                         1              800324
         1               220      1.0784                     .9792              783670
         1               240      1.1765                     .9571              766025
         1               260      1.2745                     .9412              753231
         1               280      1.3725                      .925              740332
         1               300      1.4706                     .9124              730197
         1               320      1.5686                     .9005              720705
         1               340      1.6667                     .8869              709840
         1               360      1.7647                     .8754              700592
         1               380      1.8627                     .8645              691917
         1               400      1.9608                     .8541              683556

Multiple Buffer Pools

   INST_ID 参数名称                        Disk Reads              Memory Reads Hit Ratio
---------- ----------------- ------------------------- ------------------------- ---------------
         1 DEFAULT                              800327                  35993527 97.78%
please more [E:\oradba_output\study-sm_db_buffer.txt]。
```



### <a name='sm_share_pool'>共享池(share_pool)</a>

> `sm_share_pool.sql`

```
18:06:58 study>@sm_share_pool
正在统计．．．

共享池（share pool)相关信息:


share pool 相关参数:

   INST_ID 参数名称                      参数值
---------- ------------------------------ ------------------------------
         1 result_cache_max_size          2112K
         1 shared_pool_reserved_size      6501171
         1 shared_pool_size               0

share pool大小:

   INST_ID 共享池大小(M) 空闲内存(M)
---------- ------------- -----------
         1            72          43

共享池命中率:

dictionary cache hit ratio

   INST_ID                    Gets                    Misses Hit Rate
---------- ------------------------- ------------------------- ---------------------
         1                  15787719                    360404 97.77%

Library cache hit ratio

   INST_ID                     Hits                   Misses Hit Ratio             Reload %
---------- ------------------------- ------------------------- ------------------ ----------------
         1                   2214942                     49678 97.81%                    2.24%

share pool中内存按状态分布情况(仅能看到当前实例):
free    :不包含任何对象的chunk,可以不受限制的被分配
freeable:Freeable chunks包含session周期或调用的对象,随后可以被释放.这部分内存有时候可以全部或部分提前释放.
recr    :Recreatable chunks包含可以被临时移出内存的对象,在需要的时候，这个对象可以被重新创建.例如,许多存储共享sql代码的内存都是可以重建的
perm    :Permanent memory chunks包含永久对象.通常不能独立释放.
(注：＂R-＂代表保留共享池--SHARED_POOL_RESERVED)


library cache 中内存使用情况:

   INST_ID NAMESPACE                                                       占用内存大小(M)
---------- ---------------------------------------------------------------- ---------------
         1 SQL AREA                                                                   11.03
         1 TABLE/PROCEDURE                                                             1.87
         1 BODY                                                                        1.48
         1 SQL AREA STATS                                                              1.23
         1 MULTI-VERSION OBJECT FOR TABLE                                               .04
         1 INDEX                                                                        .03
         1 PUB SUB INTERNAL INFORMATION                                                 .02
         1 MULTI-VERSION OBJECT FOR INDEX                                               .02
         1 CLUSTER                                                                      .02
         1 OPTIMIZER FINDING                                                            .02
         1 AUDIT POLICY                                                                 .01
         1 OPTIMIZER DIRECTIVE OWNER                                                    .01
         1 SQL AREA BUILD                                                                 0
         1 SCHEMA                                                                         0
         1 USER PRIVILEGE                                                                 0
         1 SCHEDULER GLOBAL ATTRIBUTE                                                     0
         1 EDITION                                                                        0
         1 GTT SESSION PRIVATE STATS                                                      0
提示：可以使用Dbms_shared_pool.keep把对象Keep在共享池，可以通过视图v$db_object_cache.kept字段确认．

保留共享池使用情况:
保留共享池从share pool中分配，默认是大小是共享池的5%.
可用shared_pool_reserved_size来调整
Share Pool中内存大小5000字节的大段就会被存放在保留共享池里.
当REQUEST_MISSES>0或REQUEST_FAILURES>0,应考虑扩大shared_pool_reserved_size。
please more [E:\oradba_output\study-sm_share_pool.txt]。
```



### <a name='sm_buffer_obj'>object在buffer中的大小</a>

> `sm_buffer_obj.sql`计算object占用buffer内存的大小

```
18:13:17 study>@sm_buffer_obj
请输入对象的用户:med2
请输入对象的名称:bigtable

object[med2.bigtable] 在buffer中占用情况：

INST_ID OWNER  OBJECT_NAME SUBOBJECT_NAME OBJECT_TYPE   LRU_FLAG 内存状态 内存大小(M)  BLOCK_NUM
------- ------ ----------- -------------- ----------- ---------- ------- ---------- ---------
      1 MED2    BIGTABLE                       TABLE           0 xcur             9      1137
      1 MED2    BIGTABLE                       TABLE           8 xcur            11      1396
please more [E:\oradba_output\study-sm_buffer_obj(med2.bigtable).txt]。
```



### <a name='sm_result_cache'>查询结果缓存(11g)</a>

> `sm_result_cache.sql`显示查询结果缓存参数和内存

```
18:41:23 study>@11g/sm_result_cache

查询缓冲（result cache )相关信息:

result cache 相关参数:

   INST_ID 参数名称                       参数值
---------- ------------------------------ ------------------------------
         1 result_cache_max_result        5
         1 result_cache_max_size          2112K
         1 result_cache_mode              MANUAL
         1 result_cache_remote_expiration 0

result cache 内存情况:

   INST_ID 参数名称                       内存大小(byte)
---------- ------------------------------ --------------
         1 Result Cache: State Objs                 9440

result cache report:

R e s u l t   C a c h e   M e m o r y   R e p o r t
[Parameters]
Block Size          = 1K bytes
Maximum Cache Size  = 2112K bytes (2112 blocks)
Maximum Result Size = 105K bytes (105 blocks)
[Memory]
Total Memory = 9440 bytes [0.003% of the Shared Pool]
... Fixed Memory = 9440 bytes [0.003% of the Shared Pool]
... Dynamic Memory = 0 bytes [0.000% of the Shared Pool]
please more [E:\oradba_output\study-resule_cache.txt]。
```
