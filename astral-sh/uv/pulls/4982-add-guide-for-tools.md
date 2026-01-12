```yaml
number: 4982
title: Add guide for tools
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-tools
created_at: 2024-07-10T22:55:29Z
updated_at: 2024-07-15T22:04:19Z
url: https://github.com/astral-sh/uv/pull/4982
synced_at: 2026-01-12T16:06:34Z
```

# Add guide for tools

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-07-10 22:55_

---

_Label `preview` added by @zanieb on 2024-07-10 22:55_

---

_Comment by @zanieb on 2024-07-10 22:56_

I need to figure out where I'm going to draw the line between these guides and the concept documentation pages.

---

_Marked ready for review by @zanieb on 2024-07-11 14:51_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-11 14:51_

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:106 on 2024-07-11 20:36_

"in your `PATH`" instead of "on the `PATH`", maybe?

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:116 on 2024-07-11 20:37_

```suggestion
For example, the following will install the `http`, `https`, and `httpie` executables:
```

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:130 on 2024-07-11 20:38_

What's the benefit of this?

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:7 on 2024-07-11 20:38_

```suggestion
The `uvx` command is an alias for `uv tool run`, which can be used to invoke a tool without installing it.
```

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:41 on 2024-07-11 20:40_

```suggestion
The `--from` option can be used to invoke a command from a specific package, e.g. `http` which is provided by `httpie`:
```

---

_Review comment by @ibraheemdev on `docs/guides/tools.md`:91 on 2024-07-11 20:40_

```suggestion
- The `--with` flag is not needed — the required package is inferred from the command name.
```

---

_@ibraheemdev approved on 2024-07-11 20:41_

Looks good, just some small nits.

---

_@zanieb reviewed on 2024-07-11 21:17_

---

_Review comment by @zanieb on `docs/guides/tools.md`:130 on 2024-07-11 21:17_

Of explaining it? `--from` isn't supposed to be used in `uv tool install`.

---

_Comment by @T-256 on 2024-07-12 16:13_

Could you add some clarity about difference between `uvx --from` and `uvx --with`?

I think they are in common in all ways except of `--from` is one-time usable and it declares entry points of which package should be gathered.

I can suggest to merge `--with` and `--from` into one argument (e.g. `--from`). By keeping it multiple-times usable it in cli, by default, main package should be inferred from first/last passed `--from` argument. Which also could be overridden by separate argument e.g. `--main`.
Examples:
```shell
# inferred first `--from` as main package, so `fastapi` executable would be available.
$ uvx --from fastapi-cli --from fastapi fastapi dev
```

---

_@zanieb reviewed on 2024-07-15 21:59_

---

_Review comment by @zanieb on `docs/guides/tools.md`:106 on 2024-07-15 21:59_

Trying to avoid "you" / "your" so far

---

_Merged by @zanieb on 2024-07-15 22:04_

---

_Closed by @zanieb on 2024-07-15 22:04_

---

_Branch deleted on 2024-07-15 22:04_

---
