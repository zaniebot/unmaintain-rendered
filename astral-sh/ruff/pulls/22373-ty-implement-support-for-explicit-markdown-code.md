```yaml
number: 22373
title: "[ty] Implement support for explicit markdown code fences in docstring rendering"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/markdown-fail
created_at: 2026-01-04T19:07:08Z
updated_at: 2026-01-05T12:13:27Z
url: https://github.com/astral-sh/ruff/pull/22373
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Implement support for explicit markdown code fences in docstring rendering

---

_Pull request opened by @Gankra on 2026-01-04 19:07_

* Fixes https://github.com/astral-sh/ty/issues/2291

---

_Label `server` added by @Gankra on 2026-01-04 19:07_

---

_Review requested from @carljm by @Gankra on 2026-01-04 19:07_

---

_Label `ty` added by @Gankra on 2026-01-04 19:07_

---

_Review requested from @MichaReiser by @Gankra on 2026-01-04 19:07_

---

_Review requested from @AlexWaygood by @Gankra on 2026-01-04 19:07_

---

_Review requested from @sharkdp by @Gankra on 2026-01-04 19:07_

---

_Review requested from @dcreager by @Gankra on 2026-01-04 19:07_

---

_Label `internal` added by @Gankra on 2026-01-04 19:09_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/docstring.rs`:1131 on 2026-01-04 19:14_

```suggestion
    // TODO: We should not parse the contents of a markdown codefence
```

---

_@AlexWaygood had review dismissed on 2026-01-04 19:14_

---

_Renamed from "[ty] checkin snapshot test for bad result on markdown fences in doctests" to "[ty] Add support for explicit markdown code fences in docstrings" by @Gankra on 2026-01-04 19:59_

---

_Label `internal` removed by @Gankra on 2026-01-04 19:59_

---

_Comment by @Gankra on 2026-01-04 19:59_

Upgraded this into implementing a solution.

---

_Renamed from "[ty] Add support for explicit markdown code fences in docstrings" to "[ty] Implement support for explicit markdown code fences in docstring rendering" by @Gankra on 2026-01-04 20:01_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:259 on 2026-01-05 08:26_

I think we should also add support for `~~~` ([spec](https://spec.commonmark.org/0.31.2/#example-120))

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:259 on 2026-01-05 08:28_

Code blocks can also be indented (but that's maybe something for another PR) [spec](https://spec.commonmark.org/0.31.2/#example-131)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:265 on 2026-01-05 08:29_

Oh, I didn't know that you can use multiple backticks for inline code

---

_@MichaReiser reviewed on 2026-01-05 08:31_

I think we should add support for `~~~` too. While less common, it is part of the markdown standard.

---

_@Gankra reviewed on 2026-01-05 11:24_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:259 on 2026-01-05 11:24_

I expect that one will remain out of scope because python people love random indents in their docstrings and it's a struggle to disambiguate "are we markdown or just some text".

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:265 on 2026-01-05 11:27_

Yeah in both reST and markdown! In python docstrings there's a soft convention of \`x\` and \`\`x\`\` being used for different purposes (similar to \`x\` and [\`x\`] in rustdoc).

---

_@Gankra reviewed on 2026-01-05 11:27_

---

_Comment by @Gankra on 2026-01-05 11:46_

tilde fences added

---

_@MichaReiser approved on 2026-01-05 12:11_

---

_Merged by @Gankra on 2026-01-05 12:13_

---

_Closed by @Gankra on 2026-01-05 12:13_

---

_Branch deleted on 2026-01-05 12:13_

---
