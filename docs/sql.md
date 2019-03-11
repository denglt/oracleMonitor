## <a name='sql'>SQL监控和调优</a>
<a href='index.html' target="_blank">Index</a>
**相关脚本**

```
explain.sql	sql_info.sql	create_sql_profile.sql		create_sql_profile_sqlid.sql	create_sql_profile_sqltext.sql	sql_profile_info.sql	sql_tuning_sqltext.sql	sqltune_sqlid.sql
sqlAccess_sqlSet.sql	sqlAccess_sqlid.sql	sqlAccess_sqltext.sql
11g/rpt_sml.sql	11g/rpt_sml2.sql	11g/rpt_sm.sql	11g/rpt_sd.sql
```

**工具脚本** （一般不直接使用）

```
xplain.sql	readfile.sql	output_sqlplan_info.sql	xplan_cursor_noprompt.sql	xplan_cursor.sql
xplan_lastsql.sql	tuningsql.sql	tuningsql2.sql	rpt_sql_tuning.sql	access_sql.sql		access_sqlset.sql	rpt_sql_access.sql
```



### <a name='explain'>分析SQL的执行计划</a>

> `explain.sql`按照BASIC|TYPICAL|ALL|OUTLINE分析SQL的执行计划。
>
> Usage:把需要分析的SQL语句(如：使用了hint的sql)存放在`dlt_sqltext.txt`文件中，运行`@explain`根据提示输入

```
18:01:34 study>@explain
SQL执行计划类型[BASIC|TYPICAL|ALL|OUTLINE]
请输入要查看的类型(TYPICAL):

执行计划如下：

Plan hash value: 3474563263

------------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name                 | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                      |  1344 |   181K|  1347   (0)| 00:00:01 |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE             |  1344 |   181K|  1347   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX_BIGTABLE_DRUG_ID |  1344 |       |     5   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_ID"=17529)
please more [E:\oradba_output\study-xplain.txt]。
18:02:01 study>@explain
SQL执行计划类型[BASIC|TYPICAL|ALL|OUTLINE]
请输入要查看的类型(TYPICAL): ALL

执行计划如下：

Plan hash value: 3474563263

------------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name                 | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                      |  1344 |   181K|  1347   (0)| 00:00:01 |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE             |  1344 |   181K|  1347   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX_BIGTABLE_DRUG_ID |  1344 |       |     5   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------------

Query Block Name / Object Alias (identified by operation id):
-------------------------------------------------------------

   1 - SEL$1 / BIGTABLE@SEL$1
   2 - SEL$1 / BIGTABLE@SEL$1

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_ID"=17529)

Column Projection Information (identified by operation id):
-----------------------------------------------------------

   1 - "DRUG_ID"[NUMBER,22], "BIGTABLE"."DRUG_CODE"[VARCHAR2,20],
       "BIGTABLE"."DRUG_CHEM_NAME"[VARCHAR2,100], "BIGTABLE"."DRUG_PINYIN"[VARCHAR2,100],
       "BIGTABLE"."DRUG_TRADE_NAME"[VARCHAR2,100], "BIGTABLE"."DRUG_SPEC"[VARCHAR2,50],
       "BIGTABLE"."DRUG_DOSE"[VARCHAR2,20], "BIGTABLE"."DRUG_MANUFATURER"[VARCHAR2,100],
       "BIGTABLE"."DRUG_LICENSE_NO"[VARCHAR2,50], "BIGTABLE"."DRUG_SOURCE_TYPE"[NUMBER,22],
       "BIGTABLE"."DRUG_PACKING_UNIT"[VARCHAR2,20], "BIGTABLE"."DRUG_DOSAGE_UNIT"[VARCHAR2,20],
       "BIGTABLE"."DRUG_DOSAGE_NUM"[NUMBER,22], "BIGTABLE"."DRUG_APPERANCE_NUM"[NUMBER,22],
       "BIGTABLE"."DRUG_APPERANCE_UNIT"[VARCHAR2,255], "BIGTABLE"."DRUG_PHOTO"[LOB,4000],
       "BIGTABLE"."DRUG_TRADE_CODE"[VARCHAR2,255], "BIGTABLE"."DRUG_SPEC_TYPE"[NUMBER,22],
       "BIGTABLE"."STATUS"[NUMBER,22], "BIGTABLE"."REMARK"[VARCHAR2,255],
       "BIGTABLE"."DRUG_WB"[VARCHAR2,100], "BIGTABLE"."TEXT"[LOB,4000]
   2 - "BIGTABLE".ROWID[ROWID,10], "DRUG_ID"[NUMBER,22]
please more [E:\oradba_output\study-xplain.txt]。

18:02:17 study>@explain
SQL执行计划类型[BASIC|TYPICAL|ALL|OUTLINE]
请输入要查看的类型(TYPICAL): outline

执行计划如下：

Plan hash value: 3474563263

------------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name                 | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                      |  1344 |   181K|  1347   (0)| 00:00:01 |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE             |  1344 |   181K|  1347   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX_BIGTABLE_DRUG_ID |  1344 |       |     5   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------------

Outline Data
-------------

  /*+
      BEGIN_OUTLINE_DATA
      BATCH_TABLE_ACCESS_BY_ROWID(@"SEL$1" "BIGTABLE"@"SEL$1")
      INDEX_RS_ASC(@"SEL$1" "BIGTABLE"@"SEL$1" ("BIGTABLE"."DRUG_ID"))
      OUTLINE_LEAF(@"SEL$1")
      ALL_ROWS
      DB_VERSION('12.1.0.2')
      OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
      IGNORE_OPTIM_EMBEDDED_HINTS
      END_OUTLINE_DATA
  */

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_ID"=17529)
please more [E:\oradba_output\study-xplain.txt]。
```

> 详细输出内容：[SQL的执行计划日志](scriptlog/study-xplain.txt)

### <a name='sql_info'>分析SQL运行信息</a>

> `sql_info.sql`分析指定的sql_id运行信息(包括执行成本、执行计划、绑定参数等)
>
> Usage：@sql_info <sql_id>

```
15:38:17 study>@sql_info 7xbz512sqhw2k

more请输入sql_id
Specify [7xbz512sqhw2k] for sql_id

SQL信息如下：
SQL内容
----------------------------------------------------------------------------------------------
SELECT * FROM BIGTABLE WHERE DRUG_ID = :B1

SQL执行计划信息
FLAG   INST_ID SCHEMA_NAME Child Number Plan Hash Value EXECUTIONS elapsed_time(s) PHYSICAL_READ_KB DISK_READS DIRECT_WRITES BUFFER_GETS SQL_PROFILE                    EXACT_MATCHING_SIGNATURE FORCE_MATCHING_SIGNATURE
---- ---------- ----------- ------------ --------------- ---------- --------------- ---------------- ---------- ------------- ----------- ------------------------------ ------------------------ ------------------------
当前          1 MED2                   2      3474563263          1             .01                0          0             0        1364                                8060491763702991538      8060491763702991538
请输入inst_id(         1):
请输入child_number(0): 2

Using [         1]  for inst_id
Using [7xbz512sqhw2k]   for sql_id
Using [         2] for Child_Number

执行计划如下：

SQL_ID 7xbz512sqhw2k, child number 2
-------------------------------------
SELECT * FROM BIGTABLE WHERE DRUG_ID = :B1

Plan hash value: 3474563263

------------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name                 | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                      |       |       |  1347 (100)|       |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE             |  1344 |   186K|  1347   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX_BIGTABLE_DRUG_ID |  1344 |       |     5   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------------

Outline Data
-------------
  /*+
      BEGIN_OUTLINE_DATA
      IGNORE_OPTIM_EMBEDDED_HINTS
      OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
      DB_VERSION('12.1.0.2')
      ALL_ROWS
      OUTLINE_LEAF(@"SEL$1")
      INDEX_RS_ASC(@"SEL$1" "BIGTABLE"@"SEL$1" ("BIGTABLE"."DRUG_ID"))
      BATCH_TABLE_ACCESS_BY_ROWID(@"SEL$1" "BIGTABLE"@"SEL$1")
      END_OUTLINE_DATA
  */

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_ID"=:B1)

SQL参数信息如下：

参数名称                      参数类型                是否捕获     参数值           捕获时间
---------------------------- --------------------- ---------- ------------ --------------------
:B1                           NUMBER                 YES        17530        2017-06-27 15:38:02
please more [E:\oradba_output\study-sql_info_7xbz512sqhw2k.txt]。
```

![sql_info](image/sql_info.png)

> 详细输出内容：[分析SQL运行信息日志](scriptlog/study-sql_info_7xbz512sqhw2k.txt)

### <a name='sql_profile'>sql profile 固化outline</a>

**相关脚本**

```
create_sql_profile.sql		create_sql_profile_sqlid.sql	create_sql_profile_sqltext.sql	sql_profile_info.sql
```

####<a name='create_sql_profile' > 固化v$sql_plan中的outline</a>

> `create_sql_profile.sql`通过sql_id和child_number固化v$sql_plan中的执行计划

```
16:45:07 study>@create_sql_profile
Enter value for sql_id: 7xbz512sqhw2k
Enter value for child_no (0): 2
Enter value for profile_name (PROF_sqlid_planhash):
Enter value for category (DEFAULT):
Enter value for force_matching (TRUE):
SQL Profile PROF_7xbz512sqhw2k_3474563263 created.
SQL PROFILE的相关信息如下:

NAME                          : PROF_7xbz512sqhw2k_3474563263
CATEGORY                      : DEFAULT
SIGNATURE                     : 8060491763702991538
SQL_TEXT                      : SELECT * FROM BIGTABLE WHERE DRUG_ID = :B1
CREATED                       : 2017-06-27 16:46:17
LAST_MODIFIED                 : 2017-06-27 16:46:17
DESCRIPTION                   :
TYPE                          : MANUAL
STATUS                        : ENABLED
FORCE_MATCHING                : YES
TASK_ID                       :
TASK_EXEC_NAME                :
TASK_OBJ_ID                   :
TASK_FND_ID                   :
TASK_REC_ID                   :
TASK_CON_DBID                 :
-----------------

SQL PROFFILE的属性如下:
-------------------------------------
IGNORE_OPTIM_EMBEDDED_HINTS
OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
DB_VERSION('12.1.0.2')
ALL_ROWS
OUTLINE_LEAF(@"SEL$1")
INDEX_RS_ASC(@"SEL$1" "BIGTABLE"@"SEL$1" ("BIGTABLE"."DRUG_ID"))
BATCH_TABLE_ACCESS_BY_ROWID(@"SEL$1" "BIGTABLE"@"SEL$1")

请使用下面 SQL删除sql profile的相关信息
exec  dbms_sqltune.drop_sql_profile('PROF_7xbz512sqhw2k_3474563263');
```

#### <a name='create_sql_profile_sqlid'>固化自定义的 outline</a>

> `create_sql_profile_sqlid.sql` 固化自定义的outline。通过sql_id指定需要固化的sql
>
> Usage:把outline的内容放在`dlt_outline.txt`文件中，运行`@create_sql_profile_sqlid`根据提示输入

**如何获取outline**

* 自己编写，可以仿照着写，有难度不建议使用
* 在sql中加入hint得到自己需要的执行计划后，使用[@explain](#explain)进行分析获取outline

​       把`select /*+ full(BIGTABLE) */  * from   med2.BIGTABLE where drug_id = 17529;`放在`dlt_sqltext.txt`中，运行explain获取outline如下:

```
     BEGIN_OUTLINE_DATA
     FULL(@"SEL$1" "BIGTABLE"@"SEL$1")
     OUTLINE_LEAF(@"SEL$1")
     ALL_ROWS
     DB_VERSION('12.1.0.2')
     OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
     IGNORE_OPTIM_EMBEDDED_HINTS
     END_OUTLINE_DATA
```

把上面的outline内容放入到`dlt_outline.txt`后运行`create_sql_profile_sqlid.sql`

```
17:38:54 study>@create_sql_profile_sqlid
输入sql_id: 7xbz512sqhw2k
输入category (DEFAULT):
输入force_matching (true):
SQL PROFILE的相关信息如下:

NAME                          : DLT_7xbz512sqhw2k_1706271742
CATEGORY                      : DEFAULT
SIGNATURE                     : 8060491763702991538
SQL_TEXT                      : SELECT * FROM BIGTABLE WHERE DRUG_ID = :B1
CREATED                       : 2017-06-27 16:46:17
LAST_MODIFIED                 : 2017-06-27 17:42:51
DESCRIPTION                   : dlt create profiles
TYPE                          : MANUAL
STATUS                        : ENABLED
FORCE_MATCHING                : YES
TASK_ID                       :
TASK_EXEC_NAME                :
TASK_OBJ_ID                   :
TASK_FND_ID                   :
TASK_REC_ID                   :
TASK_CON_DBID                 :
-----------------

SQL PROFFILE的属性如下:
-------------------------------------
BEGIN_OUTLINE_DATA
FULL(@"SEL$1" "BIGTABLE"@"SEL$1")
OUTLINE_LEAF(@"SEL$1")
ALL_ROWS
DB_VERSION('12.1.0.2')
OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
IGNORE_OPTIM_EMBEDDED_HINTS
END_OUTLINE_DATA

请使用下面 SQL删除sql profile的相关信息
exec  dbms_sqltune.drop_sql_profile('DLT_7xbz512sqhw2k_1706271742');

please more [E:\oradba_output\study-create_sql_profile_hint2.log]。

```

#### <a name='create_sql_profile_sqltext'>固化自定义的 outline2</a>

> `create_sql_profile_sqltext.sql` 固化自定义的outline。
>
> 执行步骤:
>
> 1. 把outline的内容放在`dlt_outline.txt`文件中
> 2. 把sql文本放在`dlt_sqltext.txt`文件中
> 3. 最后执行`create_sql_profile_sqltext.sql`

```
22:24:02 study>@create_sql_profile_sqltext
输入category (DEFAULT):
输入force_matching (true):

SQL PROFILE的相关信息如下:

NAME                          : DLT_201706282224
CATEGORY                      : DEFAULT
SIGNATURE                     : 9466144111055232050
SQL_TEXT                      : select  * from  BIGTABLE where drug_id = 175291
CREATED                       : 2017-06-28 22:24:56
LAST_MODIFIED                 : 2017-06-28 22:24:56
DESCRIPTION                   : dlt create profiles
TYPE                          : MANUAL
STATUS                        : ENABLED
FORCE_MATCHING                : YES
TASK_ID                       :
TASK_EXEC_NAME                :
TASK_OBJ_ID                   :
TASK_FND_ID                   :
TASK_REC_ID                   :
TASK_CON_DBID                 :
-----------------

SQL PROFFILE的属性如下:
-------------------------------------
BEGIN_OUTLINE_DATA
FULL(@"SEL$1" "BIGTABLE"@"SEL$1")
OUTLINE_LEAF(@"SEL$1")
ALL_ROWS
DB_VERSION('12.1.0.2')
OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
IGNORE_OPTIM_EMBEDDED_HINTS
END_OUTLINE_DATA

请使用下面 SQL删除sql profile的相关信息
exec  dbms_sqltune.drop_sql_profile('DLT_201706282224');

please more [E:\oradba_output\study-create_sql_profile_hint.log]。

```

> 这个方式很少用，一般用`create_sql_profile_sqlid`

#### <a name='sql_profile_info'>查看sql profile 信息</a>

> `sql_profile_info.sql`:查看详细的sql profile信息
>
> Usage：@sql_profile_info <profile name>

```
22:29:29 study>@sql_profile_info DLT_201706282224
SQL PROFILE的相关信息如下:

NAME                          : DLT_201706282224
CATEGORY                      : DEFAULT
SIGNATURE                     : 9466144111055232050
SQL_TEXT                      : select  * from  BIGTABLE where drug_id = 175291
CREATED                       : 2017-06-28 22:24:56
LAST_MODIFIED                 : 2017-06-28 22:24:56
DESCRIPTION                   : dlt create profiles
TYPE                          : MANUAL
STATUS                        : ENABLED
FORCE_MATCHING                : YES
TASK_ID                       :
TASK_EXEC_NAME                :
TASK_OBJ_ID                   :
TASK_FND_ID                   :
TASK_REC_ID                   :
TASK_CON_DBID                 :
-----------------

SQL PROFFILE的属性如下:
-------------------------------------
BEGIN_OUTLINE_DATA
FULL(@"SEL$1" "BIGTABLE"@"SEL$1")
OUTLINE_LEAF(@"SEL$1")
ALL_ROWS
DB_VERSION('12.1.0.2')
OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
IGNORE_OPTIM_EMBEDDED_HINTS
END_OUTLINE_DATA

请使用下面 SQL删除sql profile的相关信息
exec  dbms_sqltune.drop_sql_profile('DLT_201706282224');
```

**通过[explain](#explain)验证sql profile生效**

```
22:43:01 study>alter session set current_schema = med2;

Session altered.

22:43:08 study>@explain
SQL执行计划类型[BASIC|TYPICAL|ALL|OUTLINE]
请输入要查看的类型(TYPICAL): outline

执行计划如下：

Plan hash value: 2178901462

------------------------------------------------------------------------------
| Id  | Operation         | Name     | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------
|   0 | SELECT STATEMENT  |          |     1 |   142 | 17641   (1)| 00:00:01 |
|*  1 |  TABLE ACCESS FULL| BIGTABLE |     1 |   142 | 17641   (1)| 00:00:01 |
------------------------------------------------------------------------------

Outline Data
-------------

  /*+
      BEGIN_OUTLINE_DATA
      FULL(@"SEL$1" "BIGTABLE"@"SEL$1")
      OUTLINE_LEAF(@"SEL$1")
      ALL_ROWS
      DB_VERSION('12.1.0.2')
      OPTIMIZER_FEATURES_ENABLE('12.1.0.2')
      IGNORE_OPTIM_EMBEDDED_HINTS
      END_OUTLINE_DATA
  */

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("DRUG_ID"=175291)

Note
-----
   - SQL profile "DLT_201706282224" used for this statement
please more [E:\oradba_output\study-xplain.txt]。
```



### <a name='sql_tuning'>SQL调优</a>

#### <a name='sql_tuning_sqltext'> 调优sql文本</a>

> `sql_tuning_sqltext.sql`对指定的sql进行优化
>
> Usage：
>
> 1. 把需要的sql放在`dlt_sqltext.sql`文件中
> 2. 运行`sql_tuning_sqltext.sql`,根据提示输入信息，获取优化建议

```
14:57:38 study>@sqltune_sqltext
"请执行脚本前，把要分析的SQL语句存放在[dlt_sqltext.txt]文件中。"

请输入SQL的Schema Name(USER):med2
正在tuning sql,请耐心等候．．．
任务[DLT_SQLTUNING_201706291516]分析完成。
优化建议结果如下:
--------------------------------------------------------------------------------------------------
GENERAL INFORMATION SECTION
-------------------------------------------------------------------------------
Tuning Task Name   : DLT_SQLTUNING_201706291516
Tuning Task Owner  : DBA_DLT
Workload Type      : Single SQL Statement
Scope              : COMPREHENSIVE
Time Limit(seconds): 9000
Completion Status  : COMPLETED
Started at         : 06/29/2017 15:16:25
Completed at       : 06/29/2017 15:16:25

-------------------------------------------------------------------------------
Schema Name   : MED2
Container Name: PDBORADLT
SQL ID        : 688mgaz3164p2
SQL Text      : select * from   BIGTABLE  where drug_code = '0020020003'

-------------------------------------------------------------------------------
FINDINGS SECTION (1 finding)
-------------------------------------------------------------------------------

1- Index Finding (see explain plans section below)
--------------------------------------------------
  The execution plan of this statement can be improved by creating one or more
  indices.

  Recommendation (estimated benefit: 99.39%)
  ------------------------------------------
  - Consider running the Access Advisor to improve the physical schema design
    or creating the recommended index.
    create index MED2.IDX$$_004A0001 on MED2.BIGTABLE("DRUG_CODE");

  Rationale
  ---------
    Creating the recommended indices significantly improves the execution plan
    of this statement. However, it might be preferable to run "Access Advisor"
    using a representative SQL workload as opposed to a single statement. This
    will allow to get comprehensive index recommendations which takes into
    account index maintenance overhead and additional space consumption.

-------------------------------------------------------------------------------
EXPLAIN PLANS SECTION
-------------------------------------------------------------------------------

1- Original
-----------
Plan hash value: 2178901462

------------------------------------------------------------------------------
| Id  | Operation         | Name     | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------
|   0 | SELECT STATEMENT  |          |  1344 |   186K| 17641   (1)| 00:00:01 |
|*  1 |  TABLE ACCESS FULL| BIGTABLE |  1344 |   186K| 17641   (1)| 00:00:01 |
------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("DRUG_CODE"='0020020003')

2- Using New Indices
--------------------
Plan hash value: 1156591996

------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name           | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                |  1344 |   186K|   106   (0)| 00:00:01 |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE       |  1344 |   186K|   106   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX$$_004A0001 |  1344 |       |     7   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_CODE"='0020020003')

-------------------------------------------------------------------------------


please more [E:\oradba_output\study-sql_tuning.txt]。
```

> 详细输出内容：[调优日志](scriptlog/study-sql_tuning.txt)

#### <a name='sqltune_sqlid'>调优v$sql_plan中执行计划</a>

> `sqltune_sqlid.sql` 通过sql_id和child_number调优v$sql_plan中的执行计划

```
15:28:10 study>@sqltune_sqlid 6m0xgkc6rhrf9

请输入sql_id
Specify [6m0xgkc6rhrf9] for sql_id

SQL信息如下：
SQL内容
-----------------------------------------------------------------------------------------------
select * from   BIGTABLE  where drug_code = '0020020003'

SQL执行计划信息
FLAG   INST_ID SCHEMA_NAME Child Number Plan Hash Value EXECUTIONS elapsed_time(s) PHYSICAL_READ_KB DISK_READS DIRECT_WRITES BUFFER_GETS SQL_PROFILE                    EXACT_MATCHING_SIGNATURE FORCE_MATCHING_SIG
---- ---------- ----------- ------------ --------------- ---------- --------------- ---------------- ---------- ------------- ----------- ------------------------------ ------------------------ ------------------
当前          1 MED2                   0       383109577          1             .06              832        104             0         104                                11524829442477846980     744521565823620194
当前          1 MED2                   1      2178901462          2             .99            19088       2448             0        2387                                11524829442477846980     744521565823620194
请输入inst_id(         1):
请输入child_number(0): 1

Using [         1]  for inst_id
Using [6m0xgkc6rhrf9]   for sql_id
Using [         1] for Child_Number
正在tuning sql,请耐心等候．．．
任务[DLT_SQLTUNING_201706291528]分析完成。
优化建议结果如下:
--------------------------------------------------------------------------------------------------
GENERAL INFORMATION SECTION
-------------------------------------------------------------------------------
Tuning Task Name   : DLT_SQLTUNING_201706291528
Tuning Task Owner  : DBA_DLT
Workload Type      : Single SQL Statement
Scope              : COMPREHENSIVE
Time Limit(seconds): 9000
Completion Status  : COMPLETED
Started at         : 06/29/2017 15:28:33
Completed at       : 06/29/2017 15:28:35

-------------------------------------------------------------------------------
Schema Name   : MED2
Container Name: PDBORADLT
SQL ID        : 6m0xgkc6rhrf9
SQL Text      : select * from   BIGTABLE  where drug_code = '0020020003'

-------------------------------------------------------------------------------
FINDINGS SECTION (2 findings)
-------------------------------------------------------------------------------

1- Index Finding (see explain plans section below)
--------------------------------------------------
  The execution plan of this statement can be improved by creating one or more
  indices.

  Recommendation (estimated benefit: 99.39%)
  ------------------------------------------
  - Consider running the Access Advisor to improve the physical schema design
    or creating the recommended index.
    create index MED2.IDX$$_004B0001 on MED2.BIGTABLE("DRUG_CODE");

  Rationale
  ---------
    Creating the recommended indices significantly improves the execution plan
    of this statement. However, it might be preferable to run "Access Advisor"
    using a representative SQL workload as opposed to a single statement. This
    will allow to get comprehensive index recommendations which takes into
    account index maintenance overhead and additional space consumption.

2- Alternative Plan Finding
---------------------------
  Some alternative execution plans for this statement were found by searching
  the system's real-time and historical performance data.

  The following table lists these plans ranked by their average elapsed time.
  See section "ALTERNATIVE PLANS SECTION" for detailed information on each
  plan.

  id plan hash  last seen            elapsed (s)  origin          note
  -- ---------- -------------------- ------------ --------------- ----------------
   1  383109577  2017-06-29/14:52:43        0.064 Cursor Cache    not reproducible
   2 2178901462  2017-06-29/15:23:39        0.993 Cursor Cache    original plan

  Information
  -----------
  - All alternative plans other than the Original Plan could not be
    reproduced in the current environment.
  - The plan with id 1 could not be reproduced because the object
    "IDX_BIGTABLE_DRUG_CODE" with type "INDEX" is missing.  For this reason,
    a SQL plan baseline cannot be created to instruct the Oracle optimizer to
    pick this plan in the future.

-------------------------------------------------------------------------------
EXPLAIN PLANS SECTION
-------------------------------------------------------------------------------

1- Original
-----------
Plan hash value: 2178901462

------------------------------------------------------------------------------
| Id  | Operation         | Name     | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------
|   0 | SELECT STATEMENT  |          |  1344 |   186K| 17641   (1)| 00:00:01 |
|*  1 |  TABLE ACCESS FULL| BIGTABLE |  1344 |   186K| 17641   (1)| 00:00:01 |
------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("DRUG_CODE"='0020020003')

2- Using New Indices
--------------------
Plan hash value: 3881175032

------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name           | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                |  1344 |   186K|   106   (0)| 00:00:01 |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE       |  1344 |   186K|   106   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN                  | IDX$$_004B0001 |  1344 |       |     7   (0)| 00:00:01 |
------------------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   2 - access("DRUG_CODE"='0020020003')

-------------------------------------------------------------------------------
ALTERNATIVE PLANS SECTION
-------------------------------------------------------------------------------

Plan 1
------

  Plan Origin                 :Cursor Cache
  Plan Hash Value             :383109577
  Executions                  :1
  Elapsed Time                :0.064 sec
  CPU Time                    :0.000 sec
  Buffer Gets                 :104
  Disk Reads                  :104
  Disk Writes                 :0

Notes:
  1. Statistics shown are averaged over multiple executions.
  2. This plan could not be reproduced because the object "IDX_BIGTABLE_DRUG_CODE" with type "INDEX" is missing.

--------------------------------------------------------------------------------------------------------------
| Id  | Operation                           | Name                   | Rows  | Bytes | Cost (%CPU)| Time     |
--------------------------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT                    |                        |       |       |  1317 (100)|       |
|   1 |  TABLE ACCESS BY INDEX ROWID BATCHED| BIGTABLE               |  1344 |   186K|  1317   (0)| 00:00:01 |
|   2 |   INDEX RANGE SCAN                  | IDX_BIGTABLE_DRUG_CODE |  1344 |       |     7   (0)| 00:00:01 |
--------------------------------------------------------------------------------------------------------------

Plan 2
------

  Plan Origin                 :Cursor Cache
  Plan Hash Value             :2178901462
  Executions                  :2
  Elapsed Time                :0.993 sec
  CPU Time                    :0.008 sec
  Buffer Gets                 :2387
  Disk Reads                  :2448
  Disk Writes                 :0

Notes:
  1. Statistics shown are averaged over multiple executions.
  2. The plan matches the original plan.

------------------------------------------------------------------------------
| Id  | Operation         | Name     | Rows  | Bytes | Cost (%CPU)| Time     |
------------------------------------------------------------------------------
|   0 | SELECT STATEMENT  |          |  1344 |   186K| 17641   (1)| 00:00:01 |
|*  1 |  TABLE ACCESS FULL| BIGTABLE |  1344 |   186K| 17641   (1)| 00:00:01 |
------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("DRUG_CODE"='0020020003')

-------------------------------------------------------------------------------


please more [E:\oradba_output\study-sql_tuning2_6m0xgkc6rhrf9.txt]。

15:28:36 study>
```

> 详细输出内容：[调优日志](scriptlog/study-sql_tuning2_6m0xgkc6rhrf9.txt)



### <a name='sqlAccess'>SQL ACCESS 优化</a>

* `sqlAccess_sqltext.sql`

  ```
  15:28:36 study>@sqlAccess_sqltext
  "请执行脚本前，把要分析的SQL语句存放在[dlt_sqltext.txt]文件中。"

  请输入SQL的Schema Name(USER):med2

  正在tuning sql,请耐心等候．．．
  DECLARE
  *
  ERROR at line 1:
  ORA-00604: error occurred at recursive SQL level 1
  ORA-01843: not a valid month
  ORA-06512: at "SYS.DBMS_ADVISOR", line 201
  ORA-06512: at line 142

  ```

  > 注：由于该脚本支持10g，不支持11g以后的数据库，所以报错


* `sqlAccess_sqlid.sql`

  ```
  15:57:42 study>@sqlAccess_sqlid 6m0xgkc6rhrf9

  请输入sql_id
  Specify [6m0xgkc6rhrf9] for sql_id
  正在tuning sql,请耐心等候．．．
  DECLARE
  *
  ERROR at line 1:
  ```

  > 注：由于该脚本支持10g，不支持11g以后的数据库，所以报错

* `sqlAccess_sqlSet.sql`

  ```
  16:02:43 study>@sqlAccess_sqlset 6m0xgkc6rhrf9

  请输入sql_id
  Specify [6m0xgkc6rhrf9] for sql_id
  正在tuning sql,请耐心等候．．．
  任务[DLT_SQLACCESS_201706291605]分析完成。

  任务信息如下:
     TASK_ID STATUS     RECOMMENDATION_COUNT
  ---------- ---------- --------------------
          85 COMPLETED                     1

  建议执行以下优化脚本:
  -----------------------------------------------------------------------------------------
  Rem  SQL Access Advisor: Version 12.1.0.2.0 - Production
  Rem
  Rem  Username:        DBA_DLT
  Rem  Task:            DLT_SQLACCESS_201706291605
  Rem  Execution date:
  Rem

  CREATE BITMAP INDEX "MED2"."BIGTABLE_IDX$$_00550000"
      ON "MED2"."BIGTABLE"
      ("DRUG_CODE")
      COMPUTE STATISTICS;

  优化详细信息：

  USERNAME                      : MED2
  STATEMENT                     : select * from   BIGTABLE  where drug_code = '0020020003'

  Original Cost                 : 17642
  New Cost                      : 346
  Cost Improvenment             : 17296
  Cost Improvenment(%)          : 98.04
  Execution Count               : 1
  RECOMMENDATION                : CREATE BITMAP INDEX "MED2"."BIGTABLE_IDX$$_00550000"
      ON "MED2"."BIGTABLE"
      ("DRUG_CODE")
      COMPUTE STATISTICS;

  -----------------
  please more [E:\oradba_output\study-sql_access3_6m0xgkc6rhrf9.txt]。
  ```

  ​

### <a name='11g'>11g SQL监控</a>

#### <a name='rpt_sml'>SQL监控列表</a>

> `rpt_sml.sql`查看n小时内的sql 监控列表，生成html文件
>
> 注：`rpt_sml2.sql`生成文本文件(按照`ELAPSED TIME`倒序排列)
  ```
14:23:39 study>@11g/rpt_sml
请输入一个数值，代表n小时内(1): 24
...
...
and constraint_type in (&apos;P&apos;, &apos;U&apos;, &apos;R&apos;, &apos;C&apos;)
order by decode(constraint_type, &apos;P&apos;, 0, &apos;U&apos;, 1, &apos;R&apos;, 2, 3), constraint_name
</span>
          </a>
        </td>
      </tr>
    </table>
  </body>
</html>

please more [E:\oradba_output\study-sql_monitor_list_201706301432-24.html]。

  ```

在浏览器中打开:

![rpt_sml](image/rpt_sml.png)

#### <a name='rpt_sm'>SQL监控报告</a>

> `rpt_sm.sql`
>
> 注：输入的sql_id为sql monitoring list 中的sql_id。

```
19:43:19 study>@11g/rpt_sm
输入sql_id: a3w7p0wh33s3d

<html>
 <head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
  <script language="javascript" type="text/javascript">
   <!--
      var version = "12.1.0.2.0";
     var swf_base_path = "http://download.oracle.com/otn_software/";
    
         document.write('<script language="javascript" type="text/javascript" ' +
                        'src="' + swf_base_path + 'emviewers/scripts/activeReportInit.js?' +
                        Math.floor((new Date()).getTime()/(7*24*60*60*1000)) +
                       '"></' + 'script>');
      -->
  </script>
 </head>
 <body onload="sendXML();">
  <script type="text/javascript">
       writeIframe();
     </script>
  <script id="fxtmodel" type="text/xml">
   <!--FXTMODEL-->
   <report db_version="12.1.0.2.0" elapsed_time="2.25" cpu_time="0.69" cpu_cores="1" hyperthread="N" con_id="3" con_name="PDBORADLT" timezone_offset="28800" packs="2" encode="base64" compress="zlib">
    <report_id><![CDATA[/orarep/sqlmonitor/main?sql_id=a3w7p0wh33s3d]]></report_id>
                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
qPDb8q8bqn+JZL/6L5bcv8I=
            </report>
   <!--FXTMODEL-->
  </script>
 </body>
</html>

open [E:\oradba_output\study-sql_monitor_a3w7p0wh33s3d.html]...
```

在浏览器中打开:

![rpt_sm](image/rpt_sm.png)

#### <a name='rpt_sd'>SQL监控详细信息</a>

> `rpt_sd.sql`
>
> 注：输入的sql_id为sql monitoring list 中的sql_id。

```
09:33:19 study>@11g/rpt_sd
输入sql_id: a3w7p0wh33s3d
请输入一个数值，代表n小时内(1):
<html>
 <head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
  <script language="javascript" type="text/javascript">
   <!--
      var version = "12.1.0.2.0";
     var swf_base_path = "http://download.oracle.com/otn_software/";
    
         document.write('<script language="javascript" type="text/javascript" ' +
                        'src="' + swf_base_path + 'emviewers/scripts/activeReportInit.js?' +
                        Math.floor((new Date()).getTime()/(7*24*60*60*1000)) +
                       '"></' + 'script>');
      -->
  </script>
 </head>
 <body onload="sendXML();">
  <script type="text/javascript">
       writeIframe();
     </script>
  <script id="fxtmodel" type="text/xml">
   <!--FXTMODEL-->
   <report db_version="12.1.0.2.0" elapsed_time="13.93" cpu_time="1.70" cpu_cores="1" hyperthread="N" con_id="3" con_name="PDBORADLT" timezone_offset="28800" packs="2" encode="base64" compress="zlib">
    <report_id><![CDATA[/orarep/sql_detail/main?report_level=all&sql_id=a3w7p0wh33s3d&start_time=06:30:2017 08:36:23]]></report_id>
                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
5r8JhRIbffomCJovOP8JQw/MOw==
            </report>
   <!--FXTMODEL-->
  </script>
 </body>
</html>

please more [E:\oradba_output\study-sql_detail_a3w7p0wh33s3d.html]。
```

#### <a name='rat_astt'>自动sql调优报告</a>

> `rpt_astt.sql`

```
15:29:04 study>@11g/rpt_astt

Current Instance
​~~~~~~~~~~~~~~~~
   DB Id    DB Name      Inst Num Instance
----------- ------------ -------- ------------
 1600670143 ORADLT              1 oradlt


Specify the number of days of executions to choose from
​~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Entering the number of days (n) will result in the most recent
(n) days of executions being listed.  Pressing <return> without
specifying a number lists all completed executions.


Enter value for num_days: 4
Listing the last 4 days of Completed executions
Task Name                      Execution Name                 EXECDAT              STATUS          ERROR_MESSAGE
------------------------------ ------------------------------ -------------------- --------------- ------------------------------
SYS_AUTO_SQL_TUNING_TASK       EXEC_370                       2017-06-28 22:00     COMPLETED
SYS_AUTO_SQL_TUNING_TASK       EXEC_387                       2017-06-30 15:05     COMPLETED


Specify the Begin and End Execution Names
​~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
if begin_exec and end_exec not input, will show last Execution.
Enter value for begin_exec: EXEC_370
Begin Execution  Name specified: EXEC_370

Enter value for end_exec: EXEC_387
End   Execution  Name specified: EXEC_387

-------------------------------------------------------------------------------
GENERAL INFORMATION SECTION
-------------------------------------------------------------------------------
Tuning Task Name                        : SYS_AUTO_SQL_TUNING_TASK
Tuning Task Owner                       : SYS
Workload Type                           : Automatic High-Load SQL Workload
Time Span                               : 06/28/2017 22:00 - 06/30/2017 15:14
Number of Executions                    : 2
First Execution Name                    : EXEC_370
Last Execution Name                     : EXEC_387
Number of Candidate SQLs (w/duplicates) : 26
Cumulative Elapsed Time of SQL (s)      : 168

-------------------------------------------------------------------------------
SUMMARY SECTION
-------------------------------------------------------------------------------
                      Global SQL Tuning Result Statistics
-------------------------------------------------------------------------------
Number of SQLs Analyzed (w/duplicates)       : 26
Number of SQLs Analyzed                      : 23
Number of SQLs in the Report                 : 19
Number of SQLs with Findings                 : 19
Number of SQLs with Alternative Plan Findings: 1
Number of SQLs with SQL profiles recommended : 8
Number of SQLs with Index Findings           : 2
Number of SQLs with SQL Restructure Findings : 3

-------------------------------------------------------------------------------
      SQLs with SQL Profile Findings Ordered by Maximum Benefit, Object ID
-------------------------------------------------------------------------------
execution name                 object ID  SQL ID        benefit
------------------------------ ---------- ------------- --------
EXEC_370                                6 cu0cns77yr10c   99.99%
EXEC_370                               11 3kku08dqq6vgm   98.56%
EXEC_387                               21 3k2d2bx9aa3jf   93.33%
EXEC_387                               23 bzz1sydq3g2ma   88.49%
EXEC_387                               26 3wr2mysfyh36n   50.00%
EXEC_387                               14 ccjh63b958xbt   16.12%
EXEC_387                               28 3mf07mp2t5kg9   13.61%
EXEC_370                                8 cf8fyu7n9yc27 <=10.00%

-------------------------------------------------------------------------------
         SQLs with Index Findings Ordered by Maximum Benefit, Object ID
-------------------------------------------------------------------------------
execution name                 object ID  SQL ID        benefit
------------------------------ ---------- ------------- --------
EXEC_387                               27 6m0xgkc6rhrf9   99.39%
EXEC_387                               13 a3w7p0wh33s3d   92.10%

.......
.......
.......

Object ID     : 22
Schema Name   : MED2
Container Name: PDBORADLT
SQL ID        : 8dzyy7wykhg7j
SQL Text      : select col.*, com.Comments
                from sys.all_tab_cols col,
                     sys.all_col_comments com
                where col.owner = 'MED2'
                and col.table_name = 'BIGTABLE'
                and com.Owner (+) = 'MED2'
                and com.Table_Name (+) = 'BIGTABLE'
                and com.Column_Name (+) = col.Column_Name
                order by col.column_id

-------------------------------------------------------------------------------
ADDITIONAL INFORMATION SECTION
-------------------------------------------------------------------------------
- Type of SQL statement not supported.

-------------------------------------------------------------------------------
Object ID     : 24
Schema Name   : MED2
Container Name: PDBORADLT
SQL ID        : fz4bkc3p208yd
SQL Text      : SELECT /* DS_SVC */ /*+ dynamic_sampling(0) no_sql_tune
                no_monitoring optimizer_features_enable(default) no_parallel
                result_cache(snapshot=3600) */ C1, C2, C3 FROM (SELECT /*+
                qb_name("innerQuery") NO_INDEX_FFS( "DO")  */ 4294967295 AS
                C1, COUNT(*) AS C2, SUM(CASE WHEN ("DO"."TYPE#"=92) THEN 1
                ELSE 0 END) AS C3  FROM SYS."OBJ$" "DO" WHERE
                ("DO"."TYPE#"=92)) innerQuery

-------------------------------------------------------------------------------
ADDITIONAL INFORMATION SECTION
-------------------------------------------------------------------------------
- Type of SQL statement not supported.

-------------------------------------------------------------------------------



please more [E:\oradba_output\study-auto_tuning_task_EXEC_370_EXEC_387.txt]。
```

> 详细输出内容：[自动sql调优报告:study-auto_tuning_task_EXEC_370_EXEC_387.txt](scriptlog/study-auto_tuning_task_EXEC_370_EXEC_387.txt)

