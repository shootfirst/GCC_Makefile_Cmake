# GCC_Makefile_Cmake
GCC Makefile Cmake 学习

参考：https://blog.csdn.net/Linux_LR/article/details/78597334      https://seisman.github.io/how-to-write-makefile/introduction.html

## GCC

  + GCC是什么：
  
    GNU编译器套装（英语：GNU Compiler Collection，缩写为GCC），指一套编程语言编译器，以GPL及LGPL许可证所发行的自由软件，也是GNU计划的关键部分，也是GNU工具链的主要组成部分之一。         GCC（特别是其中的C语言编译器）也常被认为是跨平台编译器的事实标准。1985年由理查德·马修·斯托曼开始发展，现在由自由软件基金会负责维护工作。
    
    原名为GNU C语言编译器（GNU C Compiler），因为它原本只能处理C语言。GCC在发布后很快地得到扩展，变得可处理C++。之后也变得可处理Fortran、Pascal、Objective-C、Java、Ada，Go与其他     语言。

    许多操作系统，包括许多类Unix系统，如Linux及BSD家族都采用GCC作为标准编译器。

    GCC原本用C开发，后来因为LLVM、Clang的崛起，它更快地将开发语言转换为C++
    
  + GCC组件
    
  + gcc与g++区别：
    
    对于 C 语言或者 C++ 程序，可以通过执行 gcc 或者 g++ 指令来调用 GCC 编译器。
    
    我们更习惯使用 gcc 指令编译 C 语言程序，用 g++ 指令编译 C++ 代码。需要强调的一点是，这并不是 gcc 和 g++ 的区别，gcc 指令也可以用来编译 C++ 程序，同样 g++ 指令也可以用于编译 C     语言程序。
    
    实际上，只要是 GCC 支持编译的程序代码，都可以使用 gcc 命令完成编译。可以这样理解，gcc 是 GCC 编译器的通用编译指令，因为根据程序文件的后缀名，gcc 指令可以自行判断出当前程序所用编     程语言的类别
    
    当然，gcc 指令也为用户提供了“手动指定代表编译方式”的接口，即使用 -x 选项。例如，gcc -xc xxx 表示以编译 C 语言代码的方式编译 xxx 文件；而 gcc -xc++ xxx 则表示以编译 C++ 代码的     方式编译 xxx 文件。
    
    但如果使用 g++ 指令，则无论目标文件的后缀名是什么，该指令都一律按照编译 C++ 代码的方式编译该文件。也就是说，对于 .c 文件来说，gcc 指令以 C 语言代码对待，而 g++ 指令会以 C++ 代     码对待。但对于 .cpp 文件来说，gcc 和 g++ 都会以 C++ 代码的方式编译。
    
    如果想使用 gcc 指令来编译执行 C++ 程序，需要在使用 gcc 指令时，手动为其添加 -lstdc++ -shared-libgcc 选项，表示 gcc 在编译 C++ 程序时可以链接必要的 C++ 标准库。
    
    
  + 如何编写GCC命令

    gcc -x file (-o target)
    
    gcc 常用选项：
    
    - E 预处理

    - S 编译不汇编

    - c 汇编
    
    - o 指定生成的文件名

    - std = 编译标准 


## makefile


    + 示例：
    
    all: main.o foo.o
	    $(info good)
	      @gcc -o simple $(baga1).o $(baga2).o
    baga1 = main
    baga2 = foo
	
    $(baga1).o: $(baga1).c
	      @gcc -o $(baga1).o -c $(baga1).c

    $(baga2).o: $(baga2).c
	      @gcc -o $(baga2).o -c $(baga2).c

     lean:
	      @rm simple $(baga1).o $(baga2).o
        
      
    + 规则：
      target ... : prerequisites ...
        command（前面有tab键）
      
      反斜杠（ \ ）是换行符的意思。这样比较便于makefile的阅读。
      
      
      
    + make执行过程：
      在默认的方式下，也就是我们只输入 make 命令。那么，

      - make会在当前目录下找名字叫“Makefile”或“makefile”的文件。

      - 如果找到，它会找文件中的第一个目标文件（target），并把这个文件作为最终的目标文件。

      - 如果第一个目标文件不存在，或是所依赖的后面的 .o 文件的文件修改时间要比第一目标文件文件新，那么，它就会执行后面所定义的命令来生成第一目标文件。

      - 如果所依赖的 .o 文件也不存在，那么make会在当前文件中找目标为 .o 文件的依赖性，如果找到则再根据那一个规则生成 .o 文件。（这有点像一个堆栈的过程）

      - 当然，你的C文件和H文件是存在的啦，于是make会生成 .o 文件，然后再用 .o 文件生成make的终极任务，也就是执行文件第一个目标文件了。
      
        
    + 构建目标：GNU Make版本3.81引入了一个名为.DEFAULT_GOAL的特殊变量，可用于告知如果在命令行中未指定目标，应该构建哪个目标（或目标）。
    
    
    + 自动推导：只要make看到一个 .o 文件，它就会自动的把 .c 文件加在依赖关系中，如果make找到一个 whatever.o ，那么 whatever.c 就会是 whatever.o 的依赖文件。并且 cc -c  
      whatever.c 也会被推导出来：如下：
      
      
      baga1 = main
      baga2 = foo
      simple: main.o foo.o
	    $(info good)
	      @gcc -o simple $(baga1).o $(baga2).o
      
      clean:
	      @rm simple $(baga1).o $(baga2).o
    
      和示例完全相同。
      
    + makefile内容：
      Makefile里主要包含了五个东西：显式规则、隐晦规则、变量定义、文件指示和注释，文件指示。其包括了三个部分，一个是在一个Makefile中引用另一个Makefile，就像C语言中的include一样         include <filename>；另一个是指根据某些情况指定Makefile中的有效部分，就像C语言中的预编译#if一样；还有就是定义一个多行的命令。
      
    + makefile环境变量：
      
      执行时，如 make BOARD = qemu ，则在makefile文件中可以使用变量BOARD，它的值是qemu
      
    + makefile工作方式：
      
      - 读入所有的Makefile。

      - 读入被include的其它Makefile。

      - 初始化文件中的变量。

      - 推导隐晦规则，并分析所有规则。

      - 为所有的目标文件创建依赖关系链。

      - 根据依赖关系，决定哪些目标要重新生成。

      - 执行生成命令。
      
      
      
    + 伪目标：
      伪目标是这样一个目标：它不代表一个真正的文件名，在执行make时可以指定这个目标来执行其所在规则定义的命令，有时也可以将一个伪目标称为一个标签。使用伪目标有两点要求： 
        - 避免在我们的Makefile中定义的只执行命令的目标和工作目录下的实际文件出现名字冲突。 
        - 提高执行make时的效率，特别是对一个大型的工程来说，编译的效率也许你同样关心。
        
       使用场合：
       
       - 规则中“rm”不是创建文件“clean”的命令，而是删除当前目录下的所有.o 文件和temp文件。当工作目录下不存在“clean”这个文件时，我们输入“make clean”就可以完成上述的删除工作。但是
         如果在当前工作目录下存在“clean”这个文件就不一样了，同样我们输入“make clean”，由于这个规则没有任何依赖文件，所以目标被认为是最新的而不去执行规则所定义的命令，因此“rm”将不
         会被执行。为解决这个问题，我们将目标“clean”声明为伪目标。
         
       - 在Makefile中，一个伪目标可以有自己的依赖。在一个目录下如果需要创建多个可执行程序，我们可以将所有程序的重建规则在一个Makefile中描述。因为Makefile中第一个目标是“终极目
         标”，约定的做法是使用过一个称为“all”的伪目标来作为终极目标，它的依赖文件就是那些需要创建的程序。

       
    + 命令书写：
      - 

      

    + 显示信息：
      - @echo 正在编译XXX模块...... 向屏幕输出信息
      - $(info text...)        显示信息
      - $(warning text...)     警告控制
      - $(error text...)       错误控制
      
      
    + 变量：
      =  会递归定义
      := 则不会
      ?= 没定义过则定义之
      
      定义：变量名=变量值
      引用：$(变量名) ${变量名)
      
      - 高级用法：
      	
	
      
      - 自动化变量：所谓自动化变量，就是这种变量会把模式中所定义的一系列的文件自动地挨个取出，直至所有的符合模式的文件都取完了。这种自动化变量只应出现在规则的命令中。
        
        + $@：用于表示一个规则中的目标。当我们的一个规则中有多个目标时， $@所指的是其中任何造成命令被运行的目标。
        
        + $^：则表示的是规则中的所有先择条件。
        
        + $<：表示的是规则中的第一个先决条件。
        
        和示例完全相同：
        
        
        all: main.o foo.o
	        @gcc -o $@ $^
        baga1 = main
        baga2 = foo
	
        $(baga1).o: $(baga1).c
	        @gcc -o $@ -c $^

        $(baga2).o: $(baga2).c
	        @gcc -o $@ -c $^

        .PHONY: clean
        clean:
	        @rm simple $(baga1).o $(baga2).o
          
          
          
        
      - 特殊变量：make程序定义好的变量
        
        CC = cc #c语言编译器的名称
        CPP = $(cc) -E #c文件预处理器的名称
        CFLAGS #C文件的编译选项
        CPPFLAGS #C文件预处理的编译选项
        CXXFLAGS #CPP文件的编译选项
        LDFLAGS #连接的动态库
         
        CURDIR := /home/zxy/... #当前路径
        MAKEFLAGS = p #make命令选项
        RM = rm -f
        VPATH #文件的搜索路径
         
        MAKE：指make命令名是什么。当我们需要在 Makefile 中调用另一个 Makefile 时需要用到这个变量， 采用这种方式，有利于写一个容易移植的 Makefile。
        MAKECMDGOALS： 指的是用户输入的目标。
        
        
        
        
    - 模式匹配
        baga1 = main
        baga2 = foo

        simple: main.o foo.o
	      $(info good)
	        @gcc -o $@ $^

	
        %.o:%.c
	        @gcc -o $@ -c $^

        .PHONY: clean
        clean:
	        @rm simple $(baga1).o $(baga2).o
		
		
		
    - 函数
    	
	+ 语法：$(<function> <arguments>)
	
	+ addprefix 函数：是用来在给字符串中的每个子串前加上一个前缀，其形式是：$(addprefix prefix, names...)
	
	+ filter 函数：用于从一个字符串中，根据模式得到满足模式的字符串，其形式是：$(filter pattern..., text)
	
	+ patsubst 函数：是用来进行字符串替换的，其形式是：$(patsubst pattern, replacement, text)
	
	+ wildcard 函数：通过它可以得到我们所需的文件，这个函数如果我们在 Windows 或是Linux 命令行中的“*”。 其形式是：$(wildcard pattern)
    
    
    - 条件判断
      不加else也可
    	<conditional-directive>
	<text-if-true>
	else
	<text-if-false>
	endif
    	
	+ ifeq ifneq ifdef ifndef
	
	  
    

      
  
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

  
  
  
  
  
  
