```yaml
number: 9763
title: "Fix issue where output format mode would not change to `full` if preview mode was set in configuration file"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
assignees: []
merged: true
base: main
head: jane/bugs/fix-output-format-preview-mode
created_at: 2024-02-01T21:20:16Z
updated_at: 2024-02-01T22:19:27Z
url: https://github.com/astral-sh/ruff/pull/9763
synced_at: 2026-01-12T15:55:30Z
```

# Fix issue where output format mode would not change to `full` if preview mode was set in configuration file

---

_@snowsignal_

## Summary

This was causing build failures for #9599. We were referencing the command line overrides instead of the merged configuration data, hence the issue.

## Test Plan

A snapshot test was added.

---

_Review requested from @zanieb by @snowsignal on 2024-02-01 21:20_

---

_Label `bug` added by @snowsignal on 2024-02-01 21:20_

---

_Review comment by @zanieb on `crates/ruff/src/lib.rs`:326 on 2024-02-01 21:25_

We should also link a TODO here to #8232 to make sure this references the _global_ preview setting not the linter specific one

---

_@zanieb reviewed on 2024-02-01 21:25_

---

_Comment by @zanieb on 2024-02-01 21:25_

cc @AlexWaygood 

---

_Review requested from @AlexWaygood by @snowsignal on 2024-02-01 21:28_

---

_@zanieb approved on 2024-02-01 21:28_

---

_@AlexWaygood reviewed on 2024-02-01 21:28_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/integration_test.rs`:784 on 2024-02-01 21:28_

Could possibly return a `Result` from this test? Though I don't think it makes too much difference

```suggestion
fn full_output_preview_config() -> Result<()> {
    let tempdir = TempDir::new()?;
    let pyproject_toml = tempdir.path().join("pyproject.toml");
    fs::write(
        &pyproject_toml,
        r#"
[tool.ruff]
preview = true
"#,
    )?;
    let mut cmd = RuffCheck::default().config(&pyproject_toml).build();
    assert_cmd_snapshot!(cmd.pass_stdin("l = 1"), @r###"
    success: false
    exit_code: 1
    ----- stdout -----
    -:1:1: E741 Ambiguous variable name: `l`
      |
    1 | l = 1
      | ^ E741
      |

    Found 1 error.

    ----- stderr -----
    "###);
    Ok(())
```

---

_@zanieb reviewed on 2024-02-01 21:30_

---

_Review comment by @zanieb on `crates/ruff/tests/integration_test.rs`:784 on 2024-02-01 21:30_

Yeah that can feel cleaner than writing unwrap :)

---

_Comment by @AlexWaygood on 2024-02-01 21:30_

I think the added test also passes on the `main` branch. I was having difficulty finding a test that would fail for me on the `main` branch as a result of this issue. But the change looks correct to me, anyway!

---

_Review requested from @AlexWaygood by @snowsignal on 2024-02-01 21:38_

---

_Comment by @github-actions[bot] on 2024-02-01 21:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
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
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
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

_Comment by @zanieb on 2024-02-01 22:00_

> I think the added test also passes on the `main` branch. I was having difficulty finding a test that would fail for me on the `main` branch as a result of this issue. But the change looks correct to me, anyway!

Really? That doesn't make sense to me. We should try to have a regression test.

---

_Comment by @zanieb on 2024-02-01 22:07_

Going to merge so we can move forward with the release. We should investigate more separately.

---

_Merged by @zanieb on 2024-02-01 22:07_

---

_Closed by @zanieb on 2024-02-01 22:07_

---

_Branch deleted on 2024-02-01 22:07_

---

_Comment by @AlexWaygood on 2024-02-01 22:19_

Thanks for the quick fix @snowsignal! :D

---
