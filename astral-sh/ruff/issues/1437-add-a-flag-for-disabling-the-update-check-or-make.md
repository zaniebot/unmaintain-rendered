```yaml
number: 1437
title: Add a flag for disabling the update check or make it opt-in
type: issue
state: closed
author: stinos
labels:
  - configuration
assignees: []
created_at: 2022-12-29T09:23:01Z
updated_at: 2022-12-30T09:16:50Z
url: https://github.com/astral-sh/ruff/issues/1437
synced_at: 2026-01-12T15:54:41Z
```

# Add a flag for disabling the update check or make it opt-in

---

_@stinos_

Something like `--no-update-check`. It's fairly uncommon for a commanline tool to have an update check, let alone one which is activated depending on log level and cannot be disabled (or I'm missing something, also possible). Alternative: don't do the update check by default but on demand only.

---

_Comment by @bluetech on 2022-12-29 14:10_

`-q` disables the update check entirely (including the network call).

https://github.com/charliermarsh/ruff/blob/058ee8e6bf6ce482b8ac7423049fe3b9cc9e80a0/src/main_native.rs#L271-L275

---

_Comment by @not-my-profile on 2022-12-29 15:16_

I don't think ruff should by default check for updates secretly in the background every time you run it ... that very much breaks the user expectations for a command-line tool. I think it would be better to just add some stderr output to `-V` / `--version` like:

```
You can check if a new version is available at https://pypi.org/project/ruff/.

To update ruff via pip run:

    pip install ruff --upgrade
```

---

_Comment by @stinos on 2022-12-29 16:04_

> `-q` disables the update check entirely (including the network call).

I'm aware. But `-q` also means no output so one cannot have verbose output combined with no update check. I'm actually not quite sure what the rationale is behind tying update on/off to log level.

---

_Comment by @charliermarsh on 2022-12-29 16:48_

Sure, we can add a setting to disable this.

---

_Comment by @charliermarsh on 2022-12-29 16:48_

(Typing the update to log level is a side effect of the log level goal: show the check violations, and no other output.)

---

_Label `configuration` added by @charliermarsh on 2022-12-29 16:48_

---

_Closed by @charliermarsh on 2022-12-29 18:06_

---

_Comment by @charliermarsh on 2022-12-29 18:32_

Going out in [v0.0.200](https://github.com/charliermarsh/ruff/releases/tag/v0.0.200).

---

_Comment by @bluetech on 2022-12-29 19:37_

Congratulations on 200 releases! Ruff is really terrific.

---

_Comment by @charliermarsh on 2022-12-30 00:58_

@bluetech - Thank you so much! That means a lot. I love hearing it :)

---

_Comment by @stinos on 2022-12-30 09:16_

> Going out in v0.0.200.

Ok that was quick, thanks!!

---
