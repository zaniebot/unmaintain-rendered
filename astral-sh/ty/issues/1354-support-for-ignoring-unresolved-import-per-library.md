---
number: 1354
title: Support for ignoring unresolved-import per library
type: issue
state: open
author: miloszwatroba
labels:
  - configuration
  - imports
assignees: []
created_at: 2025-10-14T15:17:41Z
updated_at: 2026-01-09T09:55:42Z
url: https://github.com/astral-sh/ty/issues/1354
synced_at: 2026-01-10T01:48:23Z
---

# Support for ignoring unresolved-import per library

---

_Issue opened by @miloszwatroba on 2025-10-14 15:17_

### Question

Hi,

I'd like to ask if there are plans for supporting a feature for ignoring `unresolved-import` per library, i.e. similarly to how `mypy` does it with:

```
[[tool.mypy.overrides]]
module = ["foobar.*"]
ignore_missing_imports = true 
```

The behaviour I'd expect is that all unresolved imports of `foobar` are silenced (e.g. `from foobar import X`).

I have already checked the documentation and haven't found anything similar in `ty`. 

Thanks,
Milosz

### Version

_No response_

---

_Label `question` added by @miloszwatroba on 2025-10-14 15:17_

---

_Renamed from "Support for igoring unresolved-import per package" to "Support for ignoring unresolved-import per library" by @miloszwatroba on 2025-10-14 15:19_

---

_Label `configuration` added by @AlexWaygood on 2025-10-14 15:20_

---

_Label `imports` added by @AlexWaygood on 2025-10-14 15:20_

---

_Comment by @MichaReiser on 2025-10-14 16:27_

Can you elaborate on what you expect from this configuration? Is it that all unresolved imports of `foobar` are silenced (e.g. `from foobar import X`) or that all unresolved imports within `foobar` are silenced (e.g. an `import XX` within a `foobar` module)?

---

_Comment by @miloszwatroba on 2025-10-14 16:52_

Sure! Sorry, I should have mentioned that in the beginning instead of relying on the familiarity with `mypy` :) 

The behaviour I'd expect is that all unresolved imports of `foobar` are silenced (e.g. `from foobar import X`). 

The use case/rationale behind it is e.g. an environment where we run `ty` locally in a runtime that does not include `foobar`, but the package is ultimately used in a docker container that vends `foobar` (hence lack of `foobar` outside of the docker runtime). 

---

_Comment by @MichaReiser on 2025-10-14 16:54_

Thanks, that makes sense. I was just asking because we support the latter (by using overrides). 

An allowlist for modules that are known not to exist and should be silently treated as any makes sense to me.

---

_Comment by @miloszwatroba on 2025-10-14 17:08_

I updated the description to reflect my expectation as well. 

>  I was just asking because we support the latter (by using overrides).

Yup, I found that one. 

> An allowlist for modules that are known not to exist and should be silently treated as any makes sense to me.

Is that something you considered for your roadmap? Otherwise, can we keep this one as a feature request?

---

_Comment by @MichaReiser on 2025-10-14 17:23_

> Is that something you considered for your roadmap? Otherwise, can we keep this one as a feature request?

Yes, it's something we also already discussed as a team. It's not on our immediate roadmap but it's a feature we want and this issue is great for tracking it.

---

_Label `question` removed by @MichaReiser on 2025-10-28 18:11_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:26_

---

_Comment by @MichaReiser on 2026-01-09 09:55_

I bump the priority on this one because it seems relatively easy to implement and has come up a few times.

---
