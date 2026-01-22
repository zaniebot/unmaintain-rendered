```yaml
number: 17642
title: "Add platform dependence to `exclude-dependencies`"
type: issue
state: open
author: jontwo
labels:
  - enhancement
assignees: []
created_at: 2026-01-21T16:10:11Z
updated_at: 2026-01-22T03:33:54Z
url: https://github.com/astral-sh/uv/issues/17642
synced_at: 2026-01-22T04:09:33Z
```

# Add platform dependence to `exclude-dependencies`

---

_@jontwo_

### Summary

GDAL is notoriously difficult to install on Windows, so we're using the OSGeo4W shell in our project CI because it comes with GDAL already installed. We can just add our application on top. See https://github.com/andreglauser/vscode-osgeo4w-dev-template for an example setup - you can just do the following:

```
uv venv --system-site-packages --python C:\OSGeo4W\apps\Python312\python.exe
uv sync
```

This didn't work straight away because `uv` tries to install gdal as a dependency - you need to exclude it in pyproject.toml. However, we also support linux, so don't want to exclude gdal when a job is not running on Windows.

Maybe this is possible, in which case, the doc needs to be updated with an example. I couldn't get it to work though. Instead we have this hack in our CI pipeline, which is only on Windows jobs (running on Powershell).

```
    - echo 'exclude-dependencies = ["gdal"]' | Out-File -FilePath .\pyproject.toml -Append -Encoding utf8
```

### Example

Ideally, I would be able to provide PEP508-style requirements, as you can with `constraint-dependencies`.

```
[tool.uv]
exclude-dependencies = ["gdal ; sys_platform == 'win32'"]
```

---

_Label `enhancement` added by @jontwo on 2026-01-21 16:10_

---

_Comment by @zanieb on 2026-01-22 03:33_

I think maybe you want to use `override-dependencies` instead?

I think adding marker support to `exclude-dependencies` would be challenging.

---
