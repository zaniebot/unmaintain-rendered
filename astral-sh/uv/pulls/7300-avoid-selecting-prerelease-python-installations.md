```yaml
number: 7300
title: Avoid selecting prerelease Python installations without opt-in
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-run-pre
created_at: 2024-09-11T18:17:55Z
updated_at: 2024-09-17T14:36:43Z
url: https://github.com/astral-sh/uv/pull/7300
synced_at: 2026-01-10T12:53:44Z
```

# Avoid selecting prerelease Python installations without opt-in

---

_Pull request opened by @zanieb on 2024-09-11 18:17_

Similar to our semantics for packages with pre-release versions.

We will not use prerelease versions unless there are only prerelease versions available, a specific version is requested, 
or the prerelease version is found in a reasonable source (active environment, explicit path, etc. but not `PATH`).

For example, `uv python install 3.13 && uv run python --version` will no longer use `3.13.0rc2` unless that is the only Python version available, `--python 3.13` is used, or that's the Python version that is present in `.venv`.


---

_Label `bug` added by @zanieb on 2024-09-11 18:20_

---

_Comment by @zanieb on 2024-09-11 18:20_

This change could be breaking for users with a 3.13 prerelease installed, but the existing behavior does not match our usual version semantics.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1372 on 2024-09-11 18:45_

I didn't have this at first and the Python 3.13 system test failed at https://github.com/astral-sh/uv/actions/runs/10817366263/job/30011226637

---

_@zanieb reviewed on 2024-09-11 18:45_

---

_@zanieb reviewed on 2024-09-11 18:52_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1372 on 2024-09-11 18:52_

It's still failing :/ presumably because there's another Python version available

---

_@zanieb reviewed on 2024-09-11 19:23_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1372 on 2024-09-11 19:23_

Looking at a fix in https://github.com/astral-sh/uv/pull/7306

---

_@zanieb reviewed on 2024-09-11 19:55_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1408 on 2024-09-11 19:55_

A similar change was required on Windows.

uv will now prefer another installation on the system, if it exists. In this case, Python 3.10 is available and breaks our expectations.

---

_@zanieb reviewed on 2024-09-11 19:59_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1372 on 2024-09-11 19:59_

See https://github.com/astral-sh/uv/pull/7300#discussion_r1755501611 too

---

_@charliermarsh approved on 2024-09-11 20:34_

---

_Merged by @zanieb on 2024-09-11 20:49_

---

_Closed by @zanieb on 2024-09-11 20:49_

---

_Branch deleted on 2024-09-11 20:49_

---

_Comment by @henryiii on 2024-09-16 19:16_

This also seems to break if you've set VIRTUAL_ENV to a venv with 3.13 in it? That's what cibuildwheel does; `build[uv]` is now broken in cibuildwheel (and we are pushing to get everyone to build 3.13 wheels now before release).

I think this also might break nox, tox, etc.

---

_Comment by @zanieb on 2024-09-16 22:06_

For prosperity, that isn't the case per the test in #7437 

---

_Comment by @henryiii on 2024-09-17 14:36_

Yes, the issue wasn't VIRTUAL_ENV, but rather that we were making the venv assuming it would use the Python on the top of the PATH. cibuildwheel 2.21.1 fixes this.

It's really weird, though, that it no longer takes the Python at the top of the PATH when `--system` is used. If someone goes to the trouble to put Python in the PATH, intentionally skipping over it is really surprising; and if you type `python`, you don't get the one you just installed to. For example:


```yaml
- uses: actions/setup-python@v5
  with:
    python-version: ${{ matrix.python-version }}
    allow-prereleases: true
- uses: astral-sh/setup-uv@v3
- run: uv pip install --system some_lib
- run: python -c "import some_lib"
```

now fails only on 3.13 because uv installed to the 3.12 instead of the 3.13 that's been prepared by setup-python. The `python` above is 3.13, while uv is skipping it and installing into 3.12. (I've seen this failure somewhere but have forgotten which repos it affected).

I understand this makes sense with managed Python (where you have a bunch of Pythons and uv's just picking out the best one), but it is very surprising to have it do this to the Python on PATH.

---
