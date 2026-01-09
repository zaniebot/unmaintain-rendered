---
number: 14131
title: B018 does not detect unused tuple expressions
type: issue
state: open
author: LordAro
labels:
  - bug
assignees: []
created_at: 2024-11-06T11:38:27Z
updated_at: 2025-01-11T10:44:27Z
url: https://github.com/astral-sh/ruff/issues/14131
synced_at: 2026-01-07T13:12:16-06:00
---

# B018 does not detect unused tuple expressions

---

_Issue opened by @LordAro on 2024-11-06 11:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

B018 (useless-expression) does not detect function calls with bare tuples:

```python
def foo(): ...

foo(),
```

COM818 (trailing-comma-on-bare-tuple) detects this as it should, but I feel like B018 should flag this as well?

Ruff 0.7.2

---

_Comment by @MichaReiser on 2024-11-06 12:14_

Flagging this as `B018` is only correct if `foo` has no side effects. For example, you could have

```py
print("test"),
```

Removing the `print` would change the program behavior, which is why it's not useless.

Reasoning whether a function is only be possible in the most trivial cases because standard library functions in typeshed don't annotate if they have side effects.

That's why Ruff's current behavior is correct because it would be unsafe to assume the function has no side effects.


---

_Label `question` added by @MichaReiser on 2024-11-06 12:14_

---

_Comment by @LordAro on 2024-11-06 12:17_

Indeed, and a `print(),` is originally how I found this behaviour.

Maybe the answer is just to enable flake8-commas linting...

---

_Comment by @LordAro on 2024-11-06 12:19_

Actually, hmm. It's definitely still a useless expression (unassigned tuple), just a weirdly formatted one where the "fix" is to remove the `,` rather than the entire expression

---

_Comment by @MichaReiser on 2024-11-06 12:30_

Yeah. I think detecting unnecessary tuples could be in the spirit of the rule, but it could also be its own rule, similar to `useless-comparison.` 



---

_Label `rule` added by @MichaReiser on 2024-11-06 12:30_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-06 12:30_

---

_Label `question` removed by @MichaReiser on 2024-11-06 12:30_

---

_Renamed from "B018 does not detect unused tuple expressions when they contain function calls" to "B018 does not detect unused tuple expressions" by @MichaReiser on 2024-11-06 12:30_

---

_Comment by @dylwil3 on 2024-11-06 12:48_

To me this seems to be working as I'd expect (but I'm happy to be overruled by the community).

On the one hand we have a lint rule detecting whether an expression is _useless_. I would think that if removing something useless affected the behavior of the program, then it must not have been useless to begin with. In other words, "useless" seems, to me, to be synonymous with "can be removed with no effect". The expression in question is a tuple expression, and removing it would necessarily remove all of its members. 

There is no expression in the AST that's pointing to the "nonexistent second member":
```bash
~ echo "f()," | python -m ast
Module(
   body=[
      Expr(
         value=Tuple(
            elts=[
               Call(
                  func=Name(id='f', ctx=Load()),
                  args=[],
                  keywords=[])],
            ctx=Load()))],
   type_ignores=[])
```
So there's not really an expression you could point to and call "useless", right?

On the other hand, as you point out, we have a lint rule detecting trailing commas on bare tuples. So I'd think it would be the job of that lint rule to fix this expression.

---

_Comment by @MichaReiser on 2024-11-06 13:14_

I think the idea here is that `print("hello"),` would be fixed to just `print("hello")`, because the tuple expression is useless, not its content. But that might be difficult to convey in a diagnostic.

> On the other hand, as you point out, we have a lint rule detecting trailing commas on bare tuples. So I'd think it would be the job of that lint rule to fix this expression.

I should have taken a closer look at that rule. I agree, we already have a rule that covers what you want. 

---

_Closed by @MichaReiser on 2024-11-06 13:14_

---

_Label `needs-decision` removed by @MichaReiser on 2024-11-06 13:15_

---

_Label `rule` removed by @MichaReiser on 2024-11-06 13:15_

---

_Label `question` added by @MichaReiser on 2024-11-06 13:15_

---

_Comment by @LordAro on 2024-11-06 13:51_

Ah, ok, I've properly understood what I want: [pylint W0106](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/expression-not-assigned.html) which *does* warn about such things

```python
def foo(): ...
[1, 2, foo()]
print("foo"),
```
```
test.py:1:0: W0106: Expression "[1, 2, foo()]" is assigned to nothing (expression-not-assigned)
test.py:2:0: W0106: Expression "(print('foo'), )" is assigned to nothing (expression-not-assigned)
```

obviously the fix is not to remove it entirely (as functions may have side effects) - it's one of
```
_ = [1, 2, foo()]  # a
foo()  # b
```

I don't think you'll need a type checker to do that (though pyright does detect the unused expression)

```
  /foo/test.py:1:1 - error: Expression value is unused (reportUnusedExpression)
  /foo/test.py:2:1 - error: Expression value is unused (reportUnusedExpression)
```

in #970 this lint is listed as implemented via B018, so...

---

_Comment by @dylwil3 on 2024-11-06 14:04_

Ok thanks this helps me understand better!

My takeaway is that `W0106` is a slightly different rule that should just be implemented on its own, and would maybe suggest the fix "a". 

I still feel like "useless" indicates "can be removed" not "should be modified", so I would vote to keep B018 from emitting a diagnostic for this case, but then you have `COM818` and a newly implemented `W0106` to fall back on. Does that seem ok? I'm still open to being convinced out of my pedantic interpretation of the word "useless"!

---

_Comment by @LordAro on 2024-11-06 14:07_

Actual flake8-bugbear flags up this specific example:

>B018: Found useless expression. Either assign it to a variable or remove it. Note that dangling commas will cause things to be interpreted as useless tuples. For example, in the statement `print(".."),` is the same as `(print(".."),)` which is an unassigned tuple. Simply remove the comma to clear the error.

```
$ flake8 --select B test.py
test.py:1:1: B018 Found useless List expression. Consider either assigning it to a variable or removing it.
test.py:2:1: B018 Found useless Tuple expression. Consider either assigning it to a variable or removing it.
```

---

_Comment by @dylwil3 on 2024-11-06 14:49_

Alright, I stand corrected! We should certainly have parity with bugbear for this. Thanks for your persistence in the face of my pedantry!

---

_Reopened by @dylwil3 on 2024-11-06 14:49_

---

_Label `bug` added by @dylwil3 on 2024-11-06 14:50_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-07 23:06_

---

_Label `question` removed by @MichaReiser on 2024-11-28 07:22_

---

_Comment by @InSyncWithFoo on 2025-01-11 07:22_

@dylwil3 Do you intend to work on this? I can take it on if you don't.

---

_Comment by @dylwil3 on 2025-01-11 10:44_

Actually, despite the apparent resolution above, I have had some conversations and am again thinking that this should not be implemented. I'll have a few more conversations and get back to you if we end up still going forward with this- thanks!

---
