```yaml
number: 22504
title: "[ty] Update conformance workflow to use comparison script"
type: pull_request
state: merged
author: WillDuke
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: wld/update-conformance-workflow
created_at: 2026-01-11T17:55:59Z
updated_at: 2026-01-14T19:08:10Z
url: https://github.com/astral-sh/ruff/pull/22504
synced_at: 2026-01-14T19:42:39Z
```

# [ty] Update conformance workflow to use comparison script

---

_@WillDuke_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:



- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR updates the typing conformance workflow to use the `conformance.py` script added in #22231.

Partially addresses  [#2205](https://github.com/astral-sh/ty/issues/2205).

## Test Plan

Reviewing CI result once #22231 merges. 

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-11 18:07_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Label `ci` added by @AlexWaygood on 2026-01-12 08:25_

---

_Label `ty` added by @AlexWaygood on 2026-01-12 08:25_

---

_Comment by @MichaReiser on 2026-01-12 17:32_

@WillDuke The CI job is currently failing because it can't find the file. Would you mind taking a look or would you prefer one of us doing the CI integration?

---

_Comment by @WillDuke on 2026-01-12 18:46_

> @WillDuke The CI job is currently failing because it can't find the file. Would you mind taking a look or would you prefer one of us doing the CI integration?

CI is failing since `scripts/conformance.py` does not yet exist on main. Once #22231 merges, I can close and reopen this to check that the job starts working.

---

_Closed by @WillDuke on 2026-01-13 22:21_

---

_Reopened by @WillDuke on 2026-01-13 22:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 22:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff

## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 0 | 0 | +0 | ⏬ (❌) |
| False Positives | 965 | 965 | +0 | ⏬ (✅) |
| False Negatives | 1155 | 1155 | +0 | ⏬ (✅) |
| Total Diagnostics | 965 | 965 | 0 | ⏬ |
| Precision | 0.00% | 0.00% | +0.00% | ⏬ (❌) |
| Recall | 0.00% | 0.00% | +0.00% | ⏬ (❌) |


The percentage of diagnostics emitted that were expected errors held steady at 0.00%, and the percentage of expected errors that received a diagnostic held steady at 0.00%.


```

</details>




---

_Comment by @AlexWaygood on 2026-01-13 22:49_

Ah, we may need to update some logic over on the astral-sh-bot end to get it to post the raw markdown outputted by the new script, rather than putting it all in a collapsible section.

I think that repo is still closed-source, unfortunately, but I can take a look tomorrow.

---

_Comment by @MichaReiser on 2026-01-14 16:36_

@AlexWaygood I hope you don't mind but I went ahead and created https://github.com/astral-sh/astral-bot/pull/43

---

_Comment by @AlexWaygood on 2026-01-14 17:01_

> @AlexWaygood I hope you don't mind but I went ahead and created [astral-sh/astral-bot#43](https://github.com/astral-sh/astral-bot/pull/43)

I don't mind at all, thank you very much!!

---

_Comment by @MichaReiser on 2026-01-14 18:10_

The script is failing right now. Is it because the results are identical on this PR?

Edit: No, that's not it. It isn't finding the typing conformance tests

---

_Comment by @MichaReiser on 2026-01-14 18:18_

At the current pace, I'll be able to maybe merge this PR in 1h 😆 

---

_Merged by @MichaReiser on 2026-01-14 19:06_

---

_Closed by @MichaReiser on 2026-01-14 19:06_

---

_Comment by @MichaReiser on 2026-01-14 19:08_

Thank you, @WillDuke, for all your work on this. Having this is extremely valuable and will make our lives so much easier (and it will be such a great feeling seeing how we creep in on the conformance :))

---
