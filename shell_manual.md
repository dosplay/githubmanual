# <center> shell scripts manual </center>

## 0. 从零开始
**第一行必须是"#!/bin/sh"**

- 这不是注释，是对shell的声明，说明你所在的是哪种类型的shell以及其所在路径
- shell脚本没有多行注释，所以注释内容必须以#开头

## 1. 变量

### 1.1 单变量

- 定义单变量

```bash
p_name='echo'
```
注意:变量名和等号之间不能有空格，且首字符必须为字母(中间可以使用_)

- 使用单变量

```bash
echo $p_name'^_^'  #输出echo^_^
echo $p_name^_^
cp $p_name.js   copy.js;
```
- 只读变量

使用readOnly命令可以将变量定义为只读变量

```bash
#!/bin/bash
myUrl=“http://examples.com"
readOnly myUrl
myUrl="http://test.com #提示错误
```
- 删除变量
```bash
#!/bin/bash
myUrl=“http://examples.com"
unset myUrl
echo $myUrl #没有输出
```

### 1.2 shell字符串

字符串是shell编程中常用的数据类型，字符串可以使用单引号也可以使用双引号，也可以不适用引号

1. 单引号

```bash
str='this is string'
```

单引号字符串的限制：
  - 单引号中的任何字符都会原样输出，单引号字符串中的变量是无效的
  - 单引号字符串中不能出现单引号(对单引号使用转义符后也不行)

2. 双引号
```bash
your_name='^_^'
str="Hello, I know your are \"$your_name\"! \n"
```

双引号字符串的优点：
  - 双引号中可以有变量
  - 双引号字符串中可以出现转义字符

3. 拼接字符串
```bash
your_name="haha"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting $greeting_1
```

4. 获取字符串长度
```bash
string="abcd"
echo ${#string} #输出 4
```

5. 提取子字符串

以下实例从字符串第 2 个字符开始截取 4 个字符：

```bash
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

6. 查找子字符串
查找字符 "i 或 s" 的位置：
```bash
string="runoob is a great company"
echo `expr index "$string" is`  # 输出 8
```
注意： 以上脚本中 "\`" 是反引号，而不是单引号 "'"

### 1.3 shell数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小数组的下标是从0开始的，下标可以是整数或者算术表达式

1. 定义数组

在Shell中，用括号来表示数组，数组元素用"空格"符号分割开定义数组的一般形式为：
>数组名=(值1 值2 ... 值n)

例如：
```bash
array_name=(value0 value1 value2 value3)
```
或者
```bash
array_name=(
value0
value1
value2
value3
)
```
还可以单独定义数组的各个分量：
```bash
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```
下标不需要连续

2. 读取数组
读取数组元素值的一般格式是：
>${数组名[下标]}

例如：
```bash
valuen=${array_name[n]}
```
使用@符号可以获取数组中的所有元素，例如：
```bash
echo ${array_name[@]}
```

3. 获取数组的长度
获取数组长度的方法与获取字符串长度的方法相同，例如：
```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

### 1.4 变量类型

运行shell时，会同时存在三种变量：
1. 局部变量: 局部变量在脚本或命令中定义， 作用范围仅在当前shell实例
2. 环境变量: 所有的程序包括shell启动程序都能访问环境变量
3. shell变量: shell变量是由shell程序设定的特殊变量shell变量有一部分是环境变量，有一些是局部变量，这些变量保证shell的正常运行

## 2. 运算符

shell和其他编程语言一样，支持多种运算符，包括：
- 算数运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

两点注意：
- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样
- 完整的表达式要被\` \` 包含，注意这个字符不是常用的单引号，在 Esc 键下边

### 2.1 算术运算符

下表列出了常用的算术运算符，假定变量a为10，b为20

运算符|说明|举例
----|----|----
+|加法|\`expr \$a + \$b\`
-|减法|\`expr \$a - \$b\`
\*|加法|\`expr \$a * \$b\`
/|减法|\`expr \$a / \$b\`
%|取余|\`expr \$b % \$a\`
=|赋值|	a=\$b 将把变量 b 的值赋给
==|相等|用于比较两个数字，相同则返回 true
!=|不相等|用于比较两个数字，不相同则返回 true

注意：条件表达式要放在方括号之间，并且要有空格，例如: [\$a==\$b] 是错误的，必须写成 [ \$a == \$b ]

### 2.2 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字
下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

运算符	|说明	|举例
----|----|----
-eq	|检测两个数是否相等，相等返回 true|	[ \$a -eq \$b ] 返回 false
-ne	|检测两个数是否相等，不相等返回 true|	[ \$a -ne \$b ] 返回 true
-gt	|检测左边的数是否大于右边的，如果是，则返回 true|[ \$a \-gt \$b ] 返回 false
-lt	|检测左边的数是否小于右边的，如果是，则返回 true|	[ \$a \-lt \$b ] 返回 true
-ge	|检测左边的数是否大于等于右边的，如果是，则返回 true|	[ \$a \-ge \$b ] 返回 false
-le	|检测左边的数是否小于等于右边的，如果是，则返回 true|	[ \$a \-le \$b ] 返回 true

### 2.3 布尔运算符
下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：
运算符	|说明	|举例
----|----|----
!	|非运算，表达式为 true 则返回 false，否则返回 true|	[ ! false ] 返回 true
-o	|或运算，有一个表达式为 true 则返回 true|	[ $a -lt 20 -o $b -gt 100 ] 返回 true
-a	|与运算，两个表达式都为 true 才返回 true|	[ $a -lt 20 -a $b -gt 100 ] 返回 false

### 2.4 逻辑运算符
以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:
运算符	|说明	|举例
----|----|----
&amp;&amp;	|逻辑的 AND	| [ [ \$a -lt 100 &amp;&amp; \$b -gt 100 ] ] 返回 false
\|\|	|逻辑的 OR	| [ [ \$a -lt 100 \|\| $b -gt 100 ] ] 返回 true

### 2.5 字符串运算符
下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：
运算符	|说明	|举例
----|----|----
=	|检测两个字符串是否相等，相等返回 true|	[ \$a = \$b ] 返回 false
!=	|检测两个字符串是否相等，不相等返回 true|	[ \$a != \$b ] 返回 true
-z	|检测字符串长度是否为0，为0返回 true|	[ -z $a ] 返回 false
-n	|检测字符串长度是否为0，不为0返回 true|	[ -n $a ] 返回 true
str	|检测字符串是否为空，不为空返回 true|	[ $a ] 返回 true

### 2.6 文件测试运算符
文件测试运算符用于检测 Unix 文件的各种属性
属性检测描述如下：
运算符	|说明	|举例
----|----|----
-b file	|检测文件是否是块设备文件，如果是，则返回 true|	[ -b $file ] 返回 false
-c file	|检测文件是否是字符设备文件，如果是，则返回 true|	[ -c $file ] 返回 false
-d file	|检测文件是否是目录，如果是，则返回 true|	[ -d $file ] 返回 false
-f file	|检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true|	[ -f $file ] 返回 true
-g file	|检测文件是否设置了 SGID 位，如果是，则返回 true|	[ -g $file ] 返回 false
-k file	|检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true|	[ -k $file ] 返回 false
-p file	|检测文件是否是有名管道，如果是，则返回 true|	[ -p $file ] 返回 false
-u file	|检测文件是否设置了 SUID 位，如果是，则返回 true|	[ -u $file ] 返回 false
-r file	|检测文件是否可读，如果是，则返回 true|	[ -r $file ] 返回 true
-w file	|检测文件是否可写，如果是，则返回 true|	[ -w $file ] 返回 true
-x file	|检测文件是否可执行，如果是，则返回 true|	[ -x $file ] 返回 true
-s file	|检测文件是否为空（文件大小是否大于0），不为空返回 true|	[ -s $file ] 返回 true
-e file	|检测文件（包括目录）是否存在，如果是，则返回 true|	[ -e $file ] 返回 true

### 2.7 其他
**\<command1\> &amp;&amp; \<command2\>**

如果左边的command1执行成功才执行右边的command2

**\<command1\> \|\| \<command2\>**

如果左边的command1未执行成功才执行右边的command2

## 3. 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推... \$0 为执行的文件名。

另外，还有几个特殊字符用来处理参数：
参数处理|说明
----|----
\$#	| 传递到脚本的参数个数
\$*	|以一个单字符串显示所有向脚本传递的参数。如"\$\*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
\$$	| 脚本运行的当前进程ID号
\$!	| 后台运行的最后一个进程的ID号
\$@	|与\$\*相同，但是使用时加引号，并在引号中返回每个参数。如"\$@"用「"」括起来的情况、以"\$1" "\$2" … "\$n" 的形式输出所有参数。
\$-	|显示Shell使用的当前选项，与set命令功能相同。
\$?	|显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

**\$\* 与 \$\@ 区别**
- 相同点：都是引用所有参数。
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。
