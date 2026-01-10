---
number: 19365
title: "[website]: Add structured metadata to rules, e.g. to indicate which version a rule was introduced in"
type: issue
state: closed
author: corneliusroemer
labels:
  - documentation
assignees: []
created_at: 2025-07-15T16:50:36Z
updated_at: 2025-10-27T16:12:00Z
url: https://github.com/astral-sh/ruff/issues/19365
synced_at: 2026-01-10T01:23:00Z
---

# [website]: Add structured metadata to rules, e.g. to indicate which version a rule was introduced in

---

_Issue opened by @corneliusroemer on 2025-07-15 16:50_

Turning this comment into a separate issue:

> On a similar note I have multiple times tried to figure out in which specific version of ruff that a rule was introduced or modified (bugfix, "upgraded" from nursery/preview to stable rule).
> 
> Having versioned documentation or some version information in each rule's docpage would make this much easier than today. 

 _Originally posted by @kaddkaka in [#9507](https://github.com/astral-sh/ruff/issues/9507#issuecomment-2586740259)_

I concur this would be nice. Rust's Clippy does this really well:

<img width="1261" height="522" alt="Image" src="https://github.com/user-attachments/assets/d78a0384-3391-4b8b-acf9-305d6c2c1fee" />

https://rust-lang.github.io/rust-clippy/stable/index.html

I would also appreciate (structured) metadata being shown in docs, such as version a rule was introduced in.

---

_Renamed from "Add structured metadata to rules, e.g. to indicate which version they were introduced in, made stable etc." to "Add structured metadata to rule docs, e.g. to indicate which version they were introduced in, made stable etc." by @corneliusroemer on 2025-07-15 16:50_

---

_Referenced in [astral-sh/ruff#9507](../../astral-sh/ruff/issues/9507.md) on 2025-07-15 16:51_

---

_Label `documentation` added by @ntBre on 2025-07-15 16:51_

---

_Renamed from "Add structured metadata to rule docs, e.g. to indicate which version they were introduced in, made stable etc." to "[website]: Add structured metadata to rules, e.g. to indicate which version a rule was introduced in" by @corneliusroemer on 2025-07-15 16:52_

---

_Comment by @kaddkaka on 2025-07-15 17:10_

Wezterm documentation is another great example which keeps each version specific documentation in the latest documentation, see for example https://wezterm.org/config/lua/config/window_decorations.html

---

_Comment by @corneliusroemer on 2025-07-15 17:34_

> Wezterm documentation is another great example which keeps each version specific documentation in the latest documentation, see for example https://wezterm.org/config/lua/config/window_decorations.html

Adding a screenshot to save (most) readers a link click:

<img width="1560" height="687" alt="Image" src="https://github.com/user-attachments/assets/cda9fe9d-3cb5-49ce-84ec-70ca833bd633" />

Note there are 2 types of version information here:
- when the rule was first introduced/added
- in addition, if new options became available after the initial introduction, they are inside a separate box

To my taste, this is a little hard to parse for first time visitors - though the repeat visitor might appreciate the granular information. Looks fairly tricky to maintain those versioned boxes though for changes.

---

_Comment by @ntBre on 2025-10-27 16:12_

Closing this after https://github.com/astral-sh/ruff/pull/21035! We now include the version when a rule was added, a link to related  issues, and a link to the source code, much like clippy. These are also included in the JSON output of `ruff rule`:

```shell
> uvx ruff@0.14.2 rule F401 --output-format json | jq '{status, source_location}'
{
  "status": {
    "Stable": {
      "since": "v0.0.18"
    }
  },
  "source_location": {
    "file": "crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs",
    "line": 145
  }
}
```

---

_Closed by @ntBre on 2025-10-27 16:12_

---
