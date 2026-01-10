```yaml
number: 7462
title: "Formatter instability: Instable call chain formatting"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-17T15:05:12Z
updated_at: 2023-09-19T06:29:07Z
url: https://github.com/astral-sh/ruff/issues/7462
synced_at: 2026-01-10T11:09:49Z
```

# Formatter instability: Instable call chain formatting

---

_Issue opened by @MichaReiser on 2023-09-17 15:05_

## Input 

```python
if grid is not None:
    rgrid = (rgrid
                .rio.reproject_match(grid,
                                    nodata=fillvalue) # rio.reproject nodata is use to initlialize the destination array
                .where(~grid.isnull())
                )

```

## Format 1

```python
if grid is not None:
    rgrid = (
        rgrid.rio.reproject_match(
            grid, nodata=fillvalue
        ).where(  # rio.reproject nodata is use to initlialize the destination array
            ~grid.isnull()
        )
    )
```

## Format 2

```python
if grid is not None:
    rgrid = rgrid.rio.reproject_match(
        grid, nodata=fillvalue
    ).where(  # rio.reproject nodata is use to initlialize the destination array
        ~grid.isnull()
    )

```

Sourced by #7445

---

_Label `bug` added by @MichaReiser on 2023-09-17 15:05_

---

_Label `formatter` added by @MichaReiser on 2023-09-17 15:05_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 15:05_

---

_Closed by @MichaReiser on 2023-09-19 06:29_

---
