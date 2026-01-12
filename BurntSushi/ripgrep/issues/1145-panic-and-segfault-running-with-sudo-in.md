```yaml
number: 1145
title: Panic and Segfault running with sudo in /
type: issue
state: closed
author: nabowler
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-12-20T14:02:22Z
updated_at: 2018-12-20T19:14:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1145
synced_at: 2026-01-12T16:13:23Z
```

# Panic and Segfault running with sudo in /

---

_@nabowler_

#### What version of ripgrep are you using?
apt-get:
```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

cargo:
```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
`sudo apt-get install ripgrep`
 and 
`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Linux Mint
uname -a: ``` Linux i3-NZB301-TSB 4.15.0-42-generic #45~16.04.1-Ubuntu SMP Mon Nov 19 13:02:27 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux```

#### Describe your question, feature request, or bug.

ripgrep Panics and Segfaults when run from root (/) as root via sudo

non-sudo usage has not exhibited this behavior

#### If this is a bug, what are the steps to reproduce the behavior?

1. install ripgrep
2. `cd \`
3. `sudo rg "xfce-teal.jpg"`

#### If this is a bug, what is the actual behavior?

`sudo rg "xfce-teal.jpg"`

```
thread '<unnamed>' panicked at 'slice index starts at 247 but ends at 3', libcore/slice/mod.rs:2340:5
[1]    10908 segmentation fault (core dumped)  sudo RUST_BACKTRACE=1 ~/.cargo/bin/rg "xfce-teal.jpg"
```

Notes:
 * despite RUST_BACKTRACE=1, a backtrace is usually not produced. 
 * The start index displayed varies, but always ends at 3 for me

#### If this is a bug, what is the expected behavior?

Not crashed.


---

_Comment by @BurntSushi on 2018-12-20 14:39_

Thanks for the bug report! Unfortunately, I can't reproduce this. I have some follow up questions:

1. You say that you installed ripgrep via "apt-get and cargo install." Could you please include the precise commands you ran?
2. You say that when setting `RUST_BACKTRACE=1`, a backtrace is "usually" not produced. Does that mean it is sometimes produced? If so, can you please include it?

If I'm unable to reproduce this, then you might need to do some debugging to narrow this bug down, otherwise it's unlikely that it can be fixed. To narrow it down, you'll want to find a command that exhibits the same failure mode, but is executed on as *small of input as possible*, ideally a single file.

---

_Label `question` added by @BurntSushi on 2018-12-20 14:39_

---

_Comment by @nabowler on 2018-12-20 15:40_

Thanks for the reply. I've updated the original comment with the exact install commands. I installed via cargo this morning, only after originally hitting it from the version installed via apt-get.

I was able to get it to print a backtrace once early, but it was lost to my terminal history before I decided to submit this, and was unable to get another backtrace while writing this up originally. I've now gotten one, at the expense of locking-up my machine twice and crashing Cinnamon once (neither of which happened originally), and added the backtrace, which I fear won't be that helpful.

I'll see if I can determine what file(s) it's dieing on.

---

_Comment by @BurntSushi on 2018-12-20 15:45_

@nabowler Thanks! I see your update install commands, but I don't think I see the backtrace? Or did I misunderstand and you weren't able to get it?

Also, with respect to the install commands, you say that you installed ripgrep both via apt-get and cargo? Which one are you using here? Or is the same bug present in both?

---

_Comment by @BurntSushi on 2018-12-20 15:45_

> I've now gotten one, at the expense of locking-up my machine twice and crashing Cinnamon once (neither of which happened originally)

Can you tell whether ripgrep is using a lot of memory? Is that what's causing the lockup?

---

_Comment by @nabowler on 2018-12-20 16:05_

I built the current master as debug, installed it to /opt/ripgrep/rg, and ran with RUST_BACKTRACE=1
```
sys/kernel/debug/ieee80211/phy0/reset: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_sink_crc_eDP1: Inappropriate ioctl for device (os error 25)
thread '<unnamed>' panicked at 'slice index starts at 119 but ends at 3', libcore/slice/mod.rs:2340:5
sys/kernel/debug/dri/0/i915_emon_status: No such device (os error 19)
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:211
   3: std::panicking::default_hook
             at libstd/panicking.rs:227
   4: std::panicking::rust_panic_with_hook
             at libstd/panicking.rs:476
   5: std::panicking::continue_panic_fmt
             at libstd/panicking.rs:390
   6: rust_begin_unwind
             at libstd/panicking.rs:325
   7: core::panicking::panic_fmt
             at libcore/panicking.rs:77
   8: core::slice::slice_index_order_fail
             at libcore/slice/mod.rs:2340
   9: <core::ops::range::Range<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2509
  10: <core::ops::range::RangeFrom<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2585
  11: core::slice::<impl core::ops::index::IndexMut<I> for [T]>::index_mut
             at libcore/slice/mod.rs:2327
  12: encoding_rs_io::util::read_full
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:221
  13: <encoding_rs_io::util::BomPeeker<R>>::peek_bom
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:145
  14: <encoding_rs_io::DecodeReaderBytes<R, B>>::detect
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:434
  15: <encoding_rs_io::DecodeReaderBytes<R, B> as std::io::Read>::read
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:311
  16: std::io::impls::<impl std::io::Read for &'a mut R>::read
             at libstd/io/impls.rs:23
  17: grep_searcher::line_buffer::LineBuffer::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:399
  18: <grep_searcher::line_buffer::LineBufferReader<'b, R>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:253
  19: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:57
  20: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::run
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:42
  21: grep_searcher::searcher::Searcher::search_reader
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:686
  22: grep_searcher::searcher::Searcher::search_file_maybe_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:640
  23: grep_searcher::searcher::Searcher::search_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:590
  24: rg::search::search_path
             at src/search.rs:423
  25: <rg::search::SearchWorker<W>>::search_path
             at src/search.rs:381
  26: <rg::search::SearchWorker<W>>::search_impl
             at src/search.rs:320
  27: <rg::search::SearchWorker<W>>::search
             at src/search.rs:282
  28: rg::search_parallel::{{closure}}::{{closure}}
             at src/main.rs:141
  29: ignore::walk::Worker::run
             at ignore/src/walk.rs:1291
  30: ignore::walk::WalkParallel::run::{{closure}}
             at ./home/nzb301/git/rust/ripgrep/ignore/src/walk.rs:1149
sys/kernel/debug/dri/0/i915_guc_log_control: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_next_seqno: Permission denied (os error 13)
sys/kernel/debug/dri/0/i915_cache_sharing: No such device (os error 19)
sys/kernel/debug/dri/0/i915_pipe_C_crc: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_pipe_B_crc: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_pipe_A_crc: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_forcewake_user: Invalid argument (os error 22)
thread '<unnamed>' panicked at 'slice index starts at 85 but ends at 3', libcore/slice/mod.rs:2340:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:211
   3: std::panicking::default_hook
             at libstd/panicking.rs:227
   4: std::panicking::rust_panic_with_hook
             at libstd/panicking.rs:476
   5: std::panicking::continue_panic_fmt
             at libstd/panicking.rs:390
   6: rust_begin_unwind
             at libstd/panicking.rs:325
   7: core::panicking::panic_fmt
             at libcore/panicking.rs:77
   8: core::slice::slice_index_order_fail
             at libcore/slice/mod.rs:2340
   9: <core::ops::range::Range<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2509
  10: <core::ops::range::RangeFrom<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2585
  11: core::slice::<impl core::ops::index::IndexMut<I> for [T]>::index_mut
             at libcore/slice/mod.rs:2327
  12: encoding_rs_io::util::read_full
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:221
  13: <encoding_rs_io::util::BomPeeker<R>>::peek_bom
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:145
  14: <encoding_rs_io::DecodeReaderBytes<R, B>>::detect
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:434
  15: <encoding_rs_io::DecodeReaderBytes<R, B> as std::io::Read>::read
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:311
  16: std::io::impls::<impl std::io::Read for &'a mut R>::read
             at libstd/io/impls.rs:23
  17: grep_searcher::line_buffer::LineBuffer::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:399
  18: <grep_searcher::line_buffer::LineBufferReader<'b, R>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:253
  19: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:57
  20: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::run
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:42
  21: grep_searcher::searcher::Searcher::search_reader
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:686
  22: grep_searcher::searcher::Searcher::search_file_maybe_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:640
  23: grep_searcher::searcher::Searcher::search_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:590
  24: rg::search::search_path
             at src/search.rs:423
  25: <rg::search::SearchWorker<W>>::search_path
             at src/search.rs:381
  26: <rg::search::SearchWorker<W>>::search_impl
             at src/search.rs:320
  27: <rg::search::SearchWorker<W>>::search
             at src/search.rs:282
  28: rg::search_parallel::{{closure}}::{{closure}}
             at src/main.rs:141
  29: ignore::walk::Worker::run
             at ignore/src/walk.rs:1291
  30: ignore::walk::WalkParallel::run::{{closure}}
             at ./home/nzb301/git/rust/ripgrep/ignore/src/walk.rs:1149
thread '<unnamed>' panicked at 'slice index starts at 119 but ends at 3', libcore/slice/mod.rs:2340:5
sys/kernel/debug/dri/0/i915_emon_status: No such device (os error 19)
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:211
   3: std::panicking::default_hook
             at libstd/panicking.rs:227
   4: std::panicking::rust_panic_with_hook
             at libstd/panicking.rs:476
   5: std::panicking::continue_panic_fmt
             at libstd/panicking.rs:390
   6: rust_begin_unwind
             at libstd/panicking.rs:325
   7: core::panicking::panic_fmt
             at libcore/panicking.rs:77
   8: core::slice::slice_index_order_fail
             at libcore/slice/mod.rs:2340
   9: <core::ops::range::Range<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2509
  10: <core::ops::range::RangeFrom<usize> as core::slice::SliceIndex<[T]>>::index_mut
             at libcore/slice/mod.rs:2585
  11: core::slice::<impl core::ops::index::IndexMut<I> for [T]>::index_mut
             at libcore/slice/mod.rs:2327
  12: encoding_rs_io::util::read_full
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:221
  13: <encoding_rs_io::util::BomPeeker<R>>::peek_bom
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/util.rs:145
  14: <encoding_rs_io::DecodeReaderBytes<R, B>>::detect
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:434
  15: <encoding_rs_io::DecodeReaderBytes<R, B> as std::io::Read>::read
             at ./home/nzb301/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs_io-0.1.3/src/lib.rs:311
  16: std::io::impls::<impl std::io::Read for &'a mut R>::read
             at libstd/io/impls.rs:23
  17: grep_searcher::line_buffer::LineBuffer::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:399
  18: <grep_searcher::line_buffer::LineBufferReader<'b, R>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/line_buffer.rs:253
  19: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::fill
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:57
  20: <grep_searcher::searcher::glue::ReadByLine<'s, M, R, S>>::run
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/glue.rs:42
  21: grep_searcher::searcher::Searcher::search_reader
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:686
  22: grep_searcher::searcher::Searcher::search_file_maybe_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:640
  23: grep_searcher::searcher::Searcher::search_path
             at ./home/nzb301/git/rust/ripgrep/grep-searcher/src/searcher/mod.rs:590
  24: rg::search::search_path
             at src/search.rs:423
  25: <rg::search::SearchWorker<W>>::search_path
             at src/search.rs:381
  26: <rg::search::SearchWorker<W>>::search_impl
             at src/search.rs:320
  27: <rg::search::SearchWorker<W>>::search
             at src/search.rs:282
  28: rg::search_parallel::{{closure}}::{{closure}}
             at src/main.rs:141
  29: ignore::walk::Worker::run
             at ignore/src/walk.rs:1291
  30: ignore::walk::WalkParallel::run::{{closure}}
             at ./home/nzb301/git/rust/ripgrep/ignore/src/walk.rs:1149
sys/kernel/debug/dri/0/i915_guc_log_control: Invalid argument (os error 22)
sys/kernel/debug/dri/0/i915_next_seqno: Permission denied (os error 13)
sys/kernel/debug/dri/0/i915_cache_sharing: No such device (os error 19)
sys/kernel/debug/dri/0/i915_pipe_
```
The terminal hung at that state.

I can look at memory usage, but I suspect, based on the interwined errors, that some file(s) within that area break ripgrep.

---

_Comment by @nabowler on 2018-12-20 16:15_

> but I don't think I see the backtrace

I updated the original comment, under the Actual Behavior section. I also added a separate comment with a debug-build backtrace

> Also, with respect to the install commands, you say that you installed ripgrep both via apt-get and cargo? Which one are you using here? Or is the same bug present in both?

The bug is present in both. And when built from master.

---

_Comment by @BurntSushi on 2018-12-20 18:12_

Thanks! And yeah, this is going to require a minimal reproduction, i.e., the specific contents of the file causing the error. If you can, a breadth first search starting from the root of your file system should hopefully narrow it down fairly quickly.

Note that in general, running `sudo rg foo /` or even `sudo grep -r foo` (there's nothing special about ripgrep here) is generally a bad idea. Funky things can happen when reading `/proc` and/or `/sys if I recall correctly.

The presence of the seg fault is baffling to me though. I understand the panic---it looks like a bug in transcoding somewhere?---but that shouldn't be causing a segfault.

---

_Comment by @nabowler on 2018-12-20 18:24_

I've narrowed it down to the file `/sys/kernel/debug/hid/0018:06CB:7A13.0004/events`, which appears to be a real-time feed of events from my laptop's trackpad.

`grep` on this file appears to be non-terminating and non-crashing (works as expected)
`rg` on this file will segfault immediately upon events and not terminate until events
`less` on this file will segfault eventually upon events (it works as expected until it segfaults)
`cat` on this file appears to be non-terminating and non-crashing (works as expected)
`cat events | rg Button` is non-terminating and non-crashing (works as expected)

So there's some amount of pebcak as I shouldn't have `sudo rg/grep`ed from /, though I'd expect `rg`, and `less` for that matter, to not segfault.




---

_Comment by @BurntSushi on 2018-12-20 19:05_

Interesting. I can run `rg` as root on those files and it seems to work file. It is indeed non-terminating though, but that seems expected. So in general, I wouldn't necessarily expect `sudo rg pattern /` to terminate in general.

I'm going to close this as `wontfix`. If someone can track down a more precise cause and come up with a simple fix, then I'd be happy to take it and would be curious to see what exactly is causing the segfault.

---

_Label `wontfix` added by @BurntSushi on 2018-12-20 19:05_

---

_Closed by @BurntSushi on 2018-12-20 19:05_

---
