```yaml
number: 10722
title: Omit variant when detecting compatible Python installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/x86
created_at: 2025-01-17T19:29:59Z
updated_at: 2025-01-21T15:01:21Z
url: https://github.com/astral-sh/uv/pull/10722
synced_at: 2026-01-10T11:45:06Z
```

# Omit variant when detecting compatible Python installs

---

_Pull request opened by @charliermarsh on 2025-01-17 19:29_

## Summary

Closes https://github.com/astral-sh/uv/issues/10586.


---

_Comment by @charliermarsh on 2025-01-17 19:30_

I don't have a great way to test this.

---

_Label `bug` added by @charliermarsh on 2025-01-17 19:30_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-17 19:30_

---

_Comment by @zanieb on 2025-01-17 19:33_

I think I solved this in https://github.com/astral-sh/uv/pull/9788 — one sec

---

_Comment by @zanieb on 2025-01-17 19:34_

Funny, similar https://github.com/astral-sh/uv/pull/9788/commits/d51e5d7736d249e5eddaf3a28d640e78d2528276 — it's a bit more nuanced though

---

_Comment by @charliermarsh on 2025-01-17 19:44_

Want me to port that over here? Close this entirely? Merge as-is? Up to you!

---

_Comment by @zanieb on 2025-01-17 19:52_

I won't have time to look closer today, I think my change might require the other changes in the PR, i.e., we only allow an older version from a newer architecture — not the other way around. So if we can't detect a newer architecture it doesn't solve anything.

---

_Comment by @zanieb on 2025-01-17 19:55_

Quick test looks good, I'm fine with the incremental improvement here

```
❯ uv python install "cpython-3.10.16-linux-x86_64_v2-gnu"
Installed Python 3.10.16 in 2.20s
 + cpython-3.10.16-linux-x86_64_v2-gnu
❯ uv python list
cpython-3.14.0a4+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0a4-linux-x86_64-gnu                 <download available>
cpython-3.13.1+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python3.13
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python3 -> python3.13
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python -> python3
cpython-3.13.1-linux-x86_64-gnu                   /bin/python3.13
cpython-3.13.1-linux-x86_64-gnu                   /bin/python3 -> python3.13
cpython-3.13.1-linux-x86_64-gnu                   /bin/python -> python3
cpython-3.13.1-linux-x86_64-gnu                   <download available>
cpython-3.12.8-linux-x86_64-gnu                   <download available>
cpython-3.11.11-linux-x86_64-gnu                  <download available>
cpython-3.10.16-linux-x86_64-gnu                  <download available>
cpython-3.9.21-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
pypy-3.10.14-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
pypy-3.7.13-linux-x86_64-gnu                      <download available>
❯ ./target/debug/uv python list
cpython-3.14.0a4+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0a4-linux-x86_64-gnu                 <download available>
cpython-3.13.1+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python3.13
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python3 -> python3.13
cpython-3.13.1-linux-x86_64-gnu                   /usr/bin/python -> python3
cpython-3.13.1-linux-x86_64-gnu                   /bin/python3.13
cpython-3.13.1-linux-x86_64-gnu                   /bin/python3 -> python3.13
cpython-3.13.1-linux-x86_64-gnu                   /bin/python -> python3
cpython-3.13.1-linux-x86_64-gnu                   <download available>
cpython-3.12.8-linux-x86_64-gnu                   <download available>
cpython-3.11.11-linux-x86_64-gnu                  <download available>
cpython-3.10.16-linux-x86_64-gnu                  /home/zb/.local/share/uv/python/cpython-3.10.16-linux-x86_64_v2-gnu/bin/python3.10
cpython-3.9.21-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
pypy-3.10.14-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
pypy-3.7.13-linux-x86_64-gnu                      <download available>
❯ uv run -p 3.10 python -c ""
cpython-3.10.16-linux-x86_64-gnu ------------------------------ 16.22 MiB/19.80 MiB                                          ^C
❯ ./target/debug/uv run -p 3.10 python -c ""
```

We shouldn't omit the variant in `python list`  but that's separate

---

_@zanieb approved on 2025-01-17 19:56_

---

_Comment by @zanieb on 2025-01-17 20:04_

You might want to make sure the sorting is right too (as done in the other pr)

---

_Comment by @zanieb on 2025-01-17 20:05_

```
❯ ./target/debug/uv python install 3.10
Installed Python 3.10.16 in 2.81s
 + cpython-3.10.16-linux-x86_64-gnu
❯ ./target/debug/uv python install cpython-3.10.16-linux-x86_64_v2-gnu
Installed Python 3.10.16 in 2.93s
 + cpython-3.10.16-linux-x86_64_v2-gnu
❯ ./target/debug/uv python find 3.10
/home/zb/.local/share/uv/python/cpython-3.10.16-linux-x86_64_v2-gnu/bin/python3.10
```

---

_Merged by @zanieb on 2025-01-21 15:01_

---

_Closed by @zanieb on 2025-01-21 15:01_

---

_Branch deleted on 2025-01-21 15:01_

---
