```yaml
number: 7056
title: "Document official `setup-uv` action"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/gha-setup-uv
created_at: 2024-09-04T23:44:13Z
updated_at: 2024-09-05T17:59:03Z
url: https://github.com/astral-sh/uv/pull/7056
synced_at: 2026-01-12T16:07:40Z
```

# Document official `setup-uv` action

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/7047.


---

_Label `documentation` added by @charliermarsh on 2024-09-04 23:44_

---

_@charliermarsh reviewed on 2024-09-04 23:44_

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:125 on 2024-09-04 23:44_

I find it useful to include the full example since it allows users to copy-paste. Any objections?

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:218 on 2024-09-04 23:44_

I don't know if we support custom `restore-keys` yet, looking into it.

---

_@charliermarsh reviewed on 2024-09-04 23:44_

---

_@charliermarsh reviewed on 2024-09-04 23:46_

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:218 on 2024-09-04 23:46_

Looks like the cache already does this.

---

_Comment by @charliermarsh on 2024-09-04 23:47_

\cc @eifinger

---

_Review requested from @zanieb by @charliermarsh on 2024-09-04 23:47_

---

_@zanieb reviewed on 2024-09-04 23:54_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:125 on 2024-09-04 23:54_

If you'll be repeating the full example you should use `hl_lines` to highlight the change.

---

_@charliermarsh reviewed on 2024-09-05 00:19_

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:125 on 2024-09-05 00:19_

You're a genius...

---

_Review comment by @j178 on `docs/guides/integration/github.md`:25 on 2024-09-05 01:30_

`latest` seems not the default?

> By default this action installs the version defined as default in action.yml. This gets automatically updated in a new release of this action when a new version of uv is released. If you don't want to wait for a new release of this action you can use version: latest.

---

_@j178 reviewed on 2024-09-05 01:30_

---

_@charliermarsh reviewed on 2024-09-05 01:40_

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:25 on 2024-09-05 01:40_

https://github.com/astral-sh/setup-uv/pull/37

---

_@eifinger reviewed on 2024-09-05 07:05_

---

_Review comment by @eifinger on `docs/guides/integration/github.md`:218 on 2024-09-05 07:05_

Yes. You can also supply e.g. `requirements**.txt`

---

_@eifinger reviewed on 2024-09-05 07:10_

---

_Review comment by @eifinger on `docs/guides/integration/github.md`:201 on 2024-09-05 07:10_

```suggestion
You can configure the action to use a custom cache directory on the runner:
```

What about being more explicit here so people know we don't mean the GitHub Actions Cache?

---

_@zanieb approved on 2024-09-05 17:12_

---

_Merged by @charliermarsh on 2024-09-05 17:59_

---

_Closed by @charliermarsh on 2024-09-05 17:59_

---

_Branch deleted on 2024-09-05 17:59_

---
