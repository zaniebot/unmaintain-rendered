```yaml
number: 1301
title: Files that are ignored in config are checked when passed explicitly on the command line
type: issue
state: closed
author: cpcloud
labels:
  - question
assignees: []
created_at: 2022-12-20T18:32:03Z
updated_at: 2022-12-20T18:45:50Z
url: https://github.com/astral-sh/ruff/issues/1301
synced_at: 2026-01-12T15:54:41Z
```

# Files that are ignored in config are checked when passed explicitly on the command line

---

_@cpcloud_

When a file is ignored in config, but then explicitly passed on the command line `ruff` checks it anyway.

Here's a reproducing [gist](https://gist.github.com/cpcloud/fcd285f87486a8bc712911ffc289310c).

This makes running `pre-commit run --all-files` (using the official `ruff` pre-commit hook) fail, because files are passed to `ruff` explicitly.

Ideally, the config is followed no matter what's passed on the command line in this case.

---

_Comment by @charliermarsh on 2022-12-20 18:38_

Ah yeah, this is actually intentional and is the same strategy used by Black. In short, if you pass a file directly to `ruff`, we don't ignore it, which is almost always desirable, except in the case of `pre-commit`.

In [v0.0.189](https://github.com/charliermarsh/ruff/releases/tag/v0.0.189), I added a `--force-exclude` boolean flag that disables that behavior, and causes Ruff to exclude files regardless of whether they're passed directly.

You should be able to either set that in your `pyproject.toml`, or better yet pass it as an arg to the pre-commit invocation.


---

_Label `question` added by @charliermarsh on 2022-12-20 18:38_

---

_Comment by @cpcloud on 2022-12-20 18:45_

Excellent, thank you!

---

_Closed by @cpcloud on 2022-12-20 18:45_

---
