# vim

## 配置

```shell
"语法高亮"
syntax on
"显示行号"
set number
"显示当前光标位置"
set ruler
"使用vim而非vi"
set nocp
"c/c++自动缩进"
set cindent
"缩进8个空格"
set ts=8
"不用交换文件"
set noswapfile
"文本宽度80列自动换行"
au FileType c,cpp,python,vim set textwidth=80
"竖线警示"
"set colorcolumn=81"
"高亮当前行"
set cursorline
"鼠标滚轮"
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
  set mouse=a
endif
```

