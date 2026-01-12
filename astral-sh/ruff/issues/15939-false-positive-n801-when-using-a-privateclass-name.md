```yaml
number: 15939
title: False positive(?) N801 when using a __PrivateClass name
type: issue
state: closed
author: Daraan
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-02-04T15:05:57Z
updated_at: 2025-02-06T08:38:50Z
url: https://github.com/astral-sh/ruff/issues/15939
synced_at: 2026-01-12T15:54:55Z
```

# False positive(?) N801 when using a __PrivateClass name

---

_@Daraan_

### Description

[N801](https://docs.astral.sh/ruff/rules/invalid-class-name/) complains that `class __PrivateClass: ...` should follow CapWords. In my opinion that is the case, but the dunder prefix triggers this rule.

[playground](https://play.ruff.rs/1fd9ed9e-a28e-4c68-a8c5-3371c1a2e368)

---

_Comment by @MichaReiser on 2025-02-04 15:18_

Thanks for reporting. I think this is a deviation on our end. I'm not sure if it's intentional, but the upstream rule uses [`str.strip('_')`](https://github.com/PyCQA/pep8-naming/blob/f0edf20bd88cbd0950240ebeeea4fe6f4d90a094/src/pep8ext_naming.py#L282C21-L283) where we use [`str::strip_prefix`](https://github.com/astral-sh/ruff/blob/14ba469fc099e0678859bba3ae6f67477d8934ec/crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs#L57) which only returns at most one occurrence (and only the first).

This should be easy to change.

---

_Label `bug` added by @MichaReiser on 2025-02-04 15:18_

---

_Label `good first issue` added by @MichaReiser on 2025-02-04 15:18_

---

_Label `help wanted` added by @AlexWaygood on 2025-02-04 18:17_

---

_Comment by @VascoSch92 on 2025-02-05 20:37_

Can I give a try? Or you want to fix it @Daraan  ?

---

_Comment by @Daraan on 2025-02-05 20:54_

@VascoSch92 go for it :)

---

_Assigned to @VascoSch92 by @ntBre on 2025-02-05 21:20_

---

_Comment by @dhruvmanila on 2025-02-06 08:38_

Closed by: #15988

---

_Closed by @dhruvmanila on 2025-02-06 08:38_

---
