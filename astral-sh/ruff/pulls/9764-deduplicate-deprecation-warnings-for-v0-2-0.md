```yaml
number: 9764
title: Deduplicate deprecation warnings for v0.2.0 release
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2024-02-01T21:40:01Z
updated_at: 2024-02-02T05:26:01Z
url: https://github.com/astral-sh/ruff/pull/9764
synced_at: 2026-01-10T22:57:09Z
```

# Deduplicate deprecation warnings for v0.2.0 release

---

_Pull request opened by @charliermarsh on 2024-02-01 21:40_

## Summary

Adds an additional warning macro (we should consolidate these later) that shows a warning once based on the content of the warning itself. This is less efficient than `warn_user_once!` and `warn_user_by_id!`, but this is so expensive that it doesn't matter at all.

Applies this macro to the various warnings for the v0.2.0 release, and also includes the filename in said warnings, so the FastAPI case is now:

```text
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in /Users/crmarsh/workspace/fastapi/pyproject.toml:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'isort' -> 'lint.isort'
  - 'pyupgrade' -> 'lint.pyupgrade'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
```

---

_Review requested from @zanieb by @charliermarsh on 2024-02-01 21:40_

---

_Review requested from @snowsignal by @charliermarsh on 2024-02-01 21:40_

---

_@charliermarsh reviewed on 2024-02-01 21:40_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:1451 on 2024-02-01 21:40_

The two trailing newlines at the end here looked odd to me in practice.

---

_Review comment by @snowsignal on `crates/ruff_linter/src/logging.rs`:38 on 2024-02-01 21:49_

A small nit for clarity.

```suggestion
pub static PREVIOUS_MESSAGES: Lazy<Mutex<FxHashSet<String>>> = Lazy::new(Mutex::default);
```

---

_@snowsignal approved on 2024-02-01 21:55_

This is great - just one small suggestion about naming.

---

_@zanieb approved on 2024-02-01 21:57_

Yee we really needed this

---

_Comment by @github-actions[bot] on 2024-02-01 22:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
ruff failed
  Cause: Rule `PGH002` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
ruff failed
  Cause: Rule `PGH002` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview is enabled.
```

</p>
</details>




---

_Merged by @zanieb on 2024-02-01 23:10_

---

_Closed by @zanieb on 2024-02-01 23:10_

---

_Branch deleted on 2024-02-01 23:10_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:401 on 2024-02-02 05:02_

Do we know why we call this method twice for every configuration ?

---

_@MichaReiser reviewed on 2024-02-02 05:03_

Thanks 🤩 

---

_@charliermarsh reviewed on 2024-02-02 05:15_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:401 on 2024-02-02 05:15_

Presumedly because we call it once at the very start of Ruff to find the "default configuration", and then once when we visited it as part of the directory of files we're analyzing. (We should fix that -- it's not "incorrect" but it is inefficient.)

---

_@MichaReiser reviewed on 2024-02-02 05:26_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:401 on 2024-02-02 05:26_

...and we didn't catch this in the tests because we pass --config which bypasses the discovery 

---
