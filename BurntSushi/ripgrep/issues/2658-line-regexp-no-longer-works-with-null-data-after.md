```yaml
number: 2658
title: "`--line-regexp` no longer works with `--null-data` after upgrading to 14.0.1"
type: issue
state: closed
author: Frederick888
labels:
  - rollup
assignees: []
created_at: 2023-11-27T13:24:27Z
updated_at: 2023-11-28T02:17:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2658
synced_at: 2026-01-12T16:13:24Z
```

# `--line-regexp` no longer works with `--null-data` after upgrading to 14.0.1

---

_@Frederick888_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.1

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

Arch Linux official repository.

### What operating system are you using ripgrep on?

Arch Linux x86_64 (rolling)

### Describe your bug.

Previously using ripgrep 13.0.0, when `--null-data` was given, ripgrep would basically treat NUL the same as an end of a line, and `--line-regexp` could be used to match a whole line.

After upgrading to 14.0.1, it no longer matches.

### What are the steps to reproduce the behavior?

`printf '%s\0' 'foo' 'bar' 'hello' 'world' | rg --null-data --line-regexp 'foo'`

### What is the actual behavior?

It prints nothing to stdout using ripgrep 14.0.1.

```
DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1093: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1106: heuristic chose to search stdin
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1254: found hostname for hyperlink configuration: FredArch
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1264: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.DS_*", re: "(?-u)^(?:/?|.*/)\\.DS_[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('S'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 28 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

### What is the expected behavior?

It prints `foo`, like 13.0.0 used to do. (I did not see relevant breaking changes in 14.0.0's release announcement.)

---

Ignoring some dependency patches, `git bisect` pointed me to 1035f6b1ff502eb5b1a5fc49a79f45971c772d47.

Bisect script used:
```sh
#!/usr/bin/env bash

pushd "~/Programming/Rust/ripgrep" >/dev/null

function cleanup() {
    git reset --hard
    popd >/dev/null
}

trap 'cleanup' EXIT

cat Cargo.toml | rg -v /home/andrew | sponge Cargo.toml

if ! cargo build; then
    exit 125
fi

if ! printf '%s\0' foo bar hello world | cargo run -- --line-regexp --null-data bar >/dev/null; then
    exit 1
fi

exit 0
```

```
# bad: [cd5440fb6230f72ab598916c1c5ab96686541d47] changelog: fix wording
# good: [af6b6c543b224d348a8876f0c06245d9ea7929c5] 13.0.0
git bisect start 'master' '13.0.0'
# bad: [c9584b035b19244e370a50fd872a3ae2039e2931] ci/release: use GitHub CLI
git bisect bad c9584b035b19244e370a50fd872a3ae2039e2931
# good: [7f23cd63a51d45415bb3df28d053562844194767] ignore/types: add automated test for sortedness
git bisect good 7f23cd63a51d45415bb3df28d053562844194767
# good: [335aa4937aed8f9c1bf3f5722e8fc4d671d46dcb] ignore/types: add *.pyi for Python
git bisect good 335aa4937aed8f9c1bf3f5722e8fc4d671d46dcb
# bad: [a68db3ac02fbfa2154cb2c8029c39e89d24c2792] deps: drop temporary patch and move to bstr 1.6
git bisect bad a68db3ac02fbfa2154cb2c8029c39e89d24c2792
# bad: [51480d57a67c229d624e5d1edb4aa3782e883696] regex: simplify AST analysis a bit
git bisect bad 51480d57a67c229d624e5d1edb4aa3782e883696
# good: [a7f1276021df2217dead1481b2c2b38595ed8fb3] readme: update Debian instructions
git bisect good a7f1276021df2217dead1481b2c2b38595ed8fb3
# bad: [e028ea37928930c80e5c3172d1df306b85a86758] regex: migrate grep-regex to regex-automata
git bisect bad e028ea37928930c80e5c3172d1df306b85a86758
# bad: [1035f6b1ff502eb5b1a5fc49a79f45971c772d47] deps: initial migration steps to regex 1.9
git bisect bad 1035f6b1ff502eb5b1a5fc49a79f45971c772d47
# first bad commit: [1035f6b1ff502eb5b1a5fc49a79f45971c772d47] deps: initial migration steps to regex 1.9
```

---

_Comment by @BurntSushi on 2023-11-28 02:02_

Oh this is a nasty one. It turns out that ripgrep 13 was only working accidentally here. For example, I can make it fail:

```
$ printf '%s\0' 'bar' 'hello' 'world' | rg-13.0.0 --null-data --line-regexp '\w{3}'
$
```

And I can even make ripgrep 14 succeed with a sufficiently wacky regex:

```
$ printf '%s\0' 'foo' 'bar' 'hello' 'world' | rg --null-data --line-regexp '[A-Z]*bar|[A-Y]*blahblah'
bar%
```

What is happening here are literal optimizations. When searching for `foo` with ripgrep 13, ripgrep builds a regex like `(?m:^)foo(?m:$)`, and because the old regex engine couldn't handle literal optimizations involving line anchors, ripgrep plucked out `foo` as an inner literal and scanned for that first. Once it found a "line" containing `foo`, it _strips the line terminators_ and then runs the full regex. Thus, it finds a match.

But in ripgrep 14, the regex engine is a lot more sophisticated and can handle the literal optimization on its own. So ripgrep backs off and let's it do its thing. But of course, in this context, the regex engine is running on potentially many lines at once, and so the `(?m:^)` and `(?m:$)` really need to be able to match. By default, they only match `\n`. And thus, no match is found because NUL is used as a line terminator.

Thankfully, in the rewrite of the regex engine, I added a new [config option to set the line terminator used by `(?m:^)` and `(?m:$)`](https://docs.rs/regex-automata/latest/regex_automata/meta/struct.Config.html#method.line_terminator). Indeed, I did it for _exactly_ this use case. I just haven't gotten around to making ripgrep use that option yet. Its handling of line terminators is pretty twisted and needs to be unfucked.

I think as a stop-gap, I'll make ripgrep divert to the "slower" line-by-line search when `--null-data` is used. That will ensure correct operation at least.

---

_Label `rollup` added by @BurntSushi on 2023-11-28 02:09_

---

_Closed by @BurntSushi on 2023-11-28 02:17_

---
