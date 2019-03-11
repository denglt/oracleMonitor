## <a name='config'>配置运行环境</a>
<a href='index.html' target="_blank">Index</a>
### <a name='connection'>配置数据库连接信息</a>

---

####  <a name='conn-win'>window操作系统</a>

>用`dltSqlplus.bat`登录要监控的数据库

打开`dltSqlplus.bat`文件，大致内容如下：

```bat
echo 1:学习(self)
echo 2:
echo 3:
echo q:退出
choice /C 123456789Q /M "请选择:" /T 999 /D 2 /N
set nls_lang=AMERICAN_AMERICA.ZHS16GBK
cls
set system=window

if %ERRORLEVEL%==1 (
  title "学习(self)"
  set username=dba_dlt
  set password=********
  set service="(DESCRIPTION = (ADDRESS = (PROTOCOL = TCP)(HOST = 10.10.10.128)(PORT = 1521)) (CONNECT_DATA = (SERVER = DEDICATED) (SERVICE_NAME = pdboradlt)))"
  set prompt=study
  rem sqlplus /nolog @LogonDB !username! !password! !service! !prompt!  !system!
)
if %ERRORLEVEL%==2 (
  title ""
  set username=
  set password=
  set service=
  set prompt=
)
```

**参数说明**:

* `username` :安装脚本时创建的用户名
* `password`:安装脚本时输入的密码
* `service`:连接数据库服务字符串
* `prompt`:sqlpuls提示字符串；同时作为输出结果文件名的前缀
* `system`:操作系统平台，默认window


#### <a name='conn-mac'>linux|mac操作系统</a>

> 用`dltSqlplus.sh`登录要监控的数据库

打开`dltSqlplus.sh`文件，大致内容如下：

```sh
do
  case $REPLY in
    1)
      username=dba_dlt
      password=dlt760416
      service="(DESCRIPTION = (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.105)(PORT = 1521)) (CONNECT_DATA = (SERVER = DEDICATED) (SERVICE_NAME = pdboradlt)))"
      prompt=study
      system=mac
      break
      ;;
    2)
      username=
      password=
      service=
      prompt=
      system=
      break
      ;;
    5)
      break
      ;;
    *) echo invalid option;;
  esac
done
```



> 参数`system`可以配置`mac`|`linux`
>
> 注：这五个配置参数都必须有值

#### <a name='conn-example'>连接数据库事例</a>

```
E:\oracle_dba\sale\admin>dltSqlplus.bat
1:学习(self)
2:
3:
q:退出
请选择:1
SQL*Plus: Release 12.1.0.2.0 Production on Tue Jun 13 17:38:48 2017

Copyright (c) 1982, 2014, Oracle.  All rights reserved.

Connected.

INSTANCE_NUMBER
---------------
              1
17:38:49 study>
17:39:26 study>show user
USER is "DBA_DLT"
```

> 注：`INSTANCE_NUMBER`为登陆数据库的实例id(instance_id)

### <a name='runtime'>配置运行时参数</a>

---

打开`define_global_var.sql`文件，有如下内容：

```sql
select case
         when '&system' in ('mac','linux') then
           '/Users/denglt/oradba_output/'
         else
           'E:\oradba_output\'
       end  script_output_dir ,
       case
         when '&system' in ('mac','linux') then
           '/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome'
         else
           '"C:\Program Files (x86)\Internet Explorer\iexplore.exe"'
       end  open_file_text ,
       case
         when '&system' in ('mac','linux') then
           '/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome'
         else
           '"C:\Program Files (x86)\Internet Explorer\iexplore.exe"'
       end  open_file_html,
       case
         when '&system' in ('mac','linux') then
           'mv'
         else
           'move'
       end  cmd_move_file
  from dual;

rem 是否使用工具打开文件(1:使用；其他不使用）
define  flag_use_opentool =1
ALTER SESSION SET TIME_ZONE = '+8:00';
alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss';
alter session set nls_timestamp_format = 'yyyy-mm-dd hh24:mi:ss';
alter session set nls_timestamp_tz_format = 'YYYY-MM-DD HH:MI:SS.FF TZH:TZM';
```

* `script_output_dir`:脚本输出结果文件的存放目录
* `open_file_text`:打开文本文件的工具
* `open_file_html`:打开html文件的工具
* `cmd_move_file`:操作系统移动文件的命令
* `flag_use_opentool`:是否使用工具打开输出文件
