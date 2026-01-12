```yaml
number: 394
title: Add svg file type
type: issue
state: closed
author: porglezomp
labels:
  - help wanted
  - question
assignees: []
created_at: 2017-03-06T18:57:14Z
updated_at: 2017-03-13T00:15:54Z
url: https://github.com/BurntSushi/ripgrep/issues/394
synced_at: 2026-01-12T16:13:21Z
```

# Add svg file type

---

_@porglezomp_

I'm searching through a web codebase and keep running into hundreds/thousands of lines of results from SVG

---

_Comment by @BurntSushi on 2017-03-06 21:31_

We could certainly just add a file type (in the meantime, `-g '!*.svg'` should do). A PR for that would be welcome.

Is there anything else we should be doing? svgs are plain text XML files, but it feels weird to try and search them.

---

_Label `help wanted` added by @BurntSushi on 2017-03-06 21:31_

---

_Label `question` added by @BurntSushi on 2017-03-06 21:31_

---

_Comment by @porglezomp on 2017-03-06 21:40_

I don't know, I hadn't seen the glob exclude but had seen the filetype exclude so I jumped straight to that as my solution. I don't really have any more specific suggestions because I don't work with SVGs enough.

---

_Comment by @jdhorwitz on 2017-03-10 22:50_

@BurntSushi I would love to work on this, just a point in the right direction would be great.  Thanks!

---

_Comment by @BurntSushi on 2017-03-10 23:01_

@jdhorwitz The simplest thing to do is to add an `svg` file type to ripgrep: https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs --- That way, one can ignore them by just using `-Tsvg`.

As to whether svgs should be ignored automatically or not kind of requires a UX exploration. I'm content to punt on that for now until more folks are actually having a problem with it.

---

_Closed by @BurntSushi on 2017-03-12 23:52_

---

_Comment by @porglezomp on 2017-03-13 00:15_

Thanks @jdhorwitz!

---
