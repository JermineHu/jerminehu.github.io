---
title: "Golang生成C/C++可调用的动态库.so和静态库"
date: 2017-03-29T16:06:08+08:00
categories: ["All","golang","C/C++"]
tags: ["golang","C/C++"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["go","C","C++","golang" ]
description: "Golang生成C/C++可调用的动态库.so和静态库"
---

Golang类似于C的静态语言，效率也接近于C，如果Golang也可以导出可供C调用的库，那可以和很多高级语言say goodbye了，goodbye似乎又有点武断，但至少说，Golang可以做很多事，而且效率优于很多高级语言，这样说应该没有问题。 
接下来，就从三个方面分别来介绍Golang中关于库的使用。

## Go中调用go编译的共享库，避免源码调用

```
Using Share Library
The latest Go 1.5 version is out. As part of the new features, Go compiler can compile packages as a shared libraries.
It accepts -buildmode argument that determines how a package is compiled. These are the following options:

* archive: Build the listed non-main packages into .a files. Packages named main are ignored.
* c-archive: Build the listed main package, plus all packages it imports, into a C archive file.
* c-shared: Build the listed main packages, plus all packages that they import, into C shared libraries.
* shared: Combine all the listed non-main packages into a single shared library.
* exe: Build the listed main packages and everything they import into executables. Packages not named main are ignored.
By default, listed main packages are built into executables and listed non-main packages are built into .a files.
```

平时编译、运行golang，可能用得比较多的是下面这些：

```
go run xxx.go //直接运行xxx.go，有点类似解释语言，比如lua, python，其实执行go run，是对源文件做了build，然后再run执行文件的，只是这些都在后台做了

go build xxx.go //编译产生同名的执行档，如果在源文件目录下直接go build会产生与目录名同名的可执行档
go install xxx.go //也是编译，只是编译后会将同名的执行档安装到$GOPATH/bin下面，同样在源文件目录下直接go install，会把与目录同名的可执行档安装在$GOPATH/bin下

```

以上的go build都是默认的编译，也就是-buildmode是default的


```
**By default, listed main packages are built into executables and listed non-main packages are built into .a files.**

```

现在进入正题，这里我主要是要说明下面这个选项的使用方法：

```
* shared: Combine all the listed non-main packages into a single shared library.

```

我们先看一个hello world的例子：

```go
package main

import "fmt"

int main(){
    fmt.Println("hello world")
}

```

这里的fmt就是一个share library，这是go里面自带的，现在如果想自定义一个共享库，应该怎么做？ 
假设在$GOPATH/src下有：

```
--myAdd
        --add.go
--myCall
        --main.go

```


详细目录如下：

```
Jermine@Jermine-VirtualBox:~/mygo/src$ echo $GOPATH
/home/Jermine/mygo
Jermine@Jermine-VirtualBox:~/mygo$ ls
bin  pkg  src
Jermine@Jermine-VirtualBox:~/mygo/src$ ls myAdd
add.go
Jermine@Jermine-VirtualBox:~/mygo/src$ ls myCall
main.go

```

现在我们要自定义一个myAdd的package，像fmt一样的使用，先看下myAdd/add.go的内容：

```
   package myAdd                                                                                                                  
    
   func Sum(x, y int) int {
       return x + y
   }

```

在编译自定义package前，先看下官方怎么说:

```
Before compile any shared library, the standard builtin packages should be installed as shared library. This will allow any other shared library to link with them.
意思是在编译任何共享包前，先要把官方自身的标准包先以共享包方式安装下，这样其他的包就可以与它们做link。

```

下面就按官方要求先来编译一下：

```
Jermine@Jermine-VirtualBox:~/mygo/src/myAdd$ go install -buildmode=shared -linkshared std

```

编译后会在GOROOT的pkg目录下安装标准共享库, 这里对应linux_386_dynlink这个目录：

```
Jermine@Jermine-VirtualBox:~/mygo/src/golib$ ll ~/go/pkg
total 28
drwxr-xr-x  7 Jermine Jermine 4096 12月  5 09:21 ./
drwxr-xr-x 11 Jermine Jermine 4096 5月  25  2017 ../
drwxr-xr-x  2 Jermine Jermine 4096 5月  25  2017 include/
drwxr-xr-x 30 Jermine Jermine 4096 5月  25  2017 linux_386/
drwxrwxr-x 29 Jermine Jermine 4096 12月  5 09:22 linux_386_dynlink/
drwxr-xr-x  3 Jermine Jermine 4096 5月  25  2017 obj/
drwxr-xr-x  3 Jermine Jermine 4096 5月  25  2017 tool/
Jermine@Jermine-VirtualBox:~/mygo/src/golib$ ll ~/go/pkg/linux_386_dynlink/
total 31252
drwxrwxr-x 29 Jermine Jermine     4096 12月  5 09:22 ./
drwxr-xr-x  7 Jermine Jermine     4096 12月  5 09:21 ../
drwxrwxr-x  2 Jermine Jermine     4096 12月  5 09:22 archive/
-rw-rw-r--  1 Jermine Jermine   121966 12月  5 09:21 bufio.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 bufio.shlibname
-rw-rw-r--  1 Jermine Jermine   111466 12月  5 09:21 bytes.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 bytes.shlibname
drwxrwxr-x  2 Jermine Jermine     4096 12月  5 09:22 compress/
drwxrwxr-x  2 Jermine Jermine     4096 12月  5 09:22 container/
-rw-rw-r--  1 Jermine Jermine    94220 12月  5 09:21 context.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 context.shlibname
drwxrwxr-x  4 Jermine Jermine     4096 12月  5 09:22 crypto/
-rw-rw-r--  1 Jermine Jermine    20014 12月  5 09:21 crypto.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 crypto.shlibname
drwxrwxr-x  3 Jermine Jermine     4096 12月  5 09:22 database/
drwxrwxr-x  2 Jermine Jermine     4096 12月  5 09:22 debug/
drwxrwxr-x  2 Jermine Jermine     4096 12月  5 09:22 encoding/
-rw-rw-r--  1 Jermine Jermine     5494 12月  5 09:21 encoding.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 encoding.shlibname
-rw-rw-r--  1 Jermine Jermine     3846 12月  5 09:21 errors.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 errors.shlibname
-rw-rw-r--  1 Jermine Jermine    84486 12月  5 09:21 expvar.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 expvar.shlibname
-rw-rw-r--  1 Jermine Jermine   155402 12月  5 09:21 flag.a
-rw-rw-r--  1 Jermine Jermine       10 12月  5 09:22 flag.shlibname


```

//GOROOT如下:

```
Jermine@Jermine-VirtualBox:~/samba_share/golang$ echo $GOROOT
/home/Jermine/go

```

接下来，就可以编译自定义的共享库，并link到标准std库：

```
Jermine@Jermine-VirtualBox:~/mygo/src$ go install -buildmode=shared -linkshared myAdd
Jermine@Jermine-VirtualBox:~/mygo/src$ ls
github.com  golang.org  myAdd  test
Jermine@Jermine-VirtualBox:~/mygo/src$ ls myAdd/
add.go //这里并没有产生仍何文档，产生在那里接着向下看

```

实际产生共享是在GOPATH/pkg目录下：

```
Jermine@Jermine-VirtualBox:~/mygo$ ls
bin  pkg  src
Jermine@Jermine-VirtualBox:~/mygo$ ls pkg
linux_386  linux_386_dynlink
Jermine@Jermine-VirtualBox:~/mygo$ ls pkg/linux_386_dynlink/
libmyAdd.so  myAdd.a  myAdd.shlibname //这些库在编译时已经链接到标准std库，所以下面在应用程序中可以直接使用。

```


下面是应用程序的代码：
```

Jermine@Jermine-VirtualBox:~/mygo/src$ vim myCall/main.go
  package main                                                                                                                  
   
  import (
      "fmt"
      "myAdd"  //这就是上面自定义package，如果这里不import，只要下面用了包里面的函数，gofmt也会自动把这个包加进来，是不是有点跟使用fmt等其他包一样的用法
  )
   
  func main() {
      fmt.Println("my Call application")
      fmt.Printf("Sum: %d\n", myAdd.Sum(2, 3))
  }
 Jermine@Jermine-VirtualBox:~/mygo/src$ ./main 
my Call application
Sum: 5

```


## Golang生成C可调用的动态库so
假设代码结构如下：

```sh
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ ls
exptest.go
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ vim exptest.go
```

```go

 package main
  
 import "C"  
 import "fmt"
  
 //export Summ
 func Summ(x, y int) int {
     return x + y
 }
  
 //export Hello
 func Hello(str string) {
     fmt.Printf("Hello: %s\n", str)
 }
  
 func main() {
 // We need the main function to make possible
 // CGO compiler to compile the package as C shared       library
}

```


官方说法：

```
Go functions can be executed from C applications. They should be exported by using the following comment line:
//export <your_function_name>
```

//export your_function_name目的是产生对应的头文件函数声明，以及cgo的与C之间一些转换规则，详细可以参考生成的头文件。 
另外，import “C”这个也是不能少的，表示导入一个C库

下面就是编译共享库:

```
//The packaged should be compiled with buildmode flags c-shared or c-archive：

Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ go build -buildmode=c-shared -o libexptest.so exptest.go 
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ ls
exptest.go  libexptest.h  libexptest.so

```

下面写一个C程序调用这个so动态库：

```
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ vim exptest.c
 #include <stdio.h>
 #include "libexptest.h"  //上面产生的头文件
  
 int main() {
     printf("This is exptest application.\n");
     GoString str = {"Hi JXES", 7};
     Hello(str); //调用的函数
     printf("sum: %d\n", Summ(2, 3)); //调用的函数
     return 0;
 }

 Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ export LD_LIBRARY_PATH=./
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ echo $LD_LIBRARY_PATH 
./
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ ./exptest 
This is exptest application.
Hello: Hi JXES
sum: 5

```

这里需要说明下，在linux系统中应用程序关联到共享库时，都是从LD_LIBRARY_PATH环境变量指定的目录下找需要的.so，所以这里一定要指定在当前目录，如果不指定，可以把产生的so文件复制到/usr/lib下也可以。

## Golang调用C生成的静态库.a
产生静态库只是编译的时候产生.a的静态库，库与测试程序代码如上，编译方法是:

```
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ go build -buildmode=c-archive -o libexptest.so exptest.go 
Jermine@Jermine-VirtualBox:~/mygo/src/mylib$ ls
exptest.go  libexptest.h  libexptest.a

// 应用程序编译的方法如下：
$ gcc -o exptest exptest.c libexptest.a 
```

## go调用C/C++使用值传递和指针传递

在实际项目中用到的不仅仅是基本类型之间的转换，更多的是函数封装中的值传递和指针传递，如何在C功能函数中和Go中进行各种值和指针传递呢？根本方法还是利用基本类型，包括特别常用unsafe.Pointer

```go

package main

/*
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#define MAX_FACES_PER_DETECT 64

typedef  struct Point{
    float x;
    float y;
}Point;

typedef struct Rectangle{
    Point lt;
    Point rd;
}Rectangle;

typedef struct DetectFaceInfo{
    int id;
    float score;
    Rectangle pos;
}DetectFaceInfo;



void setStruct(void **ppDetectInfo)
{
    DetectFaceInfo *pDetectInfo = (DetectFaceInfo *)malloc(sizeof(DetectFaceInfo));
    memset(pDetectInfo, 0 , sizeof(pDetectInfo));
    pDetectInfo->id = 1;
    pDetectInfo->score = 0.98f;
    pDetectInfo->pos.lt.x = 1;
    pDetectInfo->pos.lt.y = 1;
    pDetectInfo->pos.rd.x = 9;
    pDetectInfo->pos.rd.y = 10;

    fprintf(stdout, "A pDetectInfo address : %p\n", pDetectInfo);
    *ppDetectInfo = pDetectInfo;
}

int printStruct(void *pdetectinfo)
{
    DetectFaceInfo * pDetectInfo = (DetectFaceInfo *)pdetectinfo;
    fprintf(stdout, "B pDetectInfo address : %p\n", pDetectInfo);

    fprintf(stdout, "id: %d\n", pDetectInfo->id);
    fprintf(stdout, "score : %.3lf\n", pDetectInfo->score);
    fprintf(stdout, "pos.lt.x : %d\n", pDetectInfo->pos.lt.x);
    fprintf(stdout, "pos.lt.y : %d\n", pDetectInfo->pos.lt.y);
    fprintf(stdout, "pos.rd.x : %d\n", pDetectInfo->pos.rd.x);
    fprintf(stdout, "pos.rd.y : %d\n", pDetectInfo->pos.rd.y);
}

int freeStruct(void *pDetectInfo)
{
    fprintf(stdout, "C pDetectInfo address : %p\n", pDetectInfo);
    free((DetectFaceInfo*)pDetectInfo);
}

*/
import "C"

import (
    _ "fmt"
    _ "reflect"
    "unsafe"
)

func main() {
    var pDetectInfo unsafe.Pointer

    C.setStruct(&pDetectInfo)
    C.printStruct(pDetectInfo)
    C.freeStruct(pDetectInfo)
}

```

从上面的例子可以知道还是利用了C和go的基本类型转换，更多的是利用指针。得到的运行结果是：

```
A pDetectInfo address : 012832B0
B pDetectInfo address : 012832B0
id: 1
score : 0.980
pos.lt.x : 1.000
pos.lt.y : 1.000
pos.rd.x : 9.000
pos.rd.y : 10.000
C pDetectInfo address : 012832B0

```

 这是通过C的结构体将数据传送给go，同样的也可以利用在go中处理数据，然后传递给C，也是利用基本类型转换。这里做个记录：

* C中的结构体如果带了typedef的话，那么在go中定义C结构体就不需要带struct了。反则反之。
* Go中定义C结构体： var struct C.DetectFaceInfo
* 再次注意，如果在结构体中有字符数组或者是char指针，注意区别


## C/C++调用go实现struct值传递

```go

package main
/*
struct Vertex {
    int X;
    int Y;
};
*/
import "C"
import "fmt"

//export getVertex
func getVertex(X, Y C.int) C.struct_Vertex {
    return C.struct_Vertex{X, Y}
}

func main() {
    fmt.Println(getVertex(1, 2))
}


```