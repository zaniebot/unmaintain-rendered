---
number: 13556
title: Add shorthand for --directory command option
type: issue
state: open
author: wray27
labels:
  - enhancement
assignees: []
created_at: 2025-05-20T15:33:44Z
updated_at: 2025-05-24T16:28:35Z
url: https://github.com/astral-sh/uv/issues/13556
synced_at: 2026-01-10T01:25:34Z
---

# Add shorthand for --directory command option

---

_Issue opened by @wray27 on 2025-05-20 15:33_

### Summary

Currently, working inside of a project with a monorepo setup `--directory` should have a shorthand like `-C` (similar to git/poetry)

### Example

e.g.
```
uv -C my_directory sync
```

---

_Label `enhancement` added by @wray27 on 2025-05-20 15:33_

---

_Comment by @wray27 on 2025-05-20 15:36_

I dont mind giving this one a go, as seems quite straight foward

---

_Comment by @zanieb on 2025-05-20 15:40_

I think we generally want to encourage use of `--project` over `--directory`, as its semantics around relative paths are less surprising. That said, `-C` is ~standard for `--directory` (Cargo and git?) so I'm not opposed. Do we use it for something else already?

---

_Comment by @wray27 on 2025-05-21 10:39_


> I think we generally want to encourage use of `--project` over `--directory`, as its semantics around relative paths are less surprising. That said, `-C` is ~standard for `--directory` (Cargo and git?) so I'm not opposed. Do we use it for something else already?

I think its used as a build option for `uv sync` :

```
-C, --config-setting <CONFIG_SETTING>
          Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
```

but otherwise should be fine?


---

_Comment by @charliermarsh on 2025-05-24 16:28_

I think the conflict with `-C` is kind of a problem.

---
