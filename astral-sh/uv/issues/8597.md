```yaml
number: 8597
title: "Resolver: tighter requires-python bound in consumer fails to satisfy"
type: issue
state: closed
author: offbyone
labels:
  - documentation
assignees: []
created_at: 2024-10-26T16:57:22Z
updated_at: 2024-10-28T00:52:57Z
url: https://github.com/astral-sh/uv/issues/8597
synced_at: 2026-01-10T04:36:20Z
```

# Resolver: tighter requires-python bound in consumer fails to satisfy

---

_Issue opened by @offbyone on 2024-10-26 16:57_

I have two projects, both of which I control -- so this is not a blocker for me, but I think it's doing the wrong thing.

I have a consuming project, `seattle-2025`, that has `requires-python = "~=3.12.3"` and a dependency project, `nomnom-hugoawards` that has `requires-python = "~=3.12.4"`

On the surface, these seem to be satisfiable when resolving dependencies for `seattle-2025`; there are plenty of Python versions in the range, including `3.12.4`. 

However, when I attempt to sync, here's what I get:

```shellsession
$ uv sync
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.12.3, <3.13) does not satisfy Python>=3.12.4,<3.13
      and nomnom-hugoawards==2025.0.0b5.dev1 depends on Python>=3.12.4,<3.13, we can conclude that
      nomnom-hugoawards==2025.0.0b5.dev1 cannot be used.
      And because only nomnom-hugoawards==2025.0.0b5.dev1 is available and your project depends on
      nomnom-hugoawards, we can conclude that your project's requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.12.3, <3.13) includes Python versions that are not
      supported by your dependencies (e.g., nomnom-hugoawards==2025.0.0b5.dev1 only supports >=3.12.4,
      <3.13). Consider using a more restrictive `requires-python` value (like >=3.12.4, <3.13).
$ uv --version
uv 0.4.27 (36b729e92 2024-10-25)
```

Maybe I'm missing something, but I would expect dependency resolving to find the intersection of the required python versions, rather than require the consumer to be a superset. I wasn't able to infer this behavior from the [resolver docs](https://docs.astral.sh/uv/concepts/resolution/#source-distribution).

In this case, I'd expect 3.12.4 to satisfy the resolver. 

---

_Renamed from "Resolver bug - greater requires-python bound in consumer fails to satisfy" to "Resolver: greater requires-python bound in consumer fails to satisfy" by @offbyone on 2024-10-26 16:57_

---

_Renamed from "Resolver: greater requires-python bound in consumer fails to satisfy" to "Resolver: tighter requires-python bound in consumer fails to satisfy" by @offbyone on 2024-10-26 16:57_

---

_Comment by @charliermarsh on 2024-10-26 16:58_

Your project needs to be a subset, since right now, your project says it supports Python 3.12.3, but you have a dependency that doesn't support that version. We're not looking for a single version that works; we're looking to create a resolution that works for all the stated Python versions.

---

_Comment by @offbyone on 2024-10-26 17:00_

Oh... okay, that is very much not how I interpreted those bounds. 

Fair enough, and I'll make the changes on my end -- the nice part about controlling both sides of this!. I suppose on some level this is a documentation bug report, then. Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-10-27 19:11_

---

_Closed by @charliermarsh on 2024-10-28 00:52_

---

_Closed by @charliermarsh on 2024-10-28 00:52_

---
