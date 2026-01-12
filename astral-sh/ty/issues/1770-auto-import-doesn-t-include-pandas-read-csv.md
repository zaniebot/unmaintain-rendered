```yaml
number: 1770
title: "Auto-import doesn't include `pandas.read_csv`"
type: issue
state: closed
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T13:01:42Z
updated_at: 2025-12-11T18:04:26Z
url: https://github.com/astral-sh/ty/issues/1770
synced_at: 2026-01-12T15:54:25Z
```

# Auto-import doesn't include `pandas.read_csv`

---

_@BurntSushi_

Demo:

https://github.com/user-attachments/assets/eb0ea95f-41b6-42cc-8c1d-bcdbf47ab190

I haven't investigated why yet.

---

_Added to milestone `Stable` by @BurntSushi on 2025-12-05 13:01_

---

_Label `server` added by @BurntSushi on 2025-12-05 13:01_

---

_Label `completions` added by @BurntSushi on 2025-12-05 13:01_

---

_Comment by @AlexWaygood on 2025-12-05 18:42_

is this with or without `pandas-stubs` installed?

---

_Comment by @AlexWaygood on 2025-12-05 18:47_

Hmm, but I would expect it to be included in the import suggestions with or without `pandas-stubs` being installed, yeah. I can't see anything obvious in the `pandas` or `pandas-stubs` package layout that we know isn't supported by the `SymbolFinder`

---

_Comment by @BurntSushi on 2025-12-11 18:04_

So I'm not sure what happened, but `read_csv` shows up for me in auto-import now:

https://github.com/user-attachments/assets/eb3de31c-a5a9-4604-9a32-9e5701a534c3

It's also the first result, which is nice.

pylance also handles this correctly, but it _only_ shows the top-level result:

https://github.com/user-attachments/assets/356b3183-546c-4e88-9f03-178ac1cc1078

I created https://github.com/astral-sh/ty/issues/1857 to track removing the duplicate suggestions for cases like this. But I guess this issue can be closed.

---

_Closed by @BurntSushi on 2025-12-11 18:04_

---
