```yaml
number: 8686
title: "Introduce a `--multi-version` preference mode"
type: pull_request
state: closed
author: charliermarsh
labels:
  - enhancement
  - configuration
assignees: []
base: main
head: charlie/nui
created_at: 2024-10-29T20:56:15Z
updated_at: 2024-12-13T14:16:23Z
url: https://github.com/astral-sh/uv/pull/8686
synced_at: 2026-01-10T11:59:59Z
```

# Introduce a `--multi-version` preference mode

---

_Pull request opened by @charliermarsh on 2024-10-29 20:56_

## Summary

By default, we try to minimize the number of versions selected for each package when forking. This PR adds `--multi-version latest` which instead optimizes for choosing the _latest_ version in each fork, and automatically forks over the set of supported Python minor versions.

Closes https://github.com/astral-sh/uv/issues/7190.


---

_Label `enhancement` added by @charliermarsh on 2024-10-29 20:56_

---

_Label `configuration` added by @charliermarsh on 2024-10-29 20:56_

---

_Marked ready for review by @charliermarsh on 2024-10-29 21:16_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-29 21:16_

---

_Review requested from @konstin by @charliermarsh on 2024-10-29 21:16_

---

_Comment by @charliermarsh on 2024-10-29 21:17_

To make this work "as intended", I guess we'd have to fork when we notice that a package has a `requires-python` on it that's higher than the current minimum?

---

_@konstin approved on 2024-10-29 21:23_

I like it!

We can add a reference to this in https://docs.astral.sh/uv/reference/build_failures/#fixes-and-workarounds, since this fixes one of the numpy bullet points better.

---

_Comment by @charliermarsh on 2024-10-29 21:26_

Do you think we should do this "automatically", without the need to repeat the dependency declarations? I don't know how we could, it seems extremely hard, since we'd then need to fork _after_ we get distribution metadata.

---

_Comment by @charliermarsh on 2024-10-29 21:27_

@konstin -- Perhaps, in addition to this... when `--multi-version latest` is selected, should we "automatically" fork for each Python minor?

---

_Comment by @konstin on 2024-10-29 21:48_

> Do you think we should do this "automatically", without the need to repeat the dependency declarations? I don't know how we could, it seems extremely hard, since we'd then need to fork after we get distribution metadata.

I think we could, with that exact caveat you mentioned that we first have to look at the wheels and then fork afterwards which changes some assumptions we currently have (fwiw requires-python doesn't help us here https://github.com/astral-sh/uv/issues/8492#issuecomment-2437194019).

> Perhaps, in addition to this... when --multi-version latest is selected, should we "automatically" fork for each Python minor?

Good question.

As an example, each numpy release seems to support 4 Python minor versions, so when trying to make as few forks as possible and seeing a wider range than 4 Python minor versions from oldest to latest supported, we could do with two fork. E.g. with `requires-python = ">=3.7"`, we could pick a version that support 3.7 - 3.10 in one fork and one that supports 3.9 - 3.13 in another fork (3.11 to 3.15 would be ideal, but there's no version that supports a higher than 3.13 yet).

Personally, I'm a fan of fewer version (less error surface, less to test) and fewer forks (less versions and better performance), but that's not universal. There can be users that explicitly want that behavior, since they test any Python minor anyway and want to get as much dependency-version coverage as possible. If their users use `[uv] pip install <package>` a lot, this may be more desirable than a small lockfile. CC @henryiii @cbrnr @hoechenberger you have more insight into scientific package authoring than me.

---

_Comment by @henryiii on 2024-10-29 22:21_

This is a bit of conflict between projects and libraries. For a project, I could see a universal solve being preferred (might not truly be best, as fixes for newer Python versions - or just in general - are often introduced in newer versions!  But someone _might_ want to find a single set of compatible versions). But for a library, they will almost universally _want_ to test on the latest versions of packages, as anyone _using_ the package will get the latest versions when they install. If library X updates and breaks you, you'd not know because you'd have an older version of X that supported an old Python, while your users on newer versions of Python would get broken.

NumPy and the core Scientific Python libraries support Python for 36 months, following [SPEC 0](https://scientific-python.org/specs/spec-0000/) (used to be 42 months when it was NEP 29). They can do that because they are fairly stable. Most smaller projects support a wider range, since 3.7-3.9 is heavily used in practice, and while old versions of NumPy and such work fine on older Pythons, if you are writing a new or changing library, there is no old stable version to rely on.

Couldn't you either solve once per Python version then merge identical solutions, or only fork on the highest minimum bound found? The last 3-5 Python versions will usually have a single resolution, then the older ones will have unique per-Python resolutions.

---

_Comment by @henryiii on 2024-10-29 22:25_

An important point is a forked resolution is never "broken" - it's just a little bigger, but a universal install can be. At best, you might be protected against a new breaking change from a library (but only due to your minimum Python version!), but you'll never get old packages on new Pythons that don't work even after a fix has been released.

---

_Comment by @cbrnr on 2024-10-30 07:28_

In my opinion, this issue is not unique to the scientific package ecosystem or tied to whether you package an app or a library. I think the main question here is: how do we interpret requirements as specified in `pyproject.toml`?

Taking my example from #8492:

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

I have always interpreted this to mean "take the highest possible version of `scipy` available for any given Python version". For uv, this currently means "take the highest possible version of `scipy` available *across* all supported Python versions", which means that uv will use `1.13.1` for all Python versions >=3.9. This implies that for newer Python versions, where no wheels are available, uv will have to build the package from source.

As far as I know, pip has always used the first strategy, so I am pretty sure that most Python users will expect that to be the "correct" behavior. So even though I do see the value of having the exact same set of package versions across all Python versions, this is probably not what people expect (at least if they have been using pip, which is still the standard and official package manager in Python).

I'm not sure if I'm qualified to advise on how to implement this behavior or if it should be the default, but I like the idea of resolving everything separately per Python version (and then merging where possible). And if that was not already clear, I think this should be the default behavior simply because people will be expecting it.

---

_Comment by @hoechenberger on 2024-10-30 07:40_

I'd like to second everything @cbrnr has said. FWIW I'm pretty sure conda and mamba behave in the same way (always retrieve the newest possible version for any combination of Python and required packages).

---

_@RazerM reviewed on 2024-10-30 10:25_

---

_Review comment by @RazerM on `crates/uv-settings/src/settings.rs`:472 on 2024-10-30 10:25_

sorry for the drive by comment, but should this be `multi-version-mode = "latest"`? `settings.md` uses `resolution` in the examples too

---

_Comment by @charliermarsh on 2024-10-30 12:39_

I don't anticipate making this the default. `uv lock` and `pip install` (or `uv pip install`, which behaves the same way) are solving different problems. `pip install` is resolving for the single Python version and platform that you're using at time of execution. `uv lock` is resolving for all supported platforms and Python versions. In that context, you generally want to minimize the differences between environments to maximize consistency and reproducibility across users and developers (ideally you'd only have a single consistent environment, but that's often not possible). In that sense, the current behavior is the right default!


---

_@charliermarsh reviewed on 2024-10-30 12:48_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:472 on 2024-10-30 12:48_

Thank you yeah, this is wrong.

---

_Comment by @cbrnr on 2024-10-30 12:51_

I'm happy if we get a per-Python resolution with `uv lock` even if it won't be the default!

---

_Comment by @charliermarsh on 2024-10-30 12:51_

Yeah this PR sort of does it but I'm not happy with the result yet, still too much work for users :)

---

_Comment by @hoechenberger on 2024-10-30 13:27_

> I'm happy if we get a per-Python resolution with `uv lock` even if it won't be the default!

+1

---

_Comment by @henryiii on 2024-10-30 13:48_

> I don't anticipate making this the default. `uv lock`

The problem is that it's shared with `uv run`. I've really grown fond of using `uv run pytest` on every package, regardless of whether they have special uv configuration or not. By using dependency-groups and making sure there is a dev group, it just works, except for two problems: it makes a uv.lock file that ideally needs to be ignored (minor issue), and it resolves across the whole range, not just the Python I'm using, which has the problems above - and as noted, it's not unique to scientific packages, though it hits those a bit harder. But otherwise, it's almost what I've always wanted - a way to avoid fiddling with a virtual environment and activating it. Maybe if _locking_ itself was opt-in; it could be opt-out for projects (no build-system) and opt-in for packages (a build-system)?  People wanting locking could add `tool.uv.autolock = True/False`? The problem is that setting `requires-python` is fundamentally _different_ then setting the range you want to lock over. Often that's just "current-version" unless you are trying to distribute the lock file! Maybe `tool.uv.lock-current-only`, for example? Again, since there's no reason to set `python-requires` on a "project" except to specify the locking range, it would make sense to have the default depend on `[build-system]` being present. But just because you have `>=3.8` doesn't mean you want dependencies that supported 3.8 only. Libraries _must_ support a range of dependency versions anyway, while projects don't need to.

Also, again, finding a unique set of packages that "works" over the whole range doesn't help if you get an old version of a package that's actually broken on newer Python versions but you don't know it. It's quite common that packages ship fixes for newer Python versions! Personally, in the Python ecosystem at least, I believe it's best to assume newer versions of packages are better, most library authors try to have reasonable backward compatibility and deprecation periods/warnings, and mostly ship fixes and improvements.

---

_Comment by @cbrnr on 2024-10-30 14:00_

I agree with @henryiii, as I'm also using `uv run` instead of `uv pip`, but I was assuming that there would be a flag that can be set for `uv run`, `uv sync`, `uv lock`, and wherever else it might make sense. Would that not solve the issue (provided that all of this forking happens automatically without any additional user input, which is still required even after this PR is merged)?

---

_Comment by @charliermarsh on 2024-10-30 14:02_

Yeah the current iteration of this PR is such that (1) we'll solve for each Python minor automatically, no additional user input required; and (2) you can set this as an environment variable or in your `uv.toml` or whatever.

---

_Comment by @charliermarsh on 2024-10-30 14:03_

> The problem is that setting requires-python is fundamentally different then setting the range you want to lock over. Often that's just "current-version" unless you are trying to distribute the lock file!

Do you just want a way to specify the locking range separately from the `requires-python` range?


---

_Review requested from @konstin by @charliermarsh on 2024-10-30 14:06_

---

_@charliermarsh reviewed on 2024-10-30 14:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:486 on 2024-10-30 14:08_

@konstin -- Re-requesting review as I changed this setting to _also_ automatically solve for each Python minor.

---

_Comment by @henryiii on 2024-10-30 14:27_

I've always wanted that, and I've argued that tools like Poetry should support a way to specify the locking range separately. But we already have this in uv:

```toml
[tool.uv]
environments = ["python_version >= '3.10'"]
```

But I'm really hoping here is that there will be some sort of reasonable default that will be friendly toward libraries, at least in library mode. Most tools with locking solvers optimize really heavily on the project case, and have hidden issues for the very common case of working on libraries. I do a ton of teaching of python packaging, and it would be nice if the defaults were reasonable and I don't have to spend 10 or 20 minutes teaching them what a locking solver is, how to set better defaults, and why old buggy versions of dependencies are suddenly popping up.

Regardless of what the solution might be, it would be very nice if depending on a package like NumPy or Sphinx worked out of the box and didn't resolve old versions on newer pythons when working on libraries!

(Also, for anything that supports a Python range, I think you need to test at least at the lowest and highest versions of that range, I don't think you can get away with saying a single set of dependencies was found so it all will work.)

---

_Comment by @charliermarsh on 2024-10-30 14:31_

@henryiii -- I'm just trying to understand -- are you arguing that we should have defaults that are framed around library usage? Or you want straightforward _settings_ that work well for libraries?

---

_Comment by @henryiii on 2024-10-30 14:33_

I'd like both. :) Having a setting would be good, good defaults would be fantastic. :)

The fact you know the difference due to the presence of build-system gives me hope on the defaults.

---

_Comment by @konstin on 2024-10-31 15:17_

Sharing an alternative pitch of what we could do:

The goal is to use the latest version of each package for each python (minor) version.

Whether we would use a different version of a package depends on the requires-python of the packages. We can use this by forking each time we didn't select a package due to the requires python bound being to high, forking with the python version we didn't select.
Example:
We have requires python >=3.8 and a plain numpy requirement. We have to select 1.24.4 since this is the latest version matching our constraint. We know there's a new version with >=3.9 we ignored, so we create a fork for 3.9. In the >=3.9 fork, we select numpy 2.0.2, and create a fork for >=3.10. In the 3.10 fork, we select 2.1.2 (latest) and are done: We know covered all python versions, each newer python version would only select the same versions again. With this method, we let the actual requires python bound determine the forks.

Assuming that newer versions only increase the upper bound of python versions supported (by having built wheels for later versions), and us selecting the latest version, we get the most python version support possible. (For simplicity, i'm ignoring the no source dist cases here)

Implementation-wise, we would add a second fork point. Currently, we're only forking after retrieving and before adding dependencies, now we also fork in the version selection. Version selection is a hot path, so we need to be efficient in tracking whether we skipped a version due to requires python.

numpy requires python reference: https://gist.github.com/konstin/4d97de8458999f874912d628e88a0c49

---

_Comment by @charliermarsh on 2024-11-04 13:51_

I think what we have here is "fine" though it has the major disadvantage that we have to encode the maximum "known" Python version. Konsti's solution is definitely better, though more work to implement.

---

_Comment by @ryanhiebert on 2024-11-10 11:38_

I'd also really like to see this mode be the default, particularly for libraries. But I think it wouldn't be a big problem if it was the default for applications as well, because applications have a couple other tools to avoid unintentional forking: upper bounds and checked-in lockfiles.

If an application wants to support fewer (or no) version forks, they can restrict the python version upper bound. Unless the application is being packaged up for consumers to run themselves, or is in the middle of a transition between Python versions and needs to support both for a little while, I'd _highly_ encourage restricting the Python version in an application as much as possible.

Applications also should check in their lockfiles. If the resolver prefers to avoid upgrading versions in the lockfile unless explicitly instructed to (the intended behavior, if I've understood correctly), then it will generally stay on an older version, which in many cases (like a new version of a library having a higher minimum Python version) will avoid forking for a newer Python.

Given those two capabilities together, I feel that this behavior could reasonably be not just the default, but the only option: if forking minimization weren't already implemented I think it would likely be a low priority to add it as an option. That said, I can imagine some application authors preferring this resolution strategy, and many having no preference. So unless you had a strong desire to limit the code paths, I think it would make sense to have some tool configuration in the `pyproject.toml` to instruct the resolver to minimize forking.

Perhaps that could be a setting that tells `uv` whether this is an application or a library. Maybe we can autodetect it from the presence of a `build-system` as @henryiii suggested. Or perhaps based on the presence of the `uv.lock` in the `.gitignore` if we want to have a little bit of magic around the VCS.



---

_Comment by @cbrnr on 2024-11-18 12:57_

Until the new feature becomes available, would someone have a quick look at https://github.com/MIT-LCP/wfdb-python/pull/511 and help me figure out how to implement a per-Python-version resolution strategy using `uv venv` and `uv pip`? I tried some things, but couldn't make it work 😢...

---

_Comment by @ryanhiebert on 2024-11-18 14:29_

For my purposes, Django's version restrictions were causing me to be on an old version of Django with my library. To accomodate that, I updated the `dependencies` array in my `pyproject.toml` to match the python version restrictions from Django, and that forced the `uv` resolver to fork for each of these different paths and lock and install the latest Django.

```toml
[project]
dependencies = [
  "Django>=4.1 ; python_version >= '3.8'",
  "Django>=4.2 ; python_version >= '3.9'",
  "Django>=5.0 ; python_version >= '3.10'",
]
```

---

_Comment by @cbrnr on 2024-11-18 14:40_

Thanks @ryanhiebert, I'll try that! However, I was hoping to avoid cluttering my `pyproject.toml` just because uv does not resolve versions like pip. Therefore, I thought that using `uv pip` would somehow solve the issue, but I couldn't figure out how. Anyway, in the meantime I'll try your suggestion!

---

_Closed by @charliermarsh on 2024-12-13 14:16_

---
