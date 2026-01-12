```yaml
number: 1174
title: "**/**/* does not match a/foo.rs (instead, it will only ever match an absolute path)"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2019-01-23T23:26:40Z
updated_at: 2019-01-24T00:18:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1174
synced_at: 2026-01-12T16:13:23Z
```

# **/**/* does not match a/foo.rs (instead, it will only ever match an absolute path)

---

_@BurntSushi_

Originally reported by @okdana in https://github.com/BurntSushi/ripgrep/pull/1093#issuecomment-433691732

#### What version of ripgrep are you using?

master @ b48bbf52

#### How did you install ripgrep?

Compiled.

#### What operating system are you using ripgrep on?

Linux.

#### Describe your question, feature request, or bug.

The `**/**/*` pattern does not match `a/foo.rs` while git does. Similarly for `**/**/**/*` and so on.

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ mkdir /tmp/rgbug
$ cd /tmp/rgbug
$ git init
$ echo '**/**/*' > .gitignore
$ mkdir a
$ echo test > a/foo
$ rg test
foo
1:test
```

#### If this is a bug, what is the actual behavior?

`foo` is shown as a search result, but it should be ignored. The output of `--debug` shows this line:

```
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/**/*", re: "(?-u)^(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursiveZeroOrMore, ZeroOrMore]) }
```

The glob-to-regex  conversion here appears wrong, since `(?-u)^(?:/|/.*/)[^/]*$` can only ever match paths that start with a `/`.

#### If this is a bug, what is the expected behavior?

`foo` should have been ignored by the `**/**/*` pattern.


---

_Label `bug` added by @BurntSushi on 2019-01-23 23:26_

---

_Closed by @BurntSushi on 2019-01-24 00:18_

---
