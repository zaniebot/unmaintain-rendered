```yaml
number: 10629
title: "Feature Request : C0103/invalid-name: Allow specifying valid variable and constant patterns with regular expressions."
type: issue
state: open
author: astrowonk
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-27T15:26:34Z
updated_at: 2024-04-04T09:17:48Z
url: https://github.com/astral-sh/ruff/issues/10629
synced_at: 2026-01-12T15:54:50Z
```

# Feature Request : C0103/invalid-name: Allow specifying valid variable and constant patterns with regular expressions.

---

_@astrowonk_

This is similar to the closed #7660. As noted in that issue, Pylint lets you **define your own regex for acceptable variable and constant names* (enforcing snake_case  for example, or variable length,or both):

```
variable-rgx=[a-z_][a-z0-9_]{2,40}$
```

While `C0103` in pylint is checked off in #970, one can not define explicit regex for what constitutes an acceptable naming convention. #7660 was closed with updated documentation on ignore-names globbing, but I don't think that is similar functionality to pylint that I'm requesting. I would like parity with pylint to allow regex for variable and constant names, so users can enforce snake_case or pascalcase, etc. and then mismatches with that regex triggering `invalid-name`, like pylint does now. Thanks!


---

_Comment by @MichaReiser on 2024-03-27 20:51_

Is the main goal to enforce snake, pascal, or another predefined casing standard? Iˋm asking because Ruff canˋt offer autofixes when we allow a custom regex (and error message would probably also be somewhat cryptic).

Another reason for not allowing a regex is that ruff has other rules to enforce a certain variable lenght and we want to avoid overlapping rules

---

_Comment by @astrowonk on 2024-03-28 01:02_

> Is the main goal to enforce snake, pascal, or another predefined casing standard? Iˋm asking because Ruff canˋt offer autofixes when we allow a custom regex (and error message would probably also be somewhat cryptic).
> 
> Another reason for not allowing a regex is that ruff has other rules to enforce a certain variable lenght and we want to avoid overlapping rules

My particular use case is pretty much in the regex above; with it pylint flags non-compliant variable names: anything that isn't snake case or outside the min/max variable length. 

Obviously autofix is super useful but I think it's fine to have some rules that can't be autofixed. For this example, Pylint would say something like `invalid-name, doesn't match pattern [a-z_][a-z0-9_]{2,40}` 

---

_Comment by @MichaReiser on 2024-03-28 13:41_

Thanks for explaining. That makes sense. I suspect that the fact that the rule overlaps with other existing and proposed rules will make it challenging to have this rule accepted. I'll keep this open as an alternative to consider when deciding whether Ruff should have more variable-length restricting rules.

---

_Label `rule` added by @MichaReiser on 2024-03-28 13:42_

---

_Label `needs-design` added by @MichaReiser on 2024-03-28 13:42_

---

_Label `needs-design` removed by @MichaReiser on 2024-03-28 13:42_

---

_Label `needs-decision` added by @MichaReiser on 2024-03-28 13:43_

---

_Comment by @astrowonk on 2024-03-28 14:04_

Thanks. Sorry, I keep searching the rule list but haven't been able to find these: what are the other ruff rules that enforce variable length? 

---

_Comment by @MichaReiser on 2024-04-04 09:17_

Yeah sorry. I should have linked to those. There was a PR for [`too-short-name`](https://github.com/astral-sh/ruff/pull/5632/) that I closed. The rule overlaps with what you suggest and we may want to support the rule once #1774 is complete.

---
