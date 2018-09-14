### 启动vim, 打开文件;

 ```
  vim filename // 一个文件
  vim file1 file2 file3  // 打开多个文件
  :open filename // vim窗口打开一个新文件
  :split filename // 新窗口打开文件
  :bn // 切换到下一个文件
  :bp // 切换到上一个文件
  
  打开远程文件
  :e ftp://192.168.10.76/abc.txt
   :e \qadrive\test\1.txt
 ```
 
 
 
 ### vim的模式
 
 ```
 正常模式（按Esc或Ctrl+[进入） 左下角显示文件名或为空 
 插入模式（按i键进入） 左下角显示–INSERT– 
 可视模式（不知道如何进入） 左下角显示–VISUAL–
 ```
 
 ### 插入命令
 ```
i 在当前位置生前插入 
I 在当前行首插入 
a 在当前位置后插入 
A 在当前行尾插入 
o 在当前行之后插入一行 
O 在当前行之前插入一行
 ```
 
 ### 退出命令
 
 ```
:w 保存文件但不退出vi 
:w file 将修改另外保存到file中，不退出vi 
:w! 强制保存，不推出vi 
:wq 保存文件并退出vi 
:wq! 强制保存文件，并退出vi 
:q 不保存文件，退出vi 
:q! 不保存文件，强制退出vi 
:e! 放弃所有修改，从上次保存文件开始再编辑命令历史
 ```
 ### 查找命令
 
 ```
/text　　查找text，按n健查找下一个，按N健查找前一个。 
?text　　查找text，反向查找，按n健查找下一个，按N健查找前一个

:set ignorecase　　忽略大小写的查找 
:set noignorecase　　不忽略大小写的查找

:set hlsearch　　高亮搜索结果，所有结果都高亮显示，而不是只显示一个匹配。 
:set nohlsearch　　关闭高亮搜索显示 
:nohlsearch　　关闭当前的高亮显示，如果再次搜索或者按下n或N键，则会再次高亮。 
:set incsearch　　逐步搜索模式，对当前键入的字符进行搜索而不必等待键入完成。 
:set wrapscan　　重新搜索，在搜索到文件头或尾时，返回继续搜索，默认开启。
 ```
 
 ### 替换命令
 ```
ra 将当前字符替换为a，当期字符即光标所在字符。 
s/old/new/ 用old替换new，替换当前行的第一个匹配 
s/old/new/g 用old替换new，替换当前行的所有匹配 
%s/old/new/ 用old替换new，替换所有行的第一个匹配 
%s/old/new/g 用old替换new，替换整个文件的所有匹配 
:10,20 s/^/ /g 在第10行知第20行每行前面加四个空格，用于缩进。 
ddp 交换光标所在行和其下紧邻的一行。
 ```