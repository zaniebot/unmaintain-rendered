```yaml
number: 22408
title: "[ty] improve indented codefence rendering in docstrings"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/mdblock2
created_at: 2026-01-05T19:07:34Z
updated_at: 2026-01-06T15:44:34Z
url: https://github.com/astral-sh/ruff/pull/22408
synced_at: 2026-01-12T15:57:49Z
```

# [ty] improve indented codefence rendering in docstrings

---

_@Gankra_

By stripping leading indents from codefence lines to ensure they're properly understood by markdown (but otherwise preserving the indent in the codeblock so all the code renders roughly at the right indent).

As described in [this comment](https://github.com/astral-sh/ty/issues/2352#issuecomment-3711686053) this solution is very "do what I mean" for when a user has an explicit markdown codeblock in e.g. a `Returns:` section which "has" to be indented but that indent makes the verbatim codefence invalid markdown.

* Fixes https://github.com/astral-sh/ty/issues/2352

---

_Review requested from @carljm by @Gankra on 2026-01-05 19:07_

---

_Review requested from @MichaReiser by @Gankra on 2026-01-05 19:07_

---

_Review requested from @AlexWaygood by @Gankra on 2026-01-05 19:07_

---

_Review requested from @sharkdp by @Gankra on 2026-01-05 19:07_

---

_Review requested from @dcreager by @Gankra on 2026-01-05 19:07_

---

_Label `server` added by @Gankra on 2026-01-05 19:07_

---

_Label `ty` added by @Gankra on 2026-01-05 19:07_

---

_Comment by @Gankra on 2026-01-05 19:08_

We are now entering the "Aria creates a non-standard dialect of markdown" era of Astral.

---

_Comment by @Gankra on 2026-01-05 19:10_

I would be perfectly fine with the conclusion being "the user isn't allowed to do that" but idk it seems sensible..?

---

_@MichaReiser reviewed on 2026-01-06 09:08_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:1294 on 2026-01-06 09:08_

Can you share a screenshot for how e.g. VS Code renders this markdown?

---

_@Gankra reviewed on 2026-01-06 15:42_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:1294 on 2026-01-06 15:42_

<img width="642" height="426" alt="Screenshot 2026-01-06 at 10 42 11 AM" src="https://github.com/user-attachments/assets/26c58adf-19f9-440d-a42d-abd085e920aa" />


---

_@MichaReiser approved on 2026-01-06 15:43_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-06 15:44_

---

_Merged by @Gankra on 2026-01-06 15:44_

---

_Closed by @Gankra on 2026-01-06 15:44_

---

_Branch deleted on 2026-01-06 15:44_

---
