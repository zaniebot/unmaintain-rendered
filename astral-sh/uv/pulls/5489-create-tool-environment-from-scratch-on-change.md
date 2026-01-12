```yaml
number: 5489
title: Create tool environment from scratch on change
type: pull_request
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
base: main
head: charlie/sync-tool
created_at: 2024-07-26T19:36:08Z
updated_at: 2024-07-31T16:17:05Z
url: https://github.com/astral-sh/uv/pull/5489
synced_at: 2026-01-12T16:06:51Z
```

# Create tool environment from scratch on change

---

_@charliermarsh_

## Summary

Right now, if you install a tool with some new requirements, we attempt to reuse the environment and update it in-place. This leads to some unintuitive behaviors with editables and direct URLs.

For example, if you `tool install ./scripts/packages/black_editable` and then `tool install black`, we don't actually make any changes to the environment. This _does_ match our `pip install ./scripts/packages/black_editable` and then `pip install black` behavior, but I'd argue that users don't think of tool installs as stateful in this way.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 19:36_

---

_Label `preview` added by @charliermarsh on 2024-07-26 19:36_

---

_Comment by @zanieb on 2024-07-30 19:44_

Does this mean `--upgrade-package <name>` is useless?

---

_Comment by @charliermarsh on 2024-07-30 19:45_

@zanieb -- Yeah.

---

_Comment by @charliermarsh on 2024-07-30 19:46_

We could consider _not_ doing this if we do the "store a full-fidelity receipt" change.

---

_Comment by @zanieb on 2024-07-30 19:50_

While I generally agree the behavior you outlined could be confusing, I think the "full fidelity receipt" makes more sense as a solution? It seems nice to be able to upgrade a single package if you need to, for example.

---

_Comment by @charliermarsh on 2024-07-30 19:52_

Let's see that change through and revisit this after.

---

_Comment by @charliermarsh on 2024-07-31 16:17_

Closing for now in favor of #5494. We can revisit.

---

_Closed by @charliermarsh on 2024-07-31 16:17_

---
