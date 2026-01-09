---
number: 9805
title: "Formatter: Handling of `# region` & `# endregion` comments"
type: issue
state: open
author: Avasam
labels:
  - suppression
  - formatter
assignees: []
created_at: 2024-02-03T00:21:48Z
updated_at: 2025-11-13T20:25:51Z
url: https://github.com/astral-sh/ruff/issues/9805
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Handling of `# region` & `# endregion` comments

---

_Issue opened by @Avasam on 2024-02-03 00:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

# Region block comments

Is it possible to keep the alignement of `# region` / `# endregion` code-folding comments? Or at least keep them consistent between open and close?
![image](https://github.com/astral-sh/ruff/assets/1350584/88bfdabe-0390-4efd-8b27-e3e2c01c47cc)

---
copied from https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7341829

---

_Label `noqa` added by @zanieb on 2024-02-03 23:46_

---

_Label `formatter` added by @zanieb on 2024-02-03 23:46_

---

_Comment by @MichaReiser on 2024-02-04 11:21_

I'm afraid. This isn't supported today. The formatter doesn't support the concept of region comments and always indents the comments to the same level as the statements. 

I hoped that you could make it work by suppressing the comment but Ruff normalizes the indentation even in that case:

```python
def test(test: name):
    a_long_body

# fmt: off
# region test
# more
# fmt: on    
    some_region

# fmt: off
# endregion
# fmt: on
    after_region
```

Gets formatted to 

```python
def test(test: name):
    a_long_body

    # fmt: off
    # region test
    # more
    # fmt: on
    some_region

    # fmt: off
    # endregion
    # fmt: on
    after_region
```

And having to use `fmt:off` around each region is rather verbose. 

The challenge with supporting a feature like this is that I've seen different preferences and styles when it comes to formatting region comments AND the notation starting and ending a region (beyond that, it further complicates the comment formatting logic to know of region concepts). 

The first step to move this forward would be to come up with a style proposal that has wide support in the community. We could then explore how a feature like this gets implemented, considering that there's sufficient demand for it.

---

_Comment by @kbd on 2024-07-07 12:18_

It would be nice if the position, both line and indentation, of region/endregion comments were generally untouched and ignored by the formatter.

I'm currently writing some FastAPI routes and their tests inline. So I want regions for each API method + test(s) combo.

Instead of getting to do:

```python
# region create
@app.post...
def create("/users"...):
    ...

def test_create():
    ...
# endregion create

# region update
@app.post...
def update("/users/{user_id}"...
    ...

def test_update():
    ...
# endregion update
```

Ruff formats as:

```python
# region create
@app.post...
def create("/users"...):
    ...

def test_create():
    ...


# endregion create


# region update
@app.post...
def update("/users/{user_id}"...
    ...

def test_update():
    ...


# endregion update
```

It works but it's a lot of extra whitespace.

---

_Comment by @Avasam on 2024-08-18 19:18_

I don't remember if I was able to do so in previous Ruff versions, but in 0.6.1 I am able to move the #endregion comment up and indented into the end of the method. So I get consistent code folding comments and it's still working in VSCode.

---

_Referenced in [astral-sh/ruff#16401](../../astral-sh/ruff/issues/16401.md) on 2025-08-16 04:50_

---

_Comment by @pfcodes on 2025-11-13 20:19_

> The challenge with supporting a feature like this is that I've seen different preferences and styles when it comes to formatting region comments AND the notation starting and ending a region (beyond that, it further complicates the comment formatting logic to know of region concepts).

Since #region/#endregion is recognized out of the box by the major IDEs (JetBrains, VSCode, etc.) I think it could be considered a standardized way of doing it rather than a preference.

But for anyone who needs a workaround, adding an additional `#` will work:

```
##region Region name
...
##endregion
```

---
