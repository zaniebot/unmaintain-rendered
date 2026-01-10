---
number: 10474
title: Allow to add additional version constraints version based on dependency version
type: issue
state: open
author: Czaki
labels: []
assignees: []
created_at: 2025-01-10T17:46:31Z
updated_at: 2025-01-10T21:08:44Z
url: https://github.com/astral-sh/uv/issues/10474
synced_at: 2026-01-10T01:24:54Z
---

# Allow to add additional version constraints version based on dependency version

---

_Issue opened by @Czaki on 2025-01-10 17:46_

Sometimes there is a situation where one of the package dependency does not specify properly upper version constraints for its dependency. So it may happen, that it resolves to not working environment, 

The obvious option is to set upper constraints on this transient dependency, but then you need to remember to trace the dependency releases to check if a new version fixes the compatibility with its dependencies. 

It will be nice to add (even fully uv specific) option to conditionally specify constraints. 

My case was `nilearn==0.10.1` that was incompatible with NumPy 1.24+ So the `numpy<1.24` lands in dependencies and no one noticed multiple releases of nilearn since this change. 

It will be much nicer to be able to define somehow `if nilearn<=0.10.1 then numpy<1.24`. Then, if a new release of such dependency will still has some problems, the CI tests will show such problem. 

---

_Comment by @notatallshaw on 2025-01-10 18:43_

It's not as powerful as what you're asking for, but you can override the dependency metadata for each version of a package: https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata

---

_Comment by @Czaki on 2025-01-10 20:54_

@notatallshaw I do not found this part of the docs earlier. But I still not sure if I correctly understand. Does these constraints only apply to a given version or enforce this version? 

---

_Comment by @notatallshaw on 2025-01-10 21:07_

> But I still not sure if I correctly understand. Does these constraints only apply to a given version or enforce this version?

My understanding is only apply to a given version, and they override all the requirements (not just add constraints).

It's not what you're asking for, but it's related and good enough for some use cases.

---
