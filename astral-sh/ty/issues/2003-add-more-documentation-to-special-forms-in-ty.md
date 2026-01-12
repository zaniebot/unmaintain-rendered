```yaml
number: 2003
title: "Add more documentation to special forms in `ty_extensions.pyi` stub"
type: issue
state: open
author: AlexWaygood
labels: []
assignees: []
created_at: 2025-12-17T12:25:40Z
updated_at: 2025-12-17T19:56:03Z
url: https://github.com/astral-sh/ty/issues/2003
synced_at: 2026-01-12T15:54:26Z
```

# Add more documentation to special forms in `ty_extensions.pyi` stub

---

_@AlexWaygood_

It's easy for users to jump to these definitions in the stub file from inlay hints. It will be even easier to jump to these definitions if something like https://github.com/astral-sh/ty/issues/1575 is implemented.

We should add more docstrings/comments to the stub file so that users have a better experience when they land there. Ideally we would also show this documentation on hover, so that users can easily learn about what the `Unknown` type means and represents, for example.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 12:25_

---

_Comment by @Gankra on 2025-12-17 14:53_

(Also having those docs would let us show those docs whenever you hover a special form, which would be incredible)

---

_Comment by @sharkdp on 2025-12-17 15:01_

> (Also having those docs would let us show those docs whenever you hover a special form, which would be incredible)

Not directly, actually! I tried to implement this quickly before, but [since `Unknown` is not *declared*](https://github.com/astral-sh/ruff/blob/0bd7a94c2732c232cc1423756e2437d9bce558b1/crates/ty_vendored/ty_extensions/ty_extensions.pyi#L20), we show this:

<img width="785" height="265" alt="Image" src="https://github.com/user-attachments/assets/28c4ffd7-d2e0-4947-9076-558441f62fa5" />

---

_Comment by @AlexWaygood on 2025-12-17 15:07_

Yes I think three things are required for good on-hover with `Unknown`:
- Change the `Unknown` definition to `Unknown: _SpecialForm`. `Unknown = object()` is just really confusing because users get the docs for `object`
- Add an "attribute docstring" to `Unknown`, e.g.

  ```py
  Unknown: _SpecialForm
  """Some docs for `Unknown`"""
  ```

  (The PEP for these was [rejected](https://peps.python.org/pep-0224/), but they're nonetheless very popular in the community and recognised by the Sphinx documentation tool)
- Add support for showing attribute-docstrings on-hover to the LSP.

I think each of these stages on its own would be a big UX improvement, so we can do each of them in isolation

---

_Comment by @Gankra on 2025-12-17 19:55_

Item 3 is here, so y'all can go nuts with docs now:

* https://github.com/astral-sh/ruff/pull/22036

---
