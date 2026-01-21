```yaml
number: 2573
title: "Support `os.name` for platform checks"
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2026-01-21T01:42:43Z
updated_at: 2026-01-21T01:42:51Z
url: https://github.com/astral-sh/ty/issues/2573
synced_at: 2026-01-21T02:01:01Z
```

# Support `os.name` for platform checks

---

_@carljm_

> I'd like to +1 the `os.name` support request. I have some code that was using `os.name == "nt"` and `os.name != "nt"` that pyright correctly handles, but ty gives unresolved-attribute errors on. 
> On finding this thread I've changed it to `sys.platform` checks instead, which works correctly.
> If its difficult to support at this time maybe adding a mention to the docs and type system feature overview would be helpful? 

 _Originally posted by @Clarkery in [#160](https://github.com/astral-sh/ty/issues/160#issuecomment-3768337942)_

---
