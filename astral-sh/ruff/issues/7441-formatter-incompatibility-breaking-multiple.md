```yaml
number: 7441
title: "Formatter incompatibility: breaking multiple context managers"
type: issue
state: closed
author: charliermarsh
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-16T16:43:34Z
updated_at: 2023-11-10T04:32:32Z
url: https://github.com/astral-sh/ruff/issues/7441
synced_at: 2026-01-12T15:54:47Z
```

# Formatter incompatibility: breaking multiple context managers

---

_@charliermarsh_

Given:

```python
if True:
    with tempfile.TemporaryDirectory() as d1:
        symlink_path = Path(d1).joinpath("testsymlink")
        with tempfile.TemporaryDirectory(dir=d1) as d2, tempfile.TemporaryDirectory(
            dir=d1
        ) as d4, tempfile.TemporaryDirectory(dir=d2) as d3, tempfile.NamedTemporaryFile(
            dir=d4
        ) as source_file, tempfile.NamedTemporaryFile(
            dir=d3
        ) as lock_file:
            pass
```

Black formats as:

```python
if True:
    with tempfile.TemporaryDirectory() as d1:
        symlink_path = Path(d1).joinpath("testsymlink")
        with tempfile.TemporaryDirectory(dir=d1) as d2, tempfile.TemporaryDirectory(
            dir=d1
        ) as d4, tempfile.TemporaryDirectory(dir=d2) as d3, tempfile.NamedTemporaryFile(
            dir=d4
        ) as source_file, tempfile.NamedTemporaryFile(
            dir=d3
        ) as lock_file:
            pass
```

Ruff formats as:

```python
if True:
    with tempfile.TemporaryDirectory() as d1:
        symlink_path = Path(d1).joinpath("testsymlink")
        with tempfile.TemporaryDirectory(dir=d1) as d2, tempfile.TemporaryDirectory(
            dir=d1
        ) as d4, tempfile.TemporaryDirectory(dir=d2) as d3, tempfile.NamedTemporaryFile(
            dir=d4
        ) as source_file, tempfile.NamedTemporaryFile(dir=d3) as lock_file:
            pass
```


---

_Label `formatter` added by @charliermarsh on 2023-09-16 16:43_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-16 16:43_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-16 16:43_

---

_Comment by @zanieb on 2023-09-16 16:51_

Hm it'd be great if this formatted as
```python
if True:
    with tempfile.TemporaryDirectory() as d1:
        symlink_path = Path(d1).joinpath("testsymlink")
        with (
            tempfile.TemporaryDirectory(dir=d1) as d2,
            tempfile.TemporaryDirectory(dir=d1) as d4,
            tempfile.TemporaryDirectory(dir=d2) as d3,
            tempfile.NamedTemporaryFile(dir=d4) as source_file,
            tempfile.NamedTemporaryFile(dir=d3) as lock_file,
        ):
            pass
```

which Ruff did for me once but I'm not sure how I triggered it, perhaps with an old version.

---

_Comment by @MichaReiser on 2023-09-16 17:16_

> which Ruff did for me once but I'm not sure how I triggered it, perhaps with an old version.

I think this only happens if you parenthesize the with item**s** and not each individual with item.

---

_Comment by @MichaReiser on 2023-09-18 13:31_

 IMO: Splitting all calls just because a context manager splits seems unnecessary. This is probably related to ruff not splitting a call expression even if its callee expands

```python
if grid is not None:
    rgrid = """Multiline string
    more lines""".where(
        ~grid.isnull()
    )
```

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 16:03_

---

_Label `documentation` added by @MichaReiser on 2023-09-27 16:03_

---

_Comment by @charliermarsh on 2023-09-27 16:03_

We're gonna call this a known deviation that we don't plan to fix. When we implement preview style, we'll support adding parentheses here for supported versions (see: https://github.com/astral-sh/ruff/issues/7234#issuecomment-1720273440), which will achieve the formatting that @zanieb suggested.

---

_Comment by @charliermarsh on 2023-09-27 16:03_

Can be closed once we add documentation.

---

_Closed by @MichaReiser on 2023-09-28 13:42_

---

_Comment by @charliermarsh on 2023-10-04 04:24_

(I don't think was closed by #7694; I _think_ that PR closed a different issue.)

---

_Reopened by @charliermarsh on 2023-10-30 01:07_

---

_Closed by @charliermarsh on 2023-11-10 04:32_

---

_Closed by @charliermarsh on 2023-11-10 04:32_

---
