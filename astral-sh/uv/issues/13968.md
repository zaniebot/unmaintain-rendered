```yaml
number: 13968
title: "Warn when `--upgrade-package <name>` does not result in an upgrade"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-06-11T15:17:14Z
updated_at: 2025-06-11T15:34:37Z
url: https://github.com/astral-sh/uv/issues/13968
synced_at: 2026-01-10T03:32:45Z
```

# Warn when `--upgrade-package <name>` does not result in an upgrade

---

_Issue opened by @zanieb on 2025-06-11 15:17_

### Summary

We should explain why we could not upgrade a package when it was explicitly requested. As reported at https://github.com/astral-sh/uv/issues/1419#issuecomment-2961172589, we don't do this right now.

This may be a very hard problem do to the complexity of the resolver.

### Example

```
$ uv add --upgrade-package httpx
warning: cannot upgrade `httpx` because `foo` requires `httpx==0.1.0`
```

---

_Label `enhancement` added by @zanieb on 2025-06-11 15:17_

---

_Comment by @konstin on 2025-06-11 15:34_

Implementation guidance:

For the regular case, it should be possible to get this information by looking at the incompatibilities we get from pubgrub in the resolver, there should be at least one incompatibility that puts an upper bound that is below the latest version that would have been otherwise selected. This is not a full proof as our error messages are, as the source of the (transitive) incompatibility may only be a preference, not a hard constraint (that is, we could upgrade to a higher version, but we couldn't due a specific interaction between priorities and preferences - I can explain the details if required).

Another option is that there is a latest version but we didn't select it for other reasons, namely it's too high requires-python bound incompatible with the project requires-python, or, if the option is used, not having a wheel matching `tool.uv.required-environments`.

---
