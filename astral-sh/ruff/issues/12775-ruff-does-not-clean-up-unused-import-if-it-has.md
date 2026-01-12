```yaml
number: 12775
title: Ruff does not clean up unused import if it has same name like object attribute
type: issue
state: closed
author: ondrej-ivanko
labels:
  - needs-mre
assignees: []
created_at: 2024-08-09T08:07:17Z
updated_at: 2024-08-09T17:11:42Z
url: https://github.com/astral-sh/ruff/issues/12775
synced_at: 2026-01-12T15:54:52Z
```

# Ruff does not clean up unused import if it has same name like object attribute

---

_@ondrej-ivanko_

Keywords: #F401, #imports

command:
using ruff as lsp in helix running `Space - a` - to autofix code (F401 issue is selected)
There are ocassions when ruff does not even register unused imports but it's probably helix related as the options for import fix is not offered in autofix window when using `Space - a`

snippet:
```
from sqlalchemy import text   # this is not considered unused import by ruff

def f():
     resp = request.get("www.google.com")
     resp.text   # accessing attribute of request reponse, not sqlalchemy text confuses ruff

```

### settings(global):
```
line-length = 99
preview = true
output-format = "full"
show-fixes = true
unsafe-fixes = true

[format]
# Enable auto-formatting of code examples in docstrings. Markdown,
# reStructuredText code/literal blocks and doctests are all supported.
#
# This is currently disabled by default, but it is planned for this
# to be opt-out in the future.docstring-code-format = false
docstring-code-format = true

[lint]
select = ["ALL"]
ignore = ["ANN102", "ANN101", "FA100", "FA102", "PT006", "PT007", "D100", "D101", "D202", "D415", "DOC201", "CPY001", "FA100", "S101", "PERF203"]

[lint.flake8-tidy-imports]
# Disallow ..+ relative imports.
ban-relative-imports = "parents"

[lint.flake8-annotations]
suppress-none-returning = true

[lint.flake8-pytest-style]
fixture-parentheses = false

[lint.flake8-unused-arguments]
ignore-variadic-names = true

[lint.pycodestyle]
max-line-length = 99
max-doc-length = 99

[lint.pydocstyle]
# Use Google-style docstrings.
convention = "google"
```

### settings(project):
```
extend = "pyproject.toml"
target-version = "py39"
src = ["backend"]

[lint]
logger-objects = ["libs.observation.otel_logger", "libs.observation.basic_logger"]

[lint.isort]
force-wrap-aliases = true
combine-as-imports = true
lines-after-imports = 1
sections = { varistar = ["utils", "app", "api", "settings", "libs", "tests"]}
section-order = ["future", "standard-library", "first-party", "third-party", "local-folder", "varistar"]

[lint.flake8-bugbear]
# Allow default arguments like, e.g., `data: List[str] = fastapi.Query(None)`.
extend-immutable-calls = ["fastapi.Depends", "fastapi.Query", "fastapi.Path"]

[lint.flake8-import-conventions.extend-aliases]
# Declare a custom alias
"geopandas" = "gpd"

[lint.flake8-tidy-imports]
# Disallow ..+ relative imports.
ban-relative-imports = "parents"

[lint.flake8-annotations]
suppress-none-returning = true

[lint.flake8-pytest-style]
fixture-parentheses = false

[lint.flake8-unused-arguments]
ignore-variadic-names = true

[lint.pycodestyle]
max-line-length = 99
max-doc-length = 99

[lint.pydocstyle]
# Use Google-style docstrings.
convention = "google"
```

### relevant pyproject.toml sections:
```
...
[tool.flake8]
max-line-length = 99
exclude = [".git", ".venv", "__pycache__", "data", "scripts"]
ignore = [
        "E501", # TODO fix code and remove this exception
        "W503", # conflicts with black
        "W605",
]
...
[tool.ruff]
line-length = 99
```
### helix config:
```
[language-server.ruff.config.settings]
line-length = 99
preview = true
output-format = "full"
show-fixes = true
unsafe-fixes = true

[language-server.ruff]
command = "ruff"
args = ["server", "--preview"]

[[language]]
name = "python"
roots = ["pyproject.toml", "setup.py", "Poetry.lock", ".git"]
language-servers = [
  { name = "ruff" },
  { name = "pyright" },
  # { name = "basedpyright" },
  { name = "spellcheck-lsp-server" }
]
auto-format = false
formatter = { command = "ruff", args = ["format"] }
rulers = [99, 99]   # soft warp / hard wrap
diagnostic-severity = "hint"
text-width = 99
```

ruff -V == 0.5.7

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 13:14_

---

_Comment by @charliermarsh on 2024-08-09 13:16_

Unfortunately I can't reproduce this: https://play.ruff.rs/ad3442b0-75f9-4269-9519-c27b9b7fb108

Are you seeing it consistently? Happy to take a look if we can get a consistent reproduction.

---

_Label `needs-mre` added by @charliermarsh on 2024-08-09 13:16_

---

_Comment by @ondrej-ivanko on 2024-08-09 17:11_

Honestly, I can't reproduce it. It seems ruff is working now in this case. I think it was probably an issue during run in helix. I couldn't find anything in helix logs. When it tried to reproduced this scenario, ruff behaved correctly. I'm closing this. Thank you.

---

_Closed by @ondrej-ivanko on 2024-08-09 17:11_

---
