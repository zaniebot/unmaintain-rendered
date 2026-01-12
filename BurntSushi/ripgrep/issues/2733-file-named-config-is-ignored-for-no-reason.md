```yaml
number: 2733
title: "File named `.config` is ignored for no reason"
type: issue
state: closed
author: Mouradif
labels:
  - question
assignees: []
created_at: 2024-02-11T10:48:30Z
updated_at: 2024-02-11T12:45:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2733
synced_at: 2026-01-12T16:13:24Z
```

# File named `.config` is ignored for no reason

---

_@Mouradif_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

APT, Homebrew

### What operating system are you using ripgrep on?

Ubuntu 22, Mac OS

### Describe your bug.

Ripgrep ignores files named `.config` and I can't seem to be able to change that. I tried `--no-ignore`, `--no-ignore-files`, `--no-ignore-global`

### What are the steps to reproduce the behavior?

- create a file `.config`
- put some text in it (like "some text")
- run `rg some` or `rg text`
- find nothing

Also

- `rg --debug anything`
```
...
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.config: Ignore(IgnoreMatch(Hidden))
...
```

### What is the actual behavior?

```
$ mkdir folder
$ cd folder
$ echo 'some text' > .config
$ rg some
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
$ rg --debug some
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: Mourads-MacBook-Air.local
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".gitignored/*", re: "(?-u)^\\.gitignored/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('g'), Literal('i'), Literal('t'), Literal('i'), Literal('g'), Literal('n'), Literal('o'), Literal('r'), Literal('e'), Literal('d'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 9 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:448: glob converted to regex: Glob { glob: ".gitignored/*", re: "(?-u)^\\.gitignored/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('g'), Literal('i'), Literal('t'), Literal('i'), Literal('g'), Literal('n'), Literal('o'), Literal('r'), Literal('e'), Literal('d'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 9 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.config: Ignore(IgnoreMatch(Hidden))
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

### What is the expected behavior?

Find a match in file `.config`

---

_Comment by @BurntSushi on 2024-02-11 12:44_

The second sentence of the README says (emphasis mine):

> By default, ripgrep will respect gitignore rules and automatically **skip hidden files/directories** and binary files.

The GUIDE [explicitly discusses what types of automatic filtering are employed](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) and also how to disable or configure each one.

Finally, the debug logs you've included explicitly tell you the file is being ignored because it is hidden.

Use the `-./--hidden` flag to search hidden files.

---

_Closed by @BurntSushi on 2024-02-11 12:44_

---

_Label `question` added by @BurntSushi on 2024-02-11 12:45_

---
