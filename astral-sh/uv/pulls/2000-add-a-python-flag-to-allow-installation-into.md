```yaml
number: 2000
title: "Add a `--python` flag to allow installation into arbitrary Python interpreters"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
assignees: []
merged: true
base: main
head: charlie/py
created_at: 2024-02-27T01:48:00Z
updated_at: 2024-03-20T00:00:11Z
url: https://github.com/astral-sh/uv/pull/2000
synced_at: 2026-01-12T16:04:50Z
```

# Add a `--python` flag to allow installation into arbitrary Python interpreters

---

_@charliermarsh_

## Summary

This PR adds a `--python` flag that allows users to provide a specific Python interpreter into which `uv` should install packages. This would replace the `VIRTUAL_ENV=` workaround that folks have been using to install into arbitrary, system environments, while _also_ actually being correct for installing into non-virtual environments, where the bin and site-packages paths can differ.

The approach taken here is to use `sysconfig.get_paths()` to get the correct paths from the interpreter, and then use those for determining the `bin` and `site-packages` directories, rather than constructing them based on hard-coded expectations for each platform.

Closes https://github.com/astral-sh/uv/issues/1396.

Closes https://github.com/astral-sh/uv/issues/1779.

Closes https://github.com/astral-sh/uv/issues/1988.

## Test Plan

- Verified that, on my Windows machine, I was able to install `requests` into a global environment with: `cargo run pip install requests --python 'C:\\Users\\crmarsh\\AppData\\Local\\Programs\\Python\\Python3.12\\python.exe`, then `python` and `import requests`.
- Verified that, on macOS, I was able to install `requests` into a global environment installed via Homebrew with: `cargo run pip install requests --python $(which python3.8)`.

---

_Converted to draft by @charliermarsh on 2024-02-27 01:48_

---

_Label `bug` added by @charliermarsh on 2024-02-27 01:56_

---

_Label `enhancement` added by @charliermarsh on 2024-02-27 01:56_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-27 01:56_

---

_Review requested from @konstin by @charliermarsh on 2024-02-27 01:56_

---

_Marked ready for review by @charliermarsh on 2024-02-27 01:56_

---

_Converted to draft by @charliermarsh on 2024-02-27 02:03_

---

_Comment by @charliermarsh on 2024-02-27 02:13_

Marking as draft, one important thing to fix here when creating environments for isolated builds (hence test failures). I see the problem.

---

_@zanieb reviewed on 2024-02-27 03:57_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:442 on 2024-02-27 03:57_

It feels weird to consider installing packages into an interpreter. Perhaps we should rephrase?

Separately, "into which to" is really confusing

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:442 on 2024-02-27 04:25_

Focused on other things right now, I'll come back to this comment later.

---

_@charliermarsh reviewed on 2024-02-27 04:25_

---

_Marked ready for review by @charliermarsh on 2024-02-27 05:18_

---

_@charliermarsh reviewed on 2024-02-27 05:19_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:104 on 2024-02-27 05:19_

\cc @konstin - I think `include` is the same in a venv as not...

---

_@charliermarsh reviewed on 2024-02-27 05:20_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:398 on 2024-02-27 05:20_

\cc @konstin - But when installing, there's custom logic. (We had this in `install-wheel-rs`, which prior to this PR assumed a virtualenv.)

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:82 on 2024-02-27 05:21_

@konstin - AFAICT, `base_prefix` doesn't change in a virtualenv...?

```text
uv on  charlie/py:main [$⇡] is 📦 v0.1.11 via 🐍 v3.11.7 (uv) via 🦀 v1.76.0
❯ python
Python 3.11.7 (main, Dec  4 2023, 18:10:11) [Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.base_prefix
'/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11'
```

---

_@charliermarsh reviewed on 2024-02-27 05:21_

---

_@charliermarsh reviewed on 2024-02-27 05:59_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:398 on 2024-02-27 05:59_

If you look at the linked pip source, they also have this logic around `user` and `home`. Is that significant here?

---

_@charliermarsh reviewed on 2024-02-27 06:06_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:34 on 2024-02-27 06:06_

The client now passes in a struct that contains all the paths we need to use here (rather than passing in a root, and expecting this crate to construct paths).

Previously, this crate was responsible for constructing those paths, and it _assumed_ that it was installing into a virtual environment.

---

_Comment by @MichaReiser on 2024-02-27 07:22_

`cargo run pip install requests --python 'C:\\Users\\crmarsh\\AppData\\Local\\Programs\\Python\\Python3.12\\python.exe` this is confusing. I thought for a moment that it should be `cargo run uv` and not pip :)

Can you extend the test plan with a package that has a launcher script and starting that:

```
uv pip install --python <path> black
black -h
```

---

_Review comment by @MichaReiser on `crates/uv/src/main.rs`:688 on 2024-02-27 07:33_


```suggestion
    /// The Python interpreter into which packages should be uninstalled.
```

?

---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/wheel.rs`:236 on 2024-02-27 07:35_

Nit: I wasn't aware that this path must be relative. Should we add at least a `debug_assert` to `write_file_recorded` that asserts that `relative_path` is relative?

---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/wheel.rs`:229 on 2024-02-27 07:37_

What are cases where this can happen? Is it when entrypoint and site packages aren't pointing to the same drive on windows? Is this a possible solution? 



---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/target.rs`:4 on 2024-02-27 07:38_

Nit: Maybe move to `lib.rs`?

Nit: Consider renaming to `TargetEnvironment`. It seems we have multiple existing `Target` types, giving it a unique name helps with searchability (e.g. VS Code only Find all references searches by the struct name and not the full qualified name)


---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/wheel.rs`:199 on 2024-02-27 07:52_

Nit: This is not a very actionable comment so please feel free to disregard it (especially for this PR). 

I feel like that there are a couple of concept in this file that aren't represented by explicit types and, instead, we pass different, somehow related arguments to many functions (which probably have to fulfill certain constraints but they aren't explicitly expressed). I find that this makes the functions hard to understand and use (so many arguments, uff, I don't know what they are ;)). Having more explicit construct might also allow to make some of these functions methods and, by doing so, reduce the number of arguments that need to be passed in each call. But I don't understand this code and concepts behind it well enough to make any suggestions.

---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/linker.rs`:92 on 2024-02-27 07:53_

Nit: maybe just pass in the `target` and let `write_script_entrypoints` figure out which paths are relevant

---

_@MichaReiser reviewed on 2024-02-27 08:07_

The code looks good to me. I'm leaving it to someone else who understands packaging to approve the PR.

Even after reading the linked issues, I still don't fully understand its use case.  I struggle because what I read is mainly the ask to add a `pip` compatible interface, but not the problem that needs solving (what are they trying to do) and why creating and activating a venv is undesired. 

I'm not saying that increasing `pip` compatibility is bad, but I worry that adding support hinders the ecosystem from adopting a better solution. E.g, most `uv` installs use `pip`, even though installing it with pip can be slower than running `uv venv` and `uv pip install`. Using the scripts would give them a better experience. Here I see the problem with us because we don't provide an easy way of using `uv` in github actions (CI) yet that abstracts all this away.

I'm bringing this up because I'm curious if it's related to the global install problem. Could a GitHub action that installs `uv`, activates the `venv` and installs the dependencies solve the CI part of the problem? If so, would alternative solutions work for the remaining non CI workflows that don't require supporting global installs (The fact that you can uninstall packages from a global directory seems crazy to me)? 


---

_Comment by @MichaReiser on 2024-02-27 11:13_

Testing: I'll migrate the workflow mentioned in https://github.com/astral-sh/uv/issues/1779 to using the new flag 

It doesn't have to be part of this PR but should we warn or even error if we detect that `VIRTUAL_ENV` points to a directory that isn't a virtual env? We may need to warn in a first version to avoid breaking existing workflows, but I consider it an unsupported workflow that can break subtly.

---

_Review comment by @konstin on `crates/install-wheel-rs/src/target.rs`:11 on 2024-02-27 11:24_

```suggestion
    /// The `include` directory, as returned by `sysconfig.get_paths()`.
```

---

_Review comment by @konstin on `crates/install-wheel-rs/src/target.rs`:4 on 2024-02-27 11:25_

Maybe `InstallationLayout`

---

_Review comment by @konstin on `crates/install-wheel-rs/src/wheel.rs`:219 on 2024-02-27 11:28_

How's that different from `Path::strip_prefix`?

If we need it, can we copy the function into `uv_fs` (https://github.com/Manishearth/pathdiff/blob/a78acde0955a556b2bdf11e6a9efae459c4bcf80/src/lib.rs#L34-L77)? The repo looks abandoned and there are no docs.

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:82 on 2024-02-27 11:32_

Yes

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:104 on 2024-02-27 11:33_

Odd, the docs make it look as if should also change.

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:398 on 2024-02-27 11:35_

I don't think i ever had to deal with `include`, sorry, no idea

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:370 on 2024-02-27 11:36_

Shouldn't this be `!=`, if the next branch is for venvs?

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:389 on 2024-02-27 11:37_

I'd go with `SysconfigInstallationPaths` or `SysconfigInstallationPaths`

---

_Comment by @MichaReiser on 2024-02-27 11:39_

I tested #1779 and using `--python` works (see https://github.com/MichaReiser/tox-uv/actions/runs/8064054107/job/22027134724?pr=1). However, I still don't consider this a good experience because the commands are not platform agnostic (`python.exe` vs `python`) and it requires a lot of fumbling. I think we should recommend using a venv instead. But that has it's own problem because the commands for activating a venv are platform specific too. 

That's why I think we need to be careful about communicating this change. We shouldn't communicate it as *the solution* for CI. CI should use `uv venv`, `<activate venv>` and `uv install`. This is painful because it involves many platform agnostic steps (including the installation of `uv`) but leads to a better experience overall. Ultimately, we should build a setup action instead. 



---

_Review comment by @konstin on `crates/uv-interpreter/src/virtual_env.rs`:21 on 2024-02-27 11:44_

Is calling it `Virtualenv` still correct?

---

_@konstin requested changes on 2024-02-27 11:45_

This needs https://packaging.python.org/en/latest/specifications/externally-managed-environments/ or we will break environments

---

_Comment by @charliermarsh on 2024-02-27 13:44_

@konstin - so we’re just supposed to raise an error if the installation is externally managed? That’s my read of the spec.

---

_@charliermarsh reviewed on 2024-02-27 13:45_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:199 on 2024-02-27 13:45_

(For context, this was vendored in from an existing crate, so it’s evolved somewhat separately from the rest of the project.)

---

_@charliermarsh reviewed on 2024-02-27 14:33_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:104 on 2024-02-27 14:33_

Which docs? Link please!

---

_Comment by @konstin on 2024-02-27 14:33_

Yes

---

_Comment by @charliermarsh on 2024-02-27 14:34_

> Even after reading the linked issues, I still don't fully understand its use case. I struggle because what I read is mainly the ask to add a pip compatible interface, but not the problem that needs solving (what are they trying to do) and why creating and activating a venv is undesired.

@MichaReiser - My read of the commentary in the issues is that this is primarily for CI and Docker environments. In both cases, I believe that creating and activating a virtual environment would work, but that doing so tends to be inconvenient and unusual. (E.g., there were some issues stemming from the fact that in GHA, you need to activate the environment in every step, rather than once.)


---

_@charliermarsh reviewed on 2024-02-27 14:34_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:389 on 2024-02-27 14:34_

I will do `SysconfigPaths`, since that matches the name of the method.

---

_@charliermarsh reviewed on 2024-02-27 14:51_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:370 on 2024-02-27 14:51_

No, the first branch is for venvs, right? See the linked pip source.

---

_@charliermarsh reviewed on 2024-02-27 14:54_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:219 on 2024-02-27 14:54_

Trying to understand, why would `strip_prefix` work here? The entrypoint is not a subdirectory of `site_packages` (if you look at `bin_rel()` previously, it started with `../..`).

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/virtual_env.rs`:21 on 2024-02-27 14:54_

I'm open to other names. What would you suggest? `PythonEnvironment`?

---

_@charliermarsh reviewed on 2024-02-27 14:54_

---

_Comment by @charliermarsh on 2024-02-27 14:56_

> That's why I think we need to be careful about communicating this change. We shouldn't communicate it as the solution for CI. CI should use uv venv, <activate venv> and uv install. This is painful because it involves many platform agnostic steps (including the installation of uv) but leads to a better experience overall. Ultimately, we should build a setup action instead.

I'm not totally sold on this. If it were a better experience, wouldn't folks be doing it already? Why does `--python $(which python)` feel like a "lot of fumbling"?

Edit: is it more that you have to construct the Python path on Windows?'

Edit: if the latter, I want to add a separate `--system` flag that automates the lookup of the Python flag for this common case. So running in CI is just `--system`. (I think we're doing a good thing by defaulting to virtual environments and requiring dedicated opt-out for other workflows, even though it differs from pip and others.)

---

_@konstin reviewed on 2024-02-27 14:57_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/wheel.rs`:219 on 2024-02-27 14:57_

I see, makes sense

---

_Comment by @MichaReiser on 2024-02-27 15:01_

> I'm not totally sold on this. If it were a better experience, wouldn't folks be doing it already? Why does --python $(which python) feel like a "lot of fumbling"?

I don't think that works on windows. You would probably need to use `where.exe`, meaning you end up with platform specific steps.

> My read of the commentary in the issues is that this is primarily for CI and Docker environments. In both cases, I believe that creating and activating a virtual environment would work, but that doing so tends to be inconvenient and unusual. (E.g., there were some issues stemming from the fact that in GHA, you need to activate the environment in every step, rather than once.)

For me the question is why is it inconvenient in CI? I believe it is mainly so because we don't provide a better way of doing it. A `setup-uv` action could install `uv`, create the venv for you, set the `VIRTUAL_ENV` environment variable and all following steps would "just work" without the need for any platform specific code. 

I don't know much about docker but a single `uv venv` doesn't seem that bad to me? But I suspect that there are use cases that I don't understand.

>  (E.g., there were some issues stemming from the fact that in GHA, you need to activate the environment in every step, rather than once.)

That's true, but an action could set the `VIRTUAL_ENV` environment variable, so that this isn't necessary. This is okay because we know it points to a venv created by `uv`.

Edit: Another way to phrase this is: We should document what the recommended way of using `uv` in docker and CI is. I don't think it is what's outlined above, but it would allow decoupling the decision from this PR (which I think we should)

---

_@charliermarsh reviewed on 2024-02-27 15:08_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:370 on 2024-02-27 15:08_

Oh I got the condition wrong! The condition is inverted. I see.

---

_@konstin reviewed on 2024-02-27 15:16_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:104 on 2024-02-27 15:16_

https://docs.python.org/3/library/sysconfig.html#installation-paths

But i don't disagree, `include` is also an outlier for me:

```
Paths: 
        data = "/home/konsti/projects/uv/.venv"
        include = "/home/konsti/.pyenv/versions/3.10.13/include/python3.10"
        platinclude = "/home/konsti/.pyenv/versions/3.10.13/include/python3.10"
        platlib = "/home/konsti/projects/uv/.venv/lib/python3.10/site-packages"
        platstdlib = "/home/konsti/projects/uv/.venv/lib/python3.10"
        purelib = "/home/konsti/projects/uv/.venv/lib/python3.10/site-packages"
        scripts = "/home/konsti/projects/uv/.venv/bin"
        stdlib = "/home/konsti/.pyenv/versions/3.10.13/lib/python3.10"
```

Same with the apt installed python as base:

```
Paths: 
        data = "/home/konsti/projects/uv/.venv"
        include = "/usr/include/python3.11"
        platinclude = "/usr/include/python3.11"
        platlib = "/home/konsti/projects/uv/.venv/lib/python3.11/site-packages"
        platstdlib = "/home/konsti/projects/uv/.venv/lib/python3.11"
        purelib = "/home/konsti/projects/uv/.venv/lib/python3.11/site-packages"
        scripts = "/home/konsti/projects/uv/.venv/bin"
        stdlib = "/usr/lib/python3.11"
```

---

_Comment by @AlexWaygood on 2024-02-27 15:20_

I don't think telling people "you have to use our GitHub Action to use `uv` in CI" is a good solution:
- To me, GitHub Actions are things you use to simplify a bunch of otherwise-complex steps. Saying you have to use a GitHub Action here feels like an implicit admission that setting up uv to work well in CI is complex -- it shouldn't be!
- I think it's easiest for users to understand if uv is as similar as possible to use in CI compared to how they use it locally. They obviously won't be using a GitHub Action locally ;)
- Users never needed to use a GitHub Action for pip; it'll come as a surprise to them that they need to for uv

I like the idea of a bare-bones GitHub Action to do _just the first step_ (installation of `uv` itself), since the only platform-independent way of doing that currently is via pip, which is slow. But I'd really like for the rest of the `uv` experience to be simple enough that you can do it without using an Action. So while I absolutely agree that `uv`'s default of disallowing installation when a venv isn't activated is the better default, I still think a `--system` flag is the best option for CI, personally.

---

_Review requested from @konstin by @charliermarsh on 2024-02-27 15:21_

---

_@charliermarsh reviewed on 2024-02-27 15:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:89 on 2024-02-27 15:59_

I will do this in a follow-up PR.

---

_Comment by @MichaReiser on 2024-02-27 16:12_

> I don't think telling people "you have to use our GitHub Action to use uv in CI" is a good solution:

I agree on that. Ideally it works without

> I think it's easiest for users to understand if uv is as similar as possible to use in CI compared to how they use it locally.

I agree, but people are using `venv` locally, so why not use them in CI? It means that what you run on CI is different than what you run locally


To not sidetrack this discussion further. I would like to see a follow-up where we document the recommended workflow of using `uv` in CI and docker, where we can point users to (and discuss the ideal workflow once).


---

_Comment by @AlexWaygood on 2024-02-27 16:19_

> I agree, but people are using `venv` locally, so why not use them in CI?

Sure, but locally creating the venv is typically a one-time thing you do when cloning the repo for the first time and setting up your dev environment for that project (at least, it is for me), whereas in CI it has to be done every time, as you're starting from scratch every time. And the steps you'd do locally to activate the environment (`source .venv/bin/activate` or `.venv\Scripts\activate`) are totally different to the steps you need in CI, where you have to `echo` the path to the venv and the path to the venv's Python into environment variables if you want the venv to remain activated between steps

> It means that what you run on CI is different than what you run locally

But I don't think there _should_ be a meaningful difference between installing into a venv locally and installing globally in CI, since the CI environment is just discarded at the end of the workflow.

> I would like to see a follow-up where we document the recommended workflow of using `uv` in CI and docker, where we can point users to (and discuss the ideal workflow once).

Yes, fully agreed (and already tracked in #1386 / #1604 ;).

---

_Review comment by @konstin on `crates/uv-interpreter/src/virtual_env.rs`:21 on 2024-02-27 20:23_

`InterpreterRoot` maybe? Almost tempted to name it something related to site packages

---

_@konstin approved on 2024-02-27 20:24_

---

_@samypr100 reviewed on 2024-02-27 23:53_

---

_Review comment by @samypr100 on `crates/uv-interpreter/src/interpreter.rs`:104 on 2024-02-27 23:53_

Depends on the build headers I think? There was an issue a while ago with deadsnakes changing them to `/usr/local/include`  vs canonical `/usr/include` the headers had https://github.com/deadsnakes/issues/issues/237

---

_Comment by @charliermarsh on 2024-02-28 02:09_

Gonna put up some follow-up PRs. Merging for now.

---

_Merged by @charliermarsh on 2024-02-28 02:10_

---

_Closed by @charliermarsh on 2024-02-28 02:10_

---

_Branch deleted on 2024-02-28 02:10_

---

_Comment by @charliermarsh on 2024-02-28 02:10_

https://github.com/astral-sh/uv/pull/2000/commits/518e74b7ee095c669735b60ab170cfadeb66b5d5 contains a test script and a GitHub Actions workflow to install on system python on macOS and Linux. (I tried to get it to work on Windows, but I wasted too much time trying to get GitHub Actions to work.)

---

_Comment by @wimglenn on 2024-03-18 02:27_

Hi @charliermarsh , this is a very useful feature, thanks! I have a couple of questions on usage.

Q1: ~is it expected that using a relative path is not working (_edit_: on macOS - seems to work fine on Linux)?~

```
$ uv pip install --dry-run --python .venv/bin/python six 
error: Failed to query Python interpreter at `.venv/bin/python`
  Caused by: No such file or directory (os error 2)
```

~An absolute path to the same python exe is working:~

```
$ pwd
/Users/wim
$ uv pip install --dry-run --python /Users/wim/.venv/bin/python six
Audited 1 package in 1ms
Would make no changes
```

_**edit**: I can't reproduce this on 0.1.22. I checked again after downgrading back to 0.1.21 (where I originally saw the issue) and could no longer reproduce it there either... 🤷_

Q2: how do you actually use this feature with `uv pip compile`? I couldn't figure it out:

```
$ cat reqs.in                                                            
psycopg2-binary; implementation_name == 'cpython'
psycopg2; implementation_name != 'cpython'
$ uv pip compile reqs.in --python-version /tmp/pypy/.venv/bin/python
error: invalid value '/tmp/pypy/.venv/bin/python' for '--python-version <PYTHON_VERSION>': expected version to start with a number, but no leading ASCII digits were found

For more information, try '--help'.
$ uv pip compile reqs.in --python /tmp/pypy/.venv/bin/python        
error: unexpected argument '--python' found

  tip: a similar argument exists: '--python-version'

Usage: uv pip compile --python-version <PYTHON_VERSION> <SRC_FILE>...

For more information, try '--help'.
```

It works with `uv pip install` as expected.

```
$ uv pip install --dry-run --python /tmp/cpython/.venv/bin/python -r reqs.in
Resolved 1 package in 3ms
Would download 1 package
Would install 1 package
 + psycopg2-binary==2.9.9
$ uv pip install --dry-run --python /tmp/pypy/.venv/bin/python -r reqs.in   
Resolved 1 package in 3ms
Would download 1 package
Would install 1 package
 + psycopg2==2.9.9
```


---
