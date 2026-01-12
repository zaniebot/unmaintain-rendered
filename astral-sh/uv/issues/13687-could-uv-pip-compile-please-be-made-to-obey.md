```yaml
number: 13687
title: "Could `uv pip compile` please be made to obey `default-groups`?"
type: issue
state: closed
author: coredumperror
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-05-27T19:12:29Z
updated_at: 2025-05-27T20:02:06Z
url: https://github.com/astral-sh/uv/issues/13687
synced_at: 2026-01-12T16:01:35Z
```

# Could `uv pip compile` please be made to obey `default-groups`?

---

_@coredumperror_

### Summary

We started using the `default-groups` setting in `[tool.uv]` recently, and it's great to help simplify our Dockerfile. But because we also use semgrep, we need a `requirements.txt` file, and thus have to do `uv pip compile` whenever we make changes to our pyproject.toml.

I recently realized that taking out the `--group=test --group=docs` part of our `make compile` command actually made it stop including the test and docs groups' dependencies in `requirements.txt`. That was susprising, given that I assumed `uv pip compile` should work the same way as `uv sync` in regards to `default-groups`.

Is this a bug report, rather than a feature request? I'm not sure.

### Example

_No response_

---

_Label `enhancement` added by @coredumperror on 2025-05-27 19:12_

---

_Comment by @zanieb on 2025-05-27 19:19_

I don't think this is a bug. 

I'm pretty hesitant to read `default-groups` from the `uv pip compile` interface, e.g., `uv pip install -r pyproject.toml` won't respect those either and probably shouldn't because `pip` will not.

---

_Label `needs-decision` added by @zanieb on 2025-05-27 19:19_

---

_Comment by @zanieb on 2025-05-27 19:20_

I think you just want `uv export`?

---

_Comment by @coredumperror on 2025-05-27 19:52_

That looks like it might work, but what are all these dozens of "hash" lines that I'm seeing?
```
--hash=sha256:8863e8e8533a116f45cb92523938ab25879cc31dc594f5de4c3dbd9ab3d440b0
``` 

Most packages seem to have 2, but some have like a dozen of them.

---

_Comment by @zanieb on 2025-05-27 19:56_

Those would be the hashes of the distributions (i.e., wheels). You can use `--no-hashes` if you want, but they're better for security.

---

_Comment by @coredumperror on 2025-05-27 20:02_

Ohhh, OK. No reason not to keep them, I just had no idea what they were for. I'll go ahead and close this.

---

_Closed by @coredumperror on 2025-05-27 20:02_

---
