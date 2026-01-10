```yaml
number: 18220
title: "[ty] disable division-by-zero by default"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/disable-div-zero
created_at: 2025-05-20T14:52:15Z
updated_at: 2025-05-27T13:07:09Z
url: https://github.com/astral-sh/ruff/pull/18220
synced_at: 2026-01-10T18:51:02Z
```

# [ty] disable division-by-zero by default

---

_Pull request opened by @carljm on 2025-05-20 14:52_

## Summary

I think `division-by-zero` is a low-value diagnostic in general; most real division-by-zero errors (especially those that are less obvious to the human eye) will occur on values typed as `int`, in which case we don't issue the diagnostic anyway. Mypy and pyright do not emit this diagnostic.

Currently the diagnostic is prone to false positives because a) we do not silence it in unreachable code, and b) we do not implement narrowing of literals from inequality checks. We will probably fix (a) regardless, but (b) is low priority apart from division-by-zero.

I think we have many more important things to do and should not allow false positives on a low-value diagnostic to be a distraction. Not opposed to re-enabling this diagnostic in future when we can prioritize reducing its false positives.

References https://github.com/astral-sh/ty/issues/443

## Test Plan

Existing tests.


---

_Review requested from @AlexWaygood by @carljm on 2025-05-20 14:52_

---

_Review requested from @sharkdp by @carljm on 2025-05-20 14:52_

---

_Review requested from @dcreager by @carljm on 2025-05-20 14:52_

---

_Comment by @github-actions[bot] on 2025-05-20 14:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-20 14:59_

We should re-enable it for mypy primer here https://github.com/astral-sh/ruff/blob/main/.github/mypy-primer-ty.toml

I'm also not sure if we should disable it. I found the feedback around narrowing limitations useful and we'll miss out on that when it's disabled.

---

_Comment by @carljm on 2025-05-20 15:09_

> I'm also not sure if we should disable it. I found the feedback around narrowing limitations useful and we'll miss out on that when it's disabled.

I feel strongly that we need to improve our focus on the most important issues and avoid (ourselves and our users) being distracted by low-priority issues. If we keep it enabled in the ecosystem, we don't lose this feedback when we choose to make it a priority.

---

_Review requested from @MichaReiser by @carljm on 2025-05-20 15:12_

---

_Comment by @MichaReiser on 2025-05-20 15:13_

You also need to update the CLI tests. It's used a lot in there because it was one of the first rules that triggered (and it doesn't requier a lot of set up code)

---

_Renamed from "disable division-by-zero by default" to "[ty] disable division-by-zero by default" by @AlexWaygood on 2025-05-20 15:36_

---

_Label `ty` added by @AlexWaygood on 2025-05-20 15:36_

---

_Comment by @carljm on 2025-05-20 15:39_

> You also need to update the CLI tests. It's used a lot in there because it was one of the first rules that triggered (and it doesn't requier a lot of set up code)

Thanks. I was able to update those tests without having to change the code examples -- the same properties of the configuration can be demonstrated by going from "ignored" to "warn" as were previously demonstrated by going from "error" to "warn".

---

_@MichaReiser approved on 2025-05-20 16:32_

---

_Merged by @carljm on 2025-05-20 18:47_

---

_Closed by @carljm on 2025-05-20 18:47_

---

_Branch deleted on 2025-05-20 18:47_

---

_Comment by @AlexWaygood on 2025-05-27 10:51_

> b) we do not implement narrowing of literals from inequality checks. We will probably fix (a) regardless, but (b) is low priority apart from division-by-zero.

Small correction: we _do_ narrow literals from (in)equality checks, but we don't yet narrow literals from `<`, `<=`, `>` or `>=` checks: https://play.ty.dev/746ed0da-8731-41f4-b77d-c4cfb8eb7eb2

---

_Comment by @carljm on 2025-05-27 13:07_

Yes, that's what I meant. I guess there isn't a simple term for just  "greater or less-than checks"!

---
