```yaml
number: 18016
title: "How to fix ERROR: Failed to build installable wheels for some pyproject.toml based projects (ruff)?"
type: issue
state: closed
author: ZeusKC52
labels: []
assignees: []
created_at: 2025-05-11T10:28:36Z
updated_at: 2025-05-12T12:41:01Z
url: https://github.com/astral-sh/ruff/issues/18016
synced_at: 2026-01-12T15:54:56Z
```

# How to fix ERROR: Failed to build installable wheels for some pyproject.toml based projects (ruff)?

---

_@ZeusKC52_

### Summary

Im on:
Android 14
Termux 0.118.2
Python 3.12
'+fpmr' is not a recognized feature for this target (ignoring feature) keeps repeating to the top 

Log:
```
    '+fpmr' is not a recognized feature for this target (ignoring feature)
      error: linking with `cc` failed: exit status: 1
        |                                             = note:  "cc" "<sysroot>/tmp/rustchSEfy4/symbols.o" "<2076 object files omitted>" "-Wl,--as-needed" "-Wl,-Bstatic" "<sysroot>/tmp/rustchSEfy4/{libtikv_jemalloc_sys-*}.rlib" "<sysroot>/lib/rustlib/aarch64-linux-android/lib/{libcompiler_builtins-*}.rlib" "-Wl,-Bdynamic" "-lgcc" "-ldl" "-llog" "-lunwind" "-ldl" "-lm" "-lc" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-L" "<sysroot>/tmp/pip-install-irodj_gr/ruff_7045cececff84bf89f0f391ee559e6c1/target/release/build/tikv-jemalloc-sys-a3c5993bb663de14/out/build/lib" "-L" "<sysroot>/tmp/pip-install-irodj_gr/ruff_7045cececff84bf89f0f391ee559e6c1/target/release/build/tikv-jemalloc-sys-a3c5993bb663de14/out" "-o" "<sysroot>/tmp/pip-install-irodj_gr/ruff_7045cececff84bf89f0f391ee559e6c1/target/release/deps/ruff-b62721e44b326d19" "-Wl,--gc-sections" "-pie" "-Wl,-z,relro,-z,now" "-Wl,-O1" "-Wl,--strip-all" "-nodefaultlibs"                                                   = note: some arguments are omitted. use `--verbose` to show all linker arguments            = note: ld.lld: error: unable to find library -lgcc                                                 cc: error: linker command failed with exit code 1 (use -v to see invocation)
                                                    error: could not compile `ruff` (bin "ruff") due to 1 previous error                        💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit status: 101": `env -u CARGO "cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "/data/data/com.termux/files/usr/tmp/pip-install-irodj_gr/ruff_7045cececff84bf89f0f391ee559e6c1/crates/ruff/Cargo.toml" "--release" "--bin" "ruff" "--" "-C" "strip=symbols"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/data/data/com.termux/files/usr/bin/python3.12', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]                         
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for ruff       Failed to build ruff
ERROR: Failed to build installable wheels for some pyproject.toml based projects (ruff)
```

### Version

0.11.9

---

_Comment by @charliermarsh on 2025-05-11 11:37_

`unable to find library -lgcc` suggests that you're missing `libgcc`. You might need something like `pkg install libgcc` or similar.

---

_Comment by @ZeusKC52 on 2025-05-11 12:11_

Tried pkg install libgcc but it still doesnt work.

Full log:
```
 $ pkg install libgcc                        No mirror or mirror group selected. You might want to select one by running 'termux-change-repo'
Checking availability of current mirror:      [*] https://mirror.mwt.me/termux/main: bad
Testing the available mirrors:                [*] (10) https://packages-cf.termux.dev/apt/termux-main: bad                                [*] (1) https://mirror.bardia.tech/termux/termux-main: bad                                  [*] (1) https://mirror.jeonnam.school/termux/termux-main: bad                               [*] (1) https://tmx.xvx.my.id/apt/termux-main: bad                                          [*] (1) https://mirrors.krnk.org/apt/termux/termux-main: bad                                [*] (1) https://mirror.rinarin.dev/termux/termux-main: bad                                  [*] (1) https://mirrors.ravidwivedi.in/termux/termux-main: ok                               [*] (1) https://mirror.textcord.xyz/termux/termux-main: bad                                 [*] (1) https://mirror.nevacloud.com/applications/termux/termux-main: bad                   [*] (1) https://mirror.twds.com.tw/termux/termux-main: bad                                  [*] (1) https://mirror.albony.in/termux/termux-main: ok                                     [*] (1) https://mirrors.cbrx.io/apt/termux/termux-main: ok                                  [*] (1) https://termux.niranjan.co/termux-main: ok                                          [*] (1) https://mirror.freedif.org/termux/termux-main: ok                                   [*] (1) https://mirrors.nguyenhoang.cloud/termux/termux-main: ok                            [*] (1) https://mirrors.in.sahilister.net/termux/termux-main/: ok                           [*] (1) https://linux.domainesia.com/applications/termux/termux-main: ok                    [*] (1) https://mirror.meowsmp.net/termux/termux-main: ok                                   [*] (1) https://mirrors.saswata.cc/termux/termux-main: ok                                   [*] (1) https://mirrors.sustech.edu.cn/termux/apt/termux-main: ok                           [*] (1) https://mirrors.tuna.tsinghua.edu.cn/termux/apt/termux-main: ok                     [*] (1) https://mirror.nyist.edu.cn/termux/apt/termux-main: ok                              [*] (1) https://mirrors.nju.edu.cn/termux/apt/termux-main: ok                               [*] (1) https://mirrors.hust.edu.cn/termux/apt/termux-main: bad                             [*] (1) https://mirror.sjtu.edu.cn/termux/termux-main/: ok                                  [*] (1) https://mirror.iscas.ac.cn/termux/apt/termux-main: ok                               [*] (1) https://mirrors.sdu.edu.cn/termux/termux-main: ok                                   [*] (1) https://mirrors.cqupt.edu.cn/termux/termux-main: ok                                 [*] (1) https://mirrors.bfsu.edu.cn/termux/apt/termux-main: ok                              [*] (1) https://mirrors.aliyun.com/termux/termux-main: ok                                   [*] (1) https://mirrors.sau.edu.cn/termux/apt/termux-main: ok                               [*] (1) https://mirrors.ustc.edu.cn/termux/termux-main: ok                                  [*] (1) https://mirrors.zju.edu.cn/termux/apt/termux-main: ok                               [*] (1) https://mirrors.cernet.edu.cn/termux/apt/termux-main: ok                            [*] (1) https://mirrors.pku.edu.cn/termux/termux-main/: bad                                 [*] (1) https://ro.mirror.flokinet.net/termux/termux-main: ok                               [*] (1) https://mirrors.medzik.dev/termux/termux-main: ok                                   [*] (1) https://mirror.accum.se/mirror/termux.dev/termux-main: ok                           [*] (1) https://is.mirror.flokinet.net/termux/termux-main: ok                               [*] (1) https://mirror.bouwhuis.network/termux/termux-main: ok                              [*] (1) https://mirror.mwt.me/termux/main: ok
[*] (4) https://grimler.se/termux/termux-main: ok
[*] (1) https://termux.librehat.com/apt/termux-main: ok
[*] (1) https://mirror.termux.dev/termux-main: bad
[*] (1) https://mirror.sunred.org/termux/termux-main: ok
[*] (1) https://mirror.polido.pt/termux/termux-main: bad
[*] (1) https://mirrors.de.sahilister.net/termux/termux-main: ok
[*] (1) https://ftp.fau.de/termux/termux-main: ok
[*] (1) https://md.mirrors.hacktegic.com/termux/termux-main: ok
[*] (1) https://ftp.agdsn.de/termux/termux-main: ok
[*] (1) https://mirror.autkin.net/termux/termux-main: ok
[*] (1) https://termux.mentality.rip/termux-main: ok
[*] (1) https://mirror.leitecastro.com/termux/termux-main: ok
[*] (1) https://packages.termux.dev/apt/termux-main: ok
[*] (1) https://termux.cdn.lumito.net/termux-main: ok
[*] (1) https://nl.mirror.flokinet.net/termux/termux-main: ok
[*] (1) https://termux.3san.dev/termux/termux-main: ok
[*] (1) https://mirrors.cfe.re/termux/termux-main: bad
[*] (1) https://gnlug.org/pub/termux/termux-main: ok
[*] (1) https://mirror.fcix.net/termux/termux-main: ok
[*] (1) https://mirrors.utermux.dev/termux/termux-main: ok
[*] (1) https://termux.danyael.xyz/termux/termux-main: ok
[*] (1) https://dl.kcubeterm.com/termux-main: bad
[*] (1) https://mirror.quantum5.ca/termux/termux-main: ok
[*] (1) https://mirror.mwt.me/termux/main: ok [*] (1) https://mirror.vern.cc/termux/termux-main: ok                                       [*] (1) https://mirror.csclub.uwaterloo.ca/termux/termux-main: ok                           [*] (1) https://plug-mirror.rcac.purdue.edu/termux/termux-main: ok                          [*] (1) https://mirrors.middlendian.com/termux/termux-main: ok                              [*] (1) http://mirror.mephi.ru/termux/termux-main: ok                                       [*] (1) https://repository.su/termux/termux-main/: ok                                       Picking mirror: (14) /data/data/com.termux/files/usr/etc/termux/mirrors/chinese_mainland/mirror.sjtu.edu.cn
Hit:1 https://turdl.kcubeterm.com tur-packages InRelease
Get:2 https://mirrors.hit.edu.cn/termux/termux-main stable InRelease [14.0 kB]
Get:3 https://mirrors.hit.edu.cn/termux/termux-root root InRelease [14.2 kB]
Get:4 https://mirrors.hit.edu.cn/termux/termux-x11 x11 InRelease [14.0 kB]
Get:5 https://mirrors.hit.edu.cn/termux/termux-main stable/main aarch64 Packages [538 kB]
Get:6 https://mirrors.hit.edu.cn/termux/termux-main stable/main aarch64 Contents (deb) [2782 kB]                                          Get:7 https://mirrors.hit.edu.cn/termux/termux-root root/stable aarch64 Packages [20.5 kB]  Get:8 https://mirrors.hit.edu.cn/termux/termux-root root/stable aarch64 Contents (deb) [25.0 kB]
Get:9 https://mirrors.hit.edu.cn/termux/termux-x11 x11/main aarch64 Packages [161 kB]
Get:10 https://mirrors.hit.edu.cn/termux/termux-x11 x11/main aarch64 Contents (deb) [1957 kB]                                             Fetched 5526 kB in 59s (93.6 kB/s)
Reading package lists... Done                 Building dependency tree... Done
Reading state information... Done             All packages are up to date.
Reading package lists... Done                 Building dependency tree... Done
Reading state information... Done             Package libgcc is not available, but is referred to by another package.                     This may mean that the package is missing, has been obsoleted, or                           is only available from another source
However the following packages replace it:      ndk-sysroot
                                              E: Package 'libgcc' has no installation candidate
```


---

_Comment by @ntBre on 2025-05-11 17:18_

Related: https://github.com/astral-sh/ruff/issues/17527

---

_Comment by @MichaReiser on 2025-05-12 07:10_

It seems ruff has become somewhat popular on android. Let's disable jemalloc for now

---

_Comment by @ZeusKC52 on 2025-05-12 07:16_

What command do i need to run to fix my issue?

---

_Closed by @MichaReiser on 2025-05-12 12:41_

---
