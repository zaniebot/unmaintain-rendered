---
number: 15129
title: "multiple sources for same package: local path dependency and regular dependency"
type: issue
state: open
author: Ben-grmbl
labels:
  - question
assignees: []
created_at: 2025-08-07T11:54:23Z
updated_at: 2025-12-15T15:10:55Z
url: https://github.com/astral-sh/uv/issues/15129
synced_at: 2026-01-10T01:25:53Z
---

# multiple sources for same package: local path dependency and regular dependency

---

_Issue opened by @Ben-grmbl on 2025-08-07 11:54_

### Question

When developing a package that depends on another local package, I can either specify the dependency as regular pypi dependency, git dependency or local path dependency

e.g. 

```
uv add my-own-lib --index myIndex   
uv add git+https://myserver/path-to-my-own-lib 
uv add ../my-own-lib 
```

For my users, I want the first option
During local developement, I often need the second or third option

Can I configure multiple alternative sources for the same package, while keeping one version of pyproject.toml and uv.lock?

I tried doing this with optional dependency groups or dev depencies, and the [conflicting-dependencies](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies) feature, but cannot get it to work, since the packages have the same name and overwrite each other.

### Platform

_No response_

### Version

uv 0.8.5 (ce3728681 2025-08-05)

---

_Label `question` added by @Ben-grmbl on 2025-08-07 11:54_

---

_Comment by @TheNotary on 2025-09-06 19:00_

I struggled with this for a bit today.  In my situation, I had a local env and a cloud deployment, both use local dependencies, but the directory layout differed.  I wanted to have "fallbacks" for dependencies or a way of firing them conditionally.  But it turns out if any paths to dependencies don't exist, uv prints a fatal error.  

Anyway, after much experimentation, I was only able to get things working by using a single relative path, and moving the production folder paths around slightly.  

---

_Comment by @keikoro on 2025-09-12 14:05_

@TheNotary I believe your issue, while related, is slightly different in that you were trying to switch between different `paths` for known environments.

The original question, as I understood it, is about declaring different types of sources (PyPI default, custom `index`, `git`, `path`) for the same dependency in `pyproject.toml`, and being able to switch between those "dynamically" while a `uv.lock` file is present, which should remain unchanged (by e.g. any `uv sync` actions).

For which there don't seem to exist any solutions at this point.

The `dependency-groups` hacks I've seen won't work for main dependencies. Nor can e.g. `uv.toml` be used to drop in `sources` just for local development. And if a project is meant to be installed and/or worked on by other people, [environment markers](https://docs.astral.sh/uv/concepts/projects/dependencies/#multiple-sources) don't make sense either.

I like the idea of custom environment variables to differentiate sources suggested in https://github.com/astral-sh/uv/issues/7945#issuecomment-2411769064

---

_Comment by @TheNotary on 2025-09-12 14:24_

Quite true, thank you for that clarification.  I didn't mean to imply that this issue was moot.  I left my work around as an aid to anyone who didn't need to support third-party consumers, but also as an admission that more support in this area would be appreciated.  

Currently no workarounds that I've come to would apply to Ben's case, I feel it's a valid case, and if support were contributed here, it would likely benefit other use-cases wherein we have to resort to inelegant workarounds.

---

_Comment by @keikoro on 2025-09-12 18:31_

I think I've _finally_ found a workaround for my use case, even if it's not persistent:

Like @Ben-grmbl, I want my `pyproject.toml` file to default to PyPI versions of packages. For local development, I want to be able to switch to (editable) local copies of certain dependencies (without messing with `uv.lock`, which is under version control).

Background:
I used to solve this via symlinks matching import package names in my working directory, e.g. `important_dependency`, which pointed to `/PATH/TO/important-dependency`, a local copy of the dependency on my system. Whenever such a symlink was present, imports would resolve to there, i.e. the symlink would override the default version installed in `site-packages` (as per the `pyproject.toml` file).

I wasn't able to properly replicate this by putting editable local `paths` in `[tool.uv.sources]` and alternating between using `uv sync` with `--locked` (or `--frozen`) and `--no-sources`. Dependencies would often not or only partially get reinstalled from PyPI when switching back. That aside, I generally don't like the idea of having to include _my_ personal development `sources` in any "official" project config.

My solution:
With the usual symlinks in place, switch between `uv pip install -e` with local paths to replace the default PyPI versions with pointers to editable local copies and `uv sync --reinstall-package` to force reinstallation of what's defined in `pyproject.toml`. Reinstallation proved necessary to prevent `uv` from skipping dependencies when the version of the local install matches the project requirements (e.g. when I'm checked out on a branch that's based on the current version but which already has new work).

I.e.
```console
$ uv pip install -e "important-dependency @ ./important_dependency" -e "other-dependency @ ./other_dep"
```
\+
```console
$ uv sync --reinstall-package important-dependency --reinstall-package other-dependency
```

In my project, I have a helper which informs me about the origins of the relevant dependencies anyway, but on the command line, a quick check using `uv` could look something like one of the following:
```console
$ uv pip freeze | grep -i "file"
$ uv pip show important-dependency other-dependency | grep -i "editable"
```

When the requirements in `pyproject.toml` or the lock file change (independently of what I'm doing) and I want to pull in those changes, I'll obviously have to repeat the `uv pip` installing of those local dependencies in editable mode, but that's less of a hassle than e.g. having to worry about local-only sources or ephemeral changes making it into project config files. 

---

_Comment by @Capsar on 2025-12-15 15:10_

I’m encountering a similar challenge.

Would it be possible for the dependency to be installed based on the order of the available sources? For example:
1. First try to install it from `"../local-lib/"`.
2. If that’s not available, fall back to the Git repository.
3. If both are unavailable, finally try installing it from the (private) PyPI registry.

---
