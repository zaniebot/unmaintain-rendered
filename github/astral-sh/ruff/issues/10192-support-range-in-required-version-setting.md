---
number: 10192
title: Support range in required-version setting
type: issue
state: closed
author: kujenga
labels:
  - good first issue
  - configuration
  - help wanted
assignees: []
created_at: 2024-03-02T00:46:31Z
updated_at: 2024-03-03T23:43:51Z
url: https://github.com/astral-sh/ruff/issues/10192
synced_at: 2026-01-07T13:12:15-06:00
---

# Support range in required-version setting

---

_Issue opened by @kujenga on 2024-03-02 00:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Currently, it appears that the `required-version` [1] setting allows just a single specific version to be specified. When a version of ruff is present that differs from this version, the ruff tool errors, preventing checks from running.

The ask here is to allow range syntax, e.g. `>=`, `~`, etc., or perhaps treat the `required-version` as a minimum similar to `target-version`, so that ruff does not fail to run in the scenario that e.g. a developer upgrades ruff in their environment but is working within a repo that wants to ensure that everyone is on some minimum version of ruff.

```
$ ruff --version
ruff 0.3.0

$ cat ruff.toml
required-version = "0.2.2"

$ ruff check ./src/example/file.py
ruff failed
  Cause: Required version `0.2.2` does not match the running version `0.3.0`
```

[1] https://docs.astral.sh/ruff/settings/#required-version

---

_Comment by @charliermarsh on 2024-03-02 01:31_

I think it'd be reasonable to support `>=` here.

---

_Label `configuration` added by @charliermarsh on 2024-03-02 01:31_

---

_Comment by @charliermarsh on 2024-03-02 01:31_

(We just need to make it backwards-compatible such that `"0.2.2"` means `"==0.2.2"`.)

---

_Label `good first issue` added by @charliermarsh on 2024-03-02 01:31_

---

_Comment by @MichaReiser on 2024-03-02 13:04_

> I think it'd be reasonable to support `>=` here.

I think we would need to support a full semver syntax similar to [package.json engines](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#engines) or it might be surprising that e.g. `<= isn't supported which, IMO, is entirely reasonable too

A first step before a PR is to spec out the exact behavior of that field with the supported syntax to avoid long discussions on the PR

---

_Label `good first issue` removed by @MichaReiser on 2024-03-02 13:04_

---

_Label `help wanted` added by @MichaReiser on 2024-03-02 13:04_

---

_Comment by @charliermarsh on 2024-03-02 13:13_

Correct, that’s what I meant — we should use Python’s PEP 440 and PEP 508 specifiers since they’re already native, standardized, used in the same TOML file, and we have parsers for them. I don’t think there needs to be any more decision-making than that, personally!

---

_Comment by @MichaReiser on 2024-03-02 13:42_

Seems reasonable, although PEP440 is probably more flexible than what we need (local versions). PEP508 seems unrelated because we don't need a name portion. 

---

_Comment by @charliermarsh on 2024-03-02 13:48_

👍 The gist of it is: it should just function identically to `requires-python` which is a standardized field that we already parse and support (see: `crates/ruff_workspace/src/pyproject.rs`).

---

_Label `good first issue` added by @MichaReiser on 2024-03-02 14:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-03 20:59_

---

_Referenced in [astral-sh/ruff#10216](../../astral-sh/ruff/pulls/10216.md) on 2024-03-03 21:03_

---

_Closed by @charliermarsh on 2024-03-03 23:43_

---
