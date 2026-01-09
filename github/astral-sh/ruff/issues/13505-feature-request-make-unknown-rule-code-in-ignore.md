---
number: 13505
title: "Feature request: Make unknown rule code in \"ignore\" setting a warning, not an error"
type: issue
state: closed
author: Avasam
labels:
  - linter
  - rule-selection
assignees: []
created_at: 2024-09-24T15:30:47Z
updated_at: 2024-11-19T16:09:43Z
url: https://github.com/astral-sh/ruff/issues/13505
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: Make unknown rule code in "ignore" setting a warning, not an error

---

_Issue opened by @Avasam on 2024-09-24 15:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## What happens currently

At the moment, ignoring a rule code that Ruff doesn't know about in the `ignore` setting will cause an error:
For example using `ruff==0.5.6`:
```toml
[lint]
select = ["ALL"]
ignore = [
  # ...
  "DOC201", # docstring-missing-returns
  "DOC402", # docstring-missing-yields
  "DOC501", # docstring-missing-exception
]
```
Results in:
```
ruff failed
  Cause: Failed to parse [...]]\ruff.toml
  Cause: TOML parse error at line 17, column 1
   |
17 | [lint]
   | ^^^^^^
Unknown rule selector: `DOC402`
```

## What I'd like to see
I'd like such errors, for `ignore` specifically, to be warnings instead

## Why ?
For shared configurations. Not completely failing on unknown ignored error codes allows the same configuration to be valid for a wider range of Ruff versions, reducing maintenance and increasing compatibility on the configuration side. Whether it's because the consumer has updated to a new Ruff version that removed that rule, or because they're still behind but the config disabled a rule they don't have access to yet.

Whilst rn shared configs are mostly done through copy-and-paste (https://github.com/BesLogic/shared-configs/blob/main/ruff.toml) or some sort of git management (https://github.com/jaraco/skeleton/blob/main/ruff.toml). This will become especially relevant with https://github.com/astral-sh/ruff/issues/12352

## Why only `ignore` ?
This request opens the question: why not do the same for rule selection or settings? Because those convey an explicit desire for the code to conform to those rules and configurations. Ignoring a rule, whether it exists or not, results in the same lint validation.

## Prior art
ESLint works very similarly to what I'm asking, with the only difference that it completely ignores the disabled rule, instead of warning.
The "enable all rules + disable what we don't want or need" pattern has allowed me with https://www.npmjs.com/package/eslint-config-beslogic to support a wide range of peer dependencies whilst staying mostly up to date with new rules (until I need to reconfigure a non-disabled newly added rule, but with ESLint's JS-based configs I have ways to work around that too with version-detection, although that's not relevant here)

Flake8 only errors-out on rule codes that are completely impossible. For example ignoring `AAAA` results in `ValueError: Error code 'AAAA' supplied to 'extend-ignore' option does not match '^[A-Z]{1,3}[0-9]{0,3}$'`

Ruff is already able to output warnings, for example when linting non-UTF-8 files:
```
warning: Failed to lint Pythonwin\pywin\test\_dbgscript.py: stream did not contain valid UTF-8
```



---

_Label `linter` added by @zanieb on 2024-09-24 16:26_

---

_Label `rule-selection` added by @zanieb on 2024-09-24 16:26_

---

_Comment by @zanieb on 2024-09-24 16:26_

This seems reasonable to me if @charliermarsh agrees.

---

_Referenced in [astral-sh/ruff#14383](../../astral-sh/ruff/pulls/14383.md) on 2024-11-17 23:58_

---

_Referenced in [astral-sh/ruff#14384](../../astral-sh/ruff/pulls/14384.md) on 2024-11-18 00:04_

---

_Referenced in [astral-sh/ruff#14435](../../astral-sh/ruff/pulls/14435.md) on 2024-11-18 15:27_

---

_Referenced in [astral-sh/ruff#14443](../../astral-sh/ruff/issues/14443.md) on 2024-11-18 20:10_

---

_Comment by @MichaReiser on 2024-11-19 16:08_

This will ship as part of Ruff 0.8

---

_Closed by @MichaReiser on 2024-11-19 16:08_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-19 16:09_

---
