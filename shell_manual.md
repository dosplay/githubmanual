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

## 3. 流程控制

shell脚本的流程控制语句不可为空，如

```php
<?php
if (isset($_GET["q"])) {
  search(q);
}
else{
  //do nothing
}
```

在sh/bash中是不能这么写的，如果else分支没有执行语句，则不要写else。

### 3.1 if-else

**if**

语句语法格式：

```sh
if condition
then
  command1
  ...
  commandend
fi
```

写成一行(用";"隔开)：

```sh
if [ $(ps -ef | grep -c "ssh") -gt 1]; then echo "true"; fi
```

**if else**

if else 语法格式：
```sh
if condition
then
  command1
  ...
  commandend
else
  commandelse
fi
```

**if else-if else**

if else-if else语法格式：

```sh
if condition1
then
  command1
elif condition2
then
  command2
else
  commandend
fi
```

### 3.2 for 循环

for循环的一般格式为：

```sh
for var in item1 item2 ... itemN
do
  command1
  command2
  ...
  commandn
done
```
写成一行：
```sh
for var in item1 item2 ... itemN; do command1; command2; ... commandN; done;
```

### 3.3 while 循环

while循环用于不断执行一系列命令，命令通常为测试条件， 其格式为：

```sh
while condition
do
  commandNdone
done
```

while循环可以用于读取键盘信息：

```sh
echo "按下<CTRL-D>退出"
echo -n "输入:"
while read FILM
do
  echo "你输入的是$FILM"
done
```

### 3.4 until 循环

util循环执行一系列命令直到条件为真时停止。until循环与while循环在处理方式上刚好相反。

一般while循环优于until循环，但在某些时候（也只是极少数情况下），until循环更有用

until语法格式：
```sh
until condition
do
  command
done
```

条件可为任意测试条件，测试发生在循环末尾，因此**循环至少执行一次**。

### 3.5 无限循环

无限循环语法格式：

```sh
while :
do
    command
done
```
或者
```sh
while true
do
    command
done
```
或者
```sh
for (( ; ; ))
```

### 3.6 case语句

Shell case语句为多选择语句。可以用case语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。case语句格式如下：

```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

case工作方式如上所示。取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

下面的脚本提示输入1到4，与每一种模式进行匹配：

```sh
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

输入不同的内容，会有不同的结果，例如：
> 输入 1 到 4 之间的数字:
> 你输入的数字为:
> 3
> 你选择了 3

### 3.7 跳出循环

#### **break命令**

break命令允许跳出所有循环（终止执行后面的所有循环）。
下面的例子中，脚本进入死循环直至用户输入数字大于5。要跳出这个循环，返回到shell提示符下，需要使用break命令。

```sh
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 结束"
            break
        ;;
    esac
done
```

执行以上代码，输出结果为：

> 输入 1 到 5 之间的数字:3
> 你输入的数字为 3!
> 输入 1 到 5 之间的数字:7
> 你输入的数字不是 1 到 5 之间的! 结束

#### **continue**

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。
对上面的例子进行修改：

```sh
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "结束"
        ;;
    esac
done
```
运行代码发现，当输入大于5的数字时，该例中的循环不会结束，语句 echo "Game is over!" 永远不会被执行。

#### **esac**

case的语法和C family语言差别很大，它需要一个esac（就是case反过来）作为结束标记，每个case分支用右圆括号，用两个分号表示break。

## 4. 函数

### 4.1 函数定义
shell中函数的定义格式如下（\[\]表示可选）：

```sh
[ function ] funname [()]
{
  action;
  [return int;]
}
```
说明：

- 1. 可以代function fun（）定义，也可以直接定义fun()， 不带任何参数
- 2. 参数返回，可以显式的加return返回，如果不加，将以最后一条命令的结果作为返回值，return后跟数值n(0-255)
- 3. 函数返回值在调用该函数后通过 $? 来获得。

下面的例子定义了一个函数并进行调用：

```sh
#!/bin/bash
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

下面的例子是带return语句的函数：

```sh
#!/bin/sh
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

输出类似下面：
>这个函数会对输入的两个数字进行相加运算...
>输入第一个数字:
>1
>输入第二个数字:
>2
>两个数字分别为 1 和 2 !
>输入的两个数字之和为 3 !

**注意**：**所有函数在使用前必须定义**。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。

### 4.2 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...
带参数的函数示例：
```sh
#!/bin/bash

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```
输出结果：
>第一个参数为 1 !
>第二个参数为 2 !
>第十个参数为 10 !
>第十个参数为 34 !
>第十一个参数为 73 !
>参数总数有 11 个!
>作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
**注意**，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

另外，还有几个特殊字符用来处理参数：

参数|说明
:----:|:----:
\$\#	|传递到脚本的参数个数
\$\*	|以一个单字符串显示所有向脚本传递的参数
\$\$	|脚本运行的当前进程ID号
\$\!	|后台运行的最后一个进程的ID号
\$\@	|与$*相同，但是使用时加引号，并在引号中返回每个参数。
\$\-	|显示Shell使用的当前选项，与set命令功能相同。
\$\?	|显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

## 5. 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推... \$0 为执行的文件名。

另外，还有几个特殊字符用来处理参数：
参数处理|说明
----|----
\$\#	| 传递到脚本的参数个数
\$*	| 以一个单字符串显示所有向脚本传递的参数。如"\$\\\*"用「"」括起来的情况、以"\$1 \$2 … \$n"的形式输出所有参数。
\$\$	| 脚本运行的当前进程ID号
\$\!	| 后台运行的最后一个进程的ID号
\$\@	|与\$\*相同，但是使用时加引号，并在引号中返回每个参数。如"\$@"用「"」括起来的情况、以"\$1" "\$2" … "\$n" 的形式输出所有参数。
\$\-	|显示Shell使用的当前选项，与set命令功能相同。
\$\?	|显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

**\$\* 与 \$\@ 区别**
- 相同点：都是引用所有参数。
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

## 6. Shell 输入/输出重定向
大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回​​到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。
重定向命令列表如下：
命令|说明
---|---
command > file	|将输出重定向到 file。
command < file	|将输入重定向到 file。
command >> file	|将输出以追加的方式重定向到 file。
n > file	|将文件描述符为 n 的文件重定向到 file。
n >> file	|将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m	|将输出文件 m 和 n 合并。
n <& m	|将输入文件 m 和 n 合并。
<< tag	|将开始标记 tag 和结束标记 tag 之间的内容作为输入。
需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

### 6.1 输出重定向

重定向一般通过在命令间插入特定的符号来实现。特别的，这些符号的语法如下所示:
```sh
command1 > file1
```

上面这个命令执行command1然后将输出的内容存入file1。

注意任何file1内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符。

**实例**

执行下面的 who 命令，它将命令的完整的输出重定向在用户文件中(users):
```sh
$ who > users
```

执行后，并没有在终端输出信息，这是因为输出已被从默认的标准输出设备（终端）重定向到指定的文件。输出重定向会覆盖文件内容，如果不希望文件内容被覆盖，可以使用 \>\> 追加到文件末尾。

### 6.2 输入重定向

和输出重定向一样，Unix 命令也可以从文件获取输入，语法为：
```sh
command1 < file1
```
这样，本来需要从键盘获取输入的命令会转移到文件读取内容。
注意：输出重定向是大于号(>)，输入重定向是小于号(<)。
实例
接着以上实例，我们需要统计 users 文件的行数,执行以下命令：
> $ wc -l users
>       2 users

也可以将输入重定向到 users 文件：

> $  wc -l < users
>       2

注意：上面两个例子的结果不同：第一个例子，会输出文件名；第二个不会，因为它仅仅知道从标准输入读取内容。

> command1 < infile > outfile

同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

### 6.3 重定向深入讲解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，`command > file` 将 stdout 重定向到 file，`command < file` 将stdin 重定向到 file。
如果希望 stderr 重定向到 file，可以这样写：
> $ command 2 > file

如果希望 stderr 追加到 file 文件末尾，可以这样写：

> $ command 2 >> file

2 表示标准错误文件(stderr)。
如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

>$ command > file 2>&1

或者

>$ command >> file 2>&1

如果希望对 stdin 和 stdout 都重定向，可以这样写：
>$ command < file1 >file2

command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。

### 6.4 Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。
它的基本的形式如下：
```sh
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

**注意**：

结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。

开始的delimiter前后的空格会被忽略掉。

实例

在命令行中通过 wc -l 命令计算 Here Document 的行数：
>$ wc -l << EOF
>    hellow
>    world
>EOF
>2
>$

### 6.5 /dev/null 文件
如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：
```sh
$ command > /dev/null
```

/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 stdout 和 stderr，可以这样写：
```sh
$ command > /dev/null 2>&1
```

***注意：0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR***。
