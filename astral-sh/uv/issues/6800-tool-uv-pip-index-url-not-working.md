```yaml
number: 6800
title: "`tool.uv.pip.index-url` not working"
type: issue
state: closed
author: sunfkny
labels:
  - documentation
assignees: []
created_at: 2024-08-29T09:40:19Z
updated_at: 2024-08-29T16:57:14Z
url: https://github.com/astral-sh/uv/issues/6800
synced_at: 2026-01-10T04:45:09Z
```

# `tool.uv.pip.index-url` not working

---

_Issue opened by @sunfkny on 2024-08-29 09:40_

### Steps to Reproduce:

1. Create a `pyproject.toml` file with the following content:

    ```toml
    [project]
    name = "app"
    version = "0.1.0"
    dependencies = [
        "urllib3",
    ]

    [tool.uv]
    package = false

    [tool.uv.pip]
    index-url = "https://test.pypi.org/simple"
    ```

2. Run the following command:

    ```shell
    ~/app# uv sync --no-cache --upgrade --verbose 2>&1 | grep simple
    ```

3. Observe the output.

### Expected Behavior:

The output should indicate that `uv` is using the specified `index-url` (https://test.pypi.org/simple) to fetch packages.

### Actual Behavior:

The output shows that `uv` is still attempting to access the default PyPI repository:

```
DEBUG No cache entry for: https://pypi.org/simple/urllib3/
```

### Environment:

uv Platform: Ubuntu 24.04 LTS on WSL2
uv Version: uv 0.4.0

---

_Comment by @oriori1703 on 2024-08-29 10:03_

If I understand correctly, `[tool.uv.pip]` only applies to the `uv pip` subcommand.
For `uv sync`, you should use `[tool.uv]` 

---

_Comment by @sunfkny on 2024-08-29 10:53_

> If I understand correctly, `[tool.uv.pip]` only applies to the `uv pip` subcommand. For `uv sync`, you should use `[tool.uv]`

Thanks for pointing this out! It works for me.

However, it seems the current documentation on configuration files ([Configuration Files](https://docs.astral.sh/uv/configuration/files/)) might be a bit misleading. The documentation starts by showing how to configure `[tool.uv.pip]`, while the difference between `[tool.uv]` and `[tool.uv.pip]` is only mentioned at the very bottom of the page

---

_Comment by @charliermarsh on 2024-08-29 12:58_

@sunfkny -- That's a good call, we should change it.

---

_Label `documentation` added by @charliermarsh on 2024-08-29 12:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-29 14:24_

---

_Closed by @charliermarsh on 2024-08-29 16:57_

---

_Closed by @charliermarsh on 2024-08-29 16:57_

---
