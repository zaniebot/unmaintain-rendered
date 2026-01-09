---
number: 11375
title: "format: different behavior with black for `hug_parens_with_braces_and_square_brackets` and generators"
type: issue
state: open
author: trim21
labels:
  - formatter
  - preview
assignees: []
created_at: 2024-05-12T06:34:52Z
updated_at: 2024-11-15T08:46:37Z
url: https://github.com/astral-sh/ruff/issues/11375
synced_at: 2026-01-07T13:12:15-06:00
---

# format: different behavior with black for `hug_parens_with_braces_and_square_brackets` and generators

---

_Issue opened by @trim21 on 2024-05-12 06:34_

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

I'm using ruff format with `--preview` enable, and it generate different output in this case.

ruff:
```python
import shutil
import asyncio


async def main():
    by_path = {}
    loop = asyncio.get_event_loop()

    if loop:
        value = await asyncio.gather(
            *(
                loop.run_in_executor(None, lambda pp: (pp, shutil.disk_usage(pp).free), p)
                for p in by_path
            )
        )
        return value

    return None
```

black:
```python
import shutil
import asyncio


async def main():
    by_path = {}
    loop = asyncio.get_event_loop()

    if loop:
        value = await asyncio.gather(*(
            loop.run_in_executor(None, lambda pp: (pp, shutil.disk_usage(pp).free), p)
            for p in by_path
        ))
        return value

    return None
```

This is not listed in "Known Deviations from Black", is this unexpected behavoir?

pyproject.toml

```toml
[tool.black]
line-length = 100
target-version = ['py311']
#future = true

unstable = true
preview = true

[tool.ruff]
cache-dir = ".venv/.cache/ruff"
line-length = 100
target-version = 'py311'

exclude = ['dist', '.venv']

[tool.ruff.format]
preview = true
```


---

_Label `formatter` added by @charliermarsh on 2024-05-13 00:23_

---

_Comment by @153957 on 2024-05-13 05:18_

I think this is related; https://github.com/psf/black/pull/3964#issuecomment-1783318353
And this in black is the part which may be missing in ruff: https://github.com/psf/black/pull/3992

---

_Comment by @trim21 on 2024-05-13 06:05_

title of this issue is not very accurate and I don't know how black call this feature...

---

_Comment by @153957 on 2024-05-13 06:18_

I think it is called `hug_parens_with_braces_and_square_brackets`:
https://github.com/astral-sh/ruff/discussions/9945#discussioncomment-8439722

---

_Comment by @trim21 on 2024-05-13 06:44_

> I think it is called `hug_parens_with_braces_and_square_brackets`: [#9945 (comment)](https://github.com/astral-sh/ruff/discussions/9945#discussioncomment-8439722)

thanks

---

_Renamed from "format: different behavior with black for sole function parameter" to "format: different behavior with black for hug_parens_with_braces_and_square_brackets" by @trim21 on 2024-05-13 06:44_

---

_Comment by @dhruvmanila on 2024-05-15 09:26_

It seems like we don't consider generator to be huggable:

https://github.com/astral-sh/ruff/blob/5bde6a08a0b59b00ee3911be4508c72cf31a2cc5/crates/ruff_python_formatter/src/expression/mod.rs#L1126-L1126

I'm not sure if it's intentional or not but Micha might be able to provide more context on this once he's back from his PTO.

Although it does seem that a tuple expression is huggable, so I'd say that the generator should probably be as well.

---

_Label `preview` added by @MichaReiser on 2024-05-27 09:02_

---

_Renamed from "format: different behavior with black for hug_parens_with_braces_and_square_brackets" to "format: different behavior with black for `hug_parens_with_braces_and_square_brackets` and generators" by @MichaReiser on 2024-05-27 09:03_

---

_Comment by @MichaReiser on 2024-05-27 09:05_

> This is not listed in "Known Deviations from Black", is this unexpected behavior?

We don't track differences for preview styles because both Black's and Ruff's preview styles are in flux, making it difficult to track the differences over time.

> I'm not sure if it's intentional or not but Micha might be able to provide more context on this once he's back from his PTO.

I don't think this is intentional. The only thing we need to be mindful of is that generator parentheses in call expressions are optional. Meaning, we should only hug if the generator already has parentheses (the logic must be in sync with the parentheses logic in `FormatExprGenerator`)

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-20 06:25_

---

_Comment by @trim21 on 2024-11-14 22:08_

I encounter a case that ruff didn't format this code, this look similar, is this same issue ?

(blacked, version 2024)
```python
class TestCompareTestFunctions(unittest.TestCase):
    def test_read_string_or_entries(self):
        with self.assertRaises(cmptest.TestError) as assctxt:
            cmptest.read_string_or_entries(
                """

              2014-01-27 * "UNION MARKET"
                Liabilities:US:Amex:BlueCash    -22.02 USD
                Expenses:Food:Grocery

            """
            )
        self.assertRegex(str(assctxt.exception), "may not use interpolation")
```

or this:

```py
class TestCompareTestFunctions(unittest.TestCase):
    def test_read_string_or_entries(self):
        with self.assertRaises(cmptest.TestError) as assctxt:
            cmptest.read_string_or_entries("""

              2014-01-27 * "UNION MARKET"
                Liabilities:US:Amex:BlueCash    -22.02 USD
                Expenses:Food:Grocery

            """)
        self.assertRegex(str(assctxt.exception), "may not use interpolation")
```

---

_Comment by @MichaReiser on 2024-11-15 06:59_

@trim21 I suspect your example is https://docs.astral.sh/ruff/formatter/black/#call-expressions-with-a-single-multiline-string-argument although I don't fully understand what's the input 

---

_Comment by @trim21 on 2024-11-15 08:46_

> @trim21 I suspect your example is https://docs.astral.sh/ruff/formatter/black/#call-expressions-with-a-single-multiline-string-argument although I don't fully understand what's the input

oh, thanks  i miss this

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:44_

---
