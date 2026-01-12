```yaml
number: 1648
title: "-M not recognized if preceding pattern"
type: issue
state: closed
author: bedge
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-07-29T16:36:34Z
updated_at: 2023-11-21T04:51:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1648
synced_at: 2026-01-12T16:13:24Z
```

# -M not recognized if preceding pattern

---

_@bedge_

#### What version of ripgrep are you using?

[I] ➜ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

homebrew

#### What operating system are you using ripgrep on?

osx mojave


Give a high level description of the bug.

Syntax info says:
```
USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
```
So this should work:

```
[I] ➜ echo test | rg -M 900 st
error: The following required arguments were not provided:
    <PATTERN>
```

but it doesn't, one has to use:
```
[I] ➜ echo test | rg st -M 900
test
```

IOW, options before the pattern should be allowed, but it appears that at least the -M option only works if after the pattern.

---

_Comment by @BurntSushi on 2020-07-29 16:47_

I can't reproduce:

```
$ echo test | rg st -M 900
test
$ echo test | rg st -M900
test
$ echo test | rg -M 900 st
test
$ echo test | rg -M900 st
test
```

Do you have any aliases, wrapper scripts or config files being used? Can you show the same command, but with the `--debug` flag set?


---

_Label `question` added by @BurntSushi on 2020-07-29 16:47_

---

_Comment by @bedge on 2020-07-29 17:52_

No aliases:

[I] ➜ which -a rg
/usr/local/bin/rg


Here's a few permutations with --debug

```
[I] ➜ echo test | rg -M 900 st
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help


[I] ➜ echo test | rg --debug  -M 900 st
DEBUG|rg::config|crates/core/config.rs:40: /Users/edgeb1/.ripgreprc: arguments loaded from config file: ["--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}", "--debug", "-M", "900", "st"]
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help


[I] ➜ echo test | rg --debug  -M900 st
DEBUG|rg::config|crates/core/config.rs:40: /Users/edgeb1/.ripgreprc: arguments loaded from config file: ["--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}", "--debug", "-M900", "st"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ST), Complete(sT), Complete(ſT), Complete(St), Complete(st), Complete(ſt)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "git/*", re: "(?-u)^git/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*.sw[nop]", re: "(?-u)^(?:/?|.*/)[^/]*\\.sw[nop]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('n', 'n'), ('o', 'o'), ('p', 'p')] }]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "tmp/**/*", re: "(?-u)^tmp(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".history/*", re: "(?-u)^\\.history/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('h'), Literal('i'), Literal('s'), Literal('t'), Literal('o'), Literal('r'), Literal('y'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".notes/*", re: "(?-u)^\\.notes/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('n'), Literal('o'), Literal('t'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/.junk/*", re: "(?-u)^(?:/?|.*/)\\.junk/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('j'), Literal('u'), Literal('n'), Literal('k'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 5 literals, 10 basenames, 1 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 5 regexes
test

[I] ➜ echo test | rg --debug st  -M900
DEBUG|rg::config|crates/core/config.rs:40: /Users/edgeb1/.ripgreprc: arguments loaded from config file: ["--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}", "--debug", "st", "-M900"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ST), Complete(sT), Complete(ſT), Complete(St), Complete(st), Complete(ſt)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "git/*", re: "(?-u)^git/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*.sw[nop]", re: "(?-u)^(?:/?|.*/)[^/]*\\.sw[nop]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('n', 'n'), ('o', 'o'), ('p', 'p')] }]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "tmp/**/*", re: "(?-u)^tmp(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".history/*", re: "(?-u)^\\.history/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('h'), Literal('i'), Literal('s'), Literal('t'), Literal('o'), Literal('r'), Literal('y'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".notes/*", re: "(?-u)^\\.notes/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('n'), Literal('o'), Literal('t'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/.junk/*", re: "(?-u)^(?:/?|.*/)\\.junk/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('j'), Literal('u'), Literal('n'), Literal('k'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 5 literals, 10 basenames, 1 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 5 regexes
test

[I] ➜ echo test | rg --debug st  -M 900
DEBUG|rg::config|crates/core/config.rs:40: /Users/edgeb1/.ripgreprc: arguments loaded from config file: ["--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--max-columns=150", "--max-columns-preview", "--glob=!git/*", "--colors=line:style:bold", "--smart-case", "--type-add", "tfv:*.{tf,tfvars}", "--debug", "st", "-M", "900"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ST), Complete(sT), Complete(ſT), Complete(St), Complete(st), Complete(ſt)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "git/*", re: "(?-u)^git/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('g'), Literal('i'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*.sw[nop]", re: "(?-u)^(?:/?|.*/)[^/]*\\.sw[nop]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('n', 'n'), ('o', 'o'), ('p', 'p')] }]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "tmp/**/*", re: "(?-u)^tmp(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".history/*", re: "(?-u)^\\.history/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('h'), Literal('i'), Literal('s'), Literal('t'), Literal('o'), Literal('r'), Literal('y'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: ".notes/*", re: "(?-u)^\\.notes/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('n'), Literal('o'), Literal('t'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/.junk/*", re: "(?-u)^(?:/?|.*/)\\.junk/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('j'), Literal('u'), Literal('n'), Literal('k'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 5 literals, 10 basenames, 1 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 5 regexes
test

```

---

_Comment by @bedge on 2020-07-29 17:54_

Found the culprit, my ~/.ripgreprc has:

```
# Don't let ripgrep vomit really long lines to my terminal, and show a preview.
--max-columns=150
--max-columns-preview
```
commenting the `--max-columns=150` fixes the problem, but I would still call the behavior a bug.



---

_Comment by @BurntSushi on 2020-07-29 18:03_

Aye, I see it now. Yes, it is a bug, but is almost certainly in `clap`, which is the argv parser that ripgrep uses. I wonder if it might be related to #884. The next step for this issue is for someone to create a minimal Rust program that reproduces the clap bug and then report it upstream. I doubt I'll do this personally any time soon.

FWIW, if you use `-M900` instead of `-M 900`, then it works. (Which also confirms for me that this is a bug in clap.)

---

_Label `question` removed by @BurntSushi on 2020-07-29 18:03_

---

_Label `bug` added by @BurntSushi on 2020-07-29 18:03_

---

_Comment by @bedge on 2020-07-29 19:06_

Thanks for the help, makes perfect sense. Just gave me a WTF moment so I had to pass along.

---

_Comment by @ssent1 on 2022-11-12 20:05_

I also got the desired behaviour by adding `=` or removing the `<Space>` character.

It's not clear whether there is an unambiguous bug. But, updating the manpage to show:

> -MNUM --max-columns=**_NUM_**

instead of `-M, --max-columns NUM` makes the patterns that work clear.

NB: `-M NUM` and `--max-columns=NUM` seem to be the more intuitive CLI patterns.

---

_Comment by @BurntSushi on 2022-11-12 20:07_

No, it's definitely a bug. I rarely use `-x N` or `--flag=value` myself. I usually write `-xN` or `--flag value`.

---

_Label `rollup` added by @BurntSushi on 2023-11-21 00:59_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
