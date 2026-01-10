---
number: 9021
title: "Empty environment variables e.g. `UV_FROZEN=''` is treated as an error, not unset"
type: issue
state: open
author: samuelcolvin
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-11-11T16:29:06Z
updated_at: 2025-01-23T17:45:55Z
url: https://github.com/astral-sh/uv/issues/9021
synced_at: 2026-01-10T01:24:35Z
---

# Empty environment variables e.g. `UV_FROZEN=''` is treated as an error, not unset

---

_Issue opened by @samuelcolvin on 2024-11-11 16:29_

Running

```bash
UV_FROZEN='' uv add django --no-sync
```

gives:

```
error: invalid value '' for '--frozen': value was not a boolean

For more information, try '--help'.
```

I'm using `uv 0.4.29 (85f9a0d0e 2024-10-30)`, I also tried on `uv 0.5.1 (f399a5271 2024-11-08)` and got the same behaviour.

Generally empty environment variables are treat as equivalent to them not being set, so I can use an empty env var as "unset".

In my case, I'm trying to unset `UV_FROZEN` in one github action job.

---

_Comment by @zanieb on 2024-11-11 16:30_

Can you share why you'd expect an empty environment variable to be treated as unset? Why not just unset the variable properly?

---

_Comment by @zanieb on 2024-11-11 16:30_

(We have updated some variables to treat an empty value as unset, but I'd like to understand the use-case better)

---

_Comment by @samuelcolvin on 2024-11-11 16:32_

TIL: if you hit enter while writing an issue title, it creates an empty issue.

I've now added more details.

(and I'm upgrading uv just in case it's fixed on newer versions, give me a minute...)


---

_Comment by @samuelcolvin on 2024-11-11 16:34_

> (We have updated some variables to treat an empty value as unset, but I'd like to understand the use-case better)

https://github.com/pydantic/pydantic/pull/10815 - I want to unset `UV_FROZEN` in the `test-plugin` job, to avoid the errors.

---

_Label `configuration` added by @charliermarsh on 2024-11-12 17:34_

---

_Label `needs-decision` added by @charliermarsh on 2024-11-12 17:34_

---

_Comment by @samuelcolvin on 2025-01-22 15:33_

This is getting me again, and it's really ugly to work around!

@charliermarsh any chance you can respect and empty string as equivalent to the env var being unset for all boolean variables (or just `UV_FROZEN` as that's the one that's kiling me).

---

_Comment by @samuelcolvin on 2025-01-22 16:28_

See https://github.com/astral-sh/uv/issues/9021 where I'm try to run

```bash
uv run --resolution lowest-direct coverage run -m pytest
```

But for that to work, I need to unset `UV_FROZEN` which is otherwise set globally in CI.

---

_Referenced in [pydantic/pydantic-ai#743](../../pydantic/pydantic-ai/pulls/743.md) on 2025-01-22 16:28_

---

_Comment by @RazerM on 2025-01-23 17:11_

> it's really ugly to work around

How is it "really ugly" to do `UV_FROZEN=false` or `UV_FROZEN=0` instead of `UV_FROZEN=''` somewhere?

(Maybe you didn't know that uv is parsing bool-like values here?)

Even if the above were not possible, you can unset an environment variable as a one liner with `env -u UV_FROZEN uv ...`

---

_Comment by @samuelcolvin on 2025-01-23 17:45_

If that works, it's not ugly, but I thought it didn't.

---
