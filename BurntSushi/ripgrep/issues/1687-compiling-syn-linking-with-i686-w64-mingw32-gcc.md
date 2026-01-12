```yaml
number: 1687
title: "compiling syn: linking with `i686-w64-mingw32-gcc` failed - undefined reference to `__mingw_vfprintf'"
type: issue
state: closed
author: LigH-de
labels: []
assignees: []
created_at: 2020-09-25T08:17:03Z
updated_at: 2020-09-25T17:08:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1687
synced_at: 2026-01-12T16:13:24Z
```

# compiling syn: linking with `i686-w64-mingw32-gcc` failed - undefined reference to `__mingw_vfprintf'

---

_@LigH-de_

#### What version of ripgrep are you using?

origin/HEAD def993b

#### How did you install ripgrep?

https://github.com/m-ab-s/media-autobuild_suite

#### What operating system are you using ripgrep on?

Windows 7 SP1, MSYS2 + MinGW

#### Describe your bug.

Compilation fails. Logs attached.

#### What are the steps to reproduce the behavior?

Run media-autobuild suite when configured to build ripgrep too.

#### What is the actual behavior?

**ab-suite.rust.build.log**
```
CPPFLAGS: -D_FORTIFY_SOURCE=0 -D__USE_MINGW_ANSI_STDIO=1
CFLAGS: -mthreads -mtune=generic -O2 -pipe
CXXFLAGS: -mthreads -mtune=generic -O2 -pipe
LDFLAGS: -pipe -static-libgcc -static-libstdc++
/opt/cargo/bin/cargo.exe build --release --target=i686-pc-windows-gnu --jobs=2 --features pcre2
   Compiling syn v1.0.41
   Compiling pcre2-sys v0.2.5
error: linking with `i686-w64-mingw32-gcc` failed: exit code: 1
  |
  = note: "i686-w64-mingw32-gcc" "-fno-use-linker-plugin" "-Wl,--nxcompat" "-Wl,--large-address-aware" "-nostartfiles" "E:\\MABS\\msys64\\mingw32\\i686-w64-mingw32\\lib\\crt2.o" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\rsbegin.o" "-L" "E:\\MABS\\msys64\\mingw32\\i686-w64-mingw32\\lib" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\self-contained" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.0.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.1.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.10.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.11.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.12.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.13.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.14.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.15.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.2.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.3.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.4.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.5.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.6.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.7.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.8.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.build_script_build.bq7etohi-cgu.9.rcgu.o" "-o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.exe" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\syn-f38b2749f2107231\\build_script_build-f38b2749f2107231.2mexlhlygsunlp05.rcgu.o" "-Wl,--gc-sections" "-nodefaultlibs" "-L" "E:\\MABS\\build\\ripgrep-git\\target\\release\\deps" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib" "-Wl,--start-group" "-Wl,-Bstatic" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libstd-e4e40256d0c086c9.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libpanic_unwind-8e02fc67161a7d8f.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libhashbrown-33804d6248981d68.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_std_workspace_alloc-057cb2a83ed8d602.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libbacktrace-951976a2e108e12f.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libbacktrace_sys-91e9eefa1109c155.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_demangle-bf0cbd257613d152.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libunwind-c34988a92fa8dc0e.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcfg_if-17273c5475206f25.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\liblibc-fb8eecdcaa771961.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\liballoc-5cf9e879ba55ea4b.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_std_workspace_core-e16b6d2c85c40d6b.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcore-b978dcbc96dc6d47.rlib" "-Wl,--end-group" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcompiler_builtins-c6570c2846c80c80.rlib" "-Wl,-Bdynamic" "-ladvapi32" "-lws2_32" "-luserenv" "-lmsvcrt" "-lmingwex" "-lmingw32" "-lmsvcrt" "-luser32" "-lkernel32" "-lgcc_eh" "-l:libpthread.a" "-lgcc" "-lmsvcrt" "-lkernel32" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\rsend.o"
  = note: E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-thread.o):(.text+0xb5): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-thread.o):(.text+0xdb): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-rwlock.o):(.text+0x25): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-rwlock.o):(.text+0x13b): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-cond.o):(.text+0x1b): undefined reference to `__mingw_vfprintf'
          

error: aborting due to previous error

error: could not compile `syn`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: linking with `i686-w64-mingw32-gcc` failed: exit code: 1
  |
  = note: "i686-w64-mingw32-gcc" "-fno-use-linker-plugin" "-Wl,--nxcompat" "-Wl,--large-address-aware" "-nostartfiles" "E:\\MABS\\msys64\\mingw32\\i686-w64-mingw32\\lib\\crt2.o" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\rsbegin.o" "-L" "E:\\MABS\\msys64\\mingw32\\i686-w64-mingw32\\lib" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\self-contained" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.0.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.1.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.10.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.11.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.12.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.13.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.14.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.15.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.2.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.3.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.4.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.5.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.6.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.7.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.8.rcgu.o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.build_script_build.9loufjrj-cgu.9.rcgu.o" "-o" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.exe" "E:\\MABS\\build\\ripgrep-git\\target\\release\\build\\pcre2-sys-f19ee9bc6d310a2e\\build_script_build-f19ee9bc6d310a2e.38i7vocrwi1sa5du.rcgu.o" "-Wl,--gc-sections" "-nodefaultlibs" "-L" "E:\\MABS\\build\\ripgrep-git\\target\\release\\deps" "-L" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib" "-Wl,-Bstatic" "E:\\MABS\\build\\ripgrep-git\\target\\release\\deps\\libpkg_config-e579756db0e83855.rlib" "E:\\MABS\\build\\ripgrep-git\\target\\release\\deps\\libcc-e7d9ff9a68884922.rlib" "E:\\MABS\\build\\ripgrep-git\\target\\release\\deps\\libjobserver-c96f095627a94b13.rlib" "-Wl,--start-group" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libstd-e4e40256d0c086c9.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libpanic_unwind-8e02fc67161a7d8f.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libhashbrown-33804d6248981d68.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_std_workspace_alloc-057cb2a83ed8d602.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libbacktrace-951976a2e108e12f.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libbacktrace_sys-91e9eefa1109c155.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_demangle-bf0cbd257613d152.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libunwind-c34988a92fa8dc0e.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcfg_if-17273c5475206f25.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\liblibc-fb8eecdcaa771961.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\liballoc-5cf9e879ba55ea4b.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\librustc_std_workspace_core-e16b6d2c85c40d6b.rlib" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcore-b978dcbc96dc6d47.rlib" "-Wl,--end-group" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\libcompiler_builtins-c6570c2846c80c80.rlib" "-Wl,-Bdynamic" "-ladvapi32" "-lole32" "-loleaut32" "-ladvapi32" "-lws2_32" "-luserenv" "-lmsvcrt" "-lmingwex" "-lmingw32" "-lmsvcrt" "-luser32" "-lkernel32" "-lgcc_eh" "-l:libpthread.a" "-lgcc" "-lmsvcrt" "-lkernel32" "E:\\MABS\\msys64\\opt\\cargo\\toolchains\\stable-i686-pc-windows-gnu\\lib\\rustlib\\i686-pc-windows-gnu\\lib\\rsend.o"
  = note: E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-thread.o):(.text+0xb5): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-thread.o):(.text+0xdb): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-rwlock.o):(.text+0x25): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-rwlock.o):(.text+0x13b): undefined reference to `__mingw_vfprintf'
          E:\MABS\msys64\mingw32\i686-w64-mingw32\lib\libpthread.a(libwinpthread_la-cond.o):(.text+0x1b): undefined reference to `__mingw_vfprintf'
          

error: aborting due to previous error

error: build failed
```

#### What is the expected behavior?

*Configure, build, install* analogues in a rust environment.


[Log file archive](https://github.com/BurntSushi/ripgrep/files/5281420/logs.zip) collected by media-autobuild suite



---

_Comment by @BurntSushi on 2020-09-25 11:16_

You didn't say what command run. Why not? It's critical information. Please try building ripgrep without PCRE2 support to see if that works.

Otherwise, you're using an old version of Windows and I do not have the resources to debug it. So you may be on your own. Sorry.

---

_Comment by @LigH-de on 2020-09-25 13:59_

I did not run any manual command. I let a pre-built batch script run. The MSYS2 environment is kept up-to-date by this suite before retrieving most recent sources of all related libraries and compiling them automatedly. It should run on Windows 7, 8, 10 without much difference, because most of it works in MSYS2 GNU-like shells.

I don't expect you to solve this problem completely, but a few thoughts may be helpful, like: Did your project have recent changes which may suddenly require different compilation parameters, or libraries an MSYS2 environment may not usually ship? Or does this error look like it is caused not by your project but by a dependency? I am not even a developer. I am only a halfwit running that suite and learning some experience in interpreting recurring error types.

My current level of experience suggests me to remove cargo completely and let it install from scratch before trying again... would you agree?

---

_Comment by @BurntSushi on 2020-09-25 14:07_

> I did not run any manual command.

I need the cargo command you used to compile ripgrep. Whether you ran it manually or not doesn't matter. For me to diagnose issues, I need to know how ripgrep is being built. And in particular, this should also allow you to build without PCRE2, which may be your problem.

> Did your project have recent changes which may suddenly require different compilation parameters, or libraries an MSYS2 environment may not usually ship? Or does this error look like it is caused not by your project but by a dependency? I am not even a developer. I am only a halfwit running that suite and learning some experience in interpreting recurring error types.

No, there were no recent relevant changes. You should be able to test this by checking out a previous version of ripgrep and building that. The instructions for doing so are in the README.

My guess is that the problem is being caused by attempting to compile PCRE2, which is a C library regex engine. But it is not required to build ripgrep. Hence why I've suggested building without PCRE2.

If you're not able to compile ripgrep yourself and are instead using someone else's packaging tool to build ripgrep, then you should report build issues to the maintainer of the packaging tool, not me.

> My current level of experience suggests me to remove cargo completely and let it install from scratch before trying again... would you agree?

No, I don't see that helping. You can try though. Sometimes reinstalling stuff works, but from what I can see, cargo is working fine. It's the C compiler configuration that isn't. That may or may not be a problem in the `pcre2-sys` dependency or in something on your system.

---

_Comment by @LigH-de on 2020-09-25 14:29_

Well, I pasted the whole build log, which contains the following line:

`/opt/cargo/bin/cargo.exe build --release --target=i686-pc-windows-gnu --jobs=2 --features pcre2`

I will forward your remark about omitting pcre2 to the developers of the media-autobuild suite.

In the meantime, I postponed building ripgrep, and a quite similar problem occured when compiling cargo-c, required for rav1e (an AV1 video encoder). So there may be an additional confirmation about a general issue, not so much related to ripgrep alone.

---

_Comment by @BurntSushi on 2020-09-25 14:34_

OK, so  try:

```
/opt/cargo/bin/cargo.exe build --release --target=i686-pc-windows-gnu --jobs=2
```

And also try:

```
/opt/cargo/bin/cargo.exe build --release --jobs=2
```

Why try building for the GNU target anyway? Why not use MSVC?

> Well, I pasted the whole build log, which contains the following line:

Consider that I maintain many open source projects with issues filed every day. Please make it easier for maintainers to diagnose problems instead of attaching a zip file that I then have to download and search through.

---

_Comment by @LigH-de on 2020-09-25 14:39_

Why GNU? Because the media-autobuild suite is based on an MSYS2 / MinGW environment, which is installed by the suite and kept up-to-date on every run, automatically.

I pasted the build log into my starting post, directly readable. It's the long code block in the chapter: **What is the actual behavior?**. This log starts with the compiler related environment variables and the command line in the first five lines.

---

_Comment by @BurntSushi on 2020-09-25 14:41_

OK, sorry I didn't see that.

I still don't understand why you're using GNU, but okay.

Keep in mind that I am not a Windows user.

---

_Comment by @LigH-de on 2020-09-25 14:50_

I use the media-autobuild suite to compile a lot of audio and video encoders and decoders, up to a common ffmpeg.exe, for a Windows audience - and ripgrep is just an additional project along with these main targets. The original author of this suite will have had reasons to use an OpenSource, free-of-charge building environment. The choice is not mine, I can only use what is provided to me. Remember, I am not a C/C++ developer, and Rust even less.

Most of the times, the media-autobuild suite in this MSYS2 / MinGW environment works nicely (except for times when the projects to be compiled contain bugs). But I must admit that the "rust and cargo" base had been a reason for trouble several times, and reinstalling cargo solved those more than once before.

---

_Comment by @LigH-de on 2020-09-25 17:08_

Closing this issue as there is more evidence that the reason is outside this project.

---

_Closed by @LigH-de on 2020-09-25 17:08_

---
