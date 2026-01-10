---
number: 10302
title: "[UV ADD] How does it perform in cyclic dependencies??"
type: issue
state: closed
author: dsantiago
labels:
  - question
assignees: []
created_at: 2025-01-05T16:38:25Z
updated_at: 2025-01-10T06:43:48Z
url: https://github.com/astral-sh/uv/issues/10302
synced_at: 2026-01-10T01:24:52Z
---

# [UV ADD] How does it perform in cyclic dependencies??

---

_Issue opened by @dsantiago on 2025-01-05 16:38_

I will try to put here each step done to make sense when trying to install a package...

```bash

uv init -p 3.11 AAA # Created a new project for test with python 3.11
cd AAA # Moved to project folder

uv add genaibook # 1st try. Tried to add desired package but got ERROR for for bitsandbytes package wheel
uv add bitsandbytes # Tried direct but ofc same error
uv add https://github.com/bitsandbytes-foundation/bitsandbytes.git # So added via git and now it's OK

uv add genaibook # 2nd try. Now appear to have some cyclic dependencies problem (llvmlite)
uv add llvmlite # Ok (adding previously mentioned packages)

uv add genaibook # 3rd try. Still more packages  cyclic problem (numba)
uv add numba # Ok

uv add genaibook # 4th try. Now it runs!!

 uv run python # So now I could run python and import it
```

Also pyproject was like this:

```toml
[project]
name = "aaa"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "bitsandbytes",
    "genaibook>=0.1.1",
    "llvmlite>=0.43.0",
    "numba>=0.60.0",
]

[tool.uv.sources]
bitsandbytes = { git = "https://github.com/bitsandbytes-foundation/bitsandbytes.git" }
```

The question is if all these errors are because this is the **right** way to add dependencies or is there alternative ways to do it with **less** workaround?

---

_Renamed from "[UV ADD] How does it perform in ciclic dependencies??" to "[UV ADD] How does it perform in cyclic dependencies??" by @dsantiago on 2025-01-06 03:42_

---

_Label `question` added by @zanieb on 2025-01-07 19:37_

---

_Comment by @zanieb on 2025-01-07 19:38_

Might need more information to help here, like logs on the errors. In general, we'll do our best to solve the dependencies despite cycles. I don't know why changing the installation order would help, other than changing the priority of the packages or constraining the problem.

---

_Comment by @dsantiago on 2025-01-08 01:21_

> Might need more information to help here, like logs on the errors. In general, we'll do our best to solve the dependencies despite cycles. I don't know why changing the installation order would help, other than changing the priority of the packages or constraining the problem.

Yeah I agree, but the errors sometimes are text massive... I tried to put exactly the commands I did in a fresh project. What I did in the end was to install the dependencies alone from the `genaibook`. I will put here the sequence screenshoots:

<img width="1437" alt="Image" src="https://github.com/user-attachments/assets/27397197-f62f-4b85-81ac-3fba02662a77" />

<img width="1438" alt="Image" src="https://github.com/user-attachments/assets/fefc8f1c-1da3-4f32-a791-3dd8dfdc23d1" />

<img width="1423" alt="Image" src="https://github.com/user-attachments/assets/b2ab5548-694e-4a20-a261-30549765a9cf" />

And now it worked:

<img width="1217" alt="Image" src="https://github.com/user-attachments/assets/55f34f3b-76d4-4749-bf78-4de167be01d2" />

Cuirous this time `llvmlite` didn't appear in the circular dependency, just `numba`

---

_Comment by @zanieb on 2025-01-08 01:41_

That error isn't a dependency cycle, that's a build failure; https://docs.astral.sh/uv/reference/build_failures/ provides details on troubleshooting those. Presumably adding the other dependencies first constrained the version of numba to avoid the one that can't build? i.e., uv used `numba==0.60.0` when installed alone but the build failure was for `numba==0.53.1`.

---

_Comment by @dsantiago on 2025-01-09 02:00_

Ohhh I see, I would not notice it that easily... Thanks for explaining the scenario @zanieb. I will close this one

---

_Closed by @dsantiago on 2025-01-09 05:21_

---

_Comment by @dsantiago on 2025-01-10 06:43_

I found out that the problem that was happening here was the same one in this issue https://github.com/astral-sh/uv/issues/7020

---
