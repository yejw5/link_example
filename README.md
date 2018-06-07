# link_example

## g++编译参数
| 参数 | 作用 |
| ----- | ----- |
| -L | 表示要链接的库所在的目录。-L.  表示要链接的库在当前目录， -L/usr/lib 表示要连接的库在/usr/lib下。目录在/usr/lib时，系统会自动搜索这个目录，可以不用指明 |
| -l (L的小写) | 表示需要链接库的名称，注意不是库文件名称，比如库文件为 libtest.so，那么库名称为test |
| -include | 包含头文件，这个很少用，因为一般情况下在源码中，都有指定头文件 |
| -I (i 的大写) | 指定头文件的所在的目录，可以使用相对路径 |
| -shared | 指定生成动态链接库 |
| -fPIC | 表示编译为位置独立的代码，不用此选项的话编译后的代码是位置相关的所以动态载入时事通过代码拷贝的方式来满足不同进程的需要，而不能达到真正代码共享的目的 |


## static link
```sh
# output dir
mkdir build

g++ -c print/print.cc -I. -o build/print.o

# pack static lib
ar cqs build/libprint.a build/print.o

g++ -c src/main.cc -I. -o build/main.o

# link
g++ -o build/main build/main.o -L./build -lprint

# run
./build/main
```

## shared link
```sh
# output dir
mkdir build

# create shared library
g++ -fPIC -shared -o build/libprint.so print/print.cc -I.

g++ -c src/main.cc -I. -o build/main.o

g++ -o build/main build/main.o -L./build -lprint

# run
LD_LIBRARY_PATH=./build/:$LD_LIBRARY_PATH ./build/main
```
