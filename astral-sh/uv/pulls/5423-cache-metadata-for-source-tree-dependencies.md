```yaml
number: 5423
title: Cache metadata for source tree dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - cache
  - cli
assignees: []
merged: true
base: main
head: charlie/cache-editable
created_at: 2024-07-24T20:23:06Z
updated_at: 2024-07-25T13:45:55Z
url: https://github.com/astral-sh/uv/pull/5423
synced_at: 2026-01-10T13:37:23Z
```

# Cache metadata for source tree dependencies

---

_Pull request opened by @charliermarsh on 2024-07-24 20:23_

## Summary

This PR re-introduces caching for source trees. In short, we treat the metadata as cached unless the `pyproject.toml`, `setup.py`, or `setup.cfg` file changes. This is a heuristic and not a good one, especially for extension modules, but without it, we have to rebuild every project every time (unless you have static metadata, like a `pyproject.toml` that we can read directly).

Now that we support persistent configuration, users should add:

```toml
[tool.uv]
reinstall = ["foo"]
```

If they want a package to always be refreshed (ignore cache) and reinstalled (ignore environment).

Closes https://github.com/astral-sh/uv/issues/5420.


---

_Label `breaking` added by @charliermarsh on 2024-07-24 20:23_

---

_Comment by @charliermarsh on 2024-07-24 20:24_

Should `refresh` imply `reinstall`? I hate that you need to specify both, it's really bad. But they mean different things. `reinstall` tells us to ignore the already-installed version, and `refresh` tells us to ignore the already-cached metadata.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-24 20:26_

---

_Review requested from @konstin by @charliermarsh on 2024-07-24 20:26_

---

_Comment by @charliermarsh on 2024-07-24 20:28_

(I expect a couple tests to fail.)

---

_Comment by @charliermarsh on 2024-07-24 20:30_

I guess it would make more sense for `reinstall` to imply `refresh`.

---

_Comment by @charliermarsh on 2024-07-24 22:15_

This is only "breaking" in the sense that some things that weren't cached are now cached, so some users that rely on it may see a change in behavior. They will not see any errors.

---

_Comment by @zanieb on 2024-07-24 22:19_

I wonder if we need another label for that kind of thing? Otherwise we're automatically bumping to the next minor version because of the breaking label (which is nice for flagging things, but annoying to change as a release manager. Alternatively, I can document how to bypass the automatic bump in Rooster?

---

_Label `breaking` removed by @charliermarsh on 2024-07-24 22:23_

---

_Label `cache` added by @charliermarsh on 2024-07-24 22:23_

---

_Label `cli` added by @charliermarsh on 2024-07-24 22:23_

---

_Comment by @charliermarsh on 2024-07-24 22:23_

I relabeled it though not sure what the best options are.

---

_@konstin approved on 2024-07-25 09:56_

---

_Merged by @charliermarsh on 2024-07-25 13:45_

---

_Closed by @charliermarsh on 2024-07-25 13:45_

---

_Branch deleted on 2024-07-25 13:45_

---
