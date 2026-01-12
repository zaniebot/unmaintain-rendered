```yaml
number: 1050
title: Directory paths are ignored even if negative-ignore exists for files underneath it
type: issue
state: open
author: infinity0
labels:
  - bug
  - gitignore
assignees: []
created_at: 2018-09-13T07:42:13Z
updated_at: 2024-12-16T14:48:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1050
synced_at: 2026-01-12T16:13:22Z
```

# Directory paths are ignored even if negative-ignore exists for files underneath it

---

_@infinity0_

Running over https://salsa.debian.org/rust-team/debcargo-conf/:

~~~~
$ cat .gitignore
/build
/src/*/*
!/src/*/debian

$ rg -q copyright 
$ rg -q copyright src
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
1
$ rg -q copyright src/*

$ git grep -q copyright 
$ git grep -q copyright src
$ git grep -q copyright src/*
~~~~


---

_Label `bug` added by @BurntSushi on 2018-09-13 10:56_

---

_Comment by @BurntSushi on 2018-09-13 11:01_

Yeah, this is one of the cases where it's tough for ripgrep to behave the same as git here. I think there have been bugs reported on this issue in the past, but I can't find them.

The fundamental issue is that `/src/*/*` matches `src/atty` (for example), and therefore, that directory is ignored and ripgrep doesn't descend into it. So the `!/src/*/debian` whitelist rule isn't even applied.

Two thoughts:

1. Should `src/atty` actually match `/src/*/*`? There might be some special handling going on with the trailing slash that allows it to match. Not sure.
2. Maybe ripgrep either needs to get smarter, or it cannot implement the optimization of avoiding to descend into directories that have been ignored.

---

_Comment by @infinity0 on 2018-09-15 16:41_

> Should src/atty actually match /src/*/*?

No, \*/\* is pretty explicit about only matching 2 levels down. I think this would take it further away from the behaviour of git.

> Maybe ripgrep either needs to get smarter, or it cannot implement the optimization of avoiding to descend into directories that have been ignored.

I think this would be the correct approach, how about not implementing the optimisation if there exists a ! whitelist rule matching potentially anything below that directory?


---

_Comment by @BurntSushi on 2018-09-15 16:47_

> how about not implementing the optimisation if there exists a ! whitelist rule matching potentially anything below that directory?

Yes, that's what I mean by "smarter." There's no infrastructure for that kind of analysis yet.

But it sounds like we should at least try to understand why `src/atty` is matching `src/*/*` and fix that.

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:39_

---

_Comment by @ssbarnea on 2024-12-16 14:46_

In fact the ignore-file implementation seems to be quite broken, I just discovered that an entry like `.pre-commit/` caused the ignore of all files in all folders named `.pre-commit-config.yaml`, which is clearly not what I was looking for.

---

_Comment by @BurntSushi on 2024-12-16 14:48_

> In fact the ignore-file implementation seems to be quite broken, I just discovered that an entry like `.pre-commit/` caused the ignore of all files in all folders named `.pre-commit-config.yaml`, which is clearly not what I was looking for.

That is entirely separate from this issue. Please open a new issue with an MRE.

---
