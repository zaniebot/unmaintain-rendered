```yaml
number: 18793
title: "[`flake8-simplify`] Fix `SIM911` autofix creating a syntax error"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-SIM911
created_at: 2025-06-19T14:10:00Z
updated_at: 2025-06-23T14:25:59Z
url: https://github.com/astral-sh/ruff/pull/18793
synced_at: 2026-01-12T15:56:25Z
```

# [`flake8-simplify`] Fix `SIM911` autofix creating a syntax error

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
The fix would create a syntax error if there wasn't a space between the `in` keyword and the following expression.
For example:
```python
for country, stars in(zip)(flag_stars.keys(), flag_stars.values()):...
```

I also noticed that the tests for `SIM911` were note being run, so I fixed that.

Fixes #18776

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-19 14:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MeGaGiGaGon on 2025-06-19 17:05_

I think this wouldn't work in all cases, since it's explicitly checking for `in`, but you can have a lot of other syntax elements there, for example `or` [playground](https://play.ruff.rs/a96a5969-675c-4929-8591-800e74292640)

My guess is the correct thing to do would be to use `fix::edits::pad` like in #7699

---

_Comment by @LaBatata101 on 2025-06-19 19:32_

> I think this wouldn't work in all cases, since it's explicitly checking for `in`, but you can have a lot of other syntax elements there, for example `or` [playground](https://play.ruff.rs/a96a5969-675c-4929-8591-800e74292640)

For that case it fixes with no issue
```diff
--- sample2.py
+++ sample2.py
@@ -1,2 +1,2 @@
 flag_stars = {}
-for country, stars in 1or(zip)(flag_stars.keys(), flag_stars.values()):...
+for country, stars in 1or flag_stars.items():...
```

> My guess is the correct thing to do would be to use `fix::edits::pad` like in #7699

Cool, didn't know that function. I'll try to update the solution.

---

_Comment by @MeGaGiGaGon on 2025-06-19 19:38_

I think it worked there because there happened to be whitespace between the `in` and next expr that isn't the `zip` that's being removed. That should make these cases fail since they don't have that whitespace: [playground](https://play.ruff.rs/af5c023d-907c-4957-bd33-ba3705046032)

---

_Comment by @LaBatata101 on 2025-06-19 19:40_

> I think it worked there because there happened to be whitespace between the `in` and next expr that isn't the `zip` that's being removed. That should make these cases fail since they don't have that whitespace: [playground](https://play.ruff.rs/af5c023d-907c-4957-bd33-ba3705046032)

Should be fine now, I've updated to use `edits::pad`

---

_@MichaReiser reviewed on 2025-06-23 08:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:105 on 2025-06-23 08:53_

Nice, I didn't know this existed

---

_@MichaReiser approved on 2025-06-23 08:53_

Thank you

---

_Label `bug` added by @MichaReiser on 2025-06-23 08:53_

---

_Label `fixes` added by @MichaReiser on 2025-06-23 08:53_

---

_@MichaReiser reviewed on 2025-06-23 08:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM911_SIM911.py.snap`:76 on 2025-06-23 08:57_

The fix now seems to insert an unnecessary space here

---

_Review requested from @MichaReiser by @MichaReiser on 2025-06-23 08:57_

---

_@LaBatata101 reviewed on 2025-06-23 14:10_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM911_SIM911.py.snap`:76 on 2025-06-23 14:10_

Just needed to update the snapshot

---

_Merged by @MichaReiser on 2025-06-23 14:24_

---

_Closed by @MichaReiser on 2025-06-23 14:24_

---

_Branch deleted on 2025-06-23 14:25_

---
