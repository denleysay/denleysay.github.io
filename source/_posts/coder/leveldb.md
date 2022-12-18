---
title:  "leveldb源码分析"
date:   2017-03-05 12:00:00
categories: 
  - 代码分析师
tags: [C++,Third-Party,Database]
---

Leveldb，一个google实现的非常高效的kv数据库

<!-- More -->

# 准备
* 下载[源码](https://github.com/google/leveldb)
* 执行`make`编译，结果分别在以下目录中
  - out-static: 静态库版本
  - out-shared: 动态库版本

以下为求统一，都只选用静态库版本进行示范

# 测试
## 功能测试
* 全部测试：`make check`
* 单个测试：`./out-static/db_test`
  
## 性能测试
* leveldb
因已编译 ，只需执行：`./out-static/db_bench`
* sqlite  
```
brew install sqlite  # 安装依赖
make out-static/db_bench_sqlite3 # 编译
./out-static/db_bench_sqlite3
```
* treedb
```
brew install kyoto-cabinet  # 安装依赖kyotocabinet
make out-static/db_bench_tree_db # 编译
./out-static/db_bench_tree_db
```
  
这里是[LevelDB、TreeDB、SQLite3性能对比测试](http://www.dedecms.com/knowledge/data-base/nosql/2012/0820/8693.html)结果

# 分析
从核心api接口入手: 接口文件是`db.h`, 实现文件是`db_impl.h`

# 参考
* [makefile文件解析](http://blog.csdn.net/u014120684/article/details/46352081)
