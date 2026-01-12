```yaml
number: 6914
title: "How do I replace using `python -m` with `uv`"
type: issue
state: closed
author: mattpodolak
labels: []
assignees: []
created_at: 2024-09-01T17:03:46Z
updated_at: 2025-12-16T11:38:21Z
url: https://github.com/astral-sh/uv/issues/6914
synced_at: 2026-01-12T15:59:08Z
```

# How do I replace using `python -m` with `uv`

---

_@mattpodolak_

To download a language for spacy, normally I would run  `python -m spacy download`, I  can't seem to figure out the `uv` way of doing this. 

**Version:** `uv 0.4.1 (823f23e22 2024-08-30)`

### as a package
I've tried adding it to my workspace and I get the error when i try to use uv run:

```
$ uv run --package spacy download es_core_news_md
error: Package `spacy` not found in workspace
```

from my `pyproject.toml`:
```
dependencies = [
    ....,
    "spacy>=3.7.6",
]

[tool.uv.sources]
spacy = { workspace = true }
```

Running `uv add spacy` gives me this error:
```
  Caused by: Failed to parse entry for: `spacy`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

### as a tool
```
$ uv tool run spacy download es_core_news_md
...\uv\tools\spacy\Scripts\python.exe: No module named pip
```

---

_Comment by @charliermarsh on 2024-09-01 17:16_

Good question. `{ workspace = true }` is intended for other _local_ projects in your workspace (like, if you had a subdirectory that was a standalone Python project, and you wanted to import it from the root of your project).

You have a few different options...

1. If you're outside of a project, and are just trying to run this as a one-off command, you could `uv run --with spacy -- spacy download es_core_news_md`. The `--with spacy` says "Download and install Spacy before running the command", and then the contents after the `--` are the actual command (`spacy download es_core_news_md`).

```console
❯  uv run --with spacy spacy download es_core_news_md
Collecting es-core-news-md==3.7.0
  Downloading https://github.com/explosion/spacy-models/releases/download/es_core_news_md-3.7.0/es_core_news_md-3.7.0-py3-none-any.whl (42.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 42.3/42.3 MB 42.5 MB/s eta 0:00:00
Requirement already satisfied: spacy<3.8.0,>=3.7.0 in /Users/crmarsh/.cache/uv/archive-v0/g5MPE_L8O18cSfqHr7c9v/lib/python3.12/site-packages (from es-core-news-md==3.7.0) (3.7.6)
Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.11 in /Users/crmarsh/.cache/uv/archive-v0/g5MPE_L8O18cSfqHr7c9v/lib/python3.12/site-packages (from spacy<3.8.0,>=3.7.0->es-core-news-md==3.7.0) (3.0.12)
Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/crmarsh/.cache/uv/archive-v0/g5MPE_L8O18cSfqHr7c9v/lib/python3.12/site-packages (from spacy<3.8.0,>=3.7.0->es-core-news-md==3.7.0) (1.0.5)
Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/crmarsh/.cache/uv/archive-v0/g5MPE_L8O18cSfqHr7c9v/lib/python3.12/site-packages (from spacy<3.8.0,>=3.7.0->es-core-news-md==3.7.0) (1.0.10)
Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/crmarsh/.cache/uv/archive-v0/g5MPE_L8O18cSfqHr7c9v/lib/python3.12/site-packages (from spacy<3.8.0,>=3.7.0->es-core-news-md==3.7.0) (2.0.8)
...
```

Alternatively, you can make a `pyproject.toml` like:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["spacy>=3.7.6"]
```

And then run `uv run -- spacy download es_core_news_md` from that directory. In this case, uv will lock and sync your project (which lists `spacy` as a dependency), then run `spacy download es_core_news_md` in your project's virtual environment.


---

_Comment by @charliermarsh on 2024-09-01 17:16_

(In both examples, the `--` is optional, I'm just including it for clarity.)

---

_Comment by @mattpodolak on 2024-09-01 17:21_

Thanks for the quick response @charliermarsh ! I was able to download the model 🙌🏽 

---

_Closed by @mattpodolak on 2024-09-01 17:21_

---

_Comment by @zanieb on 2024-09-03 14:14_

Related https://github.com/astral-sh/uv/issues/6638

---

_Comment by @lrvdijk on 2024-12-03 00:57_

I have listed spacy as one of my dependencies in my pyproject.toml (for a completely `uv` managed app package). 

However, when I run `uv run spacy download en_core_web_sm`, I get the error `[...]/.venv/bin/python3: No module named pip`.

Do you know how I can fix this?

---

_Comment by @zanieb on 2024-12-03 17:11_

@lrvdijk probably with `uv venv --seed` or `uv pip install pip`, though the specifics depend on more information about the error.

Please open new issues instead of bumping old ones though, see #9452 

---

_Comment by @devhero on 2025-04-27 10:27_

@zanieb `uv pip install pip` works, thanks!
I prefer use: `uv add pip`.

---

_Comment by @GregSotiropoulos on 2025-12-16 11:34_

Since I didn't see anyone mentioning this: I got the Spacy models to install fine **without installing pip** in my virtual environment, by adding the wheel URL for the model in pyproject.toml:
```
[project]
...
dependencies = [
    "en_core_web_trf @ https://github.com/explosion/spacy-models/releases/download/en_core_web_trf-3.8.0/en_core_web_trf-3.8.0-py3-none-any.whl",
    ...
]
```
Links to all model wheels can be found at the [spacy models releases page](https://github.com/explosion/spacy-models/releases). 

---
