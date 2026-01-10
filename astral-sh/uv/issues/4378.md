```yaml
number: 4378
title: Cache issue when installing from git urls with once missing refs
type: issue
state: closed
author: elupus
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-06-18T08:46:52Z
updated_at: 2024-07-01T17:03:51Z
url: https://github.com/astral-sh/uv/issues/4378
synced_at: 2026-01-10T05:31:37Z
```

# Cache issue when installing from git urls with once missing refs

---

_Issue opened by @elupus on 2024-06-18 08:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have an issue where the cache seem to get "corrupt". I have a project with git references. If the install was attempted once when the target SHA had not landed in the upstream URL, the cache is broken for this sha and does not recover with a "--refresh" once the sha does exist in the upstream repo.

I don't have a repro case just yet will see if i can figure out how to make one. But suspicion is that the git reset that fails below is performed before a new fetch has been performed from the upstream git repo.

Platform: Ubuntu 22.04.2 LTS
Uv: uv 0.2.12

```
<package> @ git+ssh://<url>@<sha>#subdirectory=<dir>
```

```
uv pip install --verbose --refresh -r tests/requirements.txt
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.10.12 at `/home/rtljopl/tecu/application/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.12 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: <package> @ git+ssh://<url>@<sha>#subdirectory=<dir>

DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: ssh://<url>
DEBUG Acquired lock for `ssh://<url>`
DEBUG reset /home/rtljopl/.cache/uv/git-v0/checkouts/f5528ce19486c11e/<sha> to <sha>
error: Failed to download and build: `<package> @ git+ssh://<url>@<sha>#subdirectory=<package>`
  Caused by: Git operation failed
  Caused by: process didn't exit successfully: `git reset --hard <sha>` (exit status: 128)
--- stderr
fatal: Could not parse object '<sha>'.
```


---

_Comment by @ibraheemdev on 2024-06-18 17:07_

Interesting, if the local repository is corrupted we do a fresh clone and [it looks like we assume the previous locked revision is correct](https://github.com/astral-sh/uv/blob/main/crates/uv-git/src/git.rs#L250), even if we may not have fetched that refspec correctly. However, I'm not sure why we would have a locked revision in your case because the checkout would have failed. A reproduction would be helpful.

---

_Label `bug` added by @ibraheemdev on 2024-06-18 17:07_

---

_Label `needs-mre` added by @zanieb on 2024-06-18 17:46_

---

_Comment by @eifinger on 2024-06-26 14:33_

I just encountered the same problem in a project using rye with uv enabled.

1. Have a project using a dependency in `pyproject.toml` in the form of `<PACKAGE_NAME> @ git+ssh://git@github.com/<ORG>/<PACKAGE_NAME>.git@<GIT_HASH>`
2. run `rye sync` with uv enabled
3. Push a new commit to `<ORG>/<PACKAGE_NAME>`
4. change <GIT_HASH> in `pyproject.toml`
5. run `rye sync`
6. Encounter the error above

---

_Comment by @eifinger on 2024-06-27 07:09_

Temporary workaround for me is `UV_NO_CACHE=true rye sync`. I have not yet used `uv` directly but for the initial problem @elupus reported this should work `UV_NO_CACHE=true uv pip install --verbose --refresh -r tests/requirements.txt`

---

_Comment by @zanieb on 2024-06-27 10:50_

Just as a minor note, using `--no-cache` with `--refresh` doesn't make sense, I'd use one or the other. Generally `--refresh-package <name>` is the best option.

---

_Closed by @ibraheemdev on 2024-07-01 17:01_

---

_Closed by @ibraheemdev on 2024-07-01 17:01_

---

_Comment by @elupus on 2024-07-01 17:03_

Beautiful! Thanks!

---
