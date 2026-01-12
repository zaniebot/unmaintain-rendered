```yaml
number: 2418
title: "Widen ty_extensions's Python compatibility"
type: issue
state: closed
author: bartfeenstra
labels:
  - question
assignees: []
created_at: 2026-01-09T15:13:43Z
updated_at: 2026-01-09T15:26:35Z
url: https://github.com/astral-sh/ty/issues/2418
synced_at: 2026-01-12T15:54:26Z
```

# Widen ty_extensions's Python compatibility

---

_@bartfeenstra_

### Question

ty_extensions currently requires Python 3.13 or newer. Would it be possible to support older versions as well?

I'm looking to adopt ty for an existing project that supports 3.11+ and it would be great if ty_extensions could support that, but **any** additional version support would be most welcome.

### Version

_No response_

---

_Label `question` added by @bartfeenstra on 2026-01-09 15:13_

---

_Comment by @AlexWaygood on 2026-01-09 15:15_

Can you say a bit more about what specifically is going wrong if you try to use `ty_extensions` on a project that supports Python 3.11?

---

_Comment by @bartfeenstra on 2026-01-09 15:19_

It won't install on anything less than Python 3.13. Pip is being its usual unhelpful self by complaining about a bunch of other modules, but if I remove ty_extensions from the dependencies everything installs just fine on 3.12 and below. These observations appear consistent with the ty_extensions package metadata stating 3.13 is a minimum requirement.

---

_Comment by @bartfeenstra on 2026-01-09 15:20_

Ah, in my 3.12 environment pip gives a more helpful message: `ERROR: Could not find a version that satisfies the requirement ty_extensions~=0.1.0; extra == "test" (from betty[test]) (from versions: none)`

---

_Comment by @AlexWaygood on 2026-01-09 15:22_

We don't publish ty_extensions to PyPI at all right now, I'm afraid! You have to import it in an `if TYPE_CHECKING` block if you want to use it in a non-stub file in your project.

Uploading an installable runtime shim to PyPI is tracked in https://github.com/astral-sh/ty/issues/2084. If we did that, we would make sure it's installable on all versions of Python ty supports

---

_Closed by @AlexWaygood on 2026-01-09 15:22_

---

_Comment by @bartfeenstra on 2026-01-09 15:26_

I now see that https://pypi.org/project/ty-extensions/has nothing to do with the current state of things at all :) @AlexWaygood Thanks!

@charliermarsh Could an update to that PyPI package page avoid others from trying to use the package?

---
