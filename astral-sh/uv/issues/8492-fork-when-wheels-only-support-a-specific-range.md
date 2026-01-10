---
number: 8492
title: Fork when wheels only support a specific range
type: issue
state: closed
author: cbrnr
labels:
  - enhancement
  - resolver
assignees: []
created_at: 2024-10-23T09:03:41Z
updated_at: 2024-12-13T20:33:47Z
url: https://github.com/astral-sh/uv/issues/8492
synced_at: 2026-01-10T01:24:29Z
---

# Fork when wheels only support a specific range

---

_Issue opened by @cbrnr on 2024-10-23 09:03_

EDIT: See https://github.com/astral-sh/uv/issues/8492#issuecomment-2431979599 for an explanation of this behavior

---

I don't know if this is intentional, but the following project cannot be installed on Python 3.13 if I want to avoid building source distributions:

1. `uv init example`
2. `cd example`
3. Use this `pyproject.toml`:
    ```
    [project]
    name = "example"
    version = "0.1.0"
    description = "Add your description here"
    readme = "README.md"
    requires-python = ">=3.9"
    dependencies = [
        "scipy >= 1.13.1",
    ]
    ```
4. `uv sync --python=3.12` works (same for `3.9`, `3.10`, and `3.11`)
5. `uv sync --python=3.13 --no-build-package=scipy` fails, even though it could (should?) use `scipy==1.14.1`, which is available as a binary wheel.

This surprised me because I expected that by specifying `scipy >= 1.13.1`, uv would automatically choose a newer version with a binary wheel for Python 3.13. Indeed, there are binary wheels for version 1.13.1 available for Python 3.9 through 3.12, but only version ≥ 1.14.0 has a binary wheel for Python 3.13 (though it no longer supports 3.9). However, it seems that uv is trying to maintain consistent package versions across _all_ supported Python versions.

---

_Comment by @konstin on 2024-10-23 10:47_

This is known problem with scientific packages unfortunately. Does splitting the scipy version on the python version as documented at the bottom of https://github.com/astral-sh/uv/blob/main/docs/reference/build_failures.md (this doc will ship with the next release) help?



---

_Label `question` added by @konstin on 2024-10-23 10:48_

---

_Comment by @cbrnr on 2024-10-23 11:03_

Unfortunately not:

```
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    "scipy >= 1.13.1; python_version <= '3.12'",
    "scipy >= 1.14.0; python_version >= '3.13'",
]
```

It tries to build NumPy 2.0.2 from source on Python 3.13, no idea why it doesn't just use >= 2.1:

```
❯ uv sync --python=3.13 --no-build-package=scipy --no-build-package=numpy
Using CPython 3.13.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 4 packages in 0.97ms
error: Distribution `numpy==2.0.2 @ registry+https://pypi.org/simple` can't be installed because it is marked as `--no-build` but has no binary distribution
```

Maybe because it also uses NumPy 2.0.2 on Python <= 3.12?

Besides, I think splitting like this feels a bit off, since the ranges actually overlap (unlike in the example).

---

_Comment by @konstin on 2024-10-23 11:15_

You need to also split numpy, probably as a constraint since it's not a direct dependency:

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
  "scipy >=1.13.1,<1.14.0; python_version < '3.13'",
  "scipy >=1.14.0; python_version >= '3.13'",
]

[tool.uv]
constraint-dependencies = [
  # Supports 3.8 to 3.10
  "numpy <=2.1.0,>=1.22.4; python_version < '3.13'",
  # Support 3.10 to 3.13
  "numpy >=2.1.0; python_version >= '3.13'",
]
```

Overlapping version ranges are fine in uv's model, but you can also make them exclusive as in the example above.

Fwiw it would be nice if we could make this split automatically for non-abi3 wheels.

---

_Comment by @cbrnr on 2024-10-23 11:18_

Uhm, can you explain the reasoning behind this resolution? Why does uv not pick scipy 1.14.1 for Python 3.13? I'm explicitly specifying `scipy >= 1.13.1`, so 1.14.1 is perfectly valid?

---

_Comment by @hoechenberger on 2024-10-23 11:23_

This feels very un-intuitive to me, too 🤯

---

_Comment by @konstin on 2024-10-23 12:27_

There's three parts of the uv resolver interacting here: requires-python, forking and wheel compatibility. uv will only select packages with a requires-python lowest bound at least as high as your project. For example, your project has `requires-python = ">=3.9"`, while scipy 1.14.1 has `Requires-Python: >=3.10` (https://inspector.pypi.io/project/scipy/1.14.1/packages/64/68/3bc0cfaf64ff507d82b1e5d5b64521df4c8bf7e22bc0b897827cbee9872c/scipy-1.14.1-cp310-cp310-macosx_10_13_x86_64.whl/scipy-1.14.1.dist-info/METADATA), so we can't select this version and must choose an older version. (If we would select that version, you would get an error trying to `uv sync` on Python 3.9)

uv also has the forking mechanism (https://docs.astral.sh/uv/reference/resolver-internals/#forking): Whenever we see that a dependency has different requirements for different markers in the same package, we fork and solve once for the one markers and once for the other markers (assuming the markers are disjoint, if they aren't we split some more until we have disjoint environments, there are some implementation details to it). Forking can tighten the requires-python range: If we know we're in an environment from a `python_version >= "3.12"` marker, we can pick a more recent version - such as scipy 1.14.1 - since the `python_version >= "3.9" and python_version < "3.12"` range is already handled by the other fork, which can select an older version.

uv is conservative with forking, we only fork on above condition. In a single fork, we have to enforce that there is only a single version per package per resolution (since we can only install a single version). uv also tries to reuse versions between forks when possible, to reduce the divergence between them (less different versions, less things that can go wrong/need to be tested). The latter is the reason that you often have to add an upper bound on the version for getting the right forking results.

Pure python projects don't have an upper bound on their python compatibility: Usually a package published now will work on future python versions. Even for most compiled projects with per-python version shared libraries, you can use them on a newer version by building the source dist. For these and reasons related to how our universal dependency resolution works, we're discarding upper bounds on requires-python (long explanation in https://github.com/astral-sh/uv/issues/4022).

For numpy, scipy and a number of related projects this assumption isn't true, they really only work for a specific Python range for each version, and you're also not supposed to build them from source (often failing to compile on newer Python versions). Here's the part where uv falls short and you need the manual intervention of specifying `python_version` markers: uv can't detect that we're trying a package that has compatibility and wheels only for a specific range, and we need to fork to satisfy the full Python version range from the lowest version your project requires to the latest version the dependency supports.

---

_Comment by @cbrnr on 2024-10-23 12:38_

Thanks for this detailed explanation! I do like how thoroughly uv has been designed, but given that the scientific ecosystem now pushes it beyond its boundaries, I feel like it makes it very hard to use uv in this context. In the simple example with just a single `scipy` dependency I have to manually specify constraints for `numpy`, but in a more realistic project there will probably be dozens of such constraints that I don't want to handle manually.

> Fwiw it would be nice if we could make this split automatically for non-abi3 wheels.

I'm not sure if I understand what you're saying, but do you think that uv should do this automatically so that my original example "just works"?

---

_Comment by @cbrnr on 2024-10-24 05:12_

Also, `pip` resolves these requirements differently: it installs `scipy==1.14.1` on `>=3.10` and `scipy==1.13.1` on `==3.9` (together with suitable binary wheels for `numpy`). This is the expected behavior at least for me, but probably also for the Python community.

---

_Comment by @hoechenberger on 2024-10-24 05:47_

> This is the expected behavior at least for me, but probably also for the Python community.

I would like to second this ☝️

---

_Referenced in [MIT-LCP/wfdb-python#511](../../MIT-LCP/wfdb-python/pulls/511.md) on 2024-10-24 19:51_

---

_Label `question` removed by @konstin on 2024-10-25 08:19_

---

_Label `enhancement` added by @konstin on 2024-10-25 08:19_

---

_Renamed from "Dependency resolution across Python versions" to "Fork when wheels only support a specific range" by @konstin on 2024-10-25 08:19_

---

_Comment by @konstin on 2024-10-25 08:25_

For numpy at least, `Requires-Python` doesn't tell us about the upper bounds (https://inspector.pypi.io/project/numpy/2.1.2/packages/1c/a2/40a76d357f168e9f9f06d6cc2c8e22dd5fb2bfbe63fe2c433057258c145a/numpy-2.1.2-cp310-cp310-macosx_10_9_x86_64.whl/numpy-2.1.2.dist-info/METADATA#line.999), so we would need to find a strategy of forking dependent on the wheel on PyPI, e.g.:

Pick a numpy version, analyse the range of https://pypi.org/project/numpy/#files, and if we see consistent range of compiled `cp3x` tags while there exists a newer version that covers a wider range, fork and so we can pick a newer numpy on newer Python versions. 

---

_Referenced in [astral-sh/uv#7190](../../astral-sh/uv/issues/7190.md) on 2024-10-29 17:02_

---

_Comment by @konstin on 2024-10-29 17:04_

Another possible solution would be https://github.com/astral-sh/uv/issues/7190

---

_Comment by @henryiii on 2024-10-29 17:07_

You should never add an upper bounds to Requires-Python. Most packages don't know it until after they are released, and even for ones that know they won't support the next version (like numpy), the existing handing of this field is wrong and things like pip and poetry break if you cap. Looking at the binary wheels available to guide version ranges would be really helpful, though, I think, especially if #7190 wasn't the default. You could also check to see if classifiers are present, and only consider a classifier disappearing as you look at older packages to be the starting point of support for that Python version (for packages with universal wheels). Many packages don't have classifiers, but for the ones that do, having a classifiers is a sign of solid support. (That's a lot weirder, though, I don't think a single tool today checks classifiers during a solve).

https://discuss.python.org/t/requires-python-upper-limits/12663 was a discussion about what to do with upper caps.

---

_Comment by @cbrnr on 2024-10-29 18:09_

Yes, I think #7190 would be a great default. In my example at the top, I'm not setting an upper bound at all.

---

_Referenced in [astral-sh/uv#8686](../../astral-sh/uv/pulls/8686.md) on 2024-10-29 21:48_

---

_Referenced in [google/magika#591](../../google/magika/pulls/591.md) on 2024-10-30 02:55_

---

_Referenced in [astral-sh/uv#8978](../../astral-sh/uv/issues/8978.md) on 2024-11-10 10:42_

---

_Referenced in [astral-sh/uv#9211](../../astral-sh/uv/issues/9211.md) on 2024-11-18 22:23_

---

_Label `resolver` added by @konstin on 2024-11-18 22:23_

---

_Comment by @hoechenberger on 2024-11-20 10:08_

Hello @konstin  and @charliermarsh,

I’m not entirely sure whether to post here or in #7190, but I wanted to take a moment to share some thoughts. The challenge @cbrnr has described is a significant blocker for `uv` adoption in both my company and several open-source projects I’m involved with.

In our use cases, we typically need to support multiple Python versions while ensuring that the latest possible versions of all packages are installed for each Python version. This is critical for us across local development, CI testing, and deployment workflows.

Unfortunately, the current workaround using `python_version` selectors to enforce forking isn’t feasible for us. With projects that have dozens of dependencies, adding what could amount to hundreds of additional (and repetitive) lines to `pyproject.toml` simply isn’t manageable.

As it stands, this issue is preventing our entire data science department—comprising hundreds of professionals—from adopting `uv`, despite the significant enthusiasm and interest we have for it.

I’d sincerely appreciate it if this issue could be re-prioritized, and I’m more than happy to assist with testing or provide further examples if that would be helpful.

Thank you so much for your hard work, time, and dedication to this project! Your efforts are truly appreciated, and we’re excited about the possibilities that `uv` offers.

Warm regards,
Richard

---

_Comment by @ryanhiebert on 2024-11-20 10:18_

@hoechenberger I wonder if you can clarify whether your need for the latest version possible on each Python is for libraries, applications, or both. A thought is that this non-forking approach currently taken is better for applications because it reduces the variations in a lockfile, and I'm curious what other people's needs are.

I'd prefer the forking behavior in applications as well, but I'm not sure whether that's just my library-maker brain having myopia because it's not getting what it wants right now.

---

_Comment by @cbrnr on 2024-11-20 10:22_

In my opinion, packages should be resolved to their highest possible versions for each Python version separately. This is independent whether the project is a library or an app (the distinction is sometimes tricky anyway). If you develop an app, I think you should pin the Python version and all dependencies to fixed versions anyway, so this problem is not relevant. In addition, Python's official package manager `pip` has always done this, and now `uv` is doing something very different.

---

_Comment by @hoechenberger on 2024-11-20 10:22_

@ryanhiebert In the beginning I was only thinking about libraries here, but I'm quite certain some colleagues of mine would want this behavior for their apps, too. I can definitely see some good arguments for forking when resolving app dependencies, but of course also plenty of arguments against it. I believe for libraries, the case is more clear-cut.

Edit: FWIW I'd argue that for an app the Python version should be fixed anyway, so the question of forking doesn't really come up.

---

_Comment by @hoechenberger on 2024-11-20 10:25_

@cbrnr 
> In addition, Python's official package manager `pip` has always done this, and now `uv` is doing something very different.

I believe this is actually an under-appreciated comment. Regardless of whether forking is the "good" or "worse" thing to do, I think we can argue that users and devs have been "trained" over the years to expect this "pip resolution strategy". (I know, `pip` doesn't support universal lock files – but I'd suggest that this is beside the point here!)

---

_Comment by @ryanhiebert on 2024-11-20 11:11_

> FWIW I'd argue that for an app the Python version should be fixed anyway, so the question of forking doesn't really come up.

I think that's worth a lot, and what I'd generally recommend. And if that's a generally good rule to follow for applications, then this exceptional behavior really only affects applications that need to simultaneously support multiple python versions. I suspect that's pretty rare; if I were distributing an application it would almost certainly be in a docker image, so the Python version would be specified as well.

---

_Comment by @notatallshaw on 2024-11-20 14:55_

> In my opinion, packages should be resolved to their highest possible versions for each Python version separately. This is independent whether the project is a library or an app (the distinction is sometimes tricky anyway). 

This is _very_ use case specific, for all my current uses cases I would rather have the same version of the library where possible, as when I'm testing my application across Python versions I want to minimize the amount of differences there are when investigating issues.

> In addition, Python's official package manager `pip` has always done this, and now `uv` is doing something very different.

Pip doesn't do any kind of universal resolution and is a package installer, not a package manager, this is comparing apples to oranges. It would be much more apt to compare against Poetry.

That said, if all you want it a matrix of resolutions, uv does offer a pip compile API, and you can do,  `uv pip compile --python 3.10 --python-platform windows ...`, `uv pip compile --python 3.11 --python-platform linux ...`, etc.

FYI I'm not arguing this use cases shouldn't be supported, just it reads as universal declarations to things which are absolutely not universal.

---

_Comment by @ryanhiebert on 2024-11-20 15:09_

> FYI I'm not arguing this use cases shouldn't be supported, just it reads as universal declarations to things which are absolutely not universal.

@notatallshaw I really appreciate you chiming in! For libraries, it seems like the forking behavior would be the optimal default. For applications, I'm of two minds about it, and wish consistency was a good option, but I think for those moments where you really need your application to straddle Pythons it likely makes sense to default to avoiding forking.

---

_Comment by @cbrnr on 2024-11-20 15:23_

> This is _very_ use case specific, for all my current uses cases I would rather have the same version of the library where possible, as when I'm testing my application across Python versions I want to minimize the amount of differences there are when investigating issues.

This is also *very* use case specific.

> Pip doesn't do any kind of universal resolution and is a package installer, not a package manager, this is comparing apples to oranges. It would be much more apt to compare against Poetry.

Pip also resolves dependencies, but in a different way. The only important difference here is that pip resolves per Python version, whereas uv resolves across all Python versions.

The point is that we think there should be a flag to switch between these two different resolution modes, that's all. No one is saying that the current mode should be removed.

> That said, if all you want it a matrix of resolutions, uv does offer a pip compile API, and you can do, `uv pip compile --python 3.10 --python-platform windows ...`, `uv pip compile --python 3.11 --python-platform linux ...`, etc.

This would be a lot of manual work, which is exactly what we would like to avoid.

> FYI I'm not arguing this use cases shouldn't be supported, just it reads as universal declarations to things which are absolutely not universal.

I never said that it was universal, but pip is still the official standard. It interprets requirements defined in `pyproject.toml` a certain way, and `uv` deviates from this standard.

---

_Comment by @hoechenberger on 2024-11-20 15:29_

@cbrnr 
> The point is that we think there should be a flag to switch between these two different resolution modes, that's all. No one is saying that the current mode should be removed.

I would like to second that.

Like I said above, I don't see a need to argue about which approach is "better":

@hoechenberger 
> Regardless of whether forking is the "good" or "worse" thing to do, I think we can argue that users and devs have been "trained" over the years to expect this "pip resolution strategy".

We should just try to accommodate those users (and devs!) too, that's all.

@cbrnr 
> > That said, if all you want it a matrix of resolutions, uv does offer a pip compile API, and you can do, `uv pip compile --python 3.10 --python-platform windows ...`, `uv pip compile --python 3.11 --python-platform linux ...`, etc.
> 
> This would be a lot of manual work, which is exactly what we would like to avoid.

Agreed. If that's the proposed way forward, `uv` adoption is sadly off the table for me and my colleagues.

I'm still very positive that we'll be able to see a solution that works for all! 🙂  

---

_Comment by @notatallshaw on 2024-11-20 15:43_

> This is also _very_ use case specific.

Isn't this also Poetry's default behavior? And therefore the most common behavior among Python universal resolvers.

I agree it's use case specific, but currently it seems to be the most common case for universal resolvers.

> Pip also resolves dependencies, but in a different way. The only important difference here is that pip resolves per Python version, whereas uv resolves across all Python versions.

As a maintainer of resolvelib, the resolver for pip, and a regular contributor to pip's resolver code, I would say that isn't at all an accurate description of what pip does.

Pip only collects packages and requirements for a specific environment, that is, for all the environment markers defined by PEP 496, the resolver itself has no concept of a Python version.

>  The point is that we think there should be a flag to switch between these two different resolution modes, that's all. No one is saying that the current mode should be removed.

I strongly support this, but comparing universal resolution which is aware of environment markers to pip's resolution is fundamentally flawed way of looking at this.

> I never said that it was universal, but pip is still the official standard. It interprets requirements defined in `pyproject.toml` a certain way, and `uv` deviates from this standard.

Pip is *not* an official standard. It is bundled into CPython to allow users to bootstrap their environment. Several core developers have expressed strong aversion to bundling a dedicated package manager.

Further, there are no standards for resolution, only standards for specifier on a set of versions. The standards documents are very careful to avoid saying anything on how resolution should work.

If you want a pip API from uv then use `uv pip`, it's very good, I rely on it heavily and am quick to report if there's any unknown deviation with pip. 

---

_Comment by @ryanhiebert on 2024-11-20 15:47_

I think that @notatallshaw has brought up valid points. I do seem to recall having similar trouble with using poetry for libraries. pip itself is an installer only, not a locker, so I do think that there is a very relevant distinction between their behaviors.

---

_Comment by @cbrnr on 2024-11-20 15:55_

Yes, I agree that these are extremely valid points. I really do not want to discuss the technical details here, as I think they are not relevant. I'm simply saying that uv cannot be used for things where pip just works (see my example at the very top).

So let me focus on the most important point where everyone seems to agree:

> The point is that we think there should be a flag to switch between these two different resolution modes, that's all. No one is saying that the current mode should be removed.

Agreed?

---

_Comment by @hoechenberger on 2024-11-20 15:57_

I'm afraid this discussion is derailing a bit. The goal of uv is to replace existing tools like venv, poetry, pip, ...

It's no use to fight over what behavior is better or worse.

Ultimate it's a question of who's the targeted user group.

And all @cbrnr and I are saying: without the ability to change the resolution strategy, we and large parts of our professional networks will fall **outside** of this group. Which is totally alright, no hard feelings. We just wanted to make the uv devs aware of this.

Have a nice day, everyone! 🤗 

---

_Comment by @notatallshaw on 2024-11-20 16:11_

Yes, agreed it would be great for uv to support this use case, and agreed it's a real need for you. 

That said, one nitpick:

> I'm simply saying that uv cannot be used for things where pip just works

You can definitely use `uv pip` to replace `pip` for everything you've said in this thread. If there's a missing use case there you should raise a separate issue on that.

---

_Comment by @cbrnr on 2024-11-20 16:13_

> You can definitely use `uv pip` to replace `pip` for everything you've said in this thread.

Then I would lose all the nice things that `uv` brings to the table like being able to just `uv run` things 😢, and it would "just" be a faster version of pip. I hope that's not what `uv` is intended to do.

---

_Comment by @zanieb on 2024-11-20 17:12_

Regarding the "specificity" of use-cases, uv tries to use consistent versions whenever feasible — this is a fundamental design principle of uv's user experience. I think it's fine to allow you to opt-in to different behavior, but changing the default behavior seems inconsistent.

---

_Comment by @henryiii on 2024-11-20 17:14_

> This is very use case specific, for all my current uses cases I would rather have the same version of the library where possible, as when I'm testing my application across Python versions I want to minimize the amount of differences there are when investigating issues.

FYI, this will often mean you'll be using an old version of a library on a version of Python that wasn't released when it was released. That's the issue this is referring to; NumPy (and most other major scientific libraries) support the last three releases, instead of the last 5 or 6 which is more common (due to EoL often being followed to some degree). And at least NumPy specifically never works on a newer version of Python. If any library you depend on has a restricted Python range (libraries should be allowed to drop old Pythons!), that means you will very often end up with a resolution that would be impossible for pip, an old library on a new Python. It would be nice (and I think possible) if this could be detected and auto-forked if it happens; you never want to resolve a version of a library that is older than the Python you are using if there's a newer release available! This is true even for applications.

I believe this is a case where "consistent versions" is unfeasible. Maybe three modes: `nofork`, `auto`, and `fork`, where auto will fork only if the python minimum required version causes back solving and therefore a non-ideal version for the latest supported Pythons.

---

_Comment by @henryiii on 2024-11-20 17:23_

IMO this _application_ should work out of the box without configuration on Python 3.13:

```toml
[project]
name = "numexample"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
    "numpy",
]
```

Currently it takes almost two minutes to set up or crashes building NumPy, and likely has issues since it's using a NumPy (2.0.2) that doesn't support 3.13. I think the ideal case would be for it to fork on Python 3.9, so 3.9 has numpy==2.0.2, and 3.10-3.13 has numpy==2.1.2 (which takes 131ms to install).

---

_Comment by @cbrnr on 2024-11-20 17:25_

@henryiii and it gets even worse if you look at my initial example at the very top, because you'd need to manually fork not only the direct dependency, but also all indirect ones.

---

_Comment by @notatallshaw on 2024-11-20 17:26_

> that means you will very often end up with a resolution that would be impossible for pip, an old library on a new Python.

I'm not quite groking what you mean here. If you come up with a resolution using pip on an old version of Python then it can also likely be impossible to use that resolution on a new version of Python. 

> And at least NumPy specifically never works on a newer version of Python.

As an aside, I'd like to understand this a little better. If you don't want to support old versions of numpy for your project, why are you resolving for versions of Python no longer supported by numpy?

---

_Comment by @cbrnr on 2024-11-20 17:29_

> As an aside, I'd like to understand this a little better. If you don't want to support old versions of numpy for your project, why are you resolving for versions of Python no longer supported by numpy?

It's the opposite. We want to support an older version, because we'd like to keep supporting Python 3.9 for example. But with uv, we get the old version for *all* Python versions, including a source build for 3.13.

---

_Comment by @charliermarsh on 2024-11-20 17:30_

I'd like to curtail further discussion about the motivation for this feature. I understand the motivation and why it's important. I'm not really interested in changing the defaults right now. But I am interested in supporting it. It is fairly challenging ~since (e.g.) the NumPy package metadata does not give any indication that (e.g.) NumPy 2.0.2 "doesn't support" Python 3.13, so it requires a variety of new and highly nuanced logic~. (Edit: I'll just leave it at "fairly challenging" since it's not worth debating why and what needs to change -- we understand why and what needs to change.)

---

_Referenced in [astral-sh/uv#9412](../../astral-sh/uv/issues/9412.md) on 2024-11-25 10:05_

---

_Referenced in [astral-sh/uv#9827](../../astral-sh/uv/pulls/9827.md) on 2024-12-11 23:15_

---

_Comment by @charliermarsh on 2024-12-11 23:19_

Ok, I believe I have this working at https://github.com/astral-sh/uv/pull/9827.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-11 23:19_

---

_Closed by @charliermarsh on 2024-12-13 20:33_

---

_Closed by @charliermarsh on 2024-12-13 20:33_

---

_Referenced in [astral-sh/uv#12060](../../astral-sh/uv/issues/12060.md) on 2025-09-18 10:09_

---
