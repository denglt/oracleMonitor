## <a name='event'>跟踪事件</a>
<a href='index.html' target="_blank">Index</a>
**相关脚本**

```
trace_10046.sql		trace_10053.sql		trace_10500.sql		trace_filename.sql	trace_getfile.sql	trace_sess_event.sql 	cancel_sql.sql 	tkprof.sql
```

>* `trace_filename.sql`:获取trace event文件名
>* `trace_getfile.sql`:从服务器拉取trace文件到本地
>* `trace_sess_event.sql`:session 跟踪基础脚本
>
>注：基于以上的脚本可以开发自己的trace event



### <a name='10046'>10046</a>

#### <a name='trace_10046'>trace 10046 event</a>

> `trace_10046.sql`:trace 10046 event

```
17:53:44 study>@trace_10046
Enter value for sid of session: 76
Enter value for serial# of session: 62178
跟踪级别：
Level 1:启用标准的SQL_TRACE功能
Level 4:等价于Level 1+绑定值
Level 8:等价于Level 1+等待事件跟踪
Level 12:等价于Level 1+Level 4+Level 8
Enter value for Event Level(12):
对[76.62178]session进行10046跟踪

跟踪文件存放目录：
-------------------------------------------------------------------------------------------------
D:\APP\ORACLE\diag\rdbms\oradlt\oradlt\trace
D:\app\oracle\diag\rdbms\oradlt\oradlt\cdump

跟踪文件名称：
------------------------------
oradlt_ora_1968.trc

输入1停止跟踪
Enter value for stop trace(0): 1
对[76.62178]session停止10046跟踪!


*****************************************************
使用下面命令获取跟踪文件:
@trace_getfile.sql      oradlt_ora_1968.trc
*****************************************************
18:11:30 stud>
```

#### <a name='trace_getfile'>获取跟踪文件</a>
> `trace_getfile.sql`:直接从数据库服务器获取跟踪文件到本地配置的输出文件夹。

```
18:12:55 study>@trace_getfile.sql      oradlt_ora_1968.trc
File oradlt_ora_1968.trc size        .73M

Creating file oradlt_ora_1968.trc

File [E:\oradba_output\study-oradlt_ora_1968.trc] has been created.

please more [E:\oradba_output\study-oradlt_ora_1968.trc]。

如要分析文件，请使用：@tkprof E:\oradba_output\study-oradlt_ora_1968.trc
```

[study-oradlt_ora_1968.trc](scriptlog/study-oradlt_ora_1968.trc)

#### <a name='tkprof' >tkprof 分析跟踪文件</a>

> `tkprof.sql`

```
18:13:32 study>@tkprof E:\oradba_output\study-oradlt_ora_1968.trc

正在使用tkprof对[E:\oradba_output\study-oradlt_ora_1968.trc]进行转化。。。


TKPROF: Release 12.1.0.2.0 - Development on Mon Jul 3 13:53:31 2017

Copyright (c) 1982, 2014, Oracle and/or its affiliates.  All rights reserved.


转换完成。文件为[E:\oradba_output\study-oradlt_ora_1968.txt]

please more [E:\oradba_output\study-oradlt_ora_1968.txt]。
```

[study-oradlt_ora_1968.txt](scriptlog/study-oradlt_ora_1968.txt)

### <a name=trace_10053>10053</a>

> `trace_10053.sql` :跟踪sql plan 成本的计算

```
13:57:58 study>@trace_10053
Enter value for sid of session: 76
Enter value for serial# of session: 13378
10053提供了两个跟踪级别，但是级别2的跟踪信息比级别1少
Enter value for Event Level(1):
对[76.13378]session进行10053跟踪

跟踪文件存放目录：
--------------------------------------------------------------------------------------------------
D:\APP\ORACLE\diag\rdbms\oradlt\oradlt\trace
D:\app\oracle\diag\rdbms\oradlt\oradlt\cdump

跟踪文件名称：
------------------------------
oradlt_ora_448.trc

输入1停止跟踪
Enter value for stop trace(0): 1
对[76.13378]session停止10053跟踪!


*****************************************************
使用下面命令获取跟踪文件:
@trace_getfile.sql      oradlt_ora_448.trc
*****************************************************
15:23:54 study>@trace_getfile.sql      oradlt_ora_448.trc
File oradlt_ora_448.trc size        .12M

Creating file oradlt_ora_448.trc

File [E:\oradba_output\study-oradlt_ora_448.trc] has been created.

please more [E:\oradba_output\study-oradlt_ora_448.trc]。

如要分析文件，请使用：@tkprof E:\oradba_output\study-oradlt_ora_448.trc
```

[study-oradlt_ora_448.trc](scriptlog/study-oradlt_ora_448.trc)

### <a name='trace_10500'>10500</a>

> `trace_10500.sql`:跟踪SMON进程

```
15:25:40 study>@trace_10500

目标：turn on traces for SMON
跟踪级别：
Level:  <=5  trace instance recovery;  > 5  trace posting of SMON

Enter value for Event Level(10):
SMON进程id
------------------------
1244

跟踪文件存放目录：
--------------------------------------------------------------------------------------------
D:\APP\ORACLE\diag\rdbms\oradlt\oradlt\trace
D:\app\oracle\diag\rdbms\oradlt\oradlt\cdump

跟踪文件名称：
------------------------------
oradlt_smon_1244.trc
执行下面语句停止跟踪：
alter system set events '10500 trace name context off';
15:47:44 study>@trace_10500

目标：turn on traces for SMON
跟踪级别：
Level:  <=5  trace instance recovery;  > 5  trace posting of SMON

Enter value for Event Level(10):
SMON进程id
------------------------
1244

跟踪文件存放目录：
--------------------------------------------------------------------------------------------
D:\APP\ORACLE\diag\rdbms\oradlt\oradlt\trace
D:\app\oracle\diag\rdbms\oradlt\oradlt\cdump

跟踪文件名称：
------------------------------
oradlt_smon_1244.trc


*****************************************************
使用下面命令获取跟踪文件:
@trace_getfile.sql      oradlt_smon_1244.trc
*****************************************************


执行下面语句停止跟踪：
alter system set events '10500 trace name context off';
15:54:25 study>@trace_getfile.sql      oradlt_smon_1244.trc
File oradlt_smon_1244.trc size          0M

Creating file oradlt_smon_1244.trc

File [E:\oradba_output\study-oradlt_smon_1244.trc] has been created.

please more [E:\oradba_output\study-oradlt_smon_1244.trc]。

如要分析文件，请使用：@tkprof E:\oradba_output\study-oradlt_smon_1244.trc
15:54:43 study>alter system set events '10500 trace name context off';

System altered.

15:55:05 study>
```

[study-oradlt_smon_1244.trc](scriptlog/study-oradlt_smon_1244.trc)

### <a name='cancle_sql'>10237 取消正在运行的SQL</a>

> `cancel_sql.sql`:取消指定session当前正在运行的sql。
>
> Usage： `@cancel_sql.sql  <sid>  <serial#>`

```
16:06:17 study>@cancel_sql  76 13378

当前session info：

SADDR                         : 000007FF31AC76E8
SID                           : 76
SERIAL#                       : 13378
AUDSID                        : 810310
PADDR                         : 000007FF31E4C780
USER#                         : 113
USERNAME                      : MED2
COMMAND                       : 3
OWNERID                       : 2147483644
TADDR                         :
LOCKWAIT                      :
STATUS                        : ACTIVE
SERVER                        : DEDICATED
SCHEMA#                       : 113
SCHEMANAME                    : MED2
OSUSER                        : denglt-PC\denglt
PROCESS                       : 3952:3620
MACHINE                       : WORKGROUP\DENGLT-PC
PORT                          : 50142
TERMINAL                      : DENGLT-PC
PROGRAM                       : sqlplus.exe
TYPE                          : USER
SQL_ADDRESS                   : 000007FF22B94138
SQL_HASH_VALUE                : 1619591992
SQL_ID                        : 07a9y41h8k0ts
SQL_CHILD_NUMBER              : 0
SQL_EXEC_START                : 2017-07-03 16:06:32
SQL_EXEC_ID                   : 16777217
PREV_SQL_ADDR                 : 000007FF22B94138
PREV_HASH_VALUE               : 1619591992
PREV_SQL_ID                   : 07a9y41h8k0ts
PREV_CHILD_NUMBER             : 0
PREV_EXEC_START               : 2017-07-03 15:22:37
PREV_EXEC_ID                  : 16777216
PLSQL_ENTRY_OBJECT_ID         :
PLSQL_ENTRY_SUBPROGRAM_ID     :
PLSQL_OBJECT_ID               :
PLSQL_SUBPROGRAM_ID           :
MODULE                        : SQL*Plus
MODULE_HASH                   : 3669949024
ACTION                        :
ACTION_HASH                   : 0
CLIENT_INFO                   :
FIXED_TABLE_SEQUENCE          : 7118517
ROW_WAIT_OBJ#                 : 92338
ROW_WAIT_FILE#                : 9
ROW_WAIT_BLOCK#               : 12224
ROW_WAIT_ROW#                 : 0
TOP_LEVEL_CALL#               : 94
LOGON_TIME                    : 2017-07-03 15:19:23
LAST_CALL_ET                  : 9
PDML_ENABLED                  : NO
FAILOVER_TYPE                 : NONE
FAILOVER_METHOD               : NONE
FAILED_OVER                   : NO
RESOURCE_CONSUMER_GROUP       : OTHER_GROUPS
PDML_STATUS                   : DISABLED
PDDL_STATUS                   : ENABLED
PQ_STATUS                     : ENABLED
CURRENT_QUEUE_DURATION        : 0
CLIENT_IDENTIFIER             :
BLOCKING_SESSION_STATUS       : NOT IN WAIT
BLOCKING_INSTANCE             :
BLOCKING_SESSION              :
FINAL_BLOCKING_SESSION_STATUS : NOT IN WAIT
FINAL_BLOCKING_INSTANCE       :
FINAL_BLOCKING_SESSION        :
SEQ#                          : 197
EVENT#                        : 384
EVENT                         : SQL*Net message to client
P1TEXT                        : driver id
P1                            : 1413697536
P1RAW                         : 0000000054435000
P2TEXT                        : #bytes
P2                            : 1
P2RAW                         : 0000000000000001
P3TEXT                        :
P3                            : 0
P3RAW                         : 00
WAIT_CLASS_ID                 : 2000153315
WAIT_CLASS#                   : 7
WAIT_CLASS                    : Network
WAIT_TIME                     : -1
SECONDS_IN_WAIT               : 7
STATE                         : WAITED SHORT TIME
WAIT_TIME_MICRO               : 1
TIME_REMAINING_MICRO          :
TIME_SINCE_LAST_WAIT_MICRO    : 7248885
SERVICE_NAME                  : pdboradlt
SQL_TRACE                     : DISABLED
SQL_TRACE_WAITS               : FALSE
SQL_TRACE_BINDS               : FALSE
SQL_TRACE_PLAN_STATS          : FIRST EXEC
SESSION_EDITION_ID            : 133
CREATOR_ADDR                  : 000007FF31E4C780
CREATOR_SERIAL#               : 179
ECID                          :
SQL_TRANSLATION_PROFILE_ID    : 0
PGA_TUNABLE_MEM               : 1387520
CON_ID                        : 3
EXTERNAL_NAME                 : denglt-PC\denglt
-----------------

当前执行sql：

SQL_FULLTEXT
---------------------------------------------------------------------------------------
select drug_code, count(1) from BIGTABLE group by drug_code

:是否取消(默认Y，其他为否):
取消成功!
16:06:50 study>

```

**sql执行session：**

```
SQL> select drug_code, count(1) from BIGTABLE group by drug_code;
select drug_code, count(1) from BIGTABLE group by drug_code
                                *
ERROR at line 1:
ORA-01013: user requested cancel of current operation
```
