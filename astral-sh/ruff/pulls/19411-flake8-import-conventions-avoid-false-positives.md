```yaml
number: 19411
title: "[`flake8_import_conventions`] Avoid false positives for NFKC-normalized `__debug__` import aliases in ICN001"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19118
created_at: 2025-07-17T20:07:53Z
updated_at: 2025-08-06T12:36:21Z
url: https://github.com/astral-sh/ruff/pull/19411
synced_at: 2026-01-10T17:52:17Z
```

# [`flake8_import_conventions`] Avoid false positives for NFKC-normalized `__debug__` import aliases in ICN001

---

_Pull request opened by @danparizher on 2025-07-17 20:07_

## Summary

Fixes #19118


---

_Comment by @github-actions[bot] on 2025-07-17 20:18_

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

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs`:73 on 2025-07-18 13:18_

It would be better to do this normalization when loading the settings. We otherwise pay the cost for every import statement

---

_@MichaReiser reviewed on 2025-07-18 13:18_

---

_Label `bug` added by @ntBre on 2025-07-18 13:23_

---

_Label `fixes` added by @ntBre on 2025-07-18 13:23_

---

_@danparizher reviewed on 2025-07-18 15:51_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs`:73 on 2025-07-18 15:51_

I'm not sure if I've worked with settings before, how would it be implemented? Via these?

- `crates\ruff_workspace\src\options.rs`
- `crates\ruff_linter\src\rules\flake8_import_conventions\settings.rs`


---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs`:73 on 2025-07-28 12:46_

Ideally, we'd validate the settings when loading the configuration. The idea is to rename the [`into_settings`](https://github.com/astral-sh/ruff/blob/8c0743df97dc7afa1deab7506b322551f2e392bb/crates/ruff_workspace/src/options.rs#L1653-L1674) method to `try_into_settings` and change its return type to `Result`, similar to 

https://github.com/astral-sh/ruff/blob/8c0743df97dc7afa1deab7506b322551f2e392bb/crates/ruff_workspace/src/options.rs#L1416-L1428

---

_@MichaReiser reviewed on 2025-07-28 12:46_

---

_Comment by @github-actions[bot] on 2025-07-28 19:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-28 19:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-07-29 07:19_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1678 on 2025-07-29 07:19_

Oh. I don't think we need to use `try_into_settings` if we only do the normalization here. 

What I had in mind is to error if the user provides an alias that normalizes to `__debug__`. Or are there cases where we use `aliases` where `__debug__` should be allowed?

---

_@danparizher reviewed on 2025-07-30 15:10_

---

_Review comment by @danparizher on `crates/ruff_workspace/src/options.rs`:1678 on 2025-07-30 15:10_

I don't think `__debug__` is ever valid as an alias. Is this the right idea?

```rust
let mut normalized_aliases: FxHashMap<String, String> = FxHashMap::default();
for (module, alias) in aliases {
    let normalized_alias = alias.nfkc().collect::<String>();
    if normalized_alias == "__debug__" {
        anyhow::bail!(
            "Invalid alias for module '{module}': alias normalizes to '__debug__', which is not allowed."
        );
    }
    normalized_aliases.insert(module, normalized_alias);
}
```

---

_@MichaReiser reviewed on 2025-07-31 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1678 on 2025-07-31 06:34_

Yes, I think that would be the ideal user experience (telling them that the configuration is invalid). 

An alternative is to use `warn_user_once` to log a warning instead (and not adding the alias)

I think either's fine

---

_Review requested from @MichaReiser by @danparizher on 2025-07-31 22:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs`:80 on 2025-08-04 07:58_

Is this check still required, given that we now error in `Flake8ImportConventionsOptions`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_import_conventions/mod.rs`:231 on 2025-08-04 07:59_

I don't think it's possible to implement a fix in the linter. We can implement this as a unit test next to `Flake8ImportConventionsOptions` or as a CLI test (see ruff/tests)

---

_@MichaReiser reviewed on 2025-08-04 07:59_

---

_Review requested from @MichaReiser by @danparizher on 2025-08-05 16:05_

---

_Merged by @MichaReiser on 2025-08-06 06:42_

---

_Closed by @MichaReiser on 2025-08-06 06:42_

---

_Branch deleted on 2025-08-06 12:36_

---
