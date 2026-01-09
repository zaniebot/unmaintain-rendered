---
number: 11205
title: "PLR0912: Pylint counts `try: ... except:` statements as single branch"
type: issue
state: closed
author: Kakadus
labels:
  - rule
assignees: []
created_at: 2024-04-29T16:46:16Z
updated_at: 2024-05-22T03:38:52Z
url: https://github.com/astral-sh/ruff/issues/11205
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR0912: Pylint counts `try: ... except:` statements as single branch

---

_Issue opened by @Kakadus on 2024-04-29 16:46_

PLR0912 is not equivalent to pylint. The following function is accepted by pylint but rejected by ruff:
```py
def capital(country):
    if country == "Australia":
        return "Canberra"
    elif country == "Brazil":
        return "Brasilia"
    elif country == "Canada":
        return "Ottawa"
    elif country == "England":
        return "London"
    elif country == "France":
        return "Paris"
    elif country == "Germany":
        return "Berlin"
    elif country == "Poland":
        return "Warsaw"
    elif country == "Romania":
        return "Bucharest"
    elif country == "Spain":
        return "Madrid"
    elif country == "Thailand":
        return "Bangkok"
    elif country == "Turkey":
        return "Ankara"
    try:
        return None
    except:
        return "Some"
```
Pylint only detects 12 branches, while ruff counts 13. Is this a bug or a feature? If the latter, it should probably be documented. I can also imagine a setting, enabling/disabling this behavior


---

_Comment by @MichaReiser on 2024-04-30 07:34_

The rule was added in https://github.com/astral-sh/ruff/pull/2550 but there's no conversation in that PR nor is there any change to the rule explaining if that divergence is indeed intentional. 

@chanman3388 as author of the rule. What's  your take on this? 

To me, counting `except` as its own branch does make sense. Because the except might or might not be executed. So the try block has two possible exit branches.

---

_Label `rule` added by @dhruvmanila on 2024-04-30 07:48_

---

_Comment by @chanman3388 on 2024-04-30 08:09_

> The rule was added in #2550 but there's no conversation in that PR nor is there any change to the rule explaining if that divergence is indeed intentional.
> 
> @chanman3388 as author of the rule. What's your take on this?
> 
> To me, counting `except` as its own branch does make sense. Because the except might or might not be executed. So the try block has two possible exit branches.

I don't believe that this divergence was intentional, pylint does some odd things, I'd be inclined to agree with @MichaReiser that is _is_ its own branch or brances, but I guess the question to me is whether we should remain faithful to pylint's implementation which looks like:
```python
    def visit_try(self, node: nodes.Try) -> None:
        """Increments the branches counter."""
        branches = len(node.handlers)
        if node.orelse:
            branches += 1
        if node.finalbody:
            branches += 1
        self._inc_branch(node, branches)
        self._inc_all_stmts(branches)
```
or if we should go with our own interpretation. Looking at the python [AST](https://docs.python.org/3/library/ast.html#ast.Try) I can't tell if this was oversight or if the intention was to consider exceptions differently?

---

_Referenced in [astral-sh/ruff#11408](../../astral-sh/ruff/issues/11408.md) on 2024-05-13 15:36_

---

_Comment by @charliermarsh on 2024-05-22 02:29_

It looks like the difference is whether the try _body_ counts as a branch? I'd say it should, probably. Although it's perhaps strange that `finally` counts as a branch, since it always runs? Ultimately, though, the rule is trying to capture some measure of complexity, and both the `try` body and the `finally` contribute to complexity in a way...

---

_Comment by @charliermarsh on 2024-05-22 02:32_

I'd be fine to remove the body from the count though. It's a bit more consistent with Pylint and is pretty marginal either way.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 03:02_

---

_Referenced in [astral-sh/ruff#11487](../../astral-sh/ruff/pulls/11487.md) on 2024-05-22 03:08_

---

_Comment by @dhruvmanila on 2024-05-22 03:13_

Related: https://github.com/astral-sh/ruff/issues/11421

---

_Closed by @charliermarsh on 2024-05-22 03:38_

---
