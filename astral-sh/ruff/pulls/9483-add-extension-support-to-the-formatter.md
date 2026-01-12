```yaml
number: 9483
title: "Add `--extension` support to the formatter"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2024-01-12T01:11:54Z
updated_at: 2024-01-12T19:01:17Z
url: https://github.com/astral-sh/ruff/pull/9483
synced_at: 2026-01-12T15:55:29Z
```

# Add `--extension` support to the formatter

---

_@charliermarsh_

## Summary

We added `--extension` to `ruff check`, but it's equally applicable to `ruff format`.

Closes https://github.com/astral-sh/ruff/issues/9482.

Resolves https://github.com/astral-sh/ruff/discussions/9481.

## Test Plan

`cargo test`

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-12 01:11_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-12 01:11_

---

_Marked ready for review by @charliermarsh on 2024-01-12 01:12_

---

_Label `cli` added by @charliermarsh on 2024-01-12 01:12_

---

_@charliermarsh reviewed on 2024-01-12 01:12_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:1085 on 2024-01-12 01:12_

(This is called `config` in the `LinterConfiguration` impl, so made it consistent.)

---

_@charliermarsh reviewed on 2024-01-12 01:12_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:1089 on 2024-01-12 01:12_

(New setting.)

---

_Comment by @github-actions[bot] on 2024-01-12 01:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_cli/src/args.rs`:295 on 2024-01-12 06:50_

What's the reason for hiding the option? IMO, we should either add options if we feel confident that they're good or not add them, but adding them as hidden feels indecisive. 

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/args.rs`:748 on 2024-01-12 06:53_

Nit: This clones the entries while iterating rather than cloning the entire vector, iterating, and than collecting it into a new `Vec`. I suspect that Rust can optimize this properly in release builds, but I find this expresses the intent better.
```suggestion
            config.extension = Some(extension.iter().cloned().collect());
```

---

_@MichaReiser approved on 2024-01-12 06:57_

Looks good.

I recommend removing the hidden from the CLI argument. We should not add it if we don't feel confident about supporting it.

---

_@charliermarsh reviewed on 2024-01-12 13:59_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:295 on 2024-01-12 13:59_

Oops, I copied this over from the `linter` version -- I will remove both.

---

_Merged by @charliermarsh on 2024-01-12 18:53_

---

_Closed by @charliermarsh on 2024-01-12 18:53_

---

_Branch deleted on 2024-01-12 18:53_

---
