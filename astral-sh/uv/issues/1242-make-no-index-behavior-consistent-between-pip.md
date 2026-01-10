```yaml
number: 1242
title: "Make `--no-index` behavior consistent between `pip sync` and `pip install`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-02-03T16:45:22Z
updated_at: 2024-02-04T23:46:03Z
url: https://github.com/astral-sh/uv/issues/1242
synced_at: 2026-01-10T05:40:31Z
```

# Make `--no-index` behavior consistent between `pip sync` and `pip install`

---

_Issue opened by @charliermarsh on 2024-02-03 16:45_

If you run `pip sync --no-index`, the install plan _will_ allow you to use cached distributions. But if you run `pip install --no-index`, it will fail (unless you're using local dependencies of `--find-links`, etc.), since we won't be able to find the _metadata_ for the package you're installing.

We should probably have a consistent experience here. Arguably, `--no-index` shouldn't work in either case? We could also consider adding an `--offline` option to force reading from the cache (but still respecting indexes).


---

_Label `enhancement` added by @charliermarsh on 2024-02-03 16:45_

---

_Comment by @konstin on 2024-02-03 16:48_

I'd stick to pip and have any action fail if there is a package with only name (no path/url), you're using `--no-index` and there's no matching find links index. From `pip install --help`:

>   --no-index                  Ignore package index (only looking at --find-links URLs instead).

---

_Comment by @charliermarsh on 2024-02-03 16:53_

Ok, so make `pip sync` more strict, right?

---

_Comment by @konstin on 2024-02-03 16:53_

yes

---

_Comment by @zanieb on 2024-02-03 16:54_

Added a test case in https://github.com/astral-sh/puffin/pull/1241/commits/915bd38f32cac392aa1860b07cdfb68b08629db6 and the behavior looks consistent with `pip install`?

---

_Comment by @charliermarsh on 2024-02-03 17:33_

Sorry yes, you're right, the behavior _is_ consistent. We may just want to change the documentation for the setting. (And we may want a `--offline` flag, which is tracked elsewhere.)

---

_Closed by @charliermarsh on 2024-02-04 23:46_

---
