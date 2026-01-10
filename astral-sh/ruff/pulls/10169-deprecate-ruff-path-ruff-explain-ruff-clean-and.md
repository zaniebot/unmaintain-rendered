```yaml
number: 10169
title: "Deprecate `ruff <path>` `ruff --explain`, `ruff --clean` and `ruff --generate-shell-completion`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - cli
assignees: []
merged: true
base: main
head: deprecate-command-aliases
created_at: 2024-02-29T10:52:08Z
updated_at: 2024-02-29T13:50:02Z
url: https://github.com/astral-sh/ruff/pull/10169
synced_at: 2026-01-10T22:47:01Z
```

# Deprecate `ruff <path>` `ruff --explain`, `ruff --clean` and `ruff --generate-shell-completion`

---

_Pull request opened by @MichaReiser on 2024-02-29 10:52_

## Summary

Deprecate `ruff <path>` `ruff --explain`, `ruff --clean` and `ruff --generate-shell-completion` in favor of `ruff check <path>`, `ruff rule`, `ruff clean` and `ruff generate-shell-completion`.

Closes https://github.com/astral-sh/ruff/issues/9692

## Test Plan

**`--clean`**

```shell
./target/debug/ruff --clean
warning: `ruff --clean` is deprecated. Use `ruff clean` instead.
Removing cache at: .ruff_cache

❯ ./target/debug/ruff clean

```

**`--generate-shell-completion`**

```shell
❯ ./target/debug/ruff --generate-shell-completion fish > /dev/null
warning: `ruff --generate-shell-completion <SHELL>` is deprecated. Use `ruff generate-shell-completion <SHELL>` instead.


❯ ./target/debug/ruff generate-shell-completion fish > /dev/null


```

**`ruff --explain`**

```shell
❯ ./target/debug/ruff --explain RUF001
warning: `ruff --explain <RULE>` is deprecated. Use `ruff rule <RULE>` instead.
# ambiguous-unicode-character-string (RUF001)

Derived from the **Ruff-specific rules** linter.

...

❯ ./target/debug/ruff rule RUF001
# ambiguous-unicode-character-string (RUF001)
```

**`ruff`**

```shell
❯ ./target/debug/ruff .
warning: `ruff <path>` is deprecated. Use `ruff check <path>` instead.
warning: Detected debug build without --no-cache.
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `crates/ruff/resources/test/fixtures/include-test/nested-project/pyproject.toml`:
  - 'select' -> 'lint.select'

❯ ./target/debug/ruff check  .
warning: Detected debug build without --no-cache.
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `crates/ruff/resources/test/fixtures/include-test/nested-project/pyproject.toml`:
  - 'select' -> 'lint.select'

```


---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-29 10:55_

---

_Label `breaking` added by @MichaReiser on 2024-02-29 10:55_

---

_Label `cli` added by @MichaReiser on 2024-02-29 10:55_

---

_Comment by @github-actions[bot] on 2024-02-29 11:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-02-29 13:39_

---

_@charliermarsh reviewed on 2024-02-29 13:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/main.rs`:41 on 2024-02-29 13:40_

I’ll check, but this one might be used in our Homebrew formula. (Which is ok.)

---

_@MichaReiser reviewed on 2024-02-29 13:49_

---

_Review comment by @MichaReiser on `crates/ruff/src/main.rs`:41 on 2024-02-29 13:49_

Looks good https://github.com/Homebrew/homebrew-core/blob/cfad11e82cc27ed035268a13082278a576bcdb8a/Formula/r/ruff.rb#L23

---

_Merged by @MichaReiser on 2024-02-29 13:50_

---

_Closed by @MichaReiser on 2024-02-29 13:50_

---

_Branch deleted on 2024-02-29 13:50_

---
