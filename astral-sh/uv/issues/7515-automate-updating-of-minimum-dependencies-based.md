```yaml
number: 7515
title: Automate updating of minimum dependencies based on time since initial minor release 
type: issue
state: open
author: namurphy
labels: []
assignees: []
created_at: 2024-09-18T19:01:33Z
updated_at: 2025-10-17T00:48:32Z
url: https://github.com/astral-sh/uv/issues/7515
synced_at: 2026-01-12T15:59:14Z
```

# Automate updating of minimum dependencies based on time since initial minor release 

---

_@namurphy_

## Feature request

It would be quite helpful if uv had a command for automagically updating the minimum supported versions of dependencies specified in `pyproject.toml` after a specified time frame. ⏱️  

For example, if the minimum allowed version of NumPy is $≥24$ months old, and there was a release of NumPy that is ≥18 months old, then uv would update the minimum allowed version of NumPy in `pyproject.toml` to be the oldest minor release that $≤24$ months old. There should also be a check to make sure that the lowest-direct resolution strategy works with the updated requirements.

It would be helpful to make this configurable for different time frames, with options to run this only for a single dependency or exclude certain dependencies.

These updates should respect excluded versions (i.e., `numpy != 1.25`) and not change upper limits.

## Backstory

[Scientific Python Enhancement Proposal (SPEC) 0 ](https://scientific-python.org/specs/spec-0000/) proposes a community policy on minimum supported dependencies:

> 1. Support for Python versions be dropped 3 years after their initial release.
> 2. Support for core package dependencies be dropped 2 years after their initial  release.

My understanding is that SPEC 0 has been adopted by core packages like NumPy, SciPy, and matplotlib; as well as by numerous other packages. This policy is for minor releases (e.g., 1.24) rather than patch releases (e.g., 1.24.4).  

(This issue doesn't request automatic updates to `python-requires` in `pyproject.toml` because  often more significant changes in the CI infrastructure and documentation are necessary.)

## Motivation

Adherence to SPEC 0's policy is somewhat tedious.  Every few months, I go into `pyproject.toml`, look up the PyPI release dates of all of the dependencies, verify that there has been a more recent release, and then update the minimum requirement as necessary.  While this happens to be one of my favorite strategies for procrastination-driven development (PDD) 😅, this task could be automated.

Adding this capability could encourage packages within the scientific pythoniverse to adopt uv, and for a wider variety of packages to adopt practices aligned with SPEC 0.

## Additional context

SPEC 0 supersedes [NumPy Enhancement Proposal (NEP) 29](https://numpy.org/neps/nep-0029-deprecation_policy.html).

Thank you times ten billion! 🙏🏻 I'm curious how many others would make use of this capability.


---

_Comment by @notatallshaw on 2024-09-18 19:16_

> For example, if the minimum allowed version of NumPy is ≥ 24 months old, and there was a release of NumPy that is ≥18 months old, then uv would update the minimum allowed version of NumPy in `pyproject.toml` to be the oldest minor release that ≤ 24 months old. There should also be a check to make sure that the lowest-direct resolution strategy works with the updated requirements.

FYI, this would require the index being queries to support Simple Repository API version 1.1 or later i.e. [PEP 700](https://peps.python.org/pep-0700/).

For indexes which are on  Simple Repository API version 1.0 uv would have no way of getting this data. I mention this because PyTorch appears to still be on 1.0 and I'm sure other popular non-PyPI indexes are on 1.0, so there would need to be a clear warning from uv when using an index that doesn't provide this information.

---

_Comment by @namurphy on 2025-10-17 00:48_

Coming back to this after a while, #6794 and #8585 are potentially related issues.

I've since learned about [`nep29`](https://github.com/hmaarrfk/nep29) (named after [NEP 29](https://numpy.org/neps/nep-0029-deprecation_policy.html)) which prints out the minor versions of a package that have been released in the past period of time (defaulting to 24 months).  An example usage is:

```bash
$ uvx --with setuptools nep29 plasmapy --n_months 24
|  version  |    date    |
| :-------: | :--------: |
|  2025.8.0 | 2025-08-07 |
| 2024.10.0 | 2024-10-30 |
|  2024.7.0 | 2024-07-21 |
|  2024.5.0 | 2024-05-08 |
|  2024.2.0 | 2024-02-06 |
| 2023.10.0 | 2023-10-20 |
```

I've played around with a Nox session to automate SPEC 0 updates in https://github.com/PlasmaPy/PlasmaPy/pull/3048 (stalled).

---
