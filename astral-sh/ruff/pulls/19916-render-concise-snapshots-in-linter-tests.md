```yaml
number: 19916
title: Render concise snapshots in linter tests
type: pull_request
state: closed
author: ntBre
labels:
  - testing
assignees: []
base: main
head: brent/concise-snapshots
created_at: 2025-08-14T13:58:34Z
updated_at: 2025-08-18T12:42:47Z
url: https://github.com/astral-sh/ruff/pull/19916
synced_at: 2026-01-10T17:52:17Z
```

# Render concise snapshots in linter tests

---

_Pull request opened by @ntBre on 2025-08-14 13:58_

Summary
--

This is a quick implementation of the idea in https://github.com/astral-sh/ruff/pull/19900#issuecomment-3188463902. Especially as we start using new diagnostic features, it seems like a good idea to keep an eye on the `concise` output format as well as the `full`. This PR just appends a second rendering of the diagnostics in our rule snapshots in the `concise` format. The concise message is typically reused in the other output formats too, so this kind of gives us a bit of coverage for all the output formats.

I noticed that some of our concise messages are still quite long. That might be another good class of diagnostic to use new features for. Some explanatory pieces might fit better as sub-diagnostics, now that we're not limited to a single line in the default case.

Adding these lines (and adding a `let` binding for `emitter`) is the only code change:

https://github.com/astral-sh/ruff/blob/1fbb88f6505a2f05b30267defed0e62ca88e196a/crates/ruff_linter/src/test.rs#L371-L383

Test Plan
--

Existing snapshots. Again, I clicked through these individually, but very, very quickly


---

_Label `testing` added by @ntBre on 2025-08-14 14:06_

---

_Comment by @github-actions[bot] on 2025-08-14 14:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-08-14 14:11_

---

_Review requested from @AlexWaygood by @ntBre on 2025-08-14 14:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-14 14:11_

---

_Comment by @MichaReiser on 2025-08-18 07:32_

Thanks for looking into this. 

IMO, this contains too much redundant information and will only increase the noise when making changes to our tests. That's why I'd prefer a more granular opt in to testing the concise output

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-18 07:53_

---

_Closed by @ntBre on 2025-08-18 12:42_

---

_Branch deleted on 2025-08-18 12:42_

---
