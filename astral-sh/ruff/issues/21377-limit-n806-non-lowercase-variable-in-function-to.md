```yaml
number: 21377
title: Limit N806 (non-lowercase-variable-in-function) to fix first letter only?
type: issue
state: closed
author: carstenfuchs
labels:
  - question
assignees: []
created_at: 2025-11-11T10:19:24Z
updated_at: 2025-12-10T17:26:58Z
url: https://github.com/astral-sh/ruff/issues/21377
synced_at: 2026-01-12T15:54:57Z
```

# Limit N806 (non-lowercase-variable-in-function) to fix first letter only?

---

_@carstenfuchs_

### Question

Hello,

I have an old codebase that uses a lot of uppercase variable and function names like:
```python
SomeVariable = 3
def GetANumber():
    return 7
```
Rule N806 converts names to lowercase _and_ turns `camelCase` to `snake_case`.
But that’s a bit too much for us right now.

Is there a way to configure N806 to only lowercase the first letter?
In other words, I’d like to end up with:
```python
someVariable = 3
def getANumber():
    return 7
```


### Version

ruff 0.14.3

---

_Label `question` added by @carstenfuchs on 2025-11-11 10:19_

---

_Comment by @ntBre on 2025-11-13 22:16_

I don't believe there's an existing configuration option for that since N806 tries to enforce the snake-case part of PEP 8 too.

---

_Comment by @carstenfuchs on 2025-11-14 09:47_

Thanks for your reply!

- Is there any alternative?
- Would it have good prospects as a feature request?

---

_Comment by @ntBre on 2025-11-18 20:26_

No problem, thanks for opening an issue!

It seems kind of counter to the spirit of the rule to me, but I'm curious what others think.

---

_Comment by @carstenfuchs on 2025-11-19 07:25_

Great, thanks!
Just to summarize my use case:

- We inherited a large codebase with many variable and function names that don’t conform to either `camelCase` or `snake_case`.
- There are far too many to clean up in one big step.
- The code mixes English and German terms. But in German, all nouns are capitalized, so in many cases `berechneSeitenbreite()` feels more natural (and, in our context, more consistent) than `berechne_seitenbreite()`.
- It would be helpful if we could migrate step-by-step: first normalize names from `BerechneSeitenbreite()` to `berechneSeitenbreite()`, and then – more slowly and deliberately – convert to `berechne_seitenbreite()` or eventually to  `compute_page_width()`.


---

_Comment by @MichaReiser on 2025-11-19 12:22_

I agree with @ntBre that this change isn't in the spirit of the rule and I personally would go right from `berechneSeitenbreite` to `compute_path_with` (or `berechne_seiten_breite`), to avoid unnecessary churn because of renames. 

I think your best solution here is to use ruff's `--add-noqa=<REASON>` feature to suppress all existing `N806` by adding an inline noqa comment. You can then search for `<REASON>` to find the existing violations or fix the violations if you're changing a function with a suppressed N806. 


---

_Closed by @MichaReiser on 2025-12-10 17:26_

---
