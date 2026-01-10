---
number: 12505
title: Improve documentation for EXE001 and EXE002
type: issue
state: closed
author: DaniBodor
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-07-25T08:33:49Z
updated_at: 2024-07-28T20:23:01Z
url: https://github.com/astral-sh/ruff/issues/12505
synced_at: 2026-01-10T01:22:52Z
---

# Improve documentation for EXE001 and EXE002

---

_Issue opened by @DaniBodor on 2024-07-25 08:33_

For most rules there is some indication of how the rule is evaluated and an example what changes to make in the code to fix the error. This is not the case for rules [EXE001](https://docs.astral.sh/ruff/rules/EXE001) and [EXE002](https://docs.astral.sh/ruff/rules/EXE002).

Unfortunately, the documentation of the original [flake8-executable](https://github.com/xuhdev/flake8-executable/tree/master) also does not provide any details. Looking into the code, it turns out that the command it runs to check this is `os.access(filename, os.X_OK)` ([see here](https://github.com/xuhdev/flake8-executable/blob/2b6c3fec0f7ccb175fe34f2a613b762af2a35536/flake8_executable/__init__.py#L137)). 

Ideally, there would be some indication of how the file is evaluated (by `os.access`) to determine if it's executable, but this might be a bit much to go into. But at least, it would be useful to mention that this command is used to evaluate (especially useful for [WSL users who currently cannot run the rules locally](#10084)). Maybe also give an example of a file that incorrectly does/doesn't contain a shebang and show the corrected version, to be consistent with other rules (even within the [EXE package](https://docs.astral.sh/ruff/rules/EXE003)).

---

_Label `documentation` added by @charliermarsh on 2024-07-25 21:32_

---

_Label `help wanted` added by @MichaReiser on 2024-07-26 06:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 20:01_

---

_Referenced in [astral-sh/ruff#12547](../../astral-sh/ruff/pulls/12547.md) on 2024-07-28 20:01_

---

_Closed by @charliermarsh on 2024-07-28 20:23_

---
