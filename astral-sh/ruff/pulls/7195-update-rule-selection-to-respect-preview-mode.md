```yaml
number: 7195
title: Update rule selection to respect preview mode
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zanie/rule-preview
created_at: 2023-09-06T15:16:34Z
updated_at: 2023-09-11T18:35:54Z
url: https://github.com/astral-sh/ruff/pull/7195
synced_at: 2026-01-12T15:55:23Z
```

# Update rule selection to respect preview mode

---

_@zanieb_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Extends work in #7046 (some relevant discussion there)

Changes:

- All nursery rules are now referred to as preview rules
- Documentation for the nursery is updated to describe preview
- Adds a "PREVIEW" selector for preview rules
    - This is primarily to allow `--preview --ignore PREVIEW --extend-select FOO001,BAR200`
- Using `--preview` enables preview rules that match selectors

Notable decisions:

- Preview rules are not selectable by their rule code without enabling preview
- Retains the "NURSERY" selector for backwards compatibility
- Nursery rules are selectable by their rule code for backwards compatiblity

Additional work:

- Selection of preview rules without the "--preview" flag should display a warning
- Use of deprecated nursery selection behavior should display a warning
- Nursery selection should be removed after some time

## Test Plan

<!-- How was it tested? -->

Manual confirmation (i.e. we don't have an preview rules yet just nursery rules so I added a preview rule for manual testing)

New unit tests


---

_Comment by @zanieb on 2023-09-06 15:22_

~Note nursery selection backwards compatibility is still a work in progress.~

---

_Comment by @codspeed-hq[bot] on 2023-09-06 15:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/rule-preview)

### Merging #7195 will **degrade performances by 4.4%**

<sub>Comparing <code>zanie/rule-preview</code> (6351494) with <code>main</code> (171b66c)</sub>



### Summary

`❌ 4` regressions
`✅ 21` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/rule-preview)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/rule-preview` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 70.5 ms | 72.8 ms | -3.16% |
| ❌ | `linter/all-rules[large/dataset.py]` | 158 ms | 164.8 ms | -4.15% |
| ❌ | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.57% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 33.4 ms | 35 ms | -4.4% |


---

_Comment by @github-actions[bot] on 2023-09-06 16:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-09-06 16:14_

---

_Review comment by @zanieb on `crates/ruff_benchmark/benches/linter.rs`:82 on 2023-09-06 16:14_

Presumably the benchmark regressions are due to including our nursery/preview rules now

---

_@zanieb reviewed on 2023-09-06 19:05_

---

_Review comment by @zanieb on `crates/ruff_benchmark/benches/linter.rs`:82 on 2023-09-06 19:05_

Yep https://github.com/astral-sh/ruff/pull/7195/commits/9522dfbc0119727b3c8ec8a7b0991159c6229c8c — I'll do this in a separate PR to avoid blocking this one

---

_@zanieb reviewed on 2023-09-06 19:09_

---

_Review comment by @zanieb on `crates/ruff_benchmark/benches/linter.rs`:82 on 2023-09-06 19:09_

#7208 

---

_Marked ready for review by @zanieb on 2023-09-06 21:43_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_docs.rs`:48 on 2023-09-07 14:13_

Is the "explicitly selecting" part still true? I thought this applied to the existing nursery rules for backwards compatibility, but not for preview rules in general?

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:59 on 2023-09-07 14:17_

Can we mark as `#[deprecated(...)]` to avoid accidental usages in the future?

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:108 on 2023-09-07 14:18_

I bet this could be encoded in the type system somehow, this feels awkward (I know that I wrote it lol). Not asking for a change here since it will likely involve more extensive changes in the macro, etc., just expressing that it feels strange.

---

_@charliermarsh approved on 2023-09-07 14:20_

---

_@charliermarsh reviewed on 2023-09-07 14:21_

---

_Review comment by @charliermarsh on `ruff.schema.json`:1898 on 2023-09-07 14:21_

There is some potential for confusion here that didn't exist in the nursery whereby if a user does` --select E11` without `--preview`, we won't throw a hard error. Instead, we'll select no rules and emit no diagnostics (IIUC), though I imagine in the future we'll want to show a warning.

---

_@charliermarsh reviewed on 2023-09-07 14:22_

Just confirming (even though it's mentioned in the test plan) that no rules are actually moved to `--preview` here -- it's only enabling the functionality.

---

_@zanieb reviewed on 2023-09-07 14:50_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_docs.rs`:48 on 2023-09-07 14:50_

👍 yep thanks will fix

---

_@zanieb reviewed on 2023-09-07 14:51_

---

_Review comment by @zanieb on `ruff.schema.json`:1898 on 2023-09-07 14:51_

Yep #7210 will show a warning

---

_Comment by @zanieb on 2023-09-07 14:51_

@charliermarsh well all of the nursery rules will be enabled with `--preview` but yes there are no rules that are preview-only without being in the nursery.

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:59 on 2023-09-07 15:13_

Same for `RuleSelector::Nursery`

---

_Review comment by @MichaReiser on `crates/ruff/src/rule_selector.rs`:95 on 2023-09-07 15:14_

Nit: Should this be a method on `RuleCodePrefix`?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/rule.rs`:22 on 2023-09-07 15:17_

Should we use `PreviewMode` here?

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:133 on 2023-09-07 15:19_

We could move all of this logic out of the macro if we instead implement `impl From<#linter> for RuleCodePrefix` because the `RuleCodePrefix::#linter(linter)` code is the only code that requires code-gen

---

_@MichaReiser reviewed on 2023-09-07 15:19_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/rule.rs`:22 on 2023-09-07 17:44_

The rule is in preview and is enabled or disabled by preview mode :D I want to think a bit more about "preview" vs "preview mode" in general so thanks for pointing this out.

---

_@zanieb reviewed on 2023-09-07 17:44_

---

_@zanieb reviewed on 2023-09-07 17:46_

---

_Review comment by @zanieb on `crates/ruff_macros/src/map_codes.rs`:133 on 2023-09-07 17:46_

Interesting. I'd like to separate further changes to the macro from this pull request though, if that's okay?

---

_@zanieb reviewed on 2023-09-07 17:46_

---

_Review comment by @zanieb on `crates/ruff/src/rule_selector.rs`:108 on 2023-09-07 17:46_

I will open a tracking issue once this is merged

---

_@zanieb reviewed on 2023-09-07 17:55_

---

_Review comment by @zanieb on `crates/ruff/src/rule_selector.rs`:95 on 2023-09-07 17:55_

Yeah maybe but it's in the macro 🤔 

---

_Comment by @zanieb on 2023-09-07 23:29_

I'd like to do a release tomorrow before merging this.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rule_selector.rs`:338 on 2023-09-08 03:37_

```suggestion
    /// The specificity when selecting via a rule prefix at three-character depth (e.g., `--select PLE120`).
    Prefix3Chars,
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rule_selector.rs`:340 on 2023-09-08 03:37_

```suggestion
    /// The specificity when selecting via a rule prefix at four-character depth (e.g., `--select PLE120`).
    Prefix4Chars,
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rule_selector.rs`:334 on 2023-09-08 03:44_

I got a bit confused with "rule prefix" which made me think of the _prefix_ part of the code i.e., "PLE" in the given example. But I think this is related to the number part, right? Maybe "Suffix1Char"?

---

_@dhruvmanila approved on 2023-09-08 03:53_

---

_@zanieb reviewed on 2023-09-08 15:26_

---

_Review comment by @zanieb on `crates/ruff/src/rule_selector.rs`:334 on 2023-09-08 15:26_

Hm well the specificity is that it's a prefix e.g. including "PLE10" but I agree the character count part is confusing since it just refers to the code length.

---

_@MichaReiser reviewed on 2023-09-08 15:33_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:133 on 2023-09-08 15:33_

Yeah absolutely. I should have written this more clearly. This is feedback based on last week's discussion on how it probably would be possible to move a lot out of the macros if we abstracted the dynamic parts by traits instead of where we generate the implementations for our types. But this isn't something that concerns this PR. I just noticed it as a good example because the dynamic part is limited to a single line.

---

_@MichaReiser reviewed on 2023-09-08 15:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/rule_selector.rs`:95 on 2023-09-08 15:34_

You can have multiple `impl` blocks even in different files. For as long as they are all in the same crate. Rust will merge them all together.

```
// macro generated
impl RuleCodePrefix {
}

// somewhere else 
impl RuleCodePrefix {
	fn is_single_rule_selector(&self) -> bool {
		...
	}
}
```

---

_Merged by @zanieb on 2023-09-11 17:28_

---

_Closed by @zanieb on 2023-09-11 17:28_

---

_Branch deleted on 2023-09-11 17:28_

---

_Label `preview` added by @zanieb on 2023-09-11 18:35_

---
