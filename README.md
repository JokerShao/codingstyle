# joker's style guide

基于Google 开源项目风格指南——中文版：https://github.com/zh-google-styleguide/zh-google-styleguide

update:2019-03-31
当代码是某个算法的实现时，命名规范应优先符合算法公式中的定义，以便理解。
代码释出版本号，三位数字：`1.2.2`，其中大版本号看产品经理，中位软件更新升级添加功能，末位bug修复。



## C

与C++规范大致相同，只不过函数的命名遵循全部小写字母，下划线分词，如：

```C
int handle_event(int arg0, int arg1)
{
    return arg0 + arg1;
}
```



## C++

### 0 头文件包含

1. 头文件应该能够自给自足（self-contained,也就是可以作为第一个头文件被引入），以 .h 结尾。至于用来插入文本的文件，说到底它们并不是头文件，所以应以 .inc 结尾。不允许分离出 -inl.h 头文件的做法.
2. 用`ifndef def`，`endif`避免头文件重复包含
3. 若头文件中使用了其他定义，应将定义所在头文件包含进来；如果只用声明，则使用前向声明的方法，尽量不要出现交叉引用头文件。包含的话就包干净的纯外部的头，否则可能会导致包含顺序不确定，最终出现莫名其妙的编译错误。
4. 在`stdafx.h`中放置项目中使用较多的系统和第三方的依赖头，**经常改动的头文件不要放在里面**
5. 包含头文件时应加上文件相对路径，如`#include "base/logging.h"`，而不是`#include "logging.h"`
6. 对于`xxx.cpp`，头文件包含顺序应为：
```text
xxx.h
OS SDK.h
C标准库.h
C++标准库.h
其他第三方库.h
本项目内.h
```

#### 前置声明

前置声明优缺点，来自google开源项目指南。

1. 前置声明隐藏了依赖关系，头文件改动时，用户的代码会跳过必要的重新编译过程。
2. 前置声明可能会被库的后续更改所破坏。前置声明函数或模板有时会妨碍头文件开发者变动其 API.例如扩大形参类型，加个自带默认参数的模板形参等等。
3. 前置声明来自命名空间std:: 的 symbol 时，其行为未定义。
4. 很难判断什么时候该用前置声明，什么时候该用 #include 。极端情况下，用前置声明代替 includes 甚至都会暗暗地改变代码的含义。

使用前置声明时需要注意的地方：

1. 尽量避免前置声明那些定义在其他项目中的实体。
2. 函数：总是使用#include。
3. 类模板：优先使用#include。


### 1 内联函数

只有当函数只有10行甚至更少时才将其定义为内联函数。


### 2 函数参数

- 变量名一律小写，下划线分词。
- 函数参数的顺序为：输入参数在先，后跟输出参数。
- 当函数参数比较多时，应考虑用结构代替。
- 如果不能避免函数参数比较多，应在排版上考虑每个参数占用一行，参数名竖向对齐。


### 3 命名规则

#### 3.0 目录名

全部小写，使用下划线_进行分词，如`dir_name`。

#### 3.1 文件名

全部小写，如`filename.cpp`。

可接受的文件命名示例:

```bash
my_useful_class.cc
my-useful-class.cc
myusefulclass.cc
```

#### 3.2 类型名

首字母大写，使用驼峰命名法，不含下划线：

```C++
// 类、结构体和枚举
class UrlTable {
    ;
}

struct UrlTableProperties {
    ;
}

enum UrlTableErrors {
    ;
}
```

#### 3.3 成员函数

遵循驼峰命名法，首字母小写，后面大写分词，如：

```c++
class Test {

public:
    publicFunc(int* num_ptr);    // 公有成员函数

private:
    _privateFunc(int* num_ptr);  // 私有成员函数
};
```

#### 3.4 变量命名

**避免名字中出现数字编号：如`value1, value2`等，除非逻辑上的确需要编号。**

- 2.3.0 普通变量与结构体变量

    不管是静态的还是非静态的, 结构体数据成员都可以和普通变量一样, 不用像类那样接下划线，全部小写字母，下划线分词，如：

```C++
std::string table_name;

struct UrlTableProperties {
string name;
int num_entries;
static Pool<UrlTableProperties>* pool;
};
```

- 2.3.1 类数据成员

    不管是静态的还是非静态的, 类数据成员都可以和普通变量一样, 但要接下划线，变量名后加一个下划线_，如：

```C++
class TableInfo {
private:
    string table_name_;
    string tablename_;
    static Pool<TableInfo>* pool_;
};
```

- 2.3.2 指针变量

    用p打头，如：

```C++
unsigned char* p_gpu_memory;
```

#### 3.5 宏、枚举值

宏和枚举值由全大写字母组成，单词间通过下划线来分割，如：

```C++
#define ERROR_UNKNOWN 1
enum EnumType {
    OP_STOP = 0,
    OP_START
};
```



## Python

PEP是Python Enhancement Proposal的缩写，通常翻译为“Python增强提案”。
每个PEP都是一份为Python社区提供的指导Python往更好的方向发展的技术文档，其中的第8号增强提案（PEP 8）是针对Python语言编订的代码风格指南。
PEP8 命名规约：https://www.python.org/dev/peps/pep-0008/#prescriptive-naming-conventions。
本规约基于PEP8标准，部分编码规范细节参考Google开源项目风格指南。


### 0 排版格式

#### 0.0 缩进

用4个空格来缩进代码。

#### 0.1 空格

1. 使用空格来表示缩进而不要用制表符（Tab）。这一点对习惯了其他编程语言的人来说简直觉得不可理喻，因为绝大多数的程序员都会用Tab来表示缩进，但是要知道Python并没有像C/C++或Java那样的用花括号来构造一个代码块的语法，在Python中分支和循环结构都使用缩进来表示哪些代码属于同一个级别，鉴于此Python代码对缩进以及缩进宽度的依赖比其他很多语言都强得多。在不同的编辑器中，Tab的宽度可能是2、4或8个字符，甚至是其他更离谱的值，用Tab来表示缩进对Python代码来说可能是一场灾难。
2. 和语法相关的每一层缩进都用4个空格来表示。
3. 每行的字符数不要超过79个字符，如果表达式因太长而占据了多行，除了首行之外的其余各行都应该在正常的缩进宽度上再加上4个空格。
4. 二元运算符的左右两侧应该保留一个空格，而且只要一个空格就好。
5. 括号内不要有空格，如：
```Python
def spam(ham[1], {eggs: 2}, [])     # OK
def spam( ham[ 1 ], { eggs: 2 }, [ ] )     # Bad
```
6. 索引或切片的左括号前不应加空格，如：
```Python
dict['key'] = list[index]   # OK
dict ['key'] = list [index] # Bad
```
7. 当’=’用于指示关键字参数或默认参数值时，不要在其两侧使用空格，如：
```Python
Yes: def complex(real, imag=0.0): return magic(r=real, i=imag)
No:  def complex(real, imag = 0.0): return magic(r = real, i = imag)
```

#### 0.2 分号

不要在行尾加分号, 也不要用分号将两条命令放在同一行。

#### 0.3 括号

宁缺毋滥的使用括号，不要使用反斜杠链接行，Python会将圆括号，中括号和花括号中的行隐式的连接起来，可以利用这个特点。
如果需要，可以在表达式外围增加一对额外的圆括号，如：

```Python
foo_bar(self, width, height, color='black',
        design=None, x='foo',
        emphasis=None, highlight=0)
     if (width == 0 and height == 0 and
         color == 'red' and emphasis == 'strong'):
            pass
```

如果一个文本字符串在一行放不下，可以使用圆括号来实现隐式行连接：

```Python
x = ('This will build a very long long '
     'long long long long long long string')
```

#### 0.4 空行

（来自google开源项目规范）
1. 顶级定义之间空两行，比如类与类，类与函数，函数与函数之间。
2. 方法定义，类定义与第一个方法之间，都应该空一行。函数中的逻辑段落间加空行，把相关的代码紧凑写在一起，作为一个逻辑段落，段落间以空行分隔。
3. 在import不同种类的模块间加空行

```python
class Test(object):

    """Test class,提供通用的方法"""
    def __init__(self):
        """Test的构造器:"""
        pass

    def function1(self):
        pass
 
    def function2(self):
        pass


def function3():
    pass
```

#### 0.5 行长度

每行不超过80个字符，换行反斜杠

```python
with open('test.txt','w') as file_1, \
     open("test2.txt", 'w') as file_2:
    file_2.write(file_1.read())

# 字符串
query_str = ('my name'
             'is'
             ' %s') % "Tom"
print query_str

# 二元运算符
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

#### 0.6 序列元素尾部逗号

仅当 ], ), } 和末位元素不在同一行时，推荐使用序列元素尾部逗号. 当末位元素尾部有逗号时，元素后的逗号可以指示 YAPF 将序列格式化为每行一项.

```python
Yes:   golomb3 = [0, 1, 3]
Yes:   golomb4 = [
           0,
           1,
           4,
           6,
       ]
No:    golomb4 = [
           0,
           1,
           4,
           6
       ]
```


### 1 导入

仅对包和模块使用导入,而不单独导入函数或者类。`typing`模块例外。（来自google开源项目指南）

#### 1.0 局部导入和全局导入

尽量使用全局导入，那么什么时候使用函数本地导入呢？有以下三种情况:

1. 您不能更早地使用导入。这发生在例如在运行时通过配置或系统检查选择数据库或其他系统/功能的后端。
2. 否则你有循环导入。这是一种罕见的情况，也是一种代码味道，因此如果有必要，请考虑重构。
3. 通过推迟模块导入来减少启动时间。但这很少有用。

#### 1.1 避免通配符导入

应该避免通配符导入（from tt_module import *)，每个导入应该独占一行，按照从最通用到最不通用的顺序分组:

1. 标准库导入
2. 第三方库导入
3. 应用程序指定导入

```python
>>> import my_module
>>> my_module.external_func()
23
>>> my_module._internal_func()
42
```


### 2 命名约定

所谓”内部(Internal)”表示仅模块内可用, 或者, 在类内是保护或私有的.
将相关的类和顶级函数放在同一个模块里. 不像Java, 没必要限制一个类一个模块.

Python之父Guido推荐的规范

|Type|Public|Internal|
|:-:|:-:|:-:|
|Modules|lower_with_under|_lower_with_under|
|Packages|lower_with_under||
|Classes|CapWords|_CapWords|
|Exceptions|CapWords||
|Functions|lower_with_under()|_lower_with_under()|
|Global/Class Constants|GLOBAL_CONSTANT_NAME/CAPS_WITH_UNDER|_CAPS_WITH_UNDER|
|Global/Class Variables|global_var_name/lower_with_under|_lower_with_under|
|Instance Variables|lower_with_under|_lower_with_under (protected) or __lower_with_under (private)|
|Method Names|lower_with_under()|_lower_with_under() (protected) or __lower_with_under() (private)|
|Function/Method Parameters|lower_with_under|	 
|Local Variables|lower_with_under|

#### 2.0 包命名

小写字母，单词之间用_分割，如`package_name`

#### 2.1 模块命名

(PEP8)
对类名使用大写字母开头的单词(如CapWords, 即Pascal风格), 但是模块名应该用小写加下划线的方式(如lower_with_under.py). 尽管已经有很多现存的模块使用类似于CapWords.py这样的命名, 但现在已经不鼓励这样做, 因为如果模块名碰巧和类名一致, 这会让人困扰.
When an extension module written in C or C++ has an accompanying Python module that provides a higher level (e.g. more object oriented) interface, the C/C++ module has a leading underscore (e.g. _socket).

因为很多模块文件存与模块名称一致的类，模块采用小写，类采用首字母大写，这样就能区分开模块和类
小写字母，单词之间用_分割，如：`ad_stats.py`

#### 2.2 类命名

首字母大写，驼峰命名法(CamelCase)，不使用下划线连接单词，使用大小写分词，对于基类，可以加一个`Base`或者`Abstract`前缀，，私有类可用一个下划线开头如：

```Python
class _PrivateClass(object):
    pass
class TestClass(object):
    pass
class BaseCookie(object):
    pass
class AbstractGroup(object):
    pass
```

#### 2.3 函数命名

类的实例方法，应该把第一个参数命名为`self`以表示对象自身。
类的类方法，应该把第一个参数命名为`cls`以表示该类自身。

类内部函数命名，用单下划线(_)开头（该函数可被继承访问）
类内私有函数命名，用双下划线(__)开头（该函数不可被继承访问） 

```python
class Person(object):

    def public_func(object):
        pass

    def _protected_func(object):
        pass

    def __private_func(object)
        pass
```

#### 2.4 变量命名

大小写混用仅允许在已有代码中为了与现有风格保持一致（PEP8）；
双下划线开头，双下划线结尾，为系统内置变量。

```python
class TestClass(object):

    # 类的公有变量，小写字母，下划线分词，如：
    self.public_variable = True

    # 类的保护变量，单下划线开头，全部小写字母，下划线分词，如：
    # 单个下划线是一个Python命名约定，表示这个名称是供内部使用的。 它通常不由Python解释器强制执行，仅仅作为一种对程序员的提示。
    # 该变量可被继承访问，保护变量；
    self._protected_variable = False

    # 双下划线前缀会导致Python解释器重写属性名称，以避免子类中的命名冲突。
    # 该变量不可被继承访问，私有变量；
    # Python mangles these names with the class name: if class Foo has an attribute named __a, it cannot be accessed by Foo.__a. (An insistent user could still gain access by calling Foo._Foo__a.) Generally, double leading underscores should be used only to avoid name conflicts with attributes in classes designed to be subclassed.
    # Note: there is some controversy about the use of __names (see below).
    self.__private_variable = True


def func():
    # 普通变量
    this_is_a_variable = True


if __name__ == '__main__':

    # 全局变量，global开头，小写字母，下划线分词，如：
    global_var = 0
```

#### 2.5 常量命名

模块级常量全部大写，下划线分词，如`MAX_OVERFLOW`和`TOTAL`

```python
MAX_CLIENT = 100
MAX_CONNECTION = 1000
CONNECTION_TIMEOUT = 600
```


### 3 特殊变量

按照习惯，有时候单个独立下划线是用作一个名字，来表示某个变量是临时的或无关紧要的。
例如，在下面的循环中，我们不需要访问正在运行的索引，我们可以使用“_”来表示它只是一个临时值：

```python
for _ in range(32):
    print('Hello, World.')

# 你可以在一个解释器会话中访问先前计算的结果，或者，你是在动态构建多个对象并与它们交互，无需事先给这些对象分配名字：
>>> 20 + 3
23
>>> _
23
>>> print(_)
23

>>> list()
[]
>>> _.append(1)
>>> _.append(2)
>>> _.append(3)
>>> _
[1, 2, 3]
```


### 4 文档生成

Python有一种独一无二的的注释方式: 使用文档字符串. 文档字符串是包, 模块, 类或函数里的第一个语句. 这些字符串可以通过对象的 __doc__ 成员被自动提取, 并且被pydoc所用. (你可以在你的模块上运行pydoc试一把, 看看它长什么样). 我们对文档字符串的惯例是使用三重双引号”””( PEP-257 ). 一个文档字符串应该这样组织: 首先是一行以句号, 问号或惊叹号结尾的概述(或者该文档字符串单纯只有一行). 接着是一个空行. 接着是文档字符串剩下的部分, 它应该与文档字符串的第一行的第一个引号对齐. 下面有更多文档字符串的格式化规范.
每个文件应该包含一个许可样板. 根据项目使用的许可(例如, Apache 2.0, BSD, LGPL, GPL), 选择合适的样板. 其开头应是对模块内容和用法的描述.

```python
"""A one line summary of the module or program, terminated by a period.

Leave one blank line.  The rest of this docstring should contain an
overall description of the module or program.  Optionally, it may also
contain a brief description of exported classes and functions and/or usage
examples.

Typical usage example:

foo = ClassFoo()
bar = foo.FunctionBar()
"""
```


### 5 表达式和语句

在Python之禅（可以使用import this查看）中有这么一句名言：“There should be one-- and preferably only one --obvious way to do it.”，翻译成中文是“做一件事应该有而且最好只有一种确切的做法”，这句话传达的思想在PEP 8中也是无处不在的。

1. 采用内联形式的否定词，而不要把否定词放在整个表达式的前面。例如if a is not b就比if not a is b更容易让人理解。
2. 不要用检查长度的方式来判断字符串、列表等是否为None或者没有元素，应该用if not x这样的写法来检查它。
3. 就算if分支、for循环、except异常捕获等中只有一行代码，也不要将代码和if、for、except等写在一起，分开写才会让代码更清晰。
4. import语句总是放在文件开头的地方。
5. 引入模块的时候，from math import sqrt比import math更好。
6. 如果有多个import语句，应该将其分为三部分，从上到下分别是Python标准模块、第三方模块和自定义模块，每个部分内部应该按照模块名称的字母表顺序来排列。
7. 除非是用于实现行连接, 否则不要在返回语句或条件语句中使用括号. 不过在元组两边使用括号是可以的。

