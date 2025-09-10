# vim notes

## profile 简介
vim 功能强大，本文档记录了一些常用的命令和技巧

## contents 目录
- [可视块模式快速多行注释](#可视块模式快速多行注释)
- [批量删除行前固定数量字符](#批量删除固定列数量字符)
- [忘记 sudo 强制保存编辑内容](#忘记-sudo-强制保存编辑内容)

## notes 笔记

#### 可视块模式快速多行注释  
> scenarios: 当语言不支持多行注释符或多行注释符不够方便时  

1. 光标移动至要注释的首行   
2. `Ctrl+v` 进入 *Visual Block Mode* . 注意 lowercase，Ctrl+V或Ctrl+Shift+v被终端拦截作为Paste 
3. `j`/`k` 方向键选择要注释的所有行  
4. `Shift+I` 进入行首插入模式, 注意 uppercase   
5. 输入注释符（#, //, ", --, etc.）  
6. 按 `Esc` 后等待所有选中行都被自动注释  

---
  
#### 批量删除固定列数量字符  
> scenarios: 取消多行内容的单行注释符、删除错误复制的行号等  

*方法1 Visual Block Mode* 
1. 将光标移至欲删除块的首行的要删除位置的第一个字符处  
2. `Ctrl+v` 进入可视块模式.  
3. `h`/`j`/`k`/`l` 选择所有要处理的行和列  
4. 按 `d` 选中要删除的块  

*方法2 正则匹配与替换*
1. `:5,10s/^.\{3\}//` 删除 5-10 行的前 3 个字符  
2. `:%s/^.\{5\}//` 删除所有行的前 5 个字符  

*方法3 normal 命令*
1. `:2,6normal 3x` 删除 2-6 行前 3 个字符  
 
---

#### 忘记 sudo 强制保存编辑内容  
> scenarios: 忘记以 sudo 身份执行 vim 命令无法保存编辑内容  

1. `:w !sudo tee %` 将缓冲区内容通过管道传递给外部 shell 命令 `sudo tee %`, % 代表当前文件  
2. 忽略缓冲区警告，`:q!` 退出 或 `:e!` 重新加载文件  

---

#### 字符集编码转换（ UTF-8 和 GBK ）
> scenarios: 切换文本字符集编码
1. 查看当前编码
`:set fileencoding?` 若没有正确识别源编码，使用++enc 参数指定打开时的编码

2. 将 GBK/GB2312/GB18030 转换为 UTF-8
`:set fileencoding=utf-8`
`:w`

3. 将 UTF-8 转换为 GBK/GB2312/GB18030
`:set fileencoding=gb18030` 转换为 GB18030
`:set fileencoding=cp936` 转换为 GBK ，或者用 gbk
`:set fileencoding=gb2312` 转换为 GB2312

4. 以指定编码格式打开文件
```bash
vim file.txt -c "e ++enc=gbk"
vim file.txt -c "set fileencoding=gbk"
```
`:e ++enc=gbk` 在Vim中打开文件后以指定的编码重新加载文件
`:e ++enc=utf-8` 以 UTF-8 编码重新加载文件 

5. 批量转换多个文件
```bash
sudo vim *.txt # 打开所有需要转换的文件
```
:argdo set fileencoding=gbk | update
:argdo set fileencoding=utf-8 | update


#### LF 与 CRLF 转换
1. `:set ff?` 或 `:set fileformat?` 查看行为格式，unix, dos, mac (旧版本mac) 
2. `:e ++ff=unix` or `:e ++ff=dos` 以指定格式重新加载文件
3. 批量转换多个文件
```bash
vim *.txt
```
`argdo set ff=unix | update`
`argdo set ff=dos | update`
4. 处理混合不同格式的行尾
`:set ff=dos` 先转换为 DOS 格式
`:set ff=unix` 再转换为 Unix 格式
`:w` write to file
`:%s/\r//g` 也可用替换命令移除所有的CR字符

#### 以16进制方式显示文件
```bash
vim -b file.txt
```
`:%!xxd` 以 16 进制格式显示，左侧为 16 进制字节，右侧对应 ASCII
`:%!xxd -r` 返回正常文本模式

#### 复制粘贴操作 
省略


#### 多文件编程与多窗口
1. 打开多个文件
启动时打开多文件 vim file1.cpp file1.h
在Vim命令行中打开：`:e path/to/file.cpp` (:e 是 :edit 的缩写)
2. 窗口分割
`:sp` or `:spilt filename` 水平分割，若省略文件名则分割当前文件
`:vsp` or `:vsplit filename` 垂直分割
```bash
# 直接从命令行启动时分割
vim -o file1.cpp file1.h # 水平分割
vim -O file1.cpp file2.h # 垂直分割
```
3. 窗口间切换
按下`Ctrl+w`松手进入^W模式 (状态栏显示 ^W)
`h`/`j`/`k`/`l` 进行窗口切换, `w`循环切换到下一窗口

4. 关闭窗口
`:q` 同关闭文件
`Ctrl-w+c` 只关闭窗口但不退出Vim

5. 标签页
`:tabnew filename` 打开新标签页
gt(下一个), gT(上一个) 切换标签页








