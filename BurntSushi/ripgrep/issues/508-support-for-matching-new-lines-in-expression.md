```yaml
number: 508
title: Support for matching new lines in expression?
type: issue
state: closed
author: ideasman42
labels: []
assignees: []
created_at: 2017-06-07T05:35:58Z
updated_at: 2017-06-07T14:09:36Z
url: https://github.com/BurntSushi/ripgrep/issues/508
synced_at: 2026-01-12T16:13:22Z
```

# Support for matching new lines in expression?

---

_@ideasman42_

I was looking to replace `pcregrep` with `ripgrep`, but found it doesn't have support for newline.

Example search for:
```
    }
    else some_text;
```
Command:
```
find . -name '*.c' | xargs pcregrep -n -M '\s}\n\s+else.*;\s*$'
```
Does ripgrep plan to support newline literals?

---

_Renamed from "Support for new lines in expression" to "Support for matching new lines in expression?" by @ideasman42 on 2017-06-07 05:43_

---

_Comment by @BurntSushi on 2017-06-07 11:23_

How is this different than multiline support? See: #176 

---

_Comment by @ideasman42 on 2017-06-07 14:09_

Sorry for duplicate, searched existing issues for `newline` `new line`.

---

_Closed by @ideasman42 on 2017-06-07 14:09_

---
