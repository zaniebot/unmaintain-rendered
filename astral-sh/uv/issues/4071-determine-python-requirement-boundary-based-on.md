---
number: 4071
title: "Determine Python requirement boundary based on supported `Requires-Python` range"
type: issue
state: open
author: charliermarsh
labels: []
assignees: []
created_at: 2024-06-05T20:28:58Z
updated_at: 2024-08-20T18:23:36Z
url: https://github.com/astral-sh/uv/issues/4071
synced_at: 2026-01-10T01:23:34Z
---

# Determine Python requirement boundary based on supported `Requires-Python` range

---

_Issue opened by @charliermarsh on 2024-06-05 20:28_

Rather than writing the user's `Requires-Python` to the lockfile, we _could_ instead write the minimum bounding `Requires-Python` of all the requirements. Or we could even omit it and use the `Requires-Python` of each package.


---

_Label `preview` added by @charliermarsh on 2024-06-05 20:29_

---

_Referenced in [astral-sh/uv#4022](../../astral-sh/uv/issues/4022.md) on 2024-06-05 21:46_

---

_Comment by @charliermarsh on 2024-06-05 23:22_

I'm now somewhat uncertain that this makes sense, because we include the project itself in the lockfile, and so the range will be _at most_ the range of the project itself.

---

_Comment by @zanieb on 2024-06-06 00:23_

I think that's okay; the goal is to write the narrowest bound, right? That it's "at most the range of the project" makes a ton of sense i.e. you can constrain project's Python requirement more narrowly than its dependencies.

---

_Comment by @charliermarsh on 2024-06-06 00:24_

I think it will always end up being exactly the bound of the project, right?

---

_Comment by @charliermarsh on 2024-06-06 00:25_

Because we require that all dependencies have bounds that are >= the bound of the project. So the project is _already_ the narrowest bound, by definition.

---

_Comment by @zanieb on 2024-06-06 00:36_

Ah I was thinking we'd drop the requirement that the project's python bound is narrower than all its dependencies'.

It feels incorrect to require that a project's Python requirement is narrower than all of its dependencies _unless_ they're all pinned (i.e. in a lockfile) because otherwise you're overly constraining against future versions of the dependencies.

Imagine if I'm developing a library and a new version of a dependency gets published with a relaxed Python requirement. If that new dependency version is within my published constraints, the supported Python range should expand but uv will have forced me to narrow my published supported Python range to the previous value. 




---

_Comment by @charliermarsh on 2024-06-06 00:51_

Sorry, can you give a concrete example?

---

_Comment by @zanieb on 2024-06-06 01:40_

[Henry's comment](https://github.com/astral-sh/uv/issues/4022#issuecomment-2150979188) gives a concrete example, I'll modify it here.

Say your project has one dependency `foo>=0.2.0` with one published version `0.2.0` which requires `python>=3.9,<3.13`. Poetry, and uv today, will force you to change your project's Python requirement to `python>=3.9,<3.13` or narrower. However, if your project is being published as a Python library, that's not ideal. For example, if `foo` releases a new version `0.3.0` which requires `python>=3.9` (drops the upper bound) or `python>=3.9,<3.14` (adds explicit support for a new version) users of your library _should_ be able to use `foo==0.3.0` and `python==3.13` but they cannot until you publish a new version of your project. In this scenario, a solver _forced a constraint_ to be added to the published package's Python requirement to facilitate creation of a lockfile. Instead, the lockfile should contain the narrower constraint and the package metadata should be allowed to be broader. When generating a lockfile we have _pinned_ versions for all dependencies and we know all of the Python requirements will be constant so the narrowest intersection is fixed. However, when publishing a project without pinned dependency versions, we don't know if the Python requirements will be relaxed in future releases, changing the narrowest intersection of the bounds.


---

_Comment by @charliermarsh on 2024-06-06 01:53_

Ahh, I don't think that's solved by the thing in this issue, though. I thought that was solved by other things:

1. Avoiding requiring users to set an upper bound in the first place.
2. Separating the `Requires-Python` of the package from the supported Python range for installing it (from a lockfile). That is, the Python requirements for installing the project from the lockfile should not be coupled to the supported Python versions of the package itself.

When locking, we _do_ need some user-defined range of "Python versions you want to lock for"; and then when solving, we _do_ need to ensure that all the packages we include are supported on all of those versions. I think the critical thing, though, is that the "Python versions you want to lock for" should not be coupled to the `Requires-Python` of the project itself -- they should be different concepts, right? Like, the versions you locked against should not impact the versions that consumers of your package can use? But today, that _is_ the case in Poetry.


---

_Comment by @charliermarsh on 2024-06-06 01:54_

Like, maybe the code in the project itself only requires Python 3.7. Great. So you put `Requires-Python = ">= 3.7"` in your `pyproject.toml`.

But when locking, that might too permissive. You might want to lock against a narrower range, so that you're not required to use versions that support Python 3.7. This suggests that you want some other control that _isn't_ `Requires-Python`.


---

_Comment by @zanieb on 2024-06-06 02:44_

> "Python versions you want to lock for" should not be coupled to the Requires-Python of the project itself -- they should be different concepts, right?

Yes this sounds good to me. I think we're on the same page. I don't think we should suggest constraining `Requires-Python` to match your dependencies. Is there a way we can start with `Requires-Python` then infer the narrower constraint required for resolution from the dependencies and write that to the lock file?



---

_Comment by @charliermarsh on 2024-06-06 02:51_

Are you suggesting "enforce `Requires-Python` while locking (as we do now), but then write the less-strict constraint based on all your dependencies"? Or "ignore `Requires-Python` while locking, and then compute the constraint from the dependencies"?

---

_Comment by @zanieb on 2024-06-06 02:57_

I'm thinking about that error case we raise saying to narrow your `Requires-Python`. Let's say we have some alternative field `Lock-Python` that has the same semantics but is null by default. Is there a way we can do the narrowing for the user (and write it to the lock file) instead of throwing an error saying you need to populate `Lock-Python`?

I think this is different than both of the options you presented. Though, I am confused by:

> enforce `Requires-Python` while locking (as we do now), but then write the **less-strict constraint** based on all your dependencies

Isn't the failure case that the constraint of your dependencies is _more_ strict than `Requires-Python`? I don't think I'm suggesting ignoring `Requires-Python` just allowing it to be "too broad". If `Lock-Python` is set, then we'd just make sure it's narrower than `Requires-Python` when locking.


---

_Comment by @charliermarsh on 2024-06-06 03:00_

> Isn't the failure case that the constraint of your dependencies is more strict than Requires-Python?

Yeah. In that case, I was suggesting that you have a successful resolve, and then we try to figure out how broad the `Requires-Python` of _your dependencies_ could be. If we succeeded, then it must be _at least_ as broad as your own `Requires-Python`, maybe broader. IDK what we would do with that though.


---

_Comment by @charliermarsh on 2024-06-06 03:01_

What you've described above makes sense, though. If we fail, tell the user, "Hey, we could only create a universal environment for Python 3.10 and later, so set that as your lock constraint." And critically "your lock constraint" is not `Requires-Python` but something else.

---

_Referenced in [astral-sh/uv#4087](../../astral-sh/uv/issues/4087.md) on 2024-06-06 04:06_

---

_Comment by @konstin on 2024-06-06 07:50_

> But when locking, that might too permissive. You might want to lock against a narrower range, so that you're not required to use versions that support Python 3.7. This suggests that you want some other control that isn't Requires-Python.

If you'd lock for let's say >=3.8, it means ou can't run test for python 3.7 with the lockfile. The lockfile would lose a part of it's universally by breaking down for a section of possible environments.

If we want to achieve higher versions with more recent python versions, we'd need to apply fork-and-merge when encounter a requires-python bound splitting our supported range. When we lock for >=3.7 and encounter a python >=3.9 numpy, we resolve once for >=3.9 and once for <3.9,>=3.7, locking two different numpy versions depending on the python you install on. These would be two resolver modes: A python-universal mode that enforces >=3.7 support for all your dependencies, and a python-specific mode that gives you the highest version per python version.

While problems with using features from newer versions that will not be supported on older versions could be irritating when working on more complex projects, i believe the main problem is performance, we potentially get a lot of splits.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---
