```yaml
number: 7390
title: "Add `explicit-preview-rules` to toggle explicit selection of preview rules"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zanie/preview-explicit
created_at: 2023-09-14T17:37:36Z
updated_at: 2023-09-28T20:00:35Z
url: https://github.com/astral-sh/ruff/pull/7390
synced_at: 2026-01-12T15:55:23Z
```

# Add `explicit-preview-rules` to toggle explicit selection of preview rules

---

_@zanieb_

Closes #7434 

Replaces the `PREVIEW` selector (removed in #7389) with a configuration option `explicit-preview-rules` which requires selectors to use exact rule codes for all preview rules. This allows users to enable preview without opting into all preview rules at once.

## Test plan

Unit tests

---

_Label `configuration` added by @zanieb on 2023-09-14 17:37_

---

_Label `preview` added by @zanieb on 2023-09-14 17:37_

---

_@zanieb reviewed on 2023-09-14 17:40_

---

_Review comment by @zanieb on `crates/ruff/src/rule_selector.rs`:242 on 2023-09-14 17:40_

To avoid passing these individually to `RuleSelector.rules`

---

_@zanieb reviewed on 2023-09-14 17:43_

---

_Review comment by @zanieb on `crates/ruff/src/rule_selector.rs`:204 on 2023-09-14 17:43_

I struggled a little with the borrow checker here

---

_Comment by @github-actions[bot] on 2023-09-14 18:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @MichaReiser on 2023-09-14 19:32_

Should we wait with adding the new option so that we can add it to the right settings section?

---

_Comment by @zanieb on 2023-09-14 20:42_

@MichaReiser maybe. I'm not in a rush to merge this, I'd be interested in seeing if anyone needs this functionality. However, I'm also not particularly concerned about adding a new option here since we're going to need to do a big rename for the `[lint]` section anyway.

---

_Comment by @codspeed-hq[bot] on 2023-09-26 17:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-explicit)

### Merging #7390 will **improve performances by 2.48%**

<sub>Comparing <code>zanie/preview-explicit</code> (f04b7b0) with <code>main</code> (70ab4b8)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/preview-explicit` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35 ms | 34.2 ms | +2.48% |


---

_Marked ready for review by @zanieb on 2023-09-26 20:26_

---

_@charliermarsh reviewed on 2023-09-26 20:38_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:556 on 2023-09-26 20:38_

I think the doc needs to be above the `#[option]` macro.

---

_@charliermarsh reviewed on 2023-09-26 20:39_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:555 on 2023-09-26 20:39_

Should `will be be` be `will _not_ be`?

---

_@charliermarsh reviewed on 2023-09-26 20:40_

If you set `explicit-preview-rules` without enabling `preview`, what happens? Do we respect `--select SOME_PREVIEW_RULE_123`? Or do you see warnings as before?

---

_@zanieb reviewed on 2023-09-26 20:41_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:555 on 2023-09-26 20:41_

Yep thanks!

---

_Comment by @zanieb on 2023-09-26 20:42_

We only respect `--select EXACTCODE` without the `--preview` flag for nursery rules as a temporary backwards compatible measure. This setting will not affect that behavior (you will see a warning that you're using deprecated nursery selection or that your selection doesn't include any rules).

---

_@charliermarsh approved on 2023-09-27 03:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_selector.rs`:243 on 2023-09-27 06:13_

Nit: Add a comment explaining what `require_explicit` means

---

_Review comment by @MichaReiser on `docs/preview.md`:59 on 2023-09-27 06:16_

Nit: Maybe `explicit-select-preview-rules` to clarify that it is about rule selection?

---

_@MichaReiser approved on 2023-09-27 06:16_

---

_@zanieb reviewed on 2023-09-27 14:18_

---

_Review comment by @zanieb on `docs/preview.md`:59 on 2023-09-27 14:18_

I considered that but I think it's a little bit too verbose and it could be confusing since it's not just the `--select` option it's _any_ option that takes a rule selector. Rule selectors aren't really a concept users have a strong understanding of.

---

_Merged by @zanieb on 2023-09-28 20:00_

---

_Closed by @zanieb on 2023-09-28 20:00_

---

_Branch deleted on 2023-09-28 20:00_

---
