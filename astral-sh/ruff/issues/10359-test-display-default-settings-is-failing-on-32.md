---
number: 10359
title: Test display_default_settings is failing on 32 bits archs
type: issue
state: closed
author: raspbeguy
labels:
  - bug
  - testing
assignees: []
created_at: 2024-03-12T14:20:53Z
updated_at: 2024-03-12T19:59:39Z
url: https://github.com/astral-sh/ruff/issues/10359
synced_at: 2026-01-10T01:22:50Z
---

# Test display_default_settings is failing on 32 bits archs

---

_Issue opened by @raspbeguy on 2024-03-12 14:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi,

I'm trying to fix [Alpine's packages for ruff](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/testing/ruff/APKBUILD) which is broken on 32 bits architectures because of failing test `display_default_settings`.

Here is the output on x86:

```
---- display_default_settings stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff/tests/snapshots/show_settings__display_default_settings.snap
Snapshot: display_default_settings
Source: crates/ruff/tests/show_settings.rs:30
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
program: ruff
args:
  - check
  - "--show-settings"
  - unformatted.py
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────
  230   230 │ 	gettext,
  231   231 │ 	ngettext,
  232   232 │ ]
  233   233 │ linter.flake8_implicit_str_concat.allow_multiline = true
  234       │-linter.flake8_import_conventions.aliases = {"matplotlib": "mpl", "matplotlib.pyplot": "plt", "pandas": "pd", "seaborn": "sns", "tensorflow": "tf", "networkx": "nx", "plotly.express": "px", "polars": "pl", "numpy": "np", "panel": "pn", "pyarrow": "pa", "altair": "alt", "tkinter": "tk", "holoviews": "hv"}
        234 │+linter.flake8_import_conventions.aliases = {"pandas": "pd", "panel": "pn", "plotly.express": "px", "seaborn": "sns", "tensorflow": "tf", "numpy": "np", "matplotlib": "mpl", "matplotlib.pyplot": "plt", "tkinter": "tk", "polars": "pl", "pyarrow": "pa", "networkx": "nx", "altair": "alt", "holoviews": "hv"}
  235   235 │ linter.flake8_import_conventions.banned_aliases = {}
  236   236 │ linter.flake8_import_conventions.banned_from = []
  237   237 │ linter.flake8_pytest_style.fixture_parentheses = true
  238   238 │ linter.flake8_pytest_style.parametrize_names_type = tuple
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'display_default_settings' panicked at /home/alpine/.cargo/registry/src/index.crates.io-1cd66030c949c28d/insta-1.35.1/src/runtime.rs:563:9:
snapshot assertion for 'display_default_settings' failed in line 30
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @charliermarsh on 2024-03-12 14:22_

Looks like the test is reliant on the order, can fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-12 14:22_

---

_Label `bug` added by @charliermarsh on 2024-03-12 14:22_

---

_Label `testing` added by @zanieb on 2024-03-12 14:25_

---

_Referenced in [astral-sh/ruff#10370](../../astral-sh/ruff/pulls/10370.md) on 2024-03-12 19:13_

---

_Closed by @charliermarsh on 2024-03-12 19:59_

---

_Referenced in [astral-sh/ruff#10470](../../astral-sh/ruff/issues/10470.md) on 2024-03-19 08:02_

---

_Referenced in [astral-sh/ruff#13009](../../astral-sh/ruff/issues/13009.md) on 2024-08-20 17:37_

---
