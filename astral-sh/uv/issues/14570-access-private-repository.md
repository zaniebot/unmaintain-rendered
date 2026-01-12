```yaml
number: 14570
title: Access private repository
type: issue
state: closed
author: jairhenrique
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-07-11T20:36:01Z
updated_at: 2025-07-13T22:56:50Z
url: https://github.com/astral-sh/uv/issues/14570
synced_at: 2026-01-12T16:01:51Z
```

# Access private repository

---

_@jairhenrique_

### Summary

We're encountering an issue in our CI pipeline after updating uv to version 0.7.20. Our GitHub Actions workflow has separate steps for installing dependencies and linting the code.

Here's how our relevant steps are configured:


```yaml
      - name: Install uv
        uses: astral-sh/setup-uv@v6
        with:
          version: "latest"
          enable-cache: true
          cache-dependency-glob: "uv.lock"
          prune-cache: false
        
       - name: Install Dependencies
        run: make dev-dependencies
        env:
          UV_INDEX_PRIVATE_USERNAME: my_private_user
          UV_INDEX_PRIVATE_PASSWORD: ${{ secrets.MY_PRIVATE_PAT }}
        
        - name: Lint
          run: make lint 
          # this make lint runs `uv run ruff check .` and other stuffs
```

The `Install Dependencies` step successfully installs all dependencies, including those from a private repository, by using the `UV_INDEX_PRIVATE_USERNAME` and `UV_INDEX_PRIVATE_PASSWORD` environment variables.

However, since the update to `uv 0.7.20`, the Lint step has started failing. The error indicates that it's trying to access the private dependency repository and is missing the necessary environment variables. This is unexpected because the `Lint` step should only be running linters (like `ruff check .`) on the already installed dependencies and code, not attempting to re-access or re-resolve private packages.

It seems that uv is now attempting to resolve dependencies again during the `uv run` command in the `Lint` step, even though they were already installed in a previous step. This behavior was not present in previous `uv` versions.

### Platform

Linux

### Version

uv 0.7.20

### Python version

Python 3.13

---

_Label `bug` added by @jairhenrique on 2025-07-11 20:36_

---

_Comment by @charliermarsh on 2025-07-12 02:20_

Can you try running `uv run --frozen --verbose ruff check .` instead of `uv run ruff check .`, and sharing the verbose logs? (Without a minimal reproduction, we'll probably need at least the full logs to help you.)

---

_Label `needs-mre` added by @charliermarsh on 2025-07-12 02:20_

---

_Comment by @jairhenrique on 2025-07-12 18:11_

@charliermarsh When I execute the command with the `--frozen` parameter, the problem does not occur.

I'm adding the output of the command without `--frozen` below.

<details><summary>OUTPUT</summary>

```
DEBUG uv 0.7.20
DEBUG Found project root: `MY_PROJECT_PATH`
DEBUG No workspace root found, using project root
DEBUG Discovered project `MY_PROJECT` at: MY_PROJECT_PATH
DEBUG Acquired lock for `MY_PROJECT_PATH`
DEBUG Reading Python requests from version file at `MY_PROJECT_PATH/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `/tmp/uv-e84e9978423f4259.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: MY_PROJECT @ file://MY_PROJECT_PATH
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `MY_PROJECT==2.11.0`
  Requested: {Requirement { name: PackageName("configcat-client"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "9.0.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("confluent-kafka"), extras: [ExtraName("avro")], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.11.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("cryptography"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "45.0.5" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "5.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django-dramatiq"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.13.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django-environ"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.12.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("djangorestframework"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.16.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("djangorestframework-camel-case"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.4.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("dramatiq"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.18.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("MY_PRIVATE_LIB"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.0.6" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/gboticario/_packaging/BoticarioFeed/pypi/simple/", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }, origin: None }, Requirement { name: PackageName("dumb-init"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.2.5.post1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("elasticsearch"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "8.18.1" }, VersionSpecifier { operator: LessThan, version: "9" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("gunicorn"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "23.0.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("httpx"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.28.1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("newrelic"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "10.14.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pid"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.0.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pika"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.3.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("psycopg"), extras: [ExtraName("binary"), ExtraName("pool")], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.2.9" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pydantic"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.11.7" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pydantic-settings"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.10.1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("structlog"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "25.4.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("tzdata"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2025.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("uvicorn"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.35.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("uvloop"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.21.0" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("configcat-client"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "9.0.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("confluent-kafka"), extras: [ExtraName("avro")], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.11.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("cryptography"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "45.0.5" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "5.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django-dramatiq"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.13.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("django-environ"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.12.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("djangorestframework"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.16.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("djangorestframework-camel-case"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.4.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("dramatiq"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.18.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("MY_PRIVATE_LIB"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.0.6" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/gboticario/_packaging/BoticarioFeed/pypi/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }, origin: None }, Requirement { name: PackageName("dumb-init"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.2.5.post1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("elasticsearch"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "8.18.1" }, VersionSpecifier { operator: LessThan, version: "9" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("gunicorn"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "23.0.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("httpx"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.28.1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("newrelic"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "10.14.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pid"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.0.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pika"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.3.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("psycopg"), extras: [ExtraName("binary"), ExtraName("pool")], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.2.9" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pydantic"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.11.7" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("pydantic-settings"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.10.1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("structlog"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "25.4.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("tzdata"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2025.2" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("uvicorn"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.35.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("uvloop"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.21.0" }]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13
DEBUG Solving split (python_full_version >= '3.13' and platform_python_implementation != 'PyPy') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.13")), UpperBound(Unbounded)) })
DEBUG Adding direct dependency: MY_PROJECT*
DEBUG Adding direct dependency: MY_PROJECT:dev*
DEBUG Searching for a compatible version of MY_PROJECT @ file://MY_PROJECT_PATH (*)
DEBUG Adding direct dependency: configcat-client>=9.0.4, <9.0.4+
DEBUG Adding direct dependency: confluent-kafka[avro]>=2.11.0, <2.11.0+
DEBUG Adding direct dependency: cryptography>=45.0.5, <45.0.5+
DEBUG Adding direct dependency: django>=5.2.4, <5.2.4+
DEBUG Adding direct dependency: django-dramatiq>=0.13.0, <0.13.0+
DEBUG Adding direct dependency: django-environ>=0.12.0, <0.12.0+
DEBUG Adding direct dependency: djangorestframework>=3.16.0, <3.16.0+
DEBUG Adding direct dependency: djangorestframework-camel-case>=1.4.2, <1.4.2+
DEBUG Adding direct dependency: dramatiq>=1.18.0, <1.18.0+
DEBUG Adding direct dependency: MY_PRIVATE_LIB>=1.0.6, <1.0.6+
DEBUG Adding direct dependency: dumb-init>=1.2.5.post1, <1.2.5.post1+
DEBUG Adding direct dependency: elasticsearch>=8.18.1, <9
DEBUG Adding direct dependency: gunicorn>=23.0.0, <23.0.0+
DEBUG Adding direct dependency: httpx>=0.28.1, <0.28.1+
DEBUG Adding direct dependency: newrelic>=10.14.0, <10.14.0+
DEBUG Adding direct dependency: pid>=3.0.4, <3.0.4+
DEBUG Adding direct dependency: pika>=1.3.2, <1.3.2+
DEBUG Adding direct dependency: psycopg[binary]>=3.2.9, <3.2.9+
DEBUG Adding direct dependency: psycopg[pool]>=3.2.9, <3.2.9+
DEBUG Adding direct dependency: pydantic>=2.11.7, <2.11.7+
DEBUG Adding direct dependency: pydantic-settings>=2.10.1, <2.10.1+
DEBUG Adding direct dependency: structlog>=25.4.0, <25.4.0+
DEBUG Adding direct dependency: tzdata>=2025.2, <2025.2+
DEBUG Adding direct dependency: uvicorn>=0.35.0, <0.35.0+
DEBUG Adding direct dependency: uvloop>=0.21.0, <0.21.0+
DEBUG Searching for a compatible version of MY_PROJECT @ file://MY_PROJECT_PATH (*)
DEBUG Adding direct dependency: MY_PROJECT:dev==2.11.0
DEBUG Searching for a compatible version of MY_PROJECT @ file://MY_PROJECT_PATH (==2.11.0)
DEBUG Adding direct dependency: deepdiff>=8.5.0, <8.5.0+
DEBUG Adding direct dependency: freezegun>=1.5.2
DEBUG Adding direct dependency: ipython>=9.4.0, <9.4.0+
DEBUG Adding direct dependency: locust>=2.37.11, <2.37.11+
DEBUG Adding direct dependency: model-bakery>=1.20.5, <1.20.5+
DEBUG Adding direct dependency: mypy>=1.16.1, <1.16.1+
DEBUG Adding direct dependency: pdbpp>=0.11.6, <0.11.6+
DEBUG Adding direct dependency: pytest>=8.4.1, <8.4.1+
DEBUG Adding direct dependency: pytest-asyncio>=1.0.0, <1.0.0+
DEBUG Adding direct dependency: pytest-cov>=6.2.1, <6.2.1+
DEBUG Adding direct dependency: pytest-deadfixtures>=2.2.1, <2.2.1+
DEBUG Adding direct dependency: pytest-django>=4.11.1, <4.11.1+
DEBUG Adding direct dependency: pytest-dotenv>=0.5.2, <0.5.2+
DEBUG Adding direct dependency: pytest-env>=1.1.5, <1.1.5+
DEBUG Adding direct dependency: pytest-socket>=0.7.0, <0.7.0+
DEBUG Adding direct dependency: pytest-vcr>=1.0.2, <1.0.2+
DEBUG Adding direct dependency: ruff>=0.12.2, <0.12.2+
DEBUG Found stale response for: https://pypi.org/simple/configcat-client/
DEBUG Sending revalidation request for: https://pypi.org/simple/configcat-client/
DEBUG Found stale response for: https://pypi.org/simple/django-dramatiq/
DEBUG Sending revalidation request for: https://pypi.org/simple/django-dramatiq/
DEBUG Found stale response for: https://pypi.org/simple/django/
DEBUG Sending revalidation request for: https://pypi.org/simple/django/
DEBUG Found stale response for: https://pypi.org/simple/djangorestframework-camel-case/
DEBUG Sending revalidation request for: https://pypi.org/simple/djangorestframework-camel-case/
DEBUG Found stale response for: https://pypi.org/simple/djangorestframework/
DEBUG Sending revalidation request for: https://pypi.org/simple/djangorestframework/
DEBUG Found stale response for: https://pypi.org/simple/django-environ/
DEBUG Sending revalidation request for: https://pypi.org/simple/django-environ/
DEBUG Found stale response for: https://pypi.org/simple/confluent-kafka/
DEBUG Sending revalidation request for: https://pypi.org/simple/confluent-kafka/
DEBUG No cache entry for: https://pkgs.dev.azure.com/gboticario/_packaging/BoticarioFeed/pypi/simple/MY_PRIVATE_LIB/
DEBUG Found stale response for: https://pypi.org/simple/dramatiq/
DEBUG Sending revalidation request for: https://pypi.org/simple/dramatiq/
DEBUG Found stale response for: https://pypi.org/simple/pika/
DEBUG Sending revalidation request for: https://pypi.org/simple/pika/
DEBUG Found stale response for: https://pypi.org/simple/psycopg/
DEBUG Sending revalidation request for: https://pypi.org/simple/psycopg/
DEBUG Found stale response for: https://pypi.org/simple/gunicorn/
DEBUG Sending revalidation request for: https://pypi.org/simple/gunicorn/
DEBUG Found stale response for: https://pypi.org/simple/pydantic-settings/
DEBUG Sending revalidation request for: https://pypi.org/simple/pydantic-settings/
DEBUG Found stale response for: https://pypi.org/simple/structlog/
DEBUG Sending revalidation request for: https://pypi.org/simple/structlog/
DEBUG Found stale response for: https://pypi.org/simple/pid/
DEBUG Sending revalidation request for: https://pypi.org/simple/pid/
DEBUG Found stale response for: https://pypi.org/simple/tzdata/
DEBUG Sending revalidation request for: https://pypi.org/simple/tzdata/
DEBUG Found stale response for: https://pypi.org/simple/dumb-init/
DEBUG Sending revalidation request for: https://pypi.org/simple/dumb-init/
DEBUG Found stale response for: https://pypi.org/simple/elasticsearch/
DEBUG Sending revalidation request for: https://pypi.org/simple/elasticsearch/
DEBUG Found stale response for: https://pypi.org/simple/uvicorn/
DEBUG Sending revalidation request for: https://pypi.org/simple/uvicorn/
DEBUG Found stale response for: https://pypi.org/simple/cryptography/
DEBUG Sending revalidation request for: https://pypi.org/simple/cryptography/
DEBUG Found stale response for: https://pypi.org/simple/httpx/
DEBUG Sending revalidation request for: https://pypi.org/simple/httpx/
DEBUG Found stale response for: https://pypi.org/simple/newrelic/
DEBUG Sending revalidation request for: https://pypi.org/simple/newrelic/
DEBUG Found stale response for: https://pypi.org/simple/pydantic/
DEBUG Sending revalidation request for: https://pypi.org/simple/pydantic/
DEBUG Found stale response for: https://pypi.org/simple/uvloop/
DEBUG Sending revalidation request for: https://pypi.org/simple/uvloop/
DEBUG Found stale response for: https://pypi.org/simple/deepdiff/
DEBUG Sending revalidation request for: https://pypi.org/simple/deepdiff/
DEBUG Found stale response for: https://pypi.org/simple/freezegun/
DEBUG Sending revalidation request for: https://pypi.org/simple/freezegun/
DEBUG Found stale response for: https://pypi.org/simple/ipython/
DEBUG Sending revalidation request for: https://pypi.org/simple/ipython/
DEBUG Found stale response for: https://pypi.org/simple/pytest-cov/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-cov/
DEBUG Found stale response for: https://pypi.org/simple/pdbpp/
DEBUG Sending revalidation request for: https://pypi.org/simple/pdbpp/
DEBUG Found stale response for: https://pypi.org/simple/pytest-asyncio/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-asyncio/
DEBUG Found stale response for: https://pypi.org/simple/model-bakery/
DEBUG Sending revalidation request for: https://pypi.org/simple/model-bakery/
DEBUG Found stale response for: https://pypi.org/simple/pytest-deadfixtures/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-deadfixtures/
DEBUG Found stale response for: https://pypi.org/simple/pytest-dotenv/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-dotenv/
DEBUG Found stale response for: https://pypi.org/simple/pytest-env/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-env/
DEBUG Found stale response for: https://pypi.org/simple/pytest-socket/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-socket/
DEBUG Found stale response for: https://pypi.org/simple/pytest-vcr/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-vcr/
DEBUG Found stale response for: https://pypi.org/simple/pytest-django/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest-django/
DEBUG Found stale response for: https://pypi.org/simple/locust/
DEBUG Sending revalidation request for: https://pypi.org/simple/locust/
DEBUG Found stale response for: https://pypi.org/simple/pytest/
DEBUG Sending revalidation request for: https://pypi.org/simple/pytest/
DEBUG Found stale response for: https://pypi.org/simple/mypy/
DEBUG Sending revalidation request for: https://pypi.org/simple/mypy/
DEBUG Found stale response for: https://pypi.org/simple/ruff/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruff/
DEBUG Found not-modified response for: https://pypi.org/simple/uvicorn/
DEBUG Found not-modified response for: https://pypi.org/simple/httpx/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest/
DEBUG Found not-modified response for: https://pypi.org/simple/mypy/
DEBUG Found not-modified response for: https://pypi.org/simple/pdbpp/
DEBUG Found not-modified response for: https://pypi.org/simple/pydantic-settings/
DEBUG Found not-modified response for: https://pypi.org/simple/cryptography/
DEBUG Found not-modified response for: https://pypi.org/simple/django-environ/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-cov/
DEBUG Found not-modified response for: https://pypi.org/simple/pika/
DEBUG Found not-modified response for: https://pypi.org/simple/django/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d2/e2/dc81b1bd1dcfe91735810265e9d26bc8ec5da45b4c0f6237e286819194c3/uvicorn-0.35.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/29/16/c8a903f4c4dffe7a12843191437d7cd8e32751d5de349d45d3fe69544e87/pytest-8.4.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c5/e1/77fa5c45fcd95a80b0d793b604c19b71c9fa8d5b27d403eced79f3842828/pdbpp-0.11.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/f0/427018098906416f580e3cf1366d3b1abfb408a0652e9f31600c24a1903c/pydantic_settings-2.10.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/e3/96964af4a75a949e67df4b95318fe2b7427ac8189bbc3ef28f92a1c5bc56/mypy-1.16.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/83/b3/0a3bec4ecbfee960f39b1842c2f91e4754251e0a6ed443db9fe3f666ba8f/django_environ-0.12.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bc/16/4ea354101abb1287856baa4af2732be351c7bee728065aed451b678153fd/pytest_cov-6.2.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/f3/f412836ec714d36f0f4ab581b84c491e3f42c6b5b97a6c6ed1817f3c16d0/pika-1.3.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f0/fb/09e28bc0c46d2c547085e60897fea96310574c70fb21cd58a730a45f3403/cryptography-45.0.5-cp311-abi3-macosx_10_9_universal2.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/14/ae/706965237a672434c8b520e89a818e8b047af94e9beb342d0bee405c26c7/django-5.2.4-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-asyncio/
DEBUG Found not-modified response for: https://pypi.org/simple/elasticsearch/
DEBUG Found not-modified response for: https://pypi.org/simple/freezegun/
DEBUG Found not-modified response for: https://pypi.org/simple/ipython/
DEBUG Found not-modified response for: https://pypi.org/simple/psycopg/
DEBUG Found not-modified response for: https://pypi.org/simple/uvloop/
DEBUG Found not-modified response for: https://pypi.org/simple/pydantic/
DEBUG Found not-modified response for: https://pypi.org/simple/tzdata/
DEBUG Found not-modified response for: https://pypi.org/simple/confluent-kafka/
DEBUG Found not-modified response for: https://pypi.org/simple/dumb-init/
DEBUG Found not-modified response for: https://pypi.org/simple/gunicorn/
DEBUG Found not-modified response for: https://pypi.org/simple/pid/
DEBUG Found not-modified response for: https://pypi.org/simple/newrelic/
DEBUG Found not-modified response for: https://pypi.org/simple/structlog/
DEBUG Found not-modified response for: https://pypi.org/simple/djangorestframework/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-vcr/
DEBUG Found not-modified response for: https://pypi.org/simple/deepdiff/
DEBUG Found not-modified response for: https://pypi.org/simple/dramatiq/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-deadfixtures/
DEBUG Found not-modified response for: https://pypi.org/simple/locust/
DEBUG Found not-modified response for: https://pypi.org/simple/configcat-client/
DEBUG Found not-modified response for: https://pypi.org/simple/model-bakery/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-socket/
DEBUG Found not-modified response for: https://pypi.org/simple/django-dramatiq/
DEBUG Found not-modified response for: https://pypi.org/simple/djangorestframework-camel-case/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-env/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-dotenv/
DEBUG Found not-modified response for: https://pypi.org/simple/pytest-django/
DEBUG Found not-modified response for: https://pypi.org/simple/ruff/
DEBUG Searching for a compatible version of configcat-client (>=9.0.4, <9.0.4+)
DEBUG Selecting: configcat-client==9.0.4 [preference] (configcat_client-9.0.4-py2.py3-none-any.whl)
DEBUG Acquired lock for `/home/runner/.cache/uv/sdists-v9/pypi/djangorestframework-camel-case/1.4.2`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/30/05/ce271016e351fddc8399e546f6e23761967ee09c8c568bbfbecb0c150171/pytest_asyncio-1.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b5/b2/68d4c9b6431121b6b6aa5e04a153cac41dcacc79600ed6e2e7c3382156f5/freezegun-1.5.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/33/62/f62e8a5c7c6f7b27481c9ffc248fb32078ad88878aa4f3731a83a14cc797/elasticsearch-8.18.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/63/f8/0031ee2b906a15a33d6bfc12dd09c3dfa966b3cb5b284ecfb7549e6ac3c4/ipython-9.4.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3f/8d/2cbef610ca21539f0f36e2b34da49302029e7c9f09acef0b1c3b5839412b/uvloop-0.21.0-cp313-cp313-macosx_10_13_universal2.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6a/c0/ec2b1c8712ca690e5d61979dee872603e92b8a32f94cc1b72d53beab008a/pydantic-2.11.7-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/23/c7abc0ca0a1526a0774eca151daeb8de62ec457e77262b66b359c3c7679e/tzdata-2025.2-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/80/9f/04e2f23d636acdabcb62c623467f37ec672ee77d301fa2e98d6abb0dc763/dumb_init-1.2.5.post1-py2.py3-none-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/7d/6dac2a6e1eba33ee43f318edbed4ff29151a49b5d37f080aad1e6469bca4/gunicorn-23.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e0/30/7ebdc66dff1611756d4b38340effdb470aa2693c8789a8fef0bd8dd9332a/pid-3.0.4-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/4a/97ee6973e3a73c74c8120d59829c3861ea52210667ec3e7a16045c62b64d/structlog-25.4.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/eb/3e/2448e93f4f87fc9a9f35e73e3c05669e0edd0c2526834686e949bb1fd303/djangorestframework-3.16.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9d/d3/ff520d11e6ee400602711d1ece8168dcfc5b6d8146fb7db4244a6ad6a9c3/pytest_vcr-1.0.2-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4a/3b/2e0797200c51531a6d8c97a8e4c9fa6fb56de7e6e2a15c1c067b6b10a0b0/deepdiff-8.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ac/00/d9ea755cdeda3d498504775b62122b72ba282b91446fd58980171fb1084c/dramatiq-1.18.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/be/8f/2364fb244568075cfc239d24e831a1331e3ecefe9b666c91446c0d2ca6c3/newrelic-10.14.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4a/c7/d8c3fa6d2de6814c92161fcce984a7d38a63186590a66238e7b749f7fe37/pytest_deadfixtures-2.2.1-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/13/4b/157c1113e317f79a257b4dfe0607dbab7f57bec67a34d053588dfb8945ac/model_bakery-1.20.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/77/a6/10a0cb96f573f868276b1704747958efd6adda0716226238572e7b7f565f/configcat_client-9.0.4-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/19/58/5d14cb5cb59409e491ebe816c47bf81423cd03098ea92281336320ae5681/pytest_socket-0.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/09/b19725f405621e41c119c6f4b43359c281ddd471794c215b754b8903f52c/django_dramatiq-0.13.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for configcat-client==9.0.4: enum-compat>=0.0.3
DEBUG Adding transitive dependency for configcat-client==9.0.4: qualname>=0.1.0
DEBUG Adding transitive dependency for configcat-client==9.0.4: requests{python_full_version >= '3.7'}>=2.31.0
DEBUG Adding transitive dependency for configcat-client==9.0.4: semver>=2.10.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/b8/87cfb16045c9d4092cfcf526135d73b88101aac83bc1adcf82dfb5fd3833/pytest_env-1.1.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of confluent-kafka[avro] (>=2.11.0, <2.11.0+)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/da/9da67c67b3d0963160e3d2cbc7c38b6fae342670cc8e6d5936644b2cf944/pytest_dotenv-0.5.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/be/ac/bd0608d229ec808e51a21044f3f2f27b9a37e7a0ebaca7247882e67876af/pytest_django-4.11.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f4/87/647ce93053cb5e35e07bded676340774fe43190388b885c54aff47d8557b/djangorestframework-camel-case-1.4.2.tar.gz
DEBUG Selecting: confluent-kafka==2.11.0 [preference] (confluent_kafka-2.11.0-cp313-cp313-macosx_13_0_arm64.whl)
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: confluent-kafka==2.11.0
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: confluent-kafka[avro]==2.11.0
DEBUG Searching for a compatible version of confluent-kafka (==2.11.0)
DEBUG Selecting: confluent-kafka==2.11.0 [preference] (confluent_kafka-2.11.0-cp313-cp313-macosx_13_0_arm64.whl)
DEBUG No static `pyproject.toml` available for: djangorestframework-camel-case==1.4.2 (FieldNotFound("project"))
DEBUG Found stale response for: https://pypi.org/simple/enum-compat/
DEBUG Sending revalidation request for: https://pypi.org/simple/enum-compat/
DEBUG Found stale response for: https://pypi.org/simple/qualname/
DEBUG Sending revalidation request for: https://pypi.org/simple/qualname/
DEBUG Found stale response for: https://pypi.org/simple/semver/
DEBUG Sending revalidation request for: https://pypi.org/simple/semver/
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG No static `PKG-INFO` available for: djangorestframework-camel-case==1.4.2 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG Using cached metadata for: djangorestframework-camel-case==1.4.2
DEBUG Released lock at `/home/runner/.cache/uv/sdists-v9/pypi/djangorestframework-camel-case/1.4.2/.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/1b/e7d6726d75a2222d3fecaa04e5d85aad954de65ddd6760959d09260b9208/confluent_kafka-2.11.0-cp313-cp313-macosx_13_0_arm64.whl.metadata
DEBUG Searching for a compatible version of confluent-kafka[avro] (==2.11.0)
DEBUG Selecting: confluent-kafka==2.11.0 [preference] (confluent_kafka-2.11.0-cp313-cp313-macosx_13_0_arm64.whl)
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: attrs*
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: authlib>=1.0.0
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: avro>=1.11.1, <2
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: cachetools*
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: fastavro{python_full_version >= '3.8'}<2
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: httpx>=0.26
DEBUG Adding transitive dependency for confluent-kafka==2.11.0: requests*
DEBUG Searching for a compatible version of cryptography (>=45.0.5, <45.0.5+)
DEBUG Selecting: cryptography==45.0.5 [preference] (cryptography-45.0.5-cp311-abi3-macosx_10_9_universal2.whl)
DEBUG Adding transitive dependency for cryptography==45.0.5: cffi{platform_python_implementation != 'PyPy'}>=1.14
DEBUG Searching for a compatible version of django (>=5.2.4, <5.2.4+)
DEBUG Selecting: django==5.2.4 [preference] (django-5.2.4-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/attrs/
DEBUG Adding transitive dependency for django==5.2.4: asgiref>=3.8.1
DEBUG Sending revalidation request for: https://pypi.org/simple/attrs/
DEBUG Adding transitive dependency for django==5.2.4: sqlparse>=0.3.1
DEBUG Adding transitive dependency for django==5.2.4: tzdata{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of django-dramatiq (>=0.13.0, <0.13.0+)
DEBUG Selecting: django-dramatiq==0.13.0 [preference] (django_dramatiq-0.13.0-py3-none-any.whl)
DEBUG Adding transitive dependency for django-dramatiq==0.13.0: django>=4.2
DEBUG Adding transitive dependency for django-dramatiq==0.13.0: dramatiq>=1.11
DEBUG Found stale response for: https://pypi.org/simple/avro/
DEBUG Sending revalidation request for: https://pypi.org/simple/avro/
DEBUG Found stale response for: https://pypi.org/simple/cachetools/
DEBUG Sending revalidation request for: https://pypi.org/simple/cachetools/
DEBUG Found stale response for: https://pypi.org/simple/authlib/
DEBUG Sending revalidation request for: https://pypi.org/simple/authlib/
DEBUG Searching for a compatible version of django-environ (>=0.12.0, <0.12.0+)
DEBUG Selecting: django-environ==0.12.0 [preference] (django_environ-0.12.0-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of djangorestframework (>=3.16.0, <3.16.0+)
DEBUG Selecting: djangorestframework==3.16.0 [preference] (djangorestframework-3.16.0-py3-none-any.whl)
DEBUG Adding transitive dependency for djangorestframework==3.16.0: django>=4.2
DEBUG Searching for a compatible version of djangorestframework-camel-case (>=1.4.2, <1.4.2+)
DEBUG Selecting: djangorestframework-camel-case==1.4.2 [preference] (djangorestframework-camel-case-1.4.2.tar.gz)
DEBUG Searching for a compatible version of dramatiq (>=1.18.0, <1.18.0+)
DEBUG Selecting: dramatiq==1.18.0 [preference] (dramatiq-1.18.0-py3-none-any.whl)
DEBUG Adding transitive dependency for dramatiq==1.18.0: prometheus-client>=0.2
DEBUG Found stale response for: https://pypi.org/simple/asgiref/
DEBUG Sending revalidation request for: https://pypi.org/simple/asgiref/
DEBUG Found stale response for: https://pypi.org/simple/sqlparse/
DEBUG Sending revalidation request for: https://pypi.org/simple/sqlparse/
DEBUG Found stale response for: https://pypi.org/simple/prometheus-client/
DEBUG Sending revalidation request for: https://pypi.org/simple/prometheus-client/
DEBUG Found stale response for: https://pypi.org/simple/cffi/
DEBUG Sending revalidation request for: https://pypi.org/simple/cffi/
DEBUG Found stale response for: https://pypi.org/simple/fastavro/
DEBUG Sending revalidation request for: https://pypi.org/simple/fastavro/
DEBUG Found not-modified response for: https://pypi.org/simple/requests/
DEBUG Found not-modified response for: https://pypi.org/simple/semver/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/e4/56027c4a6b4ae70ca9de302488c5ca95ad4a39e190093d6c1a8ace08341b/requests-2.32.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a6/24/4d91e05817e92e3a61c8a21e08fd0f390f5301f1c448b137c57c4bc6e543/semver-3.0.4-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/enum-compat/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/ae/467bc4509246283bb59746e21a1a2f5a8aecbef56b1fa6eaca78cd438c8b/enum_compat-0.0.3-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/attrs/
DEBUG Found not-modified response for: https://pypi.org/simple/cachetools/
DEBUG Found not-modified response for: https://pypi.org/simple/avro/
DEBUG Found not-modified response for: https://pypi.org/simple/authlib/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/77/06/bb80f5f86020c4551da315d78b3ab75e8228f89f0162f2c3a819e407941a/attrs-25.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/f0/2ef431fe4141f5e334759d73e81120492b23b2824336883a91ac04ba710b/cachetools-6.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a8/f9/6c55a90a834594b1c4c6184e8d1b97fa881af84be8e6f4b3ebb2e9d8da19/avro-1.12.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/29/587c189bbab1ccc8c86a03a5d0e13873df916380ef1be461ebe6acebf48d/authlib-1.6.0-py2.py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/qualname/
DEBUG Found not-modified response for: https://pypi.org/simple/cffi/
DEBUG Found not-modified response for: https://pypi.org/simple/fastavro/
DEBUG Found not-modified response for: https://pypi.org/simple/prometheus-client/
DEBUG Found not-modified response for: https://pypi.org/simple/sqlparse/
DEBUG Acquired lock for `/home/runner/.cache/uv/sdists-v9/pypi/qualname/0.1.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/ae/ec06af4fe3ee72d16973474f122541746196aaa16cea6f66d18b963c6177/prometheus_client-0.22.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d9/55/8701163104e69773bb3c9371094372b1f9057fd5fbf33ca8d3236a63a9c1/qualname-0.1.0.tar.gz
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a9/5c/bfd6bd0bf979426d405cc6e71eceb8701b148b16c21d2dc3c261efc61c7b/sqlparse-0.5.3-py3-none-any.whl.metadata
DEBUG No `pyproject.toml` available for: qualname==0.1.0
DEBUG No static `PKG-INFO` available for: qualname==0.1.0 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG Found not-modified response for: https://pypi.org/simple/asgiref/
DEBUG Using cached metadata for: qualname==0.1.0
DEBUG Released lock at `/home/runner/.cache/uv/sdists-v9/pypi/qualname/0.1.0/.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/3c/0464dcada90d5da0e71018c04a140ad6349558afb30b3051b4264cc5b965/asgiref-3.9.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/2d/e5ae05911521bf84113be349d51b16d54589e986837d2d518f63434ea3ec/locust-2.37.11-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/74/b6/2098d0126d2d3318fd5bec3ad40d06c25d377d95749f7a0c5af17129b3b1/ruff-0.12.2-py3-none-linux_armv6l.whl.metadata
DEBUG No netrc file found
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
DEBUG Searching for a compatible version of MY_PRIVATE_LIB (>=1.0.6, <1.0.6+)
DEBUG No compatible version found for: MY_PRIVATE_LIB
DEBUG Recording unit propagation conflict of MY_PRIVATE_LIB from incompatibility of (MY_PROJECT)
DEBUG Searching for a compatible version of MY_PROJECT @ file://MY_PROJECT_PATH (<2.11.0 | >2.11.0)
DEBUG No compatible version found for: MY_PROJECT
  × No solution found when resolving dependencies for split
  │ (python_full_version >= '3.13' and platform_python_implementation !=
  │ 'PyPy'):
  ╰─▶ Because MY_PRIVATE_LIB was not found in the package registry and
      your project depends on MY_PRIVATE_LIB==1.0.6, we can conclude that
      your project's requirements are unsatisfiable.

      hint: An index URL
      (https://MY_PRIVATE_PYPI)
      could not be queried due to a lack of valid authentication credentials
      (401 Unauthorized).
DEBUG Released lock at `MY_PROJECT_PATH/.venv/.lock`
make: *** [Makefile:44: _ruff] Error 1
```
</details>


The uv version `0.7.19` for example just mark dependency as already installed.

```
DEBUG Requirement already installed
```

---

_Comment by @charliermarsh on 2025-07-12 18:33_

I think you just need to update your lockfile. Your index definition includes a trailing slash on the URL, but the lockfile is omitting the trailing slash. We changed in v0.7.20 to stop stripping trailing slashes.

---

_Comment by @jairhenrique on 2025-07-13 22:56_

Thanks @charliermarsh, updating the lockfile the error does not occur in version 0.7.20!

---

_Closed by @jairhenrique on 2025-07-13 22:56_

---
