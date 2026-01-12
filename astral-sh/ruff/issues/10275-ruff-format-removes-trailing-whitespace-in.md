```yaml
number: 10275
title: ruff format removes trailing whitespace in doctests
type: issue
state: open
author: peterjc
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-03-07T15:06:03Z
updated_at: 2024-03-08T16:00:32Z
url: https://github.com/astral-sh/ruff/issues/10275
synced_at: 2026-01-12T15:54:50Z
```

# ruff format removes trailing whitespace in doctests

---

_@peterjc_

Testing under Linux, using ``test_case.py`` based on a real world example:

[test_case.py.txt](https://github.com/astral-sh/ruff/files/14525985/test_case.py.txt)

```python
"""Silly example using whitespace in doctests.

This defines a function returning a string with trailing space:

>>> pad("X", 3)
'   X   '

This can be used directly in doctests like so:

>>> print(pad("X", 3))
   X   

You can't see it, but there should be three trailing spaces
after the X, and if an editor removes them it breaks the test.
"""


def pad(text, spaces=1):
    """Add the given string with spaces before and after.

    Example:

    >>> print(pad("Barry"))
     Barry 
    >>> pad("John", 2)
    '  John  '

    That's it.
    """
    return " " * spaces + text + " " * spaces


if __name__ == "__main__":
    import doctest

    print("Running doctests...")
    doctest.testmod(verbose=True)
    print("Done")
```

Using the current version of black, the module docstring is unchanged (good), but the function doctest is broken (bad):

```
$ black --version && black --diff test_case.py
black, 24.2.0 (compiled: yes)
Python (CPython) 3.10.12
--- test_case.py        2024-03-07 14:54:01.979428+00:00
+++ test_case.py        2024-03-07 14:57:08.562391+00:00
@@ -19,11 +19,11 @@
     """Add the given string with spaces before and after.

     Example:

     >>> print(pad("Barry"))
-     Barry 
+     Barry
     >>> pad("John", 2)
     '  John  '

     That's it.
     """
would reformat test_case.py

All done! ✨ 🍰 ✨
1 file would be reformatted.
```

This seems to be a long standing bug in black https://github.com/psf/black/issues/1654 although the difference in module level versus function level docstrings might be new.

However with ruff, *both* the module docstring and the function docstring are broken (two doctests fail):

```
$ ruff --version && ruff format --diff test_case.py
ruff 0.3.1
--- test_case.py
+++ test_case.py
@@ -8,7 +8,7 @@
 This can be used directly in doctests like so:

 >>> print(pad("X", 3))
-   X   
+   X

 You can't see it, but there should be three trailing spaces
 after the X, and if an editor removes them it breaks the test.
@@ -21,7 +21,7 @@
     Example:

     >>> print(pad("Barry"))
-     Barry 
+     Barry
     >>> pad("John", 2)
     '  John  '


1 file would be reformatted
```

I agree that it is better to avoid meaningful trailing white space, but it can result in some ugly workarounds (assuming the code cannot be changed due to backward compatibility).

Indeed the GitHub editor remove the trailing spaces too when pasting in the code! I have restored them by hand.

I would like both ruff format and black not to touch trailing whitespace in the output section of a doctest.

Separately flake8 or ruff lint can warn about W291, and the real examples this is based on use ``  # noqa: W291`` on the docstring.

---

_Label `formatter` added by @charliermarsh on 2024-03-07 21:58_

---

_Label `bug` added by @MichaReiser on 2024-03-08 08:02_

---

_Comment by @MichaReiser on 2024-03-08 08:04_

> I would like both ruff format and black not to touch trailing whitespace in the output section of a doctest.

That makes sense to me and we do have the infrastructure to support when using [`docstring-code-format`](https://docs.astral.sh/ruff/settings/#format_docstring-code-format). @BurntSushi do you know how where / how complicated it would be to preserve trailing whitespace in doctest assertion lines?

---

_Comment by @BurntSushi on 2024-03-08 12:51_

I took a couple minutes to look, but it's not obvious to me where the issue is. One thing that I'd wonder about is, if we're reformatting docstring code snippets and reformatting itself will strip whitespace, then I'm not sure we can preserve trailing whitespace while `docstring-code-format` is enabled.

Otherwise, when `docstring-code-format` is disabled, I'm not sure I see any explicit stripping of trailing whitespace from a line in a docstring before printing it. So I'd wonder if this is being done at a lower level of the formatter? But I'm not sure.

---

_Comment by @MichaReiser on 2024-03-08 12:58_

> Otherwise, when docstring-code-format is disabled, I'm not sure I see any explicit stripping of trailing whitespace from a line in a docstring before printing it. So I'd wonder if this is being done at a lower level of the formatter? But I'm not sure.

Oh yeah, that could be it, depending on how we print the trailing space. The Printer doesn't print any whitespace. It defers it until you print the next (non whitespace) character. Although that only applies for `space()` not when writing a string.

Edit: Nope, not it. You can see how the trailing whitespace is missing in the written IR after the `X`. So it must be happening somewhere :laughing:  https://play.ruff.rs/ded4925d-c8be-408e-951d-c51e18c52a9b

---

_Comment by @MichaReiser on 2024-03-08 14:32_

@BurntSushi I believe it's coming from this `trim_end` call https://github.com/astral-sh/ruff/blob/fe79798c12b4771cee0b0c59964ad7bd751c3779/crates/ruff_python_formatter/src/string/docstring.rs#L382-L392

We would probably need a way for the `line` to inform whether the trailing whitespace should be trimmed or not and we would set this to false for the prompt output. But I'm a bit at a loss of where that would have to happen.


---

_Comment by @MichaReiser on 2024-03-08 16:00_

I investigated this a little more. We would need to extend our doctest formatting also to handle the expected output and print that as verbatim instead of using `print_one`. I don't think doing this would be very complicated, but is probably a day or two effort. 

Links:
* [Doctest format](https://docs.python.org/3/library/doctest.html#how-are-docstring-examples-recognized)
* [`add_code_line`](https://github.com/astral-sh/ruff/blob/fe79798c12b4771cee0b0c59964ad7bd751c3779/crates/ruff_python_formatter/src/string/docstring.rs#L874-L908) needs to handle expected output

---
