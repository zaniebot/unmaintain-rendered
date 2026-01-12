```yaml
number: 4133
title: Universal lock of warehose takes 3s
type: issue
state: closed
author: konstin
labels:
  - performance
  - preview
assignees: []
created_at: 2024-06-07T15:41:13Z
updated_at: 2024-06-17T14:30:00Z
url: https://github.com/astral-sh/uv/issues/4133
synced_at: 2026-01-12T15:58:48Z
```

# Universal lock of warehose takes 3s

---

_@konstin_

```
$ hyperfine --warmup 1 --prepare "rm -f uv.lock" "uv lock"
Benchmark 1: uv lock
  Time (mean ± σ):      3.010 s ±  0.028 s    [User: 2.620 s, System: 0.808 s]
  Range (min … max):    2.977 s …  3.054 s    10 runs
```

`pyproject.toml`:

```toml
[project]
name = "warehouse"
version = "1.0.0"
requires-python = ">=3.11, <4"
dependencies = [
  "alembic>=0.7.0",
  "alembic-postgresql-enum",
  "Automat",
  "argon2-cffi",
  "b2sdk",
  "Babel",
  "bcrypt",
  "boto3",
  "celery[sqs]>=5.2.2,<5.3.2",
  "celery-redbeat",
  "certifi",
  "click",
  "cryptography",
  "datadog>=0.19.0",
  "disposable-email-domains",
  "elasticsearch>=7.0.0,<7.11.0",
  "elasticsearch_dsl>=7.0.0,<8.0.0",
  "first",
  "forcediphttpsadapter",
  "github-reserved-names>=1.0.0",
  "google-cloud-bigquery",
  "google-cloud-storage",
  "hiredis",
  "html5lib",
  "humanize",
  "itsdangerous",
  "Jinja2>=2.8",
  "kombu[sqs]<5.3.2", # https://github.com/jazzband/pip-tools/issues/1577
  "limits",
  "linehaul",
  "lxml",
  "mistune",
  "msgpack",
  "natsort",
  "orjson",
  "packaging>=23.2",
  "packaging_legacy",
  "paginate>=0.5.2",
  "paginate_sqlalchemy",
  "passlib>=1.6.4",
  "premailer",
  "psycopg[c]",
  "pycurl",
  "pydantic",
  "pyqrcode",
  "pyramid>=2.0",
  "pymacaroons",
  "pyramid_jinja2>=2.5",
  "pyramid_mailer>=0.14.1",
  "pyramid_openapi3>=0.17.1",
  "pyramid_retry>=0.3",
  "pyramid_rpc>=0.7",
  "pyramid_services>=2.1",
  "pyramid_tm>=0.12",
  "python-slugify",
  "pytz",
  "PyJWT[crypto]>=2.8.0",
  "readme-renderer[md]>=36.0",
  "requests",
  "requests-aws4auth",
  "redis>=2.8.0,<6.0.0",
  "rfc3986",
  "sentry-sdk",
  "setuptools",
  "sqlalchemy[asyncio]>=2.0,<3.0",
  "stdlib-list",
  "stripe",
  "structlog",
  "transaction",
  "trove-classifiers",
  "ua-parser",
  "urllib3<2",  # See https://github.com/pypi/warehouse/issues/14671,
  "webauthn>=1.0.0,<3.0.0",
  "whitenoise",
  "WTForms[email]>=2.0.0",
  "zope.sqlalchemy",
  "zxcvbn",
]

[project.optional-dependencies]
deploy = [
  "gunicorn==22.0.0",
  "ddtrace==2.8.5"
]
```

Flamegraph and profile.json for the firefox profiler:

![image](https://github.com/astral-sh/uv/assets/6826232/be411072-4b3a-40fb-9307-643b6cc6fcd2)

[profile.json](https://github.com/user-attachments/files/15741922/profile.json)


---

_Comment by @charliermarsh on 2024-06-07 15:51_

Wow, so much time spent in cloning and union-ing the resolver states!

---

_Label `performance` added by @charliermarsh on 2024-06-07 15:54_

---

_Comment by @charliermarsh on 2024-06-07 15:54_

I bet we can dramatically improve this, very cool.

---

_Label `preview` added by @konstin on 2024-06-07 16:02_

---

_Comment by @BurntSushi on 2024-06-07 16:27_

It's also possible #4135 will help here, assuming there are superfluous forks. (I haven't looked.)

Otherwise, I agree that there are likely low hanging fruits. I've generally operated under the assumption that "forking is rare," and so haven't given a ton of thought to performance. Which usually means there are big gains to be had. :)

---

_Comment by @charliermarsh on 2024-06-07 18:04_

Yup not urgent!

---

_Comment by @BurntSushi on 2024-06-17 14:28_

I looked into this and it looks like I was exactly right: #4135 with `Requires-Python` filtering has resolved this particular problem. Without the filtering, `uv` was doing way more forking than necessary.

As a bonus, I converted the `pyproject.toml` to work with Poetry and included it in the benchmark below. Here's the `pyproject.toml` I used:

```toml
[tool.poetry]
name = "project"
version = "0.1.0"
description = ""
authors = ["Andrew Gallant <jamslam@gmail.com>"]

[project]
name = "warehouse"
version = "1.0.0"
requires-python = ">=3.11, <4"
dependencies = [
  "alembic>=0.7.0",
  "alembic-postgresql-enum",
  "Automat",
  "argon2-cffi",
  "b2sdk",
  "Babel",
  "bcrypt",
  "boto3",
  "celery[sqs]>=5.2.2,<5.3.2",
  "celery-redbeat",
  "certifi",
  "click",
  "cryptography",
  "datadog>=0.19.0",
  "disposable-email-domains",
  "elasticsearch>=7.0.0,<7.11.0",
  "elasticsearch_dsl>=7.0.0,<8.0.0",
  "first",
  "forcediphttpsadapter",
  "github-reserved-names>=1.0.0",
  "google-cloud-bigquery",
  "google-cloud-storage",
  "hiredis",
  "html5lib",
  "humanize",
  "itsdangerous",
  "Jinja2>=2.8",
  "kombu[sqs]<5.3.2", # https://github.com/jazzband/pip-tools/issues/1577
  "limits",
  "linehaul",
  "lxml",
  "mistune",
  "msgpack",
  "natsort",
  "orjson",
  "packaging>=23.2",
  "packaging_legacy",
  "paginate>=0.5.2",
  "paginate_sqlalchemy",
  "passlib>=1.6.4",
  "premailer",
  "psycopg[c]",
  "pycurl",
  "pydantic",
  "pyqrcode",
  "pyramid>=2.0",
  "pymacaroons",
  "pyramid_jinja2>=2.5",
  "pyramid_mailer>=0.14.1",
  "pyramid_openapi3>=0.17.1",
  "pyramid_retry>=0.3",
  "pyramid_rpc>=0.7",
  "pyramid_services>=2.1",
  "pyramid_tm>=0.12",
  "python-slugify",
  "pytz",
  "PyJWT[crypto]>=2.8.0",
  "readme-renderer[md]>=36.0",
  "requests",
  "requests-aws4auth",
  "redis>=2.8.0,<6.0.0",
  "rfc3986",
  "sentry-sdk",
  "setuptools",
  "sqlalchemy[asyncio]>=2.0,<3.0",
  "stdlib-list",
  "stripe",
  "structlog",
  "transaction",
  "trove-classifiers",
  "ua-parser",
  "urllib3<2",  # See https://github.com/pypi/warehouse/issues/14671,
  "webauthn>=1.0.0,<3.0.0",
  "whitenoise",
  "WTForms[email]>=2.0.0",
  "zope.sqlalchemy",
  "zxcvbn",
]

[project.optional-dependencies]
deploy = [
  "gunicorn==22.0.0",
  "ddtrace==2.8.5"
]

[tool.poetry.dependencies]
python = ">=3.11, <4"
alembic = ">=0.7.0"
alembic-postgresql-enum = "*"
Automat = "*"
argon2-cffi = "*"
b2sdk = "*"
Babel = "*"
bcrypt = "*"
boto3 = "*"
celery = { version = ">=5.2.2,<5.3.2", extras = ["sqs"] }
celery-redbeat = "*"
certifi = "*"
click = "*"
cryptography = "*"
datadog = ">=0.19.0"
disposable-email-domains = "*"
elasticsearch = ">=7.0.0,<7.11.0"
elasticsearch_dsl = ">=7.0.0,<8.0.0"
first = "*"
forcediphttpsadapter = "*"
github-reserved-names = ">=1.0.0"
google-cloud-bigquery = "*"
google-cloud-storage = "*"
hiredis = "*"
html5lib = "*"
humanize = "*"
itsdangerous = "*"
Jinja2 = ">=2.8"
kombu = { version = "<5.3.2", extras = ["sqs"] } # https://github.com/jazzband/pip-tools/issues/1577
limits = "*"
linehaul = "*"
lxml = "*"
mistune = "*"
msgpack = "*"
natsort = "*"
orjson = "*"
packaging = ">=23.2"
packaging_legacy = "*"
paginate = ">=0.5.2"
paginate_sqlalchemy = "*"
passlib = ">=1.6.4"
premailer = "*"
psycopg = { version = "*", extras = ["c"] }
pycurl = "*"
pydantic = "*"
pyqrcode = "*"
pyramid = ">=2.0"
pymacaroons = "*"
pyramid_jinja2 = ">=2.5"
pyramid_mailer = ">=0.14.1"
pyramid_openapi3 = ">=0.17.1"
pyramid_retry = ">=0.3"
pyramid_rpc = ">=0.7"
pyramid_services = ">=2.1"
pyramid_tm = ">=0.12"
python-slugify = "*"
pytz = "*"
PyJWT = { version = ">=2.8.0", extras = ["crypto"] }
readme-renderer = { version = "36.0", extras = ["md"] }
requests = "*"
requests-aws4auth = "*"
redis = ">=2.8.0,<6.0.0"
rfc3986 = "*"
sentry-sdk = "*"
setuptools = "*"
sqlalchemy = { version = ">=2.0,<3.0", extras = ["asyncio"] }
stdlib-list = "*"
stripe = "*"
structlog = "*"
transaction = "*"
trove-classifiers = "*"
ua-parser = "*"
urllib3 = "<2"  # See https://github.com/pypi/warehouse/issues/14671,
webauthn = ">=1.0.0,<3.0.0"
whitenoise = "*"
WTForms = { version = ">=2.0.0", extras = ["email"] }
"zope.sqlalchemy" = "*"
zxcvbn = "*"

[tool.poetry.group.deploy]
optional = true

[tool.poetry.group.deploy.dependencies]
gunicorn = "==22.0.0"
ddtrace = "==2.8.5"
```

And now hyperfine:

```
$ hyperfine --warmup 1 --prepare "rm -f uv.lock poetry.lock" \
    "uv-main lock" \
    "uv-before-requires-python lock" \
    "uv-before-disjoint-filtering lock" \
    "poetry lock"
Benchmark 1: uv-main lock
  Time (mean ± σ):      88.5 ms ±   7.1 ms    [User: 70.2 ms, System: 93.5 ms]
  Range (min … max):    78.5 ms … 104.1 ms    29 runs

Benchmark 2: uv-before-requires-python lock
  Time (mean ± σ):      1.810 s ±  0.019 s    [User: 1.968 s, System: 0.307 s]
  Range (min … max):    1.786 s …  1.847 s    10 runs

Benchmark 3: uv-before-disjoint-filtering lock
  Time (mean ± σ):      87.5 ms ±   6.8 ms    [User: 74.7 ms, System: 88.6 ms]
  Range (min … max):    77.9 ms …  98.5 ms    31 runs

Benchmark 4: poetry lock
  Time (mean ± σ):      3.263 s ±  0.021 s    [User: 3.139 s, System: 0.119 s]
  Range (min … max):    3.237 s …  3.299 s    10 runs

Summary
  uv-before-disjoint-filtering lock ran
    1.01 ± 0.11 times faster than uv-main lock
   20.68 ± 1.63 times faster than uv-before-requires-python lock
   37.29 ± 2.92 times faster than poetry lock
```

---

_Closed by @BurntSushi on 2024-06-17 14:28_

---
