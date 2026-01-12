```yaml
number: 7749
title: FEATSupport to set the Project Version using uv command
type: issue
state: closed
author: datnguye
labels: []
assignees: []
created_at: 2024-09-28T03:46:35Z
updated_at: 2024-09-28T03:50:20Z
url: https://github.com/astral-sh/uv/issues/7749
synced_at: 2026-01-12T15:59:16Z
```

# FEATSupport to set the Project Version using uv command

---

_@datnguye_

Currently I'm using `poetry` and would like move into `uv`.

One of the migration point is to set the project version in `pyproject.toml` using  `uv` command.

Similarly using `poetry`: `poetry version 1.0.0` -- This feature will help to simplify the Pypi Publishing workflow.

In my case, here is the sample Github Workflow step:
```
    - name: Build and publish
      run: |
        poetry version $(git describe --tags --abbrev=0)
        poetry build
        poetry publish --username ${{ secrets.PYPI_USERNAME }} --password ${{ secrets.PYPI_PASSWORD }}
```

My expectation is that something I can do the same for `uv`:
```
    - name: Build and publish
      run: |
        uv version $(git describe --tags --abbrev=0)
        uv build
        uv publish --username ${{ secrets.PYPI_USERNAME }} --password ${{ secrets.PYPI_PASSWORD }}
```
OR:
```
    - name: Build and publish
      run: |
        uv build --version $(git describe --tags --abbrev=0)
        uv publish --username ${{ secrets.PYPI_USERNAME }} --password ${{ secrets.PYPI_PASSWORD }}
```


---

_Closed by @datnguye on 2024-09-28 03:50_

---
