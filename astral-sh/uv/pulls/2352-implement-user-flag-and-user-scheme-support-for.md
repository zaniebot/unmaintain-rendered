```yaml
number: 2352
title: "Implement `--user` flag and user scheme support for `uv pip`"
type: pull_request
state: closed
author: imfing
labels:
  - compatibility
assignees: []
base: main
head: pip-user-scheme
created_at: 2024-03-11T08:06:26Z
updated_at: 2024-06-15T11:43:49Z
url: https://github.com/astral-sh/uv/pull/2352
synced_at: 2026-01-12T16:04:59Z
```

# Implement `--user` flag and user scheme support for `uv pip`

---

_@imfing_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Hi team, excited for your awesome work of `uv`. 

This PR attempts to implement `--user` flag for `uv pip` subcommands: `install`, `freeze`, `list` and `show`.

Closes #2077 

also #1584

Per [PEP 370 – Per user site-packages directory](https://peps.python.org/pep-0370/), `pip` supports [user scheme package installation](https://pip.pypa.io/en/stable/user_guide/#user-installs) which would be useful for multi-user environment or CI use cases without the need of virtual environment or breaking system Python packages.

Example usage:

```shell
export PYTHONUSERBASE=~/.local/myappenv
uv pip install --user SomePackage
# SomePackage is available under ~/.local/myappenv
```

#### Implementation details

The implementation aims to bring minimal changes to the existing functionalities of `uv pip`, as well as supporting `--user` for pip compatibility.

Here are some breakdowns of the implementation:

* add [`_infer_user()`](https://github.com/pypa/pip/blob/ae5fff36b0aad6e5e0037884927eaa29163c0611/src/pip/_internal/locations/_sysconfig.py#L86) to `crates/uv-interpreter/src/get_interpreter_info.py` which returns the user scheme if specified
* add `PythonEnvironment::from_user_scheme()` which queries the interpreter info with user scheme, thus the `platlib`, `purelib`, `scripts`, `data` and `include` should point to the user site
* use the user scheme Python environment for `uv pip` subcommands when the `--user` flag is present, making the subcommands use *only* the user site for package search and installation.

Right now the implementation doesn't include the global packages mentioned in [User Installs](https://pip.pypa.io/en/stable/user_guide/#user-installs) since we only consider site packages from the `scheme.purelib`. I'm thinking about adding that later for better compatibility with `pip install`.

I am new to `uv`, looking forward to the team's feedback and suggestions :)

## Test Plan

<!-- How was it tested? -->

May need some advice and guidance on what's the best practice on testing this.

---

_Label `compatibility` added by @konstin on 2024-03-11 08:49_

---

_Review requested from @charliermarsh by @konstin on 2024-03-11 08:49_

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_environment.rs`:100 on 2024-03-11 08:50_

```suggestion
                fs_err::create_dir_all(path)?;
```
We use `fs_err` over `std::fs` so errors have paths attach

---

_@konstin reviewed on 2024-03-11 08:50_

---

_Comment by @charliermarsh on 2024-03-11 17:54_

This looks like a nice PR! The main blocker is that I need to decide if we want to support `--user`. I think it leads to some limitations in `pip` but I just haven't had time to investigate it. For example, it was suggested elsewhere that `--user` and `--editable` are no longer supported in `pip` -- is that true?

---

_@charliermarsh reviewed on 2024-03-11 21:20_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:455 on 2024-03-11 21:20_

Does this intentionally _not_ cache?

---

_Comment by @charliermarsh on 2024-03-11 21:21_

Again, just want to say: this is very nice work! In fact I'm more inclined to support this now that we have a nice PR for it. But I do want to understand the implications a little more first.

---

_@charliermarsh reviewed on 2024-03-11 21:21_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:152 on 2024-03-11 21:21_

Can we use `is_some_and` or `unwrap_or` here instead of unwrapping?

---

_Comment by @imfing on 2024-03-12 07:43_

Thank you for the comments 😄 

> For example, it was suggested elsewhere that `--user` and `--editable` are no longer supported in `pip` -- is that true?

It seems there might be some confusion stemming from an issue discussed here: https://github.com/astral-sh/uv/issues/2077#issuecomment-1971579880 

> One of the problems is that you cannot run --user and --editable any more with pip. 

As far as i know, both `--user` and `--editable` are still supported in the latest [pip install options](https://pip.pypa.io/en/stable/cli/pip_install/#options). 
There appears to be no intention from the pip developers to deprecate these flags. I couldn't find any discussion about this either.

I don't have the full context so the complexity might arise specifically in the context of using Airflow. It would be great if the author of that comment could provide some further details.

For reference, I conducted a quick local test combining the `--user` and `--editable` flags:
```
❯ python3 -m venv venv --system-site-packages
❯ source venv/bin/activate
❯ pip install --user -e sampleproject/
Obtaining
  Installing build dependencies ... 
...
Successfully built sampleproject
Installing collected packages: peppercorn, sampleproject
...
Successfully installed peppercorn-0.6 sampleproject-3.0.0

❯ pip list --user
Package       Version Editable project location
------------- ------- -----------------------------------------
peppercorn    0.6
sampleproject 3.0.0   /Users/me/sampleproject
```

---

_Review comment by @imfing on `crates/uv-interpreter/src/interpreter.rs`:455 on 2024-03-12 08:09_

I've noticed that `query_cached` uses `the executable's last modified time` as a cache key. In the case of user scheme, the interpreter may remain the same, e.g. the system Python interpreter, but the `InterpreterInfo.scheme` changes to the user site, e.g. `~/.local`.

I will take a further look into using `uv_cache` to cache the script execution here.

---

_@imfing reviewed on 2024-03-12 08:09_

---

_Comment by @zanieb on 2024-05-17 13:46_

There's some more discussion on this active in https://github.com/astral-sh/uv/issues/2077

---

_Comment by @charliermarsh on 2024-05-17 14:16_

I may expand the section on `--user` in `PIP_COMPATIBILITY.md` in the interim.

---

_Comment by @Malix-Labs on 2024-06-05 12:04_

Is there any blocker that requires some help to merge this PR?
We are waiting for that feature to be merged to migrate every devcontainer we have to use `uv`

---

_Comment by @imfing on 2024-06-05 13:18_

> Is there any blocker that requires some help to merge this PR? We are waiting for that feature to be merged to migrate every devcontainer we have to use `uv`

In #2077, there were extensive discussions, and from what I gathered, it seemed the `uv` team prefer advocating for the adoption of virtual environments in `uv`.

I kept this PR open in case things change, and if the `uv` team is okay with adding the flag, I will update the PR.

---

_Comment by @Malix-Labs on 2024-06-05 13:21_

Well then, [installing in immutable containers (such as codespaces) would require an useless venv, as `--system` doesn't work](https://github.com/astral-sh/uv/issues/2077#issuecomment-2098624319)

---

_Comment by @zanieb on 2024-06-05 13:44_

@Malix-off can you please discuss this on the issue instead of this pull request? I'd prefer to keep the discussion here focused on implementation details. Thanks.

---

_Comment by @Malix-Labs on 2024-06-05 13:47_

@zanieb absolutely, good practice btw and sorry about those annoyances

---

_Comment by @imfing on 2024-06-05 14:00_

Noticed that `--target` flag was added in https://github.com/astral-sh/uv/pull/3257, which can be used as a workaround for `--user` by providing the user-specific [site-packages directory](https://docs.python.org/3/library/site.html#site.getusersitepackages)

---

_Closed by @imfing on 2024-06-15 11:43_

---

_Branch deleted on 2024-06-15 11:43_

---
