```yaml
number: 14618
title: uv workspace ignores package
type: issue
state: closed
author: cosama
labels:
  - bug
assignees: []
created_at: 2025-07-14T22:45:30Z
updated_at: 2025-07-14T22:56:02Z
url: https://github.com/astral-sh/uv/issues/14618
synced_at: 2026-01-12T16:01:53Z
```

# uv workspace ignores package

---

_@cosama_

### Summary

I have a workspace with 4 packages that depend on each other. I install them, three of them get installed, the last one is silently dropped. A minimal reproducible example is in the attached archive. I also added the logs of `uv sync --verbose`. There is no mention of why the last package would not be installed in the workspace. What is weird is that in the `uv.lock` file the missing package is marked as `virtual`, even though it is the same as others and is required in the `pyproject.toml`.

[dummy-test.zip](https://github.com/user-attachments/files/21223146/dummy-test.zip)
[uv_sync_log.txt](https://github.com/user-attachments/files/21223155/uv_sync_log.txt)

### Platform

Linux 6.15.4-200.fc42.x86_64 x86_64 GNU/Linux

### Version

uv 0.7.21

### Python version

Python 3.12.3

---

_Label `bug` added by @cosama on 2025-07-14 22:45_

---

_Comment by @zanieb on 2025-07-14 22:50_

The one that's missing does not declare a build system, and is consequently treated as a virtual package. This is similar to https://github.com/astral-sh/uv/issues/14608#issuecomment-3070039304 and something we're planning to change the default for in the next breaking release.

cc @jtfmumm 

---

_Comment by @cosama on 2025-07-14 22:53_

I was staring at this for hours and not even gemini noticed it. Thanks. That makes sense and adding a build system fixes it. I think it would be good to provide a warning or something. Want me to close this then?

---

_Comment by @zanieb on 2025-07-14 22:56_

>  I think it would be good to provide a warning or something.

Yeah sorry about that, we know it's a surprising default, hence the desire to change it :)



---

_Closed by @zanieb on 2025-07-14 22:56_

---
