<center> shell scripts manual </center>
**第一行必须是"#!/bin/sh"**

- 这不是注释，是对shell的声明，说明你所在的是哪种类型的shell以及其所在路径

注释：一行的开头为#

## 定义变量 ##

- 定义单变量

```
p_name='echo'
```
注意:变量名和等号之间不能有空格，且首字符必须为字母(中间可以使用\_)

- 使用单变量

```
echo $p_name'^-^'  #输出echo^-^ 
echo $p_name^-^
cp $p_name.js   copy.js;
``` 
- 只读变量

使用readOnly命令可以将变量定义为只读变量

```
#!/bin/bash
myUrl=“http://examples.com"
readOnly myUrl
myUrl="http://test.com #提示错误
```
- 删除变量
```
#!/bin/bash
myUrl=“http://examples.com"
unset myUrl
echo $myUrl #没有输出
```
## 逻辑符号 ##

**\<command1\> && \<command2\>**

如果左边的command1执行成功才执行右边的command2

**\<command1\> \|\| \<command2\>**

如果左边的command1未执行成功才执行右边的command2
