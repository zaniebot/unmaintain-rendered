```yaml
number: 2029
title: "Inverse of `--files` to show what files are skipped"
type: issue
state: closed
author: zachriggle
labels:
  - wontfix
assignees: []
created_at: 2021-10-21T01:55:47Z
updated_at: 2021-10-21T12:20:54Z
url: https://github.com/BurntSushi/ripgrep/issues/2029
synced_at: 2026-01-12T16:13:24Z
```

# Inverse of `--files` to show what files are skipped

---

_@zachriggle_

I feel like I might have already filed this, and hopefully there's a mechanism already built, but...

Using `--files` to see what is searched is great.  The output can be compared with e.g. `find . -type f` but that's quite slow.

Is there a `--ignored-files` or similar flag that shows which files / directories that RipGrep encountered but did NOT search?

If not, this would be nice to have.  One possible caveat is that RipGrep will not descend into ignored directories -- and sometimes it's nice to have a full listing of FILES that were ignored, not simply "we ignored everything under this directory".

---

_Comment by @BurntSushi on 2021-10-21 12:20_

> Is there a `--ignored-files` or similar flag that shows which files / directories that RipGrep encountered but did NOT search?

No.

>  One possible caveat is that RipGrep will not descend into ignored directories

This caveat is why adding this feature is difficult. I didn't build the filtering logic so that it could be inverted, because ripgrep only cares about what it _does_ search, not what it _doesn't_ search.

Now the `--debug` flag will show everything that ripgrep skips, but of course ripgrep does not descend into directories that it skips, so it isn't guaranteed to show every file that it ignored. It's only guaranteed to show one of its ancestors up to the starting path.

>  The output can be compared with e.g. `find . -type f` but that's quite slow.

I think this is an acceptable work-around for an uncommon task. "Quite slow" is probably only relevant in very large repos:

```
$ comm -23 <(find ./ -type f | sort) <(rg --files ./ | sort)
```

---

_Label `wontfix` added by @BurntSushi on 2021-10-21 12:20_

---

_Closed by @BurntSushi on 2021-10-21 12:20_

---
