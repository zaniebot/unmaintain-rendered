```yaml
number: 2236
title: "rg/globset raises error for \"dangling '\\'\" in .gitignore"
type: issue
state: closed
author: mike-hart
labels: []
assignees: []
created_at: 2022-06-14T10:53:16Z
updated_at: 2022-06-14T15:35:46Z
url: https://github.com/BurntSushi/ripgrep/issues/2236
synced_at: 2026-01-12T16:13:24Z
```

# rg/globset raises error for "dangling '\'" in .gitignore

---

_@mike-hart_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo install

#### What operating system are you using ripgrep on?

AMI Linux 1

#### Describe your bug.

Several of our repos have a pattern in their `.gitignore` file which `git grep` is fine with, but `rg` shows an error for.

#### What are the steps to reproduce the behavior?

```bash
git init foo && cd foo
echo "^\.git\/" >.gitignore
rg sometext
```

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

If the output is small, put it in code fences:

```
DEBUG|grep_regex::literal|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.9/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(sometext)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:416: glob converted to regex: Glob { glob: "**/Icon*", re: "(?-u)^(?:/?|.*/)Icon[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('I'), Literal('c'), Literal('o'), Literal('n'), ZeroOrMore]) }
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:416: glob converted to regex: Glob { glob: ".coverage.*", re: "(?-u)^\\.coverage\\.[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('.'), Literal('c'), Literal('o'), Literal('v'), Literal('e'), Literal('r'), Literal('a'), Literal('g'), Literal('e'), Literal('.'), ZeroOrMore]) }
DEBUG|globset|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.8/src/lib.rs:421: built glob set; 12 literals, 8 basenames, 10 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 2 regexes
./.gitignore: line 1: error parsing glob '^\.git\/': dangling '\'
DEBUG|ignore::walk|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.18/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|/home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.18/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

#### What is the expected behavior?

Ripgrep/globset should not consider this an error as `git grep` does not.

---

_Closed by @BurntSushi on 2022-06-14 14:40_

---

_Comment by @BurntSushi on 2022-06-14 14:41_

This is fixed.

It's worth pointing out that escaping a `/` is superfluous. So while it's technically permissible, it isn't accomplishing anything by being there.

---

_Comment by @mike-hart on 2022-06-14 15:35_

Although I appreciate it's superfluous, I don't manage (or have sufficient input-into/control-of) all the repositories I read from. As it's permissible it's great that it's no longer an error, thank you.

---
