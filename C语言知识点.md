                                            **单片机开发重要知识点（C语言&数电）**

<mark>1.基本数据类型大小（以32位系统为例）:</mark>

       数据类型               占用字节                说明

字符型    char                     1                unsinged char 相同

短整型    short                    2                unsinged short 相同

整型        int                        4                unsinged int 相同

浮点型    float                     4                unsinged float 相同（精度6位）

双精度浮点型 double         8                unsinged double 相同（精度15位）

长整型    long                      8                unsinged long 相同   

long int                                8

long long                             8

long double                        16                精度19位有效位

<mark>2.浮点数的表达方式：</mark>

（1）十进制小数形式。由数字和小数点组成，必须有小数点。例如（123.）（123.0）（.123）。  

（2）指数形式。如123e3。字母e（或E）之前必须有数字，e后面的指数必须为整数。  

（3）规范化的指数形式里面，小数点前面有且只有一位非零的数字。如1.2345e8

<mark>##科学计数法表示浮点数##</mark>

使⽤字⺟ e 来分隔⼩数部分和指数部分（ e 后⾯如果是加号 + ，加号可以省略）

eg：

double x = 123.456e+3 ;          // 123.456 x 10^3
// 等同于
double x = 123.456e3 ;
另外，科学计数法的⼩数部分如果是 0.x 或 x.0 的形式，那么 0 可以省略。
0.3E6    // 等同于     . 3E6
3.0E6    // 等同于      3.E6

<mark>3.变量未初始化的结果</mark>

C 语言中变量的默认值取决于其类型和作用域。全局变量和静态变量的默认值为 **0**，字符型变量的默认值为 \0，指针变量的默认值为 NULL，而局部变量没有默认值，其初始值是未定义的（包含垃圾值）。

<mark>4.左右值：</mark>
左值（lvalue）：指向内存位置的表达式被称为左值（lvalue）表达式。左值可以出现在赋值号的左边或右边。
右值（rvalue）：术语右值（rvalue）指的是存储在内存中某些地址的数值。右值是不能对其进行赋值的表达式，也就是说，右值可以出现在赋值号的右边，但不能出现在赋值号的左边。

<mark>5.常量（是固定值，定义后值就不能进行修改）</mark>

定义常量的方式：

（1）使用 **#define** 预处理器： #define 可以在程序中定义一个常量，它在编译时会被替换为其对应的值。

（2）使用 **const** 关键字：const 关键字用于声明一个只读变量，即该变量的值不能在程序运行时修改。

<mark>6.存储类定义</mark>

存储类定义 C 程序中变量/函数的存储位置、生命周期和作用域。

（1）auto: 是所有局部变量默认的存储类，在函数开始被创建，在函数结束被销毁。

（2）register:用于定义存储在寄存器中而不是RAM中的局部变量（访问速度快）。

（3）static：指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。

注意：修饰全局变量时，限制变量作用域在它声明的文件内，修饰局部变量可以在函数之间保持局部变量的值（只只初始化一次）。

（4）extern:用于定义在其他文件中声明的全局变量或函数。

<mark>7.运算符</mark>

算术运算符、关系运算符、逻辑运算符、位运算符、赋值运算符、杂项运算符。

<mark>8.函数的参数分为：</mark>

（1）实参：当函数被调用时，您向参数传递一个值，这个值被称为实际参数

（2）形参：如果函数要使用参数，则必须声明接受参数值的变量。这些变量称为函数的形式参数。

<mark>9.向函数传参方式</mark>

传值调用：该方法把参数的实际值复制给函数的形式参数。在这种情况下，修改函数内的形式参数不会影响实际参数。

eg:

int a = 100; int b = 200;

max(a, b);

引用调用：通过指针传递方式，形参为指向实参地址的指针，当对形参的指向操作时，就相当于对实参本身进行的操作。

eg:

int a = 100; int b = 200;

max(&a, &b);

<mark>10.指针相关</mark>

<mark>（1）函数指针：</mark>是一个指针，指针指向的是一个函数；通过把函数指针作为参数传给函数，这叫回调函数。

声明一个函数指针：void (*func) (void);
int hanshu ( <mark>void (*func) (void)</mark> )
{

}

<mark>（2）指针函数：</mark>是一个函数，这个函数返回值是指针。
int *func()
{
 static int *p;

 return p;//返回指针
}
注意：C语言不支持在调用函数时返回局部变量的地址，除非定义局部变量为 static 变量

<mark>（3）指针数组：</mark>prt是一个数组，由n个指针的元素组成；每个元素都是一个指向int值的指针。
int val=10;

int *prt[n];

prt[1]=&val;

<mark>（4）数组指针：</mark>是一个指针，指向数组的指针。
int (* p)[n] = { };//指向了含n个int类型的数组

eg：
int temp[3]={1,2,3};
int (*p)[5]=&temp;//&temp 指的是这整个数组的首地址
for(int i = 0 i < 3;i++)
{
 print("%d\n",*(*p+i));
}

说明：p所指的是数组temp的地址，
*p+i 所指的是它在数组中的位置，
*(*p + i)就表示相应位置上的值了。

<mark>eg：利用函数指针调用函数：扩张性强</mark>
#include <stdio.h>

typedef struct //定义一个结构体
{
    int  (*func)(int,int);//定义函数指针
}yun;

//定义函数
int max(int a,int b)
{
     printf("add:a+b\n");
     printf("add=%d+%d\n",a,b);
     int c=a+b;
     return c;
}

int sub(int x,int y)
{
    printf("Sub:a-b\n");
    printf("Sub=%d-%d\n",x,y);
    if (x>y)
    {
         int z=x-y;
         return z;
    }
}
//定义结构体的数组里面存放函数名（即函数的地址）
const yun arr[] =
{
    max,sub//要调用的函数名（可扩展）
};

int main()
{
    for (char i = 0; i < sizeof(arr)/sizeof(yun); i++)//遍历结构体数组的函数
    {
       int c=arr[i].func(5,3);
       printf("Valu = %d\n",c);

    }

   return 0;
}



<mark>（5）指针常量:</mark>顾名思义就是一个常量，是指针修饰的。

格式：int * const p ;//指针的指向不能被修改，值可以改。

eg：

int a，b； 

int * const p=&a //指针常量 

//那么分为一下两种操作 

*p=9;//操作成功 

p=&b;//操作错误

<mark>（6）常量指针：</mark>如果在定义指针变量的时候，数据类型前用const修饰，被定义的指针变量就是指向常量的指针变量，指向常量的指针变量称为常量指针，格式如下：

const int *p = &a; //常量指针，指针的指向可以改变，值不能改。

eg：

int a，b； const int *p=&a //常量指针 

//那么分为一下两种操作 

*p=9;//操作错误 

p=&b;//操作成功

<mark>（7）指向常量的指针常量：</mark>

const int * const b = &a;//指向和值都不能修改。

<mark>11.C语言内存分区</mark>

<img src="file:///C:/Users/LSL/AppData/Roaming/marktext/images/2023-08-06-17-52-58-image.png" title="" alt="" data-align="center">

<mark>12.重要标识及关键字：</mark>

continue 结束本次循环，开始下一次
default  其他分支
enum     枚举
goto 无条件跳转
register 声明寄存器变量
unsigned 无符号类型变量
signed   声明有符号类型变量
typedef    用以给数据类型取别名
union   声明共用体类型
volatile 意思是“易变的”，应该解释为“直接存取原始内存地址”比较合适 
告诉编译器不要优化它每次存储或读取需要从内存地址中读。

<mark>13.输入/输出</mark>
c把所有的设备都当做文件，所以设备（比如显示器）被处理的方式与文件相同。
C 语言中的 I/O (输入/输出) 通常使用 int printf(const char *format, ...)发送格式化输出到标准输出（屏幕） 和 scanf(const char *format, ...)用于从标准输入（键盘）读取并格式化 两个函数。（只要遇到一个空格，scanf() 就会停止读取）。

stdio.h 是一个头文件 (标准输入输出头文件) 

eg：
int testInteger;
scanf("%d",&testInteger);
printf("Number = %d", testInteger);

<mark>int getchar()</mark>从标准输入获取字符，与<mark>putchar(int c)</mark>把字符输出到标准输出上。也是定义在stdio.h之中，只能处理字符，而非字符串（字符实际上是以整数存储）

eg：
int ch;
ch = getchar();//向ch输入一个字符,相当于scanf("%c",&ch);

int ch = 'a';
putchar(ch);//与printf("%c",ch);作用相同

/*字符以是整数储存*/
putchar(ch+1);//输出结果为 b 

<mark>gets() & puts() 函数</mark>
char *gets(char *s) 函数从 stdin 读取一行到 s 所指向的缓冲区，直到一个终止符或 EOF。

int puts(const char *s) 函数把字符串 s 和一个尾随的换行符写入到 stdout。

<mark>14.文件读写：</mark>
使用下面函数来创建新文件或打开一个已有的文件，filename是字符串用来命名文件，mode是访问模式。
FILE *fopen( const char *filename, const char *mode );

访问模式：
r:打开已有文本文件，允许读取文件
w:打开文件，允许写入，如文件不存在则会创建新文件（从头开始写）；如果文件存在，会截断为0长度重新写入
a：打开文件在已有文件内容中追加模式写，文件不存在会创建。
r+：打开文件，允许读写
w+：打开文件允许读写，如文件存在截断为0，不存在重新创建。
a+：打开文件允许读写，不存在则创建新文件；读取从文件开头读，写入是追加模式

如果处理的是二进制文件使用下面访问模式取代上面的模式：
"rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"

int fclose( FILE *fp );
关闭文件，关闭成功返回0，失败返回EOF，这函数会清空缓冲区数据，关闭文件会释放所有用于改文件所有内存。

写入文件：
int fputc( int c, FILE *fp );//把字符写入到文件中，成功返回字符，失败返EOF
int fputs( const char *s, FILE *fp );//把字符串写到文件中,成功返非负，失败返EOF
int fprintf(FILE *fp,const char *format, ...) 把一个字符串写入到文件中

读取文件：
int fgetc( FILE * fp );//从文件读取单个字符
char *fgets( char *buf, int n, FILE *fp );//从文件读取字符串存在buf中；遇到一个换行符 '\n' 或文件的末尾 EOF，则只会返回读取到的字符
int fscanf(FILE *fp, const char *format, ...) 函数来从文件中读取字符串，但是在遇到第一个空格和换行符时，它会停止读取。

二进制输入/输出函数： 
size_t fwrite(const void *ptr, size_t size_of_elements,  size_t number_of_elements, FILE *a_file);

size_t fread(void *ptr, size_t size_of_elements,  size_t number_of_elements, FILE *a_file);

<mark>15.可变参数：</mark>

函数的参数是可变的，而不是预定义数量的参数，头文件：stdarg.h 
int func_name(int arg1, ...);//

具体步骤如下：
1.定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数。
2.在函数定义中创建一个 va_list 类型变量，该类型是在 stdarg.h 头文件中定义的。
va_list valist;

3.使用 int 参数和 va_start() 宏来初始化 va_list 变量为一个参数列表。宏 va_start() 是在 stdarg.h 头文件中定义的。
va_start(valist, num); /* 为 num 个参数初始化 valist */

4.使用 va_arg() 宏和 va_list 变量来访问参数列表中的每个项。
 for()
 {
  sum = va_arg(valist, int);
 }

5.使用宏 va_end() 来清理赋予 va_list 变量的内存。 
va_end(valist); /* 清理为 valist 保留的内存 */

<mark>16.vsnprintf函数的使用</mark>
int vsnprintf (char * sbuf, size_t n, const char * format, va_list arg );

参数sbuf：用于缓存格式化字符串结果的字符数组

参数n：限定最多打印到缓冲区sbuf的字符的个数为n-1个，因为vsnprintf还要在结果的末尾追加\0。如果格式化字符串长度大于n-1，则多出的部分被丢弃。如果格式化字符串长度小于等于n-1，则可以格式化的字符串完整打印到缓冲区sbuf。一般这里传递的值就是sbuf缓冲区的长度。

参数format：格式化限定字符串

参数arg：可变长度参数列表

返回：成功打印到sbuf中的字符的个数，不包括末尾追加的\0。如果格式化解析失败，则返回负数。

<mark>17.预处理器：</mark>

不是编译器的组成，是编译过程中的一个步骤。
#define    定义宏
#include    包含一个源代码文件
#undef    取消已定义的宏
#ifdef    如果宏已经定义，则返回真
#ifndef    如果宏没有定义，则返回真
#if    如果给定条件为真，则编译下面代码
#else    #if 的替代方案
#elif    如果前面的 #if 给定条件不为真，当前条件为真，则编译下面代码
#endif    结束一个 #if……#else 条件编译块
#error    当遇到标准错误时，输出错误消息
#pragma    使用标准化方法，向编译器发布特殊的命令到编译器中

预定义宏：ANSI C 定义了许多宏，可以使用
__DATE__    当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。
__TIME__    当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。
__FILE__    这会包含当前文件名，一个字符串常量。
__LINE__    这会包含当前行号，一个十进制常量。
__STDC__    当编译器以 ANSI 标准编译时，则定义为 1。

预处理器运算符：
（1）宏延续运算符（\）
一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下，则使用宏延续运算符
eg：
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")

（2）字符串常量化运算符（#）
在宏定义中，当需要把一个宏的参数转换为字符串常量时，则使用字符串常量化运算符    
eg：
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")

调用： message_for(Carole, Debra);
输出打印：Carole and Debra: We love you!

（3）标记粘贴运算符（##）
宏定义内的标记粘贴运算符（##）会合并两个参数。它允许在宏定义中两个独立的标记被合并为一个标记
#define tokenpaster(n) printf ("token" #n " = %d", token##n)
调用：   int token34 = 40;
         tokenpaster(34);
打印输出：token34 = 40

（4）defined() 运算符
预处理器 defined 运算符是用在常量表达式中的，用来确定一个标识符是否已经使用 #define 定义过。如果指定的标识符已定义，则值为真（非零）。如果指定的标识符未定义，则值为假（零）。
eg：
#if !defined (MESSAGE)
   #define MESSAGE "You wish!"
#endif

<mark>18.C错误处理：</mark>
在发生错误，利用错误代码errno，标识在函数调用期间发生了错误，可以在头文件errno.h之中找到各种错误代码。
perror() 函数显示您传给它的字符串，后跟一个冒号、一个空格和当前 errno 值的文本表示形式。
strerror() 函数，返回一个指针，指针指向当前 errno 值的文本表示形式。
eg：c菜鸟查找

<mark>19.递归：</mark>

递归指的是在函数的定义中使用函数自身的方法。
eg：求阶乘案列
#include <stdio.h>

double factorial(unsigned int i)
{
   if(i <= 1)
   {
      return 1;
   }
   return i * factorial(i - 1);
}
int  main()
{
    int i = 4;
    printf("%d 的阶乘为 %f\n", i, factorial(i));
    return 0;
}

<mark>20.内存管理：</mark>
C提供了几个内存分配函数，在头文件<stdlib.h>
(1)//在内存动态分配了num个长为size的连续空间，并每个字节都初始化为0
void *calloc(int num, int size);

(2)//该函数释放address所指向的内存块，释放的是动态内存空间
void free(void *address);

(3)//在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化，它们的值是未知的。
void *malloc(int num);

(4)//该函数重新分配内存，把内存扩展到 newsize
void *realloc(void *address, int newsize);

memcpy() 函数：用于从源内存区域复制数据到目标内存区域。它接受三个参数，即目标内存区域的指针、源内存区域的指针和要复制的数据大小（以字节为单位）。

memmove() 函数：类似于 memcpy() 函数，但它可以处理重叠的内存区域。它接受三个参数，即目标内存区域的指针、源内存区域的指针和要复制的数据大小（以字节为单位）。

**数字电路知识补充：**

<mark>1.大小端模式：</mark>（描述在通信协议字节顺序和计算机存储方式）

高字节  对应  高地址   --------------> 小端模式

高字节  对应  低地址   --------------> 大端模式

<mark>2.BCD码：</mark>用4位二进制数来表示1位十进制中的0~9这10个数码，是一种二进制的数字编码形式，用二进制编码的十进制代码。
0----0000
1----0001
.........
9----1001
10---0001 0000
12---0001 0010

<mark>3.原码、反码、补码：</mark>

正数的原码反码补码都一样
负数：最高位是符号位1，反码是除符号位取反，补码是除符号位反码加1

**git相关知识补充：**

<mark>1.git基本概念</mark>

工作区：就是电脑上能看到的目录
暂存区：也叫索引，是在.git目录下的index文件中
版本库：工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。

<mark>2.常用命令：</mark>

git add . : 暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。

git commit -m "提交信息" ：将暂存区目录树写到版本库中，master分支会做相应的更新。

git reset HEAD： 暂存区的目录树会被重写，被master分支指向的目录树所替换，但是工作区不受影响。 

git rm --cached <file> ：直接从暂存区删除文件，工作区则不做出改变

git checkout . 或者 git checkout -- <file> 命令：会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。

git checkout HEAD . 或者 git checkout HEAD <file> 命令：
会用HEAD指向的master分支中的全部或者部分文件替换暂存区和以及工作区中的文件。因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

查看配置信息：
git config --list

git用户信息配置：
git config --global user.name "runoob"
git config --global user.email test@runoob.com

查看历史修改记录：git log
简洁版查看 ：git log --oneline
查看历史中何时出现了分支、合并：git log --graph
逆向查看历史记录：git log --reverse

<mark>3.常用开发git命令及步骤：</mark>
1.克隆代码  git clone
2.修改代码后，查看状态：git status
3.把修改的文件添加到暂存区：git add 文件名
4.提交暂存区内容到本地版本库里：git commit -m "提交信息"
5.下拉远程库与本地同步：git pull origin 分支名
6.推送远程仓库：git push origin 分支名

<mark>githab 笔记：</mark>

方法一：

直接克隆云平台文件下来本地，然后添加文件到仓库，直接上传
命令1：git clone <服务器地址如：https://github.com/Lsl-Githu/testfile.git>

命令2：git add .               （添加所有文件）
命令3：git commit -m "提交信息" （提交信息）
命令4：git push -u origin master  （把本地仓库push 到github上）

方法二：
在本地创建仓库，再关联服务器仓库，再上传
命令1：git init   (初始化仓库)
命令2：git status （查看仓库状态）
命令3：git add .   （添加所有文件到仓库）
命令4：git commit -m "提交信息"  （把项目提交到仓库）
命令5：ssh-keygen -t rsa -C "youremail@example.com" （创建ssh key注意ssh-keygen之间没有空格）
第六步：把.push文件中秘钥添加到github上。
命令7：git remote add origin https://github.com/guyibang/TEST2.git  （关联github仓库）
命令8：git push -u origin master     (把本地仓库内容推送到远程仓库：由于远程仓库低空的需要加-u)

如果你创建远程仓库的时候初始化了仓库，就会有文件：README
所以需要添加命令：git pull --rebase origin master来合并，在往上推
