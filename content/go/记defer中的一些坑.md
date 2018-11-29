---
title: "记defer中的一些坑"
date: 2017-11-29T13:51:36+08:00
categories: ["All",golang]
tags: [golang]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: [“go”,"defer",golang ]
description: "记defer中的一些坑"
---

什么是 defer？如何理解 defer 关键字？Go 中使用 defer 的一些坑。

defer 意为延迟，在 golang 中用于延迟执行一个函数。它可以帮助我们处理容易忽略的问题，如资源释放、连接关闭等。但在实际使用过程中，有一些需要注意的地方（坑），下面我们一一道来。

## 一些结论

首先，我们来了解 defer 的一些结论：

1、若函数中有多个 defer，其执行顺序为 先进后出，可以理解为栈。

```GO
package main

import "fmt"

func main() {
  for i := 0; i < 5; i++ {
    defer fmt.Println(i)
  }
}

Output:
4
3
2
1
0
```

2、return 会做几件事：

给返回值赋值
调用 defer 表达式
返回给调用函数

```GO
package main

import "fmt"

func main() {
    fmt.Println(increase(1))
}

func increase(d int) (ret int) {
  defer func() {
    ret++
  }()

  return d
}
  
Output:
2
```

3、若 defer 表达式有返回值，将会被丢弃。

更多请参考[官方文档](http://golang.org/ref/spec#defer_statements)。

## 闭包与匿名函数
匿名函数：没有函数名的函数。
闭包：可以使用另外一个函数作用域中的变量的函数。

在实际开发中，defer 的使用经常伴随着闭包与匿名函数的使用。小心踩坑哦：

```GO
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        defer func() {
            fmt.Println(i)
        }()
    }
}

Output:
5
5
5
5
5
```

解释一下，defer 表达式中的 i 是对 for 循环中 i 的引用。到最后，i 加到 5，故最后全部打印 5。

如果将 i 作为参数传入 defer 表达式中，在传入最初就会进行求值保存，只是没有执行延迟函数而已。

```GO
for i := 0; i < 5; i++ {
    defer func(idx int) {
        fmt.Println(idx)
    }(i) // 传入的 i，会立即被求值保存为 idx
}
```

## 巩固一下
为了巩固一下上面的知识点，我们来思考几个例子。

例1：

```GO
func f() (result int) {
    defer func() {
        result++
    }()
    return 0
}
```

例2：

```GO
func f() (r int) {
    t := 5
    defer func() {
        t = t + 5
    }()
    return t
}
```

例3：

```GO
func f() (r int) {
    defer func(r int) {
        r = r + 5
    }(r)
    return 1
}
```

例4:

```GO
type Test struct {
    Max int
}

func (t *Test) Println() {
    fmt.Println(t.Max)
}

func deferExec(f func()) {
    f()
}

func call() {
    var t *Test
    defer deferExec(t.Println)

    t = new(Test)
}
```

有没有得出结果？例1的答案不是 0，例2的答案不是 10，例3的答案也不是 6。

例1，比较简单，参考结论2，将 0 赋给 result，defer 延迟函数修改 result，最后返回给调用函数。正确答案是 1。

例2，defer 是在 t 赋值给 r 之后执行的，而 defer 延迟函数只改变了 t 的值，r 不变。正确答案 5。

例3，这里将 r 作为参数传入了 defer 表达式。故 `func (r int)` 中的 r 非 `func f() (r int)` 中的 r，只是参数命名相同而已。正确答案 1。

例4，这里将发生 panic。将方法传给 deferExec，实际上在传的过程中对方法求了值。而此时的 t 任然为 nil。

参考文档
[1] https://tiancaiamao.gitbooks.io/go-internals/content/zh/03.4.html

[2] http://golang.org/ref/spec#defer_statements