---
number: 11474
title: "Dependency Group Includes From [project]"
type: issue
state: closed
author: RomainBrault
labels:
  - question
assignees: []
created_at: 2025-02-13T10:13:52Z
updated_at: 2025-02-13T16:00:15Z
url: https://github.com/astral-sh/uv/issues/11474
synced_at: 2026-01-10T01:25:06Z
---

# Dependency Group Includes From [project]

---

_Issue opened by @RomainBrault on 2025-02-13 10:13_

### Question

In my projects I am using dependency groups to list my tests dependencies (pytest, etc.).

I would like to test a minimum supported Python version (using uv's lowest resolution). However because the dependency group "tests" is not present in the wheel, the lowest resolution in my test might not be the same than the one after installing the wheel.

A solution I am thinking of would be to include the dependency group as an extra. This is mentioned in the now outdated PEP 735: https://peps.python.org/pep-0735/#use-cases-for-dependency-group-includes-from-project. Is there yet a plan to support this include in uv ?

Otherwise I would be happy to hear of alternative solution to guarantee the same resolution when installing the wheel and testing with dependency groups.

### Platform

ubuntu 24.10

### Version

0.5.31

---

_Label `question` added by @RomainBrault on 2025-02-13 10:13_

---

_Comment by @RomainBrault on 2025-02-13 10:20_

Other viable solutions I see would be:
 * supporting groups in `uv pip compile` (there is #8969 but we don't know when it will be implemented/merged)
 * give the possibility to have multiple lockfiles or have a lockfile with multiple resolution (I do not want the CI to change/update the lockfile, I usually run the tests using --frozen).

The only solution I currently see is:
 1) export the group dependency with `uv export --group -o requirements-dev.txt`
 2) compile with constraints `uv pip compile --resolution=lowest --constraints requirements-dev.txt -o requirements.txt`

But I cannot change the resolution on my tests requirements without updating the lockfile, and there is still the possibility that some user's environment will resolve dependencies when installing the wheel that were not tested, due to the dev requirements passed as constraints (that's why I'd like to add them as an extra too).


---

_Comment by @zanieb on 2025-02-13 15:37_

This was explicitly taken out of the PEP and is in the rejected ideas section https://peps.python.org/pep-0735/#why-not-support-dependency-group-includes-in-project-dependencies-or-project-optional-dependencies

We won't be implementing something that's not complaint with the `[project]` table standards.

It sounds like an extra is the best option here? I'm not sure. Running the test suite against wheels is unfortunately tricky.

---

_Comment by @RomainBrault on 2025-02-13 16:00_

Thanks for pointing me out the relevant paragraph!

---

_Closed by @RomainBrault on 2025-02-13 16:00_

---
