```yaml
number: 2078
title: Support suppressing errors based on message
type: issue
state: closed
author: duncanmmacleod
labels:
  - configuration
assignees: []
created_at: 2025-12-18T16:16:52Z
updated_at: 2025-12-18T18:02:25Z
url: https://github.com/astral-sh/ty/issues/2078
synced_at: 2026-01-12T15:54:26Z
```

# Support suppressing errors based on message

---

_@duncanmmacleod_

### Question

In https://github.com/astral-sh/ty/issues/2076 I reported `ty` emitting errors when a module provides dynamic attributes that cannot be inferred statically, but they _will_ be available at runtime.

To work around that, it would be very nice to be able to suppress errors based on the message of the error itself. This is orthogonal to suppressing errors based on the containing file(s), and would resolve the linked issue by being able to configure something like:

```toml
[[tool.ty.overrides]]
match = "Module `astropy.constants` has no member"

[tool.ty.overrides.rules]
unresolved-attribute = "ignore"
```

This seems cleaner (as a heavily biased user) than enumerating every file that _might_ reference the offending third-party module, or adding `# ty: ignore[unresolved-attribute]` on every instance in the code.

---

This is literally day 1 of me toying with `ty`, thanks for creating it!

### Version

_No response_

---

_Label `question` added by @duncanmmacleod on 2025-12-18 16:16_

---

_Comment by @AlexWaygood on 2025-12-18 16:19_

A very different configuration option that also could have helped with #2078 might be something like https://pyrefly.org/en/docs/configuration/#replace-imports-with-any%20option

---

_Label `configuration` added by @AlexWaygood on 2025-12-18 16:19_

---

_Label `question` removed by @AlexWaygood on 2025-12-18 16:19_

---

_Comment by @MichaReiser on 2025-12-18 17:20_

I prefer a narrower setting than allowing matching on the error message. That seems very fragile and makes every message change a breaking change.

There's already an issue for allowlisting `unresolved-import` for some modules. I'm not sure if pyrefly's option address the same or a slightly different use case.

---

_Comment by @AlexWaygood on 2025-12-18 17:26_

> There's already an issue for allowlisting `unresolved-import` for some modules. I'm not sure if pyrefly's option address the same or a slightly different use case.

Pyrefly's option not only allowlists `unresolved-import` for some modules. It means that even if resolving the module succeeds, rather than getting a module-literal type inferred you'd just get a symbol with type `Any`. That means that if a third-party package is really badly typed (or is really dynamic and doesn't have types), and the types coming from that third-party package therefore end up causing many type errors, there's a way of getting round all that.

---

_Comment by @carljm on 2025-12-18 18:02_

I also much prefer the "replace imports with Any" config over suppressing based on message. We can't have our diagnostic messages locked in by worries that we'll break user suppressions.

I filed https://github.com/astral-sh/ty/issues/2082 for that feature, and would be inclined to close this issue as "not planned."

Thanks @duncanmmacleod for the reports, and for trying ty!

---

_Closed by @carljm on 2025-12-18 18:02_

---
