---
title: "Golang Cross Compiled Config - go语言跨平台交叉编译"
date: 2016-10-23T14:14:13+08:00
categories: ["All","golang"]
tags: ["golang","交叉编译","cross-compiling"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["golang","交叉编译","cross compiling","go build"]
description: "Golang Cross Compiled Config - go语言跨平台交叉编译"
---

# go在各个平台交叉编译的介绍

`Golang` 支持交叉编译，在一个平台上生成另一个平台的可执行程序，最近使用了一下，非常好用，这里备忘一下。


### Mac 下的交叉编译

`Mac` 下编译 `Linux` 和 `Windows 64` 位可执行程序

```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go 
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

```

### Linux 下的交叉编译

`Linux` 下编译 `Mac` 和 `Windows 64`位可执行程序

```
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go 
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

```

### Windows 下的交叉编译

`Windows` 下编译 `Mac` 和 `Linux 64`位可执行程序

```
SET CGO_ENABLED=0 
SET GOOS=darwin 
SET GOARCH=amd64 
go build main.go 
SET CGO_ENABLED=0 
SET GOOS=linux 
SET GOARCH=amd64 
go build main.go
```

### 参数解释

`GOOS`：目标平台的操作系统（`darwin、freebsd、linux、windows`） 
`GOARCH`：目标平台的体系架构（`386、amd64、arm`） 
交叉编译不支持 `CGO` 所以要禁用它

上面的命令编译 64 位可执行程序，你当然应该也会使用 386 编译 32 位可执行程序.

### 获取go已经支持的平台

To execute this code `go tool dist list`，you  will get a list that all supported platforms.

获取结果如下：

```
Husee@Jermine-PC MINGW64 ~/Desktop
$ go tool dist list
android/386
android/amd64
android/arm
android/arm64
darwin/386
darwin/amd64
darwin/arm
darwin/arm64
dragonfly/amd64
freebsd/386
freebsd/amd64
freebsd/arm
linux/386
linux/amd64
linux/arm
linux/arm64
linux/mips
linux/mips64
linux/mips64le
linux/mipsle
linux/ppc64
linux/ppc64le
linux/s390x
nacl/386
nacl/amd64p32
nacl/arm
netbsd/386
netbsd/amd64
netbsd/arm
openbsd/386
openbsd/amd64
openbsd/arm
plan9/386
plan9/amd64
plan9/arm
solaris/amd64
windows/386
windows/amd64

```


# go对各个操作系统和指令集的支持详情如下

### Go (Golang) GOOS and GOARCH

All of the following information is based on `go version go1.8.3 darwin/amd64`.

#### A list of valid GOOS values

(Bold = supported by `go` out of the box, ie. without the help of a C compiler, etc.)

- `android`
- **`darwin`**
- **`dragonfly`**
- **`freebsd`**
- **`linux`**
- **`nacl`**
- **`netbsd`**
- **`openbsd`**
- **`plan9`**
- **`solaris`**
- **`windows`**
- `zos`

#### A list of valid GOARCH values

(Bold = supported by `go` out of the box, ie. without the help of a C compiler, etc.)

- **`386`**
- **`amd64`**
- **`amd64p32`**
- **`arm`**
- `armbe`
- **`arm64`**
- `arm64be`
- **`ppc64`**
- **`ppc64le`**
- **`mips`**
- **`mipsle`**
- **`mips64`**
- **`mips64le`**
- `mips64p32`
- `mips64p32le`
- `ppc`
- `s390`
- **`s390x`**
- `sparc`
- `sparc64`

#### A list of valid 32-bit GOARCH values

(Bold = supported by `go` out of the box, ie. without the help of a C compiler, etc.)

- **`386`**
- **`amd64p32`**
- **`arm`**
- `armbe`
- **`mips`**
- **`mipsle`**
- `mips64p32`
- `mips64p32le`
- `ppc`
- `s390`
- `sparc`

#### A list of valid 64-bit GOARCH values

(Bold = supported by `go` out of the box, ie. without the help of a C compiler, etc.)

- **`amd64`**
- **`arm64`**
- `arm64be`
- **`ppc64`**
- **`ppc64le`**
- **`mips64`**
- **`mips64le`**
- **`s390x`**
- `sparc64`

#### A list of GOOS/GOARCH supported by `go` out of the box

- `darwin/386`
- `darwin/amd64`
- `dragonfly/amd64`
- `freebsd/386`
- `freebsd/amd64`
- `freebsd/arm`
- `linux/386`
- `linux/amd64`
- `linux/arm`
- `linux/arm64`
- `linux/ppc64`
- `linux/ppc64le`
- `linux/mips`
- `linux/mipsle`
- `linux/mips64`
- `linux/mips64le`
- `linux/s390x`
- `nacl/386`
- `nacl/amd64p32`
- `nacl/arm`
- `netbsd/386`
- `netbsd/amd64`
- `netbsd/arm`
- `openbsd/386`
- `openbsd/amd64`
- `openbsd/arm`
- `plan9/386`
- `plan9/amd64`
- `plan9/arm`
- `solaris/amd64`
- `windows/386`
- `windows/amd64`

#### A list of 32-bit GOOS/GOARCH supported by `go` out of the box

- `darwin/386`
- `freebsd/386`
- `freebsd/arm`
- `linux/386`
- `linux/arm`
- `linux/mips`
- `linux/mipsle`
- `nacl/386`
- `nacl/amd64p32`
- `nacl/arm`
- `netbsd/386`
- `netbsd/arm`
- `openbsd/386`
- `openbsd/arm`
- `plan9/386`
- `plan9/arm`
- `windows/386`

#### A list of 64-bit GOOS/GOARCH supported by `go` out of the box

- `darwin/amd64`
- `dragonfly/amd64`
- `freebsd/amd64`
- `linux/amd64`
- `linux/arm64`
- `linux/ppc64`
- `linux/ppc64le`
- `linux/mips64`
- `linux/mips64le`
- `linux/s390x`
- `netbsd/amd64`
- `openbsd/amd64`
- `plan9/amd64`
- `solaris/amd64`
- `windows/amd64`

#### Support Grid

|                   | `a` | `m` | `d` | `f` | `l` | `c` | `n` | `o` | `p` | `s` | `w` | `z` |
| ----------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **`386`**         | `A` | `O` |     | `O` | `O` | `O` | `O` | `O` | `O` |     | `O` |     |
| **`amd64`**       | `A` | `O` | `O` | `O` | `O` |     | `O` | `O` | `O` | `O` | `O` |     |
| **`amd64p32`**    |     |     |     |     |     | `O` |     |     |     |     |     |     |
| **`arm`**         | `A` | `B` |     | `O` | `O` | `O` | `O` | `O` | `O` |     |     |     |
| **`armbe`**       |     |     |     |     |     |     |     |     |     |     |     |     |
| **`arm64`**       | `A` | `C` |     |     | `O` |     |     |     |     |     |     |     |
| **`arm64be`**     |     |     |     |     |     |     |     |     |     |     |     |     |
| **`ppc64`**       |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`ppc64le`**     |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`mips`**        |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`mipsle`**      |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`mips64`**      |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`mips64le`**    |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`mips64p32`**   |     |     |     |     |     |     |     |     |     |     |     |     |
| **`mips64p32le`** |     |     |     |     |     |     |     |     |     |     |     |     |
| **`ppc`**         |     |     |     |     |     |     |     |     |     |     |     |     |
| **`s390`**        |     |     |     |     |     |     |     |     |     |     |     |     |
| **`s390x`**       |     |     |     |     | `O` |     |     |     |     |     |     |     |
| **`sparc`**       |     |     |     |     |     |     |     |     |     |     |     |     |
| **`sparc64`**     |     |     |     |     |     |     |     |     |     |     |     |     |

##### Key

`a` = `android`, `m` = `darwin` (`macos`), `d` = `dragonfly`, `f` = `freebsd`,
`l` = `linux`, `c` = `nacl`, `n` = `netbsd`, `o` = `openbsd`, `p` = `plan9`,
`s` = `solaris`, `w` = `windows`, `z` = `zos`

(blank): Unsupported

`O`: Supported

`A`:
```
warning: unable to find runtime/cgo.a
/usr/local/go/pkg/tool/darwin_amd64/link: running clang failed: exit status 1
ld: unknown option: -z
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

`B`:
```
warning: unable to find runtime/cgo.a
/usr/local/go/pkg/tool/darwin_amd64/link: running clang failed: exit status 1
ld: warning: ignoring file /var/folders/dd/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/T/go-link-xxxxxxxxx/go.o, file was built for armv7 which is not the architecture being linked (x86_64): /var/folders/dd/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/T/go-link-xxxxxxxxx/go.o
Undefined symbols for architecture x86_64:
  "_main", referenced from:
     implicit entry/start for main executable
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

`C`:
```
warning: unable to find runtime/cgo.a
/usr/local/go/pkg/tool/darwin_amd64/link: running clang failed: exit status 1
ld: warning: ignoring file /var/folders/dd/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/T/go-link-xxxxxxxxx/go.o, file was built for unsupported file format ( 0xCF 0xFA 0xED 0xFE 0x0C 0x00 0x00 0x01 0x00 0x00 0x00 0x00 0x01 0x00 0x00 0x00 ) which is not the architecture being linked (x86_64): /var/folders/dd/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/T/go-link-xxxxxxxxx/go.o
Undefined symbols for architecture x86_64:
  "_main", referenced from:
     implicit entry/start for main executable
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

#### Source 1

##### main.go

```go
package main

func main() {}
```

##### make.sh

```sh
#!/bin/sh

os_archs=()

# Reference:
# https://github.com/golang/go/blob/master/src/go/build/syslist.go
for goos in android darwin dragonfly freebsd linux nacl netbsd openbsd plan9 solaris windows zos
do
    for goarch in 386 amd64 amd64p32 arm armbe arm64 arm64be ppc64 ppc64le mips \
        mipsle mips64 mips64le mips64p32 mips64p32le ppc s390 s390x sparc sparc64
    do
        GOOS=${goos} GOARCH=${goarch} go build -o /dev/null main.go >/dev/null 2>&1
        if [ $? -eq 0 ]
        then
            os_archs+=("${goos}/${goarch}")
        fi
    done
done

for os_arch in "${os_archs[@]}"
do
    echo ${os_arch}
done
```

#### Source 2

##### main.go

```go
package main

const (
	hello uint = 0xfedcba9876543210
)

func main() {}
```

##### make.sh

```sh
#!/bin/bash

# Reference:
# https://github.com/golang/go/blob/master/src/go/build/syslist.go
os_archs=(
    darwin/386
    darwin/amd64
    dragonfly/amd64
    freebsd/386
    freebsd/amd64
    freebsd/arm
    linux/386
    linux/amd64
    linux/arm
    linux/arm64
    linux/ppc64
    linux/ppc64le
    linux/mips
    linux/mipsle
    linux/mips64
    linux/mips64le
    linux/s390x
    nacl/386
    nacl/amd64p32
    nacl/arm
    netbsd/386
    netbsd/amd64
    netbsd/arm
    openbsd/386
    openbsd/amd64
    openbsd/arm
    plan9/386
    plan9/amd64
    plan9/arm
    solaris/amd64
    windows/386
    windows/amd64
)

os_archs_32=()
os_archs_64=()

for os_arch in "${os_archs[@]}"
do
    goos=${os_arch%/*}
    goarch=${os_arch#*/}
    GOOS=${goos} GOARCH=${goarch} go build -o /dev/null main.go >/dev/null 2>&1
    if [ $? -eq 0 ]
    then
        os_archs_64+=(${os_arch})
    else
        os_archs_32+=(${os_arch})
    fi
done

echo "32-bit:"
for os_arch in "${os_archs_32[@]}"
do
    printf "\t%s\n" "${os_arch}"
done
echo

echo "64-bit:"
for os_arch in "${os_archs_64[@]}"
do
    printf "\t%s\n" "${os_arch}"
done
echo
```