```yaml
number: 11104
title: Ignore group completely when resolving depencencies.
type: issue
state: open
author: fizcris
labels:
  - enhancement
assignees: []
created_at: 2025-01-30T17:26:59Z
updated_at: 2025-12-19T02:29:38Z
url: https://github.com/astral-sh/uv/issues/11104
synced_at: 2026-01-10T03:11:33Z
```

# Ignore group completely when resolving depencencies.

---

_Issue opened by @fizcris on 2025-01-30 17:26_

### Summary

If we have a `project.toml` like:

```
[project]
name = "example"
version = "1.0.0"
description = "desccription"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "matplotlib>=3.10.0",
    "pandas>=2.2.3",
]

[dependency-groups]
internal = [
    "my-app-1">=1.0.0",
    "my-app-2">=2.0.0",
]

[tool.uv.sources]
my-app-1 = { index = "internalRepo"}
my-app-2 = { index = "internalRepo2"}

[[tool.uv.index]]
name = "internalRepo"
url = "https://my-internal-repo.com/simple"
explicit = true

[[tool.uv.index]]
name = "internalRepo2"
url = "https://my-internal-repo2.com/simple"
explicit = true
```


If in the moment we are solving dependencies there is access to the private repositories it will fail and there is no way to exclude the `internal` group.

So the following command will fail `uv sync --no-group internal`. And there is no known way to completely exclude the `internal` group from dep resolution.


### Example

_No response_

---

_Label `enhancement` added by @fizcris on 2025-01-30 17:26_

---

_Comment by @zanieb on 2025-01-30 17:31_

No, there's no way to exclude a group from the resolution at this time. If you did, it wouldn't be installable. What's the use-case for not having access to the index?

---

_Comment by @fizcris on 2025-01-30 18:06_

That is ok if it is not installable.

If the index is a private repository and the app can only be installed when there is access to that repository, hence if you only need that library for some parts of the code you would have to install them manually when required. 

The app could partially work without those libraries when it does not have access but is not able to resolve the deps hence renders the uv unusable and needs manual installation of the libraries.

---

_Comment by @zanieb on 2025-01-30 18:37_

> hence if you only need that library for some parts of the code you would have to install them manually when required.

How would you install these?

Why include them in `[dependency-groups]` if they can't actually be installed?

Why not generate the lockfile with access to the private index? You can still do partial installs later without access to the index.

---

_Comment by @T-256 on 2025-01-30 21:44_

Same as #10201.

I'm thinking of two possible solutions for this (if we want to support such situations...):

First, similar to https://github.com/astral-sh/uv/issues/11064 but, instead, destruct `uv.lock` into per-group files (e.g. `internal.uv.lock`).

Second, it could lazy solving dependency for specified indexes:

```toml
[tool.uv.sources]
my-app-1 = { index = "internalRepo"}

[[tool.uv.index]]
name = "internalRepo"
url = "https://my-internal-repo.com/simple"
explicit = true
lazy = true
```
I don't know how much this is possible, via this option, dependencies with lazy indexes are not resolved in lockfile but when actually requested for install (e.g. `uv sync --group internal`).

---

_Comment by @fizcris on 2025-01-30 23:07_

Well image that you have private machines and public machines, you dont want to be maintaining multiple lockfiles.



---

_Comment by @zanieb on 2025-01-31 18:33_

I don't understand why you'd need multiple lockfiles. You'd lock on the private machine and it'd be usable everywhere? 

---

_Comment by @eremeevfd on 2025-06-30 16:02_

We also found ourselves in a similar situation.  
Our case is: we need the libraries from private registry only for testing, but for production build, we don't want to expose credentials for registry and have these dependencies present at all.   
But it's currently impossible to install only non-private dependencies unfortunately. Maybe it's possible to disable resolver at all if it's not needed?

---

_Comment by @zanieb on 2025-06-30 16:10_

@eremeevfd you can `uv sync --frozen` and skip performing a resolution.

---

_Comment by @eremeevfd on 2025-07-03 14:33_

@zanieb Thanks for your advice, just tried it and got this error:
```
2025-07-03T14:19:44: #11 [stage-0 5/5] RUN uv sync --frozen --no-cache --compile-bytecode
2025-07-03T14:19:45: #11 0.890 Downloading cpython-3.11.13-linux-x86_64-gnu (download) (30.1MiB)
2025-07-03T14:19:46: #11 2.217  Downloading cpython-3.11.13-linux-x86_64-gnu (download)
2025-07-03T14:19:47: #11 2.412 Using CPython 3.11.13
2025-07-03T14:19:47: #11 2.412 Creating virtual environment at: /.venv
2025-07-03T14:19:47: #11 2.491   × Failed to download `private-dependency==3.0.2`
2025-07-03T14:19:47: #11 2.491   ├─▶ Failed to fetch:
2025-07-03T14:19:47: #11 2.491   │   `{here was a url to private pypi trying to download private-dependency}`
2025-07-03T14:19:47: #11 2.491   ╰─▶ HTTP status client error (401 Unauthorized) for url
2025-07-03T14:19:47: #11 2.491       (...)
2025-07-03T14:19:47: #11 2.491   help: `private-dependency` (v3.0.2) was included because
2025-07-03T14:19:47: #11 2.491         `my-current-project` (v0.1.0) depends on
2025-07-03T14:19:47: #11 2.491         `private-dependency`
2025-07-03T14:19:48: #11 ERROR: process "/bin/sh -c uv sync --frozen --no-cache --compile-bytecode" did not complete successfully: exit code: 1
```
however I have a suspicion that it happens because we use `pyturbojpeg` and this library doesn't have any wheels, so it is being built with `setup.py` every installation. And when it's trying to be built from scratch it tries to download `private-dependency`. So maybe it's not a UV issue at all or I should maybe change build-backend.

---

_Comment by @auwi-nordic on 2025-10-08 12:51_

We have a similar usecase and same issue.. indices that are available during development or testing may not be available when building, e.g. building in a Dockerfile.

A workaround can be to mount a pre-generated lock file, and using `--frozen`.
This worked for me in the first step of a two-step install with `--no-install-project`,
but when trying to run `uv sync` a second time to install the project, uv complains about
not being able to find uv_build from the private index which it then doesn't have access to.

One dirty workaround that works OK in docker is to modify pyproject.toml, e.g. disable all the indices
`RUN sed -i 's/tool.uv.index/disabled/g' pyproject.toml`

But it would be nice if uv had some better options to work around these issues. 



---

_Comment by @zanieb on 2025-10-08 13:31_

@auwi-nordic we shouldn't look for `uv_build` in an index if the requested version range is compatible with the current `uv` version.

---

_Comment by @ketozhang on 2025-12-18 22:55_

@zanieb, it looks like you're assuming that if you take the union of dependencies from all groups, they must all be compatible. I do not find that statement in [the specification](https://packaging.python.org/en/latest/specifications/dependency-groups/#dependency-groups).

Let's expand the use case beyond the above (i.e., dependency-group & private index).  

Here's another use case where you want to migrate between two major version of a doc builder (here I use Jupyter Book). When a user intends to use either group `docs-jb1` and not `docs-jb2` (and vice-versa), this fails to be resolved.

```toml
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[dependency-groups]
docs-jb1 = [
    "jupyterbook >=1,<2",
]

docs-jb2 = [
    "jupyterbook >=2",
]
```

```console
❯ uv sync --group docs-jb1
  × No solution found when resolving dependencies:
  ╰─▶ Because myproject:docs-jb2 depends on jupyterbook>=2 and myproject:docs-jb1 depends on jupyterbook>=1,<2, we can conclude that myproject:docs-jb1 and myproject:docs-jb2 are incompatible.
      And because your project requires myproject:docs-jb1 and myproject:docs-jb2, we can conclude that your project's requirements are unsatisfiable.
```

> I don't understand why you'd need multiple lockfiles. You'd lock on the private machine and it'd be usable everywhere?

There are a lot of use case for this for hybrid application & package. You may want to build a lock file per deployed environment which may have slightly differences in constraints (e.g., A/B testing)

---

_Comment by @zanieb on 2025-12-18 22:59_

> it looks like you're assuming that if you take the union of dependencies from all groups, they must all be compatible. I do not find that statement in [the specification](https://packaging.python.org/en/latest/specifications/dependency-groups/#dependency-groups).

There is no specification which defines how dependency resolution should be performed in Python projects. This is a constraint of our resolution and lock approach (some background in https://docs.astral.sh/uv/concepts/resolution/#universal-resolution).

We do allow the use-case you've just described. You can declare conflicting groups, see https://docs.astral.sh/uv/concepts/resolution/#conflicting-dependencies

---

_Comment by @ketozhang on 2025-12-18 23:24_

Okay that's fair. If that's how uv want to define their own lock files. 

I want to bring up pylock.toml is supports lock file that represents a subset of dependency groups:
https://packaging.python.org/en/latest/specifications/pylock-toml/#dependency-groups

Here `uv pip compile` is only supports `dependency-groups=[]` (i.e., lock file compatible with all groups OR none). Doing more is optional...

> Lockers MAY choose to not support writing lock files that support extras and dependency groups (i.e. tools may only support exporting a single-use lock file).

, however I would encourage uv supports this because we really need a tooling support for deploys (which includes this issue's usage)

A few examples of lockfile sets and purposes:
- A deployment lockfile: ['dev']
- A private server deployment lockfile: ['test', 'prod']
- A doc building lockfile: ['docs-jb1']; ['docs-jb2']

---

_Comment by @ketozhang on 2025-12-18 23:26_

Related, but is resolved #8969

---

_Comment by @ketozhang on 2025-12-18 23:39_

Sorry I'm mistaken, this is already supported by `uv pip sync` and `uv pip compile` variant (just not `uv sync` and `uv lock`).

```
❯ uv pip compile --group docs-jb1 --format pylock.toml | grep jupyter-book -A 2
Resolved 93 packages in 29ms
name = "jupyter-book"
version = "1.0.4.post1"
sdist = { url = "https://files.pythonhosted.org/packages/cf/ee/5d10ce5b161764ad44219853f386e98b535cb3879bcb0d7376961a1e3897/jupyter_book-1.0.4.post1.tar.gz", upload-time = 2025-02-28T14:55:48Z, size = 67412, hashes = { sha256 = "2fe92c49ff74840edc0a86bb034eafdd0f645fca6e48266be367ce4d808b9601" } }

❯ uv pip compile --group docs-jb2 --format pylock.toml | grep jupyter-book -A 2
Resolved 79 packages in 164ms
name = "jupyter-book"
version = "2.1.0"
sdist = { url = "https://files.pythonhosted.org/packages/9d/ef/618a3080c9c2f76f6d17212b7954db448d7f92da6459450a6acce1e0d596/jupyter_book-2.1.0.tar.gz", upload-time = 2025-12-05T13:16:57Z, size = 14440795, hashes = { sha256 = "f42b6f264e6e270b07362f4430ef671edccbb1be36cd0872afd231c642a4ba7e" } }
```

---

However the `dependency-groups` field is not written in the lock file.

---

_Comment by @zanieb on 2025-12-19 02:29_

The `dependency-groups` field is for allowing consumers of the lock to select a subset of the lockfile to install. We don't support that — it's a single-use lockfile. I believe we're explicitly supposed to leave it as `[]` in this case, though I understand the specification is a little confusing.

---
