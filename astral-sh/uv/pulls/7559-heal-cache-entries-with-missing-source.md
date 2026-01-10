```yaml
number: 7559
title: Heal cache entries with missing source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/cache-safety
created_at: 2024-09-19T19:26:38Z
updated_at: 2024-09-19T20:31:56Z
url: https://github.com/astral-sh/uv/pull/7559
synced_at: 2026-01-10T12:53:50Z
```

# Heal cache entries with missing source distributions

---

_Pull request opened by @charliermarsh on 2024-09-19 19:26_

## Summary

`uv cache prune --ci` will remove the source distribution directory. If we then need to build a _different_ wheel (e.g., you're building a package that has Python minor version-specific wheels), we fail, because we expect the source to be there.

Now, if the source is missing, we re-download it. It would be slightly easier to just _ignore_ that revision, but that would mean we'd also lose the already-built wheels -- so if you ran against many Python versions, we'd continuously lose the cached data.

Closes https://github.com/astral-sh/uv/issues/7543.

## Test Plan

We can add tests, but they _need_ to build non-pure Python wheels, which tends to be expensive...

For reference:

```console
$ cargo run venv --python 3.12
$ cargo run pip install mercurial==6.8.1 --verbose
$ cargo run cache prune --ci
$ cargo run venv --python 3.11
$ cargo run pip install mercurial==6.8.1 --verbose
```

I also did this with a local `.tar.gz` that I downloaded from PyPI.

---

_Label `bug` added by @charliermarsh on 2024-09-19 19:27_

---

_Label `cache` added by @charliermarsh on 2024-09-19 19:27_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-19 19:27_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-19 19:28_

---

_Converted to draft by @charliermarsh on 2024-09-19 19:42_

---

_@zanieb approved on 2024-09-19 19:42_

---

_Comment by @charliermarsh on 2024-09-19 19:42_

Ahh sorry, that test failure is correct. This isn't quite right.

---

_Comment by @zanieb on 2024-09-19 19:42_

Uh oh!

---

_Marked ready for review by @charliermarsh on 2024-09-19 20:15_

---

_Comment by @charliermarsh on 2024-09-19 20:15_

Ok, a bit more invasive but better.

---

_Merged by @charliermarsh on 2024-09-19 20:31_

---

_Closed by @charliermarsh on 2024-09-19 20:31_

---

_Branch deleted on 2024-09-19 20:31_

---
