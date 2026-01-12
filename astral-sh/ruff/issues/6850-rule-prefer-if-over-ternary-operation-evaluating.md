```yaml
number: 6850
title: "Rule: prefer `if` over ternary operation evaluating to `None` if cond doesn't hold"
type: issue
state: open
author: dorschw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-24T11:21:54Z
updated_at: 2024-01-09T12:57:37Z
url: https://github.com/astral-sh/ruff/issues/6850
synced_at: 2026-01-12T15:54:46Z
```

# Rule: prefer `if` over ternary operation evaluating to `None` if cond doesn't hold

---

_@dorschw_

### Proposal
Disallow ternary operations that:
* if `condition` holds, call a method
* otherwise, do nothing (the whole expression evaluates to`None`)

e.g. `method(...) if condition else None`

### Why is it bad?
Can be autofixed, simplified, and be clearer for future maintainers. 

### Examples & autofix 
```diff
- print("🐍") if condition else None
+ if condition:
+     print("🐍")
```

```diff
- dictionary.update({"foo":"bar"}) if condition else None
+ if condition:
+     dictionary.update({"foo":"bar"})
```

_(the second example can be simplified to `dictionary["foo"]="bar"`, but the example is what inspired this proposal, and the simplification doesn't serve as good example)_

---

_Renamed from "Rule: useless ternary operation - `method() if cond else None`" to "Rule: prefer `if` over ternary operation evaluating to `None` if cond doesn't hold" by @dorschw on 2023-08-24 11:25_

---

_Label `rule` added by @zanieb on 2023-08-24 21:09_

---

_Comment by @wookie184 on 2023-08-26 14:03_

Would a better rule just be one that just triggered if the result of a ternary is unused (i.e. on a line by itself)? I think that would still catch both of your cases, but would avoid potential false positives e.g.
```python
maybe_int = int(s) if s.isdecimal() else None
```
And would also catch extra cases like:
```python
print("hello") if cond else print("goodbye")
```

---

_Comment by @wookie184 on 2023-08-26 15:39_

I can have a go at implementing this, I assume it would be a `RUF` rule since I can't see an implementation elsewhere?

---

_Label `needs-decision` added by @charliermarsh on 2023-08-26 15:42_

---

_Comment by @charliermarsh on 2023-08-26 15:53_

We could include it as a `RUF` rule. I'd like to see if it flags anything on the ecosystem CI checks before committing to merging it. Are you open to implementing, then evaluating whether there are valid patterns that we might be overlooking based on what gets flagged?

(I think your suggestion of checking if the result is unused makes sense -- so checking for `StmtExpr` where the `Expr` is a ternary.)


---

_Comment by @wookie184 on 2023-08-26 16:09_

Yeah that sounds good, it doesn't look too difficult to implement so there'd be little loss if we decide not to continue after opening a PR 👍 

---

_Comment by @fgimian on 2024-01-09 12:57_

I think there's merit in a rule that completely disallows ternary expressions as they [cannot be accurately measured by coverage.py](https://github.com/nedbat/coveragepy/issues/509). There's [flake8-if-expr](https://github.com/afonasev/flake8-if-expr) which does this.

Would it be possible to support this as part of this feature please?

Thanks heaps
Fotis

---
