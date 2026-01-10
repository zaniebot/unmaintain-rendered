---
number: 13330
title: "Allow character ranges or locales in `allowed-confusables` for RUF001, RUF002, & RUF003"
type: issue
state: open
author: namurphy
labels: []
assignees: []
created_at: 2024-09-12T01:32:08Z
updated_at: 2024-09-12T19:33:40Z
url: https://github.com/astral-sh/ruff/issues/13330
synced_at: 2026-01-10T01:22:53Z
---

# Allow character ranges or locales in `allowed-confusables` for RUF001, RUF002, & RUF003

---

_Issue opened by @namurphy on 2024-09-12 01:32_

Rules [RUF001](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-string/), [RUF002](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-docstring/), and [RUF003](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-comment/) help flag potentially ambiguous Unicode characters. While the configuration variable `allowed-confusables` can be used to specify allowed Unicode characters, it can become cumbersome to define `allowed-confusables` for projects whose docstrings use a lot of mathematical notation, or projects that are written in languages that use (for example) the Cyrillic alphabet.  

It would be quite convenient if `allowed-confusables` could contain character ranges. Some possible use cases are:

```toml
allowed-confusables = ["α-ω"]  # lower case Greek letters for use in equations
allowed-confusables = ["∀-⋿"]  # Mathematical operators Unicode block
allowed-confusables = ["Ѐ-ӿ"]  # Cyrillic alphabet
```

Allowing character ranges would make it easier to configure exceptions to these rules.  It'd also be less likely that we'd forget to include certain Unicode characters.  

A related possibility would be to allow regular expressions, since there may be some use cases where a certain character should be allowed but only in a specific context.

Many thanks!

---

_Referenced in [PlasmaPy/PlasmaPy#2854](../../PlasmaPy/PlasmaPy/pulls/2854.md) on 2024-09-12 01:45_

---

_Comment by @MichaReiser on 2024-09-12 18:42_

That does sound annoying. I just looked through VS code's confusable code and it seems to have some concept for locale confusables. That's why I'm not sure if allowing character ranges is the right solution. Maybe a better approach is to allow configuring the locale(s) and if mathematical operators should be allowed. 

---

_Renamed from "Allow character ranges in `allowed-confusables` for RUF001, RUF002, & RUF003" to "Allow character ranges or locales in `allowed-confusables` for RUF001, RUF002, & RUF003" by @namurphy on 2024-09-12 18:51_

---

_Comment by @namurphy on 2024-09-12 19:32_

> That does sound annoying. I just looked through VS code's confusable code and it seems to have some concept for locale confusables. That's why I'm not sure if allowing character ranges is the right solution. Maybe a better approach is to allow configuring the locale(s) and if mathematical operators should be allowed.

Very interesting! I hadn't thought of that!  Defining locales (and being able to specify more than one) would make it even easier to configure allowed confusables.  I updated the title to include that possibility. 

---

_Referenced in [astral-sh/ruff#14403](../../astral-sh/ruff/pulls/14403.md) on 2024-11-18 08:47_

---

_Referenced in [astral-sh/ruff#15056](../../astral-sh/ruff/issues/15056.md) on 2024-12-23 08:26_

---
