```yaml
number: 1326
title: "Is it possible to get ripgrep to output filename and directory via a \"file://\" style hyperlink so that more terminal emulators could open it directly like iterm2 does it?"
type: issue
state: closed
author: adrianocr
labels:
  - duplicate
assignees: []
created_at: 2019-07-22T20:53:43Z
updated_at: 2019-07-22T21:25:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1326
synced_at: 2026-01-12T16:13:23Z
```

# Is it possible to get ripgrep to output filename and directory via a "file://" style hyperlink so that more terminal emulators could open it directly like iterm2 does it?

---

_@adrianocr_

Not following the issue template because this isn't an issue. 

I switched to Linux from macOS. On macOS I used iterm2 which automatically handled file links, Ripgrep would list a file out such as `src/subdirectory/subdirectory/filename.json` and if you alt+clicked that directory+filename listing, it'd open your editor. Unfortunately from what I can see none of the Linux terminal apps do this. However, if links were outputted as such `file://home/usr/projectname/src/subdirectory/subdirectory/filename.json`, it'd work just fine. 

This is a good post on links in terminals: https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda.

Is this something that can be configured or would it need to be added to the rg source and merged in? 

---

_Comment by @okdana on 2019-07-22 21:16_

#665

I thought there was a more recent one too but i can't find it

---

_Comment by @BurntSushi on 2019-07-22 21:21_

I don't think anything has changed since #665.

---

_Closed by @BurntSushi on 2019-07-22 21:21_

---

_Label `duplicate` added by @BurntSushi on 2019-07-22 21:22_

---

_Comment by @adrianocr on 2019-07-22 21:25_

@BurntSushi as in there's no interest in supporting it? 

With all due respect (I'm only saying that because there's no "tone" attached to what I'm about to say despite it kind of seeming like there is) but what's the issue with not many other tools doing it? Why would rg need other tools to do it to consider it a useful feature? 

Plus (and correct me if I'm wrong here) but I don't even believe that this would be more than a 1-2 line change in the source. Its just tweaking how the filename representation is output. 

---
