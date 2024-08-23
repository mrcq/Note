# indent

> 不过就算你尝试让 emacs 正确的格式化代码失败了，也并不意味着你失去了一切：还可 以用 `indent` 。
>
> 不过，GNU indent 也有和 GNU emacs 一样有问题的设定，所以你需要给它一些命令选 项。不过，这还不算太糟糕，因为就算是 GNU indent 的作者也认同 K&R 的权威性 (GNU 的人并不是坏人，他们只是在这个问题上被严重的误导了)，所以你只要给 indent 指定选项 `-kr -i8` (代表 `K&R，8 字符缩进`)，或使用 `scripts/Lindent` 这样就可以以最时髦的方式缩进源代码。
>
> `indent` 有很多选项，特别是重新格式化注释的时候，你可能需要看一下它的手册。 不过记住： `indent` 不能修正坏的编程习惯。
>
> [Linux 内核代码风格 — The Linux Kernel documentation](https://www.kernel.org/doc/html/v4.14/translations/zh_CN/coding-style.html)

ubuntu：sudo apt-get install indent

\# indent -选项 C源文件名

如

\# indent -npro -nbad -bap -bbo -nbc -br -brs -c33 -cd33 -ncdb -ce -ci4 -cli0 -cp33 -di1 -nfc1 -nfca -hnl -i4 -ip0 -l100 -lp -npcs -nprs -npsl -saf -sai -saw -nsc -nsob -nss -ncs -bli0 -ncdw -bad -nut my_c_src_file.c

然后会将源文件备份（文件名后加~），将格式化后的文件替换到源文件。

 

以下是自己定制的风格对应的选项，并封装为一个function放在 ~/.bashrc 文件

```bash
 function cindent_kr()
{
	if [ "$(which indent 2> /dev/null)" = "" ] ;then
		return
	fi
    if [ "$#" = "0" ] ;then
        echo "error: empty parameter."
        return
    fi
    local c_src_file=$*
 
    # MY_KR_STYLE_PARAM="-npro -kr -i4 -ts4 -nut -nsob -l100 -nbs -bad -bad -bbb -bbo -hnl -bli0 -cli0 -ncs"
    MY_KR_STYLE_PARAM="-npro -nbad -bap -bbo -nbc -br -brs -c33 -cd33 -ncdb -ce -ci4 -cli0 -cp33 -di1 -nfc1 -nfca -hnl -i4 -ip0 -l100 -lp -npcs -nprs -npsl -saf -sai -saw -nsc -nsob -nss -ncs -bli0 -ncdw -bad -nut"
 
	indent ${MY_KR_STYLE_PARAM} ${c_src_file}
}
```

使用时更方便，如

\# cindent_kr  my_c_src_file.c