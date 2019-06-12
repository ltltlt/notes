# Practical Vim

* C-a, C-x add or substract current value by 1(can use number prefix)
* ! use external command to filter
* R into replace mode, r temporary into replace mode(only last for one character)
* insert mode C-k char1 char2 二合字母<<: «, 14: ¼
* insert mode C-r(use register) = expression
* insert mode C-o into normal mode execute a command and back to insert mode

## open file

### path variable

This variable is used to :find file, if you want to do recursive search, you need to add **, vim will expand it. Note that is tab expansion is supported.

Find abc.java in ~/jvm recursively:

```vim
:set path+=~/jvm/**
:find abc.java
```

## netrw

this vim plugin can open network files(example: :e https://www.google.com/)

## 实际行和屏幕行

j, k 在实际行间移动
gj, gk 在屏幕行间移动
