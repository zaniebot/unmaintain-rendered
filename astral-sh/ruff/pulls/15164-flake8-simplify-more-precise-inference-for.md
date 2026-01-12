```yaml
number: 15164
title: "[`flake8-simplify`] More precise inference for dictionaries (`SIM300`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: SIM300
created_at: 2024-12-28T03:25:52Z
updated_at: 2024-12-30T10:42:26Z
url: https://github.com/astral-sh/ruff/pull/15164
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-simplify`] More precise inference for dictionaries (`SIM300`)

---

_@InSyncWithFoo_

## Summary

Resolves #14761.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-28 03:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2024-12-28 03:46_

As for the case of `C().A == print('B')`, I think it is reasonable to expect that a constant-like attribute access will have no side effects.

---

_@MichaReiser reviewed on 2024-12-28 09:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/yoda_conditions.rs`:115 on 2024-12-28 09:24_

Nit
```suggestion
                dict
                .items
                .iter()
                .map(|item| {
                    ConstantLikelihood::from(&item.value).min(
                        item.key
                            .as_ref()
                            .map(ConstantLikelihood::from)
                            .unwrap_or(ConstantLikelihood::Definitely),
                    )
                })
                .min()
                .unwrap_or(ConstantLikelihood::Definitely)
```

---

_@MichaReiser reviewed on 2024-12-28 09:27_

I'm probably missing something but don't we need to take the value into consideration as well? E.g. `{ "a": 1} == { "a": 2 }` returns `False`

---

_Label `rule` added by @MichaReiser on 2024-12-28 09:28_

---

_@InSyncWithFoo reviewed on 2024-12-28 10:54_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_simplify/rules/yoda_conditions.rs`:115 on 2024-12-28 10:54_

I'm not sure why this functional style should be preferred. It adds 4 more LOC and one or two more levels of indentation. Does it trigger compiler optimization or has some kind of obvious advantage over the traditional, imperative `for` loop that I'm simply missing?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/yoda_conditions.rs`:115 on 2024-12-30 10:34_

To me it's mainly that `Iterator::min` encodes the `accumulator` + for loop pattern. It allows me to skip over most code because I see: Okay, we compute the minimum from a sequence of items and it falls back to `Definetely` if the sequence is empty. This is different from the `acc` + `for` loop where my thought process was:

* `acc`: That probably means `accumulator`? What's it accumualting? 
* `acc` starts with `Definitely`. Is this the default? 
* We take the `min` of the `key` and the current `acc` value, okay
* and we then take the min of the `value` and the `acc` value
* Okay, I think this calculates the min over all value and keys

I reworked my code and it's now down to a total of 6 lines. It's also closer to how we handle lists, which I think is nice.

---

_@MichaReiser reviewed on 2024-12-30 10:34_

---

_Merged by @MichaReiser on 2024-12-30 10:41_

---

_Closed by @MichaReiser on 2024-12-30 10:41_

---

_Branch deleted on 2024-12-30 10:41_

---
