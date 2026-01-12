```yaml
number: 9460
title: "[`flake8-simplify`] Implement `SIM911`"
type: pull_request
state: merged
author: trag1c
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: simplify-SIM911
created_at: 2024-01-10T23:14:14Z
updated_at: 2024-01-11T19:47:12Z
url: https://github.com/astral-sh/ruff/pull/9460
synced_at: 2026-01-12T15:55:28Z
```

# [`flake8-simplify`] Implement `SIM911`

---

_@trag1c_

## Summary

Closes #9319, implements the [`SIM911` rule from `flake8-simplify`](https://github.com/MartinThoma/flake8-simplify/pull/183).


#### Note
I wasn't sure whether or not to include
```rs
if checker.settings.preview.is_disabled() {
    return;
}
```
at the beginning of the function with violation logic if the rule's already declared as part of `RuleGroup::Preview`.
I've seen both variants, so I'd appreciate some feedback on that :)

---

_Comment by @github-actions[bot] on 2024-01-10 23:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-01-11 03:12_

Awesome, excited to review!

---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-11 03:12_

---

_Label `rule` added by @charliermarsh on 2024-01-11 03:12_

---

_Label `preview` added by @charliermarsh on 2024-01-11 03:12_

---

_Comment by @charliermarsh on 2024-01-11 03:13_

```rust
if checker.settings.preview.is_disabled() {
    return;
}
```

Ah yeah -- this shouldn't be required for rules that are in `RuleGroup::Preview`, only for logic _within_ rules that varies based on whether preview is enabled. So omitting it here is correct in my view.

---

_Comment by @trag1c on 2024-01-11 10:04_

Thanks, corrected it. Double-checked my findings and it turns out that what I saw was actually a stable rule that had the preview check. I'm assuming that's still redundant?

---

_@charliermarsh reviewed on 2024-01-11 18:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:87 on 2024-01-11 18:45_

Nit: should this be `var2`, or is it intentionally `val2`?

---

_@charliermarsh reviewed on 2024-01-11 18:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:93 on 2024-01-11 18:45_

We know these are the same at this point, right? So could we just check one of them?

---

_@charliermarsh reviewed on 2024-01-11 18:46_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:873 on 2024-01-11 18:46_

Nit: for new rules, we're preferring to pass in `call`, rather than `expr`, since it ensures that we don't pass in expressions of the wrong type.

---

_@charliermarsh reviewed on 2024-01-11 18:46_

Looks great! Only minor feedback.

---

_Comment by @charliermarsh on 2024-01-11 18:47_

> Thanks, corrected it. Double-checked my findings and it turns out that what I saw was actually a stable rule that had the preview check. I'm assuming that's still redundant?

Yeah, which rule is this?


---

_Comment by @trag1c on 2024-01-11 18:55_

> > Thanks, corrected it. Double-checked my findings and it turns out that what I saw was actually a stable rule that had the preview check. I'm assuming that's still redundant?
> 
> Yeah, which rule is this?

I checked to find the rule code only to realize it's just ONE OF the functions that has the preview check in which case it's probably fine 🤦‍♂️ forgive my silliness

---

_@trag1c reviewed on 2024-01-11 18:55_

---

_Review comment by @trag1c on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:87 on 2024-01-11 18:55_

It should be `var2`, thanks :)

---

_@trag1c reviewed on 2024-01-11 18:56_

---

_Review comment by @trag1c on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:93 on 2024-01-11 18:56_

Yep, good catch. Looks like a remnant from my prior code 😄

---

_Comment by @charliermarsh on 2024-01-11 19:00_

@trag1c - I'm happy to include this in today's release. Do you wanna do the follow-up feedback otherwise I can do it quickly (no bother either way)?

---

_Comment by @trag1c on 2024-01-11 19:05_

Sure, let's get that released today! 😄

---

_Comment by @codspeed-hq[bot] on 2024-01-11 19:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/trag1c:simplify-SIM911)

### Merging #9460 will **degrade performances by 4.35%**

<sub>Comparing <code>trag1c:simplify-SIM911</code> (ea5a54a) with <code>main</code> (f192c72)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/trag1c:simplify-SIM911)._

### Benchmarks breakdown

|     | Benchmark | `main` | `trag1c:simplify-SIM911` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @charliermarsh on 2024-01-11 19:32_

@trag1c - think it just needs to be updated against `main`, just pushed.

---

_Comment by @trag1c on 2024-01-11 19:36_

My brain ignored "just pushed" and decided to update it myself either way 🤦‍♂️
Sorry for messing up again


---

_Merged by @charliermarsh on 2024-01-11 19:42_

---

_Closed by @charliermarsh on 2024-01-11 19:42_

---

_Comment by @charliermarsh on 2024-01-11 19:42_

(This doesn't touch the parser at all so it must be noise.)

---
