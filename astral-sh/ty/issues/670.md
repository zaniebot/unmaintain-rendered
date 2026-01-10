```yaml
number: 670
title: "Panic `attempt to subtract with overflow` in `crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - diagnostics
  - fatal
  - fuzzer
assignees: []
created_at: 2025-06-17T12:55:53Z
updated_at: 2025-12-05T13:08:56Z
url: https://github.com/astral-sh/ty/issues/670
synced_at: 2026-01-10T01:56:40Z
```

# Panic `attempt to subtract with overflow` in `crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16`

---

_Issue opened by @qarmin on 2025-06-17 12:55_

(Release build of ty with overflow-checks = true)

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
					s_data['d_dails'] = bb['contacted'][hostip]['ansible_facts']['ansible_devices']['vda']['vendor'] + " " + bb['contacted'][hostip]['an
```

command
```
timeout -v 300 ty check TEST___FILE.py
```

cause this
```
error[invalid-syntax]
 --> /opt/BROKEN_FILES_DIR/606_IDX_0_RAND_648625473186818115740701_minimized_101.py:1:1
  |
1 | ...   s_data['d_dails'] = bb['contacted'][hostip]['ansible_facts']['ansible_devices']['vda']['vendor'] + " " + bb['contacted'][hostip...
^ | Unexpected indentation
  |


WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.

thread 'main' panicked at crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16:
attempt to subtract with overflow
stack backtrace:
   0: __rustc::rust_begin_unwind
             at /rustc/45acf54eea118ed27927282b5e0bfdcd80b7987c/library/std/src/panicking.rs:697:5
   1: core::panicking::panic_fmt
             at /rustc/45acf54eea118ed27927282b5e0bfdcd80b7987c/library/core/src/panicking.rs:75:14
   2: core::panicking::panic_const::panic_const_sub_overflow
             at /rustc/45acf54eea118ed27927282b5e0bfdcd80b7987c/library/core/src/panicking.rs:175:17
   3: ruff_annotate_snippets::renderer::margin::Margin::compute
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/margin.rs:74:16
   4: ruff_annotate_snippets::renderer::margin::Margin::new
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/margin.rs:53:11
   5: ruff_annotate_snippets::renderer::display_list::format_body
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1648:18
   6: ruff_annotate_snippets::renderer::display_list::format_snippet
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1125:20
   7: ruff_annotate_snippets::renderer::display_list::format_message
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:1033:19
   8: ruff_annotate_snippets::renderer::display_list::DisplayList::new
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/display_list.rs:127:20
   9: ruff_annotate_snippets::renderer::Renderer::render
             at ./ruff-main/crates/ruff_annotate_snippets/src/renderer/mod.rs:167:9
  10: <ruff_db::diagnostic::render::DisplayDiagnostic as core::fmt::Display>::fmt
             at ./ruff-main/crates/ruff_db/src/diagnostic/render.rs:123:40
  11: core::fmt::rt::Argument::fmt
             at /rustc/45acf54eea118ed27927282b5e0bfdcd80b7987c/library/core/src/fmt/rt.rs:173:76
  12: core::fmt::write
             at /rustc/45acf54eea118ed27927282b5e0bfdcd80b7987c/library/core/src/fmt/mod.rs:1460:25
  13: std::io::default_write_fmt
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/io/mod.rs:639:11
  14: std::io::Write::write_fmt
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/io/mod.rs:1954:13
  15: ty::MainLoop::main_loop
             at ./ruff-main/crates/ty/src/lib.rs:304:33
  16: ty::MainLoop::run_with_progress
             at ./ruff-main/crates/ty/src/lib.rs:235:27
  17: ty::MainLoop::run
             at ./ruff-main/crates/ty/src/lib.rs:226:14
  18: ty::run_check
             at ./ruff-main/crates/ty/src/lib.rs:143:19
  19: ty::run
             at ./ruff-main/crates/ty/src/lib.rs:41:39
  20: ty::main
             at ./ruff-main/crates/ty/src/main.rs:6:5
  21: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

##### Automatic Fuzzer note, output status "Some(101)", output signal "None"
```

[compressed.zip](https://github.com/user-attachments/files/20775651/compressed.zip)

---

_Label `bug` added by @sharkdp on 2025-06-17 13:51_

---

_Label `diagnostics` added by @sharkdp on 2025-06-17 13:51_

---

_Label `fatal` added by @sharkdp on 2025-06-17 13:51_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-06-17 13:53_

---

_Comment by @BurntSushi on 2025-06-26 15:13_

Closed by https://github.com/astral-sh/ruff/pull/18962

---

_Closed by @BurntSushi on 2025-06-26 15:13_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:08_

---
