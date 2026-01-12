```yaml
number: 9437
title: "RFE: Improve the \"No interpreter found\" error message when python-downloads is set to \"manual\" "
type: issue
state: open
author: hroncok
labels:
  - error messages
assignees: []
created_at: 2024-11-26T10:03:25Z
updated_at: 2025-02-16T23:54:34Z
url: https://github.com/astral-sh/uv/issues/9437
synced_at: 2026-01-12T15:59:50Z
```

# RFE: Improve the "No interpreter found" error message when python-downloads is set to "manual" 

---

_@hroncok_

Hello.

Consider `/etc/uv/uv.toml` with:

```
python-downloads = "manual"
```

And a system without Python 3.X installed.

When I attempt to use Python 3.X, I get an error:

```
$ uv venv --python=python3.X venv
  × No interpreter found for Python 3.X in managed installations or search path
```

This is expected. I opened this issue to ask if the error could be more verbose (and actionable). The most relevant things that I would love to see there are:

* Not downloading Python 3.X because python-downloads is set to "manual" in /etc/uv/uv.toml.
* Run `uv python install 3.X` to manually install it.

Thanks for considering this.

Unfortunately, my Rust skills are 0 and I am not familiar with uv codebase at all, so I won't be able to contribute this.

---

* The current uv platform: Fedora Linux
* The current uv version (`uv --version`): 0.5.4



---

_Label `error messages` added by @charliermarsh on 2024-11-26 14:26_

---

_Assigned to @zanieb by @zanieb on 2025-02-16 23:54_

---
