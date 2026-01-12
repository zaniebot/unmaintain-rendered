```yaml
number: 12803
title: "`import-outside-top-level (PLC0415)` is incompatible with `flake8-tidy-imports.banned-module-level-imports`"
type: issue
state: closed
author: Avasam
labels:
  - bug
  - accepted
assignees: []
created_at: 2024-08-11T17:23:28Z
updated_at: 2025-01-24T10:07:23Z
url: https://github.com/astral-sh/ruff/issues/12803
synced_at: 2026-01-12T15:54:52Z
```

# `import-outside-top-level (PLC0415)` is incompatible with `flake8-tidy-imports.banned-module-level-imports`

---

_@Avasam_

Reframed issue:

`import-outside-top-level (PLC0415)` and `banned-module-level-imports (TID253)` are incompatible together.

`ruff.toml`
```toml

[lint]
preview = true
extend-select = [
	"TID253", # banned-module-level-imports, see banned-module-level-imports below
	"PLC0415", # import-outside-top-level
]

[lint.flake8-tidy-imports]
# Modules we want setuptools to be able to work without requiring
banned-module-level-imports = [
	"ctypes", # see #4461 for example
	"typing_extensions", # Useful for development, shouldn't be required at runtime
	# platform-specific imports
	"winreg",
]
```

Code:
```py
import winreg # raises TID253

def win_only_method():
    import winreg # raises PLC0415

    print(winreg)
```

<details>
<summary>Original issue</summary>

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  PLC0415, top level module import, banned api
* The current Ruff version (`ruff --version`): ruff 0.5.5

I came across a use-case where it would be preferable to check that certain imports are never imported directly at the module level. This is mainly for dependencies that should stay optional, or that are known to be be ostly and only useful in specific codepaths.

For example, setuptools could configure `ctypes`, `typing_extensions`, and more, to ensure that they're never unconditionally imported.

A check that the import is behind a conditional (if-else, try-except) or in a function, should be sufficient.

This is similar in idea to the https://docs.astral.sh/ruff/rules/#flake8-type-checking-tch (TCH001, TCH002, TCH003) rules.

This would conflict with https://docs.astral.sh/ruff/rules/import-outside-top-level/ (PLC0415). My original idea was to suggest an ignore list for PLC0415, but I think this could be pushed further with a rule that actually enforces it.
If this idea is rejected, I'd still like an ignore/allowlist for PLC0415 .

</details>



---

_Label `rule` added by @charliermarsh on 2024-08-11 19:24_

---

_Comment by @charliermarsh on 2024-08-11 19:25_

I think this is pretty reasonable. I've implemented this as a custom lint rule (for tensorflow -- to ensure it's always imported lazily) in the past.

---

_Label `accepted` added by @charliermarsh on 2024-08-11 19:25_

---

_Comment by @charliermarsh on 2024-08-11 19:25_

Any imports that are marked as required-top-level should also be ignored in `PLC0415`.

---

_Comment by @Avasam on 2024-08-11 20:07_

> Any imports that are marked as required-top-level should also be ignored in `PLC0415`.

Do you mean "required<b>-not-</b>top-level" ?

Also I just found https://docs.astral.sh/ruff/rules/banned-module-level-imports/ , it seems to be doing exactly what I want. But it's not respected by `PLC0415`. So I'll reframe my request.

---

_Renamed from "Feature Request: Configure imports that shouldn't be a module-level" to "`import-outside-top-level (PLC0415)` is incompatible with `flake8-tidy-imports.banned-module-level-imports`" by @Avasam on 2024-08-11 20:09_

---

_Comment by @charliermarsh on 2024-08-11 20:30_

Yes sorry, that’s what I meant. Thanks for spotting that!

---

_Label `rule` removed by @charliermarsh on 2024-08-11 20:30_

---

_Label `bug` added by @charliermarsh on 2024-08-11 20:30_

---

_Comment by @charliermarsh on 2024-08-11 20:31_

I’ll call it a bug that they’re aren’t compatible.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 16:03_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-30 16:20_

---

_Closed by @MichaReiser on 2025-01-24 10:07_

---
