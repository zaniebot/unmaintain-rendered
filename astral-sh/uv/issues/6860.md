```yaml
number: 6860
title: dynamic version in editable install
type: issue
state: closed
author: pmeier
labels:
  - needs-design
assignees: []
created_at: 2024-08-30T12:06:53Z
updated_at: 2024-12-19T16:41:10Z
url: https://github.com/astral-sh/uv/issues/6860
synced_at: 2026-01-10T04:36:20Z
```

# dynamic version in editable install

---

_Issue opened by @pmeier on 2024-08-30 12:06_

[PEP 621](https://peps.python.org/pep-0621/) allows for dynamic metadata in `pyproject.toml`. Tools like [`setuptools-scm`](https://pypi.org/project/setuptools-scm/) make use of this feature by computing the version of the package at build time.

Imagine I have a git repository with nothing but this `pyproject.toml` file that is committed.

```toml
[build-system]
requires = ["setuptools>=64", "setuptools_scm>=8"]
build-backend = "setuptools.build_meta"

[project]
name = "foo"
dynamic = ["version"]

[tool.uv]
dev-dependencies = [
    "setuptools-scm>=8.1.0",
]

[tool.setuptools_scm]
```

```shell
$ ls -lA
total 8
drwxr-xr-x 8 pmeier pmeier 4096 Aug 30 11:40 .git
-rw-r--r-- 1 pmeier pmeier  238 Aug 30 11:34 pyproject.toml
$ git log --oneline
1105907 (HEAD -> main) initial commit
```

Running `uv sync` gives me the following block in `uv.lock`

```
[[package]]
name = "foo"
version = "0.1.dev1+g1105907"
source = { editable = "." }
```

We commit that and move on with development.

```shell
$ git add uv.lock
$ git commit -m "add lock" > /dev/null
$ git log --oneline
409a76d (HEAD -> main) add lock
1105907 initial commit
```

Re-running, `uv sync` results in no changes in the lock file or venv, although we have

```shell
$ uv run python -m setuptools_scm
0.1.dev2+g409a76d
```

Running `uv sync --reinstall-package foo` installs the correct version into the venv (checked with `uv pip show foo`), but again the `uv.lock` file stays untouched. Even why I manually change the version in the lock file to `0+dynamic` (`uv` complains if the string doesn't start with a number), `uv lock` does not change the `uv.lock` file at all.

IMO, in general this is good behavior given that it is impossible to lock the version of this editable package, since its version is computed dynamically. However, I'd prefer not have a quickly outdated version number "hardcoded" in the lock file.

Would it be possible to set the version to `"dynamic"` (or a different sentinel) if the package is an editable install and declares that the version is computed dynamically?


---

_Comment by @AbdealiLoKo on 2024-08-30 12:26_

I was using dynamic dependencies and version with setup.py/setuptools-scm and had the same issue.
So, this is kinda similar: https://github.com/astral-sh/uv/issues/6822

Currently uv aggressively caches packages, and dynamic metadata are not considered to break the cache. So, as per the current documentation what you are seeing is expected behavior.
https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata

You can set `reinstall-package = [...]` to always reinstall specific packages where you know dynamic metadata is present

It did feel confusing to me when I saw it first too - and I feel like there will be many more such issues as users use uv more :)
Maybe worth enhancing !


---

_Label `needs-design` added by @charliermarsh on 2024-08-30 12:51_

---

_Comment by @charliermarsh on 2024-08-30 12:51_

Agree that what you're seeing here (persisting the stale hash) is not quite right...

---

_Comment by @pmeier on 2024-08-30 12:53_

Thanks a ton for the pointer. Same as you, I did not find this in the documentation. 

In my case I don't need and more specifically don't want re-installation given that I already have an editable install for a pure Python package. I just don't want an outdated package version in my lock file. Setting it to something like `"0+dynamic"` seems to work, but I have no idea how brittle this is, i.e. does `uv` at some point simply override this?

---

_Closed by @charliermarsh on 2024-09-09 20:19_

---

_Closed by @charliermarsh on 2024-09-09 20:19_

---

_Reopened by @charliermarsh on 2024-09-23 01:15_

---

_Comment by @charliermarsh on 2024-09-23 01:15_

This is still somewhat unsolved

---

_Comment by @cpascual on 2024-11-04 10:50_

Should this discussion be merged with #7533 ?



---

_Comment by @pmeier on 2024-11-11 20:44_

Updating this after trying the same workflow on `uv==0.5.1`:

- Putting in a sentinel like `0+dynamic` for the version into the `uv.lock` file no longer works. It gets replaced every time I run `uv sync`
- Running `uv sync` does not re-trigger a build. Meaning, if the version changes between two runs, e.g. by adding a `git tag` in between, the `uv.lock` file stays untouched
- `uv sync --reinstall-package foo` triggers a rebuild and updates the `uv.lock` file

---

_Comment by @my1e5 on 2024-11-11 23:26_

> Running uv sync does not re-trigger a build. Meaning, if the version changes between two runs, e.g. by adding a git tag in between, the uv.lock file stays untouched

@pmeier - On this point, are you using the latest improvements to `cache-keys`? See https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata

```
[tool.uv]
cache-keys = [{ git = { commit = true, tags = true } }]
```

Git tag support recently got added. See
* https://github.com/astral-sh/uv/issues/7997 
* https://github.com/astral-sh/uv/pull/8259

So I believe you should be able to trigger a re-build with just `uv sync` if a new Git tag is detected. 

As for `uv.lock` file behaviour, that I'm less sure about. The topic is more fully discussed in 
* https://github.com/astral-sh/uv/issues/7533

---

_Comment by @pmeier on 2024-11-18 09:37_

Thanks for the info @my1e5, but this is the opposite of what I want to do. Since the version of my app changes with every single commit and tag, but for most of the time no dependencies change, I'd like the `uv.lock` file just to stay constant. Whatever version that `uv` puts into the `uv.lock` file is going to be obsolete very soon.

Basically fresh development installs will always have to be done with `uv sync --frozen` to avoid adding "arbitrary" changes to the `uv.lock` file.

---

_Comment by @zanieb on 2024-11-18 15:36_

Most of the discussion is happening in https://github.com/astral-sh/uv/issues/7533 — let's track there to avoid splitting the conversation.

---

_Closed by @zanieb on 2024-11-18 15:36_

---
