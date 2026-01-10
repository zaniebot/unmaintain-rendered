---
number: 17401
title: "✨FR: quote-style = \"smart\""
type: issue
state: closed
author: dotysan
labels:
  - configuration
  - formatter
assignees: []
created_at: 2025-04-14T23:51:45Z
updated_at: 2025-04-22T16:46:21Z
url: https://github.com/astral-sh/ruff/issues/17401
synced_at: 2026-01-10T01:22:58Z
---

# ✨FR: quote-style = "smart"

---

_Issue opened by @dotysan on 2025-04-14 23:51_

### Summary

Wonder if others would like to have ruff enforce this Perl-esque formatting?

 - `'normal string'` <= un-expanding strings are still single-quoted
 - `f"formatted {foo}"` <= double-quotes here!
 - `"don't"` and other exceptions still use double-quotes for readability

Instead of blindly forcing all strings one way or the other ("double" | "single"). Or blindly accepting my possibly poorly written code ("preserve"). I propose a feature `quote-style = "smart"` (or some other name) that enforces single-quotes when it can, but always uses double-quotes for f-strings.

---

_Label `configuration` added by @AlexWaygood on 2025-04-15 07:19_

---

_Label `formatter` added by @AlexWaygood on 2025-04-15 07:19_

---

_Label `configuration` removed by @MichaReiser on 2025-04-21 10:23_

---

_Label `style` added by @MichaReiser on 2025-04-21 10:23_

---

_Label `style` removed by @MichaReiser on 2025-04-21 10:27_

---

_Label `configuration` added by @MichaReiser on 2025-04-21 10:27_

---

_Comment by @MichaReiser on 2025-04-22 09:07_

Ruff already gives you the "smart" behavior in that it picks the opposite quote when it helps to avoid escape sequences (`"don't"` instead of `'don\'t'` when using `quote-style="single"`). The feature that's missing for what you describe is a more flexible quote configuration that allows specifying the preferred quote style for:

* normal strings
* f-strings
* (in the future) template strings


Unfortunately, that won't be enough for some use cases. We would also have to support:

* docstrings
* all of the above when using a triple quoted strings

We intentionally decided against such a flexible configuration because it makes configuring the most common usages harder and the community has aligned around using either single quotes or double quotes. You can read more about our decision on the quote-style setting in https://github.com/astral-sh/ruff/issues/7615

The proposed settings would also require extending the `flake8-quotes` setting to be at least as flexible.

Because of the complexity it introduces for everyone, I don't see us supporting this unless there's wider demand for it.

---

_Closed by @MichaReiser on 2025-04-22 09:07_

---

_Comment by @dotysan on 2025-04-22 16:46_

Thank you for the careful consideration and explanation!

---
