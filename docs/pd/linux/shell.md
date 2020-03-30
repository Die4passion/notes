### 有用的技巧

- `!$` 上一个命令的最后一个字符串
  
- `sudo !!` 以管理员身份执行上一个命令
  
- `pstree -p` 显示进程树
  
- `date -d@时间戳` 时间戳转时间
  
- `mtr xxxxx.com` 测试网络连接
  
- `echo &quot;ls -l&quot; | at midnight` 在某个时间运行某个命令
  
- `ps aux | sort -nk +4 | tail` 列出头十个最耗内存的进程
  
- `ascii` 查看ascii码表
  
- **`netstat –tlnp`** 列出本机进程监听的端口号
  
- **`lsof –i`** 实时查看本机网络服务的活动状态。
  
- **`python -m SimpleHTTPServer`** 一句话实现http服务
  
- 使用 nohup 或 disown 如果你要让某个进程运行在后台。
  
- `awk &#39;{ x += $3 } END { print x }&#39;` 第三列之和
  
- `ls -v | cat -n | while read n f; do ((n--)); mv -i -- $f banner_0$n.jpg; done` 批量重命名文件
  
- `a=1;for i in *.jpg; do new=$(printf &quot;banner_0%d.jpg&quot; &quot;$a&quot;); mv -i -- $i $new; ((a++)); done` 也是批量重命名文件
  
- `iconv` 文本文件转码
  
- `-z &quot;${var}&quot; ` 空字符串 `-n &quot;${var}&quot;` 非空字符串
  
- ```bash
  # 彻底删除 xxxxx  rpm系统
  [root@test ~]# rpm -qa|grep xxxxx|xargs rpm -ev --allmatches --nodeps
  [root@test ~]# whereis xxxxx|xargs rm -frv
  ```
  
- `pandoc`
  

```bash

  # pandoc 用来转换文件 支持格式

  Input formats:  docbook, haddock, html, json, latex, markdown, markdown_github,

                  markdown_mmd, markdown_phpextra, markdown_strict, mediawiki,
                  native, opml, rst, textile

  Output formats: asciidoc, beamer, context, docbook, docx, dzslides, epub, epub3,

                  fb2, html, html5, json, latex, man, markdown, markdown_github,
                  markdown_mmd, markdown_phpextra, markdown_strict, mediawiki,
                  native, odt, opendocument, opml, org, pdf*, plain, revealjs,
                  rst, rtf, s5, slideous, slidy, texinfo, textile
                  [*for pdf output, use latex or beamer and -o FILENAME.pdf]
  # 将rst文件转为md 并输出到新建文件

  pandoc -f 111.rst -o 111.md   

  # 将网页转为markdown

  pandoc -s -r html http://www.gnu.org/software/make/ -o example12.text

  # 将 LaTeX 文档转换为 mathMathML.html：

  pandoc math.tex -s --mathml  -o mathMathML.html

  # 置顶代码高亮主题

  pandoc code.md -s --highlight-style monochrome -o example18c.html
```

### bash快捷键

#### 光标移动命令

| 按键  | 行动  |
| --- | --- |
| Ctrl - a | 移动光标到行首。 |
| Ctrl - e | 移动光标到行尾。 |
| Ctrl - f | 光标前移一个字符；和右箭头作用一样。 |
| Ctrl - b | 光标后移一个字符；和左箭头作用一样。 |
| Alt - f | 光标前移一个字。 |
| Alt - b | 光标后移一个字。 |
| Ctrl - l | 清空屏幕，移动光标到左上角。clear 命令完成同样的工作。 |
| Ctrl - w | 删除最后一个单词 |
| Ctrl - U | 删除一行 |
| Ctrl - - | 撤销  |

#### 文本编辑命令

| 按键  | 行动  |
| --- | --- |
| Ctrl-d | 删除光标位置的字符。 |
| Ctrl-t | 光标位置的字符和光标前面的字符互换位置。 |
| Alt-t | 光标位置的字和其前面的字互换位置。 |
| Alt-l | 把从光标位置到字尾的字符转换成小写字母。 |
| Alt-u | 把从光标位置到字尾的字符转换成大写字母。 |

#### 剪切和粘贴命令

| 按键  | 行动  |
| --- | --- |
| Ctrl-k | 剪切从光标位置到行尾的文本。 |
| Ctrl-u | 剪切从光标位置到行首的文本。 |
| Alt-d | 剪切从光标位置到词尾的文本。 |
| Alt-Backspace | 剪切从光标位置到词头的文本。如果光标在一个单词的开头，剪切前一个单词。 |
| Ctrl-y | 把剪切环中的文本粘贴到光标位置。 |

### 一些语法

```bash
# 将前面make的stderr(2)合并到stdout(1)   tee命令写入到build.log
$ make –f build_example.mk 2&gt;&1 | tee build.log

# tr -d(删除) -s(替换)
# 将windows文件换行转为linux格式
$ tr -d &#39;\r&#39; &lt; dosfile.txt &gt; unixfile.txt
# 也可用sed
$ sed -i &#39;s/\r//&#39; filename1 filename2 filename3 ...
$ cat dosfile.txt | sed &#39;s/^M//&#39; &gt; unixfile.txt
$ sed -i &#39;s/^M//&#39; filename1 filename2 filename3 ...

也可用tr
$ tr -d &#39;\r&#39; &lt; dos_file &gt; unix_file
```

#### tldr 查看命令参数神器

```bash
root:~# tldr -e    # 随机一个例子
```

!> rsync 文件同步

#### vim基本设置

```bash
root:~# vim ~/.vimrc

set number

syntax on

set hlsearch

set tabstop=4

set autoindent
```

#### bash shell

文件放在 `~/bin ` 目录下

```bash
file # 查看文件
stat # 文件或文件夹详细信息
```

#### 一些有用的

```
# 生成图标  github readme风格
https://shields.io

# 快速生成图表js
https://quickchart.io/

# ip api
curl -s https://api.ip.sb/ip    # string ip
curl -s https://api.ip.sb/jsonip    # 简单
curl -s https://api.ip.sb/geoip/{ip_address}     # 详细

```
