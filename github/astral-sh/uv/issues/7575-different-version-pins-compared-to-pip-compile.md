---
number: 7575
title: Different version pins compared to pip-compile when multiple equivalent resolutions are possible
type: issue
state: closed
author: jf2
labels:
  - question
assignees: []
created_at: 2024-09-20T09:28:24Z
updated_at: 2024-09-20T17:36:16Z
url: https://github.com/astral-sh/uv/issues/7575
synced_at: 2026-01-07T13:12:17-06:00
---

# Different version pins compared to pip-compile when multiple equivalent resolutions are possible

---

_Issue opened by @jf2 on 2024-09-20 09:28_

I've been trying out uv to use as replacement for my workflows and so far, am very impressed by how much has been achieved in such a short period of time.

# Problem Description and Reproduction

The issue below is somewhat tricky, as the behavior is not *wrong* per se, but the outputs are different when comparing with pip-compile output. To demonstrate, below the simple requirements manifest:

```requirements.in
numpy
pandas < 2.2
```

## Using uv
`uv pip compile requirements.in`

Which yields the following output:

```compiled-reqs.txt
numpy==2.1.1
    # via
    #   -r requirements.in
    #   pandas
pandas==2.1.1
    # via -r requirements.in
...
```

## Using pip-compile
`pip-compile requirements.in`

Which yields the following output:
```
numpy==1.26.4

    # via
    #   -r test-requirements.in
    #   pandas
pandas==2.1.4
    # via -r test-requirements.in
...
```

I think that both resolutions are satisfying constraints. The original sin is that `pandas 2.1.1` did not limit `numpy < 2`, whereas `2.1.4` does. `uv` first takes `numpy` and finds the latest possible version. It then tries to find a `pandas` version for which the `numpy == 2.1.1` is satisfied, which correctly resolves to `pandas == 2.1.1` (since this one doesn't limit numpy below 2 yet). pip-compile appears to resolve it differently, but also correctly. 

Interestingly, if you change the order in the requirements manifest file:

```
pandas < 2.2
numpy
```

then `uv` also yields the same output as `pip-compile`. So this is now a tangential issue whereby the order of dependencies influences how the versions are resolved (should that be the case?)

# How to Fix
- Make version resolution independent of the dependency order (possibly enforcing just by sorting the dependency list after parsing)
- (opinionated) When dependency `A == vA` limits dependency `B < vB`, do not check whether a lower version of A (`A < vA`) satisfies `B == vB`.

# System

- pip-compile: 7.4.1
- uv: 0.4.8
- Python: 3.11.7
- OS: AlmaLinux 8.9

# Additional clarifications
This particular can easily be fixed by removing `numpy` as a dependency, which will ensure equivalent output to pip-compile. In this particular case it's easy, since the dependency list is easy. In more general case, an app might specify dependency `A` and `B` as they are both needed in the project, with the author being fully oblivious to the fact that `A` also depends on `B`.

---

_Comment by @charliermarsh on 2024-09-20 17:14_

Thanks for the clear write-up. I think this is roughly by-design. Absent other information, we prioritize dependencies in the order in which they're provided. See, e.g., #5474. IMO, if you want to ensure that you get a certain resolution, the best thing to do is add an additional constraint. (It's also documented, see https://github.com/astral-sh/uv/pull/6211.)

---

_Label `question` added by @charliermarsh on 2024-09-20 17:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 17:14_

---

_Comment by @jf2 on 2024-09-20 17:36_

Yes, after reading through some comments and issues I’ve come to the same conclusion, being that this behaviour is intended. 

I think we can close this issue, it will possibly serve as a reference. 

---

_Closed by @jf2 on 2024-09-20 17:36_

---
