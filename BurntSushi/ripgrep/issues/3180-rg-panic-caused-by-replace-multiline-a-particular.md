```yaml
number: 3180
title: rg panic caused by --replace, --multiline, a particular pattern, and search text containing repeats and newlines
type: issue
state: closed
author: edhalter
labels:
  - bug
assignees: []
created_at: 2025-10-12T17:29:32Z
updated_at: 2025-10-12T21:25:20Z
url: https://github.com/BurntSushi/ripgrep/issues/3180
synced_at: 2026-01-12T16:13:25Z
```

# rg panic caused by --replace, --multiline, a particular pattern, and search text containing repeats and newlines

---

_@edhalter_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

Gentoo Portage

### What operating system are you using ripgrep on?

Gentoo Linux

### Describe your bug.

When using the --replace and --multiline flags w/ certain search patterns, ripgrep panics when it searches text containing certain sequences of repeated text and newlines. The panic message is "slice index starts at x but ends at y", where x and y are integers and y < x.

Background: I have a program that runs ripgrep (and other regex software), parses its output, and counts and displays word sequences. Ripgrep has performed great; I've used many, many different search patterns on ~50 MiB of English text in ~5,000 files. Recently ran across this bug. The initial search pattern was much more complicated -- I spent some time simplifying it down to these 2 minimal test cases. (If it's useful, I can explain why the various groups in the pattern are there.)

Each minimal search pattern is extremely specific. Removing almost any element will prevent the bug from appearing; for example, changing the initial group from `(^|[^a-z])` to `^`, or changing the "\s" in the middle to " ". Similar situation for the search text: for the lines2 test case, changing the initial " " to "a" prevents the bug from appearing.

### What are the steps to reproduce the behavior?

test case 1:

```
lines2=$(printf " b b b b b b b b\nc")
echo "$lines2" > lines2.txt
rg "(^|[^a-z])((([a-z]+)?)\s)?b(\s([a-z]+)?)($|[^a-z])" --replace=x --multiline lines2.txt
```

test case 2:

```
lines5=$(printf " b\nb\nb\nb\nc")
echo "$lines5" > lines5.txt
rg "(^|[^a-z])((([a-z]+)?)\s)?b(\s([a-z]+)?)($|[^a-z])" --replace=x --multiline lines5.txt
```

### What is the actual behavior?

Note: The backtraces for the lines2 test case and the lines5 test case are identical, except for the slice indices mentioned in the panic messages at the top.

cmd:

`RUST_BACKTRACE=1 rg "(^|[^a-z])((([a-z]+)?)\s)?b(\s([a-z]+)?)($|[^a-z])" --replace=x --multiline --debug lines2.txt`

output:
```
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: i7-5820k
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
thread 'main' panicked at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/printer/src/util.rs:568:22:
slice index starts at 18 but ends at 17
stack backtrace:
   0: rust_begin_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:662:5
   1: core::panicking::panic_fmt
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panicking.rs:74:14
   2: core::slice::index::slice_index_order_fail_rt
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/slice/index.rs:85:5
   3: core::slice::index::slice_index_order_fail
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/slice/index.rs:78:5
   4: <core::ops::range::Range<usize> as core::slice::index::SliceIndex<[T]>>::index
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/slice/index.rs:464:13
   5: core::slice::index::<impl core::ops::index::Index<I> for [T]>::index
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/slice/index.rs:16:9
   6: grep_printer::util::replace_with_captures_in_context
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/printer/src/util.rs:568:22
   7: grep_printer::util::Replacer<M>::replace_all
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/printer/src/util.rs:81:13
   8: grep_printer::standard::StandardSink<M,W>::replace
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/printer/src/standard.rs:769:13
   9: <grep_printer::standard::StandardSink<M,W> as grep_searcher::sink::Sink>::matched
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/printer/src/standard.rs:835:9
  10: <&mut S as grep_searcher::sink::Sink>::matched
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/sink.rs:234:9
  11: grep_searcher::searcher::core::Core<M,S>::sink_matched
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/core.rs:468:25
  12: grep_searcher::searcher::core::Core<M,S>::matched
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/core.rs:94:9
  13: grep_searcher::searcher::glue::MultiLine<M,S>::sink_matched
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/glue.rs:299:19
  14: grep_searcher::searcher::glue::MultiLine<M,S>::run
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/glue.rs:172:38
  15: grep_searcher::searcher::Searcher::search_slice
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/mod.rs:770:13
  16: grep_searcher::searcher::Searcher::search_file_maybe_path
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/mod.rs:671:20
  17: grep_searcher::searcher::Searcher::search_path
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/searcher/src/searcher/mod.rs:636:9
  18: rg::search::search_path
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/search.rs:387:13
  19: rg::search::SearchWorker<W>::search_path
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/search.rs:345:33
  20: rg::search::SearchWorker<W>::search
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/search.rs:264:13
  21: rg::search
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/main.rs:126:35
  22: rg::run
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/main.rs:87:54
  23: rg::main
             at /var/tmp/portage/sys-apps/ripgrep-14.1.1-r1/work/ripgrep-14.1.1/crates/core/main.rs:44:11
  24: core::ops::function::FnOnce::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/ops/function.rs:250:5
```

slightly-longer backtrace w/ [RUST_BACKTRACE=full](https://gist.github.com/edhalter/bb05fea4784390663ffb88d6aed33bf1)

### What is the expected behavior?

ripgrep should have returned matching text

---

_Label `bug` added by @BurntSushi on 2025-10-12 20:32_

---

_Comment by @BurntSushi on 2025-10-12 20:32_

Nice find, thank you! This will be fixed in the next release.

---

_Closed by @BurntSushi on 2025-10-12 21:25_

---
