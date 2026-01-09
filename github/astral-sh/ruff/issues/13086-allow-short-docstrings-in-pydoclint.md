---
number: 13086
title: Allow short docstrings in pydoclint
type: issue
state: closed
author: valentincalomme
labels:
  - configuration
  - docstring
  - accepted
assignees: []
created_at: 2024-08-24T13:37:57Z
updated_at: 2025-01-16T22:05:12Z
url: https://github.com/astral-sh/ruff/issues/13086
synced_at: 2026-01-07T13:12:15-06:00
---

# Allow short docstrings in pydoclint

---

_Issue opened by @valentincalomme on 2024-08-24 13:37_

As of `ruff==0.6.2`, `pydoclint` rules (DOC) doesn't allow any settings. If prioritization is required, I would personally really value having the `--skip-checking-short-docstrings` setting (see: https://jsh9.github.io/pydoclint/config_options.html)

This is currently blocking me from using the rule altogether.

---

_Comment by @tjkuson on 2024-08-24 13:47_

Possibly related to #5860

EDIT: I should probably elaborate. The proposal to restrict docstring diagnostics based on docstring complexity (what you describe) is distinct to what I linked (which is a proposal to limited based on function complexity). However, they face overlapping design challenges, so it might make sense to link them.

---

_Label `configuration` added by @dhruvmanila on 2024-08-26 04:55_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-26 04:55_

---

_Referenced in [astral-sh/ruff#13302](../../astral-sh/ruff/pulls/13302.md) on 2024-09-10 05:19_

---

_Label `docstring` added by @AlexWaygood on 2024-09-10 13:37_

---

_Comment by @AlexWaygood on 2024-09-10 13:42_

@valentincalomme or @augustelalande, would you be able to elaborate a bit more about why this config option is desirable? I don't feel like #13302 gives a great description of what the config option is for right now, but I'm not sure how to improve it as I don't fully feel like I understand why it would be useful.

---

_Comment by @valentincalomme on 2024-09-11 09:22_

@AlexWaygood for me it is desirable in the very simple scenario where I want to add a "one-liner" docstring, just to explain briefly what my method does. I dont' want pydoclint to complain that I am missing descriptions for the parameters. Actually, a good description is how `darglint` describes their strictness (https://pypi.org/project/darglint/). They have 3 modes:

- short: One-line descriptions are acceptable; anything more and the docstring will be fully checked.
- long: One-line descriptions and descriptions without arguments/returns/yields/etc. sections will be allowed. Anything more, and the docstring will be fully checked.
- full: (Default) Docstrings will be fully checked.
 
 Right now, the `DOC` rules are always set to "full" strictness, which forces all docstrings to include everything (parameters, returns, raises, etc.). I personally like to follow the "if you add it, it needs to be correct, but omitting it is ok" approach for docstrings

---

_Comment by @tmke8 on 2024-09-11 14:02_

I, too, always use `--skip-checking-short-docstrings` for pylint. I'm using type hints for all parameters anyway, so if it's just a simple function, I don't feel the need to document all parameters explicitly with a sentence; all I want is a one-line explanation of what the function does:

```python
def square(x: float) -> float:
    """Square the given number."""
    ...
```

I mean I guess it is a little weird to tie the requirement to document all parameters to the number of lines in the docstring, but I found this heuristic to work well.

---

_Comment by @tmke8 on 2024-09-12 13:12_

Maybe a better rule would actually be:

- If one `:param ...:` has been specified, all params and return value and exceptions need to be documented.

Instead of

- If the docstring has more than one line, all params and return value and exceptions need to be documented.

---

_Comment by @kemus on 2024-10-15 17:25_

Hey @AlexWaygood,

Google's python style guide explicitly allows for one-liner docstrings to [omit the special sections](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods) like `Args:`, `Returns:`, and `Raises:`.

>Certain aspects of a function should be documented in special sections, listed below. Each section begins with a heading line, which ends with a colon. All sections other than the heading should maintain a hanging indent of two or four spaces (be consistent within a file). **These sections can be omitted in cases where the function’s name and signature are informative enough that it can be aptly described using a one-line docstring.**

(Emphasis mine). Huge amounts of google-style python code follows this pattern for simple methods.

The main purpose is to avoid having to litter your code with meaningless docstrings, for example:

```python
def clamp(value: float, lower_bound: float, upper_bound: float):
    """Clamps a value between lower and upper bounds."""
    return max(min(value, maximum), minimum)
```
Currently this complains with:
>`return` is not documented in docstring Ruff [DOC201]

A workaround right now is to take advantage of the fact that pydoclint's parsing actually seems to work on the short line (which might be a separate bug now that I think about it), and write:
```python
def clamp(value: float, lower_bound: float, upper_bound: float):
    """Returns a value clamped between lower and upper bounds."""
    return max(min(value, maximum), minimum)
```
But that's overly verbose and not actually helpful for understanding the code.

The full example:

```python

def clamp(value: float, minimum: float, maximum: float) -> float:
    """Clamp a value clamped between a minimum and maximum.

    Args:
        value: The value to clamp.
        minimum: The minimum value.
        maximum: The maximum value.

    Returns:
        The clamped value.
    """
    return max(min(value, maximum), minimum)
```
Is so verbose as to be actively unhelpful for readability.

---


Since we support Google's docstrings style, we should not only allow short docstrings in pydoclint, but also enable it by default if the following setting is enabled:
```toml
[tool.ruff.lint.pydocstyle]
convention = "google"
```


---

_Comment by @AlexWaygood on 2025-01-13 12:29_

I tried running the pydoclint rules on my [typeshed-stats](https://github.com/AlexWaygood/typeshed-stats) project while considering whether they were ready to be stabilised for the 0.9 release. I also found that the majority of complaints were for one-line docstrings that I didn't really want to fix, but that most of the complaints regarding my multiline docstrings were helpful. So I think I'm now persuaded that this config option would indeed be useful (and it's a point in its favour that the config option also exists in the upstream tools).

---

_Label `needs-decision` removed by @AlexWaygood on 2025-01-13 12:29_

---

_Label `accepted` added by @AlexWaygood on 2025-01-13 12:29_

---

_Closed by @dylwil3 on 2025-01-16 22:05_

---

_Closed by @dylwil3 on 2025-01-16 22:05_

---
