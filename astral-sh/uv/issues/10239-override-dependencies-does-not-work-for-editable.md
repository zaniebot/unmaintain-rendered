```yaml
number: 10239
title: Override dependencies does not work for editable installs?
type: issue
state: closed
author: phiweger
labels:
  - bug
assignees: []
created_at: 2024-12-30T13:20:06Z
updated_at: 2024-12-30T20:13:40Z
url: https://github.com/astral-sh/uv/issues/10239
synced_at: 2026-01-12T16:00:09Z
```

# Override dependencies does not work for editable installs?

---

_@phiweger_

Hi,

In my `pyproject.toml` I try to "redefine" the dependencies for a package (`pgvector-haystack`); it has as a dependency `psycopg[binary]`, which has no Mac distributions for the current version of `psycopg`, but `psycopg[c]` works, too.

```toml
[project]
name = "basics"
version = "0.0.1"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "loguru>=0.7.3",
    "tqdm>=4.67.1",
    "sqlmodel>=0.0.22",
    "pydantic>=2.10.4",
    "tiktoken>=0.8.0",
    "numpy>=2.2.1",
    "openai>=1.58.1",
    "instructor>=1.7.2",
    "groq>=0.13.1",
    "anthropic>=0.42.0",
    "python-dotenv>=1.0.1",
    "sqlalchemy>=2.0.36",
    "pgvector-haystack==1.2.0",
]

[tool.uv.sources]
basics = { workspace = true }


[[tool.uv.dependency-metadata]]
name = "pgvector-haystack"
version = "1.2.0"
requires-dist = ["haystack-ai", "pgvector", "psycopg[c]==3.2.3"]

[dependency-groups]
dev = [
    "pytest>=7.4.0",
    "pytest-asyncio>=0.23.6",
    "black>=24.10.0",
]
```

I now run

```bash
uv pip install -r pyproject.toml
```

... and it installs `psycopg-c` (which is what I want, the target library works with that, too).

Now I run 

```bash
uv pip install -e .
```

And it will install `psycopg-binary` (an old version) which I am trying to not do.

How can I install my local repo in editable mode while still forcing the dependency override?

Thanks!

---

_Comment by @charliermarsh on 2024-12-30 15:55_

Unfortunately I can't reproduce this. `uv pip install -e .` gave me:

```
 + aiohappyeyeballs==2.4.4
 + aiohttp==3.11.11
 + aiosignal==1.3.2
 + annotated-types==0.7.0
 + anthropic==0.42.0
 + anyio==4.7.0
 + attrs==24.3.0
 + backoff==2.2.1
 + basics==0.0.1 (from file:///Users/crmarsh/workspace/uv/foo)
 + certifi==2024.12.14
 + charset-normalizer==3.4.1
 + click==8.1.8
 + distro==1.9.0
 + docstring-parser==0.16
 + frozenlist==1.5.0
 + groq==0.13.1
 + h11==0.14.0
 + haystack-ai==2.8.0
 + haystack-experimental==0.4.0
 + httpcore==1.0.7
 + httpx==0.28.1
 + idna==3.10
 + instructor==1.7.2
 + jinja2==3.1.5
 + jiter==0.8.2
 + lazy-imports==0.4.0
 + loguru==0.7.3
 + markdown-it-py==3.0.0
 + markupsafe==3.0.2
 + mdurl==0.1.2
 + monotonic==1.6
 + more-itertools==10.5.0
 + multidict==6.1.0
 + networkx==3.4.2
 + numpy==2.2.1
 + openai==1.58.1
 + pandas==2.2.3
 + pgvector==0.3.6
 + pgvector-haystack==1.2.0
 + posthog==3.7.4
 + propcache==0.2.1
 + psycopg==3.2.3
 + psycopg-c==3.2.3
 + pydantic==2.10.4
 + pydantic-core==2.27.2
 + pygments==2.18.0
 + python-dateutil==2.9.0.post0
 + python-dotenv==1.0.1
 + pytz==2024.2
 + pyyaml==6.0.2
 + regex==2024.11.6
 + requests==2.32.3
 + rich==13.9.4
 + shellingham==1.5.4
 + six==1.17.0
 + sniffio==1.3.1
 + sqlalchemy==2.0.36
 + sqlmodel==0.0.22
 + tenacity==9.0.0
 + tiktoken==0.8.0
 + tqdm==4.67.1
 + typer==0.15.1
 + typing-extensions==4.12.2
 + tzdata==2024.2
 + urllib3==2.3.0
 + yarl==1.18.3
```

---

_Label `needs-mre` added by @charliermarsh on 2024-12-30 15:55_

---

_Comment by @phiweger on 2024-12-30 16:04_

Sorry if this is a dump question: But you ran:

```bash
uv pip install -r pyproject.toml
uv pip install -e .
```

Correct?

---

_Comment by @phiweger on 2024-12-30 16:08_

The exact commands used (uv is v0.5.13):

```bash
conda create -y -n uvtest8 python=3.12 ipython uv && conda activate uvtest8
uv pip install -r pyproject.toml       
uv pip install -e .
```

<img width="250" alt="Image" src="https://github.com/user-attachments/assets/554fe2ac-182c-4037-b2fb-d4a09964f547" />


---

_Comment by @charliermarsh on 2024-12-30 16:16_

I only ran the second command. It looks like I can reproduce if I run both commands. I'll take a look, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-30 16:16_

---

_Comment by @charliermarsh on 2024-12-30 16:21_

Ah ok, it's because once `pgvector-haystack` is installed, we read the metadata from the installed version.

---

_Label `needs-mre` removed by @charliermarsh on 2024-12-30 16:23_

---

_Label `bug` added by @charliermarsh on 2024-12-30 16:23_

---

_Closed by @charliermarsh on 2024-12-30 17:47_

---

_Closed by @charliermarsh on 2024-12-30 17:47_

---

_Comment by @phiweger on 2024-12-30 20:13_

awesome, thanks for the quick fix @charliermarsh !

---
