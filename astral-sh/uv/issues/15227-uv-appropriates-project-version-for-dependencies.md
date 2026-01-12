```yaml
number: 15227
title: uv appropriates project version for dependencies when UV_CACHE_DIR is set to a subdirectory of the project
type: issue
state: open
author: mikolak-net
labels:
  - bug
  - external
assignees: []
created_at: 2025-08-11T21:20:46Z
updated_at: 2025-08-15T04:14:27Z
url: https://github.com/astral-sh/uv/issues/15227
synced_at: 2026-01-12T16:02:06Z
```

# uv appropriates project version for dependencies when UV_CACHE_DIR is set to a subdirectory of the project

---

_@mikolak-net_

### Summary

It appears uv will "force" a dependency version to be that of the project when building at least some packages, when `UV_CACHE_DIR` is set to a subpath of the project.

**To reproduce**

1. Create a new project, e.g. `uv init repro`
1. `cd repro`
1. Open `pyproject.toml` and add an affected dependency to the `dependencies` entry, e.g. `dependencies = ["localstack==3.8.1"]`
1. Set up the cache dir in the subpath of the project, e.g. `export UV_CACHE_DIR=.uv-cache`, as provided [in a documentation example](https://docs.astral.sh/uv/guides/integration/gitlab/#caching)
1. run `uv sync`

**Expected result**

The project builds.

**Encountered result**

An error is generated to the tune of:

```bash
Using CPython 3.12.xx
Creating virtual environment at: .venv
Resolved 41 packages in 2.54s
      Built pyaes==1.6.1
      Built tailer==0.4.1
      Built localstack==3.8.1                                                                                                                                                                                                    × Failed to download and build `localstack-ext==3.8.1`
  ╰─▶ Package metadata version `0.1.dev0` does not match given version `3.8.1`
  help: `localstack-ext` (v3.8.1) was included because `repro` (v0.1.0) depends on `localstack` (v3.8.1) which depends on `localstack-ext`
```

PS. The error reporting is a bit obtuse. In the last line, please take note of the dependencies' version vs the project's version.

### Platform

Linux

### Version

uv 0.8.8

### Python version

Python 3.12

---

_Label `bug` added by @mikolak-net on 2025-08-11 21:20_

---

_Comment by @zanieb on 2025-08-12 02:21_

I think this is not a bug with uv, but rather a bug with how that package is determining its version. I presume it's escaping the cache directory and reading from your project's git repository or something?

---

_Comment by @mikolak-net on 2025-08-12 11:52_

@zanieb : it's completely possible that localstack is doing something nonstandard – however, creating an equivalent project using e.g. poetry works fine.

BTW, while the version of the package is somewhat older, localstack isn't exactly the most esoteric project, so I'd assume the maintainers would yank the version in question if it was causing build problems across the board. 

FWIW, the latest stable version, i.e. `4.7.0`, also fails to install.

_(EDIT: forgot to set cache dir for `4.7.0` when first checking it – using that version of localstack *also* exhibits the same issue)_

---

_Comment by @zanieb on 2025-08-12 13:54_

Sorry, but... this still doesn't sound like a problem with uv. They're using setuptools-scm. I don't know why that'd escape the cache when searching for a version, you'll need to raise the problem with setuptools-scm or localstack.

---

_Comment by @zanieb on 2025-08-12 13:54_

I would also just strongly recommend not putting the cache in your project?

---

_Label `external` added by @zanieb on 2025-08-12 13:54_

---

_Comment by @mikolak-net on 2025-08-12 15:02_

> Sorry, but... this still doesn't sound like a problem with uv. [...]

This is your (as in the uv team's/Astral's) prerogative. All I can say that, as a user, I would find it troubling if such usage problems were readily dismissed, especially given:

* we're talking about [one of the more popular packages](https://github.com/localstack/localstack) in the Python ecosystem, and
* the issue is _apparently unique to uv_ and not other package managers.

I also suspect localstack's maintainers would be baffled to receive a bug report with this specific scenario. Speaking of which...

> I would also just strongly recommend not putting the cache in your project?

To reiterate, this use case is reflected [in uv's own documentation](https://docs.astral.sh/uv/guides/integration/gitlab/#caching). And that makes sense, as GitLab's CI requires the cache directory to be placed within the project directory in order to use its, er, caching facility.

As to whether that requirement make sense... personally, I think not. But apparently whoever on your team was writing that documentation page didn't see a problem with uv's usage in this manner.

---

_Comment by @zanieb on 2025-08-12 16:17_

This is reproducible with `pip` if you put its build directory in your project:

```
❯ mkdir example
❯ cd example
❯ git init
❯ mkdir tmp
❯ export TMPDIR=$(pwd)/tmp
❯ uvx python -m venv .venv
❯ .venv/bin/pip install localstack==3.8.1
Collecting localstack==3.8.1
  Downloading localstack-3.8.1.tar.gz (5.7 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting localstack-core (from localstack==3.8.1)
  Downloading localstack_core-4.7.0-py3-none-any.whl.metadata (5.6 kB)
Collecting localstack-ext==3.8.1 (from localstack==3.8.1)
  Downloading localstack_ext-3.8.1.tar.gz (7.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.8/7.8 MB 32.3 MB/s  0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
  WARNING: Requested localstack-ext==3.8.1 from https://files.pythonhosted.org/packages/fb/1e/8e5404825f32b4ce40faf7160defac104eafb669c3b0db6fa25cfec0df8d/localstack_ext-3.8.1.tar.gz (from localstack==3.8.1), but installing version 0.1.dev0
Discarding https://files.pythonhosted.org/packages/fb/1e/8e5404825f32b4ce40faf7160defac104eafb669c3b0db6fa25cfec0df8d/localstack_ext-3.8.1.tar.gz (from https://pypi.org/simple/localstack-ext/) (requires-python:>=3.8): Requested localstack-ext==3.8.1 from https://files.pythonhosted.org/packages/fb/1e/8e5404825f32b4ce40faf7160defac104eafb669c3b0db6fa25cfec0df8d/localstack_ext-3.8.1.tar.gz (from localstack==3.8.1) has inconsistent version: expected '3.8.1', but metadata has '0.1.dev0'
INFO: pip is looking at multiple versions of localstack to determine which version is compatible with other requirements. This could take a while.
ERROR: Ignored the following yanked versions: 0.0.0, 2.0.0, 3.0.0, 3.0.0.post1, 3.0.0.post2, 3.0.0.post3
ERROR: Could not find a version that satisfies the requirement localstack-ext==3.8.1 (from localstack)
...
```

---

_Comment by @zanieb on 2025-08-12 16:21_

As before, I think you need to raise this issue with setuptools-scm. They probably shouldn't go past project boundaries when discovering the version to use? However, I'm not sure about the implications for them, nor am I familiar with their implementation details — that's why I'm trying to redirect you.

Regarding the GitLab CI cache... I'm not really sure what to do there. That was contributed by another user in https://github.com/astral-sh/uv/pull/8000. I'm not an expert on GitLab CI either.

---

_Comment by @RonnyPfannschmidt on 2025-08-12 20:29_

hi, i just yanked setuptools_scm 9.1.1 - the new feature of simplified self activation resulted in a chain of regressions - an with every iteration i found new interesting ways people configure their projects

the next iteration will use a more robust mechanism - but thats far away

---

_Comment by @mikolak-net on 2025-08-13 22:20_

@zanieb : re reproducibility, to clarify – as written [in this comment](https://github.com/astral-sh/uv/issues/15227#issuecomment-3178993862), whether or not the issue is ultimately due to the package performing some shenanigans is not something that's being challenged here.

The aim here was to demonstrate the user's perspective: building works in one system (e.g. poetry), but not in another (uv).

Re GitLab: I believe the uv documentation in question is factually correct. The cache directory *has to be* contained in the project dir, otherwise GL CI will not pick up the resultant files as "cache".

In general, I want to stress I am raising awareness of this issue not in a "fix it now" manner, but to highlight something that is potentially happening to many users, but does not clear the threshold of reporting for many of them (in this particular case, caching shaves off little time compared to the overall speed improvements that come from uv adoption).

----

@RonnyPfannschmidt : thanks for the prompt reaction (and kudos to @zanieb if they brought this into your attention)! However, I think the problem you mentioned is not the underlying issue: `localstack==3.8.1` (or rather, [`localstack-ext`](https://pypi.org/project/localstack-ext/3.8.1/) ) pre-dates [`setuptools_scm==9.1.1` ](https://pypi.org/project/setuptools-scm/9.1.1/) by almost a year.

---

_Comment by @zanieb on 2025-08-13 22:43_

`localstack-ext` is being built from source because they don't provide pre-built wheels, so `setuptools-scm` is being invoked there, and because it is not common to have constraints on build dependencies, the latest version of `setuptools-scm` is used. It doesn't matter that `localstack-ext` is old. e.g., there are only lower bound constraints on `setuptools-scm`

https://inspector.pypi.io/project/localstack-ext/3.8.1/packages/fb/1e/8e5404825f32b4ce40faf7160defac104eafb669c3b0db6fa25cfec0df8d/localstack_ext-3.8.1.tar.gz/localstack_ext-3.8.1/pyproject.toml#line.3

Anyway, I think @RonnyPfannschmidt misdiagnosed the problem. It does reproduce on an older version of `setuptools-scm`

```
❯ echo "setuptools-scm==8.3.1" > constaints.txt
❯ UV_BUILD_CONSTRAINT=constraints.txt uv pip install localstack==3.8.1 --no-cache
Resolved 38 packages in 2.03s
      Built pyaes==1.6.1
      Built tailer==0.4.1
      Built localstack==3.8.1                                                                                                                                                               × Failed to download and build `localstack-ext==3.8.1`
  ╰─▶ Package metadata version `0.1.dev0` does not match given version `3.8.1`
  help: `localstack-ext` (v3.8.1) was included because `localstack` (v3.8.1) depends on `localstack-ext`
```


---

_Comment by @notatallshaw on 2025-08-15 01:04_

And outside take on this issue: 

> * we're talking about [one of the more popular packages](https://github.com/localstack/localstack) in the Python ecosystem, and

localstack appears to be the 5,834th most popular package on PyPI: https://hugovk.github.io/top-pypi-packages/.

> the issue is apparently unique to uv and not other package managers.

As @zanieb has already shown, this issue _does_ happen with pip and placing the build directory in the project.

It appears the issue here is _likely_ one of:

 * A bug in the way localstack has defined where to source the version number
 * Or, a design issue in setuptools-scm where protection could be added to prevent this but currently isn't
 * Or, a design limitation with gitlab, not allowing the much richer uv cache to not interfere with the sdist build process, in which case you may need to turn off cache on gitlab, or find some other workaround configuration to prevent setuptools-scm for inspecting outside the cache

What I don't think has been made clear in this discussion, but when a Python package installer frontend (like uv, pip, poetry, etc.) calls a build backend (like setuptools, hatch, etc.) it does not have any control over what the build backend does, the backend code executes and the frontend waits for the results.

So your insistence on saying the cause is uv is _likely_ untrue. At least in the sense of being a bug, and not a series of application designs interacting with each other from multiple projects. If you would like to find out the root cause you need to engage with all the parties of you're very specific case (localstack, localstacks build dependencies, gitlab, uv cache inside the project directory). Maybe uv could add some protection here, but I don't obviously see how.

I suspect this will affect any installer that uses fully expanded packages in their cache with hard links pointing to them. And as uv has shown their cache design brings a lot of benefits other installers, like Poetry, may well adopt this in the future. 

---

_Comment by @RonnyPfannschmidt on 2025-08-15 04:14_

One confusing aspect is that Setuptools_scm intentionally doesn't escape the build directory 
Yet it happens here 


Logs of the build process with SETUPTOOLS_SCM_DEBUG=1 would help 

---
