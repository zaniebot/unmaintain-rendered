```yaml
number: 530
title: Absolute path glob patterns on windows
type: issue
state: open
author: roblourens
labels:
  - bug
  - question
  - icebox
assignees: []
created_at: 2017-06-28T00:08:08Z
updated_at: 2018-08-22T18:04:22Z
url: https://github.com/BurntSushi/ripgrep/issues/530
synced_at: 2026-01-12T16:13:22Z
```

# Absolute path glob patterns on windows

---

_@roblourens_

If I do a search like this:

`C:\> rg -g "!C:/foo/bar/**" search "C:\foo\"`

Then the `-g` glob is not excluded. I have to write it like `-g "!/foo/bar"`. I think this is **expected** because globs starting with / should be matched as relative from the cwd. However, if I'm searching across drives,

`C:\> rg -g "!Z:/foo/bar/**" search "Z:\foo\"`

then the exclude glob is applied. I think this is correct too because `!/foo/bar` would match something in C:, not Z:.

But if that works, then is there a reason that `-g "!C:/foo/bar/**"` can't also be applied when `C:\` is the cwd, just for consistency?

My particular case is searching folders across multiple drives simultaneously, and processing glob patterns to be applied to those different folders, and not wanting to special-case C:.

---

_Label `question` added by @BurntSushi on 2017-07-03 11:28_

---

_Comment by @BurntSushi on 2017-07-03 11:29_

On the surface, this seems reasonable. I don't think I had ever taken drive letters into account when doing globs to be honest.

---

_Label `bug` added by @BurntSushi on 2017-07-03 11:29_

---

_Label `icebox` added by @BurntSushi on 2018-08-22 12:45_

---

_Comment by @BurntSushi on 2018-08-22 12:48_

So looking into this, I realized that my Windows laptop only has a `C:/` drive and I'm not sure how to get a `Z:/` to test with.

My other thoughts here:

* I have no idea why `C:/` is being treated differently from `Z:/`.
* In general, I'm not even sure I'd expect this to work at all, since the `-g` flag doesn't work with absolute globs. (Which I think you mentioned.)

---

_Comment by @roblourens on 2018-08-22 17:21_

Absolute globs do work if you are searching from `c:/`. That's what I do currently but I'm gradually changing to a different implementation that doesn't do that. This isn't a blocking issue on my end but if you want another drive letter to test with,

- Use `subst x: C:/some/folder` to mount some folder under a new drive letter
- Or plug in a usb drive

---

_Comment by @BurntSushi on 2018-08-22 18:04_

OK, so the issue here is that a glob like `C:/foo/bar` is treated no differently than a glob like `quux/foo/bar`. That is, the leading `C:` has no significance to ripgrep's glob matcher, so it matches it like normal against the target file name. *However*, in both searches, the root of ripgrep is `C:\\` and the root is always stripped off the file path before applying a glob match. In this case, `C:/` gets stripped from the file path, which will cause it to not match a glob that itself starts with `C:/`.

I'm not sure exactly what to do here. I'm finding it very difficult to reason about how ripgrep does globbing in corner cases like this (among several others). I'm not sure if that's incidental or necessary complexity, but this might not get addressed unless glob matching is re-worked. I do kind of want to re-work the `ignore` crate---which is where most of this stuff happens---because when a bug springs up from there, I find them super hard to fix.

---
