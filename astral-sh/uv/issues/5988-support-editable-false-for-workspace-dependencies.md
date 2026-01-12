```yaml
number: 5988
title: "Support `editable: false` for workspace dependencies"
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-08-10T00:28:16Z
updated_at: 2025-06-06T03:59:34Z
url: https://github.com/astral-sh/uv/issues/5988
synced_at: 2026-01-12T15:59:00Z
```

# Support `editable: false` for workspace dependencies

---

_@charliermarsh_

See: #5985.

---

_Label `enhancement` added by @charliermarsh on 2024-08-10 00:28_

---

_Label `preview` added by @charliermarsh on 2024-08-10 00:28_

---

_Comment by @charliermarsh on 2024-08-10 12:01_

I'm wondering if this should be a property of the workspace member itself rather than a property of the _dependency_?

---

_Comment by @charliermarsh on 2024-08-10 16:11_

The more I think about this, the less I like it. Even if we support `editable: false`, the _requiring_ package will still be installed as editable. I think editable should either be (1) a property of the package (not the dependency), or better (2) a global setting.

---

_Comment by @zanieb on 2024-08-10 16:31_

I don't think we need to make a decision on this for the upcoming stabilizations, but don't have strong feelings about how it works. Curious to hear from @konstin and community members trying out workspaces.

---

_Comment by @charliermarsh on 2024-08-10 16:33_

Agree. Just hard for me to imagine a use-case where `root` depends on `child`, and you want `child` installed as non-editable but `root` as editable? And then there's _no_ way to install `root` as non-editable?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @LucidDan on 2025-01-10 03:24_

I have one scenario I ran into that led me to this issue.

If I'm building a mono-repo using uv workspaces, and I have a type stubs package that I want to have in my repo as a workspace that is used by several projects...mypy doesn't work properly with editable dependencies (see https://github.com/python/mypy/issues/12313, apparently there is no plan to change this behaviour)

In that case, I need the type stubs package installed non-editable, even when the rest of the workspaces are editable.
I work around this by defining the type stub as a path dependency instead of a workspace source, eg:

```
[tool.uv.sources]
demo = { workspace = true }
lh = { workspace = true }
my-typestubs = { path = "packages/my-typestubs", editable = false }
```

This is a reasonable workaround - it just means I end up with a separate lock and .venv inside the type stubs package, and its slightly slower than if I could keep it as a workspace.

I can't really judge if this is something many people would want to do, but for me at least, it's a valid use case.


---

_Comment by @zanieb on 2025-01-10 04:44_

Thanks for weighing in! cc @AlexWaygood 

---

_Comment by @xmakro on 2025-06-06 03:59_

We are also running into this use case in a large monorepo.

```
[tool.uv.sources]
demo = { workspace = true, editable = false }
```

would be ideal

---
