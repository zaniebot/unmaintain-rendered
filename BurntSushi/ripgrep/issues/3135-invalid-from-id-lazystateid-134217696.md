```yaml
number: 3135
title: "invalid 'from' id: LazyStateID(134217696)"
type: issue
state: closed
author: kenorb
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-08-31T12:57:41Z
updated_at: 2025-10-11T00:13:33Z
url: https://github.com/BurntSushi/ripgrep/issues/3135
synced_at: 2026-01-12T16:13:25Z
```

# invalid 'from' id: LazyStateID(134217696)

---

_@kenorb_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1


### How did you install ripgrep?

`cargo` I think

### What operating system are you using ripgrep on?

Ubuntu 24.04

### Describe your bug.

I'm filtering out some script and got the error.

Command:

`some_input_stream | rg --regex-size-limit 2G -wFf hexes.txt -v`

where `hexes.txt` is a file with 110726 hexes. 

### What are the steps to reproduce the behavior?

Example command which crashes:

`cat hexes.txt | rg --regex-size-limit 2G -wFf hexes.txt -v`


### What is the actual behavior?

```
$ cat hexes.txt | rg --regex-size-limit 2G -wFf hexes.txt -v
thread 'main' panicked at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/dfa.rs:2601:9:
invalid 'from' id: LazyStateID(134217696)
stack backtrace:
   0:     0x60a9b5bfe91b - std::backtrace_rs::backtrace::libunwind::trace::h59b05d333ef250f9
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/../../backtrace/src/backtrace/libunwind.rs:104:5
   1:     0x60a9b5bfe91b - std::backtrace_rs::backtrace::trace_unsynchronized::hbad10f6c65416841
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
   2:     0x60a9b5bfe91b - std::sys_common::backtrace::_print_fmt::hda4a67a10610ef78
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:67:5
   3:     0x60a9b5bfe91b - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::he8d8a3d249a948a2
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:44:22
   4:     0x60a9b5c40470 - core::fmt::rt::Argument::fmt::h507ca91fbb1ef494
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/core/src/fmt/rt.rs:142:9
   5:     0x60a9b5c40470 - core::fmt::write::h4d5f6025aa566322
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/core/src/fmt/mod.rs:1120:17
   6:     0x60a9b5bf307d - std::io::Write::write_fmt::h1b572fe055a121e6
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/io/mod.rs:1762:15
   7:     0x60a9b5bfe704 - std::sys_common::backtrace::_print::hcaa91a137f39fd45
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:47:5
   8:     0x60a9b5bfe704 - std::sys_common::backtrace::print::hdf91ee03271e5d94
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:34:9
   9:     0x60a9b5c2530a - std::panicking::default_hook::{{closure}}::h59f1f95e0a9a0472
  10:     0x60a9b5c24fad - std::panicking::default_hook::h731f67f92b204ede
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:292:9
  11:     0x60a9b5c25728 - std::panicking::rust_panic_with_hook::hbcae08ba0ccf4c11
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:779:13
  12:     0x60a9b5bfecfe - std::panicking::begin_panic_handler::{{closure}}::h9083954869cd1afa
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:657:13
  13:     0x60a9b5bfeb36 - std::sys_common::backtrace::__rust_end_short_backtrace::hb8caade51576a0bc
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:170:18
  14:     0x60a9b5c25452 - rust_begin_unwind
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:645:5
  15:     0x60a9b5968615 - core::panicking::panic_fmt::hef0307862026e6f9
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/core/src/panicking.rs:72:14
  16:     0x60a9b5b35a79 - regex_automata::hybrid::dfa::Lazy::set_transition::hc52640126510cfe5
  17:     0x60a9b5962e8c - regex_automata::hybrid::dfa::Lazy::cache_next_state::h465e82b3aa35fb91
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/dfa.rs:2146:9
  18:     0x60a9b5b4ec9a - regex_automata::hybrid::dfa::DFA::next_state::h3edac6f81bca9a51
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/dfa.rs:1228:9
  19:     0x60a9b5b4ec9a - regex_automata::hybrid::search::find_fwd_imp::hed9eaf4fa7abd659
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/search.rs:227:23
  20:     0x60a9b5b4ec9a - regex_automata::hybrid::search::find_fwd::h1e829e1b7b2bf8f6
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/search.rs:44:13
  21:     0x60a9b5b19fb1 - regex_automata::hybrid::dfa::DFA::try_search_fwd::h44702e4e04b3be0f
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/hybrid/dfa.rs:595:24
  22:     0x60a9b5b19fb1 - regex_automata::meta::wrappers::HybridEngine::try_search_half_fwd::h2b5536ebec9708df
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/meta/wrappers.rs:669:13
  23:     0x60a9b5b19fb1 - <regex_automata::meta::strategy::Core as regex_automata::meta::strategy::Strategy>::search_half::hfc46da411c3b25bc
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/meta/strategy.rs:752:19
  24:     0x60a9b59b6713 - regex_automata::meta::regex::Regex::search_half::h339334cddaf68de7
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/regex-automata-0.4.9/src/meta/regex.rs:980:22
  25:     0x60a9b59a8a4f - <grep_regex::matcher::RegexMatcher as grep_matcher::Matcher>::shortest_match_at::h873c8d8f3e06654d
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.13/src/matcher.rs:477:12
  26:     0x60a9b59a8a4f - grep_matcher::Matcher::shortest_match::hf67145e54caf32fa
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-matcher-0.1.7/src/lib.rs:1005:9
  27:     0x60a9b59a8a4f - <grep_regex::matcher::RegexMatcher as grep_matcher::Matcher>::find_candidate_line::h3d5a902cd4e20c8a
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.13/src/matcher.rs:503:22
  28:     0x60a9b59a8a4f - <&M as grep_matcher::Matcher>::find_candidate_line::hfcb6502f7ac53201
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-matcher-0.1.7/src/lib.rs:1368:9
  29:     0x60a9b59a8a4f - <&M as grep_matcher::Matcher>::find_candidate_line::h0e80c914a98abd3c
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-matcher-0.1.7/src/lib.rs:1368:9
  30:     0x60a9b59a8a4f - grep_searcher::searcher::core::Core<M,S>::find_by_line_fast::h2fb0871a95c5750c
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/core.rs:407:19
  31:     0x60a9b59a8a4f - grep_searcher::searcher::core::Core<M,S>::match_by_line_fast_invert::h7d3f4a120a24b861
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/core.rs:362:34
  32:     0x60a9b59a8a4f - grep_searcher::searcher::core::Core<M,S>::match_by_line_fast::hf2e3c861e812c537
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/core.rs:327:21
  33:     0x60a9b59a8a4f - grep_searcher::searcher::core::Core<M,S>::match_by_line::h792fd5f23db831a6
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/core.rs:124:19
  34:     0x60a9b59a2775 - grep_searcher::searcher::glue::ReadByLine<M,R,S>::run::hdb2abbe61df033ff
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/glue.rs:40:35
  35:     0x60a9b597cbd9 - grep_searcher::searcher::Searcher::search_reader::h065b2c1229fda649
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-searcher-0.1.14/src/searcher/mod.rs:743:13
  36:     0x60a9b5a11164 - rg::search::search_reader::h5de37fe01bca312d
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/search.rs:424:13
  37:     0x60a9b5a11164 - rg::search::SearchWorker<W>::search_reader::h266c7df85b0095aa
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/search.rs:369:33
  38:     0x60a9b5a11164 - rg::search::SearchWorker<W>::search::h70044c220a4c5ccf
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/search.rs:258:13
  39:     0x60a9b59f8110 - rg::search::hd0a2a67cd8cd04d8
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/main.rs:126:35
  40:     0x60a9b59f8110 - rg::run::hec62c20163d312e9
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/main.rs:87:54
  41:     0x60a9b59f627e - rg::main::hecd825c78a6ae630
                               at /home/kenorb/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.1/crates/core/main.rs:44:11
  42:     0x60a9b5a26383 - core::ops::function::FnOnce::call_once::h481e920187dc4850
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/core/src/ops/function.rs:250:5
  43:     0x60a9b5a26383 - std::sys_common::backtrace::__rust_begin_short_backtrace::h3751da6b685022f7
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/sys_common/backtrace.rs:154:18
  44:     0x60a9b5a5a729 - std::rt::lang_start::{{closure}}::h41bdde9c58545b5b
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/rt.rs:167:18
  45:     0x60a9b5c25344 - core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once::h9504c37abfb6f513
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/core/src/ops/function.rs:284:13
  46:     0x60a9b5c25344 - std::panicking::try::do_call::h4e8ae0d3fdaef5a1
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:552:40
  47:     0x60a9b5c25344 - std::panicking::try::hd686c4fbf6e25a1e
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:516:19
  48:     0x60a9b5c25344 - std::panic::catch_unwind::h6357f6a92a957bb2
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panic.rs:142:14
  49:     0x60a9b5c25344 - std::rt::lang_start_internal::{{closure}}::h6e901a66b8cf246f
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/rt.rs:148:48
  50:     0x60a9b5c25344 - std::panicking::try::do_call::h7474622977779c90
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:552:40
  51:     0x60a9b5c25344 - std::panicking::try::h88d686af4e2ea353
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panicking.rs:516:19
  52:     0x60a9b5c0cd7b - std::panic::catch_unwind::h602261075d2dc6e5
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/panic.rs:142:14
  53:     0x60a9b5c0cd7b - std::rt::lang_start_internal::hea207582130b429d
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/rt.rs:148:20
  54:     0x60a9b5a5a71e - std::rt::lang_start::h62fdab7bb22e71d6
                               at /build/rustc-kAv1jW/rustc-1.75.0+dfsg0ubuntu1~bpo0/library/std/src/rt.rs:166:17
  55:     0x7baacf02a1ca - __libc_start_call_main
                               at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  56:     0x7baacf02a28b - __libc_start_main_impl
                               at ./csu/../csu/libc-start.c:360:3
  57:     0x60a9b5968ee5 - _start
  58:                0x0 - <unknown>
```

### What is the expected behavior?

Not to crash.

---

_Comment by @BurntSushi on 2025-08-31 13:09_

Please provide an MRE.

---

_Comment by @kenorb on 2025-08-31 13:44_

Example with compressed file:
```
RUST_BACKTRACE=full rg --regex-size-limit 2G -wFf <(zcat hexes-example.txt.gz) -v <(zcat hexes-example.txt.gz)
```
or with uncompressed file:
```
RUST_BACKTRACE=full rg --regex-size-limit 2G -wFf hexes-example.txt -v hexes-example.txt
```

What's interesting, when I've replaced all 7 with 0, then crash doesn't happen:

```
RUST_BACKTRACE=full rg --regex-size-limit 2G -wFf <(zcat hexes-example.txt.gz | sed s/7/0/g) -v <(zcat hexes-example.txt.gz | sed s/7/0/g)
```

File:  [hexes-example.txt.gz](https://github.com/user-attachments/files/22066734/hexes-example.txt.gz)


---

_Comment by @BurntSushi on 2025-08-31 13:52_

Nice, thank you for the repro! I was able to reproduce it as well.

This might be a repro for an elusive bug reported against the regex engine: https://github.com/rust-lang/regex/issues/1083

---

_Label `bug` added by @BurntSushi on 2025-08-31 13:52_

---

_Closed by @BurntSushi on 2025-10-06 21:18_

---

_Reopened by @BurntSushi on 2025-10-06 21:18_

---

_Comment by @BurntSushi on 2025-10-06 21:19_

Whoops, accidentally closed this. It's fixed upstream, but shouldn't be closed here until a new version of `regex` is in ripgrep's `Cargo.lock` file.

---

_Label `rollup` added by @BurntSushi on 2025-10-07 02:11_

---

_Closed by @BurntSushi on 2025-10-11 00:13_

---
