```yaml
number: 17149
title: UV return returncode 4294967295
type: issue
state: closed
author: Wintreist
labels:
  - bug
assignees: []
created_at: 2025-12-16T14:29:20Z
updated_at: 2025-12-16T14:37:59Z
url: https://github.com/astral-sh/uv/issues/17149
synced_at: 2026-01-12T16:02:44Z
```

# UV return returncode 4294967295

---

_@Wintreist_

### Summary

I run Pytest via `uv run` and read the returncode from the process. For some reason, the exit code is 4294967295. 
I went to the UV and Pytest repositories and searched for this number. 
Pytest has nothing.
UV has this number in the [test](https://github.com/astral-sh/uv/blob/0cee76417ff30d1e9329ca9b7bf3d03f4f1f33b2/crates/uv/tests/it/extract.rs#L183). 
I do not know how it happens. Usually this behavior is not stable, but today I have a lot of tests completed with this exit code:

<img width="330" height="529" alt="Image" src="https://github.com/user-attachments/assets/b12ec610-df0c-4f46-be9c-ab61ffa7a519" />

My code:
```python
proc = subprocess.run(
    ["uv", "run", "pytest", *cmd],
    text=True,
    cwd=self.test_suite.root_dir,
)
r_code = proc.returncode
```
I do this because I'm starting a UV process from another process, with a different environment.

### Platform

Windows 10 x86_64

### Version

0.9.17

### Python version

3.12.12

---

_Label `bug` added by @Wintreist on 2025-12-16 14:29_

---

_Comment by @konstin on 2025-12-16 14:32_

The number you're seeing is 0xFFFFFFFF, or -1 for a u32. I'm not sure why you're seeing this exit code specifically.

---

_Comment by @Wintreist on 2025-12-16 14:37_

@konstin 
Wow, thanks a lot. I hadn't thought of that. Yes, this solves my problem, since this is a "Custom" exit code that my company added to the pytest logic. We just used to get it directly from pytest.
Thanks again, it's not a UV problem.

---

_Closed by @Wintreist on 2025-12-16 14:37_

---
