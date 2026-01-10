```yaml
number: 14395
title: "No syntax error for `[(a := ...) for a in b]`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - rule
assignees: []
created_at: 2024-11-17T02:11:19Z
updated_at: 2025-03-21T18:45:27Z
url: https://github.com/astral-sh/ruff/issues/14395
synced_at: 2026-01-10T11:09:56Z
```

# No syntax error for `[(a := ...) for a in b]`

---

_Issue opened by @InSyncWithFoo on 2024-11-17 02:11_

Currently, Ruff accepts this as valid ([playground](https://play.ruff.rs/75f3f80d-2fee-4502-9141-1934f368ef04)):

```python
[(a := 0) for a in range(0)]
```

However, it isn't:

```shell
$ python -c "[(a := 0) for a in range(0)]"
  File "<string>", line 1
SyntaxError: assignment expression cannot rebind comprehension iteration variable 'a'
```

---

_Comment by @InSyncWithFoo on 2024-11-17 02:17_

Considering that we have [`PLE1142`](https://docs.astral.sh/ruff/rules/await-outside-async/) for a very similar "`SyntaxError`", should this be made a normal rule instead?

---

_Comment by @dylwil3 on 2024-11-17 02:42_

Related: #11934 

---

_Label `rule` added by @MichaReiser on 2024-11-17 09:16_

---

_Comment by @dhruvmanila on 2024-11-18 06:23_

Yeah, I'll add this to #11934 as it's a syntax error raised by the compiler and not the parser. It can be verified by:
```console
$ echo "[(a := 0) for a in range(0)]" | python -m ast -
Module(
   body=[
      Expr(
         value=ListComp(
            elt=NamedExpr(
               target=Name(id='a', ctx=Store()),
               value=Constant(value=0)),
            generators=[
               comprehension(
                  target=Name(id='a', ctx=Store()),
                  iter=Call(
                     func=Name(id='range', ctx=Load()),
                     args=[
                        Constant(value=0)],
                     keywords=[]),
                  ifs=[],
                  is_async=0)]))],
   type_ignores=[])
```

---

_Closed by @ntBre on 2025-03-21 18:45_

---
