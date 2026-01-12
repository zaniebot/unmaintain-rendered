```yaml
number: 2217
title: "Failed 'cargo test'"
type: issue
state: closed
author: gvanem
labels: []
assignees: []
created_at: 2022-05-19T10:09:18Z
updated_at: 2022-05-19T10:41:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2217
synced_at: 2026-01-12T16:13:24Z
```

# Failed 'cargo test'

---

_@gvanem_

#### What version of ripgrep are you using?
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

I built it myself. A `cargo build --release` works fine.

#### What operating system are you using ripgrep on?

Win-10. 21H1. Build 19043.1706.

#### Describe your bug.

Running a `cargo test --release`, causes this link failure:
``` 
....
Compiling ignore v0.4.18 (F:\MingW32\src\misc\ripgrep\crates\ignore)
Compiling grep-pcre2 v0.1.5 (F:\MingW32\src\misc\ripgrep\crates\pcre2)
error: linking with `link.exe` failed: exit code: 1120
  |
  = note: "F:\\gv\\vc_2022\\VC\\Tools\\MSVC\\14.31.31103\\bin\\HostX64\\x64\\link.exe" "/NOLOGO" "F:\\MingW32\\src\\misc\\ripgrep\\target\\release\\deps\\grep_pcre2-54cec357a768ae6a.grep_pcre2.53b17d51-cgu.0.rcgu.o" 
....

symbol pcre2_code_free_8 referenced in function _ZN4core3ptr37drop_in_place$LT$pcre2..ffi..Code$GT$17h2f9a7bb884d39615E
          libpcre2-1f713fde52e9d89a.rlib(pcre2-1f713fde52e9d89a.pcre2.b312f923-cgu.2.rcgu.o) : error LNK2001: 
unresolved externalsymbol pcre2_code_free_8
...
``` 

All in all, these 19 symbols are missing:
```
pcre2_code_free_8
pcre2_compile_context_free_8
pcre2_jit_stack_free_8
pcre2_match_data_free_8
pcre2_match_context_free_8
pcre2_compile_context_create_8
pcre2_set_newline_8
pcre2_compile_8
pcre2_jit_compile_8
pcre2_match_8
pcre2_pattern_info_8
pcre2_get_error_message_8
pcre2_config_8 referenced
pcre2_match_context_create_8
pcre2_match_data_create_from_pattern_8
pcre2_jit_stack_create_8
pcre2_jit_stack_assign_8
pcre2_get_ovector_pointer_8
pcre2_get_ovector_count_8
```

I even did a `rustup update` w/o any help.

---

_Comment by @BurntSushi on 2022-05-19 10:41_

This looks like an environment issue on Windows. Sadly, I'm not equipped to debug it as I don't use Windows.

I will say that I find it a little suspicious that it is trying to build with PCRE2 even though you didn't enable it. So if you truly are just running `cargo test --release` and aren't enabling the `pcre2` feature, then that's the first thing to figure out.

I'm moving this to a Discussion question.

---

_Locked by @ghost on 2022-05-19 10:41_

---
