---
number: 19537
title: "Feature request: Allow opting into selected format preview styles"
type: issue
state: closed
author: codethief
labels:
  - question
  - formatter
assignees: []
created_at: 2025-07-24T19:01:49Z
updated_at: 2025-07-25T09:52:24Z
url: https://github.com/astral-sh/ruff/issues/19537
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature request: Allow opting into selected format preview styles

---

_Issue opened by @codethief on 2025-07-24 19:01_

Right now, `tool.ruff.format.preview = true` [opts into all preview styles and preview styles can change even in patch releases](https://github.com/astral-sh/ruff/discussions/13013#discussioncomment-10403574).

In my case, though, I am only interested in the [`hug_parens_with_braces_and_square_brackets` preview style](https://github.com/astral-sh/ruff/issues/8279), though, which seems relatively stable, [has also been implemented](https://github.com/psf/black/pull/3964) by [black](https://github.com/psf/black), and hasn't seen a lot of controversy since [the first discussions back in 2020](https://github.com/psf/black/issues/1811).

In other words: That preview style on its own seems relatively safe to use in production code while as an average user I don't know much about the whole set of styles enabled by `tool.ruff.format.preview = true`, so enabling all of them seems much more risky.

As far as I can tell from looking at [`preview.rs`](https://github.com/astral-sh/ruff/blob/f9091ea8bb5aea454c3c6e8291470d453755c85f/crates/ruff_python_formatter/src/preview.rs), implementing the feature should be relatively straight-forward and only involve changing a few lines(?) (+ config parsing + documentation, of course).




---

_Label `question` added by @MichaReiser on 2025-07-25 06:46_

---

_Label `formatter` added by @MichaReiser on 2025-07-25 06:46_

---

_Comment by @MichaReiser on 2025-07-25 06:48_

We intentionally don't expose preview specific feature flags because:

* We want to get feedback on fall preview features
* We want preview to be a lightweight process on our end (there should be little to no reason for us not to use it). This is less of a problem for the formatter but I want to avoid having settings for each preview feature in the linter.
* But most importantly, adding a dedicated setting makes the feature appear more stable than they are and it leaves us in an awkward position when stabilizing the feature because users will then start asking for the opposite: Just leave the setting so that I can disable this formatter behavior.

I do understand that opting in to all preview features can feel uncomfortable and not being able to use it can be frustrating (we have the same with some rust format features). But preview features are intentionally under preview and we don't want users opting into them lightly because they don't have the same stability guarantees as other features (and might be removed any day)

---

_Closed by @MichaReiser on 2025-07-25 06:48_

---

_Comment by @codethief on 2025-07-25 09:52_

Fair enough, thank you for considering it anyway!

---
