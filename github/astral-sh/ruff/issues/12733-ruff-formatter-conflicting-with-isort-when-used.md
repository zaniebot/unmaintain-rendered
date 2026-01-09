---
number: 12733
title: Ruff formatter conflicting with isort when used as pre-commit hooks.
type: issue
state: closed
author: vmatekole
labels:
  - question
assignees: []
created_at: 2024-08-07T15:17:12Z
updated_at: 2024-08-08T05:57:48Z
url: https://github.com/astral-sh/ruff/issues/12733
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff formatter conflicting with isort when used as pre-commit hooks.

---

_Issue opened by @vmatekole on 2024-08-07 15:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

A conflict is created — when running ruff format as a pre-commit hook, together with isort. Hooks never pass, as a workaround I have placed #fmt off / #fmt on around the imports:

```python
# fmt: off
import os

from azure.appconfiguration.provider import AzureAppConfigurationProvider
from dotenv import load_dotenv
from pydantic import Field
from pydantic_settings import (
    BaseSettings,
    SettingsConfigDict
)
# fmt: on
 ```

**Keywords searched**: isort pre-commit hooks isort imports formatting recursive failure

pyproject.toml(excerpt)
``` toml
[tool.isort]
multi_line_output = 3
force_grid_wrap = 2

[tool.ruff]
line-length = 80
[tool.ruff.format]
quote-style = "double"
indent-style = "space"
docstring-code-format = true

[tool.ruff.lint]
select = [
    "A",  # prevent using keywords that clobber python builtins
    "B",  # bugbear: security warnings
    "D", # pydocstyle
    "E",  # pycodestyle
    "F",  # pyflakes
    "ISC",  # implicit string concatenation
    "RUF",  # the ruff developer's own rules
]

ignore = [
    "B904",  # raise-without-from-inside-except: syntax not compatible with py2
    "D100", # Missing docstring in public module
    "D101", # Missing docstring in public class
    "D102", #  Missing docstring in public method
    "D103", #  Missing docstring in public function
    "D104", #  Missing docstring in public package
    "D105", # Missing docstring in magic method
    "E731",  # lambda-assignment: lambdas are substential in maintenance of py2/3 codebase
    "ISC001",  # conflicts with ruff format command
    "RUF005",  # collection-literal-concatenation: syntax not compatible with py2
    "RUF012", # mutable-class-default: typing is not available for py2
    "I001", # unsorted imports https://docs.astral.sh/ruff/rules/unsorted-imports/#unsorted-imports-i001
    "W191",
    "E111",
    "E114",
    "W191",
    "W191",
    "W191",
    "W191",
    "E117",
    "D206",
    "D300",
    "Q000",
    "Q001",
    "Q002",
    "Q003",
    "COM812",
    "COM819",
    "ISC001",
    "ISC002",
]

[tool.ruff.lint.pydocstyle]
convention = "google"
```
Code formatted app_config.py
```python
import os

from azure.appconfiguration.provider import AzureAppConfigurationProvider
from dotenv import load_dotenv
from pydantic import Field
from pydantic_settings import (
    BaseSettings,
    SettingsConfigDict
)

load_dotenv()


def load_azure_config() -> any:
    return AzureAppConfigurationProvider.load(
        connection_string=""
    )


class Settings(BaseSettings):
    model_config = SettingsConfigDict(env_file_encoding="utf-8")
    authority: str
    redirect_path: str
    scope: str
    session_type: str
    port: int
    azure_sz_config_connection_string: str
    env: str
    azure_credentials: AzureAppConfigurationProvider = Field(
        default_factory=load_azure_config
    )  # type: ignore


sz_config = Settings(_env_file=os.environ["APP_ENV"])

```

.pre-commit-config.yml
```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
      -   id: trailing-whitespace
      -   id: end-of-file-fixer
      -   id: check-yaml
      -   id: check-added-large-files
  - repo: https://github.com/astral-sh/ruff-pre-commit
    # Ruff version.
    rev: v0.5.6
    hooks:
      # Run the linter.
      - id: ruff
        args: [ --fix ]
      # Run the formatter.
      - id: ruff-format
  - repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
      - id: isort
        name: isort (python)
  - repo: https://github.com/compilerla/conventional-pre-commit
    rev: v3.4.0
    hooks:
      - id: conventional-pre-commit
        stages: [commit-msg]
        args: []

```

pre-commit run -v  --files app_config.py 

![Screenshot 2024-08-07 at 17 10 42](https://github.com/user-attachments/assets/1e67f303-a752-40a2-8a32-c6d5fdcf3e6d)

ruff 0.5.6

---

_Label `question` added by @MichaReiser on 2024-08-08 05:53_

---

_Comment by @MichaReiser on 2024-08-08 05:57_

Ruff and isort (or Black and isort) are only compatible when using isort with `profile=black`. Full compatibility with isort is not a goal of the formatter. Maybe long-term if we decide to build isort into the formatter. 

For now, I recommend you to either use isort with [`profile=black`](https://pycqa.github.io/isort/docs/configuration/black_compatibility.html) or use [Ruff's isort](https://docs.astral.sh/ruff/rules/#isort-i) which warns you if you use an incompatible setting.


---

_Closed by @MichaReiser on 2024-08-08 05:57_

---
