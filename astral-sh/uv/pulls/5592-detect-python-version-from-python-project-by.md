```yaml
number: 5592
title: "Detect python version from python project by default in `uv venv`"
type: pull_request
state: merged
author: Vigilans
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: vigilans/uv-venv-detect-project-python-version
created_at: 2024-07-30T09:39:02Z
updated_at: 2024-07-31T13:54:17Z
url: https://github.com/astral-sh/uv/pull/5592
synced_at: 2026-01-12T16:06:54Z
```

# Detect python version from python project by default in `uv venv`

---

_@Vigilans_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`uv venv` should support adopting python version specified in `requires-python` from `pyproject.toml`. This allows customization on the venv setup when syncing from python project.

Closes https://github.com/astral-sh/uv/issues/5552.

It also serves as a workaround to close https://github.com/astral-sh/uv/issues/5258.

## Test Plan

<!-- How was it tested? -->

1. Run `uv venv` in folder with `pyroject.toml` specifying `requries-python = "<3.10"`. Python 3.9 is selected for venv.
2. Change to `requries-python = "<3.11"` and run `uv venv` again. Python 3.10 is selected now.
3. Switch to a folder without `pyproject.toml` then run `uv venv`. Python 3.12 is selected now.

---

_Review comment by @Vigilans on `crates/uv/src/commands/venv.rs`:168 on 2024-07-30 09:42_

Code here is adopted from `run.rs`:
https://github.com/astral-sh/uv/blob/c0d3da8b6ae134036b3ed024762b08c4fb9f967b/crates/uv/src/commands/project/run.rs#L176-L182
and `FoundInterpreter` in `mod.rs`:
https://github.com/astral-sh/uv/blob/c0d3da8b6ae134036b3ed024762b08c4fb9f967b/crates/uv/src/commands/project/mod.rs#L158-L172

---

_@Vigilans reviewed on 2024-07-30 09:42_

---

_@charliermarsh approved on 2024-07-30 12:29_

---

_Comment by @charliermarsh on 2024-07-30 12:32_

Can you add a test for this in `tests/venv.rs`?

---

_Label `configuration` added by @charliermarsh on 2024-07-30 12:33_

---

_Label `preview` added by @charliermarsh on 2024-07-30 12:33_

---

_Comment by @T-256 on 2024-07-30 13:39_

When initializing python project, it is common to specify minimum version in pyproject.toml (e.g. `requires-python = ">=3.8"`). Within development of project, we need to have minimum supported environment to avoid using newest features that is unavailable in minimum required python version.

Is it make sense to have an option to change resolution strategy for python discovery? just like `--resolution` for dependencies resolver.

### Example
`pyproject.toml`
```toml
[project]
requires-python = ">=3.8"
```
By using `uv venv --python-resolution=lowest-minor` it will select `3.8.19`. (IIRC, this is default behavior of Rye?!)

---

_Comment by @zanieb on 2024-07-30 14:17_

Note this interacts with some work-in-progress at https://github.com/astral-sh/uv/pull/5035; specifically the [merged lines on my branch](https://github.com/astral-sh/uv/compare/zb/pin-find#diff-008a5d315b289d3a263e6f01768b573a21e6958fffeb19af6ae3140c84a53fe0R143-R155). I don't think that should block this change though.

We'll also need to consider the following in the future (no changes needed here):

- https://github.com/astral-sh/uv/issues/4971
- https://github.com/astral-sh/uv/issues/4970
- https://github.com/astral-sh/uv/issues/5228

---

_Comment by @zanieb on 2024-07-30 14:20_

I am tempted to agree that we should be selecting the lowest compatible version by default? This is tough to balance, as it's nice to get the performance and error handling improvements of a newer release. It may make sense to just expose a way to use the lowest compatible version instead (as suggested by @T-256) so it's easy to do in CI? However, I think in that case you'd want to actually guarantee you're getting the lowest version not the one nearest to your lower bound?

---

_Comment by @T-256 on 2024-07-30 15:54_

> I am tempted to agree that we should be selecting the lowest compatible version by default?

Another case for inconsistency: today if you specify `requires-python = ">=3.10"` and `uv venv` it will create 3.12 environment, but in next year when a contributor wants init an environment, it would be 3.13 which brings different development environments.
IMO, this is not a big concern since it could be solved by `.python-version` file. then I think we should be stricter on python version resolving and tip users to use (and also commit) `.python-version` file.


> However, I think in that case you'd want to actually guarantee you're getting the lowest version not the one nearest to your lower bound?

Yes, it is makes sense to be strict on there. for example, with `requires-python = ">=3.10"` and installed python 3.12, it should download and use 3.10. in cases which download is unavailable it should fail with error.


---

_Comment by @T-256 on 2024-07-30 16:00_

> It may make sense to just expose a way to use the lowest compatible version instead (as suggested by @T-256) so it's easy to do in CI?

@zanieb what if minimum version is not provided and only used lower-bound? for example `requires-python = "<3.12"` should select which python version when used with that shipped option (assume `--python-resolution=lowest-minor`)?

---

_Comment by @Vigilans on 2024-07-30 16:06_

I prefer to use the newest version.

Pros:
* For users who want to stick to specific python version, they will use `== 3.8.*` or `>=3.8,<3.9`.
* For users who specify only lower bound, using newest python version that breaks their project, is a hint to users that they should set upper bound to their `pyproject.toml`. This helps make their specification more robust. 

Cons:
* For non-author users cloning the project, they may fail to run the project because the selected version is too new. Even users add the newer version to upper bound, it may be hard to sync the change to upstream. 

A flag altering this behavior to use lowest possible version should be introduced indeed to ensure stable build in CI like docker image building, or for build script/devcontainer to ensure end users can run on happy path after their cloning.  

---

_Comment by @zanieb on 2024-07-30 16:27_

Note we basically never recommend adding an upper bound to Python version constraints and actively ignore it in some contexts.

> in next year when a contributor wants init an environment, it would be 3.13 which brings different development environments.

Yeah this problem is solved by encouraging pin of the version (see also #4970)

---

_Review comment by @Vigilans on `crates/uv/tests/venv.rs`:335 on 2024-07-30 16:28_

I found that the default behavior for `>=3.x` is not **select newest version**, but **select first possible version** in the list. For a python list `[3.11, 3.10, 3.12]`:
* `>=3.10` will select `3.11`.
* `>=3.11` will select `3.11`.
* `>3.11` will select `3.11` (becuase `3.11.x` > `3.11`).
* `>=3.12` will select `3.12`.

---

_@Vigilans reviewed on 2024-07-30 16:29_

Test added. Found some more behaviors that should be reviewed.

---

_Comment by @T-256 on 2024-07-30 16:36_

> I prefer to use the newest version.

@Vigilans I Agree, And I think it could be opt-in to that behavior.

created https://github.com/astral-sh/uv/issues/5609 to track it separately.

---

_@charliermarsh reviewed on 2024-07-30 19:50_

---

_Review comment by @charliermarsh on `crates/uv/tests/venv.rs`:295 on 2024-07-30 19:50_

Should this be 3.11?

---

_@charliermarsh reviewed on 2024-07-30 19:50_

---

_Review comment by @charliermarsh on `crates/uv/tests/venv.rs`:319 on 2024-07-30 19:50_

Should this be 3.12?

---

_Review comment by @Vigilans on `crates/uv/tests/venv.rs`:319 on 2024-07-31 00:03_

Wow, I got these tests code totally wrong! Since the commit is pushed near my bed time I did not check them carefully after pasting. Good news that tests still pass after correction. Apology for my carelessness.

---

_@Vigilans reviewed on 2024-07-31 00:03_

---

_@zanieb approved on 2024-07-31 13:32_

Thanks!

---

_Merged by @zanieb on 2024-07-31 13:54_

---

_Closed by @zanieb on 2024-07-31 13:54_

---
