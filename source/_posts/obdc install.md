---
title: ODBC(Open Data Base Connection) Install
tags:
  - Database
categories: CS数据库
---

# odbc简介
https://blog.csdn.net/Alearn_/article/details/80213280

# oracle odbc驱动的下载
在安装好oracle 11g 11.2.0.1的前提下,再去oracle官网找到有关odbc的两个压缩包,合并后安装.
https://blog.csdn.net/w1820020635/article/details/87705190
https://www.oracle.com/database/technologies/releasenote-odbc-ic.html
```
e:
cd E:\download\instantclient_11_2
odbc_install.exe
```
能在windows\system32找到64位的odbcad32.exe,即ODBC数据源管理程序

# 创建Oracle普通用户
https://housen1987.iteye.com/blog/1345496
```
sqlplus sys/dwh as sysdba;
    create user yi identified by root;  
    conn sys/dwh as sysdba;
    grant create session to yi;
    grant connect,resource to yi;
    conn yi/root;
```  


# ODBC 连接 oracle
https://www.experts-exchange.com/questions/27810045/ODBC-driver-configuration.html
添加新的数据源,让C++与此数据源交互



<!-- Data Source Name yi0
TNS Service Name localhost:1521/orcl
User ID yi
PASSWORD root -->
```
