```yaml
number: 10569
title: Make E741 not apply to stub files
type: issue
state: closed
author: AlexWaygood
labels:
  - rule
  - linter
assignees: []
created_at: 2024-03-25T12:49:36Z
updated_at: 2024-11-17T02:54:36Z
url: https://github.com/astral-sh/ruff/issues/10569
synced_at: 2026-01-10T11:09:52Z
```

# Make E741 not apply to stub files

---

_Issue opened by @AlexWaygood on 2024-03-25 12:49_

I think it would make sense to have E741 ([`ambiguous-variable-name`](https://docs.astral.sh/ruff/rules/ambiguous-variable-name/)) not apply to stub files.

Unlike with runtime `.py` files, stub authors generally have no control over the names of variables that appear in a stub. A well-written stub should aim to faithfully represent the interface of the equivalent `.py` file as it exists at runtime, and that includes any ambiguously named variables in the runtime module. Even if the source code for the stubs is located in the same repository as the runtime module being stubbed, you likely won't be able to change an ambiguously named variable in the stub without also changing the corresponding runtime module, and tools like [stubtest](https://mypy.readthedocs.io/en/stable/stubtest.html) will generally help stub authors ensure consistency between the runtime module and the stub.

Typeshed has [always ignored this rule for `.pyi` files in our ruff config](https://github.com/python/typeshed/blob/db8e620e3db6eee13f70fbb6cfc90a851e4260c1/pyproject.toml#L92) (and before that, in our `.flake8` config), and I can't think of a situation in which it would be useful when applied to stubs.

Would we need some kind of deprecation period for this change? Or could we just make it?

(F403 and F405 (regarding `*` imports) similarly flag things that are out of the control of stub authors. But these rules highlight places where ruff's inference capabilities will be severely limited due to the specific idioms being used, so I think we should probably keep these enabled by default, even for `.pyi` files.)

---

_Label `rule` added by @AlexWaygood on 2024-03-25 12:49_

---

_Label `linter` added by @AlexWaygood on 2024-03-25 12:49_

---

_Comment by @charliermarsh on 2024-05-03 00:58_

I think this makes sense! We could ship it as part of v0.5.0 if you want?

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-06-01 23:26_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-27 11:40_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-27 11:40_

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-12 10:46_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 10:46_

---

_Comment by @AlexWaygood on 2024-08-27 14:02_

We've now made this change in preview mode; we'll be stabilising it in Ruff 0.7. Thanks @calumy!

---

_Removed from milestone `v0.7` by @AlexWaygood on 2024-10-17 15:18_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-17 15:18_

---

_Closed by @dylwil3 on 2024-11-17 02:54_

---
