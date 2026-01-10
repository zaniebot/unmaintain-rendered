```yaml
number: 2039
title: Add option pass environment variables for SDist building
type: pull_request
state: merged
author: tdejager
labels:
  - rustlib
assignees: []
merged: true
base: main
head: feat/env-variables-for-sdist-building
created_at: 2024-02-28T08:45:43Z
updated_at: 2024-02-29T11:03:49Z
url: https://github.com/astral-sh/uv/pull/2039
synced_at: 2026-01-10T14:54:43Z
```

# Add option pass environment variables for SDist building

---

_Pull request opened by @tdejager on 2024-02-28 08:45_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

With this PR I've added the option environment variables to the wheel building process, through the `BuildDispatch`. When integrating uv with our project pixi (https://github.com/prefix-dev/pixi/pull/863). We ran into this missing requirement, I've made a rough version here, could maybe use some refinement. 

### Why do we need this?

Because pixi allow the user to use a conda activated prefix for wheel building, this comes with a number of environment variables, like `PATH` but also `CONDA_PREFIX` amongst others. This allows the user to use system dependencies from conda-forge to use during an sdist build. Because we use `uv` as a library we need to pass in the options programatically. Additionally, in general there is nothing holding a python sdist back from actually depending on an environment variable, see 
e.g the test package: https://pypi.org/project/env-test-package/

### What about `ConfigSettings`

I think `ConfigSettings` does not suffice because e.g. CMake could function differently when the `CONDA_PREFIX` is set. Also, we do not know if the user supplied backend actually support these settings.

### Path handling

Because the user can now also supply a PATH in the environment map, the logic I had was the following, I format the path so that it has the following precedence

1. venv scripts dir.
2. user supplied path.
3. system path.

### Improvements

There is some path modification and copying happening everytime we use the `run_python_script` function, I think we could improve this but would like some pointers where to best put the maybe split and cached version, we might also want to use some types to split these things up.


### Finally

I did not add any of these options to the uv executables, I first would like to know if this is a direction we would want to go in. I'm happy to do this or make any changes that you feel would benefit this project.

Also tagging @wolfv to keep track of this as well.  

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:874 on 2024-02-28 09:41_

Shouldn't the caller determine the entire `PATH`? Then we also wouldn't need to modify it and could pass it in as a reference.

---

_Review comment by @konstin on `crates/uv-dispatch/Cargo.toml`:32 on 2024-02-28 09:42_

```suggestion
```

---

_Review comment by @konstin on `crates/uv-dispatch/src/lib.rs`:40 on 2024-02-28 09:45_

This should be named something like `build_extra_env_vars`

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:920 on 2024-02-28 09:47_

Could we put the `.envs(environment_variables)` before the `.env("VIRTUAL_ENV", venv.root())` to ensure `VIRTUAL_ENV` never gets overridden?

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:336 on 2024-02-28 09:50_

Do we want the keys to be `String` or `OsString`? This kinda depends where you get them from, but if it's from other os apis `OsString` would remove a potentially erroneous conversion.

---

_@konstin approved on 2024-02-28 09:51_

---

_Comment by @tdejager on 2024-02-28 11:07_

Thanks @konstin I will look at the comments tomorrow, it's my free day with the kids, they don't allow for programming ;)

Thanks for the quick review!

---

_@tdejager reviewed on 2024-02-28 11:22_

---

_Review comment by @tdejager on `crates/uv-build/src/lib.rs`:874 on 2024-02-28 11:22_

Sure and you want to get the env var at the caller site as well?

---

_@tdejager reviewed on 2024-02-28 11:23_

---

_Review comment by @tdejager on `crates/uv-build/src/lib.rs`:336 on 2024-02-28 11:23_

Very good idea!

---

_@konstin reviewed on 2024-02-28 12:19_

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:874 on 2024-02-28 12:19_

I hope i got this in the right order, but i'd try to get `PATH` from `environment_variables` first and then from the real env, prepend the build venv and then pass to the command so that our now computed `PATH` takes precedence over `.envs(environment_variables)`

---

_Label `rustlib` added by @zanieb on 2024-02-28 17:47_

---

_@tdejager reviewed on 2024-02-29 10:22_

---

_Review comment by @tdejager on `crates/uv-dispatch/src/lib.rs`:40 on 2024-02-29 10:22_

Done

---

_@tdejager reviewed on 2024-02-29 10:22_

---

_Review comment by @tdejager on `crates/uv-build/src/lib.rs`:336 on 2024-02-29 10:22_

I changed this 👍 

---

_@tdejager reviewed on 2024-02-29 10:23_

---

_Review comment by @tdejager on `crates/uv-build/src/lib.rs`:874 on 2024-02-29 10:23_

So I've now changed this to kreep track of the path seperately, so they are computed one and passed in by reference.

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:336 on 2024-02-29 10:24_

```suggestion
    /// Modified PATH that contains the `venv_bin`, `user_path` and `system_path` variables in that order
```

Rust docstrings are markdown

---

_@konstin approved on 2024-02-29 10:24_

Clippy is failing, otherwise it's ready to be merged!

---

_Comment by @tdejager on 2024-02-29 10:27_

Last question I had @konstin is that the behavior is now that if NO path variable is set, that the venv bin is not added to the path of the python script that is run. I'm unsure if this is the correct behavior, wouldn't you want the venv bin directory to always be added to the path?

---

_Comment by @konstin on 2024-02-29 10:29_

The venv bin should always be added, i didn't consider it because i never saw an environment where `PATH` was empty/unset

---

_Comment by @tdejager on 2024-02-29 10:29_

Okay let me change that, sure that should not happen a lot, but just to be sure.

---

_Comment by @tdejager on 2024-02-29 10:31_

> The venv bin should always be added, i didn't consider it because i never saw an environment where `PATH` was empty/unset

Should be done!

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:496 on 2024-02-29 10:50_

```suggestion
                        .map_err(|err| {
                            Error::RequirementsInstall("setup.py build (resolve)", err)
                        })?;
```

We want to keep that message

---

_@konstin reviewed on 2024-02-29 10:50_

---

_@tdejager reviewed on 2024-02-29 10:51_

---

_Review comment by @tdejager on `crates/uv-build/src/lib.rs`:496 on 2024-02-29 10:51_

Wow, thanks, thats sloppy of me.

---

_Merged by @konstin on 2024-02-29 10:55_

---

_Closed by @konstin on 2024-02-29 10:55_

---

_Branch deleted on 2024-02-29 11:03_

---
