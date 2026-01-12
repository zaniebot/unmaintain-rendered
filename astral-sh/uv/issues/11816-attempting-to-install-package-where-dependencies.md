```yaml
number: 11816
title: Attempting to install package where dependencies give git+url and needs environment variables to be set.
type: issue
state: closed
author: CompRhys
labels:
  - question
assignees: []
created_at: 2025-02-27T01:52:20Z
updated_at: 2025-04-18T19:12:51Z
url: https://github.com/astral-sh/uv/issues/11816
synced_at: 2026-01-12T16:00:46Z
```

# Attempting to install package where dependencies give git+url and needs environment variables to be set.

---

_@CompRhys_

### Question

I am attempting to uv to create an environment for a package using https://github.com/facebook/Ax and https://github.com/pytorch/botorch both of these are installed using git+url in the pyproject. The issue is that for Ax not to require a pinned version of botorch it checks for the environment variable "ALLOW_BOTORCH_LATEST" in https://github.com/facebook/Ax/blob/main/setup.py. All the ways I have tried to export this as true before running uv sync still lead to the error 
```
  × No solution found when resolving dependencies:
  ╰─▶ Because only ax-platform==0.1.dev3838 is available and ax-platform==0.1.dev3838 depends on botorch==0.13.0, we can
      conclude that all versions of ax-platform depend on botorch==0.13.0.
      And because there is no version of botorch==0.13.0 and your project depends on ax-platform, we can conclude that your
      project's requirements are unsatisfiable.
```

How can I attempt to resolve this bug/error?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @CompRhys on 2025-02-27 01:52_

---

_Comment by @konstin on 2025-02-28 13:41_

I assume that the Ax build got cached on the first build with the too-strict requirement, you can try `--reinstall-package Ax` to rebuild it.

If that doesn't work, you can also try building Ax without [build isolation](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation).

---

_Comment by @CompRhys on 2025-02-28 14:40_

```
>>> ALLOW_BOTORCH_LATEST=true uv sync --reinstall-package Ax 
    Updated ssh://git@github.com/Radical-AI/radical.git (0a48a221c78eaf93bf3660f5f2765c32511b4ca0)
  × No solution found when resolving dependencies:
  ╰─▶ Because only ax-platform==0.1.dev3838 is available and ax-platform==0.1.dev3838 depends on botorch==0.13.0, we can conclude that all versions of ax-platform depend on
      botorch==0.13.0.
      And because there is no version of botorch==0.13.0 and your project depends on ax-platform, we can conclude that your project's requirements are unsatisfiable.
```

I don't think the first idea works. I will explore the second some more but I also don't think it's the right solution here. Will explore more with Ax developers whether they can relax their dependencies a little.

---

_Comment by @charliermarsh on 2025-03-05 16:38_

Have you tried setting an override?

---

_Comment by @CompRhys on 2025-03-06 01:36_

Adding the following to the `pyproject.toml` seems to work by stopping `ax-platform` from complaining about the botorch version
```
[tool.uv]
override-dependencies = [
    "botorch @ git+https://github.com/Radical-AI/botorch.git@main",
]
```

---

_Closed by @CompRhys on 2025-03-06 01:43_

---

_Comment by @charliermarsh on 2025-03-06 01:53_

Nice, thanks for following up.

---

_Comment by @charliermarsh on 2025-03-06 01:53_

You can read about overrides here if helpful: https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides

---

_Comment by @CompRhys on 2025-03-24 21:43_

@charliermarsh I was going further with this same package today and moved these to an optional dependency, if they're installed as an optional dependency then the override-dependencies "fix" from before doesn't stop it from complaining. Is there any further logic to add here to deal with this?

---

_Reopened by @CompRhys on 2025-03-24 21:43_

---

_Comment by @CompRhys on 2025-03-25 13:13_

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only ax-platform==0.1.dev3938 is available and
      ax-platform==0.1.dev3938 depends on botorch>=0.13.0, we can conclude
      that all versions of ax-platform depend on botorch>=0.13.0.
      And because only botorch<0.13.0 is available and
      radical-comp[doe]==0.1.0 depends on ax-platform, we can conclude that
      radical-comp[doe]==0.1.0 cannot be used.
      And because only radical-comp[doe]==0.1.0 is available and you
      require radical-comp[doe], we can conclude that your requirements are
      unsatisfiable.
```

---

_Comment by @zanieb on 2025-04-01 19:10_

Perhaps you want to add a source? https://docs.astral.sh/uv/concepts/projects/dependencies/#git

---

_Comment by @CompRhys on 2025-04-18 19:12_

I think the issue here was that installing like `uv pip install "./package"`  and `cd package; uv pip install .` give different behavior due to uv reading the pyproject in the current directory and then ignoring commands in other directories. This means that the "override-dependencies" will need to be in the toml of everything in the monorepo we use but seemingly I now understand what was going wrong.

---

_Closed by @CompRhys on 2025-04-18 19:12_

---
