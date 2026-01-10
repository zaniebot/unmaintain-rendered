```yaml
number: 3601
title: uv run can install two versions of the same package (and forces upgrades)
type: issue
state: closed
author: henryiii
labels:
  - bug
  - preview
assignees: []
created_at: 2024-05-15T04:13:27Z
updated_at: 2024-05-15T15:48:33Z
url: https://github.com/astral-sh/uv/issues/3601
synced_at: 2026-01-10T05:31:37Z
```

# uv run can install two versions of the same package (and forces upgrades)

---

_Issue opened by @henryiii on 2024-05-15 04:13_

I was trying out different versions of pytest with a pytest plugin to see when it was broken. I mentally was thinking of `uv run` as the Python launcher for UNIX, a way to avoid activating a venv (`py` instead of `.venv/bin/python`). It seems to do some installing automatically - including three strange things:

```console
$ uv venv
$ uv pip install -e .
$ uv pip install 'pytest==7.0.*'
$ uv run pytest
warning: `uv run` is experimental and may change without warning.
Resolved 5 packages in 8ms
   Built pytest-github-actions-annotate-failures @ file:///Users/henryschreiner/git/software/pytest-github-actions-annotate-failures                                                                    Downloaded 1 package in 1.25s
Installed 2 packages in 33ms
 - pytest==7.0.1
 + pytest==8.2.0
 - pytest-github-actions-annotate-failures==0.2.0 (from file:///Users/henryschreiner/git/software/pytest-github-actions-annotate-failures)
 + pytest-github-actions-annotate-failures==0.2.0 (from file:///Users/henryschreiner/git/software/pytest-github-actions-annotate-failures)
... pytest output ...
```

At this point, I hadn't noticed it had forcibly upgraded the pytest I was testing to the latest version, so I thought 7.0.x worked. (Some else in parallel found it was 7.4 that broke us).

Then the next line caught me by surprise, it uninstalls two pytests:

```console
$ uv pip install 'pytest==6.*'
Resolved 7 packages in 145ms
Downloaded 2 packages in 68ms
Installed 2 packages in 6ms
 - pytest==7.0.1
 - pytest==8.2.0
 + pytest==6.2.5
 + toml==0.10.2
```

And finally, the final `uv run` actually works just fine, and doesn't upgrade pytest:

```console
$ uv run pytest
... pytest output ...
```


---

_Comment by @charliermarsh on 2024-05-15 04:16_

Thanks. To be clear, I wouldn't recommend `uv run` for "real" use yet but I do appreciate bug reports. Are you able to reproduce the double-uninstall reliably?

---

_Comment by @henryiii on 2024-05-15 06:20_

Same sequence does do it again:

```console
$ uv venv
$ uv pip install 'pytest==7.0.*'
$ uv run pytest
$ uv pip list
Package                                 Version
--------------------------------------- -------
attrs                                   23.2.0
iniconfig                               2.0.0
packaging                               24.0
pluggy                                  1.5.0
py                                      1.11.0
pytest                                  7.0.1
pytest                                  8.2.0
pytest-github-actions-annotate-failures 0.2.0
tomli                                   2.0.1
```

The editible install line can be skipped, same result. Using checkout of https://github.com/pytest-dev/pytest-github-actions-annotate-failures. 

---

_Comment by @charliermarsh on 2024-05-15 12:56_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-15 12:56_

---

_Comment by @zanieb on 2024-05-15 13:13_

@charliermarsh this looks like it might be because we use `EmptyInstalledPackages` in `resolve` instead of passing the `SitePackages` we grabbed earlier.

---

_Comment by @charliermarsh on 2024-05-15 14:35_

Thanks @zanieb.

---

_Label `bug` added by @charliermarsh on 2024-05-15 14:35_

---

_Label `preview` added by @charliermarsh on 2024-05-15 14:35_

---

_Comment by @henryiii on 2024-05-15 14:48_

> To be clear, I wouldn't recommend uv run for "real" use yet

If it were finished, where would the fun be in me using it? ;)

---

_Closed by @charliermarsh on 2024-05-15 15:48_

---
