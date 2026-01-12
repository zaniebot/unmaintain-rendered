```yaml
number: 21510
title: "Optimized version of `parenthesized_ranges`"
type: issue
state: closed
author: sharkdp
labels:
  - good first issue
  - performance
assignees: []
created_at: 2025-11-18T13:42:37Z
updated_at: 2025-12-03T08:15:19Z
url: https://github.com/astral-sh/ruff/issues/21510
synced_at: 2026-01-12T15:54:57Z
```

# Optimized version of `parenthesized_ranges`

---

_@sharkdp_

Rewrite (if possible) or provide alternatives to [`parenthesized_range`](https://github.com/astral-sh/ruff/blob/5ca9c15fc80070ff99f101af591a29cf1b1e11c2/crates/ruff_python_ast/src/parenthesize.rs#L60-L67) and [`parentheses_iterator`](https://github.com/astral-sh/ruff/blob/5ca9c15fc80070ff99f101af591a29cf1b1e11c2/crates/ruff_python_ast/src/parenthesize.rs#L14-L56) that take `&Tokens` as arguments instead of the `CommentRanges`, and don't use `SimpleTokenizer` or `BackwardsTokenizer` at all.

The ultimate goal of this would be to optimize calls sites such as [this one in ty](https://github.com/astral-sh/ruff/blob/5ca9c15fc80070ff99f101af591a29cf1b1e11c2/crates/ty_python_semantic/src/types/diagnostic.rs#L2146-L2150) (which uses `parentheses_iterator`).

Suggested by @MichaReiser here: https://github.com/astral-sh/ruff/pull/21476#discussion_r2537959967

---

_Label `good first issue` added by @sharkdp on 2025-11-18 13:42_

---

_Label `performance` added by @sharkdp on 2025-11-18 13:42_

---

_Comment by @denyszhak on 2025-11-22 11:54_

Hi @sharkdp 
May I work on this one?

---

_Assigned to @denyszhak by @MichaReiser on 2025-11-22 13:54_

---

_Comment by @denyszhak on 2025-12-01 16:28_

I'm putting a PR together today/tomorrow

---

_Comment by @FarhanAliRaza on 2025-12-01 17:13_


> I'm putting a PR together today/tomorrow

Sorry, I worked on a pr for this before you commented this. Should I open it? @denyszhak  .

Edit: I opened a pr. If you have work ready and want to do it, I can close it, no worries. I started it today just wanted to explore the code base. 


---

_Comment by @denyszhak on 2025-12-01 17:30_

@FarhanAliRaza This is weird, whatever. I've done work that I dedicated my time to and communicated earlier so I'm going to open a PR:) Would be glad i you can close yours, thanks

---

_Comment by @FarhanAliRaza on 2025-12-01 17:46_

closed

---

_Comment by @denyszhak on 2025-12-01 17:50_

> closed

Thank you!

---

_Closed by @MichaReiser on 2025-12-03 08:15_

---
