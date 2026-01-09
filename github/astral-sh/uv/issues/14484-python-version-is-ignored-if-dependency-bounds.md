---
number: 14484
title: Python version is ignored if dependency bounds are not specified
type: issue
state: closed
author: tpoliaw
labels:
  - question
assignees: []
created_at: 2025-07-07T11:52:15Z
updated_at: 2025-07-17T09:27:16Z
url: https://github.com/astral-sh/uv/issues/14484
synced_at: 2026-01-07T13:12:18-06:00
---

# Python version is ignored if dependency bounds are not specified

---

_Issue opened by @tpoliaw on 2025-07-07 11:52_

### Summary

If a project specifies a dependency without any version bounds, the python version doesn't seem to be checked and an invalid version is selected. If a bound (that includes the previously selected version) is added, resolution fails with a python version error.

# Minimal example
Given the following `pyproject.toml`

```toml
[project]
name = "demo"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
    "numba"
]
```

Running `uv tree` gives
```
Resolved 7 packages in 5ms
demo v0.1.0
└── numba v0.61.2
    ├── llvmlite v0.44.0
    └── numpy v2.2.6
```

Updating the pyproject.toml to include a bound on the `numba` dependency (note `>=0.61` should include the 0.61.2 previously selected),

```toml
[project]
name = "demo"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
    "numba>=0.61"
]
```

`uv tree` now fails with a conflict error
```
❯ uv tree
  × No solution found when resolving dependencies for split
  │ (python_full_version == '3.9.*'):
  ╰─▶ Because the requested Python version (>=3.9) does not satisfy
      Python>=3.10 and numba>=0.61.0 depends on Python>=3.10, we can
      conclude that numba>=0.61.0 cannot be used.
      And because only the following versions of numba are available:
          numba<=0.61.0
          numba==0.61.2
      and your project depends on numba>=0.61, we can conclude that your
      project's requirements are unsatisfiable.

      hint: Pre-releases are available for `numba` in the requested
      range (e.g., 0.61.1rc1), but pre-releases weren't enabled (try:
      `--prerelease=allow`)

      hint: The `requires-python` value (>=3.9) includes Python versions
      that are not supported by your dependencies (e.g., numba>=0.61.0 only
      supports >=3.10). Consider using a more restrictive `requires-python`
      value (like >=3.10).

      hint: While the active Python version is 3.12, the resolution failed
      for other Python versions supported by your project. Consider limiting
      your project's supported Python versions using `requires-python`.
```


### Platform

arch linux 6.12.31-1-lts

### Version

uv 0.7.18 (87e9ccfb9 2025-07-01)

### Python version

3.12.9

---

_Label `bug` added by @tpoliaw on 2025-07-07 11:52_

---

_Label `bug` removed by @konstin on 2025-07-07 12:32_

---

_Label `question` added by @konstin on 2025-07-07 12:32_

---

_Comment by @konstin on 2025-07-07 12:34_

uv realizes that the latest version of numba works on Python 3.12, while resolving an older version for Python 3.9. In `uv.lock`, you can see that uv has split the resolution for different Python versions:

```
[[package]]
name = "debug"
version = "0.1.0"
source = { editable = "." }
dependencies = [
    { name = "numba", version = "0.60.0", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version < '3.10'" },
    { name = "numba", version = "0.61.2", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version >= '3.10'" },
]
```

As the current Python version is 3.12, numba 0.61.2 can be used, while the older version will be installed on Python 3.9.

We recommend always adding lower bounds to dependencies, to avoid such problems.

---

_Comment by @tpoliaw on 2025-07-07 13:29_

Ok, thank you for the quick response and explanation. I think I may have misdiagnosed the problem I'm actually getting.

In an empty directory
```
uv init
uv add sparse
uv add numpy
```
works fine, whereas
```
uv init
uv add sparse numpy # fails
```
fails with it selecting a version of `numba` that isn't compatible with the python version.

Oddly it works again if you include `numba` explicitly
```
uv init
uv add sparse numpy numba # works
```

I can open a new issue for that. Feels kind of similar to [this one](https://github.com/astral-sh/uv/issues/6281#issuecomment-2316240724) but in this case there *is* a way of distinguishing between the two resolutions.

---

_Comment by @charliermarsh on 2025-07-07 14:41_

I'd suggest adding a constraint somewhere. This is likely just the resolver choosing a certain outcome that isn't incorrect, but also isn't what you want, so you can provide more information to guide it. (We don't respect upper-bounds on `requires-python`.)

---

_Referenced in [SpikeInterface/spikeinterface#4046](../../SpikeInterface/spikeinterface/pulls/4046.md) on 2025-07-09 12:57_

---

_Comment by @Vonfry on 2025-07-16 12:29_

Some projects, especially in ML, such as ChatTTS and funasr, aren't set versions for some dependencies usually in both pypi and repo. I think uv could make it working like pip in such cases by installing/syncing dependencies with current python version or respecting `requires-python`.

I agree on "always adding lower bounds to dependencies", but dependencies are from anywhere and other authors may not follow this.

At least, uv could add a warning if there is a dependency without lower bounds.

---

_Comment by @tpoliaw on 2025-07-16 12:48_

Yeah, adding a lower bound fixes it so I'm happy for this to be closed but I'd question "the resolver choosing a certain outcome that isn't incorrect" as it's selecting a combination that isn't valid. It might be a consequence of ignoring python upper bounds. Is there a reason for that decision?

It's also a little annoying having to add a lower bound to `numba` when we wouldn't otherwise depend on it directly.

Might just be being picky though as `uv` has been a great improvement in every other respect so far.

---

_Comment by @konstin on 2025-07-17 09:27_

@Vonfry You can set constraints for transitive dependencies too (https://docs.astral.sh/uv/reference/settings/#constraint-dependencies). uv already tries to determine a good resolution, but sometimes it's not possible to tell the better choice algorithmically, where you need to add bounds manually.

> Yeah, adding a lower bound fixes it so I'm happy for this to be closed but I'd question "the resolver choosing a certain outcome that isn't incorrect" as it's selecting a combination that isn't valid. It might be a consequence of ignoring python upper bounds. Is there a reason for that decision?

That's a complex story, the context is in and linked to in https://github.com/astral-sh/uv/issues/4022.

---

_Closed by @konstin on 2025-07-17 09:27_

---
