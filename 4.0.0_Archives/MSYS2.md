# 安装

请直接按照[**官网平台**](https://msys2.org)中的说明安装和前置操作

或者安装完成之后按照以下描述进行前置操作：

1. 请先安装一些工具，eg.mingw-w64 GCC ，这样才能开始编译项目：
    ```
    $ pacman -S mingw-w64-ucrt-x86_64-gcc
    ```
此时终端中将显示如下的输出，按Enter继续：
```
    resolving dependencies...
looking for conflicting packages...

Packages (15) mingw-w64-ucrt-x86_64-binutils-2.41-2
            mingw-w64-ucrt-x86_64-crt-git-11.0.0.r216.gffe883434-1
            mingw-w64-ucrt-x86_64-gcc-libs-13.2.0-2  mingw-w64-ucrt-x86_64-gmp-6.3.0-2
            mingw-w64-ucrt-x86_64-headers-git-11.0.0.r216.gffe883434-1
            mingw-w64-ucrt-x86_64-isl-0.26-1  mingw-w64-ucrt-x86_64-libiconv-1.17-3
            mingw-w64-ucrt-x86_64-libwinpthread-git-11.0.0.r216.gffe883434-1
            mingw-w64-ucrt-x86_64-mpc-1.3.1-2  mingw-w64-ucrt-x86_64-mpfr-4.2.1-2
            mingw-w64-ucrt-x86_64-windows-default-manifest-6.4-4
            mingw-w64-ucrt-x86_64-winpthreads-git-11.0.0.r216.gffe883434-1
            mingw-w64-ucrt-x86_64-zlib-1.3-1  mingw-w64-ucrt-x86_64-zstd-1.5.5-1
            mingw-w64-ucrt-x86_64-gcc-13.2.0-2

Total Download Size:    49.38 MiB
Total Installed Size:  418.82 MiB

:: Proceed with installation? [Y/n]
[... downloading and installation continues ...]
```

2. 此时就可以使用`gcc`来build software in windows了：
    ```
    $ gcc --version
    gcc.exe (Rev2, Built by MSYS2 project) 13.2.0
    ```

---

# 使用

此时已经不要再使用cmd和powershell了，请直接双击打开msys2 mingw64终端

1. 更新包管理器 Pacman
在mingw64终端中，运行以下命令，这相当于linux中的 apt-get update
```bash
pacman -Syu
```
这可能会提示关闭终端，按照要求操作后，请重新启动终端

2. 安装编译工具和依赖库
使用pacman(MSYS2的包管理器)来安装项目所需要的内容，这相当于linux中的sudo spt-get install ...
```bash
pacman -S --needed base-devel mingw-w64-x86_64-toolchain mingw-w64-x86_64-sqlite3 mingw-w64-x86_64-openssl mingw-w64-x86_64-argon2
```
- base-devel 和 mingw-w64-x86_64-toolchain 包含了 gcc, make 等核心编译工具
- 后面的包就是依赖的 sqlite3, openssl 和 argon2 的 Windows 版本，**请根据需要修改**

3. 修改Makefile **这一步非常的重要**
由于在windows和linux在链接库时的方式略有不同，需要对makefile做一个修改:
```Makefile
LDFLAGS = -lsqlite3 -lcrypto -largon2
```
修改为：
```Makefile
LDFLAGS = -lsqlite3 -l:libcrypto.a -l:libssl.a -largon2
```
**在 MinGW 环境下，OpenSSL 的库文件名是 libcrypto.a 和 libssl.a，而 -lcrypto 的简写有时可能无法被正确找到。使用 -l:libcrypto.a 可以精确指定链接哪个文件**

4. 编译和运行
现在，需要在msys2 mingw64终端中，cd 到项目根目录中，就可以像是在linux中一样的操作了：
```bash
make all
./oracipher_demo.exe
# 注意，在Windows上生成的可执行文件通常有 .exe 后缀
```

---

# 特殊命令

1. 诊断path变量
```bash
echo $PATH
```
一个正常的path输出应该类似如此：
```bash
/ucrt64/bin:/mingw64/bin:/usr/local/bin:/usr/bin:/bin:...
```

2. 强制更新核心包
在ucrt64终端中，运行以下的命令；
这个命令会更新系统，uu这个参数在修复不一致性时非常有效：
```bash
pacman --Synn
```

3. 安装meta-package元包，这能保证开发环境都被正确的安装
在ucrt64终端中运行：
```bash
pacman -S --needed mingw-w64-ucrt-x86_64-toolchain
```
这个命令会自动安装 gcc, binutils, make 以及其他所有编译所需的基础工具。因为我们之前已经安装了 gcc，它应该会提示很多包已经存在并跳过它们

