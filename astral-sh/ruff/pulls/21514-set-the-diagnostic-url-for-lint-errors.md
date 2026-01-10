```yaml
number: 21514
title: Set the diagnostic URL for lint errors
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - diagnostics
assignees: []
merged: true
base: main
head: brent/ruff-links
created_at: 2025-11-18T17:43:46Z
updated_at: 2025-11-18T18:34:52Z
url: https://github.com/astral-sh/ruff/pull/21514
synced_at: 2026-01-10T16:48:02Z
```

# Set the diagnostic URL for lint errors

---

_Pull request opened by @ntBre on 2025-11-18 17:43_

Summary
--

This PR wires up the `Diagnostic::set_documentation_url` method from #21502 to Ruff's lint diagnostics. This enables the links for the full and concise output formats without any other changes.

I considered also including the URLs for the grouped and pylint output formats, but the grouped format is still in `ruff_linter` instead of `ruff_db`, so we'd have to export some additional functionality to wire it up with `fmt_with_hyperlink`; and the pylint format doesn't currently render with color, so I think it might actually be machine readable rather than human readable?

The other ouput formats (json, json-lines, junit, github, gitlab, rdjson, azure, sarif) seem more clearly not to need the links.

Test Plan
--

I guess you can't see my cursor or the browser opening, but it works for lint rules, which have links, and doesn't include a link for syntax errors, which don't have valid links.

![out](https://github.com/user-attachments/assets/a520c7f9-6d7b-4e5f-a1a9-3c5e21a51d3c)


---

_@MichaReiser reviewed on 2025-11-18 17:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:128 on 2025-11-18 17:46_

Do we still need `Diagnostic::to_ruff_url` or can we replace all usages with `diagnostic.documentation_url()`?

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 17:54_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@ntBre reviewed on 2025-11-18 18:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:128 on 2025-11-18 18:03_

Oh right, yes we can remove it. I just had to set the URL in some of the rendering tests too.

---

_Review requested from @carljm by @ntBre on 2025-11-18 18:04_

---

_Review requested from @sharkdp by @ntBre on 2025-11-18 18:04_

---

_Review requested from @dcreager by @ntBre on 2025-11-18 18:04_

---

_Review request for @dcreager removed by @ntBre on 2025-11-18 18:05_

---

_Review request for @carljm removed by @ntBre on 2025-11-18 18:05_

---

_Review request for @sharkdp removed by @ntBre on 2025-11-18 18:05_

---

_Label `cli` added by @ntBre on 2025-11-18 18:05_

---

_Label `diagnostics` added by @ntBre on 2025-11-18 18:05_

---

_@MichaReiser approved on 2025-11-18 18:13_

Awesome

---

_Merged by @ntBre on 2025-11-18 18:34_

---

_Closed by @ntBre on 2025-11-18 18:34_

---

_Branch deleted on 2025-11-18 18:34_

---
