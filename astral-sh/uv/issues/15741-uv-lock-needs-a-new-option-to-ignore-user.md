---
number: 15741
title: "`uv lock` needs a new option to ignore user-configured indexes"
type: issue
state: open
author: Arcitec
labels:
  - enhancement
assignees: []
created_at: 2025-09-08T18:36:18Z
updated_at: 2025-11-24T22:58:55Z
url: https://github.com/astral-sh/uv/issues/15741
synced_at: 2026-01-10T01:25:58Z
---

# `uv lock` needs a new option to ignore user-configured indexes

---

_Issue opened by @Arcitec on 2025-09-08 18:36_

### Summary

After a lot of painstaking tests, I can conclude that `uv lock` always loads the user's personal `~/.config/uv/uv.toml`, which means that projects `uv.lock` can become poisoned with the user's own custom PyPI mirror. Thereby generating a `uv.lock` with non-trusted URLs and package hashes.

Example:

- `~/.config/uv/uv.toml`

```toml
[[index]]
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```

- Project's `pyproject.toml` cannot override this. `uv lock --show-settings` reveals that the custom mirror is always included.

I have tried everything:

- Putting another `uv.toml` in the repo which forces the URL to be PyPI.
- Forcing URLs with `uv lock --upgrade --index-url "https://pypi.org/simple"` and `uv lock --upgrade --default-index "https://pypi.org/simple"`
- Adding a `pyproject.toml` rule to default to PyPI (still doesn't do it):

```toml
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true
```

The **only** thing that seems to work is `uv lock --upgrade --no-config` but [according to the docs](https://docs.astral.sh/uv/reference/cli/#uv-auth-login--no-config), that also makes it ignore all custom config rules inside pyproject.toml, which is not what I want.

What we need for safety and consistency is a way to configure `pyproject.toml` to tell it "always generate uv.lock based on PyPI, not the user's local mirrors".

### Example

_No response_

---

_Label `enhancement` added by @Arcitec on 2025-09-08 18:36_

---

_Comment by @konstin on 2025-09-09 17:55_

We're planning to support the reverse feature: You can use PyPI URLs in the lockfile, but set a setting that instead resolves and installs with the base URL replaced on your machine. See https://github.com/astral-sh/uv/issues/6349 and https://github.com/astral-sh/uv/pull/11782. I think this broadly a duplicate of #6349 and #9056.

---

_Comment by @Arcitec on 2025-09-09 18:01_

@konstin Thank you so much for the analysis and links to related ideas. I had a look at the linked requests.

The most desirable outcome would be that `uv.lock` always uses PyPI URLs and hashes (configured via some `pyproject.toml` field to mark pypi as the "ground truth" that must be used for generating lockfiles), and then the user's local-machine is free to use local mirrors on their own base machine.

It basically works like that already; users can currently override the index of the lockfile via `uv sync --default-index "foo"`.

The only thing missing is the ability to force the `uv.lock` to ignore the user's local repos and always use the repos defined in `pyproject.toml`. It could be added with a config setting. The only thing I am unsure about is whether users should then be forced to manually define PyPI's repo in the pyproject.toml or if it should be auto-inherited via uv. I think people will want the ability to exclude PyPI entirely, so a design like this would make sense:

```toml
[tool.uv]
strict-lockfile-indexes = true

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "pytorch-cuda"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

---

_Comment by @Anchorfall on 2025-11-24 22:36_

@konstin Second this, it would be great if we could explicitly force uv to disregard the user's ~/.config/uv/uv.toml file, and exclusively use the indices defined in pyproject.toml when resolving dependencies.

Ideally, we'd be able to configure this by changing a setting in pyproject.toml (something like the example above), as opposed to adding an argument to 'uv lock' on the command line every time. 

---

_Comment by @Anchorfall on 2025-11-24 22:50_

> [@konstin](https://github.com/konstin) Second this, it would be great if we could explicitly force uv to disregard the user's ~/.config/uv/uv.toml file, and exclusively use the indices defined in pyproject.toml when resolving dependencies.
> 
> Ideally, we'd be able to configure this by changing a setting in pyproject.toml (something like the example above), as opposed to adding an argument to 'uv lock' on the command line every time.

To provide some context, we're in a situation where uv keeps using an index that I don't want it to use, to install transitive dependencies. We don't want it to touch that index because it's inaccessible to the environment we're deploying to, and contains package versions that aren't available elsewhere.  However, the index is commonly used outside of this project, so certain team members have the 'forbidden' index listed in their uv.toml file. So whenever someone runs 'uv lock' with that index in their uv.toml, we run into this problem.

The only way around this is to explicitly list out all transitive dependencies in pyproject.toml, and force them to the correct index under [tool.uv.sources]. Which works, but isn't ideal, and will only 'solve' the problem for as long as nobody adds another package and runs 'uv lock'. Either that, or we tell all team members to forever remove the forbidden index from their uv.toml files, even though it's a valid index for every other project. 

It would be better if we could (optionally) exert more control over which sources uv can and cannot not use, when resolving dependencies for a particular project. 

---
