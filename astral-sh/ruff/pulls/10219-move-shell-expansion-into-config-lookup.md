```yaml
number: 10219
title: "Move shell expansion into `--config` lookup"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/path
created_at: 2024-03-04T00:07:49Z
updated_at: 2024-03-04T17:46:11Z
url: https://github.com/astral-sh/ruff/pull/10219
synced_at: 2026-01-10T22:47:01Z
```

# Move shell expansion into `--config` lookup

---

_Pull request opened by @charliermarsh on 2024-03-04 00:07_

## Summary

When users provide configurations via `--config`, we use `shellexpand` to ensure that we expand signifiers like `~` and environment variables.

In https://github.com/astral-sh/ruff/pull/9599, we modified `--config` to accept either a path or an arbitrary setting. However, the detection (to determine whether the value is a path or a setting) was lacking the `shellexpand` behavior -- it was downstream. So we were always treating paths like `~/ruff.toml` as values, not paths.

Closes https://github.com/astral-sh/ruff-vscode/issues/413.


---

_Label `bug` added by @charliermarsh on 2024-03-04 00:07_

---

_Label `configuration` added by @charliermarsh on 2024-03-04 00:07_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-03-04 00:07_

---

_Comment by @github-actions[bot] on 2024-03-04 02:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -8 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L21'>pandas/core/internals/__init__.py:21:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L33'>pandas/core/internals/__init__.py:33:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L37'>pandas/core/internals/__init__.py:37:16:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L53'>pandas/core/internals/__init__.py:53:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L57'>pandas/core/internals/__init__.py:57:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L61'>pandas/core/internals/__init__.py:61:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L65'>pandas/core/internals/__init__.py:65:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/1bf86a35a56405e07291aec8e07bd5f7b8b6b748/pandas/core/internals/__init__.py#L69'>pandas/core/internals/__init__.py:69:13:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 7 | 0 | 7 | 0 | 0 |
| PLR6201 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:1169 on 2024-03-04 09:49_

No need for the tempdir filter here -- the tempdir path doesn't appear in the stderr or stdout:

```suggestion
    assert_cmd_snapshot!(Command::new(get_cargo_bin(BIN_NAME))
        .args(STDIN_BASE_OPTIONS)
        .arg("--config")
        .arg("${NAME}.toml")
        .env("NAME", "ruff")
        .arg("-")
        .current_dir(tempdir.path())
        .pass_stdin(r#"
import os

def func():
    x = 1
"#), @r###"
    success: false
    exit_code: 1
    ----- stdout -----
    -:2:8: F401 [*] `os` imported but unused
    Found 1 error.
    [*] 1 fixable with the `--fix` option.

    ----- stderr -----
    "###);
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:827 on 2024-03-04 09:51_

```suggestion
            shellexpand::full(value).map(|config| PathBuf::from(&config))
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:815 on 2024-03-04 09:55_

What's the motivation for changing the ordering from path, value to value, path? Is it because `shellexpand` requires an UTF8 path? 

True, it's an edge case but I think it might be worth falling back to normal paths if it's not valid UTF 8

```
let Ok(value) = value.to_str() else {
  let path_to_config_file = PathBuf::from(value);
  if path_to_config_file.is_file() {
    return Ok(SingleConfigArgument::FilePath(path_to_config_file));
  }
  return Err(clap::error::ErrorKind::InvalidUtf8);
}
```

---

_@MichaReiser approved on 2024-03-04 09:58_

---

_Label `cli` added by @MichaReiser on 2024-03-04 09:58_

---

_Label `configuration` removed by @MichaReiser on 2024-03-04 09:58_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:829 on 2024-03-04 10:00_

The error message on line 884/887 specifically complains to the user that the configuration path "does not exist", but with this new logic, we're (correctly) checking that it's a _file_ that exists as well as just checking that it exists. So maybe we should change that error message to something like

```diff
diff --git a/crates/ruff/src/args.rs b/crates/ruff/src/args.rs
index 6c3e0f056..93f9ebe3e 100644
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -884,7 +884,7 @@ A `--config` flag must either be a path to a `.toml` configuration file
                     "
 
 It looks like you were trying to pass a path to a configuration file.
-The path `{value}` does not exist"
+The path `{value}` does not point to a configuration file."
```

---

_@AlexWaygood reviewed on 2024-03-04 10:00_

---

_Comment by @AlexWaygood on 2024-03-04 10:06_

Thanks for fixing -- sorry I missed this!

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:827 on 2024-03-04 17:13_

You'll be surprised to learn that this doesn't work, because `config` is `Cow<Xstr>`.

---

_@charliermarsh reviewed on 2024-03-04 17:13_

---

_@MichaReiser reviewed on 2024-03-04 17:19_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:827 on 2024-03-04 17:19_

You just need to find the right number of stars: Maybe `&**config`?

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:817 on 2024-03-04 17:26_

Worth adding a test with a non-UTF-8 path?

---

_@AlexWaygood approved on 2024-03-04 17:26_

---

_Merged by @charliermarsh on 2024-03-04 17:45_

---

_Closed by @charliermarsh on 2024-03-04 17:45_

---

_Branch deleted on 2024-03-04 17:45_

---

_@charliermarsh reviewed on 2024-03-04 17:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:817 on 2024-03-04 17:46_

I'm gonna defer it, I don't even know if Ruff as a whole will work with non-UTF-8 paths.

---
