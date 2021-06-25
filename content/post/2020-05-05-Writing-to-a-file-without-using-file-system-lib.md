---
date: "2020-05-05T00:00:00Z"
subtitle: Intro to *tee* unix command
tags:
- random
title: Writting to a file without using a file system library of that language
---

# tee unix command
Manytimes you want to write things to a file programmatically. That means, you write a code so as to fill the file. This involves
importing the `file system` library in the respective language and using it. For example python and JS both use `fs` library.
But there is a simpler way for super lazy people !

The idea is to just use the print the output and write the ouput to a file. For example you want to write, 1 to 10 numbers in a file.
Write a simple python code for that
```
for i in range(1,11):
  print(i)
```

Now when you run `python FILENAME.py` it prints the numbers 1 to 10 on the terminal. Now we will use the `tee` command to write it to a another file

```
python FILENAME.py | tee ONE_TO_TEN.txt
```

There will be a file by name `ONE_TO_TEN.txt` created with content of numbers from 1 to 10.
Learn more about tee command from [here](https://linuxize.com/post/linux-tee-command/)

*Why did I write a blog about such a small thing? I got excited after knowing about such useful and awesome linux command. There have been situations in the past where I wanted to save the output of a command in some file. But never knew a proper way to do it :(*
