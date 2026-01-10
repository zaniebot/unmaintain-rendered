```yaml
number: 2223
title: "Escape Windows paths with spaces in `venv` activation command"
type: pull_request
state: merged
author: charliermarsh
labels:
  - windows
assignees: []
merged: true
base: main
head: charlie/esc
created_at: 2024-03-05T22:53:21Z
updated_at: 2024-03-06T02:46:13Z
url: https://github.com/astral-sh/uv/pull/2223
synced_at: 2026-01-10T14:54:43Z
```

# Escape Windows paths with spaces in `venv` activation command

---

_Pull request opened by @charliermarsh on 2024-03-05 22:53_

## Summary

Ensure that we print `& "foo bar\Scripts\activate"` if necessary.


---

_Label `windows` added by @charliermarsh on 2024-03-05 22:53_

---

_Marked ready for review by @charliermarsh on 2024-03-05 22:53_

---

_Merged by @charliermarsh on 2024-03-05 23:11_

---

_Closed by @charliermarsh on 2024-03-05 23:11_

---

_Branch deleted on 2024-03-05 23:11_

---

_Comment by @samypr100 on 2024-03-06 02:00_

@charliermarsh In cmd.exe it would just be `"foo bar\Scripts\activate"` instead of `& "foo bar\Scripts\activate"`, so maybe doing something like
`format!("& \"{executable}\" (PS) or \"{executable}\" (CMD)")` would suffice given shell detection on windows is not very friendly?

---

_Comment by @charliermarsh on 2024-03-06 02:00_

Yeah I can't figure out how to detect if it's PowerShell or CMD... Do you know of any way to do it?

---

_Comment by @samypr100 on 2024-03-06 02:25_

From an env perspective? Not that I'm aware of unfortunately. I did see some code rely on `PSModulePath` for powershell detection by parsing it's value and checking it contains more than 3 entries (see https://stackoverflow.com/a/55598796)
which basically does `is_power_shell = len(os.getenv('PSModulePath', '').split(os.pathsep)) >= 3 `.

It's heuristic because a user could potentially modify PSModulePath to their system environment variables and it would break this detection, but without modifications it would work since PowerShell itself modifies PSModulePath on launch [per docs](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables) and always prefixes three or more entries on startup (regardless of system env) since it's a special PowerShell variable.

---

_Comment by @samypr100 on 2024-03-06 02:29_

Thinking about the oposite angle (which might be simpler), an asnwer on the same SO also mentions cmd.exe always defines `PROMPT` where as PowerShell does not, so maybe we can rely on that to detect cmd.exe and fallback to PS? https://stackoverflow.com/a/66415037

---

_Comment by @samypr100 on 2024-03-06 02:46_

Example implementation of last comment in https://github.com/astral-sh/uv/pull/2226

---
