```yaml
number: 11647
title: "Update the \"Locking and syncing\" page"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/sync-docs-ii
created_at: 2025-02-19T22:57:03Z
updated_at: 2025-02-26T18:20:28Z
url: https://github.com/astral-sh/uv/pull/11647
synced_at: 2026-01-12T16:09:56Z
```

# Update the "Locking and syncing" page

---

_@zanieb_

I need to self-review this still.

Updates the "Locking and syncing" page to actually have content on syncing — which was the original intent, the rest of this file was just copied out of the "Projects" page when I split it into multiple pages.

---

_Label `documentation` added by @zanieb on 2025-02-19 22:57_

---

_Comment by @zanieb on 2025-02-19 23:16_

cc @edmorley if you're interested — I'd be curious to hear if this covers what you're looking for.

---

_Comment by @zanieb on 2025-02-20 20:44_

@charliermarsh I won't get to this before Monday. Feel free to review if you'd like.

---

_Review comment by @edmorley on `docs/concepts/projects/sync.md`:38 on 2025-02-25 11:10_

```suggestion
For example, if you add a dependency to your `pyproject.toml`, the `uv.lock` will be outdated.
```

---

_Review comment by @edmorley on `docs/concepts/projects/sync.md`:41 on 2025-02-25 11:11_

(dangling sentence; but presuming due to this being a draft)

---

_Review comment by @edmorley on `docs/concepts/projects/sync.md`:82 on 2025-02-25 11:12_

```suggestion
    If the project does not define a build system, it will not be installed.
```

---

_Review comment by @edmorley on `docs/concepts/projects/sync.md`:182 on 2025-02-25 11:17_

(though the existing wording possibly also correct?)

```suggestion
If you need to integrate uv with other tools or workflows, you can export `uv.lock` to the
```

---

_@edmorley reviewed on 2025-02-25 11:21_

> cc @edmorley if you're interested — I'd be curious to hear if this covers what you're looking for.

Sorry for the delayed reply - never enough hours in the day!

The additions/changes here are excellent - they cover the things I had in mind and more, whilst still remaining clear and concise. Much better than anything I could have worded - thank you! 😄 

---

_Comment by @zanieb on 2025-02-26 17:38_

No problem! Thank you so much for giving it a look.

---

_Marked ready for review by @zanieb on 2025-02-26 17:47_

---

_@charliermarsh reviewed on 2025-02-26 17:55_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:14 on 2025-02-26 17:55_

Should this be `--frozen`?

---

_@charliermarsh reviewed on 2025-02-26 17:55_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:14 on 2025-02-26 17:55_

Nevermind.

---

_@charliermarsh reviewed on 2025-02-26 17:55_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:20 on 2025-02-26 17:55_

```suggestion
If the lockfile is not up-to-date, uv will raise an error instead of updating the lockfile.
```

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:38 on 2025-02-26 17:56_

```suggestion
For example, if you add a dependency to your `pyproject.toml`, the lockfile will be considered outdated.
```

---

_@charliermarsh reviewed on 2025-02-26 17:57_

---

_@charliermarsh reviewed on 2025-02-26 17:57_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:40 on 2025-02-26 17:57_

```suggestion
excluded, the lockfile will be considered outdated. However, if you change the version constraints such that
```

---

_@charliermarsh reviewed on 2025-02-26 17:57_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:60 on 2025-02-26 17:57_

```suggestion
While the lockfile is created [automatically](#automatic-lock-and-sync) via `uv run`, the lockfile may also be
```

---

_@charliermarsh reviewed on 2025-02-26 17:58_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:69 on 2025-02-26 17:58_

```suggestion
While the environment is synced [automatically](#automatic-lock-and-sync) via `uv run`, it may also be explicitly
```

---

_@charliermarsh reviewed on 2025-02-26 17:58_

---

_Review comment by @charliermarsh on `docs/concepts/projects/sync.md`:82 on 2025-02-26 17:58_

```suggestion
_editable_ packages, such that re-syncing is not necessary for changes to be reflected in the
environment.
```

---

_@charliermarsh approved on 2025-02-26 17:59_

---

_@zanieb reviewed on 2025-02-26 18:02_

---

_Review comment by @zanieb on `docs/concepts/projects/sync.md`:60 on 2025-02-26 18:02_

And `uv sync` and `uv tree` etc.? I don't think I want to specify that here

---

_Merged by @zanieb on 2025-02-26 18:20_

---

_Closed by @zanieb on 2025-02-26 18:20_

---

_Branch deleted on 2025-02-26 18:20_

---
