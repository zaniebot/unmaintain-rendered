```yaml
number: 16053
title: PLC2801 fix ignores the expression’s syntactic context
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-09T15:51:51Z
updated_at: 2025-02-18T10:20:06Z
url: https://github.com/astral-sh/ruff/issues/16053
synced_at: 2026-01-10T11:09:57Z
```

# PLC2801 fix ignores the expression’s syntactic context

---

_Issue opened by @dscorbett on 2025-02-09 15:51_

### Description

The fix for [unnecessary-dunder-call (PLC2801)](https://docs.astral.sh/ruff/rules/unnecessary-dunder-call/) in Ruff 0.9.5 sometimes misinterprets the context surrounding the rewritten expression, causing changes to program behavior or failing due to syntax errors.

Parentheses are missing when the default precedence of operators does not match the intended evaluation order.
```console
$ cat >plc2801_1.py <<'# EOF'
print((not 1).__add__(1))
try:
    print(list("x".__add__(y for y in "y")))
except TypeError as e:
    print(e)
print(type((lambda: 0).__eq__("x")))
print(("a" and "x").__contains__("x"))
print(("" or "x").__contains__("x"))
print(("" if False else "x").__contains__("x"))
# EOF

$ python plc2801_1.py
1
can only concatenate str (not "generator") to str
<class 'NotImplementedType'>
True
True
True

$ ruff --isolated check --preview --select PLC2801 plc2801_1.py --unsafe-fixes --fix
Found 6 errors (6 fixed, 0 remaining).

$ cat plc2801_1.py
print(not 1 + 1)
try:
    print(list("x" + y for y in "y"))
except TypeError as e:
    print(e)
print(type(lambda: 0 == "x"))
print("x" in "a" and "x")
print("x" in "" or "x")
print("x" in "" if False else "x")

$ python plc2801_1.py
False
['xy']
<class 'function'>
False
x
x
```

Parentheses are missing when all the relevant operators have the same precedence but the intended evaluation order is not left to right.
```console
$ cat >plc2801_2.py <<'# EOF'
print(3 - (2).__add__(1))
# EOF

$ python plc2801_2.py
0

$ ruff --isolated check --preview --select PLC2801 plc2801_2.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat plc2801_2.py
print(3 - 2 + 1)

$ python plc2801_2.py
2
```

Parentheses are missing around an expression rewritten to be a chainable comparison when it is a chaining context.
```console
$ cat >plc2801_3.py <<'# EOF'
print("x".__eq__("y").__eq__(False))
print(False.__eq__("y".__eq__("z")))
# EOF

$ python plc2801_3.py
True
True

$ ruff --isolated check --preview --select PLC2801 plc2801_3.py --unsafe-fixes --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat plc2801_3.py
print("x" == "y" == False)
print(False == "y" == "z")

$ python plc2801_3.py
False
False
```

An assignment expression usually needs to be parenthesized.
```console
$ echo 'print((x := "x").__contains__("y"))' | ruff --isolated check --preview --select PLC2801 - --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

Expressions starting with `not` or `lambda` need to be parenthesized when moved to the right side of a binary operator.
```console
$ echo 'print((not 0).__radd__(1))' | ruff --isolated check --preview --select PLC2801 - --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.

$ echo 'print((not "x").__contains__("y"))' | ruff --isolated check --preview --select PLC2801 - --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.

$ echo 'print((lambda: 0).__contains__("x"))' | ruff --isolated check --preview --select PLC2801 - --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The fix introduces a syntax error when the argument is a starred expression. The rule should flag the expression but not offer a fix.
```console
$ echo 'print("x".__add__(*"y"))' | ruff --isolated check --preview --select PLC2801 - --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

---

_Label `bug` added by @AlexWaygood on 2025-02-09 16:05_

---

_Label `fixes` added by @AlexWaygood on 2025-02-09 16:05_

---

_Comment by @VascoSch92 on 2025-02-12 09:31_

Hey, 
this seems a very nice bug to fix :-) 

Do you mind if I give a try? Or you would like to solve it internally ? (as there is no label `help-needed` :-) ) 

---

_Comment by @MichaReiser on 2025-02-12 09:38_

Sure. Give it a try. There's an `OperatorPrecedence` enum that might be useful to fix this error

---

_Comment by @VascoSch92 on 2025-02-15 10:29_

Hey, 

I was working on this issue, and I have some questions/clarifications to ask:

For instance, it is possible to solve the following bugs:

- `print((not 1).__add__(1))`
-  `try: print(list("x".__add__(y for y in "y"))) except TypeError as e: print(e)`
- `print(("a" and "x").__contains__("x"))`
- `print(("" or "x").__contains__("x"))`
- `print(("" if False else "x").__contains__("x"))`
- `print((x := "x").__contains__("y"))`
- `print((not 0).__radd__(1))`

But I have problems for the following:

1. `print(3 - (2).__add__(1))`: the problem here is that Ruff needs to know not just the `value` and the `arg` of `__add__` but also what come before the call. In this case, the error happens because there is a `-`. Is it something which is possible to control?
2. `print((not "x").__contains__("y"))`: here we are asking if a boolean contains a string. This thrown a Python error as the booleans don't implement the method `__contains__`.  So Ruff is trying to fix something which is already broken. Should Ruff understand that or we should assume that the python syntax/code is correct before launching Ruff?
3. `print(type((lambda: 0).__eq__("x")))`: the function `lambda: 0` is an instance of `function`, which does not define a custom `__eq__` method. It inherits the default `__eq__` from object, which returns `NotImplemented` when the comparison is unsupported. Now, I don't know how to solve it to have the same output. 

Should I open a PR to start discussing the solutions for the above mentioned solved bugs?

---

_Comment by @MichaReiser on 2025-02-17 09:39_

1. I'd have to look at the implementation and I'm not sure what you mean by control but this is something we'd have to support. I assume the problem you're running into is that the rule runs on call expressions. So you don't have access to the parent from the node but you can use `checker.semantic().current_expression_parent()` to get the parent of the current expression
2. Ideally yes, but I think it's fine if the provided fix doesn't break the code in new ways. Ideally, it would still emit the same runtime error after applying the fix
3. This seems to be the same as 2.

---

_Comment by @VascoSch92 on 2025-02-18 10:20_

1. Thanks. I think I fixed it ;-) 
2. The `Exception` are not equal/equivalent, e.g., for `print((not "x").__contains__("y"))` we have a `AttributeError: 'bool' object has no attribute '__contains__'`, while for the relative fix `print("y" in (not "x"))` we have a `TypeError: argument of type 'bool' is not iterable`. Should we just flag these kind of dunder calls without providing a fix or should we also try to fix them even if the relative exceptions are different?

---
