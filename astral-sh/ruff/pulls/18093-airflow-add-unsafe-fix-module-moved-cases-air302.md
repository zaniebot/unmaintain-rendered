```yaml
number: 18093
title: "[`airflow`] Add unsafe fix module moved cases (`AIR302`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: improve-autofix-to-AIR302
created_at: 2025-05-14T13:29:39Z
updated_at: 2025-05-29T20:30:40Z
url: https://github.com/astral-sh/ruff/pull/18093
synced_at: 2026-01-12T15:56:12Z
```

# [`airflow`] Add unsafe fix module moved cases (`AIR302`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add utility functions `generate_import_edit` and `generate_remove_and_runtime_import_edit` to generate the fix needed for the airflow rules.

1. `generate_import_edit` is for the cases where the member name has changed. (e.g., `airflow.datasts.Dataset` to `airflow.sdk.Asset`) It's just extracted from the original logic
2. `generate_remove_and_runtime_import_edit` is for cases where the member name has not changed. (e.g., `airflow.operators.pig_operator.PigOperator` to `airflow.providers.apache.pig.hooks.pig.PigCliHook`) This is newly introduced. As it introduced runtime import, I mark it as an unsafe fix. Under the hook, it tried to find the original import statement, remove it, and add a new import fix

---

* rules fix
    * `airflow.sensors.external_task_sensor.ExternalTaskSensorLink` → `airflow.providers.standard.sensors.external_task.ExternalDagLink`

## Test Plan

<!-- How was it tested? -->
The existing test fixtures have been updated


---

_@Lee-W reviewed on 2025-05-14 13:52_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:983 on 2025-05-14 13:52_

Hey @ntBre , I managed to put up a somewhat work version, but have a quick question. Is there a better way or correct way we can get rid of these expect?

---

_@ntBre reviewed on 2025-05-14 14:47_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:983 on 2025-05-14 14:47_

I guess it depends a bit on what could cause them to fail. I haven't really looked closely enough at `match_head` to see how they can fail. Just in general, you might be able to use `diagnostic.try_set_optional_fix` here, and you can return `Ok(None)` instead of calling `unwrap` or `expect`. Something like this:

```rust
let head = match_head(expr) else {
    return Ok(None);
};
```

That's probably the approach I would take if the result of `match_head` is truly _optional_. If it's an error you want logged, you could instead return an `anyhow::Result` from `match_head` and just use `?` here.

(The main benefit of `try_set_fix` is that it logs any errors that occur in the closure.)

---

_@ntBre reviewed on 2025-05-14 14:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:983 on 2025-05-14 14:50_

Oh I see now that there are other `unwrap`/`expect` calls below. You may just want to factor out another function returning an `Option<Fix>` (or another type if `Fix` isn't right) and only call `diagnostic.try_set_fix` if it returns `Some`:

```rust
if let Some(fix) = new_fix_generating_function(/* args */) {
    diagnostic.try_set_fix(|| /* more code finalizing the fix */);
}
```

Then in `new_fix_generating_function` you can use `?` anywhere you'd otherwise call unwrap or expect.

---

_Comment by @github-actions[bot] on 2025-05-20 09:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Improve autofix to air302" to "[`airflow`] Add unsafe fix module moved cases (`AIR302`)" by @Lee-W on 2025-05-20 09:35_

---

_@Lee-W reviewed on 2025-05-20 09:35_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:983 on 2025-05-20 09:35_

It works great!

---

_Marked ready for review by @Lee-W on 2025-05-20 10:02_

---

_Comment by @Lee-W on 2025-05-27 14:34_

@ntBre  Didn't notice another PR was merged 🤦‍♂️ Just rebased and fix this PR! If the idea looks good, I'm going to apply it to other rules. Thanks!

---

_Comment by @ntBre on 2025-05-28 19:25_

It looks like one of my PRs today caused another conflict. I was going to fix it and push here for you, but it looks like I can't edit the branch. I think you can see the diff [here](https://github.com/astral-sh/ruff/compare/f6f7bd16ef88a244a92ecdccf36f6b032be5975d..950c1462e496e9b425aedf40c9f9ab3daf3e24de#diff-5222e811c4461edf353d24c329e4a07e14c48837c8eeff05b6810912de30135cR1203-R1236) if it helps.

I'll go ahead and take a look now anyway.

---

_Label `rule` added by @ntBre on 2025-05-28 19:25_

---

_Label `preview` added by @ntBre on 2025-05-28 19:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/helpers.rs`:184 on 2025-05-28 19:33_

I think this can just be:

```suggestion
        Expr::Attribute(ExprAttribute { value, .. }) => value.as_name_expr(),
```

I thought this was recursing down the whole attribute sequence, but if by "head" you mean the first part, I think this should work too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:1227 on 2025-05-28 19:51_

I wish there were a way to distinguish between these two cases without trying to generate both edits, but I played with it a bit and couldn't come up with something better. It would also be nice if we could propagate the `Result`s from the import calls to `try_set_fix`, but that looked awkward as well. If we stick with this implementation, I think we can still simplify it slightly to this:

```suggestion
    if let Some(fix) = generate_import_edit(expr, checker, module, name, ranged)
        .or_else(|| generate_remove_and_runtime_import_edit(expr, checker, module, name))
    {
        diagnostic.set_fix(fix);
    }
```

---

_@ntBre approved on 2025-05-28 20:07_

Thanks. I didn't review all of the snaps in depth, but this makes sense to me. Just a couple of suggestions and the merge conflicts and then this is good to go.

---

_@Lee-W reviewed on 2025-05-29 08:31_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:1227 on 2025-05-29 08:31_

I just tried a few methods, and the current one looks the best.

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:184 on 2025-05-29 08:44_

Seems to work fine! 

---

_@Lee-W reviewed on 2025-05-29 08:44_

---

_Merged by @ntBre on 2025-05-29 20:30_

---

_Closed by @ntBre on 2025-05-29 20:30_

---
