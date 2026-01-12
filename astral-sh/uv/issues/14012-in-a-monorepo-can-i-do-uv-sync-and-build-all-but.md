```yaml
number: 14012
title: "In a monorepo, can I do `uv sync` and build *all but one* package?"
type: issue
state: closed
author: aw632
labels:
  - question
assignees: []
created_at: 2025-06-13T00:25:32Z
updated_at: 2025-06-17T17:29:47Z
url: https://github.com/astral-sh/uv/issues/14012
synced_at: 2026-01-12T16:01:41Z
```

# In a monorepo, can I do `uv sync` and build *all but one* package?

---

_@aw632_

### Question

I can do something like `uv sync --all-packages` to build all the packages in a monorepo, but on a certain hardware platform (`aarch64`), I want to skip the building of just one of the packages in the monorepo. I've tried to simply iterate over all the package names and then build each with `--package`, but it looks like building one package will uninstall/remove a previously built package, resulting in a fragile environment.

Is there some flag to add where I can exclude a package (or a few) from the --all-packages flag? I've also tried adding `--no-install-package <NAME>`, but it looks like `--all-packages` overrides this. 

### Platform

Linux 5.15.148-tegra aarch64 GNU/Linux

### Version

uv 0.6.3

---

_Label `question` added by @aw632 on 2025-06-13 00:25_

---

_Comment by @konstin on 2025-06-13 10:36_

We don't currently support this feature directly, but there could be a workaround: You can have a root package that depends on all workspace members (e.g. in `project.dependencies`, but `dependency-groups.dev` should also work), but for that one member it uses an `platform_machine != 'aarch64'` marker.

---

_Closed by @aw632 on 2025-06-17 17:29_

---
