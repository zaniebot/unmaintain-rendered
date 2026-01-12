```yaml
number: 3144
title: Gitignore rules not behaving correctly when inside a subdirectory
type: issue
state: closed
author: JelteF
labels:
  - rollup
assignees: []
created_at: 2025-09-10T08:30:21Z
updated_at: 2025-09-10T11:51:46Z
url: https://github.com/BurntSushi/ripgrep/issues/3144
synced_at: 2026-01-12T16:13:25Z
```

# Gitignore rules not behaving correctly when inside a subdirectory

---

_@JelteF_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

Latest `main` (but also with official 14.1.1 build)
```
❯ ../ripgrep/target/release/rg --version
ripgrep 14.1.1 (rev 119a58a400)

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

### How did you install ripgrep?

Both cargo and binary from github releases

### What operating system are you using ripgrep on?

Ubuntu 24.04

### Describe your bug.

The gitignore logic has a bug where it's ignoring files that should not be ignored when the current working directory is not at the root of the repo. 


### What are the steps to reproduce the behavior?

I created this [tiny git repo to show the bug](https://github.com/JelteF/ripgrep-ignore-failure). If you check out that repo and you run `rg xyz`, you will find the file `abc/cmd/abc/hello` when in the root of the repo. But when navigating to the `abc` directory that same `rg xyz` suddenly cannot find any files anymore and ripgrep tells you:

```
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

The structure of the repo:
```
.
├── abc
│   └── cmd
│       └── abc
│           └── hello
└── .gitignore
```

And the `.gitignore` file contains the single line `/abc/abc` (obviously in the actual case where I ran into this problem, that file exists and needed to be ignored.

### What is the actual behavior?

```
rg xyz --debug
rg: DEBUG|rg::flags::parse|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: jduck
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ripgrep-14.1.1/crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/jelte/dotfiles/git/gitignore_global
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.DS_Store?", re: "(?-u)^(?:/?|.*/)\\.DS_Store[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('S'), Literal('_'), Literal('S'), Literal('t'), Literal('o'), Literal('r'), Literal('e'), Any]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/Icon?", re: "(?-u)^(?:/?|.*/)Icon[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('I'), Literal('c'), Literal('o'), Literal('n'), Any]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.*.*.*.sw?", re: "(?-u)^(?:/?|.*/)\\.[^/]*\\.[^/]*\\.[^/]*\\.sw[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), ZeroOrMore, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Any]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.*.*.sw?", re: "(?-u)^(?:/?|.*/)\\.[^/]*\\.[^/]*\\.sw[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Any]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:448: glob converted to regex: Glob { glob: "**/.*.sw?", re: "(?-u)^(?:/?|.*/)\\.[^/]*\\.sw[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Any]) }
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:453: built glob set; 4 literals, 9 basenames, 19 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 6 regexes
rg: DEBUG|ignore::gitignore|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/jelte/opensource/ripgrep-ignore-failure/.gitignore
rg: DEBUG|globset|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/globset-0.4.16/src/lib.rs:453: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ignore-0.4.23/src/gitignore.rs:393: opened gitignore file: /home/jelte/opensource/ripgrep-ignore-failure/.git/info/exclude
rg: DEBUG|ignore::walk|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ignore-0.4.23/src/walk.rs:1799: ignoring ./cmd/abc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/jelte/opensource/ripgrep-ignore-failure/.gitignore"), original: "/abc/abc", actual: "abc/abc", is_whitelist: false, is_only_dir: false })))
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

This seems to be the problematic line from the debug output:
```
rg: DEBUG|ignore::walk|/home/jelte/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ignore-0.4.23/src/walk.rs:1799: ignoring ./cmd/abc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/jelte/opensource/ripgrep-ignore-failure/.gitignore"), original: "/abc/abc", actual: "abc/abc", is_whitelist: false, is_only_dir: false })))
```


### What is the expected behavior?

The `cmd/abc/hello` file should have been searched and returned a match.

---

_Comment by @BurntSushi on 2025-09-10 11:51_

Dupe of #829, #2731, #2747, #2778, #2836, #3144 

It was fixed by #2933.

---

_Closed by @BurntSushi on 2025-09-10 11:51_

---

_Label `rollup` added by @BurntSushi on 2025-09-10 11:51_

---
