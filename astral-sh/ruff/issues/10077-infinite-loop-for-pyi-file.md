```yaml
number: 10077
title: "[Infinite loop] for pyi file"
type: issue
state: closed
author: gaborbernat
labels:
  - bug
assignees: []
created_at: 2024-02-22T02:37:55Z
updated_at: 2024-03-05T10:09:17Z
url: https://github.com/astral-sh/ruff/issues/10077
synced_at: 2026-01-12T15:54:49Z
```

# [Infinite loop] for pyi file

---

_@gaborbernat_

https://github.com/tox-dev/pyproject-api/blob/df39546/src/pyproject_api/_backend.pyi#L1

```
❯ pre-commit run --all-files
fix end of files.........................................................Passed
trim trailing whitespace.................................................Passed
Validate GitHub Workflows................................................Passed
codespell................................................................Passed
tox-ini-fmt..............................................................Passed
pyproject-fmt............................................................Passed
ruff-format..............................................................Failed
- hook id: ruff-format
- files were modified by this hook

1 file reformatted, 15 files left unchanged

ruff.....................................................................Failed
- hook id: ruff
- exit code: 1
- files were modified by this hook

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `src/pyproject_api/_backend.pyi`, the rule codes E302, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

src/pyproject_api/_backend.pyi:3:1: E302 Expected 2 blank lines, found 1
Found 105 errors (104 fixed, 1 remaining).
[*] 1 fixable with the --fix option.

```

---

_Renamed from "[Infinite loop]" to "[Infinite loop] for pyi file" by @gaborbernat on 2024-02-22 02:38_

---

_Comment by @gaborbernat on 2024-02-22 02:42_

For now, managed to sidestep the issue with [tox-dev/pyproject-api@`989d105` (#124)](https://github.com/tox-dev/pyproject-api/pull/124/commits/989d105fbb47287298a1d893bf69f0ac936ea159)

---

_Comment by @charliermarsh on 2024-02-22 02:43_

Thanks! I'll take a look at this tonight.

---

_Label `bug` added by @charliermarsh on 2024-02-22 02:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 02:43_

---

_Comment by @MichaReiser on 2024-02-22 15:07_

Okay, it seems this isn't specific to `pyi` files but the conflict is between `E302` and `I001` where `I001` enforces at most 1 blank line (depending on what you specified in `lines-after-imports` and `E302` enforces two empty lines.  

That's also why I haven't been able to reproduce this behavior when running the above code in an isolated unit test where `I001` isn't enabled.

---

_Comment by @gaborbernat on 2024-02-22 15:11_

I wonder if ruff should fail when conflicting settings are set, or pick a winner and ignore the other rules 🤔 

---

_Comment by @MichaReiser on 2024-02-23 12:47_

@gaborbernat, thank you for all your high-quality issues with code examples. It makes fixing issues so much easier
@charliermarsh I'm working on this now. I change the blank line rule to not check empty lines after import statements if isort is enabled.

---

_Unassigned @charliermarsh by @MichaReiser on 2024-02-23 13:02_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-23 13:02_

---

_Closed by @MichaReiser on 2024-03-05 10:09_

---
