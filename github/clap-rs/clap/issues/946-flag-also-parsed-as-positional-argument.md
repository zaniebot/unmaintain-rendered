---
number: 946
title: Flag also parsed as positional argument
type: issue
state: closed
author: Mark-Simulacrum
labels:
  - C-bug
assignees: []
created_at: 2017-05-05T04:02:05Z
updated_at: 2018-08-02T03:30:06Z
url: https://github.com/clap-rs/clap/issues/946
synced_at: 2026-01-07T13:12:19-06:00
---

# Flag also parsed as positional argument

---

_Issue opened by @Mark-Simulacrum on 2017-05-05 04:02_

### Rust Version

N/A, but tested on latest nightly

### Affected Version of clap

2.22.1

### Expected Behavior Summary

With a single, optional positional argument and a flag, we don't parse the flag as the positional argument. I also set `AllowLeadingHyphen`, since some of my arguments start with `-C`.

### Actual Behavior Summary

The optional positional argument is parsed as `Some("--verbose")` instead of `None`, and `--verbose` is also parsed.

### Steps to Reproduce the issue

I'm not really sure what to put here, but basically

`app --verbose` is parsed as:
 - `is_present("--verbose") = true`
 - `value_of("filter") == Some("--verbose")

### Sample Code or Link to Sample Code

https://github.com/Mark-Simulacrum/rust/tree/cleanup is the branch, but you'll need to cd to src/tools/compiletest and then just cargo build should work.

After that, running this command reproduces the issue, pay attention to how filter is printed as `Some("--verbose")` in the debug output:

`./src/target/debug/compiletest "--compile-lib-path" "./rust/build/x86_64-unknown-linux-gnu/stage1/lib" "--run-lib-path" "./rust/build/x86_64-unknown-linux-gnu/stage1/lib/rustlib/x86_64-unknown-linux-gnu/lib" "--rustc-path" "./rust/build/x86_64-unknown-linux-gnu/stage1/bin/rustc" "--rustdoc-path" "./rust/build/x86_64-unknown-linux-gnu/stage1/bin/rustdoc" "--src-base" "./rust/src/test/compile-fail" "--build-base" "./rust/build/x86_64-unknown-linux-gnu/test/compile-fail" "--stage-id" "stage1-x86_64-unknown-linux-gnu" "--mode" "compile-fail" "--target" "x86_64-unknown-linux-gnu" "--host" "x86_64-unknown-linux-gnu" "--llvm-filecheck" "./rust/build/x86_64-unknown-linux-gnu/llvm/build/bin/FileCheck" "--nodejs" "/usr/bin/node" "--host-rustcflags" "-Crpath -O" "--target-rustcflags" "-Crpath -O -Lnative=./rust/build/x86_64-unknown-linux-gnu/native/rust-test-helpers" "--docck-python" "/usr/bin/python2.7" "--lldb-python" "/usr/bin/python2.7" "--gdb" "/usr/bin/gdb" "--llvm-version" "4.0.1\n" "--verbose" "--cc" "" "--cxx" "" "--cflags" "" "--llvm-components" "" "--llvm-cxxflags" "" "--adb-path" "adb" "--adb-test-dir" "/data/tmp/work" "--android-cross-path" ""`

### Debug output

https://gist.github.com/Mark-Simulacrum/b86c4f16f922be7ef542e7853cf93e01, with what I think being relevant here: https://gist.github.com/Mark-Simulacrum/b86c4f16f922be7ef542e7853cf93e01#file-gistfile1-txt-L1590

---

_Comment by @kbknapp on 2017-05-06 18:12_

Thanks for taking the time to file this! I've confirmed the bug, and it should be an easy one to fix.

Also, just to note `AllowLeadingHyphen` is for when *values* may start with a hyphen, and should be used sparingly if possible. A better option is to use [`allow_hyphen_values`](https://docs.rs/clap/2.24.0/clap/struct.Arg.html#method.allow_hyphen_values) only on the args who's values may start with a leading hyphen. It's not critical, I just wanted to note it (and if you already knew this I apologize :smile:)

---

_Label `D: easy` added by @kbknapp on 2017-05-06 18:12_

---

_Label `P2: need to have` added by @kbknapp on 2017-05-06 18:12_

---

_Label `T: bug` added by @kbknapp on 2017-05-06 18:12_

---

_Label `W: 2.x` added by @kbknapp on 2017-05-06 18:12_

---

_Added to milestone `2.24.1` by @kbknapp on 2017-05-06 18:13_

---

_Comment by @Mark-Simulacrum on 2017-05-06 18:35_

Great! Thanks for pointing out that there's a difference between `AllowLeadingHypen` and `allow_hyphen_values`, but I think I'm confused (feel free to tell me I should move this to another issue). The documentation for [`AllowLeadingHyphen`](https://docs.rs/clap/2.24.0/clap/enum.AppSettings.html#variant.AllowLeadingHyphen) suggests that I *have to* set that globally: "NOTE: This can only be set application wide and not on a per argument basis." But what you're saying seems to suggest that's not the case? 

---

_Comment by @kbknapp on 2017-05-06 18:48_

> there's a difference between AllowLeadingHypen and allow_hyphen_values, but I think I'm confused

They do the same thing, one is globally (`AllowLeadingHyphen`) and the other is per argument (`allow_hyphen_values`).

The documentation is out of date (*embarrassed*). That was true before `allow_hyphen_values` was introduced. I'm updating it as we speak.

---

_Closed by @kbknapp on 2017-05-07 14:46_

---

_Comment by @kbknapp on 2017-05-07 14:47_

2.24.1 is on crates.io

---
