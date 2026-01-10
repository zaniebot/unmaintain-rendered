```yaml
number: 10738
title: Dependency sorting issue in pyproject.toml
type: issue
state: closed
author: Kavan72
labels: []
assignees: []
created_at: 2025-01-18T19:03:50Z
updated_at: 2025-04-11T18:31:21Z
url: https://github.com/astral-sh/uv/issues/10738
synced_at: 2026-01-10T03:41:46Z
```

# Dependency sorting issue in pyproject.toml

---

_Issue opened by @Kavan72 on 2025-01-18 19:03_

Related issues

Relates to https://github.com/astral-sh/uv/issues/10076.

Setup
- uv 0.5.21
- Python 3.10
- macOS Sequoia 15.2

pyproject.toml
```toml
[project]
name = "backend"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "aiohttp>=3.11.11",
    "alembic>=1.14.0",
    "asgi-correlation-id>=4.3.4",
    "asyncpg>=0.30.0",
    "celery>=5.4.0",
    "cryptography>=44.0.0",
    "email-validator>=2.2.0",
    "faker>=33.3.0",
    "fastapi-distributed-websocket>=0.2.0",
    "fastapi-pagination>=0.12.34",
    "fastapi[all]>=0.115.6",
    "gunicorn>=23.0.0",
    "loguru>=0.7.3",
    "pyjwt>=2.10.1",
    "pytest-order>=1.3.0",
    "pytest>=8.3.4",
    "sentry-sdk[fastapi]>=2.19.2",
    "sib-api-v3-sdk>=7.6.0",
    "sqlalchemy[asyncio]>=2.0.36",
    "sqlalchemy-utils>=0.41.2",
    "tomlkit>=0.13.2",
    "uvicorn>=0.34.0",
    "websockets>=14.1",
    "jsonschema>=4.23.0",
]

[tool.pytest.ini_options]
minversion = "6.0"
addopts = "-ra"
testpaths = [
    "tests",
]

[tool.uv]
dev-dependencies = [
    "ruff>=0.8.6",
]

[tool.ruff]
line-length = 100

[tool.ruff.lint]
select = ["I", "N", "A", "UP", "F", "E", "T20"]
```

When I try to remove and re-add jsonschema, which is already present, it gets added to the end of the dependencies. 

---

_Comment by @charliermarsh on 2025-01-18 19:24_

You need to switch the order of `"sqlalchemy[asyncio]>=2.0.36"` and `"sqlalchemy-utils>=0.41.2"`.

---

_Comment by @Kavan72 on 2025-01-18 19:38_

> You need to switch the order of `"sqlalchemy[asyncio]>=2.0.36"` and `"sqlalchemy-utils>=0.41.2"`.

Sorry, I don't get you, you mean remove `"sqlalchemy[asyncio]>=2.0.36"`, `"sqlalchemy-utils>=0.41.2"`  and add `jsonschema` ?

---

_Comment by @charliermarsh on 2025-01-18 19:41_

You need to swap the order, like:

```toml
dependencies = [
    "aiohttp>=3.11.11",
    "alembic>=1.14.0",
    "asgi-correlation-id>=4.3.4",
    "asyncpg>=0.30.0",
    "celery>=5.4.0",
    "cryptography>=44.0.0",
    "email-validator>=2.2.0",
    "faker>=33.3.0",
    "fastapi-distributed-websocket>=0.2.0",
    "fastapi-pagination>=0.12.34",
    "fastapi[all]>=0.115.6",
    "gunicorn>=23.0.0",
    "loguru>=0.7.3",
    "pyjwt>=2.10.1",
    "pytest-order>=1.3.0",
    "pytest>=8.3.4",
    "sentry-sdk[fastapi]>=2.19.2",
    "sib-api-v3-sdk>=7.6.0",
    "sqlalchemy-utils>=0.41.2",
    "sqlalchemy[asyncio]>=2.0.36",
    "tomlkit>=0.13.2",
    "uvicorn>=0.34.0",
    "websockets>=14.1",
    "jsonschema>=4.23.0",
]
```

Notice how, earlier, you have `fastapi-distributed-websocket` before `fastapi`.

---

_Comment by @charliermarsh on 2025-01-18 19:44_

We only retain sorting if dependencies are already sorted, and lexicographically, `sqlalchemy-utils` comes before `sqlalchemy`.

---

_Comment by @charliermarsh on 2025-01-18 19:45_

I guess we could consider making the sort detection robust to these kinds of prefixes, but (e.g.) if you "sort alphabetically" in your editor, you should see the same result.

---

_Comment by @Kavan72 on 2025-01-19 05:25_

> We only retain sorting if dependencies are already sorted, and lexicographically, `sqlalchemy-utils` comes before `sqlalchemy`.

The issue arose when I added some libraries with UV version 0.5.12 (which has a sorting issue: https://github.com/astral-sh/uv/issues/10076). This broke the sorting. After upgrading UV to the latest version, adding any extra library now results in it being appended to the end of the dependencies.

---

_Comment by @charliermarsh on 2025-01-19 14:19_

This is the order that uv considers sorted:

```toml
dependencies = [
    "aiohttp>=3.11.11",
    "alembic>=1.14.0",
    "asgi-correlation-id>=4.3.4",
    "asyncpg>=0.30.0",
    "celery>=5.4.0",
    "cryptography>=44.0.0",
    "email-validator>=2.2.0",
    "faker>=33.3.0",
    "fastapi-distributed-websocket>=0.2.0",
    "fastapi-pagination>=0.12.34",
    "fastapi[all]>=0.115.6",
    "gunicorn>=23.0.0",
    "loguru>=0.7.3",
    "pyjwt>=2.10.1",
    "pytest>=8.3.4",
    "pytest-order>=1.3.0",
    "sentry-sdk[fastapi]>=2.19.2",
    "sib-api-v3-sdk>=7.6.0",
    "sqlalchemy-utils>=0.41.2",
    "sqlalchemy[asyncio]>=2.0.36",
    "tomlkit>=0.13.2",
    "uvicorn>=0.34.0",
    "websockets>=14.1",
]
```

I don't _think_ any version of uv would consider the dependencies you posted sorted, even prior to 0.5.12, because `sqlalchemy-utils` needs to come after `sqlalchemy[asyncio]` lexicographically, and you have the order reversed.

Again, please try this version and run `uv add`.

---

_Closed by @Kavan72 on 2025-01-25 17:36_

---

_Comment by @wu-clan on 2025-02-07 03:17_

Hi, @charliermarsh 

This is my dependencies:

```toml
dependencies = [
    "alembic>=1.14.1",
    "asgiref>=3.8.0",
    "asyncmy>=0.2.10",
    "asyncpg>=0.30.0",
    "bcrypt>=4.2.1",
    "casbin>=1.38.0",
    "casbin_async_sqlalchemy_adapter>=1.7.0",
    "celery==5.3.6",
    "cryptography>=44.0.0",
    "fast-captcha>=0.3.2",
    "fastapi[all]==0.111.0",
    "fastapi-limiter>=0.1.6",
    "fastapi-pagination>=0.12.34",
    "itsdangerous>=2.2.0",
    "loguru>=0.7.3",
    "msgspec>=0.19.0",
    "pwdlib>=0.2.1",
    "path==17.0.0",
    "phonenumbers>=8.13.0",
    "psutil>=6.0.0",
    "pydantic>=2.10.6",
    "python-jose>=3.3.0",
    "redis[hiredis]>=5.2.0",
    "sqlalchemy[asyncio]>=2.0.30",
    "user-agents==2.2.0",
    "XdbSearchIP>=1.0.2",
    "fastapi_oauth20>=0.0.1a2",
    "flower>=2.0.0",
    "sqlalchemy-crud-plus==1.6.0",
    "jinja2>=3.1.4",
    "aiofiles>=24.1.0",
    # When celery version < 6.0.0
    # https://github.com/celery/celery/issues/7874
    "celery-aio-pool==0.1.0rc8",
    "asgi-correlation-id>=4.3.3",
    "python-socketio>=5.12.0",
]
```

It did not sort as expected, and whenever I add any dependencies, they are always added to the end

---

_Comment by @charliermarsh on 2025-02-07 03:18_

We only retain sorting if the dependencies are already sorted. The list you gave above is out-of-order?

---

_Comment by @wu-clan on 2025-02-07 03:23_

The dependencies order after `"XdbSearchIP>=1.0.2",` does not seem to be sorted alphabetically, and my view on sorting is that they are always parsed and sorted alphabetically, but I did not find any related information in the documentation.

---

_Comment by @charliermarsh on 2025-02-07 03:28_

If your dependencies are _already_ sorted, then we add in sorted order. If they're not, we add to the end.

Here's the sorted version of the dependencies up to and including `XdbSearchIP`:

```toml
dependencies = [
    "alembic>=1.14.1",
    "asgiref>=3.8.0",
    "asyncmy>=0.2.10",
    "asyncpg>=0.30.0",
    "bcrypt>=4.2.1",
    "casbin>=1.38.0",
    "casbin_async_sqlalchemy_adapter>=1.7.0",
    "celery==5.3.6",
    "cryptography>=44.0.0",
    "fast-captcha>=0.3.2",
    "fastapi-limiter>=0.1.6",
    "fastapi-pagination>=0.12.34",
    "fastapi[all]==0.111.0",
    "itsdangerous>=2.2.0",
    "loguru>=0.7.3",
    "msgspec>=0.19.0",
    "path==17.0.0",
    "phonenumbers>=8.13.0",
    "psutil>=6.0.0",
    "pwdlib>=0.2.1",
    "pydantic>=2.10.6",
    "python-jose>=3.3.0",
    "redis[hiredis]>=5.2.0",
    "sqlalchemy[asyncio]>=2.0.30",
    "user-agents==2.2.0",
    "XdbSearchIP>=1.0.2",
]
```

There were a few errors in your existing sort:

```diff
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -15,16 +15,16 @@ dependencies = [
     "celery==5.3.6",
     "cryptography>=44.0.0",
     "fast-captcha>=0.3.2",
-    "fastapi[all]==0.111.0",
     "fastapi-limiter>=0.1.6",
     "fastapi-pagination>=0.12.34",
+    "fastapi[all]==0.111.0",
     "itsdangerous>=2.2.0",
     "loguru>=0.7.3",
     "msgspec>=0.19.0",
-    "pwdlib>=0.2.1",
     "path==17.0.0",
     "phonenumbers>=8.13.0",
     "psutil>=6.0.0",
+    "pwdlib>=0.2.1",
     "pydantic>=2.10.6",
     "python-jose>=3.3.0",
     "redis[hiredis]>=5.2.0",
```

---

_Comment by @wu-clan on 2025-02-07 03:42_

Thank you very much.

I want to know whether should add some documentation/console output for dependency sorting explanation

---

_Comment by @dhimmel on 2025-04-11 18:31_

> I guess we could consider making the sort detection robust to these kinds of prefixes, but (e.g.) if you "sort alphabetically" in your editor, you should see the same result.

@charliermarsh that would be awesome.

Or at least a recommendation on this thread for a tool that sorts dependencies since IDE sorting differs.

Or perhaps a pyproject option to tell `uv` to always sort?

---
