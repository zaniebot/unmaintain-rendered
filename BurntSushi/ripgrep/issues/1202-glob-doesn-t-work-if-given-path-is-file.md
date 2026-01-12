```yaml
number: 1202
title: "--glob doesn't work if given path is file"
type: issue
state: closed
author: mars90226
labels: []
assignees: []
created_at: 2019-02-22T03:14:06Z
updated_at: 2019-02-23T03:43:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1202
synced_at: 2026-01-12T16:13:23Z
```

# --glob doesn't work if given path is file

---

_@mars90226_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Use `cargo install ripgrep`.

#### What operating system are you using ripgrep on?

Distributor ID: Ubuntu
Description:    Ubuntu 18.04.2 LTS
Release:        18.04
Codename:       bionic

#### Describe your question, feature request, or bug.

rg --glob option should apply even when path is a file.

#### If this is a bug, what are the steps to reproduce the behavior?

Create a simple file
`$ echo 123 > test.txt`

Searching the file content with glob that excluding the file
```
$ rg --glob '!test.txt' 123 test.txt
1:123
```

#### If this is a bug, what is the actual behavior?

` $ rg --debug --glob '!test.txt' 123 test.txt`

```
DEBUG|grep_regex::literal|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.1/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(123)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.syntastic_*_config", re: "(?-u)^(?:/?|.*/)\\.syntastic_.*_config$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('s'), Literal('y'), Literal('n'), Literal('t'), Literal('a'), Literal('s'), Literal('t'), Literal('i'), Literal('c'), Literal('_'), ZeroOrMore, Literal('_'), Literal('c'), Literal('o'), Literal('n'), Literal('f'), Literal('i'), Literal('g')]) }
DEBUG|globset|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.eslintrc.*", re: "(?-u)^(?:/?|.*/)\\.eslintrc\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('e'), Literal('s'), Literal('l'), Literal('i'), Literal('n'), Literal('t'), Literal('r'), Literal('c'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|/root/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.2/src/lib.rs:429: built glob set; 0 literals, 23 basenames, 1 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 2 regexes
1:123
```

#### If this is a bug, what is the expected behavior?

`ripgrep` should output nothing, because the glob excludes the file.

I've tested the behavior of `grep`. Here's the result:
`$ grep --include '!test.txt' 123 test.txt`
outputs nothing.


---

_Comment by @okdana on 2019-02-22 03:29_

The manual says:

>Paths specified explicitly on the command line override glob and ignore rules.

I'm just guessing, but did this come up because you're trying to make ripgrep ignore some of the results of a shell glob expansion? If so, you might be able to just use multiple `-g` patterns (in place of the shell globs). Alternatively, your shell might be able to do something for you; for example, in zsh (with extended globbing enabled) you can do `rg mypattern foo*~foobar` to search all files starting with `foo` except `foobar`.

---

_Comment by @mars90226 on 2019-02-22 06:36_

I'm trying to use the following snippet from [here](https://stackoverflow.com/questions/42771468/git-grep-on-files-from-last-commit-only?rq=1). But I want to use `ripgrep` instead of `grep`.
```
git diff-tree -z --no-commit-id --name-only -r HEAD \
   | xargs -0 grep --include 'pattern' -- "regex"
```
I know it can be done by piping the filenames to `ripgrep` to grep the filename before searching file content like the following snippet. But it will need much more typing and is more difficult to remember.
```
git diff-tree -z --no-commit-id --name-only -r HEAD \
   | rg --null-data 'pattern' \
   | xargs -0 rg -- "regex"
```

---

_Comment by @BurntSushi on 2019-02-22 10:44_

This is intended behavior. Sorry.

---

_Closed by @BurntSushi on 2019-02-22 10:44_

---

_Comment by @mars90226 on 2019-02-23 03:43_

Ok, thanks for you response.

---
