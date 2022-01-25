## Makefile

**主要内容**
- Makefile的结构
-
-


### 1. Makefile的结构：一个简单的makefile
```makefile
#(目标)  (源) 
hello:hello.c
    gcc -I{HOME}/incl -c hello.c        # 预编译
    gcc -o hello hello.o                # 链接
    rm -f hello.o                       # 删除中间文件
    mv hello ${HOME}/bin                # 移动目标
```

Makefile里包含：
- 变量定义：使用$取值
- 显式规则：如何生成目标文件
- 隐晦规则
- 文件指示：在Makefile中引用另一个Makefile等
- 注释：使用#号，只有行注释

### 2. 隐晦规则
隐晦规则帮助Makefile自动推导执行规则内容。
```makefile
INCL=-I${HONE}/incl

# 定义了如何将.cpp .c文件转变为.o
.SUFFIXES: .cpp .c
.cpp.o:
    g++ ${INCL} -c $<
.c.o:
    gcc ${INCL} -c $<

hello:hello.o
    g++ -o hello hello.o    # 无须再写显示规则生成.o
```

### 3. 预定义变量
- $* 不包含扩展名的目标文件名称
- $+ 所有依赖文件，以空格分开，以出现前后为序，可能包含重复文件
- $< 第一个依赖文件的名称
- $? 所有的依赖文件，以空格分开，修改日期比目标创建日期晚
- $@ 目标的完整名称
- $^ 所有依赖文件，以空格分开，不包含重复的依赖文件
- $% 若目标是归档成员，则该变量表示目标的归档成员名称