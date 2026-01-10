```yaml
number: 14168
title: Forbidden combinations with PEP-654
type: issue
state: open
author: jakkdl
labels:
  - parser
assignees: []
created_at: 2024-11-07T16:15:38Z
updated_at: 2024-11-08T15:54:28Z
url: https://github.com/astral-sh/ruff/issues/14168
synced_at: 2026-01-10T11:09:56Z
```

# Forbidden combinations with PEP-654

---

_Issue opened by @jakkdl on 2024-11-07 16:15_

### continue/break/return in except*
continue/break/return in `except*` is a syntax error, see https://peps.python.org/pep-0654/#forbidden-combinations
```python
def foo():
    for _ in range(5):
        try:
            ...
        except* Exception:
            continue
        except* ValueError:
            break
        except* TypeError:
            return
```
```sh
$ ruff check --isolated --select=ALL --ignore=D,ANN ruff_except.py
ruff_except.py:5:9: S112 `try`-`except`-`continue` detected, consider logging the exception
ruff_except.py:5:9: PERF203 `try`-`except` within a loop incurs performance overhead
ruff_except.py:5:17: BLE001 Do not catch blind exception: `Exception`
Found 3 errors.
```
The BLE001 and PERF203 are probably fine, but the S112 seems like a misfire. And of course we should get three errors.

### except + except* error is uninformative
```python
try:
    ...
except BaseException:
    ...
except* BaseException:
    ...
```
```sh
$ ruff check --isolated --select=ALL --ignore=D ruff_except2.py 
error: Failed to parse ruff_except2.py:5:7: Unexpected token '*'
ruff_except2.py:5:7: E999 SyntaxError: Unexpected token '*'
Found 1 error.
```
### as is the other way around
```python
try:
    ...
except* BaseException:
    ...
except BaseException:
    ...
```
```sh
$ ruff check --isolated --select=ALL --ignore=D ruff_except3.py
error: Failed to parse ruff_except3.py:5:8: Expected '"*"', but got 'BaseException'
ruff_except3.py:5:8: E999 SyntaxError: Expected '"*"', but got 'BaseException'
Found 1 error.
```

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


---

_Label `parser` added by @dylwil3 on 2024-11-07 17:53_

---

_Comment by @dylwil3 on 2024-11-08 05:19_

Thanks for catching this, and for the great examples!

Would you mind sharing which ruff version you're using? For some reason I was only able to get the same messages as you when using v0.3. Here's what it looks like with the latest version:

```console
➜  tempruff uv tool run ruff check --select ALL --ignore D,ANN --output-format concise
⠙ Resolving dependencies...                                                             INFO add_decision: root @ 0a0.dev0 without checking dependencies
⠙ ruff==0.7.2                                                                           INFO add_decision: ruff @ 0.7.2 without checking dependencies
ruff_except.py:5:9: S112 `try`-`except`-`continue` detected, consider logging the exception
ruff_except.py:5:9: PERF203 `try`-`except` within a loop incurs performance overhead
ruff_except.py:5:17: BLE001 Do not catch blind exception: `Exception`
ruff_except2.py:3:8: BLE001 Do not catch blind exception: `BaseException`
ruff_except2.py:5:9: BLE001 Do not catch blind exception: `BaseException`
ruff_except2.py:5:9: B025 try-except block with duplicate exception `BaseException`
ruff_except3.py:3:9: BLE001 Do not catch blind exception: `BaseException`
ruff_except3.py:5:8: BLE001 Do not catch blind exception: `BaseException`
ruff_except3.py:5:8: B025 try-except block with duplicate exception `BaseException`
Found 12 errors.
[*] 3 fixable with the `--fix` option.
```

And here's what it looks like in v0.3:

```console
➜  tempruff uv tool run ruff@0.3 check --select ALL --ignore D,ANN
⠙ Resolving dependencies...                                                             INFO add_decision: root @ 0a0.dev0 without checking dependencies
⠙ ruff==0.3.0                                                                           INFO add_decision: ruff @ 0.3.0 without checking dependencies
error: Failed to parse ruff_except2.py:5:7: Unexpected token '*'
error: Failed to parse ruff_except3.py:5:8: Expected '"*"', but got 'BaseException'
ruff_except.py:5:9: S112 `try`-`except`-`continue` detected, consider logging the exception
ruff_except.py:5:9: PERF203 `try`-`except` within a loop incurs performance overhead
ruff_except.py:5:17: BLE001 Do not catch blind exception: `Exception`
ruff_except2.py:5:7: E999 SyntaxError: Unexpected token '*'
ruff_except3.py:5:8: E999 SyntaxError: Expected '"*"', but got 'BaseException'
Found 8 errors.
[*] 3 fixable with the `--fix` option.
```


Ruff switched to a hand-written parser in v0.4 (see the [announcement post](https://astral.sh/blog/ruff-v0.4.0)), and the handling of syntax errors changed (see [this v0.5 post](https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors)). This has certainly been a net big improvement! However, there are still some known limitations in the new implementation, and I can see that this particular syntax error is one such: https://github.com/astral-sh/ruff/blob/272d24bf3e4dabe59d2ec49f505e8fa4e00ba798/crates/ruff_python_parser/src/parser/statement.rs#L1341

When this functionality gets added, this will be a great reminder to have an informative error message!

Finally - you mention that S112 looks like a misfire. Could you say more about that? My understanding was that exception groups are, essentially, collections of exceptions that you might catch. S112 is suggesting doing something with these (e.g. you could iterate over them and log them) rather than just continuing the loop. Is the concern that the message doesn't distinguish between exceptions and groups of exceptions?

Thanks again and let me know if I'm missing anything here!

---

_Comment by @dhruvmanila on 2024-11-08 08:12_

> continue/break/return in `except*` is a syntax error, see [peps.python.org/pep-0654#forbidden-combinations](https://peps.python.org/pep-0654/#forbidden-combinations)

This should be covered by #11934. The other two examples doesn't raise any syntax errors currently (https://play.ruff.rs/6e84baaa-228d-454c-a137-4d7f87f837ae) and as mentioned by @dylwil3 the TODO mentioned should also be part of #11934. I'd prefer to merge this into the linked issue if it's all related to syntax errors raised by the compiler.

---

_Comment by @jakkdl on 2024-11-08 11:46_

Ah, it indeed appears I did some of my testing in an old venv with an ancient ruff version (0.3.7), sorry!

> Finally - you mention that S112 looks like a misfire. Could you say more about that? My understanding was that exception groups are, essentially, collections of exceptions that you might catch. S112 is suggesting doing something with these (e.g. you could iterate over them and log them) rather than just continuing the loop. Is the concern that the message doesn't distinguish between exceptions and groups of exceptions?

S112 talks about wanting to do more than just `continue` inside the `except*` ... but having a `continue` inside an `except*` is fully invalid! `try-except*-pass` makes sense to warn about, but if a user is doing `try-except*-continue` the first priority is to fix the syntaxerror, and once doing so you no longer get any S112, so I think the effect in most cases is just to confuse the user with a redundant error.
I can come up with some contrived example where the user gets S112, remembers that they should handle that problem, and makes sure to also log it when fixing the syntaxerror. But for that you'd probably want a separate improved rule that will trigger more generally on "not making any use of an exception" (though that'd be a very noisy rule, so ehh) so it keeps triggering once the user refactors the code as e.g.
```python
for _ in range(1):
    try:
        ...
    except* ValueError:
        should_continue = True

    if should_continue:
        continue
```



.
Feel free to track/merge the syntax errors however you want :)

---

_Comment by @dylwil3 on 2024-11-08 15:54_

>  if a user is doing try-except*-continue the first priority is to fix the syntaxerror, and once doing so you no longer get any S112, so I think the effect in most cases is just to confuse the user with a redundant error.

Makes sense to me, thanks for the explanation!

---
