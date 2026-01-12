```yaml
number: 10139
title: "uv run script won't install required python version?"
type: issue
state: closed
author: myarcana
labels:
  - question
assignees: []
created_at: 2024-12-24T10:53:03Z
updated_at: 2024-12-24T15:09:18Z
url: https://github.com/astral-sh/uv/issues/10139
synced_at: 2026-01-12T16:00:07Z
```

# uv run script won't install required python version?

---

_@myarcana_

I've made a script with metadata


```py
# /// script
# requires-python = "==3.9"
# ///
```

and when I run `uv run myscript.py`, I get an error

    error: No interpreter found for Python ==3.9 in virtual environments, managed installations, or search path

That's surprising; I would expect `uv` to install the required Python version automatically.

---

_Comment by @FishAlchemist on 2024-12-24 12:51_

![Image](https://github.com/user-attachments/assets/1cc6aef9-0bbd-4adb-8cda-7711564a8056)
**My uv version**: uv 0.5.11 (c4d0caaee 2024-12-19)
I cannot reproduce the issue you described on my Windows system. Can you provide the output with the ``--verbose`` flag?
```
uv run --verbose myscript.py 
```

If possible, please use the following command to provide UV setting information.
```
 uv run --show-settings
```

---

_Comment by @charliermarsh on 2024-12-24 13:02_

(As an aside, you likely do _not_ want `requires-python = "==3.9"`. That requests exactly `3.9.0`, and no later patch versions. You're probably looking for `requires-python = ">=3.9"` or `requires-python = "==3.9.*"`.)

---

_Comment by @charliermarsh on 2024-12-24 13:03_

I think @zanieb would know the details on intended behavior here.

---

_Comment by @zanieb on 2024-12-24 13:48_

We don't have a managed download of `3.9.0` so we can't automatically download the requested version.

---

_Comment by @zanieb on 2024-12-24 13:48_

Perhaps the canonical display of requests should add a trailing zero to these versions.

---

_Comment by @charliermarsh on 2024-12-24 14:18_

Do we have one for Windows, then?

---

_Comment by @charliermarsh on 2024-12-24 15:09_

Ahh ok yeah. It exists for Windows and Intel macOS, but not ARM.

---

_Closed by @charliermarsh on 2024-12-24 15:09_

---

_Label `question` added by @charliermarsh on 2024-12-24 15:09_

---
