```yaml
number: 2462
title: Clarify beta status of ty
type: issue
state: open
author: gpapia
labels:
  - documentation
assignees: []
created_at: 2026-01-12T12:16:43Z
updated_at: 2026-01-13T00:13:52Z
url: https://github.com/astral-sh/ty/issues/2462
synced_at: 2026-01-13T00:22:28Z
```

# Clarify beta status of ty

---

_@gpapia_

Hi,

It’s currently a bit unclear whether ty should be considered a beta release.

The [latest blog post](https://astral.sh/blog/ty) announcing ty explicitly refers to it as a beta:

> Today, we're announcing the Beta release of [ty](https://github.com/astral-sh/ty).

Similarly, the `pyproject.toml` includes a [beta classifier](https://github.com/astral-sh/ty/blob/6b5938a5cbff201f1a6bebad991df9095ee76cbf/pyproject.toml#L11).

However, the published version number (currently 0.0.11) does not include a beta pre-release segment as defined by [PEP 440](https://peps.python.org/pep-0440/#pre-releases), and the `README.md` does not mention a beta status.

It would be helpful to clarify the intended release status of ty and ensure it is communicated consistently—either by clearly marking it as beta everywhere, or by removing the beta designation entirely.

Thanks!

---

_Label `documentation` added by @AlexWaygood on 2026-01-12 12:19_

---

_Comment by @carljm on 2026-01-13 00:12_

We could add a notice about beta status in the README.

The version number is intentional -- using a "pre-release" version number causes too many difficulties in practice for users wanting to try ty, and the usual reasons for preferring a pre-release version number do not apply when there has never yet been any "stable" release that users might want to prefer. We instead use the initial `0.0` to indicate that ty has not yet reached stable status. We won't switch to using a beta pre-release segment as defined by PEP 440, but this does not mean that the ty project is not still in beta.

I guess the notice in the README could say something about the version number.

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-13 00:13_

---
