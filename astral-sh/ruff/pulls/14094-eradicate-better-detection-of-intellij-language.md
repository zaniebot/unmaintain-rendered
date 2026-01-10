```yaml
number: 14094
title: "[`eradicate`] Better detection of IntelliJ language injection comments (`ERA001`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: ERA001
created_at: 2024-11-04T17:11:11Z
updated_at: 2024-11-07T20:18:35Z
url: https://github.com/astral-sh/ruff/pull/14094
synced_at: 2026-01-10T20:50:57Z
```

# [`eradicate`] Better detection of IntelliJ language injection comments (`ERA001`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-04 17:11_

## Summary

Enhances #14069.

## Test Plan

A few tests are added to cover edge cases.


---

_Review requested from @charliermarsh by @AlexWaygood on 2024-11-04 17:23_

---

_Comment by @InSyncWithFoo on 2024-11-04 17:23_

By the way, do we want that giant regex to be case-insensitive? [`type: ignore`](https://mypy-play.net/?mypy=latest&python=3.13&flags=strict&gist=793b6d57f6c02fc34c9eef1d62b70d5b), [`pyright`](https://pyright-play.net/?pyrightVersion=1.1.387&pythonVersion=3.14&strict=true&code=IYLgBAlgdgLmC8YDkSxgMRgAoE8BOEA5gBYzhFQD2eApgLABQQA) and [`SPDX-License-Identifier`](https://spdx.dev/learn/handling-license-info/) are a few non-examples.

---

_Comment by @github-actions[bot] on 2024-11-04 17:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-04 21:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/eradicate/detection.rs`:21 on 2024-11-04 21:06_

Did you change the regex in anyway or is it just added tests? Kind of hard to spot.

Regarding case-sensitive. The regex should be case-sensitive because `FMT: OFF` is not a valid suppression comment.

---

_@InSyncWithFoo reviewed on 2024-11-04 22:57_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/eradicate/detection.rs`:21 on 2024-11-04 22:57_

Consider it a rewrite.

The original rule from `eradicate` [ignores casing for all of these](https://github.com/PyCQA/eradicate/blob/5e9eb212c99afd8734b6f33d9d5b5107d10ed9cd/eradicate.py#L75). I sorted them into three sections for now. Should I proceed to check the "unknown" ones, or should that be part of a new PR?

---

_@MichaReiser requested changes on 2024-11-05 06:51_

Could you help me review this more efficiently by listing yoour changes so that I don't have to diff the Regex? 

For case-sensitivity: I think we should use case-sensitive for the suppression comments that are case-sensitive and allow case-insensitivity for the ones that are not. 

I also think we should gate this change behind preview mode unless all changes relax the rule's strictness.

---

_Comment by @InSyncWithFoo on 2024-11-05 15:43_

Here's a comparison table:

|                            | Before (all case insensitive) | After                                    |
|----------------------------|-------------------------------|------------------------------------------|
| `pylint`                   |                               | No change                                |
| `pyright`                  |                               | Case sensitive                           |
| `noqa`                     |                               | No change                                |
| `nosec`                    |                               | No change                                |
| `region`/`endregion`       |                               | Case sensitive                           |
| `type: ignore`             |                               | Case sensitive                           |
| `fmt`                      | `fmt:\s*(on\|off)`            | `fmt:\s*(on\|off\|skip)`, case sensitive |
| `isort`                    |                               | No change                                |
| `mypy`                     |                               | Case sensitive                           |
| `SPDX-License-Identifier:` |                               | Case sensitive                           |
| `encoding`                 |                               | No change                                |

As for `language=`:

* Before: `(?i)` `language=[a-zA-Z](?: ?[-_.a-zA-Z0-9]+)+(?:\s+prefix=\S+)?(?:\s+suffix=\S+)?`
* After: `language=[-_.a-zA-Z0-9]+`

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-11-05 15:43_

---

_Label `rule` added by @MichaReiser on 2024-11-05 20:48_

---

_@MichaReiser approved on 2024-11-06 11:43_

Thanks for the table. That made reviewing so much easier. 

This looks good to me. The main open question is if the change should be gated behind preview. I'm leaning towards no, considering that there are no  changes in the ecosystem check. @AlexWaygood are you okay with this?

---

_@AlexWaygood approved on 2024-11-06 18:23_

I'm fine with this. It feels like a bugfix to me and, as you say, the ecosystem report indicates that it shouldn't be that disruptive.

---

_Renamed from "Better detection of IntelliJ language injection comments" to "[`eradicate`] Better detection of IntelliJ language injection comments (`ERA001`)" by @AlexWaygood on 2024-11-06 18:24_

---

_Merged by @AlexWaygood on 2024-11-06 18:24_

---

_Closed by @AlexWaygood on 2024-11-06 18:24_

---

_Branch deleted on 2024-11-07 20:18_

---
