```yaml
number: 8721
title: "New rule suggestion: Force parentheses in `A or B and C`"
type: issue
state: closed
author: JelleZijlstra
labels:
  - rule
assignees: []
created_at: 2023-11-16T14:38:46Z
updated_at: 2024-01-09T20:30:32Z
url: https://github.com/astral-sh/ruff/issues/8721
synced_at: 2026-01-12T15:54:48Z
```

# New rule suggestion: Force parentheses in `A or B and C`

---

_@JelleZijlstra_

When I see code like `A or B and C`, I never know immediately whether it means `(A or B) and C` or `A or (B and C)`. (The answer is the latter; `and` has higher precedence than `or`.) I would suggest adding a lint rule that triggers if an `and` operation contains an unparenthesized `or`, or if an `or` contains an unparenthesized `and`.

I considered implementing this in flake8-bugbear or Black, but this rule can't be implemented on the AST, so it's hard to do in flake8-bugbear. It could be done in Black, but I would like to be able to see violations of this rule explicitly listed so I can check for possible bugs, and that's easier to do in a linter.

---

_Comment by @zanieb on 2023-11-16 15:20_

I like it.

---

_Label `rule` added by @zanieb on 2023-11-16 15:20_

---

_Comment by @tjkuson on 2023-12-02 13:28_

Would it be possible for this rule to also lint against the use of compound `in` comparisons without parentheses? `False == False in [False]` evaluating to `True` is likely unintended and surprising.

EDIT: Actually, this might be better as a different rule due to the AST differences.

---

_Comment by @AlexWaygood on 2024-01-08 10:19_

(I'm having a go at implementing this.)

How should this rule interact with the `not` operator? My initial instinct was that we should also flag ambiguous uses of `not` as part of this rule, as it's not immediately obvious to me what's going on with `not a or b or c`: `(not a) or b or c` is much clearer, in my opinion. At the same time, though, `a or b or not c` feels pretty clear: emitting a diagnostic there feels like it would be overly noisy.

My suggestion is that we do the following -- what do we think?

- `not a or b or c` => diagnostic: use `(not a) or b or c)`
- `a or not b or c` => diagnostic: use `a or (not b) or c`
- `a or b or not c` => no diagnostic; using it at the end is fine.



---

_Assigned to @AlexWaygood by @MichaReiser on 2024-01-08 10:37_

---

_Comment by @MichaReiser on 2024-01-08 10:43_

Hmm, considering the `not` operator as well raises the question what the scope of the rule is:

* Is it a specific rule only concerned about `and` and `or`
* Is it a more general rule recommending parentheses to clarify operator precedence. 

I really like what Prettier is doing in the JS ecosystem by normalizing parentheses for all code. Unfortunately, our formatter can't do this because Python supports operator overloading, and the overloads may not adhere to the standard operator-semantic (making it unsafe to add or remove parentheses). It would be nice to have a lint rule that establishes a good standard around parenthesizing binary expressions, but that significantly increases this issue's scope. 



---

_Comment by @AlexWaygood on 2024-01-08 10:52_

I agree with keeping the scope narrow for now. The reason why I think `not` fits in pretty well, though, is that it's considered a "logical/boolean" operator by the language reference: https://docs.python.org/3/reference/expressions.html#boolean-operations. This naturally results in it being right next to the other two in the operator precedence: https://docs.python.org/3/reference/expressions.html#operator-precedence. If we're covering interactions between the other two boolean operators, it feels like it makes sense to include the third one as well?

---

_Comment by @JelleZijlstra on 2024-01-08 13:45_

I would prefer to keep `not` out, as the precedence is a lot easier to remember and code like `if not x or y` is likely to be innocuous. Here are a few examples from the mypy codebase:

```
mypy/checker.py:        if not isinstance(typ, TupleType) or not all(
mypy/checker.py:        if not self.options.disallow_any_decorated or self.is_stub:
mypy/build.py:        if not module or not graph[module].tree:
mypy/main.py:        elif not messages or n_notes == len(messages):
mypy/config_parser.py:    if not filename or not modules:
```

I don't think Ruff should flag any of these.

---

_Comment by @AlexWaygood on 2024-01-08 13:49_

> Here are a few examples from the mypy codebase:
> 
> ```
> mypy/checker.py:        if not isinstance(typ, TupleType) or not all(
> mypy/checker.py:        if not self.options.disallow_any_decorated or self.is_stub:
> mypy/build.py:        if not module or not graph[module].tree:
> mypy/main.py:        elif not messages or n_notes == len(messages):
> mypy/config_parser.py:    if not filename or not modules:
> ```
> 
> I don't think Ruff should flag any of these.

Alright, those are pretty persuasive. I'll leave `not` out for now, thanks!

---

_Closed by @charliermarsh on 2024-01-09 01:28_

---

_Closed by @charliermarsh on 2024-01-09 01:28_

---

_Comment by @johnslavik on 2024-01-09 20:30_

That might be a bit off-topic, but I felt like I need to say it, as you might be teaching others how to read a code like this without parentheses:
The precedence can be explained with a math analogy.
Broadly speaking, in combinatorics, when creating formulas that describe probabilities, multiplication is like `and` (the rule of product) and addition is like `or` (the rule of sum). That's how I teach my students and it becomes very simple for them to read cases like `A or B and C` and see clearly that `B and C` gets evaluated before `A or ...`, as it would in `A + B * C`.
But yeah, I agree it might be confusing for the majority of people.

---
