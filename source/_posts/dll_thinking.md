---
title: C++ 库总结
date: 2010-09-16 15:54:00
categories: 
- 技术
- C++
---

个人对DLL的一些心得总结

<!-- More -->

以下总结只针对windows平台:
* dll文件应有static 和 dynamic的版本，编译时临时文件的目录分别是: Debug, Release, DebugDll, ReleaseDll。  一般推荐使用dll的static版本，因为这样就不用指定dll文件所在的路径，以后对文件大小或其它要求时再换成dynamic版本的；但中间要注意的是:如果static版本文件有所改变，依赖其的项目要手动重新编译，不然使用的还是老版本的static 库。

* dll文件命名为: `XxD.lib(Debug), Xx.lib(Release), XxD_dll.lib(Debug Dynamic), Xx_dll.lib(Release Dynamic)`, 如果是Unicode版本,在相应的'.'或'D'字符前加U，如:`XxUD.lib(Debug Unicode), XxU.lib(Release Unicode)`。

* 作为第三方API时，应提供完整的include, lib, bin目录，readme.txt文件，可选择提供example, doc目录。 其中：include文件夹中放依赖的头文件；lib中放所有的*.lib文件；bin中放所有的*.dll文件。

* 作为第三方API时，最好是通过def的方式生成，这样可以跨语言使用。如果只限于c++调用，并且要导出的类比较多，则可以折中考虑使用dllexport的方式。
以下总结是以前在Linux平台：(Eclipse+CDT)

如何新建库项目: 先新建一空项目，再修改设置：项目Property页--->C/C++ Buildings--->Settings--->Build Artifact--->Artifact Type中选择库类型。

静态库使用：
* 设置include头文件目录:.I./../XxLib/include;
* 设置Linker库文件目录: -L./../XxLib/lib;
* 设置Linker库文件: -lXxLib

动态库使用:
* 隐式调用同上面的静态库；
* 显式调用在Linker中: -ldl -lXxLib;
* 如果动态库不是放在/lib,/usr/lib目录下，需设置环境变量：LD_LIBRARY_PATH=./../XLib/lib

当同时存在该库的静态版本和共享版本时，链接器优先使用共享版本Xx.so，此时你可以使用-static链接选项指定链接静态版本Xx.a
动态库可以导出两个特殊的函数：_init和_fini，前者在动态库被加载后调用，后者在动态库被卸载前调用，我们可以使用这两个函数做些特别的工作。

需要注意的是：在定义这两个函数后编译时，需要使用-nostartfiles选项，否则编译器报重复定义错误。
应用程序与库混合调试：项目Property页--->C/C++ General--->Paths and Symbols--->References--->选择引用库。

* ldd用来查看程序所依赖的共享库，同时也方便我们判断共享库是否被找到; 
* nm命令查看obj文件(.so也是一个obj)中的标识(函数、变量)。

Q：在Linux的DLL中如何使用stdcall调用方式

# 库编译
## APR
### vs6
1. 下载解压apr,apr-util,apr-iconv到同一工作目录下;
2. 去掉各文件夹中的版本号, 如文件夹名apr-1.4.2改为apr(使用IDE时需要);
3. 使用IDE打开apr-util/aprutil.dsw(可能提示找不到apr-app.dsp,可替换aprutil.dsw中的"apr_app"为"aprapp");
4. 选择"匹创建", build-all(只要build win32的就可以了);
5. 设置好include, lib, bin目录。

如何使用静态编译编译出来的APR文件
1. 定义宏 APR_DECLARE_STATIC；
2. 连接类库 apr-1.lib kernel32.lib ws2_32.lib RPCRT4.LIB MSWSOCK.LIB ADVAPI32.LIB

## Boost
1. 下载[boost 1.47.0](http://sourceforge.net/projects/boost/files/boost/1.47.0/);  
2. 解压缩后，在boost根目录下运行bootstrap.bat批处理文件，得到bjam.exe;  
3. 进入VS2008的Command Prompt （方法：Tools -> Visual Studio 2008 Command Prompt），转到boost目录。（例如，我的boost目录:D:\boost_1_47_0）  
4. 输入“bjam --toolset=msvc-9.0 --build-type=complete stage”后，等待约30分钟，完成编译。编译成的lib文件，放在stage\lib下，形如“libboost_program_options-vc90-sgd-1_47.lib”.    
PS: 编译过程与1.46.1版本的编译比较类似，1.46.1版本编译，请[参考](http://blog.csdn.net/great3779/article/details/6454663)

## MagicK
项目[地址](http://sourceforge.net/projects/graphicsmagick/), 最好使用：HG版本控制可实时更新(解压.hg.rar文件后使用HG的update)  
### vs2008
1. 打开”GraphicsMagicK\VisualMagick\configure”下的项目文件编译生成向导程序；
2. 通过向导程序选择生成哪种类型的动态库；
3. 执行完成后在” GraphicsMagicK\VisualMagick”目录下生成vs2009的项目文件；
4. 打开此文件编译即可在”GraphicsMagicK\VisualMagick \VisualMagick\lib目录下得到所需要的库文件。

## Log4Cpp
### vs6
1. 下载log4cpp并解压;
2. 打开\log4cpp-0.3.4b\msvc6\msvc6.dsw;
3. 编译log4cpp工程Release版;
4. 将编译后的log4cpp.lib复制到VC的Lib目录中;
5. 将头文件的目录log4cpp-0.3.4b\include\log4cpp\复制到VC的Include目录(或者添加log4cpp-0.3.4b\include到VC的Include环境变量);
6. 目标工程包含库log4cpp.lib ws2_32.lib（要选择库连接方式相同的库）。

### vs2003
1. 报错error PRJ0019: 工具从"正在执行自定义生成步骤"，详细可以查看其BuildLog.htm 将转换后的vcproj打开，需要手工修改，原来的倒数几行的如下内容，使用了错误的可执行文件路径和错误的双引号，请注意，有的人说需要把NTEventLogCategories.mc这个文件删掉，我在我的机子上验证了下，那个文件不能删，并且需要把下面的内容替换替换为后面的内容：
   ```
      <FileConfiguration Name="Debug|Win32">
        <Tool Name="VCCustomBuildTool" CommandLine="if not exist $(OutDir) md $(OutDir)
        &quot;$(DevEnvDir)..\Tools\Bin\mc.exe&quot; -h $(OutDir) -r $(OutDir) $(ProjectDir)..\$(InputName).mc
        &quot;$(DevEnvDir)..\..\vc7\Bin\RC.exe&quot; -r -fo $(OutDir)\$(InputName).res $(OutDir)\$(InputName).rc
        &quot;$(DevEnvDir)..\..\VC7\Bin\link.exe&quot; /MACHINE:IX86 -dll -noentry -out:$(OutDir)\NTEventLogAppender.dll $(OutDir)\$(InputName).res"
        Outputs="$(OutDir)\NTEventLogAppender.dll"/>
      </FileConfiguration>
      <FileConfiguration Name="Release|Win32">
        <Tool Name="VCCustomBuildTool" CommandLine="if not exist $(OutDir) md $(OutDir)
        &quot;$(DevEnvDir)..\Tools\Bin\mc.exe&quot; -h $(OutDir) -r $(OutDir) $(ProjectDir)..\$(InputName).mc
        &quot;$(DevEnvDir)..\..\vc7\Bin\RC.exe&quot; -r -fo $(OutDir)\$(InputName).res $(OutDir)\$(InputName).rc
        &quot;$(DevEnvDir)..\..\VC7\Bin\link.exe&quot; /MACHINE:IX86 -dll -noentry -out:$(OutDir)\NTEventLogAppender.dll $(OutDir)\$(InputName).res"
        Outputs="$(OutDir)\NTEventLogAppender.dll"/>
      </FileConfiguration>
  ```
  替换为：
  ```
	<FileConfiguration Name="Debug|Win32">
	  <Tool Name="VCCustomBuildTool" CommandLine="if not exist &quot;$(OutDir)&quot; md &quot;$(OutDir)&quot;&quot;$(DevEnvDir)..\..\VC98\Bin\mc.exe&quot; -h &quot;$(OutDir)&quot; -r &quot;$(OutDir)&quot; &quot;$(ProjectDir)&quot;..\&quot;$(InputName)&quot;.mc&quot;$(DevEnvDir)Bin\RC.exe&quot; -r -fo &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.res &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.rc&quot;$(DevEnvDir)..\..\VC98\Bin\link.exe&quot; /MACHINE:IX86 -dll -noentry -out:&quot;$(OutDir)&quot;\NTEventLogAppender.dll &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.res"                               Outputs="$(OutDir)\NTEventLogAppender.dll"/>             
	</FileConfiguration>  
	<FileConfiguration Name="Release|Win32">                     
	  <Tool Name="VCCustomBuildTool" CommandLine="if not exist &quot;$(OutDir)&quot; md &quot;$(OutDir)&quot;&quot;$(DevEnvDir)..\..\VC98\Bin\mc.exe&quot; -h &quot;$(OutDir)&quot; -r &quot;$(OutDir)&quot; &quot;$(ProjectDir)&quot;..\&quot;$(InputName)&quot;.mc&quot;$(DevEnvDir)Bin\RC.exe&quot; -r -fo &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.res &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.rc&quot;$(DevEnvDir)..\..\VC98\Bin\link.exe&quot; /MACHINE:IX86 -dll -noentry -out:&quot;$(OutDir)&quot;\NTEventLogAppender.dll &quot;$(OutDir)&quot;\&quot;$(InputName)&quot;.res"                               Outputs="$(OutDir)\NTEventLogAppender.dll"/>               
	</FileConfiguration>
  ```
  
2. log4cppDLL项目编译时会报8个连接错误，提示符号std::_Tree找不到  
  将include\log4cpp\FactoryParams.hh文件中的`const_iterator find(const std::string& t) const`; 
  修改为const_iterator find(const std::string& t) const { return storage_.find(t); }后重新编译
3. log4cppDLL项目编译时会报1个连接错误，提示符号log4cpp::localtime找不到  
  将src\localtime.cpp文件添加到项目中重新编译
4. log4cppDLL项目编译时找不到localtime_s找不到符号
  因为VS版本低，所以头文件time.h里面没有这个函数，打开src里面的文件Localtime.cpp，修改`localtime_s(t, time)`为
  `::tm* tmp = ::localtime(time); memcpy(t, tmp, sizeof(::tm))`;编译则通过；  
5. 编译testPattern工程时，提示找不到localtime符号
  log4cpp项目编译时，把src\localtime.cpp文件添加到项目的Source Files中；把src\localtime.hh文件添加到项目的Header Files中重新编译；  

## OpenSSL
### vs6
1. 下载[OpenSSL](http://www.openssl.org/source/openssl-0.9.8l.tar.gz)
2. 安装Perl;
3. 设置VC6环境：运行VCVARS32.BAT, 进入openssl源码目录。
   以下为参照该目录下的文件INSTALL.W32的执行过程： 
    ```
    运行configure：  
    perl Configure VC-WIN32 no-asm
    创建Makefile文件：  
    ms\do_ms

    编译动态库：（注意编译问题中的8库函数版本问题）  
    nmake -f ms\ntdll.mak  
    编译静态库：  
    nmake -f ms\nt.mak  
    测试动态库：  
    nmake -f ms\ntdll.mak test  
    测试静态库：  
    nmake -f ms\nt.mak test  
    安装动态库：  
    nmake -f ms\ntdll.mak install  
    安装静态库：  
    nmake -f ms\nt.mak install  
    清除上次动态库的编译，以便重新编译：  
    nmake -f ms\ntdll.mak clean  
    清除上次静态库的编译，以便重新编译：  
    nmake -f ms\nt.mak clean  
    ```

编译错误
1. mc相关错误：VC编译环境未设置，运行VCVARS32.BAT，重启机器；
2. ml相关错误：可能masm32未安装或者未设置PATH参数；（使用no-asm方式）
3. link相关错误：可能masm32未安装或者未设置PATH参数；
4. in6_addr错误:遇到缺少结构定义in6_addr，把下列代码拷到apps\s_cb.c文件中#include后面即可
  ```cpp  
	struct in6_addr   
	{  
	union  
	    {  
			u_char  Byte[16];  
			u_short Word[8];  
		} u;  
	};  
  ```
5.	ActivePerl环境未设置：修改Path参数，重启机器；
6.	动态库、静态库要分开编译，否则会出错；
7.	编译出错之后，如果要重新开始编译，请把openssl目录全部删除，再重新解压缩新文件到目录，确保编译环境正确；
8.	库函数版本问题
调整OpenSSL的静态函数库使用的库函数版本即可，调整过程如下：
编辑文件 ms\nt.mak，将该文件第19行
```
CFLAG= /MD /Ox /O2 /Ob2 /W3 /WX /Gs0 /GF /Gy /nologo -DOPENSSL_SYSNAME_WIN32 -DWIN32_LEAN_AND_MEAN -DL_ENDIAN -DDSO_WIN32 -D_CRT_SECURE_NO_DEPRECATE -D_CRT_NONSTDC_NO_DEPRECATE /Fdout32 -DOPENSSL_NO_CAMELLIA -DOPENSSL_NO_SEED -DOPENSSL_NO_RC5 -DOPENSSL_NO_MDC2 -DOPENSSL_NO_TLSEXT -DOPENSSL_NO_KRB5 -DOPENSSL_NO_DYNAMIC_ENGINE
```
中的"/MD"修改为"/MT"。然后重新编译安装OpenSSL即可。

### vs2008
1. 安装Perl （ActivePerl 5.10.1）;
2. 下载并解压OpenSSL包 （openssl-0.9.8l）;
3. 打开VS2008 Express的命令行，并进入OpenSSL的根目录;
4. 执行命令：perl Configure VC-WIN32 （32位机器）;
5. 执行命令：ms\do_ms.bat;
6. 执行命令：nmake -f ms\ntdll.mak （生成DLL库，在out32dll子目录中）;
7. 执行命令：nmake -f ms\nt.mak （生成静态链接库，在out32子目录中）;  
注1：库文件有两个：libeay32.lib 和 ssleay32.lib  
注2：头文件在“inc32”子目录中

## ZXing
项目[地址](http://code.google.com/p/zxing/)

### vs6
* 在ZXING的CPP源码中需求更改文件名称的（以避免冲突，我觉得也可以采用设置编译器选项来避免此问题的，
例如通过更改相同文件生成中链接文件.obj在不同路径来避免冲突，但我没有偿试过）：
datamatrix中相同的文件（需要改名称我在文件名后加了个DM）：  
detector目录:  
	Detector.cpp/h      =====>> DetectorDM.cpp/h  
	BitMatrixParser.cpp/h         =====>> BitMatrixParserDM.cpp/h  
	DataBlock.cpp/h      =====>> DataBlockDM.cpp/h  
	DecodedBitStreamParser.cpp/h  =====>> DecodedBitStreamParserDM.cpp/h  
	Decoder.cpp/h      =====>> DecoderDM.cpp/h  
	./:
	Version.cpp/h      =====>> VersionDM.cpp/h  

* 由于ZXING的CPP源码是在LINUX下生成编译的，我们在编译时需要一些对应在WINDOWS平台下的头文件或库，
以下是我所用到的：
GnuWin32：主要用到了iconv.h头文件和libiconv.lib库，以及三个DLL，分别是libcharset1.dll，libiconv2.dll和libintl3.dll
nan.h：此文件别人传我的：-），我也没查到底从哪来的。。。

* 静态成员变量的实始化定义问题。在编译过程中可能会出现错误如下：
  error C2258: illegal pure syntax, must be '= 0'
  出现此错误的原因是由于在类中定义了静态成员变量而没有正确的进行
  初始化，例如以下代码（此代码在类定义头文件中）就会出现此问题：

  static const DecodeHintType BARCODEFORMAT_QR_CODE_HINT = 1 << BarcodeFormat_QR_CODE;
  我的解决方案是把此变量的初始化放到CPP文件中去，修改后如下：
  在头文件中定义：
  static const DecodeHintType BARCODEFORMAT_QR_CODE_HINT;
  在CPP文件中初始化：
  const DecodeHintType DecodeHints::BARCODEFORMAT_QR_CODE_HINT = 1 << BarcodeFormat_QR_CODE;

* 重载函数的显式指定类型的问题。在编译过程中可能会现错误如下：
  error C2593: 'operator =' is ambiguous
  这一般是由于有多个重载函数时，在使用时没有显式的指定所要使
  用的函数。例如，以下是出现此错误的代码：
  row = new BitArray(width_);
  修改后的代码：
  row = (Ref<BitArray>)new BitArray(width_);

* 定义枚举类型的常量问题。在编译过程中可能会出现错误如下：
  error C2057: expected constant expression
  这是由于在定义数组或枚举以及一些需要指定数值的变量时，
  其数值在预编译阶段还不能确定值的大小所引起的。例如，以
  下是出现此错误的代码：
   enum {MAX_AVG_VARIANCE = (unsigned int) (PATTERN_MATCH_RESULT_SCALE_FACTOR * 0.25f)};
   在此语句中PATTERN_MATCH_RESULT_SCALE_FACTOR的定义是：static const int PATTERN_MATCH_RESULT_SCALE_FACTOR;
   它在预编译阶段的值还没确定（因为初始化静态变量的代码放到CPP中了），因此出现此错误。
  我的临时解决办法是，在定义此枚举变量前定义一个变量，此变量值确定，从而避免此错误。
  例如：
    
  ```cpp
  #ifndef MAC_MAX_AVG_VARIANCE  
  const int MY_INTEGER_MATH_SHIFT = 8;  
  const int MY_PATTERN_MATCH_RESULT_SCALE_FACTOR = 1 << MY_INTEGER_MATH_SHIFT;  
  #endif  
  enum {MAX_AVG_VARIANCE = (unsigned int) (MY_PATTERN_MATCH_RESULT_SCALE_FACTOR * 0.25f)};
  ```
  
  类似的还有如下语句：
  int patternLength = patternLen;
  int counters[patternLength];//此句会出错，因为patternLength并不确定
  我修改为通过动态创建来避免：
  int *counters = new int[patternLength];
  注意这里用完*counters要记得删除，以免造成内存泄漏。

* 要运行ZXING源码的CPP包中提供的样例（在CPP的magick文件夹下），还需要安装ImageMagick库， 我安装的是其WINDOWS下的版本ImageMagick-6.6.8-10

经过以上折腾后我的ZXING工程算是编译通过了，然后也能读取一些QR码了，其它的功能还有待挖掘。

# 参考
* [阅读原文](http://www.cppblog.com/ietj/archive/2010/09/16/126777.html)
