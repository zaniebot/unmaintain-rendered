```yaml
number: 308
title: "Consider not displaying project name if multiple `pyproject.toml` files are given"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-11-03T15:23:23Z
updated_at: 2025-12-02T09:49:27Z
url: https://github.com/astral-sh/uv/issues/308
synced_at: 2026-01-12T15:58:22Z
```

# Consider not displaying project name if multiple `pyproject.toml` files are given

---

_@zanieb_

If multiple projects have names, we will use the first one we see. However, this could result in ambiguous error messages for `pip-compile`. The alternative is to just say `root`.

_Originally posted by @charliermarsh in https://github.com/astral-sh/puffin/pull/295#discussion_r1380917499_
            

---

_Comment by @charliermarsh on 2023-11-03 15:41_

For issues like this, I'm kind of partial to just deciding and fixing (or closing) rather than tracking, since it feels unlikely we'll want to return to it.

---

_Comment by @zanieb on 2023-11-03 15:45_

I don't think we'll really know what the right behavior here is without a real use-case of multiple pyproject.toml files. I think of maybe like... dagster? Perhaps #311 is a better solution in general.

---

_Comment by @zanieb on 2025-12-02 09:49_

I think right now we won't do this?

```
❯ uv init foo
Initialized project `foo` at `/Users/zb/workspace/uv/foo`
❯ uv add --project foo anyio==4.11.0
❯ uv init bar
❯ uv add --project bar anyio==4.12.0
❯ uv pip compile foo/pyproject.toml bar/pyproject.toml
  × No solution found when resolving dependencies:
  ╰─▶ Because you require anyio==4.11.0 and anyio==4.12.0, we can conclude that your requirements are unsatisfiable.
```

---

_Closed by @zanieb on 2025-12-02 09:49_

---
