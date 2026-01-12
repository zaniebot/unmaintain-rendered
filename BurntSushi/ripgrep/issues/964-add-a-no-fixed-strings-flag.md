```yaml
number: 964
title: add a --no-fixed-strings flag
type: issue
state: closed
author: jsatk
labels:
  - enhancement
assignees: []
created_at: 2018-06-25T20:21:02Z
updated_at: 2018-06-25T21:28:53Z
url: https://github.com/BurntSushi/ripgrep/issues/964
synced_at: 2026-01-12T16:13:22Z
```

# add a --no-fixed-strings flag

---

_@jsatk_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.13.5

#### Describe your question, feature request, or bug.

I rarely ever search using regular expressions. Normally I'm wanting to find some usage of a function or string. I was hoping there was a way to set `rg --fixed-strings` as the default behavior. Perhaps with a `.rgrc` or an environment variable?

Here's what I had in mind.

```sh
$ cat ~/.rgrc 
{
  "defaultSearchBehavior": "fixed-strings"
}
$ rg ".i8n(" # Would not explode
```

---

_Comment by @okdana on 2018-06-25 20:23_

https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file

ETA: Might be useful to have a `--no-fixed-strings` or whatever so that people who put `-F` in their defaults can negate it tho

---

_Label `enhancement` added by @BurntSushi on 2018-06-25 20:29_

---

_Comment by @BurntSushi on 2018-06-25 20:29_

@okdana I agree that a `--no-fixed-strings` flag would be useful.

---

_Renamed from "Is there a way to make `--fixed-strings` the default?" to "add a --no-fixed-strings flag" by @BurntSushi on 2018-06-25 20:29_

---

_Closed by @BurntSushi on 2018-06-25 21:02_

---

_Comment by @jsatk on 2018-06-25 21:28_

Oops! I dug around and didn't see this info. Thank you!

---
