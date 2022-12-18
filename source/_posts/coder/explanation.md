---
title:  "几点说明"
date:   2017-02-26 12:00:00
categories: 
  - 代码分析师
---

没有规矩，不成方圆  

<!-- More -->

为了避免后续文章的重复说明，现统一如下：（其中ca前缀是代码分析师“code analyst”的缩写）

# 环境说明  

|  |最低要求|我的|建议|
|---|---|---|---|
| 电脑   |  | MacBook Air |  |
| 操作系统 |  | macOS Sierra 版本 10.12.3（16D32) | Mac OS/Ubuntu |
| 文本编辑器 |  | TextMate version 2.0-rc.4 | TextMate/Atom |
| 版本控制系统 |支持分布式 | Git version 2.7.4 (Apple Git-66) | Git |
| Github客户端	|  | Github desktop Deer Types (222) | Github desktop |

注：如无特别的环境说明，后续文章都是在"我的"环境下完成的。

# 操作约定
* 我的Github仓库：www.github.com/denleyhsiao  
	后续统一表示为$ca_repo；
* 如何获取到文章中相关的项目资料？  
	`git clone $ca_repo/$project_name`  
	其中`$project_name`为项目名称，后续统一用`$project_path`表示项目目录，  
	当然通过Github desktop也是可以的获取的；  
* 示例，文档等资料在哪里？  
	`$project_path/ca_XXX`目录下, 如`Sigslot/ca_unit_test`、`Sigslot/ca_doc`；
* 如何贡献到开源社区？  
	由于主分支master中加入了与文章相关的内容（如：示例，文档等），每个项目会另开一个origin分支，其上的更新才会PR到原开源项目上；

# 新增文件(夹)
&emsp;&emsp;以下文件（夹）都位于`$project_path`目录下 

* `.gitignore`：定义git所要忽略的文件；  
* `.gitmodules`: 定义项目所依赖的第三方库；  
* `ca_third_party`: 项目所依赖的第三方库；  
* `ca_unit_test`: 项目单元测试目录；  
* `ca_doc`: 项目文档。  

# 单元测试库
&emsp;&emsp;由于所有代码分析都是测试驱动的，因此每个项目都会有一个第三方单元测试工具（项目本身带单元测试的除外）；  
&emsp;&emsp;c++项目的单元测试工具是CppUnitLite，位于`$project_name/ca_third_party/CppUnitLite`, 后续统一表示为`$ca_unit_test_lib`。

# 你要做的
1. 下载源码：详见上面【操作约定】的第二步；  
2. 终端下进入`$project_path`目录；
3. 编译测试：（进入`$project_path/ca_unit_test`目录）   
	执行命令 `./autogen.sh`；  
4. 运行测试：`./{$project_name}_test`。  
	如Sigslot项目为执行 `./Sigslot_test` 。