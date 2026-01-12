```yaml
number: 13212
title: Handle singular case for incompatible rules warning
type: pull_request
state: merged
author: RubenVanEldik
labels:
  - cli
assignees: []
merged: true
base: main
head: dont-use-plural-when-single-rule-warning
created_at: 2024-09-02T11:41:20Z
updated_at: 2024-09-02T13:16:07Z
url: https://github.com/astral-sh/ruff/pull/13212
synced_at: 2026-01-12T15:55:43Z
```

# Handle singular case for incompatible rules warning

---

_@RubenVanEldik_

When running Ruff format I got a slightly confusing message. The Ruff message was confusing, because it was in plural, even though only a single rule was giving me this error.

## Summary

- Updated the code to call `warn_user_once!` twice, handling both singular and plural cases separately.
- Ensures the warning message is grammatically correct when only one incompatible rule is present.

## Test Plan

It's a minor change, so the automatic tests should cover this.


---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:817 on 2024-09-02 11:43_

```suggestion
				if let [rule] == rule_names.as_slice() {
            warn_user_once!("The following rule may cause conflicts when used with the formatter: {rule}. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `select` or `extend-select` configuration, or adding it to the `ignore` configuration.");
        } else {
            warn_user_once!("The following rules may cause conflicts when used with the formatter: {}. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.", rule_names.join(", "));
        }
```

---

_@MichaReiser approved on 2024-09-02 11:43_

Thanks

---

_@RubenVanEldik reviewed on 2024-09-02 11:46_

---

_Review comment by @RubenVanEldik on `crates/ruff/src/commands/format.rs`:817 on 2024-09-02 11:46_

That's way cleaner!

Also learned some new Rust syntax! 🙃 (I'm still very new to Rust haha)

---

_Comment by @codspeed-hq[bot] on 2024-09-02 11:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/RubenVanEldik:dont-use-plural-when-single-rule-warning)

### Merging #13212 will **improve performances by 4.9%**

<sub>Comparing <code>RubenVanEldik:dont-use-plural-when-single-rule-warning</code> (a1e5734) with <code>main</code> (0f85769)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `RubenVanEldik:dont-use-plural-when-single-rule-warning` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 866.2 µs | 825.7 µs | +4.9% |


---

_Label `cli` added by @MichaReiser on 2024-09-02 11:56_

---

_Comment by @github-actions[bot] on 2024-09-02 12:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-09-02 13:16_

---

_Closed by @MichaReiser on 2024-09-02 13:16_

---
