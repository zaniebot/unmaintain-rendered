```yaml
number: 3231
title: "Implement pylint's `else-if-used` rule (`PLR5501`)"
type: pull_request
state: merged
author: chanman3388
labels:
  - rule
assignees: []
merged: true
base: main
head: PLR5501
created_at: 2023-02-26T02:47:49Z
updated_at: 2023-02-26T22:43:23Z
url: https://github.com/astral-sh/ruff/pull/3231
synced_at: 2026-01-12T15:55:12Z
```

# Implement pylint's `else-if-used` rule (`PLR5501`)

---

_@chanman3388_

Attempt to implement else-if-used
https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/else-if-used.html

Issue #970 

---

_Comment by @chanman3388 on 2023-02-26 02:55_

Annoyingly, `else-if-used` _is_ the name of the rule, but it's disallowed, suggestions? `collapsible-else-if`?

---

_Comment by @charliermarsh on 2023-02-26 03:36_

Yeah `collapsible-else-if` seems reasonable to me.

---

_@charliermarsh reviewed on 2023-02-26 03:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:27 on 2023-02-26 03:37_

Can we not use the `orelse` passed into the function already?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 03:39_

Should this be `orelse.len() == 1`? I thought we'd be looking for `orelse.len() == 1 && matches!(orelse[0].node, StmtKind::If { .. })`, which typically indicates an `elif` (or an `else: if:` in this case, which we'd have to differentiate somehow).

---

_@charliermarsh reviewed on 2023-02-26 03:39_

---

_@charliermarsh reviewed on 2023-02-26 03:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 03:42_

Ah, here `stmt` is the `if` in the `else: if: ...`. I'd assumed `stmt` was the outer `if` in:

```py
if ...:
  pass
else:
  if:
    ...
```

---

_@charliermarsh reviewed on 2023-02-26 03:43_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pylint/else_if_used.py`:6 on 2023-02-26 03:43_

It looks like this is being flagged as an error in the fixture, when it should be considered ok.

---

_@charliermarsh reviewed on 2023-02-26 03:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 03:44_

Hmm, I think we _do_ need to structure this such that it looks at this statement:

```py
if ...:
  pass
else:
  if:
    ...
```

Rather than the nested `if`. Otherwise, how can you tell if you're in an `else` or not?

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:27 on 2023-02-26 04:25_

Yes you're right, going to give that a go.

---

_@chanman3388 reviewed on 2023-02-26 04:25_

---

_@chanman3388 reviewed on 2023-02-26 04:42_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 04:42_

Yeah, this is more complicated than I thought, namely because you don't know where the else is.
Pylint does have separate `elif` tokens which makes it easier...the only way I can think of doing it might be a bit janky where you use the locator to see if 'elif' is in the line or something like that.

---

_@chanman3388 reviewed on 2023-02-26 05:15_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 05:15_

So this can be done via the text method...is that ok? It feels a bit...dirty...

---

_Comment by @ngnpope on 2023-02-26 13:38_

Other suggestions for names: `uncollapsed-else-if` or `unnecessary-nested-else-if`? 🤔 

---

_Comment by @chanman3388 on 2023-02-26 14:59_

> Other suggestions for names: `uncollapsed-else-if` or `unnecessary-nested-else-if`? 🤔

I quite like the latter.

---

_@charliermarsh reviewed on 2023-02-26 22:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/else_if_used.rs`:29 on 2023-02-26 22:39_

Yeah it's ok. The alternative would be to do it by lexing and looking at the token stream. But this is effectively equivalent. There's no way to detect this AFAIK from the AST alone.

---

_Merged by @charliermarsh on 2023-02-26 22:42_

---

_Closed by @charliermarsh on 2023-02-26 22:42_

---

_Comment by @charliermarsh on 2023-02-26 22:43_

Merged as-is since I'm totally fine with the name, but happy to tweak in a follow-up PR if you feel strongly.

---

_Label `rule` added by @charliermarsh on 2023-02-26 22:43_

---
