```yaml
number: 2050
title: How to configure per-module ignores?
type: issue
state: closed
author: AlecThomson
labels:
  - question
assignees: []
created_at: 2025-12-18T02:24:03Z
updated_at: 2025-12-19T07:25:30Z
url: https://github.com/astral-sh/ty/issues/2050
synced_at: 2026-01-12T15:54:26Z
```

# How to configure per-module ignores?

---

_@AlecThomson_

### Question

Hi there,

Thanks for the huge amount of effort on this, and congrats on the v0.2 release!

I'm wanting to configure `ty` to ignore issues for a specific module. For example, I have a `mypy` configuration like:

```
[[tool.mypy.overrides]]
module = "astropy.*"
```

In truth, I'd rather be able to ignore a _specific_ error on a per-module basis. For example, here I'd like to ignore just `unresolved-attribute` errors from `astropy.units`. Is it possible to configure this, currently? Thanks!

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `question` added by @AlecThomson on 2025-12-18 02:24_

---

_Comment by @MichaReiser on 2025-12-18 07:16_

You can use `tool.ty.overrides` to change the severity (enable or disable rules) for a subset of files:

```toml
[[tool.ty.overrides]]
include = ["tests/**", "**/test_*.py"]

[tool.ty.overrides.rules]
possibly-unresolved-reference = "warn"
```

Unlike mypy, it isn't module-based, but it should allow you to accomplish the same.

Unless you want to disable attribute errors anywhere in the code base if accessed on an object with a type from any `astrolpy.*` module.

https://docs.astral.sh/ty/reference/configuration/#overrides

---

_Comment by @AlecThomson on 2025-12-18 12:32_

Thanks @MichaReiser.

> Unless you want to disable attribute errors anywhere in the code base if accessed on an object with a type from any astrolpy.* module.

That's indeed what I'd be after. I don't want to disable the error globally, rather just for external modules that cause issues.

---

_Comment by @MichaReiser on 2025-12-18 12:34_

Great. I'll close this issue then. But let us know if you run into any problems.

---

_Closed by @MichaReiser on 2025-12-18 12:34_

---

_Comment by @AlecThomson on 2025-12-18 12:38_

Oh sorry, just to check. Is it just the case that its not possible at the moment to do per-module ignores?

---

_Reopened by @MichaReiser on 2025-12-18 12:43_

---

_Comment by @MichaReiser on 2025-12-18 12:43_

> Oh sorry, just to check. Is it just the case that its not possible at the moment to do per-module ignores?

Can you say more about what you mean by per-module ignores?

---

_Comment by @carljm on 2025-12-19 00:54_

> Unless you want to disable attribute errors anywhere in the code base if accessed on an object with a type from any `astrolpy.*` module.

Is this really a feature mypy offers? I'm not clear, because the config in the OP doesn't seem to be complete? It shows that you're overriding something for a certain set of modules, but it's not clear what config options you are setting for those modules.

This almost sounds like a use case for #2082 ?

---

_Comment by @AlecThomson on 2025-12-19 02:12_

Sorry for any confusion. Indeed, the mypy configuration above simply ignores _all_ errors from the specified module. I was curious to know if it was possible to configure something similar in `ty`.

My follow up question, which perhaps confused matters, was that it would potentially be preferable to have more granularity on a per-issue + per module basis. As far as I know this isn't possible MyPy, however.

In that sense, I think my question is a duplicate of #1354

---

_Comment by @MichaReiser on 2025-12-19 07:25_

I'll close this as I considere it covered by #2082 and #1354

---

_Closed by @MichaReiser on 2025-12-19 07:25_

---
