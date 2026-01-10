```yaml
number: 9245
title: "[msys2] fails to build on GCC-based environments"
type: issue
state: closed
author: ognevny
labels: []
assignees: []
created_at: 2023-12-22T13:04:41Z
updated_at: 2024-02-17T18:11:17Z
url: https://github.com/astral-sh/ruff/issues/9245
synced_at: 2026-01-10T11:09:51Z
```

# [msys2] fails to build on GCC-based environments

---

_Issue opened by @ognevny on 2023-12-22 13:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
[build script](https://github.com/msys2/MINGW-packages/blob/72a78f9dce392292022aa6f5ad2f7d01e1560666/mingw-w64-ruff/PKGBUILD) for version 0.1.9 (package ruff_cli) fails with

<details>

```
  error: linking with `x86_64-w64-mingw32-gcc` failed: exit code: 1
    |
    = note: "x86_64-w64-mingw32-gcc" "-fno-use-linker-plugin" "-Wl,--dynamicbase" "-Wl,--disable-auto-image-base" "-m64" "-Wl,--high-entropy-va" <a huge number of files invoked>
    = note: D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x2043): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x2113): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x220b): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x22de): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x2372): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.aho_corasick-bc7bfad95b99dc10.aho_corasick.b96c42f095208567-cgu.07.rcgu.o.rcgu.o:aho_corasick.b96c4:(.text+0x2405): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.libcst_native-729622e976b52f45.libcst_native.4faaf57094d1fd9-cgu.12.rcgu.o.rcgu.o:libcst_native.4faa:(.text+0x10d7): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.libcst_native-729622e976b52f45.libcst_native.4faaf57094d1fd9-cgu.12.rcgu.o.rcgu.o:libcst_native.4faa:(.text+0x1c98): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex-4f57ff077656d547.regex.52d01696f5b97498-cgu.03.rcgu.o.rcgu.o:regex.52d01696f5b9:(.text+0x8d8): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x5d7e): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x5fb9): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x60a3): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x63de): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x64d9): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x66b3): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x6c56): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x6d28): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x6e01): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x6ed4): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x7495): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x75aa): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x76a2): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x78e8): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.02.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x7e17): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.10.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x636e): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.10.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x64a2): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr2_raw2FN17h7e29201c65c3cb9bE'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.10.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x65f5): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr11memchr3_raw2FN17ha0437855073b6886E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.14.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x1990): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.14.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x19c3): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.regex_automata-68811d2bf19a347d.regex_automata.cf7c7b0cc1719559-cgu.14.rcgu.o.rcgu.o:regex_automata.cf7:(.text+0x1cc6): undefined reference to `__imp__ZN6memchr4arch6x86_646memchr10memchr_raw2FN17h6e22d7878904c6e1E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.tracing-9c7748c7eeb2656e.tracing.c906aa592a079469-cgu.01.rcgu.o.rcgu.o:tracing.c906aa592a:(.text+0x85): undefined reference to `__imp__ZN12tracing_core10dispatcher15GLOBAL_DISPATCH17h25fad95899cc9180E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.tracing-9c7748c7eeb2656e.tracing.c906aa592a079469-cgu.01.rcgu.o.rcgu.o:tracing.c906aa592a:(.text+0xa1): undefined reference to `__imp__ZN12tracing_core10dispatcher15GLOBAL_DISPATCH17h25fad95899cc9180E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.tracing-9c7748c7eeb2656e.tracing.c906aa592a079469-cgu.01.rcgu.o.rcgu.o:tracing.c906aa592a:(.text+0x275): undefined reference to `__imp__ZN12tracing_core10dispatcher15GLOBAL_DISPATCH17h25fad95899cc9180E'
            D:/M/msys64/ucrt64/bin/../lib/gcc/x86_64-w64-mingw32/13.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\_\B\src\ruff-0.1.9\target\release\deps\ruff-7982543447b9eecc.tracing-9c7748c7eeb2656e.tracing.c906aa592a079469-cgu.01.rcgu.o.rcgu.o:tracing.c906aa592a:(.text+0x2a0): undefined reference to `__imp__ZN12tracing_core10dispatcher15GLOBAL_DISPATCH17h25fad95899cc9180E'
            collect2.exe: error: ld returned 1 exit status
            
    = note: some `extern` functions couldn't be found; some native libraries may need to be installed or have their path specified
    = note: use the `-l` flag to specify native libraries to link
    = note: use the `cargo:rustc-link-lib` directive to specify the native libraries to link with Cargo (see https://doc.rust-lang.org/cargo/reference/build-scripts.html#cargorustc-link-libkindname)
  
  error: could not compile `ruff_cli` (bin "ruff") due to previous error
  💥 maturin failed
    Caused by: Failed to build a native library through cargo
    Caused by: Cargo build finished with "exit code: 101": `"cargo" "rustc" "--all-features" "--message-format" "json-render-diagnostics" "--locked" "--manifest-path" "C:\\_\\B\\src\\ruff-0.1.9\\crates\\ruff_cli\\Cargo.toml" "--release" "--bin" "ruff" "--" "-C" "link-arg=-s"`
```

see PR actions for full log

</details>

on GCC-based environments (MINGW64 and UCRT64). clang-based environment (CLANG64) work fine

---

_Renamed from "[msys2] failed to build on GCC-based environments" to "[msys2] fails to build on GCC-based environments" by @ognevny on 2023-12-22 14:16_

---

_Comment by @BurntSushi on 2024-01-04 12:32_

This looks like something wrong in the environment, or more likely, something wrong in the toolchain. From what I can see, you're getting linker errors from Rust symbols. That generally shouldn't happen AIUI. But to be honest, I don't know much about the `x86_64-w64-mingw32-gcc` target.

For reference, one of the functions in the linker error is [this one](https://github.com/BurntSushi/memchr/blob/cedf318090876c6d557f234b158dd4fdc91c41ec/src/arch/x86_64/memchr.rs#L174-L178). And the only way that function isn't defined is [if `target_os != "x86_64"`](https://github.com/BurntSushi/memchr/blob/cedf318090876c6d557f234b158dd4fdc91c41ec/src/arch/mod.rs#L15).

One thing that catches my eye is that the target seems to be 64-bit, but it also has a "32" in the name via `mingw32`. I wonder if there is some significance to that.

The `memchr` crate is widely use in the Rust ecosystem. Do you have any trouble building other Rust programs for the same target?

---

_Comment by @ognevny on 2024-01-04 15:05_

`mingw32` is just the name like `win32`. msys2 uses mingw-w64, which supports 64-bit of course. in msys2 we actively maintain rust toolchain and we already have dozens of packages compiled with rust. but there is no `x86_64-w64-mingw32` target, so we use some hacks while building rust. we didn't encounter any issues with `memchr` target

---

_Comment by @BurntSushi on 2024-01-04 15:09_

Yeah I dunno. I'm also the maintainer of `memchr`, `regex-automata` and `aho-corasick`. The linker errors for Rust symbols definitely lead me to guess that the issue is not on our/my end. But I don't have any guesses beyond that unfortunately.

---

_Comment by @mati865 on 2024-01-04 22:02_

> I don't know much about the x86_64-w64-mingw32-gcc target.

> here is no x86_64-w64-mingw32 target, so we use some hacks while building rust

There are no hacks involved, x86_64-w64-mingw32 is GNU name for this target and x86_64-pc-windows-gnu is LLVM name (inherited by Rust) but they mean the same thing.

Looking at the logs it looks like the statics might incorrectly imported/exported or the link order is wrong but that shouldn't be caused by these crates. It's likely an issue with rustc but I won't have time to debug it soon.

---

_Comment by @ognevny on 2024-01-04 22:15_

> Looking at the logs it looks like the statics might incorrectly imported/exported or the link order is wrong but that shouldn't be caused by these crates. It's likely an issue with rustc but I won't have time to debug it soon.

I haven't tested ruff 0.1.11 yet and also didn't try to rebuild with rust 1.75.0. I can do this only on January 8th

---

_Comment by @mati865 on 2024-01-04 22:17_

I have tested current git repo with 1.74 and 1.75, they both have the same issue.

---

_Comment by @mati865 on 2024-01-04 22:44_

It has "regressed" in https://github.com/astral-sh/ruff/commit/3ce145c47669645c9031ec42ef9b339835e548c9 and changing `lto = "thin"` to `lto = "fat"`  makes it build which sounds like https://github.com/rust-lang/rust/issues/109797

---

_Comment by @ognevny on 2024-01-05 00:31_

> It has "regressed" in https://github.com/astral-sh/ruff/commit/3ce145c47669645c9031ec42ef9b339835e548c9 and changing `lto = "thin"` to `lto = "fat"`  makes it build which sounds like https://github.com/rust-lang/rust/issues/109797

thanks for investigation!

---

_Comment by @ognevny on 2024-01-08 08:41_

we fixed the issue. can be closed

---

_Closed by @ognevny on 2024-01-08 08:41_

---

_Comment by @cr1901 on 2024-02-17 18:11_

I ran into this exact error while trying to add `ruff` as a dependency to a `pdm` project. This is a virtual/isolated environment, and I _have_ to do a source build due to wheels not being provided for mingw by PyPI. I also wanted to avoid the need to fork the `ruff` repo just to tweak the `release` profile to remove thin LTO (and manually track new versions).

Since [per-target profiles](https://github.com/rust-lang/cargo/issues/4897) aren't really a thing, I was able to get around this issue by setting the environment variable `CARGO_PROFILE_RELEASE_LTO` to `false`. This'll work for me for now until https://github.com/rust-lang/rust/issues/109797 is fixed.

---
