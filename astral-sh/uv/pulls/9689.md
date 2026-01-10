```yaml
number: 9689
title: "Docs: Add details of `--seed` to pip environments docs"
type: pull_request
state: open
author: GCHQDeveloper314
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2024-12-06T16:32:46Z
updated_at: 2026-01-05T09:28:20Z
url: https://github.com/astral-sh/uv/pull/9689
synced_at: 2026-01-10T05:49:14Z
```

# Docs: Add details of `--seed` to pip environments docs

---

_Pull request opened by @GCHQDeveloper314 on 2024-12-06 16:32_

Add a mention of how to include pip when creating a virtual environment. This will help avoid confusion for those who are expecting and/or require pip in the environment (e.g. #3876).


---

_Renamed from "docs: add details of `--seed` to pip environments docs" to "Docs: Add details of `--seed` to pip environments docs" by @GCHQDeveloper314 on 2025-03-11 12:46_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-14 00:30_

---

_Label `documentation` added by @charliermarsh on 2025-03-14 00:30_

---

_Comment by @GCHQDeveloper314 on 2026-01-02 11:45_

@zanieb Checks were showing as pending, so I've rebased and they're now passing

---

_@woodruffw approved on 2026-01-02 18:05_

Looks reasonable to me! 

We could also maybe mention `UV_VENV_SEED` as an equivalent, but doesn't seem critical.

---

_Comment by @zanieb on 2026-01-03 13:47_

I'm a little worried this will cause confusion for beginners 🤔 

---

_Review comment by @zanieb on `docs/pip/environments.md`:41 on 2026-01-03 13:48_

```suggestion
uv does not include `pip` in virtual environments by default. To include it, use [`--seed`](../reference/cli.md#uv-venv--seed):

```console
$ uv venv --seed
```
```

---

_@zanieb reviewed on 2026-01-03 13:48_

---

_Review comment by @zanieb on `docs/pip/environments.md`:41 on 2026-01-03 13:48_

(GitHub of course cannot handle suggestions with code blocks so this is broken)

---

_@zanieb reviewed on 2026-01-03 13:48_

---

_Review comment by @GCHQDeveloper314 on `docs/pip/environments.md`:41 on 2026-01-05 09:28_

Should hopefully be applied now. I've removed the bit about "may also include other seed packages", as this is fully documented at that link, no need duplicate with ambiguous wording.

---

_@GCHQDeveloper314 reviewed on 2026-01-05 09:28_

---
