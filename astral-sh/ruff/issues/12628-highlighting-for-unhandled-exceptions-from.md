```yaml
number: 12628
title: Highlighting for unhandled exceptions from docstrings
type: issue
state: open
author: divaltor
labels:
  - docstring
  - type-inference
assignees: []
created_at: 2024-08-02T13:05:22Z
updated_at: 2024-10-24T14:33:06Z
url: https://github.com/astral-sh/ruff/issues/12628
synced_at: 2026-01-12T15:54:52Z
```

# Highlighting for unhandled exceptions from docstrings

---

_@divaltor_

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
**Thanks for your work and the awesome tool** ❤️

### Problem

Some docstring formats support raise blocks for describing potential exceptions a function may throw. However, neither LSP (pyright) nor IDEs (PyCharm) can suggest handling these exceptions when a function is called. This limitation often leads to two bad practices:

- Handling all types of errors using a broad `except Exception` block, which can mask specific issues and make debugging more difficult.
- Completely overlooking potential exceptions, resulting in unhandled errors that can cause unexpected runtime bugs.

Is it possible to create a rule for highlighting unhandled exceptions? Most related feature in PHP Storm - https://blog.jetbrains.com/phpstorm/2017/11/bring-exceptions-under-control/#unhandled-exception

```python
def positive_sum(a: int, b: int) -> int:
	"""
	Summarize two positive integers

	Raises:
		ValueError: when value is not positive
	"""
	if a < 0 or b < 0:
		raise ValueError('value must be positive integer')

	return a + b

positive_sum(15, 15)   <-- RUF999: Unhandled ValueError exception

try:
	positive_sum(15, 15)  <-- OK
except ValueError:
	...

try:
	positive_sum(15, 15)  <-- OK, but may be should consider case when user use common `Exception` for handle it
except Exception:
	...
```

But there can be problem (may be, I didn't read a whole ruff code to understand how parser and semantic work):

```python
def validate_number(num: int) -> int:
	"""
	Validate number and return error if it's not positive

	Raises:
		ValueError: when value is not positive
	"""
	if num < 0:
		raise ValueError('value must be positive integer')
	
	return num

# There is no docstring
def positive_sum(a: int, b: int) -> int:
	validate_number(a)  <-- RUF999: Unhandled ValueError exception
	validate_number(b)  <-- RUF999: Unhandled ValueError exception

	return a + b

positive_sum(15, 15)  <-- RUF999: Unhandled ValueError exception
```

Also there could be cases when we use `assert` statements without docstring, like

```python
def positive_sum(a: int, b: int) -> int:
	assert a > 0
	assert b > 0

	return a + b

# Strongly not sure if should cover such case, because assertion is supposed to find bugs by `fuzzing`
positive_sum(15, 15)  <-- RUF999: Unhandled AssertionError exception
```

**Is it possible to implement such a rule at all? Because my concern is that is job for LSP, not for linter.**

### Possible limitations

1. Ruff can't analyze external libraries or their docstrings. This means it's not possible to highlight code that calls external libraries, like `requests`.
2. Developers may forget to update docstrings after modifying code, leading to outdated or inaccurate exception information.

---

_Comment by @MichaReiser on 2024-08-02 13:14_

Thanks for the kind words.

If I'm not mistaken than this rule shipped as part of ruff 0.5.5 but under preview mode only. See https://github.com/astral-sh/ruff/pull/11471

---

_Comment by @divaltor on 2024-08-02 13:19_

> Thanks for the kind words.
> 
> If I'm not mistaken than this rule shipped as part of ruff 0.5.5 but under preview mode only. See #11471

I guess this rule just prevent cases when user forgets to update its docstring, but do nothing when user _actually executes_ function with `Raises` block, see my example again (or look at PHPStorm blog which I've attached to at the top)

---

_Comment by @MichaReiser on 2024-08-02 13:23_

Oh yeah sorry. That's something else. I don't think we have the necessary infrastructure in place today because ruff can't do any cross-file analysis. 

---

_Label `multifile-analysis` added by @MichaReiser on 2024-08-02 13:23_

---

_Label `docstring` added by @AlexWaygood on 2024-08-02 14:12_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
