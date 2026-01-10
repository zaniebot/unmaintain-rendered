```yaml
number: 11118
title: Ruff fails to reject invalid syntax in annotation scopes
type: issue
state: closed
author: JelleZijlstra
labels:
  - rule
  - linter
assignees: []
created_at: 2024-04-24T00:26:58Z
updated_at: 2025-04-03T21:56:57Z
url: https://github.com/astral-sh/ruff/issues/11118
synced_at: 2026-01-10T11:09:53Z
```

# Ruff fails to reject invalid syntax in annotation scopes

---

_Issue opened by @JelleZijlstra on 2024-04-24 00:26_

Ruff produces no errors for these three lines:

```python
type X[T: (yield 1)] = int
type Y = (yield 1)
def _f[T](x: (yield 1)) -> (y := 3): return x
```

But they are syntax errors in Python:

```
>>> type X[T: (yield 1)] = int
  File "<stdin>-0", line 1
SyntaxError: yield expression cannot be used within a TypeVar bound
>>> type Y = (yield 1)
  File "<stdin>-1", line 1
SyntaxError: yield expression cannot be used within a type alias
>>> def _f[T](x: (yield 1)) -> (y := 3): return x
... 
  File "<stdin>-2", line 1
SyntaxError: yield expression cannot be used within the definition of a generic
```

Producing errors for these cases feels low priority, because I don't know why anyone would do this other than to be difficult; I noticed it while looking at the parser. Still, I'd expect Ruff to tell me about all SyntaxErrors in my Python code.

Ruff has a few analogous rules for constructs that produce syntax errors at runtime:
- https://docs.astral.sh/ruff/rules/yield-outside-function/
- https://docs.astral.sh/ruff/rules/await-outside-async/



---

_Label `bug` added by @AlexWaygood on 2024-04-24 00:35_

---

_Label `parser` added by @AlexWaygood on 2024-04-24 00:35_

---

_Comment by @JelleZijlstra on 2024-04-24 00:41_

Not sure if the "parser" label is appropriate here. This sort of thing cannot really be detected in the parser itself, because it requires contextual information that is only available once you have a parse tree. In CPython, these errors get detected during symtable construction. In Ruff, I think it makes the most sense to detect them in standard AST-based lint rules, similar to the two rules I mentioned above.

The AST nodes affected are `yield`, `yield from`, `await`, and the walrus. This applies to all forms of annotation scopes: TypeVar bounds/constraints, generic class bases, generic function annotations, and the values of type aliases.

---

_Label `parser` removed by @AlexWaygood on 2024-04-24 00:47_

---

_Comment by @AlexWaygood on 2024-04-24 00:50_

Fair point... In that case, I wonder if this is this even a "bug". Perhaps this could be classified as a "feature request for a new rule" 😛

---

_Comment by @dhruvmanila on 2024-04-24 05:44_

Thanks! So, as per the grammar this is a valid syntax which is why the parser doesn't raise a syntax error. 
```
Module(
   body=[
      TypeAlias(
         name=Name(id='X', ctx=Store()),
         type_params=[
            TypeVar(
               name='T',
               bound=Yield(
                  value=Constant(value=1)))],
         value=Name(id='int', ctx=Load()))],
   type_ignores=[])
```

Correct me if I'm wrong but I believe that in CPython this is being done at compile time. There are a couple more syntax errors which we want to look into, so this should be a new rule. Also, all of these syntax errors which are being raised at compile time would be categorized in a "Correctness" (or similar) category after #1774 is done.

---

_Label `bug` removed by @dhruvmanila on 2024-04-24 05:45_

---

_Label `rule` added by @dhruvmanila on 2024-04-24 05:45_

---

_Label `linter` added by @dhruvmanila on 2024-04-24 05:45_

---

_Closed by @ntBre on 2025-04-03 21:56_

---

_Closed by @ntBre on 2025-04-03 21:56_

---
