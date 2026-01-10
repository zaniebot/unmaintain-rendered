```yaml
number: 17359
title: "[ty] Show related information in diagnostic"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/diagnostic-related-information
created_at: 2025-04-11T17:04:36Z
updated_at: 2025-05-19T16:52:14Z
url: https://github.com/astral-sh/ruff/pull/17359
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Show related information in diagnostic

---

_Pull request opened by @MichaReiser on 2025-04-11 17:04_

## Summary


Uses the `relatedInformation` field in diagnostics to render additional data.

For now, this only renders subdiagnostics and annotations with location information. I played with showing other annotations as well, but the rendering in VS code looked awful (I mainly played with the overload diagnostic). I think this is an improvement over the status quo and we can iterate on it as we go.

Closes https://github.com/astral-sh/ty/issues/170

## Test Plan

https://github.com/user-attachments/assets/475e2826-44cd-4c2e-ad0a-c8ce3ba98408



---

_Label `red-knot` added by @MichaReiser on 2025-04-11 17:04_

---

_Comment by @github-actions[bot] on 2025-04-11 17:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-05-17 18:37_

---

_Review requested from @carljm by @MichaReiser on 2025-05-17 18:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-17 18:37_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-17 18:37_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-17 18:37_

---

_Label `server` added by @MichaReiser on 2025-05-17 18:38_

---

_Renamed from "[red-knot] Show related information in diagnostic" to "[ty] Show related information in diagnostic" by @AlexWaygood on 2025-05-17 18:38_

---

_Comment by @carljm on 2025-05-17 22:17_

This looks great to me, but I'll probably leave review to @BurntSushi 

---

_Review request for @carljm removed by @carljm on 2025-05-17 22:18_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-17 22:18_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-05-17 22:18_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-05-17 22:19_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/diagnostic/mod.rs`:431 on 2025-05-18 14:46_

This looks the same as `Diagnostic::concise_message`. I think @BurntSushi might have better recommendation but we could use a trait to avoid the duplication.

---

_@dhruvmanila approved on 2025-05-18 14:49_

> For now, this only renders subdiagnostics and annotations with location information. I played with showing other annotations as well, but the rendering in VS code looked awful (I mainly played with the overload diagnostic).

I think specifically for overloads this seems like a reasonable amount of information to display using related information because a user can hover over the function variable to look at the possible overloads. For example, here the first group is the ty diagnostics while the last group is the hover content:

<img width="937" alt="Screenshot 2025-05-18 at 10 33 49" src="https://github.com/user-attachments/assets/16f7b2e5-8264-47f7-9adf-5bb5f614ee6b" />

How this plays out with other diagnostics, that's something that we can play around in follow-ups but this is a good start as you've mentioned.

---

I know Neovim doesn't support related information but I'm not sure about editors other than VS Code like Zed.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:431 on 2025-05-19 16:38_

I would actually prefer the duplication over a trait. :-)

If the duplication gets obnoxious, another path we could follow is split the common parts of `Diagnostic` and `SubDiagnostic` out into a new internal helper type, and implement what we need there. But I don't think that's quite worth doing yet.

My bias is that I tend to be pretty trait-averse.

---

_@BurntSushi approved on 2025-05-19 16:39_

This looks like a great start to me!

---

_Merged by @MichaReiser on 2025-05-19 16:52_

---

_Closed by @MichaReiser on 2025-05-19 16:52_

---

_Branch deleted on 2025-05-19 16:52_

---
