---
number: 12038
title: "running `uv pip install .` should systematically reinstall the package"
type: issue
state: closed
author: 12rambau
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-03-07T10:52:22Z
updated_at: 2025-03-17T21:12:23Z
url: https://github.com/astral-sh/uv/issues/12038
synced_at: 2026-01-10T01:25:14Z
---

# running `uv pip install .` should systematically reinstall the package

---

_Issue opened by @12rambau on 2025-03-07 10:52_

### Summary

When dealin with local packages, uv is also caching the installation parameter making it very dificult when I work on package to run my tox or nox session with a uv backend as everytime I make a modification my local package in the venv are not updated. I fail to see a configuration where people would install a local package without wanting to update it as usually when the package is locally installed it means that you are currently working on it. 


I quote @henryiii from https://github.com/astral-sh/uv/issues/2844: 

> Running -e. (or even .) again is a common way to rebuild binary packages, and it's a lot less convenient if it doesn't actually do anything. I don't think local packages should be cached. Local packages tend to have changes without releasing versions (since you are developing on the package).

Is there any rational to go against this statement that I'm missing ? It makes maintenance of development environment tedious and I know the more option I add (metadata in my pyproject included like in https://github.com/astral-sh/uv/issues/7282 will eventually be forgotten by maintainers and create build issues when things (were/should) be simple. 

Note that `-e` is not an option in the following example as I want to test it as close as when user will install it from pypi. I already had some issues in ipywidget related packages that are not reproducible with `-e` installation and it took me forever to find them. 

### Example

I'm using nox to build my environment for test suits as such using a "uv" backend: 

```python 
@nox.session(reuse_venv=True, venv_backend="uv")
def test(session):
    """Run the selected tests and report coverage in html."""
    session.install(".[test]") # this is equivalent to uv pip install .[test] in a nox siloted context
    test_files = session.posargs or ["tests"]
    session.run("pytest", "--cov", "--cov-report=html", *test_files)
```

and run it via: 

```
nox -s test
```

The problem is also mentioned directly in the nox documentation:

> The uv backend does not reinstall, even for local packages, so you need to include --reinstall-package <pkg-name> (uv-only) if reusing the environment.



---

_Label `enhancement` added by @12rambau on 2025-03-07 10:52_

---

_Comment by @henryiii on 2025-03-07 15:28_

I can speak for the reasoning: while making `uv pip install .` a no-op is likely not what anyone wants, `uv run` shouldn't re-install the package every time. I think these two should be treated differently (`uv run` is triggering an install as a by-product, while `uv pip install .` is a user request to do an install, and nothing else), that's why it is this way. It's not ideal, but you can specify `tool.uv.pip.reinstall-package = ["name-of-local-package"] (I wonder if `"."` is valid here?) to help for a specific project.

---

_Comment by @charliermarsh on 2025-03-07 15:50_

I think using different behaviors for `uv pip install .` vs. `uv run` is fairly reasonable, if we view the former as an "explicit" request vs. an "implicit" request for `uv run`. What do you think, @zanieb?

---

_Label `needs-decision` added by @charliermarsh on 2025-03-07 15:50_

---

_Comment by @zanieb on 2025-03-07 16:33_

Yeah that's fine with me. My concern is that this would then differ from our behavior for other packages? Are we specializing source trees?

---

_Comment by @charliermarsh on 2025-03-07 17:32_

@zanieb -- Yeah, I think we would say that any source tree passed on the CLI would be run with `--reinstall` or equivalent.

---

_Comment by @zanieb on 2025-03-07 17:35_

That makes sense, I think I've wanted this before.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-07 20:55_

---

_Referenced in [astral-sh/uv#12176](../../astral-sh/uv/pulls/12176.md) on 2025-03-14 20:15_

---

_Closed by @charliermarsh on 2025-03-17 21:12_

---

_Closed by @charliermarsh on 2025-03-17 21:12_

---

_Referenced in [astral-sh/uv#13388](../../astral-sh/uv/issues/13388.md) on 2025-05-11 19:27_

---

_Referenced in [astral-sh/uv#13876](../../astral-sh/uv/issues/13876.md) on 2025-06-06 00:01_

---
