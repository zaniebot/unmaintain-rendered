---
number: 12478
title: Incorrectly sorting first-party modules when referenced by full name instead of .my_module
type: issue
state: closed
author: GellertPalfi
labels: []
assignees: []
created_at: 2024-07-23T15:00:03Z
updated_at: 2024-07-24T07:08:28Z
url: https://github.com/astral-sh/ruff/issues/12478
synced_at: 2026-01-07T13:12:15-06:00
---

# Incorrectly sorting first-party modules when referenced by full name instead of .my_module

---

_Issue opened by @GellertPalfi on 2024-07-23 15:00_

Ruff seems to incorrectly sort first-party modules when they are referenced by their full name when using a config file. 
`--select "I"` seems to work correctly.

Config file:
```
[tool.ruff]
line-length = 120

[tool.ruff.lint]
extend-select = ["Q", "I"]

[tool.ruff.lint.flake8-quotes]
docstring-quotes = "double"
inline-quotes = "single"

[tool.ruff.format]
quote-style = "single"
```
Problem:
```
from airbyte.slack_hook import slack_message_on_failure  # first party module
from dagster import build_hook_context, op
from unittest import mock
``` 
Running: 
` ruff  check . --config path/to/pyproject.toml  --fix`
Output:
```
from unittest import mock

from airbyte.slack_hook import slack_message_on_failure
from dagster import build_hook_context, op 
```
VS expected:
```
from unittest import mock

from dagster import build_hook_context, op

from airbyte.slack_hook import slack_message_on_failure
```
However when running `ruff check path/to/file --select "I" --fix ` the imports are correctly sorted:
```
from airbyte.slack_hook import slack_message_on_failure
from dagster import build_hook_context, op
from unittest import mock
```
Output:
```
from unittest import mock

from dagster import build_hook_context, op

from airbyte.slack_hook import slack_message_on_failure
````

Ruff version: 0.5.4
"ruff isort", "ruff sort imports"


---

_Comment by @charliermarsh on 2024-07-23 15:07_

What is your project structure? I believe `--config` requires that we treat the current working directory as root, whereas omitting it means we use the directory containing your configuration file as root.

---

_Comment by @GellertPalfi on 2024-07-23 15:26_

Here is my project structure:
```
PROJECT-REPO/
└── setup/
    └── dagster/
        ├── airbyte/
        └── pyproject.toml
```
I used all commands from the `PROJECT-REPO` directory

---

_Comment by @charliermarsh on 2024-07-23 15:28_

Which of `dagster` and/or `airbyte` is intended to be first-party vs. third-party?

---

_Comment by @GellertPalfi on 2024-07-23 15:31_

`dagster` is third-party and `airbyte` is first-party.

---

_Comment by @GellertPalfi on 2024-07-23 15:36_

Apologies for the confusion i see, that my folder structure example was very bad.
Here is an updated version to hopefully avoid confusion. The code snippets are from script.py
```
PROJECT-REPO/
└── setup/
    └── dagster_own/
        ├── airbyte/
        │   └── slack_hook.py
        ├── scripts/
        │   └── script.py
        └── pyproject.toml
```

---

_Comment by @charliermarsh on 2024-07-23 16:00_

If you pass `--config`, you should probably run from `dagster_own`, since we use the current working directory as root if you override the configuration discovery.

---

_Comment by @GellertPalfi on 2024-07-23 16:07_

That actually solves the issue!
Could you point me to some explanation why is this happening?
Also this problem still persists with the VSC extension although im using the same config file

---

_Comment by @charliermarsh on 2024-07-23 16:08_

There's some documentation on it here, including the `--config` behavior: https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc

---

_Comment by @GellertPalfi on 2024-07-24 07:08_

Thank you, that helped a lot!

---

_Closed by @GellertPalfi on 2024-07-24 07:08_

---
