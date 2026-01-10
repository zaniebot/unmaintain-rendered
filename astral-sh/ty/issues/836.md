```yaml
number: 836
title: "Panic `attempt to subtract with overflow` in `ruff_annotate_snippets/src/renderer/margin.rs`"
type: issue
state: open
author: qarmin
labels:
  - diagnostics
  - fatal
  - fuzzer
assignees: []
created_at: 2025-07-17T10:04:21Z
updated_at: 2025-12-05T13:08:13Z
url: https://github.com/astral-sh/ty/issues/836
synced_at: 2026-01-10T01:56:40Z
```

# Panic `attempt to subtract with overflow` in `ruff_annotate_snippets/src/renderer/margin.rs`

---

_Issue opened by @qarmin on 2025-07-17 10:04_

Quite similar to https://github.com/astral-sh/ty/issues/670

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
                                     'betting_env.datastructure.team_lineup.Tamheet.get_latest': ( 'dataStrcuture/team_lineup.htm#teamsheet.getlatest',
```

command
```
timeout -v 300 ty check TEST___FILE.py
```

cause this
```
thread 'main' panicked at crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16:
attempt to subtract with overflow
stack backtrace:
   0: __rustc::rust_begin_unwind
             at /rustc/3014e79f9c8d5510ea7b3a3b70d171d0948b1e96/library/std/src/panicking.rs:697:5
   1: core::panicking::panic_fmt
             at /rustc/3014e79f9c8d5510ea7b3a3b70d171d0948b1e96/library/core/src/panicking.rs:75:14
   2: core::panicking::panic_const::panic_const_sub_overflow
             at /rustc/3014e79f9c8d5510ea7b3a3b70d171d0948b1e96/library/core/src/panicking.rs:175:17
   3: ruff_annotate_snippets::renderer::margin::Margin::compute
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16
   4: ruff_annotate_snippets::renderer::margin::Margin::new
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/margin.rs:53:11
   5: ruff_annotate_snippets::renderer::display_list::format_body
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1645:18
   6: ruff_annotate_snippets::renderer::display_list::format_snippet
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1121:20
   7: ruff_annotate_snippets::renderer::display_list::format_message
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1029:19
   8: ruff_annotate_snippets::renderer::display_list::DisplayList::new
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:127:20
   9: ruff_annotate_snippets::renderer::Renderer::render
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/mod.rs:167:9
  10: <ruff_db::diagnostic::render::DisplayDiagnostics as core::fmt::Display>::fmt
             at ./ruff-main/crates/ruff_db/src/diagnostic/render.rs:175:52
  11: <ruff_db::diagnostic::render::DisplayDiagnostic as core::fmt::Display>::fmt
             at ./ruff-main/crates/ruff_db/src/diagnostic/render.rs:70:94
  12: core::fmt::rt::Argument::fmt
             at /rustc/3014e79f9c8d5510ea7b3a3b70d171d0948b1e96/library/core/src/fmt/rt.rs:173:76
  13: core::fmt::write
             at /rustc/3014e79f9c8d5510ea7b3a3b70d171d0948b1e96/library/core/src/fmt/mod.rs:1469:25
  14: <&mut W as core::fmt::Write::write_fmt::SpecWriteFmt>::spec_write_fmt
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/fmt/mod.rs:233:21
  15: core::fmt::Write::write_fmt
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/fmt/mod.rs:238:14
  16: ty::MainLoop::main_loop
             at ./ruff-main/crates/ty/src/lib.rs:326:37
  17: ty::MainLoop::run
             at ./ruff-main/crates/ty/src/lib.rs:249:27
  18: ty::run_check
             at ./ruff-main/crates/ty/src/lib.rs:151:19
  19: ty::run
             at ./ruff-main/crates/ty/src/lib.rs:45:39
  20: ty::main
             at ./ruff-main/crates/ty/src/main.rs:6:5
  21: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:253:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

##### Automatic Fuzzer note, output status "Some(101)", output signal "None"
```

[b.py.zip](https://github.com/user-attachments/files/21292888/b.py.zip)

---

_Label `diagnostics` added by @MichaReiser on 2025-07-17 10:07_

---

_Comment by @MichaReiser on 2025-07-17 10:07_

CC: @BurntSushi 

---

_Label `fatal` added by @AlexWaygood on 2025-07-17 10:14_

---

_Comment by @BurntSushi on 2025-07-17 12:10_

Oof. Acknowledged. I'll put this on my goals for next week.

---

_Assigned to @BurntSushi by @BurntSushi on 2025-07-17 12:10_

---

_Comment by @qarmin on 2025-09-23 06:24_

It is fixed in latest ty version

---

_Closed by @qarmin on 2025-09-23 06:24_

---

_Comment by @qarmin on 2025-09-23 06:26_

Well, I checked wrong file - this still happens

---

_Reopened by @qarmin on 2025-09-23 06:26_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:08_

---
