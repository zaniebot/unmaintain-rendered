```yaml
number: 2646
title: "`--pre`/`--pre-glob` doesn't seem to work in config file"
type: issue
state: closed
author: zgracem
labels:
  - question
assignees: []
created_at: 2023-11-15T17:27:10Z
updated_at: 2023-11-22T16:58:42Z
url: https://github.com/BurntSushi/ripgrep/issues/2646
synced_at: 2026-01-12T16:13:24Z
```

# `--pre`/`--pre-glob` doesn't seem to work in config file

---

_@zgracem_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 14.1.1 (23B81)

### Describe your bug.

I followed the [instructions](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#preprocessor) for creating a preprocessor. It works as advertised when specified on the command line, but stops working if I move the `--pre` and `--pre-glob` options into ripgrep's configuration file.

### What are the steps to reproduce the behavior?

```sh
$ cat "$RIPGREP_CONFIG_PATH"
--pre=pre-rg
--pre-glob='*.pdf'

$ cat "$(type -P pre-rg)"
#!/usr/bin/env bash
exec pdftotext "$1" -
```

### What is the actual behavior?

```sh
# options on command line :)
$ rg --debug --no-config --pre=pre-rg --pre-glob='*.pdf' overwrite test.pdf
DEBUG|rg::args|crates/core/args.rs:527: not reading config files because --no-config is present
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(overwrite)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "tmp/**/*", re: "(?-u)^tmp(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/Icon?", re: "(?-u)^(?:/?|.*/)Icon[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('I'), Literal('c'), Literal('o'), Literal('n'), Any]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 19 basenames, 5 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 4 regexes
278:existing file, you are prompted to overwrite, rename,
307:item, you are prompted to overwrite, rename,
381:Warning! This operation overwrites the entire memory in

# options in config file :(
$ rg --debug overwrite test.pdf || echo nope
﻿DEBUG|rg::config|crates/core/config.rs:40: /Users/zozo/.config/ripgrep/ripgreprc: arguments loaded from config file: ["--pre", "pre-rg", "--pre-glob", "\'*.pdf\'"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--pre", "pre-rg", "--pre-glob", "\'*.pdf\'", "--debug", "overwrite", "test.pdf"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(overwrite)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "tmp/**/*", re: "(?-u)^tmp(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/Icon?", re: "(?-u)^(?:/?|.*/)Icon[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('I'), Literal('c'), Literal('o'), Literal('n'), Any]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 19 basenames, 5 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 4 regexes
nope
```

### What is the expected behavior?

```sh
# with the config file and script as above
$ rg overwrite test.pdf
278:existing file, you are prompted to overwrite, rename,
307:item, you are prompted to overwrite, rename,
381:Warning! This operation overwrites the entire memory in
```

---

_Comment by @BurntSushi on 2023-11-22 00:15_

The debug output answers it
 Your glob has quotes in it. The quotes are for your shell, but your config file is not a shell. Remove the quotes and it should work.

---

_Closed by @BurntSushi on 2023-11-22 00:15_

---

_Label `question` added by @BurntSushi on 2023-11-22 00:15_

---

_Comment by @zgracem on 2023-11-22 16:24_

Ah, okay. I'll have to take your word for it about the debug output (I don't know enough about Rust to parse it) but I wonder if the documentation could be clearer? From reading this—

> 1. Every line is a shell argument, after trimming whitespace.

—and this—

> In particular, there is no escaping. Each line is given to ripgrep as a single command line argument verbatim.

—I (incorrectly) assumed that each line in the config file would be treated as a verbatim command line argument, except for whitespace, and would therefore need the same treatment w/r/t quotes as entering it on the actual command line.

The only place in the guide where quotes are even mentioned is here—

> Note that we put `'*.toml'` in single quotes to prevent our shell from expanding the `*`.

—which probably also contributed to my misunderstanding.

In any case, it works without the quotes, so thanks for letting me know. 👍️

---

_Comment by @BurntSushi on 2023-11-22 16:58_

> Ah, okay. I'll have to take your word for it about the debug output (I don't know enough about Rust to parse it)

I don't think you really need to know much if anything about Rust. Here, from the output you pasted:

```
﻿DEBUG|rg::config|crates/core/config.rs:40: /Users/zozo/.config/ripgrep/ripgreprc: arguments loaded from config file: ["--pre", "pre-rg", "--pre-glob", "\'*.pdf\'"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--pre", "pre-rg", "--pre-glob", "\'*.pdf\'", "--debug", "overwrite", "test.pdf"]
```

Notice that the value after `"--pre-glob"` is `"\'*.pdf\'"`. So the value that ripgrep sees contains single quotes. If you pass it via a shell and not a config argument, then your shell will see interpret the quotes and pass the raw value to the process invocation.

> each line in the config file would be treated as a verbatim command line argument, except for whitespace, and would therefore need the same treatment w/r/t quotes as entering it on the actual command line.

Yes. Each line is treated as a verbatim command line argument after trimming whitespace. But there's no whitespace here? In any case, to be doubly clear, this config file

```
--pre=pre-rg
--pre-glob=*.pdf
```

is equivalent to

```
--pre
pre-rg
--pre-glob
*.pdf
```

---
