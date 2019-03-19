---
title: "Golang与C互用以及调用C的so动态库和a静态库"
date: 2017-02-23T16:48:24+08:00
categories: ["All","golang","C/C++"]
tags: ["golang","C/C++"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["golang","C/C++" ]
description: "Golang与C互用以及调用C的so动态库和a静态库"
---

Golang与C的关系非常密切，下面主要介绍在Golang中使用C。

## Golang中嵌入C代码

```go
 package main
 //#include <stdio.h>
 //#include <stdlib.h>
 /*
 void Hello(char *str) {
     printf("%s\n", str);
 }
 */
 import "C" //假设把C当成包，其实有点类似C++的名字空间
  import "unsafe" //C指针的使用，在C代码中申请的空间，GC垃圾回收机制不会管理，所以需要自己手动free申请的空间
  func main() {
      s := "Hello Cgo"
      cs := C.CString(s)
      C.Hello(cs)
      C.free(unsafe.Pointer(cs))
  }

```
3,4行的注释也可以写/* */形式 
第4行与第5行之间不能有空行，同样第9行与第10行之间也不 
能有行，否则编译时cgo会报错：

```
Jermine@Jermine-VirtualBox:~/mygo/src/myC/tmp$ go run goc.go
# command-line-arguments
could not determine kind of name for C.Hello
could not determine kind of name for C.free

```
## Golang中调用C的动态库so

**C库源程序代码：**

```c
 //foo.c
 #include <stdio.h>
  
 int Num = 8;
  
 void foo(){
     printf("I dont have new line.");
     printf("I have new line\n");
     printf("I have return\r");
     printf("I have return and new line\r\n");
 }

```

```c
 //foo.h  
 #ifndef __FOO_H
 #define __FOO_H
  
 #ifdef __cplusplus
 extern "C" {
 #endif
  
 extern int Num;
 extern void foo();
  
 #ifdef __cplusplus
 }
 #endif
  
 #endif

```

## Go调用C库的代码：

```go
 package main
  
 /*
 #cgo CFLAGS: -I.
 #cgo LDFLAGS: -L. -lfoo
 #include <stdio.h>
 #include <stdlib.h>
 #include "foo.h"
 */
  import "C"
  import "unsafe"
  import "fmt"
  func Prin(s string) {
      cs := C.CString(s)
      defer C.free(unsafe.Pointer(cs))
      C.fputs(cs, (*C.FILE)(C.stdout))
      //C.free(unsafe.Pointer(cs))
      C.fflush((*C.FILE)(C.stdout))
  }

  func main() {
      fmt.Println("vim-go")
      fmt.Printf("rannum:%x\n", C.random())
      Prin("Hello CC")
      fmt.Println(C.Num)
      C.foo()
  }
```

注意import “C”这行与第9行之间也不能有空格，原因同上编译会报错。 
go与库、头文件的目录结构，以及编译：

```
Jermine@Jermine-VirtualBox:~/mygo/src/myC/sotest$go build cc.go //编译之后产生cc
Jermine@Jermine-VirtualBox:~/mygo/src/myC/sotest$ ls
cc  cc.go  foo.h  libfoo.so
Jermine@Jermine-VirtualBox:~/mygo/src/myC/sotest$ export LD_LIBRARY_PATH=./
Jermine@Jermine-VirtualBox:~/mygo/src/myC/sotest$ ./cc
vim-go
rannum:6b8b4567
Hello CC8
I dont have new line.I have new line
I have return and new line

```

如果c库的程序改为下面这样：

```c
 //foo.c
 #include <stdio.h>
  
 int Num = 8;
  
 void foo(){
     printf("I dont have new line.");
     //printf("I have new line\n");
     //printf("I have return\r");
     //printf("I have return and new line\r\n");
 }

``` 
此时，go中调用foo时没有任何反应，即不会输出，不会输出的原因是printf后面没有加换行符，但是如果加了8，9，10这些测试行后，第7行也会显示，原因是第10行最后有一个换行符，这个应该是向stdout输出时，字符流是放在buffer中，如果没有换行，buffer中的数据不会立即输出。在go调用C的测试程序中有写了一个测试小函数用来测试stdout，验证了没有fflush，stdout上不会显示输出信息。但平时在写C程序的时候，似乎没有这样的问题，这个是因为C的运行环境自动做了fflush的动作，这个可能是Golang的不足，也许golan表g认为这样做更好。这只是个人的分析，如有分析不对的地方，请跟贴，谢谢。

## Golang调用C的静态库a
所有源程序与动库演示代码类似，只是编译C库时，是用的静态编译，如下所示：

```s
Jermine@Jermine-VirtualBox:~/mygo/src/myC$ ls
foo.c 
Jermine@Jermine-VirtualBox:~/mygo/src/myC$ gcc -c foo.c
foo.c foo.o
Jermine@Jermine-VirtualBox:~/mygo/src/myC$ ar -rv libfoo.a foo.o
foo.c foo.o libfoo.a
```

Go调用的代码与动态库一样，下面是目录结构：

```s
Jermine@Jermine-VirtualBox:~/mygo/src/myC/atest$ ls
cc  cc.go  foo.h  libfoo.a
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


execute `go build -buildmode=c-shared -o go-test1.so go-test1.go` 将得到`.h`文件和`.so`文件如下：

```
~/Desktop/test ⌚ 14:40:44
$ go build -buildmode=c-shared -o go-test1.so go-test1.go

~/Desktop/test ⌚ 14:41:05
$ ls
go-test1.go  go-test1.h  go-test1.so

~/Desktop/test ⌚ 14:41:06
$ cat go-test1.h 
/* Code generated by cmd/cgo; DO NOT EDIT. */

/* package command-line-arguments */


#line 1 "cgo-builtin-prolog"

#include <stddef.h> /* for ptrdiff_t below */

#ifndef GO_CGO_EXPORT_PROLOGUE_H
#define GO_CGO_EXPORT_PROLOGUE_H

typedef struct { const char *p; ptrdiff_t n; } _GoString_;

#endif

/* Start of preamble from import "C" comments.  */


#line 2 "go-test1.go"

struct Vertex {
    int X;
    int Y;
};

#line 1 "cgo-generated-wrapper"


/* End of preamble from import "C" comments.  */


/* Start of boilerplate cgo prologue.  */
#line 1 "cgo-gcc-export-header-prolog"

#ifndef GO_CGO_PROLOGUE_H
#define GO_CGO_PROLOGUE_H

typedef signed char GoInt8;
typedef unsigned char GoUint8;
typedef short GoInt16;
typedef unsigned short GoUint16;
typedef int GoInt32;
typedef unsigned int GoUint32;
typedef long long GoInt64;
typedef unsigned long long GoUint64;
typedef GoInt64 GoInt;
typedef GoUint64 GoUint;
typedef __SIZE_TYPE__ GoUintptr;
typedef float GoFloat32;
typedef double GoFloat64;
typedef float _Complex GoComplex64;
typedef double _Complex GoComplex128;

/*
  static assertion to make sure the file is being used on architecture
  at least with matching size of GoInt.
*/
typedef char _check_for_64_bit_pointer_matching_GoInt[sizeof(void*)==64/8 ? 1:-1];

typedef _GoString_ GoString;
typedef void *GoMap;
typedef void *GoChan;
typedef struct { void *t; void *v; } GoInterface;
typedef struct { void *data; GoInt len; GoInt cap; } GoSlice;

#endif

/* End of boilerplate cgo prologue.  */

#ifdef __cplusplus
extern "C" {
#endif


extern struct Vertex getVertex(int p0, int p1);

#ifdef __cplusplus
}
#endif

~/Desktop/test ⌚ 14:41:22
$ file go-test1.so 
go-test1.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=59440090a28e0869a12951bb9fc4b311e038ee8f, not stripped


```

## Golang调用C++函数


lib.cpp file (这里面你可以写 c++ 代码 ，但是go 需要调用的，你需要写成 红色标记的方式导出)

```C++
extern "C" int start(void); //标记的方式导出
 
int start()//int argc, char **argv
{
  return;
}

```

注意：go不能直接调用C++，必须要以C的函数导出才能供go调用。

**lib.h** 

`int start(void);`

```golang
package main

/*
#include "lib.h"
#cgo LDFLAGS: -L.. -llib

*/
import "C"

func main() {
	C.start()
}
gcc -g -fPIC -c -o lib.o lib.cpp
gcc -g -fPIC -shared -o liblib.so lib.o

```

其他参照： 

https://blog.csdn.net/zdy0_2004/article/details/79124269

https://studygolang.com/articles/16296

https://blog.csdn.net/a1368783069/article/details/84142924