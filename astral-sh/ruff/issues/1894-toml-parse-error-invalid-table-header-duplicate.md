---
number: 1894
title: "TOML parse error: Invalid table header: Duplicate key `version`"
type: issue
state: closed
author: hugovk
labels: []
assignees: []
created_at: 2023-01-15T12:38:57Z
updated_at: 2023-01-24T16:34:01Z
url: https://github.com/astral-sh/ruff/issues/1894
synced_at: 2026-01-10T01:22:40Z
---

# TOML parse error: Invalid table header: Duplicate key `version`

---

_Issue opened by @hugovk on 2023-01-15 12:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Run `ruff .` with this `pyproject.toml` in an empty directory:

```toml
# pyproject.toml

[tool.hatch]
version.source = "vcs"

[tool.hatch.version.raw-options]
local_scheme = "no-local-version"
```

Outputs error only without `--isolated` in newest 0.0.222:

```console
$ ruff --version
ruff 0.0.222
$  ruff .
error TOML parse error at line 6, column 1
  |
6 | [tool.hatch.version.raw-options]
  | ^
Invalid table header
Duplicate key `version`

$ ruff . --isolated
warning: No Python files found under the given path(s)
```

This file is parsed fine by other tools, such as Hatchling, python-build, Black, isort etc. Fuller version at https://github.com/hugovk/norwegianblue/

Error both with and without `--isolated` in 0.0.219:

```console
$ ruff --version
ruff 0.0.219
$ ruff .
error TOML parse error at line 6, column 1
  |
6 | [tool.hatch.version.raw-options]
  | ^
Invalid table header
Duplicate key `version`

$ ruff . --isolated
error TOML parse error at line 6, column 1
  |
6 | [tool.hatch.version.raw-options]
  | ^
Invalid table header
Duplicate key `version`
```


Error only without `--isolated` in 0.0.220:

```console
$ ruff --version
ruff 0.0.220
$ ruff .
error TOML parse error at line 6, column 1
  |
6 | [tool.hatch.version.raw-options]
  | ^
Invalid table header
Duplicate key `version`

$ ruff . --isolated
warning: No Python files found under the given path(s)
```





---

_Comment by @messense on 2023-01-15 13:25_

Looks like both [`toml`](https://github.com/toml-rs/toml/tree/main/crates/toml) and [`toml_edit`](https://github.com/toml-rs/toml/tree/main/crates/toml_edit) fail to parse the toml content, see https://www.rustexplorer.com/b/vthrz3

```bash
$ ./playground
Err(
    Error {
        inner: ErrorInner {
            kind: Custom,
            line: Some(
                5,
            ),
            col: 0,
            at: Some(
                55,
            ),
            message: "duplicate key: `version`",
            key: [
                "tool",
                "hatch",
            ],
        },
    },
)
Err(
    TomlError {
        message: "TOML parse error at line 6, column 1\n  |\n6 | [tool.hatch.version.raw-options]\n  | ^\nInvalid table header\nDuplicate key `version`\n",
        line_col: Some(
            (
                5,
                0,
            ),
        ),
    },
)
```

---

_Referenced in [toml-rs/toml#439](../../toml-rs/toml/issues/439.md) on 2023-01-15 13:32_

---

_Referenced in [scikit-build/scikit-build-core#175](../../scikit-build/scikit-build-core/pulls/175.md) on 2023-01-20 01:25_

---

_Comment by @henryiii on 2023-01-20 20:48_

I'm not familiar with the Rust release process, will this be closed when https://github.com/toml-rs/toml/issues/439 is available in Ruff, so I can just watch this issue? :)

---

_Comment by @charliermarsh on 2023-01-20 21:16_

Yeah it should be! I'll try upgrading. It looks like they just cut a release.

---

_Referenced in [astral-sh/ruff#2040](../../astral-sh/ruff/pulls/2040.md) on 2023-01-20 21:26_

---

_Comment by @charliermarsh on 2023-01-20 21:26_

Looks like #2040 should close this out.

---

_Closed by @charliermarsh on 2023-01-20 22:20_

---

_Referenced in [astral-sh/ruff#2058](../../astral-sh/ruff/pulls/2058.md) on 2023-01-21 12:49_

---

_Reopened by @charliermarsh on 2023-01-21 12:49_

---

_Comment by @charliermarsh on 2023-01-21 12:50_

Sorry, a little confusion with respect to the `toml` crate -- the upgrade I performed _did_ solve this problem, but it had the potential to introduce some other incompatibilities with TOML 1. So I want to revert for now, just to minimize churn. I expect this to be closed out soon-ish, but we do need a new release. (See: #2058.)

---

_Closed by @charliermarsh on 2023-01-21 12:54_

---

_Reopened by @charliermarsh on 2023-01-21 12:55_

---

_Referenced in [astral-sh/ruff#2116](../../astral-sh/ruff/pulls/2116.md) on 2023-01-24 00:18_

---

_Closed by @charliermarsh on 2023-01-24 00:22_

---

_Comment by @hugovk on 2023-01-24 16:34_

Thank you for the fix, I can confirm this is fixed in 0.0.231.

I'm impressed it only took 9 days for: report to Ruff, report upstream to Rust TOML library, report upstream to the language-agnostic TOML test suite, then the test suite updated and released, then the TOML library updated and released, and then Ruff updated and released!



---
