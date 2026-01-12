```yaml
number: 13817
title: "`RUF027` - missing f-string - gotcha example"
type: issue
state: closed
author: tony
labels:
  - rule
assignees: []
created_at: 2024-10-19T10:31:02Z
updated_at: 2024-10-19T23:15:52Z
url: https://github.com/astral-sh/ruff/issues/13817
synced_at: 2026-01-12T15:54:53Z
```

# `RUF027` - missing f-string - gotcha example

---

_@tony_

> [!NOTE]
> This may be a duplicate. `RUF027` as a rule is nuanced, I am not sure if this may be duplicating a known caveat of `RUF027`. Fine to close if this works as intended or is a duplicate. Thank you!

[`RUF027`](https://docs.astral.sh/ruff/rules/missing-f-string-syntax/) is a preview. This is tested with [`0.7.0`](https://github.com/astral-sh/ruff/releases/tag/0.7.0).

# Example case: MyST Markdown test fixtures

via Project: gp-libs ([repo](https://github.com/git-pull/gp-libs), [docs](https://gp-libs.git-pull.com/), [PyPI](https://pypi.org/project/gp-libs/))

Examples of strings where `{doctest}` wants to be preserved:
- https://github.com/git-pull/gp-libs/blob/v0.0.7/tests/test_doctest_docutils.py#L102-L110
- https://github.com/git-pull/gp-libs/blob/v0.0.7/tests/test_doctest_docutils.py#L119-L125

Excerpt:

`````python
FIXTURES = [
    #
    # Docutils
    #
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-colons",
        files={
            "example.md": textwrap.dedent(
                """
:::{doctest}

    >>> 4 + 4
    8
:::
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-backticks",
        files={
            "example.md": textwrap.dedent(
                """
```{doctest}

    >>> 4 + 4
    8
```
        """,
            ),
        },
        tests_found=1,
    ),
]
`````

## More details

File ([source](https://github.com/git-pull/gp-libs/blob/v0.0.7/tests/test_doctest_docutils.py)):

<details>

`````python
"""Tests for doctest_docutils."""

import doctest
import pathlib
import textwrap
import typing as t

import pytest

import doctest_docutils

FixtureFileDict = t.Dict[str, str]


class DocTestFinderFixture(t.NamedTuple):
    """Test fixture for doctest_docutils."""

    # pytest
    test_id: str

    # Content
    files: FixtureFileDict
    tests_found: int


FIXTURES = [
    #
    # Docutils
    #
    DocTestFinderFixture(
        test_id="reST-doctest_block",
        files={
            "example.rst": textwrap.dedent(
                """
>>> 4 + 4
8
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="reST-doctest_directive",
        files={
            "example.rst": textwrap.dedent(
                """
.. doctest::

   >>> 4 + 4
   8
        """,
            ),
        },
        tests_found=1,
    ),
    #
    # Markdown / myst-parser
    #
    DocTestFinderFixture(
        test_id="MyST-doctest_block",
        files={
            "example.md": textwrap.dedent(
                """
```
>>> 4 + 4
8
```
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_block-python",
        files={
            "example.md": textwrap.dedent(
                """
```python
>>> 4 + 4
8
```
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_block-indented",
        files={
            "example.md": textwrap.dedent(
                """
Here's a test:

    >>> 4 + 4
    8
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-colons",
        files={
            "example.md": textwrap.dedent(
                f"""
:::{doctest}

    >>> 4 + 4
    8
:::
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-backticks",
        files={
            "example.md": textwrap.dedent(
                f"""
```{doctest}

    >>> 4 + 4
    8
```
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-eval-rst-colons",
        files={
            "example.md": textwrap.dedent(
                """
:::{eval-rst}

   .. doctest::

      >>> 4 + 4
      8
:::
        """,
            ),
        },
        tests_found=1,
    ),
    DocTestFinderFixture(
        test_id="MyST-doctest_directive-eval-rst-backticks",
        files={
            "example.md": textwrap.dedent(
                """
```{eval-rst}

   .. doctest::

      >>> 4 + 4
      8
```
        """,
            ),
        },
        tests_found=1,
    ),
    # sphinx-inline-tabs
    DocTestFinderFixture(
        test_id="MyST-doctest_block-python--sphinx-inline-tabs",
        files={
            "example.md": textwrap.dedent(
                """
````{tab} example tab
```python
>>> 4 + 4
8
```
````

````{tab} example second
```python
>>> 4 + 2
6
```
````
        """,
            ),
        },
        tests_found=2,
    ),
]


class FilePathModeNotImplemented(Exception):
    """Raised if file_path_mode not supported."""

    def __init__(self, file_path_mode: str) -> None:
        return super().__init__(f"No file_path_mode supported for {file_path_mode}")


@pytest.mark.parametrize(
    DocTestFinderFixture._fields,
    FIXTURES,
    ids=[f.test_id for f in FIXTURES],
)
@pytest.mark.parametrize("file_path_mode", ["relative", "absolute"])
def test_DocutilsDocTestFinder(
    tmp_path: pathlib.Path,
    monkeypatch: pytest.MonkeyPatch,
    test_id: str,
    files: FixtureFileDict,
    tests_found: int,
    file_path_mode: str,
) -> None:
    """Test for doctest_docutils."""
    # Initialize variables
    tests_path = tmp_path / "tests"
    first_test_key = next(iter(files.keys()))
    first_test_filename = first_test_key
    if file_path_mode == "absolute":
        first_test_filename = str(tests_path / first_test_filename)
    elif file_path_mode != "relative":
        raise FilePathModeNotImplemented(file_path_mode)

    tests_path.mkdir()
    for file_name, text in files.items():
        rst_file = tests_path / file_name
        rst_file.write_text(
            text,
            encoding="utf-8",
        )

    if file_path_mode == "relative":
        monkeypatch.chdir(tests_path)

    # Test
    finder = doctest_docutils.DocutilsDocTestFinder()
    text, _ = doctest._load_testfile(  # type: ignore
        str(first_test_filename),
        package=None,
        module_relative=False,
        encoding="utf-8",
    )
    tests = finder.find(text, str(first_test_filename))
    tests.sort(key=lambda test: test.name)

    assert len(tests) == tests_found

    for test in tests:
        doctest.DebugRunner(verbose=False).run(test)
`````

</details>

Command:

```
ruff check . --select RUF027 --fix --unsafe-fixes --preview --show-fixes; ruff format .;
```

Output:

```
Fixed 2 errors:
- tests/test_doctest_docutils.py:
    2 × RUF027 (missing-f-string-syntax)

Found 2 errors (2 fixed, 0 remaining).
11 files left unchanged
```

---

_Label `rule` added by @MichaReiser on 2024-10-19 19:14_

---

_Comment by @MichaReiser on 2024-10-19 19:14_

It took me a bit to reproduce so I created a [playground](https://play.ruff.rs/6ebf7ab4-fe76-4c4b-b96e-2a2f28af4382) demonstrating the false positive. 

I'm afraid. I don't think there's much that can be done here unless you're aware of a specific pattern that we can detect to suppress the false positive. 

To me it seems that avoiding this false positive would require understanding this specific API because it very much looks like a possible f-string, at least without knowing more about sphinx. 

I recommend you to disable the rule with a `noqa` comment. 


---

_Comment by @AlexWaygood on 2024-10-19 22:42_

I agree with @MichaReiser here, sadly. There are things we can try to do to reduce the false positives for RUF027 (I'm trying a few of them in https://github.com/astral-sh/ruff/pull/13076, though I need to revisit that PR at some point). But I can't see a way we'd ever be able to prevent this false positive in particular without hardcoding some knowledge of Sphinx's API into the rule -- which I'm somewhat reluctant to do.

---

_Comment by @tony on 2024-10-19 23:05_

@MichaReiser @AlexWaygood Thank you.

in re: https://github.com/git-pull/gp-libs/pull/35, `ignore: RUF027` at the end of the string does the trick:

`````diff
diff --git a/tests/test_doctest_docutils.py b/tests/test_doctest_docutils.py
index b21f22e..73161ff 100644
--- a/tests/test_doctest_docutils.py
+++ b/tests/test_doctest_docutils.py
@@ -102,13 +102,13 @@ class DocTestFinderFixture(t.NamedTuple):
         test_id="MyST-doctest_directive-colons",
         files={
             "example.md": textwrap.dedent(
-                f"""
+                """
 :::{doctest}
 
     >>> 4 + 4
     8
 :::
-        """,
+        """,  # noqa: RUF027
             ),
         },
         tests_found=1,
@@ -117,13 +117,13 @@ class DocTestFinderFixture(t.NamedTuple):
         test_id="MyST-doctest_directive-backticks",
         files={
             "example.md": textwrap.dedent(
-                f"""
+                """
 ```{doctest}
 
     >>> 4 + 4
     8
 ```
-        """,
+        """,  # noqa: RUF027
             ),
         },
         tests_found=1,
`````

Tests work (these are [`doctest`s](https://docs.python.org/3/library/doctest.html), I was relieved that ignoring of the rule could take effect without interfering with the tests).

@MichaReiser @AlexWaygood Preference for closing this? Keeping open? I will leave it up to you.

---

_Comment by @AlexWaygood on 2024-10-19 23:11_

I think I'll close this; I can't see a reasonable way of improving our behaviour here, at least in the short-to-medium term :/

Thanks for opening the issue, sorry we can't do more on this one!

---

_Closed by @AlexWaygood on 2024-10-19 23:11_

---

_Comment by @tony on 2024-10-19 23:15_

No worries - the ignore acts as a seemless workaround for this! Thank you for your assistance!

---
