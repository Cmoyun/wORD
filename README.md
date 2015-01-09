Box 转移 Copy
数据库
======================================
> t_user

```sql
Field           Type            Constraint          FromTable
ID              int(10)         primary key & AI            
UserName        varchar(15)     unique & not null
NickName        varchar(15)     not null
PassWord        varchar(32)     not null
Email           varchar(25)
Phone           varchar(11)
BoxNum          int(2)          check <=10 & default 0
SnapshotNum     int(2)          check <=20 & default 0
isEnable        enum('N','Y')   default 'Y'
```
> t_box

```sql
Field           Type            Constraint          FromTable
ID              varchar(64)     primary key         
BoxName         varchar(30)     not null
SnapshotNum     int(2)          check <=5 & default 0
User            int(10)         not null            t_user
```
> t_snapshots

```sql
Field           Type            Constraint          FromTable
ID              varchar(64)     primary key         
SnapshotsName   varchar(30)     not null
Box             varchar(64)     foreign key         t_box
User            int(10)         foreign key         t_user
```

# 前台访问
```
【遇到错误-响应】:
    {
        "Success":false,
        "ErrorCode":12,
        "Msg":...
    }
```

-----------------------------------------
<br/>


> POST _&_ /user/add  
描述：注册用户  
【ErrorCode】  
    101：用户名称已存在  
    109：未知错误
```
【提交要求】: form 表单 POST 提交
字段：
    UserName(用户名-不可更改-唯一,必须)
    NickName(昵称-可更改,必须)
    PassWord(可更改,必须)
    Email(可更改)
    Phone(可更改)
【正确响应格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /user/change  
描述：更改用户

```
【提交要求】: Data POST
    {
        "UserName":"mzzo",
        "Data":{
            "NickName":"MZZO-JSK",
            "PassWord":"MZZO&12458",
            "Email":"Mzzo.JSK@mzzo.com",
            "Phone":"18649738444"
        }
    }
【正确响应格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /user/login  
描述：验证用户

```
【提交要求】: form 表单 POST 提交
字段：
    UserName(必须)
    PassWord(必须)
【正确响应格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /user/see  
描述：查看资料

```
【提交要求】: GET
【正确响应格式】:
    {
        "Success":true,
        "Msg":{
            "NickName":"MZZO-JSK",
            "BoxNum":5,
            "SnapshotNum":0,
            "Email":"Mzzo.JSK@mzzo.com",
            "Phone":"18649738444"
        }
    }
```

-----------------------------------------
<br/>


> POST _&_ /box/add  
描述：创建Box,最多<font color="red">10</font>个  
【ErrorCode】  
    //201：Box名称已存在  
    209：未知错误


```
【提交要求】: Data POST 
字段：  
    {
        "BoxName":"TestBox"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /box/delete  
描述：只删除Box  
【ErrorCode】   
    209：未知错误
    205：密码错误  

```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
        "PassWord":"wewewe"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /box/deleteAll  
描述：删除Box,和 它的所有 Snapshots 【事务】  
【ErrorCode】   
    209：未知错误


```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
        "PassWord":"wewewe"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /box/change  
描述：修改Box名称  
【ErrorCode】   
    209：未知错误


```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e"，
        "BoxName":"TestBox"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":..
    }
```
> POST _&_ /box/query  
描述：查询单个Box 的 Name 和 下面的所有Snapshots  
【ErrorCode】   
    209：未知错误


```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":{
            "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
            "BoxName":"TestBox",
            "SnapshotNum":2,
            "Snapshots":[
                {
                    "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
                    "SnapshotsName":"TestSnapshot"
            },
                {
                    "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
                    "SnapshotsName":"TestSnapshot"
            }
        ]
    }
```
> POST _&_ /box/queryAll  
描述：查询所有Box ID Name  
【ErrorCode】   
    209：未知错误


```
【提交要求】: Data GET 
【正确格式】:
    {
        "Success":true,
        "Msg":[
          {
            "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
            "BoxName":"TestBox"
          },{
            "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
            "BoxName":"TestBox"
          },{
            "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
            "BoxName":"TestBox"
          }
        ]
    }
```
#Box功能模块  
  *  暂停Box: POST /box/pause
  *  启动Box: POST /box/start
  *  重启Box: POST /box/reboot  
  
格式：   
{  
     "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e"  
}

----------------------------------------------------
<br/>


> Post _&_ /snapshots/add  
描述：创建快照,<font color="red">max</font>(20-Snapshots),每个 Box 最多 5 个 Snapshots

```
【提交要求】: Data POST 
字段：  
    {
        "SnapshotsName":"TestSnapshots"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":OK
    }
```
> Post _&_ /snapshots/delete  
描述：删除 单个 Snapshots

```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
        "PassWord":"wewewe"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":OK
    }
```
> Post _&_ /snapshots/change  
描述：更改 Snapshots 名

```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
        "SnapshotsName":"testSnapshots"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":OK
    }
```
> Post _&_ /snapshots/query  
描述：查询 Snapshots

```
【提交要求】: Data POST 
字段：  
    {
        "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e"
    }
【正确格式】:
    {
        "Success":true,
        "Msg":{
               "ID":"6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
               "Snapshots":"testSnapshots"
            }
    }
```
> Post _&_ /snapshots/queryAll  
描述：查询所有 Snapshots

```
【提交要求】: Data POST 
【正确格式】:
    {
        "Success":true,
        "Msg": [
            {
              "ID": "6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
              "ParentId":"b43856fb3840ff7b00ca465e2e75e334234da82260c47f0d9c284d52c666d04d",
              "Snapshots": "testSnapshots"
            },
            {
              "ID": "6430c37e5891f45717ea4b2ffbee4276e67ff62d60b56f41dff11956ef57bd8e",
              "Snapshots": "testSnapshots"
            }
          ]
    }
```

