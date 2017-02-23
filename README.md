# build_android_kernel_x86_osx
nexus player

## Mac Hardware
MacBook Pro (Retina, 13-inch, Early 2015)

## System
macOS Sierra, 10.12.3

## OS X SDK
```
ls -ali /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs
1915385 drwxr-xr-x  5 root  wheel  170  2  9 09:37 .
1914962 drwxr-xr-x  5 root  wheel  170 12 16 06:44 ..
1915386 drwxr-xr-x  5 root  wheel  170  9 17  2017 MacOSX.sdk
2086424 drwxr-xr-x@ 5 ice   wheel  170 11 16  2015 MacOSX10.11.sdk
1948121 lrwxr-xr-x  1 root  wheel   10  2  9 09:08 MacOSX10.12.sdk -> MacOSX.sdk
```

## Issue fixing
### `elf.h` file not found
~~安装homebrew
安装libelf
brew install libelf~~
拷贝 `elf.h` (二选一，两个都行)
- [GitHub]（https://gist.github.com/mlafeldt/3885346）
- `/Volumes/android/aosp/external/elfutils/libelf/elf.h`，遇到了`features.h`文件未找到的错误，就将`#include <features.h>` 一行注释掉。
将 `elf.h` 拷贝到 `/usr/include` 或者 `/usr/local/include` 中，推荐放在后者，放在前者的话系统升级时会覆盖。
目前发现 `libelf` 不需要。

### `byteswap.h` file not found
```
In file included from arch/x86/tools/relocs_32.c:1:
arch/x86/tools/relocs.h:13:10: fatal error: 'byteswap.h' file not found
#include <byteswap.h>
         ^
1 error generated.
make[1]: *** [arch/x86/tools/relocs_32.o] Error 1
```
解决：拷贝对应文件夹中的头文件

### arch/x86/kernel/cpu/capflags.c:69:31: error: expected expression before ']' token
  [X86_FEATURE_PCLMULQDQ (4*32+]  = "pclmulqdq (4*32+”,
等这一个C文件里的错误。

根据Linux的github里面的sh脚本，这个文件应该是根据
`arch/x86/kernel/cpu/mkcapflags.sh`
`/Volumes/android/x86_64/arch/x86/include/asm/cpufeature.h` 和生成的。

解决：拷贝对应文件夹中的`capflags.c`

### 下载 `GNU sed` 和 `stat`
起初未替换`sed` 和`stat`， 内核编译好了，启动不了
```
brew install gnu-sed coreutils
```
设置环境变量`PATH`
按照上述安装命令中生成的提示信息设置`PATH`

