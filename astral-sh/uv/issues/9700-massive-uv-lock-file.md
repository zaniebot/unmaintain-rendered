```yaml
number: 9700
title: "Massive `uv.lock` file"
type: issue
state: closed
author: NellyWhads
labels: []
assignees: []
created_at: 2024-12-07T00:41:07Z
updated_at: 2024-12-07T03:07:31Z
url: https://github.com/astral-sh/uv/issues/9700
synced_at: 2026-01-12T15:59:56Z
```

# Massive `uv.lock` file

---

_@NellyWhads_

Is it safe to clean up duplicate lines in the `uv.lock` file? Can this be done as part of the `lock` procedure? In my lock file's resolution-markers section, have lines repeated thousands of times.

Example:

```toml
resolution-markers = [
    "python_full_version < '3.9' and platform_machine == 'aarch64' and platform_system == 'Linux'", # (repeated more than 19,999 times)
    "platform_system == 'Darwin'", # (repeated 18,751 times)
    # ... ad nauseum
]
```

This leads to a ~190k-line lock file. Though this is quite a nice bump to my performance metrics at work, DevOps is asking why my python repository is not mostly python code 😅.

---

_Comment by @charliermarsh on 2024-12-07 00:42_

Just to clarify... it's a single `resolution-markers` at the top, with those lines repeated infinitely?

Or you have a bunch of different `resolution-markers`, like one on each package, with unique entries?

---

_Comment by @NellyWhads on 2024-12-07 00:49_

No, this is for the single resolution-markers section at the top of the file

---

_Comment by @charliermarsh on 2024-12-07 00:51_

Yeah, those should definitely be de-duplicated. Do you have lots of conflicting extras or something?

---

_Comment by @charliermarsh on 2024-12-07 00:51_

I think it's the same as https://github.com/astral-sh/uv/issues/9296.

---

_Comment by @NellyWhads on 2024-12-07 00:54_

Fair, I'll watch that thread instead.

---

_Closed by @NellyWhads on 2024-12-07 00:54_

---

_Comment by @charliermarsh on 2024-12-07 00:56_

I'm just curious though, do you have a large number of conflicting extras defined?

---

_Comment by @NellyWhads on 2024-12-07 03:06_

Nope, it was the same pyproject.toml as #9701. After nuking the lock file and re-creating it's back down to ~3k lines with no repetitions.

At one point the `lock` actually brought down my EC2 instance. But restarting and re-locking "just worked". I unfortunately no longer have the ginormous lock file for reference.

---
