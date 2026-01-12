```yaml
number: 1482
title: ripgrep seems to be interpreting .gitignore differently from git
type: issue
state: closed
author: kbarrette
labels:
  - duplicate
assignees: []
created_at: 2020-02-11T15:15:38Z
updated_at: 2020-02-11T15:51:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1482
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep seems to be interpreting .gitignore differently from git

---

_@kbarrette_

#### What version of ripgrep are you using?

```bash
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Hombrew: `brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS Catalina 10.15.3

#### Describe your question, feature request, or bug.

ripgrep is not finding files that git does find, seemingly related to .gitignore

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
$ cat .gitignore
bar
$ ls foo/bar
baz
$ rg --files
$ git status -s
 M foo/bar/baz
$
```

#### If this is a bug, what is the actual behavior?

```bash
$ rg --files --debug
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/fishd*", re: "(?-u)^(?:/?|.*/)fishd[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('f'), Literal('i'), Literal('s'), Literal('h'), Literal('d'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 2 literals, 5 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/fishd*", re: "(?-u)^(?:/?|.*/)fishd[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('f'), Literal('i'), Literal('s'), Literal('h'), Literal('d'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 2 literals, 5 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./foo/bar: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "bar", actual: "**/bar", is_whitelist: false, is_only_dir: false })))
$
```

#### If this is a bug, what is the expected behavior?

`rg --files` finds no files, while `git status` finds one (modified) file. ripgrep seems to be ignoring `foo/bar/baz` while git finds it - expected behavior is for `rg` and `git` to find the same files.



---

_Comment by @kbarrette on 2020-02-11 15:23_

Upon further examination, it seems that rg and git are probably interpreting `.gitignore` the same way, but since the file is already tracked in git, it's showing it as modified.

ripgrep is probably behaving as designed, it was just unexpected for me in this case.

---

_Comment by @BurntSushi on 2020-02-11 15:50_

Right. ripgrep doesn't know anything about which files are or aren't tracked. The best thing to do here is to make sure your `.gitignore` files are in sync with what's actually tracked.

---

_Closed by @BurntSushi on 2020-02-11 15:50_

---

_Label `duplicate` added by @BurntSushi on 2020-02-11 15:51_

---
