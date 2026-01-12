```yaml
number: 1167
title: Panic when grepping in Ruby source
type: issue
state: closed
author: sirupsen
labels:
  - duplicate
assignees: []
created_at: 2019-01-19T13:50:09Z
updated_at: 2019-01-19T14:00:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1167
synced_at: 2026-01-12T16:13:23Z
```

# Panic when grepping in Ruby source

---

_@sirupsen_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Brew, latest version.

#### What operating system are you using ripgrep on?

MacOS Mojave, 10.14.1

#### Describe your question, feature request, or bug.

```
git clone git@github.com:ruby/ruby.git /tmp/ruby
cd /tmp/ruby
rg RUBY_VM_SET_TIMER_INTERRUPT
```

Will result in the following panic:

```
thread '<unnamed>' panicked at 'index out of bounds: the len is 1 but the index is 1', /Users/brew/Library/Caches/Homebrew/cargo_cache/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.8.6/src/handles.rs:309:21
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::panicking::panic_bounds_check
   9: encoding_rs::utf_16::Utf16Decoder::decode_to_utf8_raw
  10: encoding_rs::variant::VariantDecoder::decode_to_utf8_raw
  11: encoding_rs::Decoder::decode_to_utf8_without_replacement
  12: encoding_rs::Decoder::decode_to_utf8
  13: encoding_rs_io::util::TinyTranscoder::transcode
  14: <encoding_rs_io::DecodeReaderBytes<R, B> as std::io::Read>::read
  15: <grep_searcher::line_buffer::LineBufferReader<'b, R>>::fill
  16: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::run
  17: grep_searcher::searcher::Searcher::search_path
  18: <rg::search::SearchWorker<W>>::search_impl
  19: rg::search_parallel::{{closure}}::{{closure}}
  20: ignore::walk::Worker::run
```

#### If this is a bug, what are the steps to reproduce the behavior?

```
git clone git@github.com:ruby/ruby.git /tmp/ruby && cd /tmp/ruby && rg RUBY_VM_SET_TIMER_INTERRUPT
```


---

_Renamed from "Panic when grepping in Ruby" to "Panic when grepping in Ruby soource" by @sirupsen on 2019-01-19 13:55_

---

_Renamed from "Panic when grepping in Ruby soource" to "Panic when grepping in Ruby source" by @sirupsen on 2019-01-19 13:57_

---

_Comment by @BurntSushi on 2019-01-19 13:59_

This is a duplicate of #1052. It was fixed on master in #1065. Unfortunately, there hasn't been a release since then. I'll try to get one out soonish.

---

_Closed by @BurntSushi on 2019-01-19 13:59_

---

_Label `duplicate` added by @BurntSushi on 2019-01-19 13:59_

---

_Comment by @sirupsen on 2019-01-19 14:00_

Oops, sorry! Searched only the open bugs 🤦‍♂️, that one escaped me.

Compiled latest master locally and it's 👌 thanks for the quick reply!

---
