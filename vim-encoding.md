# vim encoding variables

- [vim encoding variables](#vim-encoding-variables)
  - [encoding](#encoding)
  - [termencoding](#termencoding)
  - [fileencoding](#fileencoding)
  - [fileencodings](#fileencodings)
  - [References](#references)

## encoding

vim 内部使用的编码, 读文件时, 会把文件从fileencoding编码转为这种编码, 字节存在内存中.

encoding 是 Vim 内部使用的字符编码方式。当我们设置了 encoding 之后，Vim 内部所有的 buffer、寄存器、脚本中的字符串等，全都使用这个编码。Vim 在工作的时候，如果编码方式与它的内部编码不一致，它会先把编码转换成内部编码。如果工作用的编码中含有无法转换为内部编码的字符，在这些字符就会丢失。因此，在选择 Vim 的内部编码的时候，一定要使用一种表现能力足够强的编码，以免影响正常工作。
由于 encoding 选项涉及到 Vim 中所有字符的内部表示，因此只能在 Vim 启动的时候设置一次。

使用特定编码重新载入文件: e ++encoding=utf-8(在vim 7.2.411上貌似这个会出错, 用e ++enc=utf-8)

## termencoding

展示文件的编码, 缺省为encoding.

termencoding 是 Vim 用于屏幕显示的编码，在显示的时候，Vim 会把内部编码转换为屏幕编码，再用于输出。内部编码中含有无法转换为屏幕编码的字符时，该字符会变成问号，但不会影响对它的编辑操作。如果 termencoding 没有设置，则直接使用 encoding 不进行转换。

## fileencoding

可以理解为写文件时用的编码.

当 Vim 从磁盘上读取文件的时候，会对文件的编码进行探测。如果文件的编码方式和 Vim 的内部编码方式不同，Vim 就会对编码进行转换。转换完毕后，Vim 会将 fileencoding 选项设置为文件的编码。当 Vim 存盘的时候，如果 encoding 和 fileencoding 不一样，Vim 就会进行编码转换。因此，通过打开文件后设置 fileencoding，我们可以将文件由一种编码转换为另一种编码。但是，由前面的介绍可以看出，fileencoding 是在打开文件的时候，由 Vim 进行探测后自动设置的。因此，如果出现乱码，我们无法通过在打开文件后重新设置 fileencoding 来纠正乱码。

## fileencodings

检测文件编码时依次使用的编码列表.

编码的自动识别是通过设置 fileencodings 实现的，注意是复数形式。fileencodings 是一个用逗号分隔的列表，列表中的每一项是一种编码的名称。当我们打开文件的时候，VIM 按顺序使用 fileencodings 中的编码进行尝试解码，如果成功的话，就使用该编码方式进行解码，并将 fileencoding 设置为这个值，如果失败的话，就继续试验下一个编码。

## References

<http://edyfox.codecarver.org/html/vim_fileencodings_detection.html>
<https://vim.fandom.com/wiki/Reloading_a_file_using_a_different_encoding>
<https://stackoverflow.com/questions/16507777/set-encoding-and-fileencoding-to-utf-8-in-vim>