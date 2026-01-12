```yaml
number: 973
title: rg not excluding full path, only relative path?
type: issue
state: closed
author: rieje
labels:
  - wontfix
assignees: []
created_at: 2018-07-07T17:27:17Z
updated_at: 2018-07-08T01:42:05Z
url: https://github.com/BurntSushi/ripgrep/issues/973
synced_at: 2026-01-12T16:13:22Z
```

# rg not excluding full path, only relative path?

---

_@rieje_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

I'm pretty sure it's not a bug, because the problem I'm experiencing is so simple. I can't exclude the path `~/system-admin/log` which contains a bunch of log files I don't want `rg` to search. `rg --no-heading -g '!*/log' db ~/system-admin/` works, but `rg --no-heading -g '!/home/rieje/system-admin/log' db ~/system-admin/` doesn't.

Thanks.


---

_Comment by @okdana on 2018-07-07 18:10_

As far as i know, all glob/ignore paths that `rg` deals with are relative to the search directory, and there's no way to change that. See also #969 for example. So using an absolute path in a glob will only ever work when you're searching `/` (or some other directory tree that happens to replicate that structure).

`!**/log/` should work from anywhere (assuming you're OK with excluding all directories named `log`), or, if you're trying to be more specific, you can anchor to the root of the search path with `!/log/`.

---

_Comment by @BurntSushi on 2018-07-08 01:40_

@okdana is correct. This fundamentally cannot work. Firstly, it would require ripgrep to canonicalize every path it sees. This would be a dramatic performance regression. Secondly, even if we could isolate that performance regression to cases where absolute file paths are set in the globs, then you still run into problems like, "what if the absolute path you specified contains a symlink in it?" The canonicalization is, by definition, not going to contain any of those symlinks and thus your glob will never work. Thirdly and finally, even detecting the absolute path is itself tricky since `gitignore` semantics already specify a meaning for patterns that begin with a `/` that is incompatible with `/` actually meaning "the root of the file system."

In other words, you need to find some other way to ignore your directory. The simplest possible solution that should work regardless of where you run ripgrep is the following simple command:

```
$ echo '/log' > /home/reije/system-admin/.ignore
```

By default, ripgrep will respect that `.ignore` file which should prevent `/home/reije/system-admin/log` and _only_ `/home/reije/system-admin/log` from being searched.

If you need/want to use the `-g/--glob` flag to achieve this, then I believe @okdana's solutions are the best you can do.

---

_Closed by @BurntSushi on 2018-07-08 01:40_

---

_Label `wontfix` added by @BurntSushi on 2018-07-08 01:40_

---
