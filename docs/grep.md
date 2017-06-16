# grep

grep [参数] <匹配符> <文件路径 文件路径>

## 参数列表

选项 | 含义 | 功能描述
--- | --- | ---
-A <显示行数> | after-context | 显示匹配行**前**指定行数
-B <显示行数> | before-context | 显示匹配行**后**指定行数
-C <显示行数> 或-<显示行数> | context | 显示匹配行**后和后**指定行数
-c | count | 显示匹配的行数
--color | - | 输出高亮匹配颜色
-E | extended-regexp | 开启扩展的正则查找
-h | no-filename | 抑制文件名的输出
-i | ignore case | 忽略大小写
-L | file-without-match | 输出不匹配的文件名
-l | file-with-match | 输出匹配的文件名
-n | number | 输出匹配行的同时在前面加上文件名及在文件名中的行数
-q | quiet / silent | 不显示任何信息, 一般用于条件测试
-s | no-messages | 不显示错误信息
-v | invert match | 不匹配匹配的

## 例子

> 注意: `seq`命令只是为了生成测试的字符串

### 查找文件内容

```shell
# 查找单文件
grep 'hello' a.log

# 查找枚举文件
grep 'a' a.log b.log

# 查找当前目录下文件
grep 'a' ./*

# 查找相对/绝对目录下文件
grep 'a' /path/1.log
grep 'a' ../../1.log

# 管道操作
seq 10 | grep '1'
```


### 显示匹配行上下文内容

```shell
# 显示匹配行+后一行内容
seq 30 | grep -A 1 '5'

# 显示匹配行+后五行内容
seq 30 | grep -A 5 '5'

# 显示匹配行+前一行内容
seq 30 | grep -B 1 '5'

# 显示匹配行+前五行内容
seq 30 | grep -B 5 '5'

# 显示匹配行+前一行+后一行内容
seq 30 | grep -C 1 '5'

# 显示匹配行+前五行+后五行内容
seq 30 | grep -C 5 '5'
```


### 显示匹配的行数

```shell
# 结果是1
seq 10 | grep -c '5'

# 结果是2
seq 20 | grep -c '5'
```

### 使用正则查找

```shell
# 查找包含 1,2,3
seq 10 | grep '[123]'

# 查找包含 1,2,3
seq 10 | grep -E '(1|2|3)'

# 查找以 1开头 3结束的
seq 100000 | grep -E '^13.*3$'

# 查找以5结束的行
seq 100 | grep -E '5$'

# 查找 1x0
seq 1000 | grep -E '^1[0-9]0$'

# 查找2位的数字
seq 1000 | grep -E '^\d{2}$'
```

### 几个常用匹配

```shell
# 忽略匹配的
seq 10 | grep -v 1

# 显示匹配的行号
seq 10 | grep -n 1

# 不输出文件名
grep -h 'a' file

# 输出不匹配的文件名, 一般用在目录查找时
grep -L 'a' *

# 只输出匹配的文件名, 不输出匹配内容, 和 `-L` 正好相反
grep -l 'a' *

# 忽略大写小查找
grep -i 'A' file
```

### 不输出任何内容, 用来测试是否通过

```shell
# -s 忽略错误, 测试时一般配合 -q 不输出内容让程序更健壮, 可判断输出用来断言
grep -sq 'a' file
echo $?
```

### 配合其他命令操作

```shell
# 递归查找当前目录下的所有 `*.conf` 文件, 并匹配出指定端口内容
find . -name '*.conf' -type f | xargs grep --color -E 'listen\s+(80|8085)\b'

# 列出当前 `npm root` 下的子目录以 fis3 开头的文件夹
ls `npm root` | grep '^fis3'
```