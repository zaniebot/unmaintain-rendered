---
number: 8481
title: "Formatter: Bug with Pragma Comment for pyright being moved to invalid position"
type: issue
state: closed
author: roshanjrajan-zip
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-11-03T21:58:01Z
updated_at: 2024-10-31T23:00:42Z
url: https://github.com/astral-sh/ruff/issues/8481
synced_at: 2026-01-10T01:22:48Z
---

# Formatter: Bug with Pragma Comment for pyright being moved to invalid position

---

_Issue opened by @roshanjrajan-zip on 2023-11-03 21:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:


* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Trying to enable ruff formatter on my repo and am getting an error when using in conjunction with a #pyright pragma comment. Its a bit convoluted of an example but this does error out. The `# pyright: ignore[reportUnknownVariableType]` should apply to the `for a in` line however keeps getting moved to where the parentheses is. If I try and move the parentheses or comment to the same line, it moves it back to what it looks like below. Interesting enough, in VsCode it moves the parentheses on the first save and then moves parentheses and comment later again on subsequent saves/running from terminal. 

Code example:
```python
def test_function(
    very_long_dictionary: dict,  # pyright:ignore
):
    something = [
        a["foo"]
        for a in
        (  # pyright: ignore[reportUnknownVariableType]
            very_long_dictionary.get(
                "longer_key_name"  # pyright: ignore
            )
            or []
        )
    ]
```

Running on ruff v0.1.3. Edit: Error still shows up on v0.1.4.

No specific rules for ruff format and config looks like this
```toml
select = [
  "F",      
  "I",     
  "RUF100", 
  "UP004",  
  "UP005",  
  "UP006", 
  "UP007",  
  "UP010", 
  "UP011", 
  "UP012", 
  "UP013", 
  "UP015", 
  "UP017",
  "UP018",
  "UP024", 
  "UP027", 
  "UP034", 
  "UP037", 

]
exclude = [".git", "__pycache__"]
extend-safe-fixes = ["UP006", "UP007"]
line-length = 88
target-version = "py311"

[isort]
required-imports = ["from __future__ import annotations"]



---

_Label `formatter` added by @charliermarsh on 2023-11-04 13:38_

---

_Comment by @roshanjrajan-zip on 2023-11-07 02:04_

Any help would be greatly appreciated! I hoped it would be fixed here - https://github.com/astral-sh/ruff/pull/8431? But it does seem to be along the same avenue.

---

_Comment by @dhruvmanila on 2023-11-07 03:23_

Can you provide the original source code? The way you're describing the comment place, is it like this:

```python
		# pyright: ignore[reportUnknownVariableType]
        for a in
        (
```

Or,

```python
        for a in  # pyright: ignore[reportUnknownVariableType]
        (
```

---

_Comment by @roshanjrajan-zip on 2023-11-07 17:44_

It should be the second one. The code that I shared should show the same result that I am describing. Are you not able to reproduce it?

---

_Comment by @roshanjrajan-zip on 2023-11-11 04:00_

Unfortunately I can't even use `# fmt:skip` on the same line as the #pyright pragma comment to skip this change. The only way to get past this is to add the `# fmt:skip` on the line with just the parentheses.
```python
def test_function(
    very_long_dictionary: dict,  # pyright:ignore
):
    something = [
        a["foo"]
        for a in  # pyright: ignore[reportUnknownVariableType]
        (  # fmt:skip <- Needed to add this here
            very_long_dictionary.get(
                "longer_key_name"  # pyright: ignore
            )
            or []
        )
    ]
```

---

_Comment by @roshanjrajan-zip on 2023-11-11 08:59_

Okay I got a way to reproduce this on terminal as it happens consistently in VSCode but not in terminal...
I suspect I see this in VSCode consistently because its probably run without cache across runs since it only formats the file on save.

Context: The most important line is where the `# pyright: ignore[reportUnknownVariableType]` is placed. It is supposed to be applied to the variable `a` and putting it on a different line invalidates its application when using pyright.

Code is in file test.py
Step 1. Copy code snippet to test.py
```python
# test.py
def test_function(
    very_long_dictionary: dict,  # pyright:ignore
):
    something = [
        a["foo"]
        for a in  # pyright: ignore[reportUnknownVariableType]
        ( 
            very_long_dictionary.get(
                "longer_key_name"  # pyright: ignore
            )
            or []
        )
    ]
```

Step 2. Run `ruff format --no-cache test.py`.
It should look like this. This is a valid case and is a perfectly valid solution.
```python
# test.py
def test_function(
    very_long_dictionary: dict,  # pyright:ignore
):
    something = [
        a["foo"]
        for a in (  # pyright: ignore[reportUnknownVariableType]
            very_long_dictionary.get(
                "longer_key_name"  # pyright: ignore
            )
            or []
        )
    ]
```

Step 3. Run `ruff format --no-cache test.py`.
It should now look like this which is incorrect. The comment isn't on the correct line and any further `ruff format --no-cache test.py` will keep the code as is.
```python
# test.py
def test_function(
    very_long_dictionary: dict,  # pyright:ignore
):
    something = [
        a["foo"]
        for a in 
        (  # pyright: ignore[reportUnknownVariableType]
            very_long_dictionary.get(
                "longer_key_name"  # pyright: ignore
            )
            or []
        )
    ]
```



---

_Comment by @roshanjrajan-zip on 2023-11-13 05:04_

I think this maybe be related to this other[ issue that I am creating to take about differences in no_cache](https://github.com/astral-sh/ruff/issues/8644). 
Uncertain whether it is the no_cache causing the problem or the way that the pragma comment is formatted.


---

_Comment by @roshanjrajan-zip on 2023-11-15 21:21_

@MichaReiser Do you have any code pointers of why there might be instability here? I'm trying to look at https://github.com/astral-sh/ruff/pull/8431 to see if there is anything there but uncertain.

---

_Label `bug` added by @zanieb on 2023-11-16 03:01_

---

_Comment by @MichaReiser on 2023-11-27 06:37_

This isn't related to caching but is an instability bug where the formatter requires two passes to reach the ultimate formatting (unfortunately, the worse). You can see this in the [Playground](https://play.ruff.rs/e285517c-1b83-40a9-9729-f7986b021c66) where I copied the formatted code of the input as the second example. 

The issue comes from that the comment is a dangling comment of the comprehension when formatting the input source, but becomes a leading comment of `b.get` when formatting it the second time. 

A workaround to unblock you is to suppress formatting of the entire statement:

```python
something = [
    a
    for a in # pyright: ignore[reportUnknownVariableType]
    (  
        b.get(
            "longer_key_name",
        )
    )
] # fmt: skip
```

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-27 06:37_

---

_Comment by @MichaReiser on 2024-09-16 16:09_

This has been fixed by https://github.com/astral-sh/ruff/pull/12282 (preview only)

---

_Closed by @MichaReiser on 2024-09-16 16:09_

---

_Comment by @roshanjrajan on 2024-10-31 23:00_

Thank you so much! ❤️ 

---
