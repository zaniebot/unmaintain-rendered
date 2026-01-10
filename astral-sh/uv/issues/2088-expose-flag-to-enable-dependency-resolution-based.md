---
number: 2088
title: Expose flag to enable dependency resolution based on packages that were available on a target date
type: issue
state: closed
author: namurphy
labels:
  - enhancement
  - great writeup
assignees: []
created_at: 2024-02-29T16:45:18Z
updated_at: 2024-03-04T17:51:21Z
url: https://github.com/astral-sh/uv/issues/2088
synced_at: 2026-01-10T01:23:12Z
---

# Expose flag to enable dependency resolution based on packages that were available on a target date

---

_Issue opened by @namurphy on 2024-02-29 16:45_

It would be helpful to have a way to solve for dependencies with constraints on the dates that dependencies were released.  For example, I would like to be able to solve for an environment based on what was available two years ago.

### Possible interface

My first thought was to have a `--target-date` flag. Doing `uv pip compile astropy --target-date="2015-04-01"` would solve the environment based on what the pythoniverse was like on 2015 April 1, and exclude versions of packages that were released after that date.  But, I am totally open to other possibilities!

### Use cases

 - **Continuous integration testing** — Packages typically run test suites in multiple environments (e.g., different Python versions, lowest allowed dependencies, different versions of NumPy, etc.).  Having a way to solve for an environment on a particular date would be a straightforward way of generating different test environments. Instead of doing both the oldest and newest environments possible, it'd be straightforward to do some intermediate dates.
 - [**SPEC 0**](https://scientific-python.org/specs/spec-0000/) proposes a policy for the scientific pythoniverse that "support for core package dependencies be dropped 2 years after their initial release." The capability proposed here would make it a lot easier to figure out when to drop dependencies.
 - **Reproducing years-old scientific research.**  If we want to reproduce research from an old Jupyter notebook, it is helpful to have a way to recreate a Python environment from the time the notebook was written.  The Jupyter notebook could have been a supplement to a research publication, or one of our own that we haven't touched in a few years.
 - **Creating an environment to run a package that is no longer being maintained.**  If a package was last updated in 2014, it's likely that it will not work with the newest versions of its dependencies. Being able to solve for an environment based on a particular target date could make it easier to get this old code running again.

### Alternatives

 - With regards to SPEC 0 compliance, I've usually looked up packages on PyPI and updated our `pyproject.toml` manually.  This suffices for small use cases but doesn't scale well.
 - Creating a standalone package would be a possibility, but it would lead to a more fragmented packaging landscape, and I don't think it would be likely to happen.
 - The `--resolution=lowest` and `--resolution=lowest-direct` flags address some of the continuous integration testing use case above. (These flags are awesome, and the reason why I switched to using `uv`!)
 - I thought about also having flags for a minimum date. A flag for a minimum date may be useful in conjunction with `--resolution=lowest` to handle situations where a package doesn't list a minimum version of a dependency and we want to avoid ending up with `v0.0.1`.  However, `--resolution=lowest-direct` would probably suffice for those situations.

Thank you so much for creating an awesome tool!

---

_Comment by @zanieb on 2024-02-29 16:50_

Hi! Thanks for including so much context in your writeup. We do actually support this already, but it's hidden

See:

- https://github.com/astral-sh/uv/issues/1358

---

_Label `duplicate` added by @zanieb on 2024-02-29 16:50_

---

_Label `question` added by @zanieb on 2024-02-29 16:50_

---

_Label `great writeup` added by @zanieb on 2024-02-29 16:50_

---

_Comment by @zanieb on 2024-02-29 16:52_

@charliermarsh should we just expose this flag?

@namurphy would you expect unsolvable requirement errors to pretend these packages do not exist at all or explain that it was unsolvable because newer packages were excluded?

I think I would want to add an environment variable that toggles that behavior so we can omit the message in our test suite but expose it to users. Otherwise, errors can be very confusing.

---

_Renamed from "Enable dependency resolution based on packages that were available on a target date" to "Expose flag to enable dependency resolution based on packages that were available on a target date" by @namurphy on 2024-02-29 17:13_

---

_Comment by @namurphy on 2024-02-29 17:20_

Awesome!  I love it when the time to issue resolution is negative!  (Next time, I'll take a look back through the closed issues & PRs instead of just the open ones!)

I updated the title of this issue accordingly: to expose this option when it is stable enough to be part of the public API.

> @namurphy would you expect unsolvable requirement errors to pretend these packages do not exist at all or explain that it was unsolvable because newer packages were excluded?

I'd prefer the latter, since it would be useful information to know that there is a newer package available.  

Thank you again!



---

_Label `duplicate` removed by @zanieb on 2024-02-29 17:50_

---

_Label `question` removed by @zanieb on 2024-02-29 17:50_

---

_Label `enhancement` added by @zanieb on 2024-02-29 17:50_

---

_Comment by @konstin on 2024-03-01 11:01_

I'm +1 on exposing `--exclude-newer`, i initially intended it for those use cases.

---

_Assigned to @zanieb by @zanieb on 2024-03-01 15:49_

---

_Referenced in [python/typing_extensions#348](../../python/typing_extensions/pulls/348.md) on 2024-03-04 06:25_

---

_Comment by @hauntsaninja on 2024-03-04 06:25_

I'd love for this to be a documented feature! Currently attempting to use it in https://github.com/python/typing_extensions/pull/348

---

_Referenced in [astral-sh/uv#2166](../../astral-sh/uv/pulls/2166.md) on 2024-03-04 17:28_

---

_Closed by @zanieb on 2024-03-04 17:51_

---

_Referenced in [astral-sh/uv#5492](../../astral-sh/uv/issues/5492.md) on 2024-07-26 20:06_

---
