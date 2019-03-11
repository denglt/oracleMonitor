## <a name='install'>安装监控脚本</a>

<a href='index.html' target="_blank">Index</a>

**安装相关脚本：**

```
ObjPriOnSys.sql             ct_dlt_migrateobj_log.sql       dlt_sys$migrateobj.plb
cr_directory.sql            ct_dlt_session_sql.sql          dlt_sys$t.pck
ct_dlt_access_sql.sql       dltInstaller.sql                dlt_sys$t.plb
ct_dlt_blocklock.sql        dlt_sys$m.pck                   dlt_sys$tr_trace_ddl.sql
ct_dlt_longlock.sql         dlt_sys$m.plb                   dlt_sys$tr_trace_servererror.sql
ct_dlt_longops.sql          dlt_sys$migrateobj.pck
```
> 其中`dltInstaller.sql`为安装入口脚本。
>
> 安装脚本将创建一个监控账号，根据提示信息，输入用户名和密码，一键式完成脚本安装。
>
> 建议使用独立的监控账号来存放监控脚本，并作为日常维护登录用

**在sqlplus中执行`dltInstaller.sql`**

```
SQL>  conn / @localhost:1521/pdboradlt  as sysdba
已连接。
SQL> @dltInstaller
创建管理用户
请输入用户名:dba_dlt
请输入密码:
用户已创建。

开始给用户[dba_dlt]授予权限
视图已创建。
同义词已创建。
授权成功。
...
...

现在以[dba_dlt]用户连接数据库，并创建监控脚本
已连接。
序列已创建。
...
...
后台脚本安装完成!
```


[完整安装日志](scriptlog\install.log)
