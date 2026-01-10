---
number: 15742
title: Nesting groups within optional-dependencies
type: issue
state: closed
author: ghostiek
labels:
  - question
assignees: []
created_at: 2025-09-08T20:38:50Z
updated_at: 2025-09-09T19:55:17Z
url: https://github.com/astral-sh/uv/issues/15742
synced_at: 2026-01-10T01:25:59Z
---

# Nesting groups within optional-dependencies

---

_Issue opened by @ghostiek on 2025-09-08 20:38_

### Question

Is there a way to achieve something like this?
```
[project.optional-dependencies]
optdep= [ ... ]

[dependency-groups]
dev = [ {include-group = "optdep"}, more-dependencies ]
```

I understand that optdep is not a dependency-group but to avoid repeated code I'd like to include all the packages from my optional dependencies in my dev group

---

_Label `question` added by @ghostiek on 2025-09-08 20:38_

---

_Comment by @notatallshaw on 2025-09-08 20:56_

It's not _officially_ part of the spec, but assuming you know the name of the package you can do it, there is an example here: https://github.com/pypa/pip/issues/11296

---

_Comment by @ghostiek on 2025-09-08 21:05_

The only issue is I'm not trying to publish whatever the dev group has, which is why I want it to be separate from the optional-dependencies metadata

---

_Comment by @zanieb on 2025-09-08 21:21_

Yeah this is the recommend way to do that right now:

```
[project]
name = "foo"

[project.optional-dependencies]
optdep= [ ... ]

[dependency-groups]
dev = [ "foo[optdep]", more-dependencies ]
```

---

_Comment by @ghostiek on 2025-09-08 21:47_

I see that makes sense, are there plans to maybe allow using {include-optional = "..."} into the pyproject.toml metadata?

---

_Comment by @notatallshaw on 2025-09-08 22:00_

> are there plans to maybe allow using {include-optional = "..."} into the pyproject.toml metadata?

There are no current plans, such changes to the Packaging spec are proposed and discussed here: https://discuss.python.org/c/packaging/14

---

_Closed by @ghostiek on 2025-09-09 19:55_

---
