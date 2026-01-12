```yaml
number: 12721
title: Changes in sub pyproject.toml do not cause a cache bust
type: issue
state: closed
author: fellhorn
labels:
  - cache
assignees: []
created_at: 2024-08-06T20:41:27Z
updated_at: 2024-08-07T19:53:46Z
url: https://github.com/astral-sh/ruff/issues/12721
synced_at: 2026-01-12T15:54:52Z
```

# Changes in sub pyproject.toml do not cause a cache bust

---

_@fellhorn_

When modifying a `pyproject.toml` in a subfolder ruff does correctly handle this as a cache bust. The same modification of the root level `pyproject.toml` causes a cache bust.

The setup can be recreated with this script:

```bash
#!/bin/bash

# Create root pyproject.toml
cat << EOF > pyproject.toml
[tool.ruff.lint]
select = []
EOF

# Create lib directory and its pyproject.toml
mkdir lib
cat << EOF > lib/pyproject.toml
[tool.ruff.lint]
select = []
EOF

# Create a Python file with a hello world function
# It violates ANN rules
cat << EOF > lib/hello.py
def hello_world():
    print("Hello, World!")
EOF

# Run ruff check
echo "Running initial ruff check:"
ruff check

# Modify lib/pyproject.toml
cat << EOF > lib/pyproject.toml
[tool.ruff.lint]
select = ["ANN"]
EOF

# Run ruff check again - it works due to a cache hit
echo "Running ruff check after modification:"
ruff check

# Ruff check without cache will fail
echo "Running ruff check without cache"
ruff check --no-cache
```

Output:
```bash
Running initial ruff check:
All checks passed!
Running ruff check after modification:
All checks passed!
Running ruff check without cache
lib/hello.py:1:5: ANN201 Missing return type annotation for public function `hello_world`
  |
1 | def hello_world():
  |     ^^^^^^^^^^^ ANN201
2 |     print("Hello, World!")
  |
  = help: Add return type annotation: `None`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```


Tested with `ruff 0.5.6`.

Searched for different combinations of these keywords and have not found an existing issue: cache pyproject.toml subfolder --no-cache subproject

---

_Renamed from "Chnages in sub pyproject.toml do not cause a cache bust" to "Changes in sub pyproject.toml do not cause a cache bust" by @AlexWaygood on 2024-08-06 23:02_

---

_Label `cache` added by @AlexWaygood on 2024-08-06 23:03_

---

_Comment by @dhruvmanila on 2024-08-07 05:27_

https://github.com/astral-sh/ruff/issues/12264 might be related

---

_Closed by @MichaReiser on 2024-08-07 19:53_

---
