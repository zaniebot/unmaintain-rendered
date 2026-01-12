```yaml
number: 2770
title: ripgrep does not properly apply ignore rules from .gitignore in parent directory
type: issue
state: closed
author: gstokkink
labels:
  - rollup
assignees: []
created_at: 2024-03-28T13:21:49Z
updated_at: 2025-10-11T02:07:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2770
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep does not properly apply ignore rules from .gitignore in parent directory

---

_@gstokkink_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.42 is available (JIT is available)
```

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 14.3.1

### Describe your bug.

ripgrep does not properly apply a glob pattern from a .gitignore in the parent directory

### What are the steps to reproduce the behavior?

```
mkdir test
cd test
git init
mkdir -p foo/bar
echo quux > foo/bar/baz

rg -l quux # => foo/bar/baz, as expected

cd foo

rg -l quux # => bar/baz, as expected

cd ..

echo "**/bar" > .gitignore

rg -l quux # => no results, as expected

cd foo

rg -l quux # => no results, as expected

cd ..

echo "**/bar/*" > .gitignore

rg -l quux # => no results, as expected

cd foo

rg -l quux # => bar/baz ??
```

As you can see, ripgrep does not apply the ignore glob correctly in the last case.

### What is the actual behavior?

```
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(FilesWithMatches))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: gerjan-macbook-pro.local
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 11 thread(s)
rg: DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: "**/bar/*", re: "(?-u)^(?:/?|.*/)bar/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([RecursivePrefix, Literal('b'), Literal('a'), Literal('r'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
bar/baz
```

### What is the expected behavior?

ripgrep should not have returned any matches

---

_Comment by @MrGreenTea on 2025-10-09 08:46_

This seems related: #3067 

---

_Label `rollup` added by @BurntSushi on 2025-10-11 01:11_

---

_Closed by @BurntSushi on 2025-10-11 02:07_

---
