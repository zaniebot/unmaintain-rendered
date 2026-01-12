```yaml
number: 749
title: "In LSP returned markdown content could have `python` language identifier"
type: issue
state: closed
author: klonuo
labels:
  - server
assignees: []
created_at: 2025-07-01T22:28:37Z
updated_at: 2025-07-03T05:20:35Z
url: https://github.com/astral-sh/ty/issues/749
synced_at: 2026-01-12T15:54:23Z
```

# In LSP returned markdown content could have `python` language identifier

---

_@klonuo_

Hi,

I'm trying this tool with LSP in Sublime Text, and must say it's speed and footprint are just so impressive, excellent.

![Image](https://github.com/user-attachments/assets/01cfaba5-c37f-43fd-80ca-e9995cf79170)

Also wanted to suggest that returned markdown content while hovering on a symbol could have `python` as language identifier, instead `text` as it currently has, so that we can get syntax highlighted output for the definition.

Thanks

---

_Label `server` added by @carljm on 2025-07-02 00:42_

---

_Comment by @carljm on 2025-07-02 00:42_

I know we've discussed this before but I'm not finding an existing issue for it; thanks for the report!

---

_Comment by @dhruvmanila on 2025-07-02 03:03_

There's some context here: https://github.com/astral-sh/ruff/pull/17057#discussion_r2028151621 and it's mentioned as a sub-task in https://github.com/astral-sh/ty/issues/264.

In short, the reason it's not `python` today is because the content might not be valid Python. This content is generated from the `Display` implementation of `Type`. There was a suggestion previously that we could have two separate implementation of `Display` where one would be always try to produce a valid Python code while the other could be more specific to the current representation.

---

_Closed by @dhruvmanila on 2025-07-03 05:20_

---
