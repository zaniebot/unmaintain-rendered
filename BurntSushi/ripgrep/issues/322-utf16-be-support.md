```yaml
number: 322
title: UTF16 BE support
type: issue
state: closed
author: martinlindhe
labels: []
assignees: []
created_at: 2017-01-15T18:10:55Z
updated_at: 2017-01-15T19:05:05Z
url: https://github.com/BurntSushi/ripgrep/issues/322
synced_at: 2026-01-12T16:13:21Z
```

# UTF16 BE support

---

_@martinlindhe_

Hello. Am currently on Windows, and there for some reason (...) unicode files are often created as UTF16BE.
I realized I have a project with mixed file encodings, most is UTF8 and some are UTF16BE, and ripgrep currently don't find anything in those files.

In the attached archive is a project I was working on, and the file src\GUIGroup.ts happens to be in UTF16BE,
so try searching the folder for "GUIGroup" causes no hits in that file.

Tested with ripgrep 0.4.0 on Windows. Same result with ripgrep 0.4.0 on macOS

Github dont want to attach zip files today, so I put it here: 
http://wikisend.com/download/290136/utf16issue_ripgrep.zip



---

_Comment by @BurntSushi on 2017-01-15 18:20_

This is a dupe of #1.

As the README says, ripgrep doesn't seamlessly support any encoding that isn't ASCII compatible. (That includes UTF-8 and latin1, but not UTF-16.)

This is a desirable feature, especially at least UTF-16 since it is used a lot on Windows.

---

_Closed by @BurntSushi on 2017-01-15 18:20_

---

_Comment by @martinlindhe on 2017-01-15 19:05_

Ah, thanks. I glanced through the open issues looking for "UTF16", thats why I didn't notice #1.

---
