```yaml
number: 8311
title: ruff much slower with --fix even without lint errors
type: issue
state: closed
author: lukaspiatkowski
labels:
  - performance
assignees: []
created_at: 2023-10-28T17:50:30Z
updated_at: 2023-10-29T16:06:37Z
url: https://github.com/astral-sh/ruff/issues/8311
synced_at: 2026-01-10T11:09:50Z
```

# ruff much slower with --fix even without lint errors

---

_Issue opened by @lukaspiatkowski on 2023-10-28 17:50_

When I call `ruff .` it finished in 0.15s on my repo, but `ruff --fix .` takes >2.5s, even if there are no lint errors and nothing to fix. Is it safe to always call `ruff .` and run it again with --fix only when the are some fixable issues? If so then maybe there is some room for improvement in ruff itself?

My use case was that we have replaced isort (and few other tools) with ruff --fix and it is much faster, but I was looking into speeding it up even more. We run black && ruff --fix, black takes 0.8s and I hoped to speed it up with `ruff format`, but then noticed that ruff --fix takes most of the execution time.

---

_Comment by @charliermarsh on 2023-10-28 18:12_

Let me look into this… I think we might be intentionally skipping the cache when the linter is run with the fix flag, but that may no longer be necessary for a variety of reasons. At least, I don’t see an inherent reason why we need to skip the cache any more.

But yes — you can always run ruff with fix after running ruff, only if there are any fixable issues.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-28 18:13_

---

_Label `performance` added by @charliermarsh on 2023-10-28 18:13_

---

_Closed by @charliermarsh on 2023-10-29 16:06_

---
