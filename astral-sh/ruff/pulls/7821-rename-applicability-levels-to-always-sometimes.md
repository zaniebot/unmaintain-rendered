```yaml
number: 7821
title: Rename applicability levels to always, sometimes, and never
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/app-refactor-freq
created_at: 2023-10-04T19:33:45Z
updated_at: 2023-10-05T18:43:47Z
url: https://github.com/astral-sh/ruff/pull/7821
synced_at: 2026-01-12T15:55:24Z
```

# Rename applicability levels to always, sometimes, and never

---

_@zanieb_

Following much discussion for #4181 at https://github.com/astral-sh/ruff/pull/5119, https://github.com/astral-sh/ruff/discussions/5476, #7769, https://github.com/astral-sh/ruff/pull/7819, and in [Discord](https://discord.com/channels/1039017663004942429/1082324250112823306/1159144114231709746), this pull request changes `Applicability` from using `Automatic`, `Suggested`, and `Manual` to `Always`, `Sometimes`, and `Never`.

Also removes `Applicability::Unspecified` (replacing #7792).


---

_Renamed from "zanie/app refactor freq" to "Refactor applicability to use always / sometimes / never " by @zanieb on 2023-10-04 19:34_

---

_Renamed from "Refactor applicability to use always / sometimes / never " to "Rename applicability levels to always, sometimes, and never" by @zanieb on 2023-10-04 19:37_

---

_Comment by @zanieb on 2023-10-05 18:09_

I'm still not certain these are the best way to express the ideas here, but they are pretty easy to rename in the future and I'd like to move forward to unblock #7769 

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/diff.rs`:55 on 2023-10-05 18:09_

What do you think is the outcome here? Like what language?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:370 on 2023-10-05 18:10_

I still prefer `Fix::always` and `Fix::suggested` over `Fix::always_applies` and `Fix::sometimes_applies`. Isn't the `_applies` implied?


---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:370 on 2023-10-05 18:11_

We'd then have `Fix::always_edits` for the multi-edit case which is admittedly strange, but is that why we're including `_applies` here?

---

_@charliermarsh approved on 2023-10-05 18:11_

---

_Comment by @codspeed-hq[bot] on 2023-10-05 18:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/app-refactor-freq)

### Merging #7821 will **degrade performances by 2.49%**

<sub>Comparing <code>zanie/app-refactor-freq</code> (63d4761) with <code>main</code> (709abd5)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/app-refactor-freq` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/default-rules[pydantic/types.py]` | 38 ms | 39 ms | -2.49% |


---

_Comment by @github-actions[bot] on 2023-10-05 18:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-10-05 18:23_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:370 on 2023-10-05 18:23_

I'm leaning towards explicit since there was some confusion expressed about the previous methods. I feel like with a bare "always" it's unclear what it refers to and could be confused with the availability?

---

_@zanieb reviewed on 2023-10-05 18:25_

---

_Review comment by @zanieb on `crates/ruff_linter/src/message/diff.rs`:55 on 2023-10-05 18:25_

I'm not sure! Perhaps what I have now with some additional commentary such as the use of `--unsafe-fixes` to enable the fix.

---

_Merged by @zanieb on 2023-10-05 18:43_

---

_Closed by @zanieb on 2023-10-05 18:43_

---

_Branch deleted on 2023-10-05 18:43_

---
