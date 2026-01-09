---
number: 9006
title: "Default `isort.split-on-trailing-comma` to `format.skip-magic-trailing-comma`"
type: issue
state: open
author: sbrugman
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-12-05T09:10:26Z
updated_at: 2025-06-23T22:50:48Z
url: https://github.com/astral-sh/ruff/issues/9006
synced_at: 2026-01-07T13:12:15-06:00
---

# Default `isort.split-on-trailing-comma` to `format.skip-magic-trailing-comma`

---

_Issue opened by @sbrugman on 2023-12-05 09:10_

For our [`PyCodeHash`](https://github.com/pycodehash/pycodehash/blob/main/pyproject.toml) project we have [turned magic trailing comma off](https://github.com/psf/black/issues/2135):
```toml
[tool.ruff.isort]
split-on-trailing-comma = false

[tool.ruff.format]
skip-magic-trailing-comma = true
```

Now I see that in [this commit](https://github.com/astral-sh/ruff/commit/93258e8d5b9f681623eea6efdc154e399af0c378
) we default `max-positional-args` to `max-args`. 

What do you think about defaulting `isort.split-on-trailing-comma` to `format.skip-magic-trailing-comma` (or vice versa)?


---

_Comment by @MichaReiser on 2023-12-05 09:38_

Hy @sbrugman

I can see how keeping the two settings consistent can be annoying. 

We do inherit values in other places but so far only from top-level to a tool specific configuration and not across different tools, because we could only inherit one way (isort from format, or format from isort) or explaining the default value might get confusing. 

What we have done so far is to make tool specific options global options with tool-specific overrides (which may not even be necessary?). The downside of this is that it complicates configuration inheritance (using `extends`) and it may result in many global options. 

That's why I'm not sure what the best approach here is, but I agree that having a consistent solution for this kind of problem would improve usability.



---

_Label `configuration` added by @dhruvmanila on 2023-12-05 14:09_

---

_Label `needs-decision` added by @dhruvmanila on 2023-12-05 14:09_

---

_Referenced in [astral-sh/ruff#19299](../../astral-sh/ruff/issues/19299.md) on 2025-07-12 13:38_

---
