---
number: 9765
title: unit test / pytest slow down exponentially after migrating from poetry to uv
type: issue
state: open
author: parfaire
labels:
  - needs-mre
assignees: []
created_at: 2024-12-10T07:23:04Z
updated_at: 2025-07-24T08:25:32Z
url: https://github.com/astral-sh/uv/issues/9765
synced_at: 2026-01-07T13:12:18-06:00
---

# unit test / pytest slow down exponentially after migrating from poetry to uv

---

_Issue opened by @parfaire on 2024-12-10 07:23_

hi first of all would like to thank everyone hardwork to build an amazing uv package manager.

I have one project's build that significantly increased after migrating, however for some unknown reasons, the tests (`TESTING=pytest pytest -x -vv --cov=./src`) are in the opposite. Here is what I am experiencing:
## in Local Environment
```
Poetry (build: 2 mins, tests: ~1.5 mins)
UV: (build: 1 mins, tests: ~1.5 mins)
```

## in Github Actions (!!where the issues are!!)
```
Poetry (build: 4 mins, tests: ~4 mins)
UV: (build: 3 mins, tests: ~15 mins)
```

# Here is the snippets of my pyproject:
## Poetry
```
[tool.poetry.dependencies]
python = "^3.13.0"
uvicorn = ">=0.27.0"
gunicorn = "^21.2.0"
sqlalchemy = "^1.4.51"
fastapi = ">=0.109.2"
mysqlclient = ">=2.2.3"

jsql = ">=0.7"
py-spy = ">=0.3.11"
pylint = ">=2.7.1"
toml = ">=0.10.0"
requests = ">=2.31.0"
sentry-sdk = ">=1.31.0"
boltons = ">=23.0.0"
mysql-connector-python = ">=8.0.26"
google-cloud-logging = ">=3.5.0"
opentelemetry-instrumentation-fastapi = ">=0.26b1"
opentelemetry-sdk = ">=1.7.1"
opentelemetry-instrumentation-requests = ">=0.26b1"
opentelemetry-instrumentation-sqlalchemy = ">=0.26b1"
google-cloud-secret-manager = ">=2.15.1"
python-json-logger = ">=2.0.2"
google-cloud-storage = ">=2.4.0"
google-cloud-core = ">=2.3.2"
google-api-core = ">=2.11.0"
pytest-cov = ">=4.0.0"
redis = ">=3.5.3"
google-cloud-bigquery = ">=3.11.4"
jinja2 = ">=2.11.3"
markupsafe = ">=2.0.1"
httpx = ">=0.25.0"
querystring-parser = ">=1.2.4"
transitions = ">=0.9.0"
python-multipart = ">=0.0.6"
retrying = ">=1.3.4"
google-cloud-pubsub = ">=2.18.4"
xlsxwriter = ">=3.1.9"
pandas = ">=2.1.2"
openpyxl = ">=3.1.2"
alembic = ">=1.12.1"
google-cloud-tasks = ">=2.14.2"
google-cloud-spanner = ">=3.41.0"
sqlalchemy-spanner = ">=1.6.2"
pre-commit = ">=2.19.0"
sqlalchemy-views = ">=0.3.2"
temporalio = "1.1.0"
pytest-asyncio = ">=0.20.3"
openpyxl-image-loader = ">=1.0.5"
pillow = ">=10.3.0"
mo-sql-parsing = ">=10.646.24152"
google-cloud-monitoring = ">=2.21.0"
genbadge = ">=1.1.1"
coverage = ">=7.4.3"
defusedxml = "^0.7.1"
google-cloud-bigquery-storage = ">=2.25.0"
protobuf = "4.23.3"
retry = ">=0.9.2"
nltk = ">=3.9.1"
```

## UV
```
[project]
dependencies = [
    "uvicorn>=0.27.0",
    "gunicorn<22.0.0,>=21.2.0",
    "sqlalchemy<2.0.0,>=1.4.51",
    "fastapi>=0.109.2",
    "mysqlclient>=2.2.3",
    "jsql>=0.7",
    "py-spy>=0.3.11",
    "pylint>=2.7.1",
    "toml>=0.10.0",
    "requests>=2.31.0",
    "sentry-sdk>=1.31.0",
    "boltons>=23.0.0",
    "mysql-connector-python>=8.0.26",
    "google-cloud-logging>=3.5.0",
    "opentelemetry-instrumentation-fastapi>=0.26b1",
    "opentelemetry-sdk>=1.7.1",
    "opentelemetry-instrumentation-requests>=0.26b1",
    "opentelemetry-instrumentation-sqlalchemy>=0.26b1",
    "google-cloud-secret-manager>=2.15.1",
    "python-json-logger>=2.0.2",
    "google-cloud-storage>=2.4.0",
    "google-cloud-core>=2.3.2",
    "google-api-core>=2.11.0",
    "pytest-cov>=4.0.0",
    "redis>=3.5.3",
    "google-cloud-bigquery>=3.11.4",
    "jinja2>=2.11.3",
    "markupsafe>=2.0.1",
    "httpx>=0.25.0",
    "querystring-parser>=1.2.4",
    "transitions>=0.9.0",
    "python-multipart>=0.0.6",
    "retrying>=1.3.4",
    "google-cloud-pubsub>=2.18.4",
    "xlsxwriter>=3.1.9",
    "pandas>=2.1.2",
    "openpyxl>=3.1.2",
    "alembic>=1.12.1",
    "google-cloud-tasks>=2.14.2",
    "google-cloud-spanner>=3.41.0",
    "sqlalchemy-spanner>=1.6.2",
    "pre-commit>=2.19.0",
    "sqlalchemy-views>=0.3.2",
    "temporalio==1.1.0",
    "pytest-asyncio>=0.20.3",
    "openpyxl-image-loader>=1.0.5",
    "pillow>=10.3.0",
    "mo-sql-parsing>=10.646.24152",
    "google-cloud-monitoring>=2.21.0",
    "genbadge>=1.1.1",
    "coverage>=7.4.3",
    "defusedxml<1.0.0,>=0.7.1",
    "google-cloud-bigquery-storage>=2.25.0",
    "protobuf==4.23.3",
    "retry>=0.9.2",
    "nltk>=3.9.1",
]
```

which if i go line by line they are all the same

## Dockerfile
```
FROM python:3.13.0

WORKDIR /src
EXPOSE 8080

RUN apt-get update && apt-get install -y curl make jq && rm -rf /var/lib/apt/lists/*

RUN pip3 install uv==0.5.4
COPY uv.lock /src/
COPY pyproject.toml /src/
RUN uv sync

COPY . /src
RUN chmod +x /src/bin/run.sh
ENV PYTHONPATH="${PYTHONPATH}:/src/src"
ENV PATH="/src/.venv/bin:${PATH}"
CMD ["/src/bin/run.sh"]
```

I have tried to debug and change a few things but im afraid nothing ive done so far makes any improvement, so I hope someone can help/advise 🙏 

---

_Comment by @TurtleOrangina on 2024-12-10 09:02_

Hi,

to me it does not make sense that the execution of the tests would take much longer depending on the package manager used. Once the virtual environment is built, there should be nothing that either poetry or uv have to do with the execution of the tests.

Can you create a minimal reproducible example?

---

_Comment by @konstin on 2024-12-10 09:35_

Poetry and uv resolve the same dependency versions, so it's not about that. Could it be that the github actions workflow has another difference, e.g. the uv version uses a different python version?

---

_Label `needs-mre` added by @konstin on 2024-12-10 09:36_

---

_Comment by @parfaire on 2024-12-10 11:57_

> Hi,
> 
> to me it does not make sense that the execution of the tests would take much longer depending on the package manager used. Once the virtual environment is built, there should be nothing that either poetry or uv have to do with the execution of the tests.
> 
> Can you create a minimal reproducible example?

I am afraid I can't even reproduce this in my local for the exact same code and command, slow-down only happens on github action job


> Poetry and uv resolve the same dependency versions, so it's not about that. Could it be that the github actions workflow has another difference, e.g. the uv version uses a different python version?

Yes, I believe so. There is something that github action does it differently. as for python version, pytest does output the lib version before it starts testing and both are consistent between poetry and uv, however I just noticed they use different binary
`platform linux -- Python 3.13.0, pytest-8.3.3, pluggy-1.5.0 -- /src/.venv/bin/python` # uv
`platform linux -- Python 3.13.0, pytest-8.3.3, pluggy-1.5.0 -- /usr/local/bin/python3.13` # poetry
but again even in my local the binaries used are also different and it yields the same durations

I am not sure what other information I could share in order to help debugging this, please let me know I am happy to provide

---

_Comment by @zanieb on 2024-12-10 14:26_

Are you using this Docker image in CI?

---

_Comment by @parfaire on 2024-12-10 16:53_

> Are you using this Docker image in CI?

yes I am

---

_Comment by @TurtleOrangina on 2024-12-11 07:50_

> I am afraid I can't even reproduce this in my local for the exact same code and command, slow-down only happens on github action job

That would be fine, create a minimal example that reproduces the issue on github actions. A minimal repo that I can clone and run the github actions to see the issue. I know creating minimal examples is a lot of work, but I don't see a way of us "guessing" what this issue is without more concrete information.

---

_Comment by @parfaire on 2025-07-24 08:25_

Hey let me try to create something with this, however, just a quick update:

I can get around it with using `requirements.txt` instead of `pyproject.toml` which is unideal because I can't use the command like `uv add` conveniently, but it does make the tests run normally


```
....
COPY requirements.txt ./
RUN uv pip install --system -r requirements.txt
```
instead of

```
...
COPY uv.lock pyproject.toml ./
RUN uv sync
```



---
