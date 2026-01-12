```yaml
number: 2487
title: folders starting with dots are always excluded from searches?
type: issue
state: closed
author: donhcd
labels:
  - invalid
assignees: []
created_at: 2023-04-12T20:49:11Z
updated_at: 2023-04-12T20:55:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2487
synced_at: 2026-01-12T16:13:24Z
```

# folders starting with dots are always excluded from searches?

---

_@donhcd_

#### What version of ripgrep are you using?

```
sh-3.2$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

OSX 13.2.1

#### Describe your bug.

I cannot get any search results for any files in directories starting with a dot. Searching within the dotdirectory does work.

#### What are the steps to reproduce the behavior?

see below

#### What is the actual behavior?

```
sh-3.2$ find .
.
./.dont-ignore-please
./.dont-ignore-please/f.txt
sh-3.2$ cat .dont-ignore-please/f.txt
hereismystring
sh-3.2$ rg --no-hidden --debug string
DEBUG|grep_regex::literal|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.11/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(string)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:426: glob converted to regex: Glob { glob: "**/.*.sw*", re: "(?-u)^(?:/?|.*/)\\.[^/]*\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:426: glob converted to regex: Glob { glob: "**/.DS_Store?", re: "(?-u)^(?:/?|.*/)\\.DS_Store[^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('D'), Literal('S'), Literal('_'), Literal('S'), Literal('t'), Literal('o'), Literal('r'), Literal('e'), Any]) }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:426: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 6 basenames, 17 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*.sw*", re: "(?-u)^(?:/?|.*/)[^/]*\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:426: glob converted to regex: Glob { glob: "**/*.sw*", re: "(?-u)^(?:/?|.*/)[^/]*\\.sw[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), ZeroOrMore]) }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 3 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.20/src/walk.rs:1740: ignoring ./.dont-ignore-please: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
sh-3.2$ rg --no-ignore --debug string
DEBUG|grep_regex::literal|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.11/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(string)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.20/src/walk.rs:1740: ignoring ./.dont-ignore-please: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/Users/don/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.10/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
sh-3.2$ cd .dont-ignore-please/
sh-3.2$ rg string
f.txt
1:hereismystring
```

#### What is the expected behavior?

What do you think ripgrep should have done?
`rg --no-hidden string` should have returned a result in f.txt

---

_Comment by @BurntSushi on 2023-04-12 20:54_

Why are you using `--no-hidden`? `--hidden` is what causes ripgrep to search hidden files and directories, as documented:

```
       -., --hidden
           Search hidden files and directories. By default,
           hidden files and directories are skipped. Note that
           if a hidden file or a directory is whitelisted in an
           ignore file, then it will be searched even if this
           flag isn’t provided.

           A file or directory is considered hidden if its base
           name starts with a dot character (.). On operating
           systems which support a hidden file attribute, like
           Windows, files with this attribute are also
           considered hidden.

           This flag can be disabled with --no-hidden.
```

`--no-hidden` is used to override `--hidden`, or in other words, restore default behavior.

And `--no-ignore` disables gitignore handling. It doesn't have anything to do with hidden files or directories.

---

_Closed by @BurntSushi on 2023-04-12 20:54_

---

_Label `invalid` added by @BurntSushi on 2023-04-12 20:55_

---
