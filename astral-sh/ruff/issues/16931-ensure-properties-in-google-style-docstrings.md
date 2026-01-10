```yaml
number: 16931
title: Ensure properties in Google-style docstrings start the correct way
type: issue
state: open
author: gtkacz
labels:
  - rule
assignees: []
created_at: 2025-03-23T23:45:07Z
updated_at: 2025-05-07T06:39:00Z
url: https://github.com/astral-sh/ruff/issues/16931
synced_at: 2026-01-10T11:09:58Z
```

# Ensure properties in Google-style docstrings start the correct way

---

_Issue opened by @gtkacz on 2025-03-23 23:45_

### Summary

From the official [Google Python style guide on docstrings](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods):
> The docstring for a `@property` data descriptor should use the same style as the docstring for an attribute or a function argument ("""The Bigtable path.""", rather than """Returns the Bigtable path.""").

I propose that a new rule be created:
1. `google-docstring-property`: Ensure that in every `@property` Google-style docstring, the summary doesn't start with keywords like "Return", "Returns", "Get",  "Gets", etc..

This should only be enforced if the `google` convention is active.

If this already exists, I'm sorry, I couldn't find it.

---

_Label `rule` added by @ntBre on 2025-03-24 14:22_

---

_Comment by @MichaReiser on 2025-03-27 20:19_

I thought we had a rule that enforces that a docstring doesn't start with `"Returns"` or similar but I wasn't able to find it just now. Do you know which rule enforces this? If not, then it seems to me what we're missing is such a rule, rather than it being specific around `@property`

---

_Comment by @gtkacz on 2025-03-28 15:52_

@MichaReiser I don't think there is one, here's a playground example with all rules enabled and a method docstring that starts with `Returns`: https://play.ruff.rs/8ec68614-783a-493f-9b8d-165a63bb8351

---

_Comment by @gtkacz on 2025-04-04 18:45_

@MichaReiser should I close this issue and open a new one that asks for a rule that ensures no docstrings start with one of those keywords?

---

_Comment by @MichaReiser on 2025-04-05 07:25_

@gtkacz, no, I already marked the issue as a rule request. 

---

_Comment by @saiden89 on 2025-05-06 11:22_

@MichaReiser Could it be [D401](https://docs.astral.sh/ruff/rules/non-imperative-mood)?

---

_Comment by @MichaReiser on 2025-05-07 06:38_

@saiden89 I don't think so D401 only applies to the first line and it also allows `Return` which the issue author also wants to disallow

---
