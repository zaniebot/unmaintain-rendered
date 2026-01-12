```yaml
number: 252
title: "Feature request: Union types"
type: issue
state: closed
author: SimenB
labels:
  - duplicate
assignees: []
created_at: 2016-11-25T14:15:29Z
updated_at: 2016-11-25T14:50:34Z
url: https://github.com/BurntSushi/ripgrep/issues/252
synced_at: 2026-01-12T18:23:11Z
```

# Feature request: Union types

---

_@SimenB_

I'd like to have a type called e.g. `styling` which searches css, sass, less and stylus, as well as a type for combined `styling`, `js` and `json` called e.g. `frontend`.

I could define those as their own types, but if I could compose other types together it would mean that if I add a new css dialect to `styling`, my `frontend` type would automatically be updated to use it.

---

_Comment by @SimenB on 2016-11-25 14:18_

Hmm, seems to be a dupe of #83, which had some traffic less than a week ago. Promising!

---

_Closed by @SimenB on 2016-11-25 14:18_

---

_Label `duplicate` added by @BurntSushi on 2016-11-25 14:50_

---
