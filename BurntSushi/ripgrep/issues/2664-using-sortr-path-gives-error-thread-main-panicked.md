```yaml
number: 2664
title: "Using `--sortr path` gives error \"thread 'main' panicked\"/\"not yet implemented\""
type: issue
state: closed
author: kjhaber
labels:
  - bug
assignees: []
created_at: 2023-11-28T05:15:40Z
updated_at: 2023-11-29T18:09:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2664
synced_at: 2026-01-12T16:13:24Z
```

# Using `--sortr path` gives error "thread 'main' panicked"/"not yet implemented"

---

_@kjhaber_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.2 (rev 6c7947b819)

features:-simd-accel,+pcre2
simd(compile):+SSE2,+SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,-AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

Homebrew (Mac, Apple Silicon) and asdf

### What operating system are you using ripgrep on?

macOS Sonoma 14.1.1

### Describe your bug.

When I use the `--sortr path` option, rg fails with:

```
thread 'main' panicked at crates/core/flags/hiargs.rs:774:35:
not yet implemented
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The error doesn't happen for the non-reversed `--sort path`.  It also doesn't happen for other options to --sort or --sortr (accessed, created, modified, none).

I'm able to reproduce the error with ripgrep 14.0.0, 14.0.1, and 14.0.2.  I can't reproduce the error in ripgrep 13.0.0.


### What are the steps to reproduce the behavior?

Run `rg --sortr path bar`.


### What is the actual behavior?

When I use the `--sortr path` option, rg fails with:

```
% rg --sortr path bar
thread 'main' panicked at crates/core/flags/hiargs.rs:774:35:
not yet implemented
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
Looks like it's pointing to the `todo!` on https://github.com/BurntSushi/ripgrep/blob/ca5e294ad6b64b03c1d2fb4d9ed8c32b73e656ae/crates/core/flags/hiargs.rs#L774C1-L774C43 

The error doesn't happen for the non-reversed `--sort path`.  It also doesn't happen for other options to --sort or --sortr (accessed, created, modified, none).

I'm able to reproduce the error with ripgrep 14.0.0, 14.0.1, and 14.0.2.  I can't reproduce the error in ripgrep 13.0.0.

With --debug flag:
```
% rg --sortr path --debug bar
DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1093: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1103: heuristic chose to search ./
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1254: found hostname for hyperlink configuration: lacerta.local
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1264: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.DS_Store?", re: "(?-u)^(?:/?|.*/)\\.DS_Store[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('S'), Literal('_'), Literal('S'), Literal('t'), Literal('o'), Literal('r'), Literal('e'), Any]) }
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/Icon?", re: "(?-u)^(?:/?|.*/)Icon[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('I'), Literal('c'), Literal('o'), Literal('n'), Any]) }
DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.sw*", re: "(?-u)^(?:/?|.*/)\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 2 literals, 20 basenames, 15 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 4 regexes
thread 'main' panicked at crates/core/flags/hiargs.rs:774:35:
not yet implemented
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```


### What is the expected behavior?

Ripgrep should return the matching results reverse-sorted by path.

---

_Closed by @BurntSushi on 2023-11-28 21:17_

---

_Comment by @kjhaber on 2023-11-29 05:07_

Confirming that the fix in 14.0.3 is working for me.  Thanks so much for ripgrep and the fast fix!

---

_Label `bug` added by @BurntSushi on 2023-11-29 12:48_

---

_Comment by @redblobgames on 2023-11-29 18:08_

Thank you! Confirming that the fix is working for me as well.

---
