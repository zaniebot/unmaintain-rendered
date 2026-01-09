---
number: 9426
title: "Variable named \"args\" leads to ambiguous docstring parsing"
type: issue
state: closed
author: fjossandon
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-01-07T23:16:51Z
updated_at: 2024-01-08T14:59:18Z
url: https://github.com/astral-sh/ruff/issues/9426
synced_at: 2026-01-07T13:12:15-06:00
---

# Variable named "args" leads to ambiguous docstring parsing

---

_Issue opened by @fjossandon on 2024-01-07 23:16_

Hello!
I usually use PyDocStyle to check my docstrings and recently found that that project was deprecated and archived, so they recommended using Ruff instead, so I installed it to test it.

Ruff version is ruff 0.1.11

This is my configuration file:
```
[project]
requires-python = ">=3.11"

[tool.ruff.lint]
extend-select = [
  "UP",  # pyupgrade
  "D",   # pydocstyle
]

[tool.ruff.lint.pydocstyle]
convention = "google"
```
But I quickly found that Ruff was giving me warnings for docstrings that didn't had any problems, because it was confusing the "args" argument name with the "Args" section of the docstring, which generated up to 5 different warnings.

The following is a mock method that shows the problem:
```python
"""Test module for Ruff."""
from typing import Any


def select_data(
        query: str,
        args: tuple[Any],
        database: str,
        auto_save: bool,
) -> None:
    """Take a list of arguments to query the database and return it.

    Args:
        query:
            Query template.
        args:
            A list of arguments.
        database:
            Which database to connect to ("origin" or "destination").
        auto_save:
            True for saving the query response in a CSV file.

    Returns:
        Returns the database data obtained from the query.

    Raises:
        ValueError:
            When the database value does not match a supported option.
    """
    print(query, args, database, auto_save)

```
For this file, Ruff gives this output:
```
$ ruff test.py 
test.py:5:5: D417 Missing argument descriptions in the docstring for `select_data`: `args`, `auto_save`, `database`
test.py:11:5: D410 [*] Missing blank line after section ("Args")
test.py:11:5: D405 [*] Section name should be properly capitalized ("args")
test.py:11:5: D214 [*] Section is over-indented ("args")
test.py:11:5: D411 [*] Missing blank line before section ("args")
Found 5 errors.
[*] 4 fixable with the `--fix` option.
```
It does not recognize the descriptions of any argument after "args" and applies the Section rules to the argument, causing all those warnings. The "args" argument is fairly common in my work, so this means that I cannot replace pydocstyle with ruff yet, which is a pity since ruff looks pretty powerful.

If I change the "args" argument name to something else or if I just change `args:` to `args :`, the messages go away, so it's obvious that Ruff is looking for a case insensitive "whitespace-args:"... Unfortunately, I don't know to code in Rust to try to find a fix, but a suggestion is that maybe you can put an internal flag on which once the "Args:" section is correctly found, it considers any other "args" as argument (especially those in lowercase) in the docstring.

Thanks in advance!


---

_Comment by @charliermarsh on 2024-01-07 23:33_

I need to look into it, but Ruff handles it correctly when formatted as:

```python
"""Test module for Ruff."""
from typing import Any


def select_data(
        query: str,
        args: tuple[Any],
        database: str,
        auto_save: bool,
) -> None:
    """Take a list of arguments to query the database and return it.

    Args:
        query: Query template.
        args: A list of arguments.
        database: Which database to connect to ("origin" or "destination").
        auto_save: True for saving the query response in a CSV file.

    Returns:
        Returns the database data obtained from the query.

    Raises:
        ValueError:
            When the database value does not match a supported option.
    """
    print(query, args, database, auto_save)
```

---

_Comment by @charliermarsh on 2024-01-07 23:37_

I think that pydocstyle may be silently failing for you in this case due to its lack of an explicit convention setting. If you run pydocstyle over this file:

```python
"""Test module for Ruff."""
from typing import Any


def select_data(
        query: str,
        args: tuple[Any],
        database: str,
        auto_save: bool,
) -> None:
    """Take a list of arguments to query the database and return it.

    Args:
        query:
            Query template.
        args:
            A list of arguments.
        database:
            Which database to connect to ("origin" or "destination").
        auto_save:
            True for saving the query response in a CSV file.
    """
    print(query, args, database, auto_save)
```

You get:

```
❯ pydocstyle foo.py --convention=google
foo.py:11 in public function `select_data`:
        D410: Missing blank line after section ('Args')
foo.py:11 in public function `select_data`:
        D417: Missing argument descriptions in the docstring (argument(s) args, auto_save, database are missing descriptions in 'select_data' docstring)
foo.py:11 in public function `select_data`:
        D405: Section name should be properly capitalized ('Args', not 'args')
foo.py:11 in public function `select_data`:
        D214: Section is over-indented ('Args')
foo.py:11 in public function `select_data`:
        D411: Missing blank line before section ('Args')
foo.py:11 in public function `select_data`:
        D417: Missing argument descriptions in the docstring (argument(s) args, auto_save, database, query are missing descriptions in 'select_data' docstring)
```

---

_Comment by @charliermarsh on 2024-01-07 23:38_

And, similarly, if you remove any of the arguments from `Args:`, pydocstyle incorrectly does not error:

```python
"""Test module for Ruff."""
from typing import Any


def select_data(
        query: str,
        args: tuple[Any],
        database: str,
        auto_save: bool,
) -> None:
    """Take a list of arguments to query the database and return it.

    Args:
        args:
            A list of arguments.
        database:
            Which database to connect to ("origin" or "destination").
        auto_save:
            True for saving the query response in a CSV file.

    Returns:
        Returns the database data obtained from the query.

    Raises:
        ValueError:
            When the database value does not match a supported option.
    """
    print(query, args, database, auto_save)
```

---

_Comment by @charliermarsh on 2024-01-07 23:39_

The issue (IIRC) is that pydocstyle uses NumPy-style convention if it finds _any_ NumPy-compatible section names. It's kind of confusing behavior, but e.g., if you have `Raises` or `Returns` in a docstring, pydocstyle will _not_ validate your `Args` section (again, IIRC -- it's been a while since I looked at this).

---

_Label `question` added by @charliermarsh on 2024-01-07 23:39_

---

_Label `docstring` added by @charliermarsh on 2024-01-07 23:39_

---

_Comment by @charliermarsh on 2024-01-07 23:40_

I think our behavior is "more correct" than pydocstyle, but we should probably also respect using a newline here since it seems consistent with the [Google style guide](https://google.github.io/styleguide/pyguide.html#doc-function-args). I'm surprised that we don't already.

---

_Renamed from "Docstring false positives when using "args" as argument name and convention "google"" to "Allow newline after argument docstrings in Google convention" by @charliermarsh on 2024-01-07 23:41_

---

_Label `question` removed by @charliermarsh on 2024-01-07 23:41_

---

_Comment by @charliermarsh on 2024-01-07 23:46_

(Repurposed the issue to look into that newline behavior, though there may be a reason it's unsupported -- need to look into it.)

---

_Comment by @charliermarsh on 2024-01-07 23:51_

Oh wow, I think we actually _do_ support newlines there. The issue is that using an argument named `args` is leading to ambiguous grammar that is hard to parse. It's specific to using a variable name that shadows a section name.

Specifically: when you have `args:` followed by a newline, the parser thinks it _might_ be a section named `Args:` (since we also detect lowercased sections so that we can suggest using upper-casing for them, as a separate lint rule).

I don't think we can properly support that, since it is legitimately ambiguous... I'd recommend using a less ambiguous variable name, or omitting the newline in that case, or putting `args` first in the list (which reduces the ambiguity), or using `*args:`, which we'll also parse correctly.

---

_Renamed from "Allow newline after argument docstrings in Google convention" to "Variable named "args" leads to ambiguous docstring parsing" by @charliermarsh on 2024-01-07 23:51_

---

_Comment by @charliermarsh on 2024-01-07 23:52_

(I'm gonna close since this is unfortunately as intended given the name collision, but I'm happy to explain in more detail or help with debugging in any other way.)

---

_Closed by @charliermarsh on 2024-01-07 23:52_

---

_Comment by @fjossandon on 2024-01-08 01:07_

Hello @charliermarsh, you wrote faster than I could reply.

As you already confirmed, the newline after the argument name is also  part of the "goggle" convention that we chose to follow, and we use it for more clarity's sake,

> I don't think we can properly support that, since it is legitimately ambiguous... I'd recommend using a less ambiguous variable name, or omitting the newline in that case, or putting args first in the list (which reduces the ambiguity), or using *args:, which we'll also parse correctly.

In my case, the argument name is args, but is not the same as *args, so I cannot use that.

I'm sorry, but I don't think is ambiguous at all, in my example, the section Args is clearly there, and all the arguments are correctly indented inside the section, also the capitalization of the section name and arguments is different. And since I'm following the standard correctly, I should not be forced to rename the argument. I really think that argument naming should not be restricted, since that is not part of the docstring guideline, it's an issue of the linter confusing the names. Another thing that comes to my mind is that in the docstring there can only be 1 Args section (as far as I know), so it makes sense to think that if there is already an "Args:" found, other "args:" cannot be a section too.

I just saw another issue similar to this one but using "raises" instead of "args", so other people are being affected too:
https://github.com/astral-sh/ruff/issues/8457

I do think you can support it by adding some extra logic in Ruff. It seems that this I caused because Ruff wants to warn about a potential error in the capitalization of the Args section (the D405), so it wants to treat any "section_keyword:"-like line as a Section, but it's too indiscriminate. In my example, the Args section is established with the correct capitalization, so I think that Ruff could flag that line as the Section, remember its indentation (number of whitespaces), and treat all the following indented lines as arguments without testing the section keywords on them again until finding either a blank line, a line with same or less indentation that the previous Section header, or finishing the docstring, and that would solve the problem. Now if the section header doesn't have the correct capitalization, then Ruff can go  bananas on warnings of course, Using that structured logic on the parsing, it would be more robust and even this extreme example would work, no naming limitation:
```
    Args:
        args:
            First argument.
        returns:
            Second argument.
        raises:
            Third argument.
        yields:
            Fourth argument.
```
Please consider it and give it a thought. I would offer to do it but I don't know how to program in Rust. =/


---

_Comment by @fjossandon on 2024-01-08 01:14_

Addendum.

> I think that pydocstyle may be silently failing for you in this case due to its lack of an explicit convention setting. If you run pydocstyle over this file:

I found this weird. I tried again with pydocstyle using your same command line with the explicit convention, but I did not get any warning myself, maybe you have a different version?? I have the last one released.
```
$ pydocstyle test.py --convention=google
$ pydocstyle --version
6.3.0
```

---

_Comment by @charliermarsh on 2024-01-08 01:32_

I'm using the same version:

```
❯ pydocstyle --version
6.3.0
```

To clarify, you see these errors when you remove `Raises:` and `Returns:`, as in:

```python
"""Test module for Ruff."""
from typing import Any


def select_data(
        query: str,
        args: tuple[Any],
        database: str,
        auto_save: bool,
) -> None:
    """Take a list of arguments to query the database and return it.

    Args:
        query:
            Query template.
        args:
            A list of arguments.
        database:
            Which database to connect to ("origin" or "destination").
        auto_save:
            True for saving the query response in a CSV file.
    """
    print(query, args, database, auto_save)
```

If you leave `Raises:` or `Returns:`, then pydocstyle doesn't validate your `Args:` at all. You can try removing an argument like `query:`, and pydocstyle will incorrectly _not_ raise an error.

---

_Comment by @charliermarsh on 2024-01-08 01:35_

I hear you. I'm happy to re-open and look into changing the behavior here, though I'll caveat that the parsing logic is somewhat precarious -- it's derived from the same logic in pydocstyle, and is based on string parsing rather than a formal grammar. Depending on how invasive it is to fix, I may close again it's more complex than is worthwhile for this edge case.

---

_Reopened by @charliermarsh on 2024-01-08 01:36_

---

_Comment by @charliermarsh on 2024-01-08 01:39_

I agree that this should be considered a bug.

---

_Label `bug` added by @charliermarsh on 2024-01-08 01:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-08 02:12_

---

_Referenced in [astral-sh/ruff#9427](../../astral-sh/ruff/pulls/9427.md) on 2024-01-08 02:35_

---

_Comment by @charliermarsh on 2024-01-08 02:35_

I _think_ I was able to fix this in https://github.com/astral-sh/ruff/pull/9427.

---

_Closed by @charliermarsh on 2024-01-08 03:41_

---

_Comment by @charliermarsh on 2024-01-08 03:41_

Fixed, will go out in the next release!

---

_Comment by @fjossandon on 2024-01-08 14:58_

> If you leave Raises: or Returns:, then pydocstyle doesn't validate your Args: at all. You can try removing an argument like query:, and pydocstyle will incorrectly not raise an error.

I see, thanks for explaining, I didn't fully understand before.

> I think I was able to fix this in https://github.com/astral-sh/ruff/pull/9427.

That's amazing, thanks a lot for the quick fix!!! If it also works with the other issue https://github.com/astral-sh/ruff/issues/8457, maybe you can close that one too. =)

I will be eagerly waiting for your next release to update it.

---

_Comment by @charliermarsh on 2024-01-08 14:59_

Oh, great call! I can close the other issue too :)

---

_Referenced in [astral-sh/ruff#12738](../../astral-sh/ruff/issues/12738.md) on 2024-08-08 04:18_

---
