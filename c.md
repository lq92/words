## C
  读取文件: h < test.txt
  写入文件: h > test.txt
  写入文件追加: h >> test.txt
  在windows下查看文件内容: type test.txt
  在Unix下查看文件内容: cat test.txt
  编译多源代码文件的程序
    假设file1.c和file2.c是两个内含C函数的文件，下面的命令将编译两个文件并生成名为a.exe的可执行文件
      `gcc file1.c file2.c`