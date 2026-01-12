```yaml
number: 7538
title: Fix gitignore to not ignore files that are required
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: clear-git-cache
created_at: 2023-09-20T10:13:56Z
updated_at: 2023-09-21T19:33:10Z
url: https://github.com/astral-sh/ruff/pull/7538
synced_at: 2026-01-12T15:55:24Z
```

# Fix gitignore to not ignore files that are required

---

_@konstin_

It is apparently possible to add files to the git index, even if they are part of the gitignore (see e.g. https://stackoverflow.com/questions/45400361/why-is-gitignore-not-ignoring-my-files, even though it's strange that the gitignore entries existed before the files were added, i wouldn't know how to get them added in that case). I ran
```
git rm -r --cached .
```
then change the gitignore not actually ignore those files with the exception of `crates/ruff_cli/resources/test/fixtures/cache_mutable/source.py`, which is actually a generated file.

---

_Label `internal` added by @konstin on 2023-09-20 10:13_

---

_Label `?̶̡̦̼̬̹̜̭̹̮͗̈̍̐̆̚̚?̵̧̹̤̠̪̥̯̼̫̖͙͔̤͘ͅ?̵͉̬̖͙̣̄͆́͝` added by @konstin on 2023-09-20 10:13_

---

_Comment by @charliermarsh on 2023-09-20 12:59_

I haven't looked into this deeply but I think we need most of these files?

---

_Comment by @konstin on 2023-09-20 13:28_

yes i think i have to fix the gitignore (and then figure out testing, because the files locally still exist, so the tests still pass, they are just deleted from what git tracks)

---

_Renamed from "`git rm -r --cached . && git add .`" to "Fix gitignore to not ignore files that are required" by @konstin on 2023-09-21 07:51_

---

_@konstin reviewed on 2023-09-21 07:58_

---

_Review comment by @konstin on `.gitignore`:55 on 2023-09-21 07:58_

personally, i'd just remove everything from gitignore we don't explicitly need. in this case, `lib` would ignore part of the resolver test case (and nobody should be writing e.g. eggs anymore anyway)

Also good news, venvs created by `python -m venv` will gitignore themselves from python 3.13 on: https://github.com/python/cpython/pull/108125. venvs created by `virtualenv` already gitignore themselves for a while already (https://github.com/pypa/virtualenv/pull/1825)

---

_Marked ready for review by @konstin on 2023-09-21 07:59_

---

_@charliermarsh reviewed on 2023-09-21 13:21_

---

_Review comment by @charliermarsh on `crates/ruff_cli/resources/test/fixtures/cache_mutable/source.py`:3 on 2023-09-21 13:21_

What's the deal with this file? Is it just not used at all?

---

_@charliermarsh reviewed on 2023-09-21 13:23_

---

_Review comment by @charliermarsh on `.gitignore`:55 on 2023-09-21 13:23_

I personally find this useful because it matches the exact GitHub-recommended `.gitignore`, so it's easy to maintain and update in the future if needed.

---

_@charliermarsh reviewed on 2023-09-21 13:24_

---

_Review comment by @charliermarsh on `.gitignore`:152 on 2023-09-21 13:24_

Can we move this up to the project-specific ignores at the top?

---

_@konstin reviewed on 2023-09-21 15:32_

---

_Review comment by @konstin on `crates/ruff_cli/resources/test/fixtures/cache_mutable/source.py`:3 on 2023-09-21 15:32_

The test cases create this file

https://github.com/astral-sh/ruff/blob/7f1456a2c9019f543fde71493aa476f132c84a57/crates/ruff_cli/src/cache.rs#L471

---

_@konstin reviewed on 2023-09-21 15:32_

---

_Review comment by @konstin on `.gitignore`:152 on 2023-09-21 15:32_

unfortunately not, the reinclusion needs to happen after the exclusion

---

_Comment by @konstin on 2023-09-21 15:33_

I've changed it to a minimal diff

---

_@charliermarsh approved on 2023-09-21 15:35_

---

_Merged by @konstin on 2023-09-21 19:33_

---

_Closed by @konstin on 2023-09-21 19:33_

---

_Branch deleted on 2023-09-21 19:33_

---
