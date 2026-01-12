```yaml
number: 3957
title: Add a uv build backend
type: issue
state: closed
author: chrisrodrigue
labels:
  - enhancement
  - build-backend
assignees: []
created_at: 2024-06-01T15:58:19Z
updated_at: 2025-07-03T17:36:18Z
url: https://github.com/astral-sh/uv/issues/3957
synced_at: 2026-01-12T15:58:47Z
```

# Add a uv build backend

---

_@chrisrodrigue_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv is a fantastic tool that is ahead of its time. In the same vein as ruff, it is bundling many capabilities that Python developers need into a single tool. It currently provides the capabilities of `pip`, `pip-tools`, and `virtualenv` in one convenient binary.

### Python can't build out of the box

As of Python 3.12, which has removed `setuptools` and `wheel` from standard Python installations, a user is unable to perform `pip install -e .` or `pip install .` in a local `pyproject.toml` project without pulling in external third-party package dependencies. 

This means that in an offline environment without access to PyPI, a developer is dead in the water and cannot even install their *own project* from source. This is a glaring flaw with Python, which is supposed to be "batteries included." 

### uv can fix this

I propose that the `uv` binary expand its capabilities to also function as a build backend.

If uv could natively build projects from source, it would be a game changer!


---

_Renamed from "uv should provide a build backend" to "`uv` should provide a build backend" by @chrisrodrigue on 2024-06-01 16:01_

---

_Comment by @potiuk on 2024-06-01 17:05_

> I think that if uv could natively build projects from source, it would be a game changer. 

I personally believe that this is pretty much against the whole idea of modern approach and splitting the backend vs. frontend responsibilities. The idea (philosophically) behind the backend/frontend split is that the maintainers of the project (via pyproject.toml) choose the backend that should be used to build their tool, while the user installing Python project is free to choose whatever fronted they prefer. This is IMHO a gamechanger in the python packaging and we should rather see a stronger push in that direction than weakening it.  And I think it's not going to go back - because more and more projects will revert to use pyproject.toml and backend specification of build environment. In case of Airflow for example - as of December there is no way to install airlfow "natively" without actually installing hatchling and build environment. But we do not give the option to the frontend. You MUST use hatchling in specified version and few other dependencies specified in specific versions to build airflow. Full stop. UV won't be able to make their own choices (It will be able to choose a way how to create such an environment but not to choose what should be used to build airlfow).

But also maybe my understanding of it is wrong and maybe you are proposing something different than I understand.

BTW. I do not think access to PyPI is needed to build a project with build backend. The frontend my still choose any mechanism (including private repos if needed) to install build environment, no PyPI is needed for it, the only requirement is that pyproject.toml specifies the environment.

---

_Comment by @chrisrodrigue on 2024-06-01 18:17_

@potiuk 

I agree and think the backend and frontend specifications should be separate, I am merely suggesting that `uv` could provide a build capability similar to its virtualenv capability (`uv venv`), such that building can be possible for users that don't have one, perhaps via Project API such as `uv install`. I like the `pdm` approach of providing its own build backend (`pdm-backend`) and installing projects as editable by default inside the automatically managed `.venv` of the project when a user does `pdm install`.

You do need access to PyPI or some repo hosting the build backend to build a project. Python does not include `setuptools` or any other build backend in the latest distributions (it used to include at least `setuptools`). It downloads the build backend specified in the `pyproject.toml` (or `setuptools` if none specified) in an isolated environment and uses it to build python projects from source. If `--no-build-isolation` is specified, it expects the build-backend to already be available in the current system/virtual environment in order to build. 

IMO, `pip install [-e] .` is broken out of the box because it relies on third party dependencies which may not be accessible. In the stdlib we get argument parsing, unit testing, logging, and other amenities, but we can't perform the most fundamental action in the software development process: installing/building your code. 

Without the capability to build from source out of the box, Python is hamstringed and can only run the most rudimentary scripts, leaving users with multi-module projects to resort to ugly `PYTHONPATH`/`sys.path` hacks to get their packages/subpackages/modules found by the interpreter.


---

_Comment by @potiuk on 2024-06-01 19:51_

> MO, pip install [-e] . is broken out of the box because it relies on third party dependencies which may not be accessible. In the stdlib we get argument parsing, unit testing, logging, and other amenities, but we can't perform the most fundamental action in the software development process. Without the capability to build from source out of the box, Python is hamstringed and can only run the most rudimentary scripts, leaving users to resort to ugly PYTHONPATH/sys.path hacks to get their packages/subpackages/modules found by the interpreter.

But this is where the whole packaging for Python is heading. The PEPs of packaging precisely specify this is the direction and if project maintainers choose so, unless you manually (like conda) maintain your build recipes for all the packages our there, you won't be able to build project "natively". 

Just to give you example of Airflow. Without `hatchling` and build hooks (implemented in `hatch_build.py`  you are not even able to know what Airflow dependencies are, because "requirements", "optional-requirements" are declared as dynamic fields - and they don't even have (as mandated by the right PEP) specification of those dependencies in pyproject.toml. And we have no `setup.py` either any more.

The only way to find out what dependencies Airflow needs for editable build, or to build a wheel package is to get the right version of hatchling and get the frontend execute the build_hook - the build hook returrns such dependencies dynamically.  You can see it yourself here https://github.com/apache/airflow/blob/main/pyproject.toml -> there is no way to build airflow from sources in current main without actually installing those packages:

```
    "GitPython==3.1.43",
    "gitdb==4.0.11",
    "hatchling==1.24.2",
    "packaging==24.0",
    "pathspec==0.12.1",
    "pluggy==1.5.0",
    "smmap==5.0.1",
    "tomli==2.0.1; python_version < '3.11'",
    "trove-classifiers==2024.5.22",
```

And letting hatchling invoke `hatch_build.py`. I am not sure what you mean by "native" installation - but you won't be able to install airflow differently.

And I think - personally (though I was not part of it) - that the decisions made by the packaging team were pretty sound and smart, and they deliberately left the decision for maintainers of a project to choose the right backend packages needed (and set of 3rd-party tools) and all frontends have no choice but to follow it.  I understand you might have different opinion, but here - the process of Python Software Foundation and Packaging team is not an opinion - they have authoritative power to decide it by voting and PEP approval. And the only way to change it is to get another PEP approved.

Here is the list of those PEPs (and I am actually quite happy Airflow after 10 years finally migrated out of setuptools and setup.py by following those standards as finally the tooling - including the modern backends you mentioned support it for sufficently long time): 

* PEP-440 Version Identification and Dependency Specification
* PEP-517 A build-system independent format for source trees
* PEP-518 Specifying Minimum Build System Requirements for Python
* PEP-561 Distributing and Packaging Type Information
* PEP-621 Storing project metadata in pyproject.toml
* PEP-660 Editable installs for pyproject.toml based builds (wheel based)
* PEP-685 Comparison of extra names for optional distribution


---

_Comment by @potiuk on 2024-06-01 19:59_

And BTW. if uv provides a build backend, you will still be able to choose it when you maintain your project - but it will also be a 3rd-party dependency :) 

Someone installing your project  with any frontend will have to download and install according to your specification in pyproject.toml. Similarly as hatch/hatchling pair that are separate. Even if you use hatch to install airlfow, it has to anyhow download hatchling in the version specified by the maintainer of the project you are installing and use it to build the package. 

---

_Comment by @chrisrodrigue on 2024-06-01 22:27_

It's interesting that `setuptools` remains the "blessed" build backend that `pip` defaults to use when no backend is specified in the `pyproject.toml`. Rather than bake `setuptools` into the stdlib, it silently forces the download and installation of it as an unauthorized third-party build dependency. This also seems to violate the principle of "explicit is better than implicit."

One of the nice things about Astral tools like `uv` and `ruff` is that they do not bloat your package/library when you use them, and bundle the capabilities of many tools into single static binaries. 

If you use `uv` specifically for its virtual environment capability, it's a single dependency. Conversely, if you were to use `virtualenv`, you've now just pulled in 3 more transitive dependencies ( `distlib`, `filelock`, `platformdirs`). Similarly, instead of using `pylint`, `flake8`, `pyupgrade`, `black`, `isort`, and all of their dependencies, you only need `ruff`. This has far reaching implications for companies abiding by Software Bill of Materials (SBOMs) that need to manually vet each piece of FOSS in their software toolchains.

I think `uv` could provide a build backend that could be specified in `pyproject.toml` and used as the default when no backend is specified. It doesn't necessarily have to be separate from the main `uv` binary, since the `uv` binary could support the build capability as builtin uv/uv pip commands (`uv install`, `uv pip install -e .`).

```toml
[build-system]
requires = ["uv"]
build-backend = "uv.api"
```

---

_Comment by @potiuk on 2024-06-02 10:14_

> I think uv could provide a build backend that could be specified in pyproject.toml and used as the default when no backend is specified. It doesn't necessarily have to be separate from the main uv binary, since the uv binary could support the build capability as builtin uv/uv pip commands (uv install, uv pip install -e .).

Well. There are always trade-offs and what you see from your position might be important for you, might not be important for others and the other way. For example - If (like we did with airflow being popular package) you had gone through some of the pains where new releases of setuptools suddently started breaking packages being built - because deliberately or accidentally breaking compatibilities and suddenly being flooded by 100s of your users having problem with installing your package without you doing anything you'd understand that bundling the way how things are run with specific version of frontend is a good idea when you have even moderately big package. 

That's what I love that we as maintainers can choose and "lock" the backend to the tools and version of our choice - rather than relying that the version of the tool that our users choose will behave consistently over the years. That was a very smart choice of packaging team based on actual learnings from their millions of users over many years, and while I often criticized their choices in the past, I came to understanding that it's my vision that is short-sighted and limited - I learned a bit of empathy.

I'd strongly recommend a bit more reading and understanding what they were (and still do) cooking there. Python actually deliberately removeed setuptools in order to drive more the adoption of what's being developed as packaging standards (and that's really smart plan that was laid out years ago and is meticulously and consistently, step-by-step put in motion. And I admire the packaging team for that to be honest.

What you really think about is not following and bringing back old "setuptools" behaviour is  something else that packaging team has already accepted and a number of tools are implementing https://peps.python.org/pep-0723/ - which allows you to define a small subset of pyproject.toml metadata (specifically dependencies) in the single-file scripts. And this is really where yes - any front-end implementing PEP-723 should indeed prepare a venv, install dependencies and run the script that specifies such dependencies. 

Anything more complex that really has a bit more complex packaging need - putting more files together, should really define their backend, in order to maintain "build consistency", otherwise you start to be at mercy of tool developers who might change their behaviours at any time and suddenly not only you but anyone else who want to build your package will suddenly have problems with it.

But yes. If `uv` provides backend to build packages, that you (as project maintainer) specify in your build dependencies, this is perfectly fine - just one more backend to choose among about 10 available today. And if - as maintainer - you will prefer to specify it in your project, you should be free to do so. But as a maintainer, I would never put faith that future versions of specific front-end (including uv) will continue building my package in the same way in future versions.

BTW. Piece of advise - for that very reason, as a maintainer you should do:

```pyproject.toml
[build-system]
requires = ["uv==x.y.z"]
build-backend = "uv.build"
```

Otherwise you never know which version of uv people have and whether they have a version that is capable of building your package at all (i.e. old version that has no backend).


---

_Comment by @potiuk on 2024-06-02 10:26_

Also - again if you have proposal how to improve packaging, there are discourse threads there and PEP could be written, so I also recommend you, if you are strongly convinced that you can come up with a complete and better solution - please start discussion there about new PEP, and propose it, lead to approval and likely help to implement in a number of tools - this is the way how standard in packaging are being developed :)

---

_Comment by @notatallshaw on 2024-06-02 15:21_

> It's interesting that `setuptools` remains the "blessed" build backend that `pip` defaults to use when no backend is specified in the `pyproject.toml`. Rather than bake `setuptools` into the stdlib, it silently forces the download and installation of it as an unauthorized third-party build dependency. This also seems to violate the principle of "explicit is better than implicit."

FYI, I beleive this is because pip maintainers are very conservative when it comes to breaking changes, not because it is the intended future of Python packaging.

For example, the old resolver, which can easily install a broken environment, is still available to use even though the new resolver has been available for over 5 years and turned on by default for over 4 years.

---

_Comment by @daviewales on 2024-09-06 03:51_

A uv-aware build backend would enable private git packages to depend on other private git packages, and still be installed with `pip`, `pipx`, `poetry`, etc, without needing the package end user to change their tools. See https://github.com/astral-sh/uv/issues/7069#issuecomment-2332944182 for a detailed example. (Poetry packages work this way, as they use the poetry-core build backend.)

---

_Comment by @chrisrodrigue on 2024-10-02 13:20_

Just some more thoughts, notes, and ramblings on this.

## Full stack uv

The build backend capability could be rolled into the `uv` binary itself as a feature rather than fractured off as a separate dependency. 

This would give developers the option to utilize uv as either a frontend, a backend, or both, while still maintaining the single static dependency.

Developers electing to use uv as both frontend and backend could have access to some optimizations that might not be possible individually. 

We could call this use case “full stack uv” since that’s usually what we call frontend + backend, right? 🤪

### Full stack uv without a specified version

```toml
requires = ["uv"]
build-backend = "uv"
```

Backend `uv` wouldn’t need to be downloaded since `uv` can just copy itself into the isolated build environment. 

Or, `uv` can special case itself as a backend and do something even more optimized. This jives with PEP 517:

>We do not require that any particular “virtual environment” mechanism be used; a build frontend might use virtualenv, or venv, or no special mechanism at all. 

### Full stack uv with pinned or maximum version

```toml
requires = ["uv==0.5"]
build-backend = "uv"
```

PEP 517 build isolation can guarantee that frontend `uv` and backend `uv` of different versions do not conflict.

> A build frontend SHOULD, by default, create an isolated environment for each build, containing only the standard library and any explicitly requested build-dependencies

However, a decision could be made to maintain backward compatibility for the backend, such that newer versions of uv could satisfy whichever version is declared.

## On PEP 517 compliance

The backend feature could be PEP 517 compliant so that other frontends (like poetry, pdm, pip, hatch, etc.) can use uv as a backend, but this could be distinct from full stack uv.

>A Python library will be provided which frontends can use to easily call hooks this way.

uv would need to expose some hooks in the build environment via Python API. The mandatory and optional hooks at the time of this writing are:

```python
# Mandatory hooks
def build_wheel(wheel_directory, config_settings=None, metadata_directory=None):
    ...

def build_sdist(sdist_directory, config_settings=None):
    ...

# Optional hooks
def get_requires_for_build_wheel(config_settings=None):
    ...

def prepare_metadata_for_build_wheel(metadata_directory, config_settings=None):
    ...

def get_requires_for_build_sdist(config_settings=None):
    ...
```

Perhaps a highly optimized, importable module or package named `uv` could be autogenerated by the `uv` binary at build time to satisfy these requirements? PEP 517 says it could even be cached:

>The backend may store intermediate artifacts in cache locations or temporary directories.

---

_Comment by @hauntsaninja on 2024-10-03 02:46_

If it was up to me, and assuming the build backend isn't too complicated, I'd consider doing both:
1) Have a `uv-build` package that provides a build backend
2) The uv frontend detects this backend and special cases the hell out of it to avoid PEP 517/660 overhead.

`uv-build` can currently basically just depend on `uv`, but a separate package gives Astral the possibility of adding a pure Python implementation of the build backend or having a slimmer build, since build dependencies with build dependencies is finicky business

---

_Comment by @ofek on 2024-10-03 02:52_

Hatch does 2. with Hatchling.

---

_Assigned to @konstin by @konstin on 2024-10-12 14:58_

---

_Label `enhancement` added by @charliermarsh on 2024-10-16 13:35_

---

_Comment by @johnthagen on 2024-10-28 13:45_

In my opinion, another benefit of `uv` providing it's own backend is simply so that users don't have to track multiple projects/maintainers/bug trackers/user guides for their entire packaging pipeline.

In the same way that `uv` replaces `build`, `twine`, etc, it would be convenient if users didn't have to track `hatchling`, `flit`, etc., projects and could come to `uv`/`uv-build` for their holistic process (like Cargo).

---

_Comment by @cthoyt on 2024-10-28 14:00_

It looks like it's in the works, check https://github.com/astral-sh/uv/pull/7976 and https://github.com/astral-sh/uv/blob/main/python/uv/_build_backend.py. It just hasn't been completed/announced yet

note: I'm not part of the uv team, just an excited observer

---

_Comment by @JacobCallahan on 2024-10-28 21:58_

@cthoyt thanks for the update! I see in the Proposed docs you linked that the uv build backend is only compatible with projects that nest their source under a `src/` directory.  It would be nice if we could configure that to be less restrictive. A lot of projects near their source under a `<project_name>/` directory. Thanks

---

_Comment by @johnthagen on 2024-10-30 17:58_

When this is complete, we should update the Python Packaging Guide to list `uv-build` as an option

- https://packaging.python.org/en/latest/flow/#the-configuration-file
- https://packaging.python.org/en/latest/tutorials/packaging-projects/#choosing-a-build-backend

Actually there are probably a lot of places in the Packaging User Guide where `uv` should be mentioned as well. Probably a search for "Poetry" would be a good start. Though maybe this should be a tracked in a separate issue.

---

_Renamed from "`uv` should provide a build backend" to "Add a build backend / uv-native build system" by @zanieb on 2024-11-26 23:51_

---

_Renamed from "Add a build backend / uv-native build system" to "Add a uv build backend" by @konstin on 2024-12-04 16:22_

---

_Comment by @konstin on 2024-12-04 16:33_

Cross-posting from #8779: There is now a basic build backend hidden behind preview on main. While it's far from production ready, you can now try it out with `UV_PREVIEW=1` and by adding:

```
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"
```

There's more (draft) docs in #8779. Please report any problems, your feedback or what you need for your project, right here in this thread or feel free to open a new issue for specific problems. 

---

_Comment by @cthoyt on 2024-12-04 23:21_

Can we have an addition to the `--build-backend` argument of `uv init` to accept uv in preview mode? I think this makes sense in general, but also will make it easier to post minimum reproducible examples for future issues

Here's what I get on uv 0.5.6 (b70c4f30e 2024-12-03):

```console
$ UV_PREVIEW=1 uv init test-package-name --build-backend uv
error: invalid value 'uv' for '--build-backend <BUILD_BACKEND>'
  [possible values: hatch, flit, pdm, setuptools, maturin, scikit]
```

I guess it should be something inside https://github.com/astral-sh/uv/blob/main/crates/uv-configuration/src/project_build_backend.rs, but I am a Rust novice and don't know if there's a tricky macro for adding something to this enum dynamically.

Then, you could update https://github.com/astral-sh/uv/blob/7939d3fb5bcb79c16b85327820993efdfc513bd0/crates/uv/src/commands/project/init.rs#L864-L914 accordingly

---

_Comment by @zanieb on 2024-12-05 01:19_

That seems like a good idea! We could add it there as a hidden attribute then fail at runtime if preview is not enabled.

---

_Comment by @johnthagen on 2024-12-05 02:27_

I'm also curious: is the plan to eventually make the uv build backend the default? It think it would be nice from user perspective to get "everything" a user needs from uv by default rather than straddling uv and hatch teams/repos. Of course, we still support opting into other build backends if needed, but I suspect the vast majority will be able to simply stick with uv.

---

_Comment by @zanieb on 2024-12-05 03:20_

Yep that's the plan, it'll give us a lot more control over error messages / user experience.

---

_Comment by @konstin on 2024-12-05 14:31_

> Can we have an addition to the --build-backend argument of uv init to accept uv in preview mode?

Good call, added in https://github.com/astral-sh/uv/pull/9661

---

_Comment by @thewh1teagle on 2024-12-07 20:30_

Can we specify to build the wheel platform dependent? (different wheel for each platform)

---

_Comment by @mgorny on 2024-12-21 07:55_

I'm sorry to be negative, I do really appreciate `uv` project in general, I really love it as a package manager! But I think this is a horrifying idea that will have a strong negative impact on downstreams. `uv`'s popularity will mean that a lot of people will decide to use this backend, and effectively will introduce a non-pure Python, and non-portable dependency in pure Python projects. Having people use uv's build backend will mean that I (and others, I'm sure) will just have to keep filing bugs, asking people to switch to a pure Python build backend, so that we can actually build their packages on more than a handful of platforms that `uv` currently works on (or will ever work on).

---

_Comment by @potiuk on 2024-12-21 08:42_

Yeah. I kind of agree with @mgorny that `uv` as build frontend can get by with using rust but as backend it's a bit problematic.

Also another negative side of it is that uv packages are huge (~15 MB) , and when you use "isolated environment", and someone uses different frontend that does not use uv caching capabilities, they will basically download that binary every time they build the project - even if their platform is supported.

But I think there is nothing wrong with uv providing it's own build backend as long as it is trimmed down to much smaller size and possibly python-only (but likely that's not where some of the `uv` internal code can be reused, so it might make no sense to develop it by astral team).

I personally love `flit` backend - which is `0-dependencies`, pure-python backend and `hatchling` when you need more versatility and dynamic zpyproject.toml` properties - hatchling shines there.

---

_Comment by @nathanscain on 2024-12-21 13:40_

I'm a bit confused on what the issue is you are attempting to draw attention to?

case: project itself is pure python => only devs need to ever build. Regardless of your platform the `-none-any.whl` will work directly and you'll never need to pull uv unless you just don't like wheels. If you are wanting to make edits, you'd also need the build frontend anyway... so you'd have full uv already. Idk how this is different than when I had to setup poetry just to contribute to a project that used it - that's just how build front end and back ends work. The project chooses the frontend for development and the backend for distribution - both for what works best for the developer. What downstreams would be affected that both can't use the wheel and don't need to make edits?

case: project is not pure python => the build backend would need to support your custom target. This is already very iffy and building someone's compiled python package on a non supported target doesn't really work most of the time (even on a supported platform like windows, many projects can't build through their backend without custom setup and new dependencies on your machine — see lxml, numpy, etc). At that point it is on the project to choose their target environments and a complementing build backend to align with that. uv supports more systems than almost all compiled python packages on PyPI - it is unclear to me how uv would even get the bug report in this case and not the project that choose not to support your environment with their compiled wheel.

Someone using the uv backend without the uv frontend would be (an odd imo) personal choice. Either way, by choosing the uv backend - they are opting into the compiled dependency (that again supports more platforms than almost anything else in the Python ecosystem)

---

_Comment by @mgorny on 2024-12-21 14:54_

> Idk how this is different than when I had to setup poetry just to contribute to a project that used it - that's just how build front end and back ends work.

If you use Poetry as a build backend, then you don't install the whole Poetry. You get a much smaller `poetry-core` package that provides only the necessary bits for the backend. It is pure Python, relatively small and works everywhere where Python works. None of these points can be made about `uv`.

> What downstreams would be affected that both can't use the wheel and don't need to make edits?

Debian, Fedora and Gentoo are the ones I'm aware of.

> Someone using the uv backend without the uv frontend would be (an odd imo) personal choice.

If you opt into using uv backend, you are effectively forcing all your users to use the uv backend if they want to *build* your package. That's how PEP 517 works. If you use `pip`, you still fetch and install `uv` into the isolated build tree just to build the package. And if there is no wheel matching your platform, you end up building `uv` for source.

> (that again supports more platforms than almost anything else in the Python ecosystem)

That is simply not true. Rust packages are far less portable than pure Python code. uv is less portable than an average Rust/Python package (say, cryptography) because it has buggy dependencies. And I know that, because I'm literally working on fixing them. Still, given the amount of moving parts, future regressions are inevitable.

On top of that, uv follows new Rust versions fast. By design, PEP517 implies pulling the newest uv version available for building. This means that whenever a new uv version starts requiring newer Rust compiler than the particular user has, all package builds will start failing on Rust toolchain being too old.

---

_Comment by @potiuk on 2024-12-21 15:16_

Yep. All that @mgorny wrote is 💯 . 

The difference between frontend and backend is that backend is chosen by maintainers (by settting it in build-dependencies in pyproject.toml), where frontend is chosen by the contributor who has their own tooling, environment, IDE, whatever. And contributor might want to contribute to project on really old, or really different platforms than maintainers ever run it with, so having a pure-python backend is generally very good idea. 

---

_Comment by @nathanscain on 2024-12-21 15:17_

> Debian, Fedora and Gentoo are the ones I'm aware of.

I run Fedora and RHEL - no idea what you are talking about. I can always pull and use `none-any` wheels. In fact, it is way easier than pulling sdist distributions.

> If you opt into using uv backend, you are effectively forcing all your users to use the uv backend if they want to _build_ your package. … And if there is no wheel matching your platform, you end up building uv for source.

As far as odd personal choices go, building a pure python package yourself instead of using the built wheel that works everywhere would certainly qualify. That’s on you, not the package maintainer. They have given you everything you need to install the package everywhere without any build dependencies.

Again `none-any` wheels support **every** platform. What is the issue?

> Rust packages are far less portable than pure Python code.

I never said otherwise, I said “uv supports more systems than almost all **compiled** python packages on PyPI”

The whole point is that the backend system for local builds is only blocking for compiled wheels, not pure python ones. At that point, uv supports you or doesn’t - either way that is the package maintainer choosing a backend that only supports their target environments.

> On top of that, uv follows new Rust versions fast. … This means that whenever a new uv version starts requiring newer Rust compiler than the particular user has, all package builds will start failing on Rust toolchain being too old.

Do you intend on building uv every time it is pulled as a build backend? The rust tool chain only matters when building uv itself - not when building wheels with uv once built. Even if there were an issue with the rust tool chain (such as for a compiled rust python package after uv’s build-backend supports such a target) that would again be on the package maintainer for not specifying a proper upper bound on the build dependency (which uv recommends you do)

Am I totally missing a whole community of people that want to build from the sdist every time and aren’t okay want the obvious negative consequences of that decision? As the person who manages pulling packages into our internal mirror - I can’t think of anything that makes sdist better except for building against Python versions that the maintainer didn’t distribute wheels for… again a package maintainer issue.

---

_Comment by @potiuk on 2024-12-21 15:20_

> I run Fedora and RHEL - no idea what you are talking about. I can always pull and use none-any wheels. In fact, it is way easier than pulling sdist distributions.

@nathanscain  This is precisely the point. `uv` being rust package has no `none-any` wheel - by definition, because it uses system libraries when calling compiled code. If you don't believe me, look at the https://pypi.org/project/uv/#files

---

_Comment by @nathanscain on 2024-12-21 15:22_

Please try saying it another way? The build backend only matters when building the package … which you literally never have to do when the package gives you wheels?

I don’t need a `none-any` wheel for a build backend that I’d never run as a user.

---

_Comment by @potiuk on 2024-12-21 15:23_

> Please try saying it another way? The build backend only matters when building the package … which you literally never have to do when the package gives you wheels?

You need it when you want to contribute to the code. Both @mgorny and myself are worried about non-maintainer contributors who would like to run and build other's python packages while contributing. 

---

_Comment by @potiuk on 2024-12-21 15:27_

For example - Apache Airflow https://github.com/apache/airflow  has 3200+ contributors and we go to a great length to attract new contributors to work with Airlfow. This means making it possible to check-out and build Airflow on various platforms and we want those contributors to use their own tooling etc, because we do not want to force them to "our only" choice. That includes working in codespaces and many others. If we choose a backend that does not work on RHEL, this means that contributors who uses RHEL will not be able to contribute to Airflow.

Bad idea.

---

_Comment by @mgorny on 2024-12-21 15:40_

> Please try saying it another way? The build backend only matters when building the package … which you literally never have to do when the package gives you wheels?

This is entirely incorrect. Linux distributions almost uniformly use source distributions (and often git archives and alike) rather than wheels. And since Gentoo is a source-first distributions, this means that practically *all* our users build from source distributions. For us, the critical drawback of wheels is that most of the time, they don't provide any way of testing the package. And before you claim otherwise, multiple times we have found issues that upstream testing did not reveal, and that could have broken production otherwise.

---

_Comment by @nathanscain on 2024-12-21 15:40_

Maybe this is just a totally different pov, but I always adopt a package’s preferred workflow when contributing - frontend and backend since both are configured in the pyproject.toml and I’d rather not reinvent the wheel (pun intended). I use uv tool install to grab their tool chain, make sure I can build with the build command in their CONTRIBUTING.md, and then proceed with my contribution.


Again, I’m not sure what RHEL issues you speak of as uv supports RHEL - I use it almost every day and it is the main target platform for internal company projects. If you have concerns about this as a maintainer, you can obviously choose anything but uv. Is it wrong for an option like the uv backend to exist for maintainers that only want to support their target environments? The whole point is that it is as fast as possible, so yeah it is compiled and I am okay with that. Potential contributors just need to install uv which is not a big ask, and it is very common for packages to require that you have their tool chain to build.

---

_Comment by @Conan-Kudo on 2024-12-21 15:42_

> > Debian, Fedora and Gentoo are the ones I'm aware of.
> 
> I run Fedora and RHEL - no idea what you are talking about. I can always pull and use `none-any` wheels. In fact, it is way easier than pulling sdist distributions.
> 
> [...]
>
> Am I totally missing a whole community of people that want to build from the sdist every time and aren’t okay want the obvious negative consequences of that decision? As the person who manages pulling packages into our internal mirror - I can’t think of anything that makes sdist better except for building against Python versions that the maintainer didn’t distribute wheels for… again a package maintainer issue.

I'm a maintainer of a large number of Python ecosystem packages in the Fedora Linux distribution, we *always* build everything from source and create our own network of binary packages (in the form of RPMs). We build and extend for platforms that the Python community may or may not care about (such as IBM POWER and Z). The Rust toolchain issue is a significant one.

I do not think it's a good idea for `uv` to be a build backend, because it significantly complicates the whole story for Python software. Build backends that aren't pure-Python are very complex to deal with and are a hassle with platform bringup.

---

_Comment by @vlad-ivanov-name on 2024-12-21 15:46_

Not to diminish everything else said in this thread about Python package distribution in mostly open-source environments, but I just wanted to say that commercial Python package delivery also exists (even though the packages themselves can be open source and licensed accordingly). For those packages that aren't community driven and have one sole code owner, being packaged for Linux distributions is unlikely. So while I understand the ecosystem concern it would be good not to deny the build backend for everyone who would actually benefit from it.

---

_Comment by @nathanscain on 2024-12-21 15:52_

Thank you, Vlan. I was starting to think I was going crazy…

To summarize my point - it is on the maintainer to choose a build system that supports their target environments. While I understand that some packages have need to support a much wider range of distribution systems, architectures, etc —  I don’t think that should mean that other packages which have zero intention to be packaged back a Linux distribution should not be allowed a faster toolchain that works for the targets we are actually targeting. By all means, choose a different backend as a maintainer if you want to support this - my packages will _never_ be rpms and will never run on anything but x86 or ARM… and I’m 1000% okay with the trade off for a better and faster system for 100% of _my_ contributors and user base.

It is okay for things to be good even if they aren’t made for your use case. 

---

_Comment by @chrisrodrigue on 2024-12-21 16:13_

I don't want to speak for the Astral team, but I am pretty certain that the top priority of uv is performance. A pure Python build backend wholly limits performance because then a Python interpreter needs to be invoked to build a project.

Users of `uv` as a frontend are under no obligation to use it as a backend, and vice versa. As already mentioned, `flit-core` is a pure Python build backend with 0 dependencies and users are free to use that. 

(I previously floated the idea of encoding a pure Python build backend in uv (#6423) so that it would be available to users of `uv` without them having to download it.)

If uv is intended to be a cargo for Python, what good would it be if `uv build` does not work out of the box? This is a fundamental development action that should be performant, not a ham-fisted external dependency.



---

_Comment by @nathanscain on 2024-12-21 16:20_

Exactly this. My internal packages are built to target RHEL and Windows - both x86_64 and almost all are pure python. I just want my editable install to not have to spend 10 seconds talking to setuptools to calculate my build metadata every time I invalidate the cache by changing the pyproject.toml or dynamic version. I’m willing to sacrifice it ever working on a nonexistent gentoo system for that to be milliseconds instead.

There are plenty of backends that run pure python - I don’t need a different backend that will take just as long for my use case. I need one that will make building the package inconsequential while developing for my user base.

---

_Comment by @potiuk on 2024-12-21 16:29_

> If uv is intended to be a cargo for Python, what good would it be if uv build does not work out of the box? This is a fundamental development action that should be performant, not a ham-fisted external dependency.

The whole frontend-backend separation was precisely introduced so that maintainers can choose their build backend (and for example like it is in airflow dynamically prepare their dependencies using hatch_build.py that exposes billd hooks), while not at all forcing the users to use front-end. While I love `uv` and we even recommend it in Airlfow to use (because we are using `workspace` feature of `uv` - we also go to a great length that anyone who runs `pip install -e .` will also get a good experience.   And yes, choosing `uv` as backend in this case would make it not work on a number of platforms (which is the main discussion point here but suddenly people skip over that) important aspect and ignore it, is a bad choice for us. Full stop.

Surely,`astral` team might choose to do it anyway, there is nothing preventing them from doing it.  And I think none of us can "win" that discussion (some of the people here behave like they really want to `win` it). 

For me the most important here is both myself and @mgorny expresses their "we think it is bad idea". Other people here think it is a "good idea". Each of us has some arguments. And we should leave it as-is. And let Astral team decide their course of action. Neither me, nor @mgorny nor @nathanscain nor @chrisrodrigue nor @vlad-ivanov-name here has a saying in it.

That also reminds us all that `uv` is a proprietary tool of `astral` - open-sourced, yes, but controlled and "owned" by them - where only them make decisions (often very good one and listening to their users). So let us make them decide - because unlke in a number of projects where community and governance rules define how to make decisions, in this case anyone elses is just "an opinion". And i think we all expressed enough of those.

---

_Comment by @nathanscain on 2024-12-21 16:39_

> is a bad choice for us. Full stop.

No dispute there. It is a perfect choice to me and many, many other maintainers. Please don't make the one tool that may solve my problem worse so that you can have a 10th tool that solves yours.

> some of the people here behave like they really want to `win` it

I started with trying to understand the point - now that I believe I do I want to be clear that I strongly believe that changing direction on the build system would be very bad choice. The ecosystem doesn't need another pure python build backend - it does need a fast one. If that fast one has limited application for certain projects, they don't have to use it.

> That also reminds us all that uv is a proprietary tool of astral

That is not what a proprietary tool is - uv is open source. full stop. Astral acting as a maintainer - choosing what goes in and what doesn't - doesn't suddenly change the license it is released under. There is a not a maintainer of OSS who would hand over full control of the projects direction. The PSF manages CPython, owns all the trademarks, and controls every decision that is made regardless of how many users want or don't want a particular choice.

---

_Comment by @potiuk on 2024-12-21 17:30_

> No dispute there. It is a perfect choice to me and many, many other maintainers.

Sure. Similarly the point we present is important to many, many maintainers. I have no quantitative data whos "many, many" is bigger. I am not trying to compete on "whos side is bigger" - just wanted to show there are different opinions and arguments. 

> That is not what a proprietary tool is - uv is open source.

I never claimed it is not open source (I actually even wrote it). And proprietary is not "non-open source". There are various "open-core" projects which are technically "open source", yet they are proprietary. Some of those projects changed licences recently and they are not "technically open source" any more (according to OSI definition of Open Source). 

> The PSF manages CPython, owns all the trademarks, and controls every decision that is made regardless of how many users want or don't want a particular choice.

I just wanted to express one difference - it is how decisions are made (aka governance). Decisions for ASF and PSF are made according to governance rules, voting, people who are contributing have voting rights on decision making. That's what I wanted say as "proprietary" when it comes to decision making. In PSF, ASF, when you are committer and express your opinion, it might turn into a vote. In case of `uv` it might not (at least for now). Also neither PSF nor ASF can change their licences to not be open-source (without changing their basic charter / stop being a foundation).

That's what I wanted to make a note of. That so far no-one in the recent round of discussion here has anything else than "opinion".  Unlike many discussions in ASF/PSF projects where people can also have "vote".

---

_Comment by @chrisrodrigue on 2024-12-21 17:35_

> For me the most important here is both myself and @mgorny expresses their "we think it is bad idea". Other people here think it is a "good idea". Each of us has some arguments. And we should leave it as-is. And let Astral team decide their course of action. Neither me, nor @mgorny nor @nathanscain nor @chrisrodrigue nor @vlad-ivanov-name here has a saying in it.

I agree with you @potiuk. It is easy for popular issue threads to derail into less useful argument rehashing, which lowers their signal to noise ratio. Maintainers then need to sift through many more comments to pick out the key observations or ideas. I think Astral knows what is best for their tool and they have done a great job.

(GitHub Discussions could be an option. From [GitHub Discussions](https://docs.github.com/en/discussions):

>GitHub Discussions is a collaborative communication forum for the community around an open source or internal project. Community members can ask and answer questions, share updates, have open-ended conversations, and **follow along on decisions affecting the community's way of working.**

But Discussions might be less visible overall and the decision to enable them is up to Astral.)

---

_Comment by @astrojuanlu on 2024-12-21 21:40_

> Exactly this. My internal packages are built to target RHEL and Windows - both x86_64 and almost all are pure python. I just want my editable install to not have to spend 10 seconds talking to setuptools to calculate my build metadata every time I invalidate the cache by changing the pyproject.toml or dynamic version. I’m willing to sacrifice it ever working on a nonexistent gentoo system for that to be milliseconds instead.

And that's fair decision to make _after having analysed the consequences_ and weighted pros and cons. The reality is that most Python users don't understand how packages are built, and as such, defaults matter a lot. Repeating what @potiuk and @mgorny have said - what _frontend_ tool package authors use doesn't affect downstream consumers and distributors, but _backend_ choice does. There are very valid concerns about the consequences of package authors mindlessly using non-Python build backends. The implications of `uv init` setting a non-Python, unversioned, "full stack" package as a build backend _by default_ are big and should be carefully assessed.

---

In addition, what will be the scope of such build backend? Many backends exist because of the many use cases that need to be supported. Or is the idea to eventually engulf the capabilities of hatchling, maturin (rustc), scikit-build-core (CMake), meson-python (Meson), even sphinx-theme-builder (Sphinx)? This might seem like a silly question but people will ask on this issue tracker. I'm sure the Astral team agrees.

The most likely outcome for a long time is that if folks that need any of these extra capabilities, they won't be able to use uv's build backend anyway. With that in mind, the point around better error messages laid out in https://github.com/astral-sh/uv/issues/3957#issuecomment-2519002760 becomes much less interesting (although it will of course serve a large majority of users that just want a default choice that works for pure Python packages).

---

With all of this in mind, I think it would be good to have some insight into what the plans are, and what does the Astral team think about the consequences of flipping the switch and making `uv init` sneakily introduce a non-Python build dependency.

---

_Comment by @daviewales on 2024-12-21 21:45_

There is another possibile approach for a build backend which is both fast and portable.

You could write it in standard C.

I'm aware that Astral are all in on Rust, and that Rust has many advantages.

But perhaps for a minimal uv-build-core, C might be a better choice.

---

_Comment by @hauntsaninja on 2024-12-21 22:18_

Maybe one concrete thing is there seems enough potential desire for a more minimal backend that `uv` should retain optionality here.

That is, instead of:
```
[build-system]
requires = ["uv"]
```
uv recommends users do:
```
[build-system]
requires = ["uv-build"]
```

This distinction should be familiar to users of poetry / hatch / flit

uv-build can currently just shim around uv. In the glorious future when all of Python packaging is uv based it becomes much simpler for uv to incrementally make their backend more portable / lighter / low MSRV / pure Python / whatever

---

_Comment by @bmwiedemann on 2024-12-22 04:01_

@nathanscain @potiuk : On the discussion on "proprietary" vs "open" , let me add https://openinfra.dev/four-opens/

As the current license is Apache or MIT, it is clearly [Free/Libre/Open Source Software](https://opensource.org/osd). However, that is only one of the abovementioned "four opens" that people appreciate about the openness of projects.

---

_Comment by @eli-schwartz on 2024-12-22 06:12_

I have similar concerns about this topic, in that I too believe it's a bad idea to implement a custom build backend, but apparently for quite different reasons... I think a lot of fairly unproductive things have been said so far that aren't really helping the discussion?

I think we can all agree that uv has a good reason to want to improve the user experience for uv users, including by making wheel builds faster, and that isn't gonna happen by constantly shelling out to a python process that is doing fairly unoriginal things. Saying "no, you're hurting the non-rust users" isn't really constructive and isn't going to convince the people that want uv to build wheels that uv should stop thinking about building wheels.

That being said, I'm not convinced that the current experiment is actually an effective way of improving the experience for uv users either. Much of the world uses build backends that aren't uv, and much of the world doesn't even use uv at all :) and it would actually be super nice if uv could improve the experience for dependencies as well as for native uv projects. Additionally, requiring users to learn even more things is... inefficient. It is a lovely experience in...

![](https://imgs.xkcd.com/comics/standards.png)

Python is uniquely unusual in programming languages IMO, in that people talk a lot about standards and how python has so many use cases as a glue language that we need to cover all of them, then immediately turning around and implementing the same build system a dozen times over with no meaningful differentiation between the lot. Can anyone actually explain to me the meaningful difference as a prospective project creator writing a pure-python project (the thing that uv is trying to help out with!), between:

- flit_core
- poetry_core
- pdm-backend (with no python-based hooks)
- hatchling (with no python-based hooks)
- setuptools (with automatic src/ discovery)
- uv (experimental backend)

How many new backends do we need that all do the exact same thing but with different `tool.*` sections to describe how to include all python files from src/? This knowledge is not optimally transferable. And when building packages you have to have a bunch of different build backends installed for frankly not compelling reasons.

ruff and uv didn't just invent novel new workflows for the fun of it. They built on prior art, and a core goal was to have close to drop-in compatibility (reusing existing lint error codes, `uv pip` adhering closely to the existing command-line interface of existing tools) to ease the process of familiarizing yourself with a new tool. I guess I was kind of expecting by default that uv would do the same thing with build backends, and it surprised me to be told that this was exactly the opposite of what uv is experimenting with.

So: why doesn't uv implement a drop-in compatible implementation of flit_core in pure rust, which builds a wheel exactly like `flit_core >=3.2,<4` does? Consider that flit_core was already designed to make the simple things obvious and straightforward (it is rather cargo-like in this respect! mandatory layout, packaging-by-convention, reduces the *pointless decisions you need to make*). And its usage guidelines already strongly advise you to pin flit_core to `<4` precisely because that's what it is promising will always be compatible with the high-level and easy to reimplement documentation on how flit_core works today. People can use an existing high-quality low-annoyance build backend which is widely acclaimed for reducing nonsense, and uv just magically starts handling it *faster* for you. Moreover, a large chunk of existing projects which don't use uv and which you happen to depend on can simply use uv under the hood to build themselves.

People who have much more complex needs including code generation or building native extensions cannot use the proposed "uv" backend anyway, and handling this use case robustly will require significantly greater work and be a constant source of issues. It's not actually a great investment though, because:
- those projects already build slowly, forking out to GCC / clang / rustc is MUCH slower than fork+exec to another build system
- build options complexity! (some light reading: https://github.com/numpy/numpy/blob/main/meson.options)
- one of the main reasons to build a native extension is because you're doing something that can't reasonably be expected to be done on the average machine of the average `uv pip install` user. Building complex C/C++ dependencies is a hassle even when you aren't doing Fortran (a compiler for which may or may not exist on any given machine, even when a C++ compiler exists) or building on Windows (expecting pip install users to have Visual Studio installed is... heh), and those are just the tip of the iceberg as described at https://pypackaging-native.github.io/

But, handling the 99% case of projects that just have pure python code and aren't doing anything fancy, can be handled by standardizing on the simple easy obvious solution, which already exists and is called flit_core. All uv needs to do really, is make that blazing fast.

(Stretch goal: learn to read the configurations for hatchling, poetry, and pdm for cases where no external build hooks are done, and optimize those too.)

What, precisely, does implementing "yet another backend" accomplish? It is not necessary to reap the benefits of increased speed, so is there another benefit instead? Are there unique features that the "uv" build backend would have that no other backend can do? As far as I can tell from e.g. https://github.com/astral-sh/uv/issues/8779, the only differentiating factor so far from "just use flit_core" is support for sysconfig "scheme" paths for non-python content such as:
- headers, unnecessary unless you also intend to support compiled projects?
- ~~data, extracted raw to sys.prefix and comes with "WARNING: may destroy existing files"~~ actually flit supports this since 3.7, TIL
- scripts, primarily useful for compiled binaries that cannot just be entrypoints, warned against?
- purelib and platlib, no valid use cases for pure python projects, few use cases for native extensions as depending on module layout you may need to have all .py files alongside the extensions in order to correctly import anything


So in effect, no use cases other than `headers`, and you can basically do that with flit_core's support for "data" by sticking it in data/include/.

I'm curious to hear if anyone else has ideas about something that can't be solved by simply making uv support flit_core, except fast?

---

_Comment by @potiuk on 2024-12-22 08:17_

> I'm curious to hear if anyone else has ideas about something that can't be solved by simply making uv support flit_core, except fast?

That is the same issue as we mentioned with @mgorny  - unfortunately - it skips over the fact that you won't be able to contribute to such projects that will choose the blazing fast backend, on a number of platforms / architectures, and on platforms that always build things fro sources - like gentoo - it's doubly problematic to use compiled, non-pure python backend.

And yes - flit and hatchling are great examples of why we have "different' backends and why it makes sense to deliberately choose some backends and not have 'one to rule them all". 

In case of Airlfow we - deliberately - chose flit for all our providers, because it has no other dependencies (which for example makes it easy to do reproducible builds which we care from the security point of view). Also it is  part of PyPA (i.e. governance of the Python Software Foundation) - and in the Apache Software Foundation we very much care about open-governance not only open-source licencing. Governance and Licencing are two different things and we simply care about both.

Also we chose `hatchling` for Airflow as backend because - while it has some - sometimes problematic for reproducibility - dependencies, we needed dynamic python build hooks that are easy to use and debug - following the https://peps.python.org/pep-0517/ (and few other PEPs) standard. We also even added some features to dependabot recently to allow us to keep build-reproducibility in an easier way in this case (because in order to achieve build reproducibility you have to pin all your build dependencies).

And your XKCD reference is pretty much contradicting your own statement.  For build backends we actually have a very good set of standards https://peps.python.org/topic/packaging/, this is the result of fantasic job done by the PyPA packaging team that they were able to come up (over the last 4 years) a set of standard for python packaging (especially the front-end/back-end separation with a very well laid reasoning and explaination why it is needed). So we do not have "14 standards" here - we have "X implementations of the same standards" - each with different properties, focus on different things. Like flit and hatchling - the fact that I can choose (as maintainer) different backend tool depending on my needs is cool and this is something i really appreciate how well thought and discussed the whole frontent/backend separation was. It is the result not a single github issue discussion, it's a reasult of years of dicussions, analysing hundreds of thousands of different cases in the wild and deliberate implementation of the standards.

If you have not read the PEPs, I heartily recommend it. I spent better part of last year Xmas break on going through all the PEPs and getting to understand all the different cases and consequences of the choices and "one implementation to rule all backends" is not something easily achievable. I see no problem with `uv` delivering another backend there of course - if it has some good properties different than others - as long as (what @mgorny wrote) it's something that is going to be small and portable, then it could even be default backend with `uv init` (but it would be great if other backends are also possible to be chosen by `uv init`). 

BTW. Just a comment here - for me the basic rule of optimisation is to optimise what makes a difference. Flit is already really fast when it comes to building packages. When we moved from the old "setup.py" aproach to flit (and we prepare sometimes 90+ packages in Airflow), the time to build the packages went down to merely seconds from minutes (all of the 90 packages). I think "fast" in this case as a goal is simply chasing a red-herring.

---

_Comment by @eli-schwartz on 2024-12-22 08:53_

> That is the same issue as we mentioned with [@mgorny](https://github.com/mgorny) - unfortunately - it skips over the fact that you won't be able to contribute to such projects that will choose the blazing fast backend, on a number of platforms / architectures, and on platforms that always build things fro sources - like gentoo - it's doubly problematic to use compiled, non-pure python backend.

You seem to have missed (how?) the part where I suggested that uv should implement an internal fast path for `flit_core.buildapi`, and that it should NOT implement `uv.buildapi`. Hence per definition you will be able to contribute to such projects as they use flit?

> Also we chose `hatchling` for Airflow as backend because - while it has some - sometimes problematic for reproducibility - dependencies, we needed dynamic python build hooks that are easy to use and debug

which are *optional* in hatchling, and that was my entire point. If you do NOT use dynamic build hooks (many, many hatchling projects do not use dynamic build hooks), then what is the difference between hatchling and flit?

More importantly, what is the difference between *uv backend* and flit, given the experimental uv backend doesn't support dynamic build hooks and therefore wouldn't be an alternative to hatchling for Airflow?



> And your XKCD reference is pretty much contradicting your own statement. For build backends we actually have a very good set of standards https://peps.python.org/topic/packaging/, this is the result of fantasic job done by the PyPA packaging team that they were able to come up (over the last 4 years) a set of standard for python packaging (especially the front-end/back-end separation with a very well laid reasoning and explaination why it is needed). So we do not have "14 standards" here - we have "X implementations of the same standards" - each with different properties, focus on different things.

I think perhaps that you do not understand what XKCD considers to be the definition of a standard. Outside of the narrow python community, having "14 competing implementations of the same standard, each with different properties and focus on different things" means you don't, in fact, have a standard, you have...

... 14 competing standards for how to build packages. 14 competing standards for the parts not covered by PEP 621.

If it was actually "X implementations of the same standard" then I would be able to run `python -c 'from setuptools.build_meta import build_wheel; build_wheel()'` to build a hatchling project since they are both implementations of the same standard.

I'd like to see `uv build` be able to produce wheels from a project that uses `flit_core.buildapi` due to uv implementing flit_core.buildapi. That would be a real standard.

> Like flit and hatchling - the fact that I can choose (as maintainer) different backend tool depending on my needs is cool and this is something i really appreciate how well thought and discussed the whole frontent/backend separation was. It is the result not a single github issue discussion, it's a reasult of years of dicussions, analysing hundreds of thousands of different cases in the wild and deliberate implementation of the standards.

I think having genuine, meaningful choice about using different backend tools depending on your needs is great! I just don't think people are making meaningful choices. Again, you seem to have missed the part of my comment where I talked about having a dozen different spellings for "I would like to package every file in src/ -- that's my package, you know".

Choosing between flit_core and maturin is a pretty meaningful choice. Choosing between flit_core and meson-python is a meaningful choice. Choosing between flit_core and hatchling's dynamic build hooks is a meaningful choice.

I don't really see how this is a meaningful choice:

```toml
[build-system]
requires = ["flit_core >=3.2,<4"]
build-backend = "flit_core.buildapi"
```

vs

```toml
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"

[tool.uv.build-backend]
module-root = "src"  # this is the default
```



> If you have not read the PEPs, I heartily recommend it. I spent better part of last year Xmas break on going through all the PEPs and getting to understand all the different cases and consequences of the choices and "one implementation to rule all backends" is not something easily achievable.

Thanks. I've read them. :) I also am one of the maintainers of the bottom half of another build backend (meson -> meson-python) which incidentally is another way of saying I'm one of the maintainers of a popular and well-regarded C/C++/D/Fortran/Java/Rust/Swift build system. I am not a newbie when it comes to build systems, and I'm more than usually aware of the challenges of having a single build system that covers all possible diverse use cases.



> I see no problem with `uv` delivering another backend there of course - if it has some good properties different than others

That is ***exactly what I am asking***. My question that you are responding to was a comparison to flit_core, asking what the benefits of using uv versus flit_core are.

Since you appear to be positive that there are benefits, can you please explain those benefits? Thanks.

---

_Comment by @potiuk on 2024-12-22 08:56_

> I'd like to see uv build be able to produce wheels from a project that uses flit_core.buildapi due to uv implementing flit_core.buildapi. That would be a real standard.


This is how it works now (`uv build producing wheels from a project that uses flit_core.bulldapi`). Without having to implement flit_core.buildapi or using uv as backend. This is what the  (single) standard is all about.

---

_Comment by @potiuk on 2024-12-22 09:01_

> That is exactly what I am asking. My question that you are responding to was a comparison to flit_core, asking what the benefits of using uv versus flit_core are.

> Since you appear to be positive that there are benefits, can you please explain those benefits? Thanks.

I don't know. I don't know what (possible) future portable and small non-experimental uv backend will have. Maybe it will be way faster (though hard to imagine it's noticeable if flit is able to build packages under 0.2 s usually). Maybe it will be even easier to have build reproducibility without having to externally provide EPOCH date variable? Not sure. I am not sure what `meson` provides, but maybe you can explain why `meson` is different than others and why you implemented it? What are the differentiating factors?

(I just never heard of meson, so I am genuinly interested)

---

_Comment by @eli-schwartz on 2024-12-22 09:27_

> I am not sure what `meson` provides, but maybe you can explain why `meson` is different than others and why you implemented it? What are the differentiating factors?
> 
> (I just never heard of meson, so I am genuinly interested)

It is the build system used by scipy, numpy, pandas, etc -- all projects which use a lot of C / C++, and in some cases also a lot of Fortran and Cython. It is also heavily used outside of the python ecosystem -- on any linux system, your /sbin/init (systemd, openrc), most of your graphics stack (libdrm, mesa, libglvnd, Xorg-server, wayland, wlroots), and probably desktop environment (most of the GNOME desktop) use it. Much of the hard work was already done, so getting it to support `Python.h` didn't take much effort and meant that projects which have any native extensions are able to use a robust and battle-tested solution.[^1]

scikit-build-core is the other major option for compiled C/C++/Fortran extensions, and it is based on cmake instead.

I really see very little point in reimplementing the complexities of native extension compilation when meson and cmake exist and can be easily used as python build-backends. For sure it's a shame if one goes to all the effort to get cross-platform C/C++ compilation with proper support for various DLL / .so dependencies working, if the resulting effort isn't even broadly usable outside the python ecosystem. In fact, that was one of the major issues with setuptools -- people felt hemmed in by setuptools' native extension building shortcomings, but it was the only build system `pip install` knew how to run.


[^1]: Meson can also be used for pure python projects, using a 3-line build config including imports. But it's probably overkill. 

---

_Comment by @vlad-ivanov-name on 2024-12-22 09:31_

There's one more point, which admittedly is a personal view, but I feel like it's worth sharing nonetheless. `uv` is not written in Python, and that, for me, is a good thing. I admit that I haven't tried `flit`, but I have had enough experience with setuptools, poetry, etc, to be sick and tired of the level of reliability that Python ecosystem provides. Rust's type system and error handling setup that forces developers to think about errors and typechecks increases overall reliability of the result. Personally, I don't even care that much that uv build backend will be faster; I do care very much that it will be one package, with mostly Rust code, with locked dependencies, where none of the Python's flakiness and bad design decisions creating errors at runtime are present.

Of course, you can write bad code in Rust too, but I wouldn't call uv code base bad or low quality by any means. Again, this is my personal opinion, but you can't write reliable software without types, and unfortunately Python ecosystem is largely free-for-all in that regard, and achieving proper type checking in production is difficult and riddled with caveats.

So I wan't uv's _reliability_ and resulting UX improvements applied to build backends, even if it's twice as slow as whatever else is out there.

---

Another thing I noticed in this thread is that there's a lot of sentiment along the lines "the users are inexperienced, we know better, so we will make the choice for them". How about we let users decide what to use? If you don't want to use uv as a build backend, don't use it; if you're a package maintainer, boycott all packages that use it as a build backend, by all means; but let others do what they want.

---

_Comment by @notatallshaw on 2024-12-22 10:10_

> ruff and uv didn't just invent novel new workflows for the fun of it. They built on prior art, and a core goal was to have close to drop-in compatibility (reusing existing lint error codes, `uv pip` adhering closely to the existing command-line interface of existing tools) to ease the process of familiarizing yourself with a new tool. I guess I was kind of expecting by default that uv would do the same thing with build backends, and it surprised me to be told that this was exactly the opposite of what uv is experimenting with.

I don't know astrals plans, but one thing I would point out is ruff and uv have been very consistent on not reading configuration from other tools.

While both tools have copied the language to other tools, they haven't ever tried to pretend they are literally the same tool. Even `uv pip` doesnt try to take the alias `pip`, read pip configuration, nor make the exact same design choices.

> So: why doesn't uv implement a drop-in compatible implementation of flit_core in pure rust, which builds a wheel exactly like `flit_core >=3.2,<4` does?

I think you're advocating that when uv sees flit_core specified it implements its own flit fast path, reading flit's tool configuration?

I think I, and others, would find it uncomfortable for uv to read and insert itself into another tools configuration. While flit is simple and stable, it is not a standard, and descrepencies between uv and flit could easily arise. Effectively leading to two tools fighting over what the same configuration means, even if uv never intended that.

For this goal, I would much rather see the development of a PEP which defined non-dynamic build configuration, then the front end installer could choose a default backend without the user needing to specify one. And users would only need to choose a beckend once they needed advanced features.

---

_Comment by @astrojuanlu on 2024-12-22 10:29_

> Can anyone actually explain to me the meaningful difference as a prospective project creator writing a pure-python project (the thing that uv is trying to help out with!), between:
> 
> - flit_core
> - poetry_core
> - pdm-backend (with no python-based hooks)
> - hatchling (with no python-based hooks)
> - setuptools (with automatic src/ discovery)
> - uv (experimental backend)

This is a bit off-topic I think, but worth discussing. I resurrected an old thread that touches on a related topic https://discuss.python.org/t/dependencies-for-build-backends-and-debundling-issues/22840/36

---

_Comment by @potiuk on 2024-12-22 11:00_

> It is the build system used by scipy, numpy, pandas, etc -- all projects which use a lot of C / C++, and in some cases also a lot of Fortran and Cython. It is also heavily used outside of the python ecosystem -- on any linux system, your /sbin/init (systemd, openrc), most of your graphics stack (libdrm, mesa, libglvnd, Xorg-server, wayland, wlroots), and probably desktop environment (most of the GNOME desktop) use it

I see. Great. Thanks! TIL. 

Yes in this case using `meson` makes perfect sense - because it likely works on very same platforms that the projects that use it need to contribute to them. Which is exactly the point why different backends are a "good" thing. Need simple thing -> go with `flit`, need build-hooks -> go `hatchling`, need c/Rust integration - > go `meson` and so on...

I think what some people like me are really afraid of here, is that `uv` has now a power to drag people along with `uv init` - due to it's popularity, and making things `c/rust - limitation (and more) - bound` - including limiting the number of platforms it will run on.

This might cause problems to contributors (which I am worried about) - and distributors (which @mgorny is worried about). SImply build back-ends (unlike build front-ends) are "fixed" by the project and they have different "actors" that rely on them: maintainers, contributors, 3rd-party distrobutors that repackage python projects for their distributions and so on. And the whole discussion here is to stress that it would be great if `astral` team takes that all potential uses into account when deciding on releasing their build backend, because it might have ripple effects.

I understand that some people want "fast" build backend because they are only looking at it from "maintainer" point of view -  but build backend impacts more than one "actor type" in the ecosystem and decisions on implementing a backend for such a popular tool like `uv` should IMHO look holistically at all those use cases and make the right choices (and I know they are listening, even if not actively participating, and I know them personally, so I am quite sure they will make the right decisions).

> This is a bit off-topic I think, but worth discussing. I resurrected an old thread that touches on a related topic https://discuss.python.org/t/dependencies-for-build-backends-and-debundling-issues/22840/36

Definitely worth it. I commented there as well. That would indeed be a good idea to have "default" simple backend, somewhat "blessed" by the PSF/Packaging and easily available as default option to serve 9x% of needs of all the package creators (again small and portable would be a very important property of it). Not sure if stdlib is the right place for it - because of necessary python-version limitations - but the idea of having a "reference" build backend is a very good one.

Then `uv init` could use it as default, but also it could do `uv init --use-backend uv`, `uv init --use-backend flit` etc. - this would be IMHO the best thing for the ecosystem.

---

_Comment by @eli-schwartz on 2024-12-22 12:02_

Some side topics which were discussed above confuse me greatly:

> It's interesting that `setuptools` remains the "blessed" build backend that `pip` defaults to use when no backend is specified in the `pyproject.toml`. Rather than bake `setuptools` into the stdlib, it silently forces the download and installation of it as an unauthorized third-party build dependency. This also seems to violate the principle of "explicit is better than implicit."


> FYI, I beleive this is because pip maintainers are very conservative when it comes to breaking changes, not because it is the intended future of Python packaging.
> 
> For example, the old resolver, which can easily install a broken environment, is still available to use even though the new resolver has been available for over 5 years and turned on by default for over 4 years.

Both the question and the answer feel quite wrong. A build backend is MANDATORY in pyproject.toml in order to utilize the PEP 517 specification of build backends, because if you don't specify a build backend then you are not in fact using PEP 517 at all. If you don't use PEP 517, then the only remaining option is setuptools since all other options came after PEP 517.

It is logically impossible to ever change the fact that setuptools is used as the "I predate PEP 517" assumption. It is quite explicit, nothing implicit at all, and it's not a matter of conservatism. It is quite simply and straightforwardly encoded in PEP 517 itself:

> If the `pyproject.toml` file is absent, or the `build-backend` key is missing, the source tree is not using this specification, and tools should revert to the legacy behaviour of running `setup.py` (either directly, or by implicitly invoking the `setuptools.build_meta:__legacy__` backend).

What is the possible use case for any tool (uv or otherwise) to determine that projects which ***do not use the pyproject.toml specification*** are using an arbitrary tool-specific backend? What is the purpose of suggesting that uv should use the "uv" backend in a situation where PEP 517 is not implemented? This is the precise opposite of interoperability.

Is this some kind of marketing thing, where the goal is to "put down" setuptools, because otherwise I'm not sure why it would be necessary to talk about anything being "blessed" rather than the correct description which is literally "deprecated".

> A uv-aware build backend would enable private git packages to depend on other private git packages, and still be installed with `pip`, `pipx`, `poetry`, etc, without needing the package end user to change their tools. See [#7069 (comment)](https://github.com/astral-sh/uv/issues/7069#issuecomment-2332944182) for a detailed example. (Poetry packages work this way, as they use the poetry-core build backend.)

Poetry enables this by having its own non PEP 621 metadata, since [their proposal](https://peps.python.org/pep-0633/) for how to describe dependencies in pyproject.toml was rejected. What you're asking is for the rejected proposal, I guess.

You can of course implement the same style anyway, which also means you are NOT compatible with PEP 621, and you MUST use dynamic metadata. I guess the idea is, you can specify the VCS sources alongside the dependency itself rather than have a separate `tool.*` table describing optional VCS mappings. Why is that a big deal in the first place, hmm...

As a mechanism for pip / pipx / poetry etc to depend on other private git packages, I don't understand how using an alternative build backend is supposed to help. Build backends cannot *install* anything for you, they just... build wheels. pip installs the wheel and its dependencies, and using a uv-specific description of private git packages will not work if pip doesn't know how to install it.

The thing is it actually does work, but only because poetry translates this as equal to the PEP-defined dependency specification:

```toml
[tool.poetry.dependencies]
package_one = {git = "https://github.com/organisation/package-one-repository.git"}


### is actually

[project]
dependencies = [
    "package_one @ git+https://github.com/organisation/package-one-repository.git"
]
```

You can look in the wheel that poetry produces, you will see that the latter, PEP-described dependency string is the one in the wheel METADATA file:

```
Requires-Dist: package_one @ git+https://github.com/organisation/package-one-repository.git
```

You can use PEP 621 "[project] dependencies" metadata today using any build backend you want -- setuptools, flit, you name it -- with this trick, and:
- enable private git packages to depend on other private git packages
- still be installed with `pip`, `pipx`, `poetry`, etc, without needing the package end user to change their tools

```
python -m build --wheel && pip install dist/*.whl
```

you will see that pip utilizes this standards-conformant wheel metadata to install private git packages without any trouble.

You do not need to create a whole new build backend for this! The standards allow you to do it with ANY build backend. The foundational premise of needing a special poetry feature to do this is 100% a misunderstanding.

---

_Comment by @eli-schwartz on 2024-12-22 12:04_

> I think you're advocating that when uv sees flit_core specified it implements its own flit fast path, reading flit's tool configuration?
> 
> I think I, and others, would find it uncomfortable for uv to read and insert itself into another tools configuration. While flit is simple and stable, it is not a standard, and descrepencies between uv and flit could easily arise. Effectively leading to two tools fighting over what the same configuration means, even if uv never intended that.

It's genuinely not hard in the slightest. In fact, it's fully 100% unquestionably possible to solve this in a way that mathematically guarantees discrepancies are impossible.

Rewrite flit_core in rust. Transcribe its algorithms from one programming language to another.

As you pointed out, flit_core is simple and stable, so it is... not even a significant effort to transcribe its algorithms, as there isn't a whole lot to transcribe.

(And if uv didn't intend to fight over what the same configuration means, there's a surefire way to not do so. It's called, don't fight. Accept that the algorithmic purity of the flit_core codebase is the sole authority on how flit_core.buildapi as a backend works. Actually, since that's the original premise in "transcribe the algorithms from one programming language to another" there will never be a discrepancy, nor a fight.)

The only possible issue that could ever arise is if flit_core changes its behavior and uv is slow to adapt. This is exactly equivalent to people building a wheel a week ago and using it today -- unless the pyproject.toml adds a build-requires lower bounds pin that is incompatible with the version that uv transcribed and implemented and claims to expose, it is a valid interpretation to use an older wheel of flit_core to build a wheel, and under various conditions `pip install` will constrain itself in this precise manner.




---

_Comment by @eli-schwartz on 2024-12-22 12:14_

> Definitely worth it. I commented there as well. That would indeed be a good idea to have "default" simple backend, somewhat "blessed" by the PSF/Packaging and easily available as default option to serve 9x% of needs of all the package creators (again small and portable would be a very important property of it). Not sure if stdlib is the right place for it - because of necessary python-version limitations - but the idea of having a "reference" build backend is a very good one.

I think it would be great to have a default simple backend somewhat blessed and easily available as a default option. Unfortunately, that was called "setuptools" -- or rather, "distutils" -- and it was removed on the specific grounds of not wanting to "choose sides". :(

distutils was never a great design, most likely since it was never actually designed -- it grew organically out of the needs of CPython's own internal native extension building. It had one major advantage, which is that it was in the stdlib so it was always available, on any OS platform, and didn't need to be bootstrapped.

python-version limitations are quite frankly utterly meaningless. It's the wheel binary package format that matters here, and that is stable by necessity since existing wheels must always be installable by any future version of python (absent `Requires-Python` limitations).

I doubt anyone will actually agree to add a default simple backend due to politics. But I'd settle for not making the landscape *worse* by way of being *more confusing*. That means people should refrain from adding new build backends into the world for *any* reason, until and unless they can write a convincing pitch on why they themselves feel their backend is contributing new functionality into the world and the existing solutions simply cannot be made to work.

Creating a new backend is inherently disruptive, in the negative sense rather than in the ruff sense. It should only be done with clear rationales and after considering and documenting the pros/cons.

---

_Comment by @pawamoy on 2024-12-22 14:38_

As a Python-project maintainer, I'm so happy and grateful that people like @mgorny spend time to make my projects available to a wide range of non-dev users, across various systems (like Gentoo). It's easy for me (a Python dev), to forget that not all users of my projects are developers and don't even know what `pip` is. I want to make @mgorny's volunteer work easier. If that means I can't use uv's backend, I won't use it 🙂

On the other hand, open-source maintainers should obviously never be pressured by users (through re-packagers/distributors): if I don't care about Gentoo users, I'll just use whatever build backend I want that is incompatible with Gentoo's way of building from source, and that's fine (even though it could break Gentoo's builds, forcing them to use an older version that worked). This just needs to be **an informed decision**. For that, Astral/uv will need great communication and documentation (which they generally have)!

I suspect however that this will only prevent *a portion* of the investigation work @mgorny will have to do when some projects suddenly switch to uv's backend. Unfortunately I don't see any way out of it 😕 I guess these concerns just needed to be expressed, so that Astral can take them into consideration for their comms/docs 🤞 

P.S.: @mgorny sorry for tagging you 4 times 😂 

---

_Comment by @rossburton on 2024-12-22 14:45_

As a maintainer of the Python ecosystem in Yocto Project/OpenEmbedded, I endorse everything that has said by @eli-schwartz @mgorny @Conan-Kudo: we _always_ build from sdists and really want build tools that are pure Python. We also cross-compile and support riscv/mips/ppc.  Yes, we have actual users using those platforms _in production_.

---

_Comment by @mgorny on 2024-12-22 15:08_

I think people really need to take the holistic approach here. When a package gets published, other people start depending on it. At this point, your decisions start impacting others, and while of course you have no formal obligations to them, I believe you should take responsibility for what you're about to do. Your package eventually becomes entangled in a complex web of interdependencies.

The vast majority of the packages I'm maintaining for Gentoo aren't the packages that I personally wanted to use it, or Gentoo users requested. They are dependencies of packages we wanted, or dependencies of their dependencies, and this goes multiple levels deep. Your package is probably in there somewhere. And please don't take me wrong, I don't want to diminish people's work, but it is there not because I wanted it there, or because our users wanted it there, but because some person used it in some project that was used in another project…

From my perspective, if you make a decision that makes it impossible for us to package your project anymore, it's no longer a case of "sorry, we no longer provide package X". It's a case of "if we removed X, we would have to remove 10, or 30, or 200 other packages". So, in reality, at this point my options include:

1. Kindly asking the people maintaining X to reconsider, explaining the problem to them. Ideally, this works out, they revert the change. Unfortunately, both sides are now frustrated — me that I had to put another fire out, and them because they didn't realize the negative impact of the decision they have been encouraged to do.
2. Kindly asking people maintaining Y that depends on X to consider removing the dependency, because it is no longer suitable for packaging. This is far more intrusive than the previous solution, and far more likely to cause tension.
3. Patching X to use another build backend. This means that we have to maintain the patch forever, and we need to take special care not to accidentally break something. And if we actually break something, users may report the issue to X's upstream, causing at least some confusion.

However, once it really comes to this, it's just trying to patch a dam that's falling apart. Many projects will switch, only some of them will switch back. We won't be able to keep patching things around forever, we won't be able to remove all packages that depend on the new backend. What will really happen is that we) are going to have to maintain an alternative implementation of the uv backend in pure Python. And by "we", I mean a lot of different people for whom uv's backend can't work, and who will either work together or separately create multiple projects serving the same purpose. These projects may be imperfect, they may cause issues and create a lot of tension between different parties.

What I'd really like to ask: is it really worth it? Building a `flit_core`-based packages takes 0.2 s on my machine, and that includes all time needed to call `python -m build` from shell. Are these 0.2 s really the critical bottleneck in your pipelines?

---

_Comment by @nathanscain on 2024-12-22 15:18_

Backtracking a bit to the conversation about the clean break between frontend and backend - I do just want to highlight that the current state already makes this dubious. The maintainer can pick any combination (good), but contributors cannot swap out the frontend when building from source and expect the same wheel to come out.

Case and point: flit will add files tracked by your VCS to the sdist by default... but only when you use their frontend of `flit build`. A contributor not building with the VCS info present or building with a different front end will have any such files excluded from their sdist. Downstream users (distro packagers, gentoo user, etc al) must use the sdist built by the flit frontend to get the files - they can't substitute their own frontend, build from the source code itself stripped of VCS info, and expect to build the same package (even if pulling the same backend).

https://flit.pypa.io/en/stable/pyproject_toml.html#including-files-committed-in-git-hg

To be clear, I'm fully supportive of this. But it does mean that - until you reach the sdist - the build frontend AND backend matter here and they are linked. As a contributor, you already need to adopt both if you wish to contribute - as it should be imo.

Summary: the frontend/backend system is made to protect downstream users of sdist - not contributors who want a different frontend. As such, we should focus the conversation on the sdist->distro side of this as the source->sdist is already widely tied together. 

---

_Comment by @pawamoy on 2024-12-22 15:34_

<details><summary>Kinda off-topic so collapsing.</summary>

> Case in point: flit will add files tracked by your VCS to the sdist by default... but only when you use their frontend of flit build

That seems terrible 🧐  You're telling me that:

```bash
git clone my_repo
cd my_repo
uvx --from build pyproject-build
```

...and:

```bash
git clone my_repo
cd my_repo
uvx flit build
```

...produce different artifacts? What's the point of separating frontend and backend if you put backend logic in the frontend again 😕 

UPDATE: ah, positive note:

> For now, including files from version control is the default for [flit build](https://flit.pypa.io/en/stable/cmdline.html#build-cmd) and [flit publish](https://flit.pypa.io/en/stable/cmdline.html#publish-cmd), and can be disabled with --no-use-vcs. **The default will switch in a future version.**

Emphasis mine.

</details>

---

_Comment by @nathanscain on 2024-12-22 15:39_

I don't use flit much at the moment, but that is my understanding based on this from their docs (if you have non python files tracked by VCS):

> Using `flit_core` as a backend to other tools such as `build` never gets the list of files for the sdist from version control.

This is why uv's frontend provides the option of specifying info to be sent to the backend - allowing a uv frontend / flit backend combo.

The separation is ONLY sdist forward. Development needs to have the frontend and/or backend configured to work with the other.

---

`setuptools` allows a similar behavior but puts it in the backend via the `setuptools_scm` extension that you add as a build dependency. Still requires that you build with SCM data present.

---

_Comment by @notatallshaw on 2024-12-22 15:45_

> It's genuinely not hard in the slightest.

I didn't suggest was hard, I said it could (and would) lead to descrepencies and that users like myself would be uncomfortable with uv co-opting another tool.

Design choices are made, platforms have quirks, flit calls the Python standard library which uv can't, and there are different implementations of the standard library on certain platforms.

There's no algorithm to copy exactly, because there's no standard to follow.

uv doesn't try to exactly copy pip, but I've seen many issues on this repo where people complain uv isn't copying pip's exact bugs. Should uv copy each bug of flit and match every bug release for release? If not there will be descrepencies.

I really believe the only way we can get the outcome you want, a standard for non dynamic building is to build a standard not just say "copy flit", that just leads to another setuptools situations. And that discussion would need to happen at https://discuss.python.org/c/packaging/std/35, it's not on Astral alone to decide what should be a Python packaging standard.

To be clear though, I'm not arguing there aren't issues with a uv backend limiting the platforms pure Python sdists can build on, nor the size of the uv binary given how popular it might become, etc. I just think creating another "copy exactly the popular tool" scenario is moving the Python packaging ecosystem backwards not forwards.

---

_Comment by @pawamoy on 2024-12-22 15:48_

> Using flit_core as a backend to other tools such as build never gets the list of files for the sdist from version control.

This is fine. The recipe for confusion lies in the frontend *enhancing* the backend. Not only it shouldn't be the default (changed in a future version), it just shouldn't be possible.

> This is why uv's frontend provides the option of specifying info to be sent to the backend - allowing a uv frontend / flit backend combo.

Terrible ripple-effect, I would say. So instead of `flit build`, I can now do `uv build`, but still cannot do `pyproject-build` 😖 I already see the matrix of supported/unsupported frontend+backend combinations 😱 

> The separation is ONLY sdist forward. Development needs to have the frontend and/or backend configured to work with the other.

Yup terrible, sorry I don't have other words 😂 In a few projects I use pyproject-build to build packages. Now I know that flit-managed projects probably won't give correct artifacts. I want to go back to not knowing.

> setuptools allows a similar behavior but puts it in the backend via the setuptools_scm extension that you add as a build dependency. Still requires that you build with SCM data present.

Completely fine: it's all in the backend. pdm-backend, depending on the options you use, will also require SCM data. Though it has fallback options. That's an OK requirement IMO. The backend-needs-frontend requirement is not OK.

---

_Comment by @nathanscain on 2024-12-22 15:54_

@notatallshaw I believe a standard for non-compiled builds would be the best outcome for the ecosystem as most packages do nothing special but a dynamic version and maybe some data files. My issue with a uv backend that just reimplements twine is less "co-opting another tool" and more just that it would be unexpected. As you mention, uv pip doesn't equal pip 100% - also ruff format doesn't equal black 100%, etc. Astral typically pulls workflows from these other tools, but isn't really in the business of trying to replicate them 1 to 1.

---

_Comment by @nathanscain on 2024-12-22 15:59_

@pawamony I don't generally disagree - came across it doing research and it seemed relavent.

My point was that worth noting that the frontend sending configuration to the backend *is* a part of the standard and it likely isn't going anywhere. If building from the repo and not the sdist, you either need to configure your frontend to send the same info or use that project's frontend.

> Now I know that flit-managed projects probably won't give correct artifacts.

It will PrObAbLy give the correct artifacts, as data files included by SCM are still a minority case.

Worth noting that there are valid reasons to want to limit files that reach the sdist and not just shove everything in for the backend to sort through.

---

_Comment by @pawamoy on 2024-12-22 16:06_

Yes, thanks @nathanscain, I guess I'm just a bit surprised 🙂 From what I understood reading the standard, that was not the intended use-case. Unless *any other* build frontend can also send these build options to the flit backend, then these are not build options, it's just backend logic hardcoded in the frontend.

> Worth noting that there are valid reasons to want to limit files that reach the sdist and not just shove everything in for the backend to sort through.

Of course, but that should be specified as backend options, not front-end ones 😄 

---

_Comment by @pawamoy on 2024-12-22 16:25_

I checked flit's backend source code, and [it doesn't use the received `config_settings`](https://github.com/pypa/flit/blob/7cc7d330d38d48ef865c07e5086a917278b19050/flit_core/flit_core/buildapi.py#L80). The frontend just [subclasses the `SdistBuilder` class](https://github.com/pypa/flit/blob/7cc7d330d38d48ef865c07e5086a917278b19050/flit/sdist.py#L149) to enhance it. Compare to PDM's backend, which actually [supports build options that can be passed from pyproject-build](https://backend.pdm-project.org/build_config/#build-config-settings). OK learned enough for today, sorry for the spam 😄 

---

_Comment by @nathanscain on 2024-12-22 16:27_

Oh wow... that is unfortunate...

---

_Comment by @eli-schwartz on 2024-12-22 17:10_

> Case and point: flit will add files tracked by your VCS to the sdist by default... but only when you use their frontend of `flit build`. A contributor not building with the VCS info present or building with a different front end will have any such files excluded from their sdist. Downstream users (distro packagers, gentoo user, etc al) must use the sdist built by the flit frontend to get the files - they can't substitute their own frontend, build from the source code itself stripped of VCS info, and expect to build the same package (even if pulling the same backend).

@nathanscain

sdists do not matter for building the project at all. In fact, when using the github archives generated tarball of a project instead of the sdist, you can build with any frontend plus flit_core and get the same files in the wheel and installed to your python environment.

This is already the case today since many projects using hatchling or poetry don't include important files needed to run testsuites in their sdists, since they explicitly `exclude =` those files, so building from Github archives is the only option. Sad but true. Flit hasn't meaningfully changed the needle here.

Again, it doesn't matter whether you use `flit build` or pyproject-build from sdists or github archives or what have you if all you want is to ***build a wheel***. That is actually kind of the point of the flit decision -- the author of flit believes it was wrong for the python ecosystem to ever specify PEP 518 hooks for build_sdist, because building sdists are "the job of a maintainer, not the job of someone who is simply installing the project". It's definitely a... decision. The correct decision would have been to simply not implement build_sdist at all so that you'd have to invoke `flit build` to produce an sdist, rather than have two ways of producing an sdist.

flit's backend hook for producing an sdist produces an sdist that is useless for a number of purposes that people use sdists for, and can only be used for building a wheel. That's fine if what you want is to *build a wheel*.

Note also that flit doesn't support dynamic versions from a *build stage* such as VCS info. For that you need https://pypi.org/project/flit-scm/ (a separate build backend that first runs setuptools-scm to write VCS info to a file and then reimports `flit_core` to get that version info from the `__version__` attribute of your package).


> My point was that worth noting that the frontend sending configuration to the backend _is_ a part of the standard and it likely isn't going anywhere. If building from the repo and not the sdist, you either need to configure your frontend to send the same info or use that project's frontend.

The standard only allows sending *user* configuration to the backend, in the form of pyproject-build's `--config-setting` / `-C` option.

> I checked flit's backend source code, and [it doesn't use the received `config_settings`](https://github.com/pypa/flit/blob/7cc7d330d38d48ef865c07e5086a917278b19050/flit_core/flit_core/buildapi.py#L80). The frontend just [subclasses the `SdistBuilder` class](https://github.com/pypa/flit/blob/7cc7d330d38d48ef865c07e5086a917278b19050/flit/sdist.py#L149) to enhance it. Compare to PDM's backend, which actually [supports build options that can be passed from pyproject-build](https://backend.pdm-project.org/build_config/#build-config-settings). OK learned enough for today, sorry for the spam 😄

Because flit doesn't support build options.

User configuration in the form of build options is a direct analogue to GNU Autotools / meson / cmake support for things like `--with-blas={openblas|netlib|MKL|blis}`, or `--prefix=/usr`, or `--gallium-drivers=kmsro,radeonsi,r300,r600,nouveau,freedreno,swrast,v3d,vc4,etnaviv,tegra,i915,svga,virgl,panfrost,iris,lima,zink,d3d12,asahi,crocus,softpipe,llvmpipe`

Flit doesn't support dynamism or compiled C extensions so config_settings is irrelevant to it.

---

_Comment by @eli-schwartz on 2024-12-22 17:17_

> I didn't suggest was hard, I said it could (and would) lead to descrepencies and that users like myself would be uncomfortable with uv co-opting another tool.
> 
> Design choices are made, platforms have quirks, flit calls the Python standard library which uv can't, and there are different implementations of the standard library on certain platforms.

I don't really understand the purpose of this FUD, sorry. Can you please point to me where you think it's philosophically possible to get different results from porting flit_core into rust code, "because it calls the python standard library which yields differences"? I'm not sure what you are getting at. Are you, I dunno, afraid that python's filesystem tree-walking algorithm for iterating over all files in src/ is going to yield a different set of installable files compared to a rust-based filesystem tree-walking algorithm for iterating over all files in src/?

> There's no algorithm to copy exactly, because there's no standard to follow.

That is not the meaning of the word "algorithm", and that is not the meaning of the word "standard", so it seems we are at an impasse but not the impasse that I expected.

---

_Comment by @pawamoy on 2024-12-22 18:04_

> Flit doesn't support dynamism or compiled C extensions so config_settings is irrelevant to it.

The point was that instead of letting users pass something like `--include-vcs-tracked` to the backend through `config_settings` (thanks to whatever `flit` CLI option), flit's frontend extends the backend, tying them together, not allowing other frontends to pass the same options.

EDIT: well, `flit build` only supports flit's backend anyway, acting as an extended backend, not a frontend. Just like Poetry only supports is own backend I think. PDM did the right thing IMO and properly separated itself from its backend. You can use `pdm build` with any PEP 517 backend.

---

_Comment by @notatallshaw on 2024-12-22 18:10_

> I'm not sure what you are getting at

Trying to identically copy a tool is bad. It leads to ad hoc implied standards, bugs, and descrepencies.

There are a myriad reasons, but I'm firmlymof the opinion the Python packaging system wasn't able to evolve until we were able to move past "do whatever setuptools and pip does". I would be sad for us to move to "do whatever flit does".

> That is not the meaning of the word "algorithm", and that is not the meaning of the word "standard", so it seems we are at an impasse but not the impasse that I expected.

I'm talking about a standard in the context of the Python packaging ecosystem which means an accepted PEP, it is all willing and interested parties coming together to agree how something should be defined for implementation. As we're talking in the context of Python packaging I would suggest we should all be using this term this way, anything else is just going to add confusion.

By algorithm I mean something that you could write out confidentially in psudo code. I think this would be non trivial for flit, least of which because of it's giant regexes which depend on the Python regex engine, as well as, yes, it's various IO interfaces which depend on Python implementations. I agree there are other and more broad usages and definitions of the word.

---

_Comment by @eli-schwartz on 2024-12-22 19:40_

> Trying to identically copy a tool is bad. It leads to ad hoc implied standards, bugs, and descrepencies.
> 
> There are a myriad reasons, but I'm firmlymof the opinion the Python packaging system wasn't able to evolve until we were able to move past "do whatever setuptools and pip does". I would be sad for us to move to "do whatever flit does".

Genuinely baffled.

The reason why the Python packaging system wasn't "able to evolve" except by moving past setuptools is because setuptools does not document what it does or how it works and it supports vast numbers of moving parts. But primarily because it exposes an extensive (and again, effectively undocumented) python API which people code against.

This is a design problem but it's not one that a generic axiom can be derived from. There is no logical reason why another build backend has to leave the majority of its own features undocumented.


(Regarding pip, lots of people moved past "do whatever pip does", way back in 2000 and in some cases earlier. Conda, a latecomer to the party, moved past it in 2012. It doesn't actually matter either way -- pip has never constrained the packaging system, it's only constrained workflow UX which I'm not all that interested in and isn't the topic of this issue really.)

> I'm talking about a standard in the context of the Python packaging ecosystem which means an accepted PEP, it is all willing and interested parties coming together to agree how something should be defined for implementation. As we're talking in the context of Python packaging I would suggest we should all be using this term this way, anything else is just going to add confusion.

I object to your definition of the word standard in the context of the Python packaging ecosystem!

A standard, in the context of the Python packaging ecosystem, is when multiple ways to do the same generic concept exist and a document (referred to by the term "standard") says how tools should expose an interoperability interface for other tools to use.

Fortuitously, that's not specific to the Python packaging ecosystem -- that's also how the term "standard" is used in other fields of endeavor -- the Python packaging ecosystem part is that standards are proposed via the PEP process.

Standards are unrelated to whether a specific program documents its own behavior with enough fidelity to port it to another programming language as necessary. That's not an interoperability topic.

> By algorithm I mean something that you could write out confidentially in psudo code. I think this would be non trivial for flit, least of which because of it's giant regexes which depend on the Python regex engine, as well as, yes, it's various IO interfaces which depend on Python implementations. I agree there are other and more broad usages and definitions of the word.

I'm only aware of two places in which flit_core meaningfully utilizes regular expressions and both of them are PEPs that have been transcribed from pseudocode into python's re module so there is no requirement to depend on the python regex engine.

The tool.flit section doesn't support passing user-defined regular expressions as far as I can tell, but I'd love to hear more if you're aware of something I missed.

Regarding IO interfaces which depend on python implementations, I simply don't believe you at all, and if what you claimed were true then it would be impossible to write software in python ***at all***[^1] because you could never be sure if it behaved according to https://docs.python.org and you would encounter impossible filesystem IO errors that defy your operating system kernel's documentation.

If that's not what you mean then please do explain what you mean when you say that it is not practical for an individual to compare behavior of a python program's usage of IO apis and a rust / C++ program's usage of IO apis and write the same program in multiple programming languages.

[^1]: this would make me very sad as I like writing software in python

---

_Comment by @notatallshaw on 2024-12-22 21:29_

> Genuinely baffled.

Yeah, it seems we don't have enough shared language to communicate on this one, so I'll leave this as my final post to this particular thread, if you're still baffled I don't think me replying any further will help sorry.

> The reason why the Python packaging system wasn't "able to evolve" except by moving past setuptools is because setuptools does not document what it does or how it works and it supports vast numbers of moving parts. But primarily because it exposes an extensive (and again, effectively undocumented) python API which people code against.

Okay, but the way that was "fixed" is by definining standards as PEPs, and getting pip and setuptools to follow them (or close enough), that other tools could successfully interoperate with them.

> Regarding pip, lots of people moved past "do whatever pip does", way back in 2000 and in some cases earlier. Conda, a latecomer to the party, moved past it in 2012. It doesn't actually matter either way -- pip has never constrained the packaging system, it's only constrained workflow UX which I'm not all that interested in and isn't the topic of this issue really

I think your timeline is off, maybe you're thinking of easy install? Pip was first released in 2008.

Conda is an excellent example of why packaging standards were needed here for both frontend and backend. Conda is not part of the Python packaging ecosystem, it is a meta packager that has to keep its own metadata separate from the Python packaging data. In a huge number of cases the build step of a conda package that packages a Python package is to call pip.

If uv had to do what conda did we wouldn't be talking about front ends or back ends or interoperability. I see this as a great success of the Python packaging standards (PEPs and their implementations).

> I object to your definition of the word standard in the context of the Python packaging ecosystem!

I'm a bit confused why because you go on to describe the PEP process:

> A standard, in the context of the Python packaging ecosystem, is when multiple ways to do the same generic concept exist and a document (referred to by the term "standard") says how tools should expose an interoperability interface for other tools to use.

This is the PEP process and the standards documents that a PEP contributes to is at: https://packaging.python.org/en/latest/specifications/

> If that's not what you mean then please do explain what you mean when you say that it is not practical for an individual to compare behavior of a python program's usage of IO apis and a rust / C++ program's usage of IO apis and write the same program in multiple programming languages.

That's not what I said or what I meant, but I genuinely don't think it's worth hashing over between the two of us. 

It only matters if Astral think that 1) creating a flit fast path meets their design goals? And 2) if they agree with you that it's doable to copy flits behavior so exactly that it will not cause issues (at least for them)?

Though I *suspect* the answer to 1) though is no, making hashing over 2) doubly pointless! But we'll see.

---

_Comment by @potiuk on 2024-12-22 21:34_

If anything - the length of this thread indicates how important and how complex the problem is. Which should - I hope - give `astral` team a bit of a pause and do a lot of consideration of what they are going to do .

---

_Comment by @potiuk on 2024-12-22 21:36_

And since holidays are coming - I hope we can all stay with our families over the next few days and get a bit of a pause on all github-related stuff.

Happy Holidays everyone !

---

_Comment by @eli-schwartz on 2024-12-22 22:19_

> I think your timeline is off, maybe you're thinking of easy install? Pip was first released in 2008.

My timeline (2000 for the date when people moved past the need for tools first released in 2008) was highly intentional and could be seen as a wry comment about pip itself. Remember that pip is merely a workflow tool and many people never adopted it in the first place.

It's simply a *bad comparison* to build backends.

> Conda is not part of the Python packaging ecosystem, it is a meta packager that has to keep its own metadata separate from the Python packaging data.

None of that metadata is defined by pip-centered standards, and conda also distributes the metadata from build *backends*, just like pip does. The *additional* metadata that conda tracks but pip doesn't track is metadata that requires packaging and distributing non-python software, which naturally doesn't participate in localized python packaging ecosystem standards and never will (conda installs pure python wheels the same way pip does, and pip doesn't install GCC or boost or OpenBLAS at all but conda does).

The proposed fixes for this discrepancy between conda and pip have been about enhancing metadata for build backends, not build frontends, because it's not a frontend problem!

But this highlights something that bothers me about the python community as a whole, and it is: people discover that software is hard and ISVs actually do serve a useful role, and respond in fear by refusing to talk about or consider making software a better place except where mandated by a PEP standard. Case in point: "no one should reimplement flit_core in rust unless flit_core is described in a PEP standard. Having flit_core documentation is not good enough because it's not official unless it's a PEP, you can't know how to implement flit_core unless it's a PEP".

Which is a pretty fearful way of looking at the world, I have to say.

> I'm a bit confused why because you go on to describe the PEP process:

My apologies for the confusion. In the later two paragraphs I implied that I felt I needed to describe the PEP process because you were *not* describing the PEP process.

> That's not what I said or what I meant, but I genuinely don't think it's worth hashing over between the two of us.

Okay, magic smoke and pixie dust it is.

You originally said "platforms have quirks, flit calls the Python standard library which uv can't, and there are different implementations of the standard library on certain platforms" as a reason for why my suggestion to *port a program from python to rust* is impractical (not create a competing implementation, program using the same configuration, ***port the original program***).

This analysis worries me because I have a use case (not related to uv) for implementing flit_core according to its documentation (but not in rust specifically) and it sounds like you would call the results buggy due to an intrinsic unreliability problem of the python programming language (but I don't see the bug).

---

_Comment by @notatallshaw on 2024-12-22 22:50_

> This analysis worries me because I have a use case (not related to uv) for implementing flit_core according to its documentation (but not in rust specifically) and it sounds like you would call the results buggy due to an intrinsic unreliability problem of the python programming language (but I don't see the bug).

I would be happy to discuss on the relevant repo in detail, I believe enough detail has been discussed here given no one for Astral has participated in a while, especially on their design goals.

It could work quite well for your needs, which I think are likely different from uv's ("just works" for millions of users and configurations and platforms and impacts a significant % of the packaging ecosystem).

Bluntly said though, I don't appreciate the magic smoke, FUD, and other subjective negative meta comments, I won't engage with you in any context if they continue. I appreciate you're frustrated I'm not willing to be dragged into details and sticking to high level examples and points, but I don't think it's worth writing technical papers here, Astral know better their technical experience implementing Python packaging tooling in Rust than I do. Maybe I'm 100% wrong. We're all here in good faith trying to help the Python packaging ecosystem with the best experience we can, and I think we've all got relevant experience here.

---

_Comment by @charliermarsh on 2024-12-22 23:05_

I appreciate all the discussion and we will of course take these concerns into account as we figure out our exact plans here.

For big questions and projects like this, we tend to take our time in working through our answers, since we're a small team (with some folks are out for the holidays in this case) and want to able to think deeply (over reacting quickly) on anything with a long-term impact and complex design considerations. So, please bear with us.

In the meantime, I'd just ask that we curtail any more conversation around the pros and cons. My impression is that everyone has had a chance to speak their minds (and should feel heard -- we'll read it all), and I'd prefer to end with productive disagreement rather than any other negative sentiment.

Happy Holidays!


---

_Comment by @T-256 on 2024-12-24 23:35_

To make it both sides of this thread happy I want to point to @hauntsaninja's comment [above](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558254966).

In addition, `uv-build` package can be _pure python_, _small size_, _0 dependency_ and _universal_ build backend that other frontends could profit. In there, `uv` as fronted, internally, most treat with `uv-build` same as `uv` build backend.

Definitely, that would have some maintainability costs:
- sync features with uv internals
- sync version bumps
- minimum supported python version

## Current vs Future implementation
I think that current path of implementation is correct (which is still in preview) that it provides an option for build backend (which is correctly not set as default) to force every build frontend to use **`uv`'s binary**.
With that said, in future we most have both `uv` and `uv-build` as build backend packages. IMHO, it covers all needs:

### Building projects with `uv` or `uv-build` as backend and `uv` as frontend:
`uv` is able to apply its special features like _direct build_ (`--no-force-pep517`) to improve performance.

### Building projects with `uv-build` as backend with other foreign frontends:
It is `uv-build` package can be _pure python_, _small size_, _0 dependency_ and `universal` so every build frontend can be performant with it.

### Building projects with `uv` as backend with other foreign frontends:
Most frontends are using `pep517`, so they need to download proper `uv` wheel or in worst case build `uv` from source.
In best case (which the frontend load `uv` wheel from its cache), the building process may be more performant (?) than `uv-build` as pure python backend.

## Conclusion
As we can see `uv-build` could be satisfy both `uv` as build frontend and other build frontends. While `uv` as build backend is still there to do the same job as `uv-build` for the `uv` as build frontend and apply force to other frontends to use `uv`'s native binary to build the project. Since I see the later one a rare case, I'd recommend make `uv-build` as the default backend for `uv init` command.
The general roadmap here would be:
1. Stabilize `uv` build backend and expose it as non-default option.
2. Implement pure python `uv-build` package with same behavior as `uv` build backend.
3. `uv` internally should now treat `uv-build` build backend same as `uv` build backend. all infrastructure already would be there since `uv` build backend stabilization. So the internal change would be, something like:
    ```diff rust
    - if project.build_backend == "uv" {
    + if project.build_backend == "uv" || project.build_backend == "uv-build" {
    ```
### Edit: Only one backend
As I also mentioned above and @pawamoy's comment [here](https://github.com/astral-sh/uv/issues/3957#issuecomment-2561496815) Since rarely use of `uv` build backend with other foreign build frontends, we can also consider this roadmap:
1. Implement pure python `uv-build` package with same behavior as current `uv` build backend.
2. Internally, in backend section, rename `uv` to `uv-build`.
3. Remove build backend support from `uv` Python package.
4. Stabilize `uv-build` and expose it as default option for build backend for `uv init` command.

---

_Comment by @pawamoy on 2024-12-25 00:00_

Interesting. So, like what Flit is doing actually, without the mistake (IMO) of extending the backend features in the frontend. Also what Hatch is doing based on a comment from @ofek earlier.

I wouldn't recommend having two build-backends though, but only one that is pure-python. Then uv, as a frontend, can detect it and hijack it with a Rust implementation. Other frontends would work as usual with the pure Python backend.

---

_Comment by @nathanscain on 2024-12-25 00:07_

The Astral team requested that we not keep going back and forth with pros/cons etc until they have time to review the current discussion and talk it out internally

As Charlie said, everyone seems to have said their piece and has been heard. Have a Merry Christmas everyone 🎄

---

_Label `build-backend` added by @konstin on 2025-01-14 08:44_

---

_Comment by @fortminors on 2025-01-30 08:55_

@charliermarsh Are there any news on this? Is it discussed only internally or is there some other location where the discussion takes place?

---

_Comment by @zanieb on 2025-01-31 20:33_

We're discussing this internally. We'll chime in here eventually.

---

_Comment by @zanieb on 2025-02-13 22:16_

We've been thinking about all the concerns noted here and have some thoughts to share. 

As a TL;DR:

1. We will be releasing a build backend written in Rust but…
2. We’ll ship it as a separate, minimal package to improve its distributability and flexibility.

I’ll talk through our decision, starting with some background on our motivations.

When we first launched uv's project management capabilities, we tried using a third-party backend by default. The feedback from users about this experience was very negative: there was confusion that errors were coming from another tool, that they needed to read its documentation to understand what was happening, and that they often had to add configuration for it to work with their project. In response to this feedback, we decided not to use a build system in `uv init` without opt-in ([ref](https://github.com/astral-sh/uv/releases/tag/0.4.0)). However, we see this as a significant disservice to Python users — using a build system solves real problems with the Python project experience. We still see quite a bit of confusion from users about build backends. Python beginners in particular would be well served by a build backend, but needing to learn another tool to get started adds a lot of complexity. We want to give users a simple and intuitive experience with pure Python projects, but that experience is significantly complicated by the lack of an integrated build backend.

We cannot deliver the user experience we want with existing build backends — we need better error messages, better defaults, and more robust handling of edge cases. Due to the intentionally isolated nature of a build backend, there is context about the user's intent that cannot be captured without integration with the build frontend. For example, an integrated backend lets us understand *why* the frontend is building the package or *why* the backend failed so we can provide error messages that guide people to a successful build. We have devoted a lot of resources to improving build error messages already by parsing the output of a build backend to provide hints to users, but that’s brittle.

While it’s a major benefit that we can special case our own backend (as done in Poetry and Hatch), the build backend will be usable with a different frontend than uv and all of the other build backends will continue to be supported by uv’s frontend. We’ve always focused heavily on standards compliance and will continue to do so. 

Shifting gears, there are a few alternative suggestions in the discussion:

1. Implement the backend in both Python and Rust
2. Implement an existing backend and override use of it ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558345514))
3. Don't write a build backend

I'll respond to these next.

With uv, we've invested heavily in a solid foundation for Python packaging in Rust. We have a lot of code and abstractions, with a focus on correctness and performance. It's not feasible for us to maintain duplicate implementations in Python, as in (1). We think this is infeasible both due to time constraints and, as mentioned by some, foundational differences in behavior between the Rust and Python standard libraries — subtly different behavior here would be a disaster. We want to keep our rate of development high and deliver features to users — we can't do that while maintaining multiple versions of our core abstractions.

For similar reasons, we think that (2) is infeasible. Even if we accept the premise that it is feasible, it's against our product principles to shadow another tool implicitly and re-use their configuration.

Additionally, our goal is not just to "make a more performant build backend". As some mentioned, the overhead of a build backend is generally minimal (though noticeable when using uv). We want to innovate on the *experience* of building Python packages because it is a core part of working on Python projects. The feedback from our users (in the form of questions and bug reports in our issue tracker) has made it clear that improving integration with a build backend will be very high impact.

At this point, we’re at “build a backend in Rust” or “don’t do it at all” (3) — there are a few recurring concerns that support the latter conclusion. We’re taking these concerns seriously, but want to balance them with the value we see for users. I'll do my best to address them here, but I'll admit that packaging is a very hard problem and I do not have good answers to all of the concerns raised.

1. The Rust toolchain is not as portable as Python ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558143545), [ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558040859), [ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558155306))
    
    Yes, Python has broader platform support than Rust. However, uv is a new project and widespread adoption is going to take time. In the short-term, it seems unlikely that uv adoption will surpass Rust platform support. In the long-term, we expect Rust platform support to continue to grow.
    
    Additionally, Rust is already a critical component of the Python ecosystem. For example, `cryptography` and `pydantic` (both in the top 25 most downloaded packages on PyPI - [ref](https://hugovk.github.io/top-pypi-packages/)) require Rust and use a Rust build backend (`maturin`). They are widely used and shipped in all major Linux distributions (e.g., [ref](https://repology.org/project/python%3Acryptography/versions)). There is a concern that uv is *different* than these packages, which we will discuss next.
    
2. uv is less portable than the average Rust application or library ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558143545))
    
    This concern can be split into a couple components (1) the minimum supported Rust version and (2) the number of dependencies. We will attempt to address both of these. (1) We rarely *need* to update the latest Rust version. We often do so because it addresses tangible problems in our codebase, but it's rare that they're user facing. If this becomes a problem, we can likely hold off on updating. (2) We will split the build backend into a separate package and remove all unnecessary dependencies, e.g., the build backend doesn't need a network stack. Unlike in the main package, we will focus intentionally on simplifying redistribution.
    
3. The uv binary is large ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558051967))
    
    The [uv wheel](https://pypi.org/project/uv/0.5.30/#files) is ~15MB. In contrast, the [setuptools wheel](https://pypi.org/project/setuptools/#setuptools-75.8.0-py3-none-any.whl) is 1.2MB. We agree it would be a waste of resources to distribute a large binary without strong justification. A brief experiment showed a wheel for a uv build backend package with minimal dependencies (as described above) was also 1.2MB. This is further justification for separating the build backend into its own package.
    For completeness, the [flit wheel](https://pypi.org/project/flit/#flit-3.10.1-py3-none-any.whl) is ~50kB. We hope to reduce the size of our binary further with more investment.
    
4. Downstream packagers build things from source ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558154653), [ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558155306))
    
    We hope the practical concerns here are largely addressed in our responses to (1) and (2), though we understand building from source is complicated by an additional toolchain. We’re lucky to have engagement from downstream package maintainers already, I’m hoping we continue to get feedback from them to guide this project.
    
    (As a bit of an aside, it's interesting to hear a primary motivation for using source distributions is access to the test suite. It'd be great to solve running test suites against wheels without requiring a build from source.)
    
5. A change to one package's build system can affect the build of many others ([ref](https://github.com/astral-sh/uv/issues/3957#issuecomment-2558486839))
    
    We hope that maintainers of critical dependencies in the Python ecosystem will be considerate with regards to their dependencies and build system. We agree that defaults matter, but we think they have less effect as a package is used by more people and the maintainer's experience grows. We empathize with the point here, but we want to focus on improving the development experience for the majority of Python users.
    

With that, I want to take a second to thank everyone for their feedback here. It’s helped us prioritize some changes (like a separate package) that we may not have otherwise.

Going forward, I'd like to focus this issue on the behavior of the build backend within this framing instead of debating if it should be done or not.

---

_Comment by @eli-schwartz on 2025-02-13 23:09_

> Downstream packagers build things from source (ref, ref)
> 
> We hope the practical concerns here are largely addressed in our responses to (1) and (2), though we understand building from source is complicated by an additional toolchain. We’re lucky to have engagement from downstream package maintainers already, I’m hoping we continue to get feedback from them to guide this project.

(2) should help a lot, I think. In particular it seems reasonable to assume that the MSRV of a dedicated build-backend crate can be a lot lower than the MSRV of uv as a whole.

In particular, if you can avoid entirely the use of problem crates such as ring.

That being said, the arguments with regard to (1) aren't as compelling as they could be. There are CPU architectures where Rust isn't available at all, and therefore cryptography isn't available either. I would say pydantic isn't available either, except that really, pydantic can be commonly downloaded all it wants but it is hardly core infrastructure. Cryptography isn't universally needed, there's absolutely loads of stuff Gentoo users commonly install that are written in Python, without ever ending up hitting a dependency tree that leads back to cryptography... but there are definitely some packages that are uninstallable for Gentoo users on less common architectures, solely because of cryptography.

Build backends are a much bigger influence here than something like pydantic or cryptography, because they could potentially influence "core infrastructure" packages that are written in pure python. For something verging on a worst-case example, what if "trove-classifiers" ported to the uv build backend because they heard it was so popular nowadays, and also it's basically a data package so what's the big deal and who really needs it except for other package *developers*. Except, setuptools and flit don't care but hatchling depends on it so now a massive number of packages can't be built on non-rust platforms.

There are other packages that are common runtime packages needed by other software which could be a huge problem as well, e.g. attrs, certifi, appdirs, xdg... so we end up in a situation where people are told about a new integrated backend that ~~is just like all the others~~ has better error messages when misconfigured, and they think "sure, I use it for development anyway, might as well use this too" and then break half the world. And it turns out that you can't use uv-backend if you're "important enough" for some nebulous definition of important, because a single package not supporting 

Having a pure python version would at least prevent this invisible line across which package maintainers have to magically know that it's socially bad to use a specific backend.


> (As a bit of an aside, it's interesting to hear a primary motivation for using source distributions is access to the test suite. It'd be great to solve running test suites against wheels without requiring a build from source.)

I'm not sure it's conceptually solvable. Some projects install their testsuite inside the wheel, that's about all you can do. But most projects consider it bad to install tests as part of the wheel, as it penalizes pip install users.

The handful that install their testsuite tend to argue that they are the exceptional case and that end users creating a virtualenv should then go ahead and import and run the testsuite to make sure everything actually works... because they're pretty confident that there are cases where it *doesn't* work. It's a common concern in the science ecosystem, for example (delicate interactions between various moving parts can do that for you :shrug:).

We may also have to first solve the problem where large subsets of the python packaging ecosystem object to including tests even in the sdist, as they regard sdists as purely for pip to build wheels and see tests as dead weight. That includes going around to other projects and advocating to not include tests in the sdist! :(

One possibility is to download both the wheel and the sdist and then install the wheel and test it using the sdist copy of the test suite. But it's not immediately clear what the advantages are over just building the sdist using a build backend. It would add great complexity to the process, doesn't solve the occasional need to backport a patch that fixes crashes, can't be used for anything that has compiled extensions, doubles or triples the download size... It's something I've thought about before, but with the association "worst case scenario, we *might* be able to do this to get out of a very sticky situation".

---

_Comment by @eli-schwartz on 2025-02-13 23:16_

> I would say pydantic isn't available either, except that really, pydantic can be commonly downloaded all it wants but it is hardly core infrastructure.

I should take this back. Sigstore appears to rely on it, which means it is extremely problematic to validate cryptographic signatures for cpython starting with 3.14. Oh well, we can just assume it was verified on an amd64 developer machine and disable signature verification -- and the Gentoo PGP signature upon the checksum manifest for cpython will prove that that was checked by someone else. Not great, but not the end of the world.

Still, I do admit I was wrong. Pydantic is used for more than I thought...

---

_Comment by @T-256 on 2025-02-14 00:49_

> With uv, we've invested heavily in a solid foundation for Python packaging in Rust. We have a lot of code and abstractions, with a focus on correctness and performance. It's not feasible for us to maintain duplicate implementations in Python, as in (1). We think this is infeasible both due to time constraints and, as mentioned by some, foundational differences in behavior between the Rust and Python standard libraries — subtly different behavior here would be a disaster. We want to keep our rate of development high and deliver features to users — we can't do that while maintaining multiple versions of our core abstractions.

@zanieb Thanks for this detailed write-up, tradeoff of each approach is now more obvious to me.
IIUC, below are tradeoff of having both **pure-python** backend (as public package) and **Rust-based** backend shadow (used internally):
- Time constraints
- Maintain duplicated implementations
- performance (python-only)
- Lack of Python's stdlib power to implement specifications based on correctness. (python-only)

When I compare these tradeoffs to the rust-only backend tradeoffs (those five mentioned [here](https://github.com/astral-sh/uv/issues/3957#issuecomment-2657825266)), and see how impactful each side is, I'd ended-up to the `uv_build` package could be pure-python. I think it worth to make an investment on it and take first step with it, of course we can always change it in future!

---

_Comment by @johnthagen on 2025-02-14 01:08_

If you're looking for tips on how to minimize the binary size of the Rust build backend, I maintain a repository with some information on that:

- https://github.com/johnthagen/min-sized-rust

I hope it can help a bit.

---

_Comment by @T-256 on 2025-02-14 01:38_

> Going forward, I'd like to focus this issue on the behavior of the build backend within this framing instead of debating if it should be done or not.

will `uv_build` come with different versioning than uv? if yes, when `build-system.requires = [ "uv_build>=0.7,<0.8" ]` and using `uv` v0.9 as frontend, will there fast-path feature available?

---

_Comment by @potiuk on 2025-02-14 02:44_

I think it's a good decision. It addresses most concerns of mine.

One suggestion for consideration though: I think it would be great if uv tooliing (`init` particularly) supports choosing different backends and allows to set different default backend. I think that would address even more of the "uv's popularity might hamper portability and strengthen separation and standard compliance for build backends/frontends/

If package creator wants to still use `uv` as a "development environment", but still want to have maximum portability (say using flit) - they should be able to do `uv init --build-backend flit` and have `pyproject.toml` with flit backend configured.

Of course, that's also a bit problematic - which other backends would be supported in this case? not sure how to solve it easily, but maybe simply arbitrary choosing (and maintaining?) some of the popular backends would be a good choice, or maybe a way to plugin different other backends as well ? not so sure.

---

_Comment by @zanieb on 2025-02-14 02:52_

> will uv_build come with different versioning than uv? if yes, when build-system.requires = [ "uv_build>=0.7,<0.8" ] and using uv v0.9 as frontend, will there fast-path feature available?

We haven't worked out the details of this yet. If we do version the build backend separately, then presumably if there have been no changes to it then a later uv version could use its bundled version of the build backend. We'll need to design this carefully. It's probably easiest to start with 1:1 versioning though.

>  I think it would be great if uv tooliing (init particularly) supports choosing different backends and allows to set different default backend

I think this makes sense. We already do support quite a few: `--build-backend <BUILD_BACKEND>  Initialize a build-backend of choice for the project [possible values: hatch, flit, pdm, setuptools, maturin, scikit]` and persistent configuration for `uv init` is on the todo list but frequently requested.

---

_Comment by @mgorny on 2025-02-14 05:31_

I'm sorry but I'm a little confused now — will this backend be providing a PEP 517 wrapper, or will packages using it be installable from source only using dedicated tooling (i.e. `uv`)?

---

_Comment by @potiuk on 2025-02-14 10:42_

> I'm sorry but I'm a little confused now — will this backend be providing a PEP 517 wrapper, or will packages using it be installable from source only using dedicated tooling (i.e. `uv`)?

As I read what @zanieb wrote - it will be fully compliant with PEP 517, but it will also have some custom ways to hook into `uv` frontend to provide more front-end integration when `uv` is used and where the current PEP 517 mechanism are not enough. 

At least that's how I interpret it - and having experience with a number of fronted + backend combinations, this is often what happens in other cases. 

Just to give an example - hatch can interact with some specific "hatchling" behaviours that allow to do some specific things, for example hatch allows to run a custom build hook when it is implemented in `hatch_build.py` - and we are utilising it in Airflow to compile assets and retrieve git version info when we are preparing "release" package.

Another example is enabling reproducible builds - while many backends have a way to produce reproducible builds, one of the ways how to do it is to pass an environment variable to set the "date" with which files will be stored in .whl, but you also should do some other stuff before you run the build - for example make sure that group bits are cleared on all source files, because by default git uses system umask and it might impact reproducibility.

Those behaviours (and likely a number of others) are not entirely covered by PEP 517 - possibly some of those will be in the future by follow-up PEPs  - but in order to make a good user experience, some custom ways of interaction between frontend and backend have to be implemented.

I think it will not impact the basic "build wheel" or "build sdist" or "install package in editable mode" features that PEP 517 standardised, it's more driven be improving user experience for more advanced cases.

But that's my interpretation, so I might be wrong.

---

_Comment by @charliermarsh on 2025-02-14 13:32_

👍  We'll distribute the build backend as a dedicated package on PyPI. And it will be usable as a standalone build backend entirely independently of uv, e.g.:

```toml
requires = ["uv-build>=0.5.15,<0.6"]
build-backend = "uv_build"
```

(We might change the package or module name before releasing this; consider it an illustrative example.)

But when you're using uv, and we see a package that's using the uv build backend, we can (e.g.) execute the build hooks directly or have other integrations.


---

_Comment by @honzajavorek on 2025-02-25 20:27_

I just discovered this super secret feature and would like to try it out. However, my package is a [namespace package](https://packaging.python.org/en/latest/guides/packaging-namespace-packages/). The build backend doesn't seem to recognize such structure though. Is there a way to instruct the backend about the structure? Should I file a separate issue?

---

_Comment by @GarbenTangheVintecc on 2025-02-26 12:56_

When I use uv-build, I get the following warning:
```toml
requires = ["uv-build>=0.5.15,<0.6"]
build-backend = "uv_build"
# warning: The value for `build_system.build-backend` should be `"uv"`, not `"uv_build"`
# warning: Expected a single uv requirement in `build-system.requires`, found ``
```
When trying to mitigate the warning, I get an error:
```toml
requires = ["uv>=0.6.3,<0.7"]
build-backend = "uv"
# Traceback (most recent call last):
#   File "<string>", line 8, in <module>
# ModuleNotFoundError: No module named 'uv'
```

Is the `UV_PREVIEW=1` environment variable required here? This seems to solve the error.

---

_Comment by @GarbenTangheVintecc on 2025-02-26 13:11_

Does the uv build backend support `flat` structures? Or is `src`-style the only supported structure?
I am trying to perform an editable install using `uv pip install --editable /path/to/package_name` but I get:
```bash
> Expected a Python module with an `__init__.py` at: `python/package_name/src/package_name`
```

---

_Comment by @konstin on 2025-02-26 13:34_

Currently, the uv build backend does not support the namespace packages.

We're in the transition from `uv` to `uv-build`, but the PR isn't merged nor shipped yet, you can follow along at #11446.

You can configure the source code directory through `tool.uv.build-backend.module-root` (https://github.com/astral-sh/uv/issues/8779), this should support the flat layout, too.

---

_Comment by @konstin on 2025-03-07 13:59_

In the latest release, we've split the build backend out into a separate `uv_build` package. The `uv` package itself doesn't contain the build backend anymore, I've updated the documentation in #8779 accordingly.

---

_Comment by @astrojuanlu on 2025-03-07 18:11_

Tried

```
$ tail -n3 pyproject.toml
[build-system]
requires = ["uv_build>=0.6,<0.7"]
build-backend = "uv_build"
```

but

```
$ uv build
Building source distribution...
Error: Unknown subcommand: build-backend (cli: ArgsOs { inner: ["/Users/juan_cano/.cache/uv/builds-v0/.tmp5tuceQ/bin/uv-build", "build-backend", "build-sdist", "/private/tmp/uv-test/dist"] })
  × Failed to build `/private/tmp/uv-test`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_sdist` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

probably because

https://github.com/astral-sh/uv/blob/f427164d9945ad65cfc5760f69ee1dc1e888ac57/crates/uv-build/python/uv_build/__init__.py#L63

doesn't match

https://github.com/astral-sh/uv/blob/f427164d9945ad65cfc5760f69ee1dc1e888ac57/crates/uv-build/src/main.rs#L18-L19

?

(also thanks @zanieb for [this writeup](https://github.com/astral-sh/uv/issues/3957#issuecomment-2657825266) and to the team for accounting for all the feedback ❤)

---

_Comment by @konstin on 2025-03-07 22:15_

Sorry, the package had the old Python shim still! Fixed in #12058.

---

_Comment by @johnthagen on 2025-03-21 13:38_

@konstin I was curious if Astral had any estimated timelines for when `uv_build` would graduate to General Availability. No rush, purely curious for some internal planning.

---

_Comment by @zanieb on 2025-03-21 13:41_

We don't have a set timeline, but it's a high priority — is the preview label blocking you somewhere?

---

_Comment by @johnthagen on 2025-03-21 14:30_

> is the preview label blocking you somewhere?

It's more that one of our small goals with moving to `uv` is to not track two project teams (`uv` + hatchling) and we're also hoping to avoid any preview features before we make the final big switch to `uv`.

---

_Comment by @eli-yip on 2025-03-21 14:49_

> Currently, the uv build backend does not support the namespace packages.
> 
> We're in the transition from `uv` to `uv-build`, but the PR isn't merged nor shipped yet, you can follow along at [#11446](https://github.com/astral-sh/uv/pull/11446).
> 
> You can configure the source code directory through `tool.uv.build-backend.module-root` ([#8779](https://github.com/astral-sh/uv/issues/8779)), this should support the flat layout, too.

Unable to build package with flat layout, `uv` shows errors like this:

```
❯ uv build
Building source distribution...
Building wheel from source distribution...
Error: Expected a Python module with an `__init__.py` at: `audio_collect`
  × Failed to build `/xxxx`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build
      environment.
```

My projects layout:

```
ls
main.py  pyproject.toml  README.md  uv.lock
```

`pyproject.toml`:

```toml
[project.scripts]
audio-collect = "main:app"

[build-system]
requires = ["uv_build>=0.6,<0.7"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-root = ""
```

I think `__init__.py` is unnecessary in a flat layout package.

---

_Comment by @notatallshaw on 2025-03-21 15:00_

> I think `__init__.py` is unnecessary in a flat layout package.

No, once your package is installed it must have an `__init__.py`, to not be an implicit namespace package:  https://peps.python.org/pep-0420/

Being flat, or not, is independent of this concept. 

I would also strongly advise users in general against implicit namespace packages, they have a lot of quirky behavior that can be difficult to reason about. Unless you're familiar with, and want, their core feature of being able to merge multiple packages together under the same implicit namespace. 

In general, if uv does eventually add support for implicit namespaces, I would very much prefer that it be opt-in, rather than opt-out. 

---

_Comment by @eli-yip on 2025-03-21 15:03_

> > I think `__init__.py` is unnecessary in a flat layout package.
> 
> No, once your package is installed it must have an `__init__.py`, to not be an implicit namespace package: [peps.python.org/pep-0420](https://peps.python.org/pep-0420/)
> 
> Being flat, or not, is independent of this concept.
> 
> I would also strongly advise users in general against implicit namespace packages, they have a lot of quirky behavior that can be difficult to reason about. Unless your familiar with, and want, their core feature of being able to merge multiple packages together under the same implicit namespace.
> 
> In general, if uv does eventually add support for implicit namespaces, I would very much prefer that it be opt-in, rather than opt-out.

Thank you for your clear guidance, but after I added `__init__.py` to the project root directory as instructed, I still encountered the same error as above.

---

_Comment by @merwok on 2025-03-21 16:04_

The layout there has a module `main.py` but no packages though!

---

_Comment by @ollie-bell on 2025-03-21 16:08_

> Thank you for your clear guidance, but after I added `__init__.py` to the project root directory as instructed, I still encountered the same error as above.

If you just have a single python module, it's possible you don't need a build-backend or package at all and you can use this https://docs.astral.sh/uv/guides/scripts/ ?

---

_Comment by @notatallshaw on 2025-03-21 16:12_

Slight correction: "flat" layout usually means not having a src directory but still having a package directory: https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/

This was in my head when I wrote my above comment, there may be some nuances to not having a package directory at all that I'm unfamiliar with. E.g you probably need to specify exactly which files are considered part of the package so, for example, you don't end up accidentally including the build contents in the package (and recursively doing so each time).




---

_Comment by @eli-yip on 2025-03-21 16:28_

> > Thank you for your clear guidance, but after I added `__init__.py` to the project root directory as instructed, I still encountered the same error as above.
> 
> If you just have a single python module, it's possible you don't need a build-backend or package at all and you can use this [docs.astral.sh/uv/guides/scripts](https://docs.astral.sh/uv/guides/scripts/) ?

Thanks for your advice. I might be facing an XY problem. My goal is to install the [typer](https://typer.tiangolo.com/) app from main.py using `uv tool install .` for global CLI access. It appears I need to build the package first, and when using uv_build as my backend, I ran into the issue. Any better suggestions?

---

_Comment by @nathanscain on 2025-03-21 16:39_

Honestly, it will just work if you use the standard `src/package/__init__.py` layout. Flat layout (`package/__init__.py`) can also work with configuration. Having just a top level Python file isn't easily package-able as far as I'm aware, and any solution would certainly be non-standard for no discernible reason.

Summary:

1. Move `main.py` to `src/audio_connect/__init__.py`
2. Update your script to point to `audio_connect:app`
3. Remove other packaging configuration

---

_Comment by @eli-yip on 2025-03-21 17:10_

> Honestly, it will just work if you use the standard `src/package/__init__.py` layout. Flat layout (`package/__init__.py`) can also work with configuration. Having just a top level Python file isn't easily package-able as far as I'm aware, and any solution would certainly be non-standard for no discernible reason.
> 
> Summary:
> 
> 1. Move `main.py` to `src/audio_connect/__init__.py`
> 2. Update your script to point to `audio_connect:app`
> 3. Remove other packaging configuration

Thanks. I misunderstood “flat layout.” Turns out uv describes what I wanted in the docs: https://docs.astral.sh/uv/reference/cli/#uv-init–app

---

_Comment by @nathanscain on 2025-03-21 17:14_

Happy to help. Yes `uv init --app --build-backend uv` should do that if I remember correctly. (Requires `UV_PREVIEW=1`)

---

_Comment by @ofek on 2025-03-22 04:35_

> I would also strongly advise users in general against implicit namespace packages, they have a lot of quirky behavior that can be difficult to reason about.

Implicit namespace packages is the recommended approach based on the official docs: https://packaging.python.org/en/latest/guides/packaging-namespace-packages/#creating-a-namespace-package

What issues have you had with their use? I personally have had less issues with them compared to the older approaches and just [switched to them](https://github.com/DataDog/integrations-core/pull/19826) at work last week since we dropped Python 2 somewhat recently.

---

_Comment by @nathanscain on 2025-03-22 04:47_


> Implicit namespace packages is the recommended approach based on the official docs: https://packaging.python.org/en/latest/guides/packaging-namespace-packages/#creating-a-namespace-package

That's not what that page says. It is recommending native namespace packages over legacy namespace packages. At no point does the linked page promote namespace packages over standard, non-namespace packages.

On the contrary, it specifically states that this type of packaging is only for when you want multiple, separate packages to install within the same import namespace when installed in the same environment - a concept that is both rare and "not appropriate in all cases" even when you have that objective (unlikely).

It is an easy mistake to make (I once thought `__init__.py` was effectively optional due to this), but no - namespace packages are not the norm, default, or recommended path for the vast, vast majority of Python packages.

Additionally, look and see that every other example on that site uses the standard non-namespace package method, and they even add this note:

> Technically, you can also create Python packages without an `__init__.py` file, but those are called namespace packages and considered an advanced topic (not covered in this tutorial). If you are only getting started with Python packaging, it is recommended to stick with regular packages and `__init__.py` (even if the file is empty).


---

_Comment by @potiuk on 2025-03-22 07:55_

@notatallshaw 

> I would also strongly advise users in general against implicit namespace packages, they have a lot of quirky behavior that can be difficult to reason about.


@nathanscain 

> That's not what that page says. It is recommending native namespace packages over legacy namespace packages. At no point does the linked page promote namespace packages over standard, non-namespace packages.

> On the contrary, it specifically states that this type of packaging is only for when you want multiple, separate packages to install within the same import namespace when installed in the same environment - a concept that is both rare and "not appropriate in all cases" even when you have that objective (unlikely).

> It is an easy mistake to make (I once thought __init__.py was effectively optional due to this), but no - namespace packages are not the norm, default, or recommended path for the vast, vast majority of Python packages.

@ofek 

> What issues have you had with their use? I personally have had less issues with them compared to the older approaches and just https://github.com/DataDog/integrations-core/pull/19826 at work last week since we dropped Python 2 somewhat recently.


My comments after huge refactor of Airlfow where we use both namespace packages (impllicit and legacy unfortunately mixed due to historical reasons for now) I must agree with @notatallshaw and @nathanscain . Implicit namespace packages introduce ambiguity especially when various tools (starting from mypy, but also pytest, various IDEs and a number of others) are trying to determine what is the `root` package we should import things from. You have to explicitly tell what are the "base" folders to start importing things from, otherwise weird things happens. When you press "Cmd + Enter" in Intellij on missing imports it will propose some semi-random choices sometimes or it might import  things including folders that were never meant to be python packages. This especially when you have workspaces with multiple python packages in the same monorepo, where current tooling is not yet adapted well to understand where such "root" folders are. 

This is especially problematic where you have implicit namespace packages in tests  and leads to things like this https://github.com/apache/airflow/blob/main/pyproject.toml#L554 - where we had to explicitly liist "src" and "test" root folders for all 100 packages we have in Airflow's monorepo. And in order to properly get imports in IDE you have to explicitly mark "tests" folder as "Test sources root" in your IDEs (unlike "src", the "tests" folders are not recognized by IDEs as root folders even if you use uv workspace) 

That's one of the reasons we had to use legacy namespace packages all over the project to keep out from the ambiguities that the implicit namespaces introduce.

I wish in the future the tooling will become a bit better in handling implicit namespaces but I do agree with the statement `__init__.py` is here to stay and you should use implicit namespace packages only when you really need it (like we do in airflow due to historical choices where various distributions are sharing the same "airflow" package namespace). I wish we had done better decisions to separate those namespaces in the past, but some of the other maintainers feel very strongly that all our distributions should share the same common "airflow" import and we have to deal with the complexity.




---

_Comment by @notatallshaw on 2025-03-22 15:02_

I appreciate we are going a little off topic here, but for anyone interested in a (long) tutorial on the import system in Python, including a good diversion into implicit namespaces, I strongly recommend [David Beazley's Modules and Packages: Live and Let Die!](https://dabeaz.com/modulepackage/)

I watched it near when it first came out, and watched it again about ~1 year ago, and this time immediately enabled [INP001](https://docs.astral.sh/ruff/rules/implicit-namespace-package/) on all my work projects.


---

_Comment by @merwok on 2025-03-22 16:35_

> Move `main.py` to `src/audio_connect/__init__.py`

Can a uv developer confirm or infirm whether uv supports modules (not packages)?
(regardless of usage of top-level vs src directory)

---

_Comment by @charliermarsh on 2025-03-22 16:38_

I don't fully understand the question. "Supports" in what sense? (You might be better off asking for help in the Discord if you have a specific question unrelated to this issue.)

---

_Comment by @merwok on 2025-03-22 17:17_

Someone gave advice to turn a module into a package. Is that required by uv, or does it support packaging simple modules?

---

_Comment by @nathanscain on 2025-03-22 17:49_

Technically, it would still be a module after that file move - it would just be a module _within a package_ (which was the objective).

I was mostly just trying to give a "this is what is standard and will just work" piece of advice. There isn't anything I'm aware of in the spec that would prevent a single python file being at the root of a wheel (for example), but it would certainly be nonstandard without much of a reason to be.

I can't say uv doesn't support that because I've never tried (though I imagine it is build-backend dependent), but note that the uv `--package` flag which adds a build-backend will also setup a standard `src` package layout.

---

_Comment by @merwok on 2025-03-22 19:30_

Let’s stop this side discussion, but I strongly disagree with your viewpoint.
Python modules that are not inside a package (import package, not a distribution) are perfectly fine and valid.

---

_Comment by @nathanscain on 2025-03-22 19:47_

> Let’s stop this side discussion, but I strongly disagree with your viewpoint.
> Python modules that are not inside a package (import package, not a distribution) are perfectly fine and valid.

I agree - last message on the subject.

That wasn't my viewpoint **at all**. Please reread. Of course Python modules are valid outside of a package, but - as the user was wishing to install it as a tool with uv - it was necessary for the module to be distributable due to the nature of how `uv tool install` works (creating a venv and installing the referenced package within it). There are other ways to make a module globally executable (combining shebang, uvx, and inline metadata comes to mind), but that wasn't the stated objective.

And finally, I specifically never even said such a construction (top level module placed directly within a distribution artifact) would be invalid - just that it would be non-standard without cause/need and would require a build-backend that supported it (which I am not aware of and couldn't immediately find when I went searching before my response).

---

_Comment by @ofek on 2025-03-22 20:06_

> I specifically never even said such a construction (top level module placed directly within a distribution artifact) would be invalid - just that it would be non-standard without cause/need and would require a build-backend that supported it (which I am not aware of and couldn't immediately find when I went searching before my response).

Hatchling can do that easily, and shipping files outside of the package directory is quite common in the case of extension modules. For example, take a look at what Mypyc produces: https://pypi.org/project/black/#files

---

_Comment by @nathanscain on 2025-03-22 20:27_

... so it was:

- valid in the spec ✅
- build-backend dependent to use ✅
- non-standard (requires additional configuration buried in the explicit select of wheel content and hooks) ✅
- irrelevant to the stated objective where the user is specifically using the `uv_build` backend for pure python code (not an extension module) ✅

I really don't get why what I said is so controversial? uv's own defaults go with the standard layout (as does hatchling's). We had a case of a user wanting to install a module as a uv tool, so I recommended packaging said module with the default and universally supported method. I'm three hours into the video that was posted earlier in this thread and it is obvious that Python will let you do _anything_ when it comes to packaging - that doesn't mean those options should be recommended for these basic cases.

---

_Comment by @eli-schwartz on 2025-03-23 02:09_

@nathanscain,

python packages, such as `{root}/src/modname/`, are just as nonstandard and build backend specific as `{root}/modname.py`. Which is to say, not very. Every build backend supports the latter, except maybe uv. At least flit and setuptools will ***autodetect*** modname.py using the default rules. I am genuinely baffled that anyone could believe otherwise. e.g.:
- setuptools: https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#automatic-discovery describes backend-specific notions that setuptools includes support for, of "flat" and "src" layout and will autodetect from both. It also takes the opportunity to describe these ***build backend specific*** layouts as such:
  > By default setuptools will consider 2 popular project layouts, each one with its own set of advantages and disadvantages [[1]](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#layout1) [[2]](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#layout2) as discussed in the following sections.
- flit: https://flit.pypa.io/en/stable/pyproject_toml.html#module-section specifically points out that it supports flat and src layouts and in both cases will package up modules using ***import name***, so `modname.py` and `modname/*.py` are equally valid


Claiming that src/ is "the standard" layout seems like a pretty hot take to me as two of the most significant build backends of all time either refuse to get into fights about which is better or go out of their way to *warn* you that it has "advantages and disadvantages" and that you should consider both the advantages and the disadvantages and decide whether it fits you well.


This conversation is... surreal to me. Half the time I cannot tell whether people in this thread are debating `{root}/modname.py` or `{root}/main.py` ***installed via backend rules via file renaming*** as `{site-packages}/modname.py`. The latter case seems logically to me like something that would have to be very advanced usage indeed, in particular it should require user-defined build rules e.g. setuptools overriding cmdclass `build_py` or meson via [`fs.copyfile()`](https://mesonbuild.com/Fs-module.html#copyfile) and installing the result.

(But the actual pyproject.toml which was posted and which used "main.py" also declared the script name as `audio-collect = "main:app"` so it seems plain the intent was to install an `import main` module. This is a terrible idea because the name can trivially conflict with software you didn't intend, and it's possible to have a discussion about the dangers of this without berating people for doing extremely common and well-supported things. For much the same reason that nobody should install an `import tests`.)

> And finally, I specifically never even said such a construction (top level module placed directly within a distribution artifact) would be invalid - just that it would be non-standard without cause/need and would require a build-backend that supported it (which I am not aware of and couldn't immediately find when I went searching before my response).

You cannot possibly have checked very hard!

https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#single-module-distribution

> There is also a handy variation of the flat-layout for utilities/libraries that can be implemented with a single Python file:
> **single-module distribution**
> 
> A standalone module is placed directly under the project root, instead of inside a package folder:

Very clearly called out in the docs for the single most significant build backend of all time, bar none.

> it is obvious that Python will let you do anything when it comes to packaging - that doesn't mean those options should be recommended for these basic cases.

Your opinion appears to be directly in contradiction with the authors of an actual build backend which is extremely widely used, who claim that this option is an excellent example of a simple use case important enough to be documented for encouragement and quick access using a zero-config pyproject.toml and autodetection.

I really do not understand why after berating people and telling them that the idea "isn't supported by anything" and being corrected, you feel the need to switch gears and start berating people for doing something "unrecommended" and "not even an extension module" and "requires explicit configuration".

Look, we get it, you hate modules and believe support for them should be removed from the cpython interpreter (we should start off by removing all the ones in https://github.com/python/cpython/tree/3.13/Lib such as `os` and `subprocess` but I digress). But maybe wait until build backend developers say they aren't supporting it before jumping down people's throats about use cases you apparently haven't even researched.

Thanks. :)

---

_Comment by @eli-schwartz on 2025-03-23 02:18_

@nathanscain,

> That's not what that page says. It is recommending native namespace packages over legacy namespace packages. At no point does the linked page promote namespace packages over standard, non-namespace packages.
> 
> On the contrary, it specifically states that this type of packaging is only for when you want multiple, separate packages to install within the same import namespace when installed in the same environment - a concept that is both rare and "not appropriate in all cases" even when you have that objective (unlikely).
> 
> It is an easy mistake to make (I once thought `__init__.py` was effectively optional due to this), but no - namespace packages are not the norm, default, or recommended path for the vast, vast majority of Python packages.

I mostly agree that namespace packages are an advanced use case which should never be unknowingly done. 

It is also worth remembering that namespace packages are slower than normal packages, as they have to keep searching the entire import path just to check whether someone has taken advantage of the defining purpose of namespace packages. You're basically shooting yourself in the foot if you use them without having a very specific reason to need them.

However please do note that the sole use case for namespace packages is when they are *not* installed in the same site-packages directory, i.e. "environment" but exist in stacked environments. If you install all packages to the same virtualenv it technically works fine, since they get tree-merged and are indistinguishable from a single wheel with multiple sub-packages. Which `__init__.py` file gets installed last and overwrites the others is unspecified by pip, and non-pip tools require the downstream packager (e.g. linux distros) to select one, usually the "parent" package, and delete all the other `__init__.py`s to prevent fatal errors when conflicting files are noted.

(Arguably if you only care about virtualenvs, you do not need namespace packages, ever. Just install them on top of each other and let them clobber each others' files. pip doesn't care, any package is allowed to hijack another package's files whenever it wishes.)

---

_Comment by @nathanscain on 2025-03-23 03:14_

Don't want to dive into this super hard again.

1) The case at hand was someone having a `uv init --app` layout - to which I simply suggested moving to the `uv init --package` layout.  I don't recall coming across someone doing this in production from my time reviewing our dependencies. I _didn't say_ that I looked hard - I did a quick search and not much came up so I choose to help the person back to (close to) what they would have been given if they specified `--package` from the start (so that it would "just work").

2) The cited example that I supposedly should have seen is a single heading and 2 sentences before diving into custom discovery. You caught me - I didn't know it existed because it isn't very common and almost never what someone means by "flat layout" in my experience - but a variation of it. I wasn't claiming that src was better than flat - if flat was the default, I would have suggested a move to that with a top-level directory. And obviously the goal was the thing we agree on - renaming the main.py. I wasn't berating people for doing standard things, not even sure what that is aimed at as I was only trying to help the original person go from broken to working in as few steps as possible - for which he seemed grateful - and made no comment on anyone else's approaches within their own projects. The example that was shown to me was not zero-config at all (I eagerly went to look to learn something). Idk why you put quotes around "isn't supported by anything" - I didn't say that - just that I couldn't immediately find mention of it and hadn't seen it - but yes valid and in-spec. I don't hate modules, obviously. I haven't been jumping down anyone's throat. All I've been doing is responding when others respond claiming that I said things I didn't say while my daughter was napping, eating, etc.

3) As far as personal growth, thank you for showing me that variation - I hadn't seen it before. Much of this could probably have been avoided if I said "common" instead of "standard" in a few places, so I'll own that. Precision of language is always important where the connotation can be lost. It is clear that that is supported by the standard implementations (rather than just the spec) as you show. The specific case of my initial response to merwok shows that I completely misunderstood his query as I wasn't aware of this variation of the flat layout. My response was simply meant to say that I put it in one of the two common layouts - looking back, it appears they were asking about uv_build's support for this case which was unknown to me. I'll leave it to that team to answer, as I still can't speak to it.

Goodnight everyone. Apologies to anyone who felt I attacked them - was never my intention and I hope you will forgive.

---

_Comment by @eli-yip on 2025-03-23 04:53_

Thank you all for your replies and assistance! I sincerely apologize if I’ve veered off-topic at any point.

I’m a Python beginner with no prior experience in Python packaging (whether with setuptools or hatchling) before writing the script (or package) mentioned above. My understanding of Python modules is also quite limited.

Based on the response here (https://github.com/astral-sh/uv/issues/3957#issuecomment-2745969875), I tried renaming `main.py` to `audio_collect.py` (without adding an `__init__.py` file). I found that I could successfully build it with setuptools and install/use my CLI program with `uv tool install .`. However, uv build doesn’t seem to support this approach.

**Could I ask if uv build plans to offer similar support for single-module distributions (as described in https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#single-module-distribution)?**

Additionally, for a simple case like mine—using a flat layout and wanting to install and use a single module from the project root directory as a package—are there any best practices? I’d prefer to use uv build rather than other build backends.

---

_Comment by @charliermarsh on 2025-03-23 13:13_

Yes, I expect us to support single-module distributions, but I'd appreciate it if you could file a separate issue for that functionality.

---

_Comment by @notatallshaw on 2025-03-25 13:58_

Sorry if this has already been addressed but [reading this comment](https://discuss.python.org/t/should-build-backends-ever-make-backwards-incompatible-changes/85847/2):

> and uv init --build-backend uv use constraints by default, e.g., requires = ["uv_build>=0.6.9,<0.7"]. I think this could be normalized.

It made me think about the scenario of installing from a uv_build backed sdist from a Python tool. If I publish a pure Python package today with build requirements `"uv_build>=0.6.9,<0.7"`, will someone in a few years  be able to install the sdist using pip on Python 3.16?

I'm assuming not, because there won't be a built wheel for  `"uv_build>=0.6.9,<0.7"` that is compatible with Python 3.16? And building an old `uv_build` from source for a new version of Python is also likely to fail.

So, if using `uv_build` means in general users shouldn't be relying on sdists for installing from other front end installers and should be relying on wheels that should be documented as a known issue that `uv_build` might not be appropriate for. This recent setuptools breaking change has shown a lot of users seemingly still depend on sdists, for whatever reasons.

---

_Comment by @konstin on 2025-03-25 22:04_

We're intentionally keeping `uv_build` small and compatible, hopefully making it forward compatible, especially on a short horizon such as Python 3.16. In that setup, the build backend should not be the bottleneck to using a library with a more modern Python version.

---

_Comment by @adamcik on 2025-04-06 11:02_

How is this supposed to work with building namespace packages? Looking at the options mentioned in https://github.com/astral-sh/uv/issues/8779 I tried configuring `module-root = "src"` and `module-name = "pkg"` which I would expect to be the "right" way of doing this.

> ## Options
> **tool.uv.build-backend.module-root**
> 
> The directory that contains the module directory, usually src, or an empty path when
> using the flat layout over the src layout.
> 
> **tool.uv.build-backend.module-name**
> 
> The name of the module directory inside module-root.
> 
> The default module name is the package name with dots and dashes replaced by underscores.
> 
> Note that using this option runs the risk of creating two packages with different names but the same module names. Installing such packages together leads to unspecified behavior, often with corrupted files or directory trees.


But this just results in the following error:

```
uv sync --frozen                                                                                                                                                                               
  × Failed to build `pkg-shared-testing @ file:///home/.../pkg`                                                                                     
  ├─▶ The build backend returned an error                                                                                                                                                      
  ╰─▶ Call to `uv_build.build_editable` failed (exit status: 1)                                                                                                                                
                                                                                                                                                                                               
      [stderr]                                                                                                                                                                                 
      Error: Expected an `__init__.py` at: `src/pkg/__init__.py`                                                                                                                                
                                                                                                                                                                                               
      hint: This usually indicates a problem with the package or the build environment.
```

But as I understand namespace packages I should not have `src/pkg/__init__.py`, but only e.g. `src/pkg/shared/testing/__init__.py` which is the root of this package.

For now I can just keep using an non uv build system, or try as suggested in https://github.com/astral-sh/uv/issues/3957#issuecomment-2745973229 and just have `pkg/__init__.py` in each package in our mono repo, as they all end up in the same venv any way.

But it would be good to either have this explicitly listed as not (yet) supported or to have a proper guidance on how to configure things with namespace packages, instead of having to have to run into the error like it did. Thanks!

---

_Comment by @merwok on 2025-04-07 14:25_

You’re trying to package pkg.shared.testing, so try this as module name, not pkg

(Not a uv user and haven’t checked the docs – in general, don’t expect tools to support namespace packages unless they say so)

---

_Comment by @adamcik on 2025-04-07 14:43_

I already tried that, just forgot to mention it. That didn't pass the validation for the option, so it didn't work. But thanks for the suggestion. 

And just to be clear, I'm not expecting help debugging my concrete issue here, that should probably be in a discussion, or wherever the maintainers want to redirect such things. I was just hoping to get a clarification of if namespaces is within scope now or perhaps later.

---

_Comment by @konstin on 2025-04-24 10:43_

@adamcik Tracking this in https://github.com/astral-sh/uv/issues/12832

---

_Comment by @konstin on 2025-04-24 10:44_

We've added reference documentation for the uv build backend to https://docs.astral.sh/uv/reference/settings/#build-backend and we're preparing more high-level documentation in https://github.com/astral-sh/uv/pull/12804, and are now looking for more feedback!

Assuming that we're only targeting universal Python wheels (no extensions modules with C/C++/Rust), would you adopt the uv build backend? What features are missing, are there any blockers for adoption, is the documentation understandable?

---

_Comment by @johnthagen on 2025-04-24 12:39_

> would you adopt the uv build backend?

Yep! Our primary motivation is to move away from `hatchling` and put our entire packaging pipeline under a single org/team (i.e. Astral/uv) so we have a single place to track releases/updates and communicate with rather than splitting attention between uv and hatch.

One feature we sometimes use from `hatchling` for applications is 

```toml
[tool.hatch.build]
packages = [
    "extra1",
    "extra2",
]
```

- https://hatch.pypa.io/latest/config/build/#packages

This is the same function as Poetry's

```toml
[tool.poetry]
packages = [
    { include = "extra1" },
    { include = "extra2" },
]
```

- https://python-poetry.org/docs/pyproject#packages

I didn't see an equivalent feature listed in the `uv_build` docs.

We're eager to move to `uv_build` and will start the process once it's out of preview.

---

_Comment by @chrisrodrigue on 2025-04-24 13:17_

To add to @johnthagen' comment, the equivalent function for `setuptools` is

```python
# setup.py

setup(
    name="seabird",
    packages=['albatross', 'albatross.beak', 'albatross.wings', 'albatross.tail'],
    # ...
)

---

_Comment by @eli-schwartz on 2025-04-24 13:58_

> Assuming that we're only targeting universal Python wheels (no extensions modules with C/C++/Rust), would you adopt the uv build backend? What features are missing, are there any blockers for adoption, is the documentation understandable?

No I wouldn't.

I would either stick with setuptools if I couldn't find a good reason to change, or adopt flit-core since it too targets universal python wheels but is easy to use, zero-configuration, and is itself written in pure python so is guaranteed to work anywhere my own code works.

uv-build has extremely limited OS / CPU availability and one of the major reasons to write universal python code is so that it is... universal. Being unable to e.g. install and test out a git commit on slightly unusual platforms because there is no way to build the project from source on all platforms feels quite unfortunate.

And it still isn't clear to me what actual advantages are provided by uv-build. What are its pros and cons compared to other build backends? Both flit-core and hatchling have a documentation page for "why use XXX (and when not to)", and meson-python / scikit-build-core both try to sell you on the advantages of `meson` or `cmake` respectively for extension modules in compiled languages.

But uv-build does not say what would make you want to use it (and even the linked documentation PR doesn't add one).

---

_Comment by @johnthagen on 2025-04-24 14:21_

> uv-build has extremely limited OS / CPU availability

Out of genuine curiosity, which platforms are you referring to that are missing?

- https://pypi.org/project/uv-build/0.6.16/#files

---

_Comment by @rossburton on 2025-04-24 14:25_

For Linux systems I can't see aarch64 on musl, 32-bit ppc, or any riscv/mips/loongarch.  Yes, I know commercials users of those.

---

_Comment by @konstin on 2025-04-24 14:28_

I'll point to https://github.com/astral-sh/uv/issues/3957#issuecomment-2657825266 which explains are motivations and trade-offs for `uv_build` and would otherwise prefer not to restart this conversation, but would rather talk about using the build backend on different kinds of projects with their different layouts and requirements. (I want give different people to chance to comment here before I start replying to the different points raised)

---

_Comment by @eli-schwartz on 2025-04-24 14:43_

> I'll point to [#3957 (comment)](https://github.com/astral-sh/uv/issues/3957#issuecomment-2657825266) which explains are motivations and trade-offs for `uv_build` and would otherwise prefer not to restart this conversation

I'm not asking you to restart the conversation.

I'm replying to your question:

> we're preparing more high-level documentation in [#12804](https://github.com/astral-sh/uv/pull/12804), and are now looking for more feedback

... with feedback about what parts of your documentation are lacking, from the perspective of someone who is *resigned* that he will not get a straightforward answer about the motivations and tradeoffs but would like whatever information is currently available to not require "inside information".

Please copy-paste your comment above, or something, into the documentation -- so that people can evaluate for themselves. :)

That is my feedback about your documentation PR.

---

_Comment by @konstin on 2025-04-24 14:51_

I see, thanks, we can include that in the docs.

---

_Comment by @johnthagen on 2025-04-25 11:41_

> is the documentation understandable

I would also recommend highlighting in the docs what the specific benefits (present or planned in the future) for using `uv_build` when _also_ using `uv` as the frontend. I believe that one of the long term goals (perhaps already implemented to some degree?) was to provide "fast paths" or other optimizations/conveniences when working with `uv` and using `uv_build` as the backend. I believe Hatch does something similar for hatchling.

This would help `uv` users decide the pros and cons of using `uv_build` vs another, non-special-cased-in-uv, build backend.

---

_Comment by @hankertrix on 2025-04-26 18:50_

> What features are missing, are there any blockers for adoption

The `uv` build backend doesn't support the default `uv init example-app` directory structure and fails to build for applications. Quite a number of applications do not need to be packaged or built, so the build backend is mainly just to build the dependencies from source if required, but the `uv` build backend doesn't support this use case at all.

Meanwhile, tools like `pdm` just require:
```toml
[tool.pdm.build]
includes = []

[build-backend]
requires = ["pdm-backend"]
build-backend = ["pdm.backend"]
```

Poetry is also similar:
```toml
[build-backend]
requires = ["poetry-core"]
build-backend = ["poetry.core.masonry.api"]
```

And it works just fine for applications that don't need to be built. I'm not sure what the `uv` equivalent is, but it seems that the directory structure must be followed for `uv` to even build dependencies as seen from the project generated by `uv init example-app --build-backend uv_build`, which is not desirable for existing codebases. Even with `module-root` set to `""`, `uv` still expects the flat directory structure to run `uv sync`.

I think this use case should be supported as I would no longer require some other build system from another package manager to build dependencies from source.

---

_Comment by @konstin on 2025-04-28 21:35_

@johnthagen Is this something you could solve by breaking the packages into a workspace, or do you specifically need the ability to have multiple packages in a single wheel?

@chrisrodrigue We include the entire folder by default, so if you have just `albatross` and `albatross.*`, the default configuration includes the correct packages.

> so the build backend is mainly just to build the dependencies from source if required, but the `uv` build backend doesn't support this use case at all.

@hankertrix Sorry I'm following here, why do you need a build backend to build dependencies from source?

---

_Comment by @johnthagen on 2025-04-28 22:02_

> @johnthagen Is this something you could solve by breaking the packages into a workspace, or do you specifically need the ability to have multiple packages in a single wheel?

So, in the use cases I've used and seen, it's less about building multiple packages into a wheel for a _library_ and more about having the build tool install editable installations for multiple top level packages for an _application_ that doesn't follow the `src/` layout.

For some applications like FastAPI, Django, etc. it's common not use a `src/` directory. In these applications, you sometimes want to have multiple top level packages, some of which might contain development-only utilities and such. Having them live in separate paths relative to `pyproject.toml` is convenient for only `COPY`ing certain folders into a container image and easily knowing you've excluded others.

In this scenario, we might still want the standard `uv sync` to editable install multiple top level "packages" into the virtual environment so the `PYTHONPATH` all works.

This is just one use case, I'm sure others have many more given that hatchling, Poetry, and setuptools at least all support this. It provides a lot of flexibility for applications that aren't following the standard `src/` + 1 package into a wheel format that most libraries on PyPI do.

---

_Comment by @hankertrix on 2025-04-28 22:06_

> Sorry I'm following here, why do you need a build backend to build dependencies from source?

Essentially, I’m trying to use `uv_build` instead of `setuptools` to build dependencies. During the install process, there is sometimes a building wheel section for some packages, and the build will fail without `setuptools`. This is the main way I use the Poetry and PDM build backends.



---

_Comment by @konstin on 2025-05-08 07:36_

Update from v0.7.3: The uv build backend now has both configuration and references documentation and is the default in preview (https://github.com/astral-sh/uv/pull/12804). Builds should now be reproducible between machines of the same platform and in most cases also independent of the  build platform (https://github.com/astral-sh/uv/pull/13171). We also added a way to escape characters in globs, extending over the PEP 639 syntax we previously used (https://github.com/astral-sh/uv/pull/13313).

---

_Comment by @Emasoft on 2025-05-10 12:39_

To truly make the builds reproducible between machines of the same platform (or even different platforms) you need to include in the build configuration the list of all uv tools used (I mean those installed with `uv tools install <tool name>`, like `nox-uv` or `tox-uv`, `bump-my-version`, etc.)  and all the specific versions of them. 

---

_Comment by @konstin on 2025-06-13 12:31_

In 0.7.13, we added support for namespaces. For packages with a single root module, you can set `module-name` for `src/cloud/sdk/db/__init__.py`:

```
[tool.uv.build-backend]
module-name = "cloud.sdk.db"
```

You can also set `namespace = true` for complex namespace packages with more than one root module.

The build backend now also supports stubs packages, including for namespace packages:

```
[tool.uv.build-backend]
module-name = "cloud-stubs.sdk.db"
```

Check the documentation for details: https://docs.astral.sh/uv/concepts/build-backend/

The core of the build backend should be feature complete with those additions and unless we find problems with the configuration, ready for stabilization. Stabilization here doesn't mean that we won't add more feature, but that all core features are present and ready for general use. As always, please try it out and report any problems!

---

_Comment by @johnthagen on 2025-06-13 13:56_

> The build backend now also supports stubs packages, including for namespace packages:

@konstin Was the example snippet in your comment supposed to be different under this line? It looks like the docs example is different.

I'd recommend explicitly showing an example that has multiple local packages

- https://github.com/astral-sh/uv/issues/3957#issuecomment-2827475439

After setting

```toml
[tool.uv.build-backend]
namespace = true
```

How does one do the equivalent of 

```toml
[tool.poetry]
packages = [
    { include = "extra1" },
    { include = "extra2" },
]
```

- https://python-poetry.org/docs/pyproject#packages

I couldn't work this out easily by reading https://docs.astral.sh/uv/concepts/build-backend/

**Edit**: Perhaps this is `tool.uv.build-backend.source-include`? An explicit example showing that if true I think would help for this common scenario.

---

_Comment by @thejcannon on 2025-06-22 19:04_

👋 (Wasn't sure whether to tack this on here, or open a new issue. Happy to take this to a new issue)

Can I make a feature request for `uv_build` to allow parent directories in (certain) `pyproject.toml` metadata values?
Specifically, using `uv` in a workspace-powered monorepo, I'd preferably have `license = { file = "../LICENSE" }` instead of symlinking the LICENSE everywhere. 

RIght now I see:
```
Caused by:
    The parent directory operator (`..`) at position 0 is not allowed in glob: `../LICENSE`
```

---

_Comment by @konstin on 2025-06-22 19:34_

Shared files in workspaces is an important unsolved problem! When building a source distribution, how would you expect the license file (or other relative files) to be stored in source distribution? For context, the `pyproject.toml` needs to the highest file in the source distribution, so we can't have files above it. (For license specifically, there's now PEP 639 which allows including a number of license files with a glob, but explicitly bans parent directory globs: https://peps.python.org/pep-0639/#add-license-files-key)

---

_Comment by @potiuk on 2025-06-22 21:16_

I think symbolically linked file is fine. Or you can use `pre-commit` to copy files.
 What is important is that in monorepo, all the distributions you have are "standalone" - i.e. you should be able to do:

* cd your_distribution_director
* whatever PEP compliant tool `build`

This basically means, that (until workspace approach has a `PEP` that is approved and implemented by many tools) - the "distribution" subfolder should have all the files that are needed to build the distribution.



---

_Comment by @potiuk on 2025-06-22 21:18_

BTW. This is what we do in Airflow - with our ~ 100 distributions we keep in monorepo. Whole workspace is cool for development and being able to install the distributions together, being able to just take the whole distribution folder extracted and being able to run "build" on it to build it - is important feature of good "workspace" 

---

_Comment by @thejcannon on 2025-06-23 02:01_

I assume when building an sdist, it'd be permissible to copy the `LICENSE` file and edit the `pyproject.toml`. This maybe doesn't hold true to other fields, but certainly for standalone files there's no measurable difference.

On the topic of symlinks though...
```
Error: Unsupported file type FileType { is_file: false, is_dir: false, is_symlink: true, .. }: `LICENSE`
```

which I suppose is consistent with the "hey, man I don't really know what you want me to do, exactly, so I'm just gonna bail" thing 😄 

---

_Comment by @konstin on 2025-06-23 09:55_

@johnthagen While I recommend using a one package per top level module and modelling this with workspaces instead (it's very easy to get clashes between packages or otherwise confusing outcomes otherwise), it is possible to include multiple top level modules:

```toml
[tool.uv.build-backend]
module-root = "src"
module-name = ""
namespace = true
```

Note that this requires all modules to be in a directory (`module-root = "src"` is the default, I've included it for clarity) and it will include everything below that directory.

---

_Comment by @konstin on 2025-06-23 09:58_

I've put up a PR for allowing to read from symlinks: https://github.com/astral-sh/uv/pull/14212

I'm not 100% sure yet this is the right call as there are drawbacks, such as that symlinks don't generally work on Windows, so Git turns it into a copy of the target file, and that we do effectively allow paths to point outside the project directory this way.

---

_Comment by @konstin on 2025-07-03 17:36_

The uv build backend is now stable - ready for production - as of 0.7.19! See the docs on how to gets started: https://docs.astral.sh/uv/concepts/build-backend/

We will follow up with with problems and feature requests (please open issues!). We don't cover everything a uv build system could do in this release, you can feature requests under the [issues with the build backend tag](https://github.com/astral-sh/uv/issues?q=state%3Aopen%20label%3A%22build-backend%22), and, as usual, find new features in our release notes.

One change that's in the pipeline that I can already announce is that we'll make the uv build backend the default in `uv init` (which is currently still hatchling as it's a behavior change).

---

_Closed by @konstin on 2025-07-03 17:36_

---
