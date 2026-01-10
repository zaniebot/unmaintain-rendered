```yaml
number: 15826
title: Improve hint for installing meson-python when missing as build backend
type: pull_request
state: open
author: tobiasdiez
labels: []
assignees: []
base: main
head: meson-python-hint
created_at: 2025-09-14T00:56:19Z
updated_at: 2025-12-22T12:17:20Z
url: https://github.com/astral-sh/uv/pull/15826
synced_at: 2026-01-10T05:49:14Z
```

# Improve hint for installing meson-python when missing as build backend

---

_Pull request opened by @tobiasdiez on 2025-09-14 00:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

If a package uses meson-python as backend, it's declared as:
```
[build-system]
build-backend = 'mesonpy'
requires = ['meson-python']
```

Currently, if meson-python is missing, one gets the following hint:
```
Traceback (most recent call last):
        File "<string>", line 8, in <module>
          import mesonpy as backend
      ModuleNotFoundError: No module named 'mesonpy'

      hint: This error likely indicates that `sagemath` depends on `mesonpy`, but doesn't declare it as a build
      dependency. If `sagemath` is a first-party package, consider adding `mesonpy` to its `build-system.requires`.
      Otherwise, either add it to your `pyproject.toml` under:

      [tool.uv.extra-build-dependencies]
      sagemath = ["mesonpy"]

      or `uv pip install mesonpy` into the environment and re-run with `--no-build-isolation`.
```

which is not quite correct as the build backend/module is called "mesonpy" but the python package one has to install is "meson-python". This hint is improved in this PR.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I didn't yet had a chance to setup a local dev env for testing this.
<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-09-14 04:01_

We shouldn't hard-code this. The mapping should be performed at https://github.com/astral-sh/uv/blob/35a8dd514ef22d4c71cab430c74af2c1b95fbfe3/crates/uv-build-frontend/src/error.rs#L403-L406

Maybe it's not right in our embedded mapping?

---

_Comment by @zanieb on 2025-09-14 13:00_

It looks like it's just not present in `crates/uv-build-frontend/src/pipreqs/mapping`

---

_Comment by @samypr100 on 2025-09-23 23:32_

> It looks like it's just not present in `crates/uv-build-frontend/src/pipreqs/mapping`

Is it worth adding it to https://github.com/bndr/pipreqs/blob/master/pipreqs/mapping and then syncing/update uv's version?

---

_Comment by @zanieb on 2025-09-24 04:33_

That seems like the most proper way to do it, yeah.

---

_Comment by @tobiasdiez on 2025-12-22 12:17_

I've now created a PR that adds the mapping upstream (https://github.com/bndr/pipreqs/pull/508) and also updated the mapping here in the repo. Is this what you had in mind?

---
