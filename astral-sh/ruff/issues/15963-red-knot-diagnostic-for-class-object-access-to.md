```yaml
number: 15963
title: "[red-knot] Diagnostic for class-object access to pure instance variables"
type: issue
state: closed
author: sharkdp
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-02-05T11:54:32Z
updated_at: 2025-02-24T14:17:44Z
url: https://github.com/astral-sh/ruff/issues/15963
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Diagnostic for class-object access to pure instance variables

---

_@sharkdp_

The goal of this ticket is to resolve those two `TODO`s and introduce a new diagnostic for accesses of pure instance variables through class objects:

https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L79-L96

part of: astral-sh/ruff#14164

---

_Renamed from "Emit a diagnostic when pure instance variables are accessed through class objects (in contrast to mypy/pyright)" to "[red-knot] Diagnostic for class-object access to pure instance variables" by @sharkdp on 2025-02-05 11:55_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 11:55_

---

_Label `help wanted` added by @sharkdp on 2025-02-05 11:58_

---

_Comment by @mishamsk on 2025-02-06 04:14_

@sharkdp I've picked this one. I spent a couple of hours diving into semantic index builder, use_def_map and the rest of the related code. Having fixed the issue yet, but I plan to. 

P.S. good issue, facilitated much deeper dive in into the code base than the other one I've fixed.

---

_Comment by @sharkdp on 2025-02-06 07:23_

> spent a couple of hours diving into semantic index builder, use_def_map and the rest of the related code

> good issue, facilitated much deeper dive in into the code base than the other one I've fixed

That sounds more like I did a bad job at picking out a beginner-friendly task, sorry. I didn't want to introduce you to everything all at once :upside_down_face:. Let us know if you need help. I *think* https://github.com/astral-sh/ty/issues/206 would only require changes to type inference (`types.rs`, `infer.rs`), but it's hard to tell without actually working on the task.

---

_Comment by @mishamsk on 2025-02-06 14:48_

@sharkdp no worries, I enjoy figuring things out and prefer hard(er) problems. Plus, I have general goal of contributing to red_knot in a major way, so I'll need to understand all of it one way or another. If this is urgent and you do not want to wait for the fix - then let me know, otherwise I'd like to tackle it before moving on to other issues.

---

_Closed by @sharkdp on 2025-02-24 14:17_

---
