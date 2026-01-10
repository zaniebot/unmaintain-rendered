---
number: 13272
title: Ruff format attempts to format git related files
type: issue
state: closed
author: naquiroz
labels:
  - question
  - needs-info
assignees: []
created_at: 2024-09-06T20:00:56Z
updated_at: 2024-10-04T17:30:45Z
url: https://github.com/astral-sh/ruff/issues/13272
synced_at: 2026-01-10T01:22:53Z
---

# Ruff format attempts to format git related files

---

_Issue opened by @naquiroz on 2024-09-06 20:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Keywords searched: `git`

When creating a branch with a name that ends in `.py`, ruff attempts so format it.

![CleanShot 2024-09-06 at 15 58 11](https://github.com/user-attachments/assets/c9b7b28d-d003-4230-9a4a-86a9ae6d6c13)

## Steps to reproduce

1. Create a branch in git with a name like `my-file.py`
2. Run the ruff formatter.
3. See how it tries to format a git file.


---

_Renamed from "Ruf format attempts to format git related files." to "Ruf format attempts to format git related files" by @naquiroz on 2024-09-06 20:01_

---

_Comment by @MichaReiser on 2024-09-06 20:40_

Hehe that's funny. Would you mind sharing your configuration? I wonder if [respect-gitignore](https://docs.astral.sh/ruff/settings/) is on or off

---

_Renamed from "Ruf format attempts to format git related files" to "Ruff format attempts to format git related files" by @naquiroz on 2024-09-06 21:35_

---

_Comment by @naquiroz on 2024-09-06 21:35_

How do I get my configuration?

---

_Comment by @MichaReiser on 2024-09-07 12:42_

Do you have a `pyproject.toml` with a `[tool.ruff]` section or a `ruff.toml` in your project? See [How to configure Ruff](https://docs.astral.sh/ruff/configuration/)

---

_Label `question` added by @MichaReiser on 2024-09-09 11:59_

---

_Comment by @Avasam on 2024-09-15 17:31_

Oh yeah I remember this happenning to me once, but forgot to report. I don't have `respect-gitignore` set in any of my projects.

---

_Comment by @MichaReiser on 2024-09-16 08:23_

I verified this locally and I can only reproduce this behavior when overriding the [`exclude`](https://docs.astral.sh/ruff/settings/#exclude) setting. 

Ruff ignores the `.gitignore` directory by default because it is part of the default `exclude` list. You may want to consider using `extend-exclude` if you want to exclude some directories in addition to the default once. 



---

_Label `needs-info` added by @MichaReiser on 2024-09-16 08:23_

---

_Comment by @naquiroz on 2024-09-17 13:09_

I'm sharing my `pyproject.toml`

I'll also take a look into the `extend-exclude`

```toml
[tool.poetry]
name = "redacted"
version = "0.1.0"
description = "Backend"
package-mode = false

[tool.poetry.dependencies]
python = "3.11.7"
babel = "2.13.1"
boto3 = "1.28.85"
django = "4.2.11"
django-model-utils = "4.3.1"
djangorestframework = "3.14.0"
django-extensions = "^3.2.3"
django-cors-headers = "^4.3.0"
djangorestframework-simplejwt = "^5.3.0"
drf-yasg = "1.21.7"
faker = "20.0.0"
fastapi = "0.104.1"
loguru = "0.7.2"
pandas = "2.2.0"
pydantic = "2.5.0"
pyjwt = "2.8.0"
python-dotenv = "1.0.0"
pytz = "2023.3"
redis = "5.0.1"
requests = "2.31.0"
rest-framework-simplejwt = "0.0.2"
sentry-sdk = "1.35.0"
xlsxwriter = "3.1.9"
openpyxl = "^3.1.2"
psycopg2-binary = "^2.9.9"
pandas-market-calendars = "^4.3.2"
resend = "^0.6.0"
heyoo = "^0.1.0"
pyparsing = "^3.1.1"
pydot = "^2.0.0"
holidays = "^0.40"
textdistance = "^4.6.1"
factory-boy = "^3.3.0"
factories = "^1.2.0"
redgreenunittest = "^0.1.1"
slack-sdk = "^3.27.1"
django-money = "^3.4.1"
whitenoise = "^6.6.0"
xstate-machine = "^3.1.2"
moto = "^5.0.4"
python-decouple = "^3.8"
flower = "^2.0.1"
python-dateutil = "^2.9.0.post0"
django-simple-history = "^3.0.0"
drf-extensions = "^0.7.1"
json-log-formatter = "^1.0"
unidecode = "^1.3.8"
celery = {version = "~5.4.0", extras = ["pytest"]}
django-axes = "^6.5.0"
django-localflavor = "^4.0"
django-countries = "^7.6.1"
uvicorn = "^0.30.1"
gunicorn = "^22.0.0"
xhtml2pdf = "^0.2.16"
jinja2 = "^3.1.4"
celery-types = "^0.22.0"
posthog= "^3.5.0"
google-cloud-logging = "^3.10.0"
phonenumbers = "^8.13.42"
django-silk = "^5.1.0"
django-autocomplete-light = "^3.11.0"
google-cloud-secret-manager = "^2.20.2"
django-filter = "^24.3"
pymupdf = "1.24.9"
djangorestframework-api-key = "^3.0.0"

[tool.poetry.group.dev.dependencies]
ruff = "^0.4.8"
coverage = "^7.5.3"
pytest = "^8.2.2"
pytest-django = "^4.8.0"
pytest-celery = {extras = ["all"], version = "^1.0.0"}
pytest-split = "^0.9.0"
pytest-xdist = {extras = ["psutil"], version = "^3.6.1"}
pytest-cov = "^5.0.0"
django-stubs = "^5.0.4"
djangorestframework-stubs = "^3.15.0"

[tool.ruff]

exclude = ["**/migrations/**.py"]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "B", "I"]

exclude = ["**/migrations/**.py", ".venv/**.py"]

[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]


[tool.ruff.format]
exclude = ["**/migrations/**.py", ".venv/**.py"]


[tool.pytest.ini_options]
DJANGO_SETTINGS_MODULE = "service.settings"
addopts = "--import-mode=importlib --reuse-db"
filterwarnings = [
    "ignore:DateTimeField.*received a naive datetime.*:RuntimeWarning",
    "ignore::DeprecationWarning"
]


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

[tool.coverage.run]
omit = ["**/migrations/**", ".venv/**", "**/tests/**"]
```


---

_Comment by @MichaReiser on 2024-09-17 13:15_

@naquiroz thanks for sharing the config.  I think you want to use `tool.ruff.extend-include` instead of `tool.ruff.exclude`. I think that should fix the problem for you.

---

_Closed by @MichaReiser on 2024-09-22 07:32_

---

_Comment by @Avasam on 2024-10-04 17:25_

edit: make sure you don't have `exclude` in your command either ^^"

---
