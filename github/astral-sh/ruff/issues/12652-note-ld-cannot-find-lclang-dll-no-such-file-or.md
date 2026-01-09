---
number: 12652
title: "  = note: ld: cannot find -lclang.dll: No such file or directory"
type: issue
state: closed
author: zzy444626905
labels:
  - needs-info
assignees: []
created_at: 2024-08-03T06:44:15Z
updated_at: 2024-08-17T14:35:59Z
url: https://github.com/astral-sh/ruff/issues/12652
synced_at: 2026-01-07T13:12:15-06:00
---

#   = note: ld: cannot find -lclang.dll: No such file or directory

---

_Issue opened by @zzy444626905 on 2024-08-03 06:44_

```
$ cargo run
   Compiling opencv v0.92.2
error: linking with `x86_64-w64-mingw32-gcc` failed: exit code: 1
  |
  = note: "x86_64-w64-mingw32-gcc" "-fno-use-linker-plugin" "-Wl,--dynamicbase" "-Wl,--disable-auto-image-base" "-m64" "-Wl,--high-entropy-va" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-window
s-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\self-contained\\crt2.o" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\rsbegin.o" "D:\\msys6
4\\tmp\\rustcZ4J1xd\\symbols.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu
.00.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.01.rcgu.o" "D:\\msy
s64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.02.rcgu.o" "D:\\msys64\\home\\xxx
\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.03.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust
_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.04.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\deb
ug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.05.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d6
4bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.06.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\buil
d_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.07.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd
1cdb96880d2.build_script_build.b78453023d80483-cgu.08.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_sc
ript_build.b78453023d80483-cgu.09.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023
d80483-cgu.10.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.11.rcgu.o
" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.12.rcgu.o" "D:\\msys64\\home\
\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.13.rcgu.o" "D:\\msys64\\home\\xxx\\rust_pro
ject\\rust_opencv\\target\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.14.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\t
arget\\debug\\build\\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.build_script_build.b78453023d80483-cgu.15.rcgu.o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\
\opencv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.2almlr6amjw67vpyp1z7qtler.rcgu.o" "-L" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps" "-L" "C:\\Program Files\\LLVM\\
bin" "-L" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib" "-Wl,-Bstatic" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debu
g\\deps\\libcc-85db4cf0c8206bbc.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libvcpkg-d7522db353770f9f.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\targe
t\\debug\\deps\\libpkg_config-1ee1198b43a14d25.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libjobserver-f6669d15d28c77a8.rlib" "D:\\msys64\\home\\xxx\\rust_project\
\rust_opencv\\target\\debug\\deps\\libshlex-e9978f01f906cf82.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libopencv_binding_generator-8ee36120e1d704e7.rlib" "D:\\msys64\\h
ome\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libpercent_encoding-f0462e9bee78e064.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libregex-96e4cda395b3a661
.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libregex_automata-051ad8e6172fe64b.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libaho
_corasick-74e2a5f702e1d8eb.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libmemchr-93da46a28fe73e20.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\d
ebug\\deps\\libregex_syntax-dd31645e7ad43f85.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libdunce-a42cd4e85bb42457.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_
opencv\\target\\debug\\deps\\libclang-b1b7cc85d8c84336.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libclang_sys-3b10f284f8166e78.rlib" "D:\\msys64\\home\\xxx\\rust_
project\\rust_opencv\\target\\debug\\deps\\liblibc-32f599c7c1ecbdb5.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libglob-93d34da06783341d.rlib" "D:\\msys64\\home\\zyzhang6
0\\rust_project\\rust_opencv\\target\\debug\\deps\\libsemver-430f432c2ba4019c.rlib" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\deps\\libonce_cell-1f0efc4ac3b4e24f.rlib" "C:\\Users\\
xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libstd-89b8387febda0c20.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\li
b\\rustlib\\x86_64-pc-windows-gnu\\lib\\libpanic_unwind-e49d308a511124a7.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libobject-8add8
442f29bae56.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libmemchr-f10a1ae1e0fd94f2.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\
\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libaddr2line-102fbe2d614bfd6e.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-win
dows-gnu\\lib\\libgimli-283756bf941c8ba9.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\librustc_demangle-1ba74d68b4a79269.rlib" "C:\\U
sers\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libstd_detect-40afa140308ed188.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-wi
ndows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libhashbrown-9266a15a0945929a.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libru
stc_std_workspace_alloc-abea35ecd03c9c00.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libminiz_oxide-5e49ccf5f87efb7f.rlib" "C:\\User
s\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libadler-c5ce23adc9aaf90d.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gn
u\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libunwind-a9606ec264f90ede.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libcfg_if-cd26a2
6d92bbdaea.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\liblibc-3c768987b0a02293.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\st
able-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\liballoc-886bb6e740d7b2dd.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gn
u\\lib\\librustc_std_workspace_core-e19f8644ec22d631.rlib" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libcore-5092f9e1db0dd9aa.rlib" "C:\
\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\libcompiler_builtins-b2472419a0746f0c.rlib" "-Wl,-Bdynamic" "-lkernel32" "-ladvapi32" "-lole32" "-
loleaut32" "-lclang.dll" "-lkernel32" "-ladvapi32" "-lkernel32" "-lntdll" "-luserenv" "-lws2_32" "-lkernel32" "-lws2_32" "-lkernel32" "-lgcc_eh" "-l:libpthread.a" "-lmsvcrt" "-lmingwex" "-lmingw32" "-lgcc" "-lm
svcrt" "-lmingwex" "-luser32" "-lkernel32" "-Wl,--nxcompat" "-nostartfiles" "-L" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib" "-L" "C:\\Use
rs\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-windows-gnu\\lib\\self-contained" "-o" "D:\\msys64\\home\\xxx\\rust_project\\rust_opencv\\target\\debug\\build\\op
encv-d64bd1cdb96880d2\\build_script_build-d64bd1cdb96880d2.exe" "-Wl,--gc-sections" "-no-pie" "-nodefaultlibs" "C:\\Users\\xxx\\.rustup\\toolchains\\stable-x86_64-pc-windows-gnu\\lib\\rustlib\\x86_64-pc-w
indows-gnu\\lib\\rsend.o"
  = note: ld: cannot find -lclang.dll: No such file or directory
```

---

_Comment by @MichaReiser on 2024-08-03 06:54_

Can you share some additional information about what you're building: what's your platform? Why you want to build ruff from source. It's otherwise near to impossible to help you figure out the build error.

---

_Label `needs-info` added by @MichaReiser on 2024-08-03 06:54_

---

_Closed by @MichaReiser on 2024-08-17 14:35_

---
