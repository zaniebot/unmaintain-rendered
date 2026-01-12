```yaml
number: 2222
title: pcre2-sys not correctly retrieved, compilation fails in MSYS2/MinGW, GCC 11
type: issue
state: closed
author: LigH-de
labels:
  - invalid
assignees: []
created_at: 2022-05-24T06:20:04Z
updated_at: 2022-05-24T13:10:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2222
synced_at: 2026-01-12T16:13:24Z
```

# pcre2-sys not correctly retrieved, compilation fails in MSYS2/MinGW, GCC 11

---

_@LigH-de_

#### What version of ripgrep are you using?

HEAD sources = a726d03641e081aa0f4c4a1b9f24f5a905fd5d04

#### How did you install ripgrep?

vcs_fetch routine in media-autobuild_suite uses git to retrieve HEAD sources:
```
vcs_fetch 
++ git rev-parse --git-dir
+ [[ -f .git/shallow ]]
+ unshallow=
+ git fetch --all -Ppft
Fetching origin
+ git remote set-head -a origin
origin/HEAD set to master
```

#### What operating system are you using ripgrep on?

MSYS2/MinGW on Windows (7 or 10)

#### Describe your bug.

[media-autobuild_suite issue 2195](https://github.com/m-ab-s/media-autobuild_suite/issues/2195) - please note especially https://github.com/m-ab-s/media-autobuild_suite/issues/2195#issuecomment-1135139851

Parts of build/ripgrep-git/ab-suite.rust.build.log:
```
CPPFLAGS: -D_FORTIFY_SOURCE=0 -D__USE_MINGW_ANSI_STDIO=1
CFLAGS: -mthreads -mtune=generic -O2 -pipe
CXXFLAGS: -mthreads -mtune=generic -O2 -pipe
LDFLAGS: -pipe -static-libgcc -static-libstdc++
/opt/cargo/bin/cargo.exe build --release --target=i686-pc-windows-gnu --jobs=1 --features pcre2

...

error: failed to run custom build command for `pcre2-sys v0.2.5`

Caused by:
  process didn't exit successfully: `E:\MABS\build\ripgrep-git\target\release\build\pcre2-sys-a3cacd8a0f6685c4\build-script-build` (exit code: 1)

...

  running: "ccache" "gcc" "-O3" "-ffunction-sections" "-fdata-sections" "-g" "-fno-omit-frame-pointer" "-m32" "-mthreads" "-mtune=generic" "-O2" "-pipe" "-I" "pcre2/src" "-I" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\include" "-DPCRE2_CODE_UNIT_WIDTH=8" "-DHAVE_STDLIB_H=1" "-DHAVE_MEMMOVE=1" "-DHEAP_LIMIT=20000000" "-DLINK_SIZE=2" "-DMATCH_LIMIT=10000000" "-DMATCH_LIMIT_DEPTH=10000000" "-DMAX_NAME_COUNT=10000" "-DMAX_NAME_SIZE=32" "-DNEWLINE_DEFAULT=2" "-DPARENS_NEST_LIMIT=250" "-DPCRE2_STATIC=1" "-DSTDC_HEADERS=1" "-DSUPPORT_PCRE2_8=1" "-DSUPPORT_UNICODE=1" "-DHAVE_WINDOWS_H=1" "-DSUPPORT_JIT=1" "-o" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\src\\pcre2_chartables.o" "-c" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\src\\pcre2_chartables.c"
  exit code: 1
  running: "ccache" "gcc" "-O3" "-ffunction-sections" "-fdata-sections" "-g" "-fno-omit-frame-pointer" "-m32" "-mthreads" "-mtune=generic" "-O2" "-pipe" "-I" "pcre2/src" "-I" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\include" "-DPCRE2_CODE_UNIT_WIDTH=8" "-DHAVE_STDLIB_H=1" "-DHAVE_MEMMOVE=1" "-DHEAP_LIMIT=20000000" "-DLINK_SIZE=2" "-DMATCH_LIMIT=10000000" "-DMATCH_LIMIT_DEPTH=10000000" "-DMAX_NAME_COUNT=10000" "-DMAX_NAME_SIZE=32" "-DNEWLINE_DEFAULT=2" "-DPARENS_NEST_LIMIT=250" "-DPCRE2_STATIC=1" "-DSTDC_HEADERS=1" "-DSUPPORT_PCRE2_8=1" "-DSUPPORT_UNICODE=1" "-DHAVE_WINDOWS_H=1" "-DSUPPORT_JIT=1" "-o" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\pcre2/src\\pcre2_auto_possess.o" "-c" "pcre2/src\\pcre2_auto_possess.c"
  exit code: 1

  --- stderr
  fatal: not a git repository (or any of the parent directories): .git


  error occurred: Command "ccache" "gcc" "-O3" "-ffunction-sections" "-fdata-sections" "-g" "-fno-omit-frame-pointer" "-m32" "-mthreads" "-mtune=generic" "-O2" "-pipe" "-I" "pcre2/src" "-I" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\include" "-DPCRE2_CODE_UNIT_WIDTH=8" "-DHAVE_STDLIB_H=1" "-DHAVE_MEMMOVE=1" "-DHEAP_LIMIT=20000000" "-DLINK_SIZE=2" "-DMATCH_LIMIT=10000000" "-DMATCH_LIMIT_DEPTH=10000000" "-DMAX_NAME_COUNT=10000" "-DMAX_NAME_SIZE=32" "-DNEWLINE_DEFAULT=2" "-DPARENS_NEST_LIMIT=250" "-DPCRE2_STATIC=1" "-DSTDC_HEADERS=1" "-DSUPPORT_PCRE2_8=1" "-DSUPPORT_UNICODE=1" "-DHAVE_WINDOWS_H=1" "-DSUPPORT_JIT=1" "-o" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\src\\pcre2_chartables.o" "-c" "E:\\MABS\\build\\ripgrep-git\\target\\i686-pc-windows-gnu\\release\\build\\pcre2-sys-7567a6bd01fdc6f7\\out\\src\\pcre2_chartables.c" with args "gcc" did not execute successfully (status code exit code: 1).
```

#### What are the steps to reproduce the behavior?

compiling with rust and gcc 11 in media-autobuild_suite as verbosely logged

#### What is the actual behavior?

see above, and [logs.zip](https://0x0.st/oa3a.zip) for more details

#### What is the expected behavior?

Successful compilation.


---

_Comment by @BurntSushi on 2022-05-24 13:10_

This isn't a problem with ripgrep or one of the crates maintained in its tree. I moved this issue to the rust-pcre2 repo: https://github.com/BurntSushi/rust-pcre2/issues/24

---

_Closed by @BurntSushi on 2022-05-24 13:10_

---

_Label `invalid` added by @BurntSushi on 2022-05-24 13:10_

---
