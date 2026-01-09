---
number: 10097
title: Rule RUF100 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-02-23T13:42:59Z
updated_at: 2024-04-01T01:00:38Z
url: https://github.com/astral-sh/ruff/issues/10097
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule RUF100 cause panic

---

_Issue opened by @qarmin on 2024-02-23 13:42_

ruff 0.2.2
 (latest changes from main branch)
```
ruff  *.py --select RUF100 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
        # model = cloudmersive_validate_api_client.models.validate_state_request.ValidateStateRequest()  #noqa: E50µ
```

error
```

error: Panicked while linting /opt/tmp_folder/F_NAME_162489350101181166.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_source_file/src/locator.rs:455:23:
byte index 107 is not a char boundary; it is inside '\u{85}' (bytes 106..108) of `        # model = cloudmersive_validate_api_client.models.validate_state_request.ValidateStateRequest()  #noqa: E50µ`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/alloc/src/boxed.rs:2029:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/mod.rs:88:9
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::Range<usize>>::index
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/traits.rs:231:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/traits.rs:61:15
  11: ruff_text_size::range::<impl core::ops::index::Index<ruff_text_size::range::TextRange> for str>::index
             at ./ruff/crates/ruff_text_size/src/range.rs:430:14
  12: ruff_source_file::locator::Locator::slice
             at ./ruff/crates/ruff_source_file/src/locator.rs:455:23
  13: ruff_linter::fix::apply_fixes
             at ./ruff/crates/ruff_linter/src/fix/mod.rs:98:25
  14: ruff_linter::fix::fix_file
             at ./ruff/crates/ruff_linter/src/fix/mod.rs:48:14
  15: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:579:14
  16: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:279:14
  17: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  18: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  19: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  20: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  21: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  22: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  23: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  24: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:123:36
  25: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:179:20
  26: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:110:9
  27: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:438:13
  28: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:397:12
  29: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:373:13
  30: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:732:9
  31: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:357:12
  32: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:708:9
  33: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:46:9
  34: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/fold.rs:59:9
  35: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/reduce.rs:15:5
  36: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/mod.rs:991:9
  37: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  38: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:419:13
  39: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:201:33
  40: ruff::main
             at ./ruff/crates/ruff/src/main.rs:49:11
  41: core::ops::function::FnOnce::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:250:5
  42: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:155:18
  43: std::rt::lang_start::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:166:18
  44: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:284:13
  45: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  46: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  47: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  48: std::rt::lang_start_internal::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:48
  49: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  50: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  51: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  52: std::rt::lang_start_internal
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:20
  53: main
  54: <unknown>
  55: __libc_start_main
  56: _start

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14385824/python_compressed.zip)



---

_Label `bug` added by @MichaReiser on 2024-02-23 13:48_

---

_Label `help wanted` added by @MichaReiser on 2024-02-23 13:48_

---

_Comment by @mikeleppane on 2024-02-25 16:43_

👍 thanks for including a test file! 
The problem is that we try to slice a str type containing unicode chars. This might be a rare edge case but still IMO need to improve fault-tolerance and prevent panics like this.

The problem occurs in the following code (`ruff/crates/ruff_source_file/src/locator.rs`): 
```rust  
/// Take the source code between the given [`TextRange`].
    #[inline]
    pub fn slice<T: Ranged>(&self, ranged: T) -> &'a str {
        &self.contents[ranged.range()]
    }
```
Below is some debug prints before panic happens: 
```bash
CONTENT: "        # model = cloudmersive_validate_api_client.models.validate_state_request.ValidateStateRequest()  #\u{85}noqa: E50µ"
CONTENT LEN: 119
RANGE: 0..107
error: Panicked while linting /home/mikko/dev/ruff-playground/src/ruff_playground/F_NAME_162489350101181166.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/mikko/dev/ruff/crates/ruff_source_file/src/locator.rs:458:23:
byte index 107 is not a char boundary; it is inside '\u{85}' (bytes 106..108) of `        # model = cloudmersive_validate_api_client.models.validate_state_request.ValidateStateRequest()  #noqa: E50µ`
```






---

_Comment by @charliermarsh on 2024-02-25 22:38_

Thanks! Where are we generating the slice? Are we adding a fixed offset (like 1) somewhere, instead of using the char width?

---

_Comment by @mikeleppane on 2024-02-26 09:05_

![ruf100_debug](https://github.com/astral-sh/ruff/assets/3906518/afefbbbe-0735-40dc-87a9-a5275c97434e)

⬆️ Could you check the above picture? There you can find the call site where we are generating the slice. 

I don't think we are not adding any fixed offset just using char width. So, now that there exists a multibyte character we are indexing it with byte indices which is not a char boundary.

You can easily reproduce this with the following code: 
```rust 
fn main() {
    let text = "  #noqa: E50µ";
    let slice = &text[0..4];
}
```
I think that the root cause originates from here (`ruff/crates/ruff_linter/src/checkers/noqa.rs`, line 237):
```rust 
else {
        Edit::deletion(
            range.start() + "# ".text_len(),
```
If I understand correctly the reason for this, we try to preserve possible comments after the noqa? 


 

---

_Comment by @diceroll123 on 2024-03-31 06:11_

It looks like this error doesn't happen anymore.

---

_Comment by @qarmin on 2024-03-31 06:57_

I still can reproduce this 

---

_Comment by @diceroll123 on 2024-03-31 06:59_

Hmm, perhaps I overlooked something, I'll take another look later 🤔

---

_Comment by @qarmin on 2024-03-31 07:30_

You should use code from python_compressed.zip.
Github removes/changes some characters from  
```
        # model = cloudmersive_validate_api_client.models.validate_state_request.ValidateStateRequest()  #�noqa: E50µ
```
so it may not always cause panic - this is only preview how file looks like

---

_Comment by @mikeleppane on 2024-03-31 08:43_

Hey! Yes, you should use the test file attached, and use it as it is. If you reformat the file, I think that you cannot reproduce the issue. I hardly doubt that this issue has been fixed. Otherwise, the project’s issue tracking is lacking something.

---

_Referenced in [astral-sh/ruff#10682](../../astral-sh/ruff/pulls/10682.md) on 2024-03-31 19:21_

---

_Comment by @diceroll123 on 2024-03-31 19:43_

Yeah, I made the mistake of copying directly from the page. 😔

Thanks y'all!

---

_Closed by @charliermarsh on 2024-04-01 01:00_

---
