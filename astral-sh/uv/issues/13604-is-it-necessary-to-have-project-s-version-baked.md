```yaml
number: 13604
title: "Is it necessary to have project's version baked into the lockfile"
type: issue
state: closed
author: ggydush
labels:
  - question
assignees: []
created_at: 2025-05-22T21:20:03Z
updated_at: 2025-07-04T02:18:31Z
url: https://github.com/astral-sh/uv/issues/13604
synced_at: 2026-01-12T16:01:33Z
```

# Is it necessary to have project's version baked into the lockfile

---

_@ggydush_

### Question

We experienced this [issue](https://github.com/astral-sh/uv/pull/13317) a few days ago, which was promptly resolved. Thinking about this more, I'm wondering what the point of having the local package's version included in the lockfile. This prevents Docker from being able to cache all of the dependency layers when the package version changes. Poetry takes a different approach (changes to local package version do not change lockfile), so I was curious what your thoughts were on this.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ggydush on 2025-05-22 21:20_

---

_Comment by @konstin on 2025-05-28 14:59_

Unlike poetry, our approach allows installing from the lockfile alone (with `--frozen`), while poetry need `pyproject.toml` to use its lockfile. Usually, the project version doesn't change often, so the cache is still used most of the time.

---

_Closed by @charliermarsh on 2025-07-04 02:18_

---
