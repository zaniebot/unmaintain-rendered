```yaml
number: 1513
title: "Poor performance with Searcher::search_slice"
type: issue
state: closed
author: lllusion3418
labels:
  - question
assignees: []
created_at: 2020-03-10T21:34:49Z
updated_at: 2020-03-11T00:16:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1513
synced_at: 2026-01-12T16:13:23Z
```

# Poor performance with Searcher::search_slice

---

_@lllusion3418_

#### What version of ripgrep are you using?

I'm using the [grep](https://crates.io/crates/grep) crate (0.2.4), master branch (50d2047ae2c0ce2edb5189ae3071538e21b15822) was also tested

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

**Disclaimer**: this does seem to require `\b` (doesn't happen with `(?-u:\b)`) [which is known to have poor performance under some conditions involving unicode](https://github.com/rust-lang/regex/blob/65c4f8ee1f1c5ef21a287856f00b780eafbcd95c/PERFORMANCE.md#unicode-word-boundaries-may-prevent-the-dfa-from-being-used) - though this behaviour happens for files and regexes that are purely ASCII (characters `|{}` are fairly common).

Under some conditions `Searcher::search_slice` (without an encoding / BOM sniffing - i.e. with `SliceByLine`) is slower than `Searcher::search_reader` (`ReadByLine`) by up to orders of magnitude even though usually the opposite is true (as I would expect it to because it adds an unnecessary transcoding step and needs to do some copying).

#### If this is a bug, what are the steps to reproduce the behavior?

I've managed to produce this fairly minimal example to reproduce it:
https://github.com/lllusion3418/rust_grep_issue_bench

1. ```
   $ git clone https://github.com/lllusion3418/rust_grep_issue_bench
   $ cd rust_grep_issue_bench
   ```
3. install rust nightly toolchain
4. `$ rustup override set nightly`
5. `$ cargo bench` and/or `$ cargo run --release`

It (obviously) also happens for some more real-life files/regexes which I'm not really comfortable sharing here - they are ~20kB html files with some `|` characters near the end.

It also seems to happen for the `/usr/share/dict/american-english` [on my system](https://www.archlinux.org/packages/community/any/words/) (2x difference)

#### If this is a bug, what is the actual behavior?

```
$ rustup show # needs nightly for Bencher
Default host: x86_64-unknown-linux-gnu
rustup home:  /home/username/.rustup

installed toolchains
--------------------

stable-x86_64-unknown-linux-gnu (default)
nightly-x86_64-unknown-linux-gnu

active toolchain
----------------

nightly-x86_64-unknown-linux-gnu (directory override for '/home/username/tmp/rust_grep_bench')
rustc 1.43.0-nightly (564758c4c 2020-03-08)

$ cargo bench
    Finished bench [optimized] target(s) in 0.03s
     Running target/release/deps/grep_bench-a2d483b24460d844

running 2 tests
test search_reader ... bench:       3,197 ns/iter (+/- 81)
test search_slice  ... bench:     105,231 ns/iter (+/- 13,126)

test result: ok. 0 passed; 0 failed; 0 ignored; 2 measured; 0 filtered out

$ cargo run --release
    Finished release [optimized] target(s) in 0.03s
     Running `target/release/grep_bench`
# search_reader
- 27.648µs
# search_slice
- 128.941µs
```

#### If this is a bug, what is the expected behavior?
I would  expect `Searcher::search_slice` to be at least as fast as `Searcher::search_reader`.

If the line `slice.extend("|".as_bytes());` is removed it behaves like this:
```
$ cargo bench
    Finished bench [optimized] target(s) in 0.03s
     Running target/release/deps/grep_bench-a2d483b24460d844

running 2 tests
test search_reader ... bench:       2,855 ns/iter (+/- 62)
test search_slice  ... bench:       2,567 ns/iter (+/- 40)

test result: ok. 0 passed; 0 failed; 0 ignored; 2 measured; 0 filtered out
```

---

Thanks for creating ripgrep and even nicely splitting it into crates that others can easily use - the tool where I originally encountered this problem would probably be 100x as long and complex if it wasn't for the [grep](https://crates.io/crates/grep) crate.

On a system which doesn't have nearly enough ram (2GB) to cache every file I wanted to search through ~130.000 files with ~200 regexes and save the results (in json format) separately but for performance reason only read and decode (from windows-1252) every file once instead of just running `rg` for each of the ~200 regexes and combined with [rayon](https://crates.io/crates/rayon) it works quite well in only 150LOC.

---

_Comment by @BurntSushi on 2020-03-11 00:12_

This is a pretty amazing bug! Firstly, thank you for the easy reproduction. Very much appreciated.

Secondly, there is an easy fix for you: enable line terminators when building your matcher. e.g.,

```rust
    let matcher = regex::RegexMatcherBuilder::new()
        .line_terminator(Some(b'\n'))
        .build(r"\bfoo(bar|baz)")
        .unwrap();
```

With that change, the benchmarks improve dramatically. Before:

```
test search_reader ... bench:       2,834 ns/iter (+/- 3)
test search_slice  ... bench:      57,202 ns/iter (+/- 716)
```

After:

```
test search_reader ... bench:         351 ns/iter (+/- 11)
test search_slice  ... bench:         110 ns/iter (+/- 30)
```

OK... So aside from the missed line terminator optimization, the actual reason why you're seeing a performance difference is actually quite interesting, and is a result of confounding factors.

First and foremost, the DFA is quitting even though I claimed in the regex performance docs that the DFA should be able to handle Unicode word boundaries on pure ASCII input. The problem is the result of another optimization that shrinks the alphabet of the DFA into equivalence classes, which is almost always much smaller than the full 256 length of a the normal every-byte alphabet. It turns out in this case that characters like `|`, `{` and `}` (which appear near the end of the ASCII range) were getting lumped into the same equivalence class as non-ASCII bytes. So when the DFA saw a `|`, it treated it as if it were in the same equivalence class as non-ASCII bytes which would in turn trip its circuit and cause it to quit.

Now that doesn't explain everything since that _should_ impact both `search_reader` and `search_slice`. Indeed it does. But the execution is slightly different. For `search_reader`, since your input is pretty small, it will fit all of it into one buffer. However, because of the "rolling" nature of the buffer, it will treat any bytes following the last line terminator as a potential incomplete line. So in your case, `search_reader` will search everything right up until immediately before the `|` you added to the end of the input. Since that input contains no bytes that trip the DFA's circuit, it's fast. It then tries to read more input, sees EOF and searches the remaining `|`. The DFA will quit immediately and fall back to the NFA. But since this is only one byte, it doesn't take much time.

Now in the case of `search_slice`, no such rolling occurs at all. It just searches the bytes. The DFA gets all the way to the last byte of your input, sees the `|`---which erroneously trips the DFA's circuit---and quits. It then has to restart all over again at the beginning but with the NFA. So it does two full complete scans over the input, where one of those scans is using the slower regex engine.

We can test this explanation by simply putting a `|` at the beginning of the input. It should slow both down instead of just one of them:

```
    let mut slice = Vec::new();
    slice.extend("|".as_bytes());
    for _ in 0..100 {
        slice.extend(b"aaaaaaaaaaaaaaaa\n");
    }
```

And benchmarking:

```
test search_reader ... bench:      51,020 ns/iter (+/- 1,293)
test search_slice  ... bench:      53,330 ns/iter (+/- 1,497)
```

I've [filed a bug against `regex` itself](https://github.com/rust-lang/regex/issues/652) to see about fixing the arrangement of equivalence classes, but otherwise, I think everything looks okay here. The main thing missing is the `line_terminator` option, because that's what allows the `grep-regex` crate to pick out some inner literals and use that as a quick prefilter. It effectively papers over any slowness caused by `\b`, whether buggy or not.

---

_Closed by @BurntSushi on 2020-03-11 00:12_

---

_Label `question` added by @BurntSushi on 2020-03-11 00:12_

---

_Comment by @BurntSushi on 2020-03-11 00:16_

Tip: the `grep-regex` crate does logging. You can use [`env_logger`](https://docs.rs/env_logger) or [something simpler](https://github.com/BurntSushi/ripgrep/blob/master/crates/core/logger.rs) to get more details about what's happening.

Without the `line_terminator` option:

```
$ RUST_LOG=trace ./target/release/grep_bench
[2020-03-11T00:15:15Z TRACE grep_regex::matcher] final regex: "\\bfoo(bar|baz)"
# search_reader
[2020-03-11T00:15:15Z TRACE grep_searcher::searcher] generic reader: searching via roll buffer strategy
[2020-03-11T00:15:15Z TRACE grep_searcher::searcher::core] searcher core: will use fast line searcher
- 32.771µs
# search_slice
[2020-03-11T00:15:15Z TRACE grep_searcher::searcher] slice reader: searching via slice-by-line strategy
[2020-03-11T00:15:15Z TRACE grep_searcher::searcher::core] searcher core: will use fast line searcher
- 136.358µs
```

And with the `line_terminator` option:

```
$ RUST_LOG=trace ./target/release/grep_bench
[2020-03-11T00:15:27Z DEBUG grep_regex::literal] required literal found: "fooba"
[2020-03-11T00:15:27Z DEBUG grep_regex::matcher] extracted fast line regex: (?-u:fooba)
[2020-03-11T00:15:27Z TRACE grep_regex::matcher] final regex: "\\bfoo(bar|baz)"
# search_reader
[2020-03-11T00:15:27Z TRACE grep_searcher::searcher] generic reader: searching via roll buffer strategy
[2020-03-11T00:15:27Z TRACE grep_searcher::searcher::core] searcher core: will use fast line searcher
- 14.785µs
# search_slice
[2020-03-11T00:15:27Z TRACE grep_searcher::searcher] slice reader: searching via slice-by-line strategy
[2020-03-11T00:15:27Z TRACE grep_searcher::searcher::core] searcher core: will use fast line searcher
- 5.716µs
```

---
