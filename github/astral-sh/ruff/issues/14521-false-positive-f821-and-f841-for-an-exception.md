---
number: 14521
title: False positive F821 and F841 for an exception object inside a lambda function
type: issue
state: closed
author: dqd
labels:
  - bug
assignees: []
created_at: 2024-11-22T08:59:38Z
updated_at: 2024-11-23T07:14:40Z
url: https://github.com/astral-sh/ruff/issues/14521
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive F821 and F841 for an exception object inside a lambda function

---

_Issue opened by @dqd on 2024-11-22 08:59_

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
Hello, when I run ruff on this code sample, I get F821 and F841 errors:
```
try:
    raise Exception("test")
except Exception as e:
    print((lambda: e)())
```
This is the exact output of the `ruff check sample.py` command (using the current ruff version, i.e. 0.7.4):
```
sample.py:3:21: F841 [*] Local variable `e` is assigned to but never used
  |
1 | try:
2 |     raise Exception("test")
3 | except Exception as e:
  |                     ^ F841
4 |     print((lambda: e)())
  |
  = help: Remove assignment to unused variable `e`

sample.py:4:20: F821 Undefined name `e`
  |
2 |     raise Exception("test")
3 | except Exception as e:
4 |     print((lambda: e)())
  |                    ^ F821
  |

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```
I believe that this is a false positive, as this code runs fine on cPython 3.9. It prints "test".

---

_Label `bug` added by @dylwil3 on 2024-11-22 12:43_

---

_Comment by @dylwil3 on 2024-11-22 18:29_

Thanks for this! Great detective work because this seems to be truly minimal - the issue really has to do with except handlers.

Is it possible, @dhruvmanila or maybe @AlexWaygood , that the AST node for except handlers is meant to have an `ExprName` here instead of just an identifier? Otherwise how will the semantic model know that `e` is being stored in `except Exception as e`?

https://github.com/astral-sh/ruff/blob/e53ac7985daa7722b17e692967c96efe2e053586/crates/ruff_python_ast/src/nodes.rs#L3135-L3142

Edit: It looks like the semantic model is smarter than me, at least, but something fishy still must be going on... I can try to figure it out, sorry for the ping 😄 

https://github.com/astral-sh/ruff/blob/e53ac7985daa7722b17e692967c96efe2e053586/crates/ruff_linter/src/checkers/ast/mod.rs#L1570-L1590

---

_Comment by @harupy on 2024-11-23 04:09_

Although this looks like a false positive, `NameError` occurs if the function is called outside the exception handler:

```python
try:
    raise Exception("test")
except Exception as e:
    f = lambda: print(e)

    f()  # works


f()  # throws
```

throws:

```python
test
Traceback (most recent call last):
  File "a.py", line 9, in <module>
    f()  # throws
  File "a.py", line 4, in <lambda>
    f = lambda: print(f"{e!r}")
NameError: name 'e' is not defined
```

---

_Comment by @dylwil3 on 2024-11-23 07:14_

Thanks @harupy, that's an interesting point! It still feels like a false positive to me, though, since `e` is used in the "local scope" (I use scope loosely here since except clauses are a bit weird). After all - the first call to `f` works.

When you leave an `except` clause, there is an [implicit deletion of the binding](https://docs.python.org/3/reference/compound_stmts.html#except-clause) of the exception to the variable in the `as` clause, which is the cause of the behavior you point out. So you can get the same behavior like this:

```python
def outer():
    x = 1
    def inner():
        print(x)
    inner()   # fine
    del x
    inner()   # throws an error
```

Since the code

```python
def outer():
    x = 1
    def inner():
        print(x)
    inner()   # fine
    del x
```

works just fine, and the printed "1" on my screen seems like evidence that `x` was _used_, I think it's a bug to flag this with either rule.

And, interestingly enough, this working example fools `F821` [playground link](https://play.ruff.rs/9e24b2a7-2bf9-4f1d-872a-1eccadf9e413) but for some reason does _not_ trigger `F841`.

As it happens similar sorts of buggy behavior with these rules have been pointed out in many issues:

- #7733
- #12451
- #9349
- #6242

Including, as it happens - the same behavior we're talking about here, which I somehow missed:
- #6878
😂 
So you can see some previous discussion on the topic. I'll close this one in favor of #6878 and we can continue the discussion there. Cheers!


---

_Closed by @dylwil3 on 2024-11-23 07:14_

---
