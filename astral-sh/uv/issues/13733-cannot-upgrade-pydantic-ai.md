---
number: 13733
title: "Cannot upgrade `pydantic-ai`"
type: issue
state: closed
author: proever
labels:
  - question
assignees: []
created_at: 2025-05-30T16:22:45Z
updated_at: 2025-05-30T20:13:23Z
url: https://github.com/astral-sh/uv/issues/13733
synced_at: 2026-01-10T01:25:37Z
---

# Cannot upgrade `pydantic-ai`

---

_Issue opened by @proever on 2025-05-30 16:22_

### Question

For some reason, trying to upgrade this package specifically results in `uv` doing nothing. Other packages work fine (btw is `uv sync -P` recommended over `uv lock`?).

In my `pyproject.toml` I have `"pydantic-ai>=0.2.4"`.

This might not be the right place for this question but I can't figure out what's going on here.

```bash
> uv tree --outdated --package pydantic-ai
Resolved 290 packages in 2ms
pydantic-ai v0.2.9 (latest: v0.2.12)
└── pydantic-ai-slim[a2a, anthropic, bedrock, cli, cohere, evals, google, groq, mcp, mistral, openai, vertexai] v0.2.9 (latest: v0.2.12)
    ├── eval-type-backport v0.2.2
    ├── griffe v1.7.3
    │   └── colorama v0.4.6
    ├── httpx v0.28.1
    │   ├── anyio v4.8.0 (latest: v4.9.0)
    │   │   ├── idna v3.10
    │   │   ├── sniffio v1.3.1
    │   │   └── typing-extensions v4.12.2 (latest: v4.13.2)
    │   ├── certifi v2025.1.31 (latest: v2025.4.26)
    │   ├── httpcore v1.0.7 (latest: v1.0.9)
    │   │   ├── certifi v2025.1.31
    │   │   └── h11 v0.14.0 (latest: v0.16.0)
    │   └── idna v3.10
    ├── opentelemetry-api v1.31.1 (latest: v1.33.1)
    │   ├── deprecated v1.2.18
    │   │   └── wrapt v1.17.2
    │   └── importlib-metadata v8.6.1 (latest: v8.7.0)
    │       └── zipp v3.21.0 (latest: v3.22.0)
    ├── pydantic v2.10.6 (latest: v2.11.5)
    │   ├── annotated-types v0.7.0
    │   ├── pydantic-core v2.27.2 (latest: v2.34.1)
    │   │   └── typing-extensions v4.12.2
    │   └── typing-extensions v4.12.2
    ├── pydantic-graph v0.2.9 (latest: v0.2.12)
    │   ├── httpx v0.28.1 (*)
    │   ├── logfire-api v3.16.0 (latest: v3.16.1)
    │   ├── pydantic v2.10.6 (*)
    │   └── typing-inspection v0.4.0 (latest: v0.4.1)
    │       └── typing-extensions v4.12.2
    ├── typing-inspection v0.4.0 (*)
    ├── fasta2a v0.2.9 (extra: a2a) (latest: v0.2.12)
    │   ├── opentelemetry-api v1.31.1 (*)
    │   ├── pydantic v2.10.6 (*)
    │   └── starlette v0.46.1 (latest: v0.47.0)
    │       └── anyio v4.8.0 (*)
    ├── anthropic v0.51.0 (extra: anthropic) (latest: v0.52.1)
    │   ├── anyio v4.8.0 (*)
    │   ├── distro v1.9.0
    │   ├── httpx v0.28.1 (*)
    │   ├── jiter v0.9.0 (latest: v0.10.0)
    │   ├── pydantic v2.10.6 (*)
    │   ├── sniffio v1.3.1
    │   └── typing-extensions v4.12.2
    ├── boto3 v1.37.13 (extra: bedrock) (latest: v1.38.26)
    │   ├── botocore v1.37.13 (latest: v1.38.26)
    │   │   ├── jmespath v1.0.1
    │   │   ├── python-dateutil v2.9.0.post0
    │   │   │   └── six v1.17.0
    │   │   └── urllib3 v2.3.0 (latest: v2.4.0)
    │   ├── jmespath v1.0.1
    │   └── s3transfer v0.11.4 (latest: v0.13.0)
    │       └── botocore v1.37.13 (*)
    ├── argcomplete v3.6.2 (extra: cli)
    ├── prompt-toolkit v3.0.50 (extra: cli) (latest: v3.0.51)
    │   └── wcwidth v0.2.13
    ├── rich v13.9.4 (extra: cli) (latest: v14.0.0)
    │   ├── markdown-it-py v3.0.0
    │   │   └── mdurl v0.1.2
    │   └── pygments v2.19.1
    ├── cohere v5.15.0 (extra: cohere)
    │   ├── fastavro v1.11.1
    │   ├── httpx v0.28.1 (*)
    │   ├── httpx-sse v0.4.0
    │   ├── pydantic v2.10.6 (*)
    │   ├── pydantic-core v2.27.2 (*)
    │   ├── requests v2.32.3
    │   │   ├── certifi v2025.1.31
    │   │   ├── charset-normalizer v3.4.1 (latest: v3.4.2)
    │   │   ├── idna v3.10
    │   │   └── urllib3 v2.3.0
    │   ├── tokenizers v0.21.1
    │   │   └── huggingface-hub v0.31.4 (latest: v0.32.3)
    │   │       ├── filelock v3.18.0
    │   │       ├── fsspec v2025.5.0 (latest: v2025.5.1)
    │   │       ├── packaging v24.2 (latest: v25.0)
    │   │       ├── pyyaml v6.0.2
    │   │       ├── requests v2.32.3 (*)
    │   │       ├── tqdm v4.67.1
    │   │       └── typing-extensions v4.12.2
    │   ├── types-requests v2.32.0.20250515
    │   │   └── urllib3 v2.3.0
    │   └── typing-extensions v4.12.2
    ├── pydantic-evals v0.2.9 (extra: evals) (latest: v0.2.12)
    │   ├── anyio v4.8.0 (*)
    │   ├── logfire-api v3.16.0
    │   ├── pydantic v2.10.6 (*)
    │   ├── pydantic-ai-slim v0.2.9 (*)
    │   ├── pyyaml v6.0.2
    │   └── rich v13.9.4 (*)
    ├── google-genai v1.16.1 (extra: google) (latest: v1.17.0)
    │   ├── anyio v4.8.0 (*)
    │   ├── google-auth v2.38.0 (latest: v2.40.2)
    │   │   ├── cachetools v5.5.2 (latest: v6.0.0)
    │   │   ├── pyasn1-modules v0.4.1 (latest: v0.4.2)
    │   │   │   └── pyasn1 v0.4.8 (latest: v0.6.1)
    │   │   └── rsa v4.9 (latest: v4.9.1)
    │   │       └── pyasn1 v0.4.8
    │   ├── httpx v0.28.1 (*)
    │   ├── pydantic v2.10.6 (*)
    │   ├── requests v2.32.3 (*)
    │   ├── typing-extensions v4.12.2
    │   └── websockets v15.0.1
    ├── groq v0.25.0 (extra: groq) (latest: v0.26.0)
    │   ├── anyio v4.8.0 (*)
    │   ├── distro v1.9.0
    │   ├── httpx v0.28.1 (*)
    │   ├── pydantic v2.10.6 (*)
    │   ├── sniffio v1.3.1
    │   └── typing-extensions v4.12.2
    ├── mcp v1.9.0 (extra: mcp) (latest: v1.9.2)
    │   ├── anyio v4.8.0 (*)
    │   ├── httpx v0.28.1 (*)
    │   ├── httpx-sse v0.4.0
    │   ├── pydantic v2.10.6 (*)
    │   ├── pydantic-settings v2.8.1 (latest: v2.9.1)
    │   │   ├── pydantic v2.10.6 (*)
    │   │   └── python-dotenv v1.0.1 (latest: v1.1.0)
    │   ├── python-multipart v0.0.20
    │   ├── sse-starlette v2.3.5 (latest: v2.3.6)
    │   │   ├── anyio v4.8.0 (*)
    │   │   └── starlette v0.46.1 (*)
    │   ├── starlette v0.46.1 (*)
    │   └── uvicorn v0.34.0 (latest: v0.34.2)
    │       ├── click v8.1.8 (latest: v8.2.1)
    │       ├── h11 v0.14.0
    │       ├── httptools v0.6.4 (extra: standard)
    │       ├── python-dotenv v1.0.1 (extra: standard)
    │       ├── pyyaml v6.0.2 (extra: standard)
    │       ├── uvloop v0.21.0 (extra: standard)
    │       ├── watchfiles v1.0.4 (extra: standard) (latest: v1.0.5)
    │       │   └── anyio v4.8.0 (*)
    │       └── websockets v15.0.1 (extra: standard)
    ├── mistralai v1.7.0 (extra: mistral) (latest: v1.8.1)
    │   ├── eval-type-backport v0.2.2
    │   ├── httpx v0.28.1 (*)
    │   ├── pydantic v2.10.6 (*)
    │   ├── python-dateutil v2.9.0.post0 (*)
    │   └── typing-inspection v0.4.0 (*)
    ├── openai v1.79.0 (extra: openai) (latest: v1.82.1)
    │   ├── anyio v4.8.0 (*)
    │   ├── distro v1.9.0
    │   ├── httpx v0.28.1 (*)
    │   ├── jiter v0.9.0
    │   ├── pydantic v2.10.6 (*)
    │   ├── sniffio v1.3.1
    │   ├── tqdm v4.67.1
    │   └── typing-extensions v4.12.2
    ├── google-auth v2.38.0 (extra: vertexai) (*)
    └── requests v2.32.3 (extra: vertexai) (*)
(*) Package tree already displayed


> uv sync -P pydantic-ai
Resolved 290 packages in 78ms
Audited 226 packages in 0.17ms


> uv lock -P pydantic-ai
Resolved 290 packages in 71ms
```

### Platform

macOS 15.5

### Version

0.7.8 via homebrew

---

_Label `question` added by @proever on 2025-05-30 16:22_

---

_Comment by @konstin on 2025-05-30 17:34_

Can you share more, such as a `pyproject.toml` or a `uv.lock`?

For debugging this, you can search in `uv.lock` for `pydantic-ai` and check if any of the packages in the has an upper bound on the package. Another option is installing ripgrep and seaching

```
rg -u "Requires-Dist: pydantic-ai"
```

in your `.venv`

---

_Comment by @proever on 2025-05-30 20:03_

Interestingly, I am not able to reproduce this issue if I just paste my deps into a new `pyproject.toml` and `sync` from there. It installs `0.2.12`, and if I manually install `0.2.9` and then relax that requirement again it also upgrades successfully. Once I add my `uv.lock` back in it stops working, which makes sense I guess.

Here's my lockfile: [uv.lock.txt](https://github.com/user-attachments/files/20526251/uv.lock.txt) (GH wouldn't let me paste the whole thing in a spoiler tag it seems)

here is the `rg` output: 
```bash
> rg -u "Requires-Dist: pydantic-ai" .venv
.venv/lib/python3.12/site-packages/agi-2.2.1.dist-info/METADATA
22:Requires-Dist: pydantic-ai>=0.2.4

.venv/lib/python3.12/site-packages/pydantic_ai-0.2.9.dist-info/METADATA
31:Requires-Dist: pydantic-ai-slim[a2a,anthropic,bedrock,cli,cohere,evals,google,groq,mcp,mistral,openai,vertexai]==0.2.9
33:Requires-Dist: pydantic-ai-examples==0.2.9; extra == 'examples'

.venv/lib/python3.12/site-packages/pydantic_evals-0.2.9.dist-info/METADATA
35:Requires-Dist: pydantic-ai-slim==0.2.9
```

since these are deps of `pydantic-ai` I don't think these should affect things?

here's the full debug output from `uv sync -vP pydantic-ai`: [uv_upgrade.log](https://github.com/user-attachments/files/20526145/uv_upgrade.log)

and here are some excerpts:
```log
DEBUG Searching for a compatible version of pydantic-ai (>=0.2.4)
DEBUG Selecting: pydantic-ai==0.2.9 [preference] (pydantic_ai-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[a2a]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[anthropic]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[bedrock]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[cli]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[cohere]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[evals]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[google]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[groq]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[mcp]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[mistral]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[openai]>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai==0.2.9: pydantic-ai-slim[vertexai]>=0.2.9, <0.2.9+
DEBUG Searching for a compatible version of pydantic-ai-slim[a2a] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[a2a]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[anthropic] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[anthropic]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[bedrock] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[bedrock]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[cli] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[cli]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[cohere] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[cohere]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[evals] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[evals]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[google] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[google]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[groq] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[groq]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[mcp] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[mcp]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[mistral] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[mistral]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[openai] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[openai]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim[vertexai] (>=0.2.9, <0.2.9+)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim==0.2.9
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-ai-slim[vertexai]==0.2.9
DEBUG Searching for a compatible version of pydantic-ai-slim (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: eval-type-backport>=0.2.0
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: griffe>=1.3.2
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: httpx>=0.27
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: opentelemetry-api>=1.28.0
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic>=2.10
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-graph>=0.2.9, <0.2.9+
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: typing-inspection>=0.4.0
DEBUG Searching for a compatible version of pydantic-ai-slim[a2a] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: fasta2a>=0.2.9, <0.2.9+
DEBUG Searching for a compatible version of pydantic-ai-slim[anthropic] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: anthropic>=0.49.0
DEBUG Searching for a compatible version of pydantic-ai-slim[bedrock] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: boto3>=1.35.74
DEBUG Searching for a compatible version of pydantic-ai-slim[cli] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: argcomplete>=3.5.0
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: prompt-toolkit>=3
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: rich>=13
DEBUG Searching for a compatible version of pydantic-ai-slim[cohere] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: cohere{sys_platform != 'emscripten'}>=5.13.11
DEBUG Searching for a compatible version of pydantic-ai-slim[evals] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: pydantic-evals>=0.2.9, <0.2.9+
DEBUG Searching for a compatible version of pydantic-ai-slim[google] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: google-genai>=1.15.0
DEBUG Searching for a compatible version of pydantic-ai-slim[groq] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: groq>=0.15.0
DEBUG Searching for a compatible version of pydantic-ai-slim[mcp] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: mcp{python_full_version >= '3.10'}>=1.6.0
DEBUG Searching for a compatible version of pydantic-ai-slim[mistral] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: mistralai>=1.2.5
DEBUG Searching for a compatible version of pydantic-ai-slim[openai] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: openai>=1.75.0
DEBUG Searching for a compatible version of pydantic-ai-slim[vertexai] (==0.2.9)
DEBUG Selecting: pydantic-ai-slim==0.2.9 [preference] (pydantic_ai_slim-0.2.9-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: google-auth>=2.36.0
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.9: requests>=2.32.2
```

`DEBUG Selecting: pydantic-ai==0.2.9 [preference] (pydantic_ai-0.2.9-py3-none-any.whl)` is odd?

EDIT: looking more closely at the logs shows this
```
DEBUG Searching for a compatible version of pydantic-ai (>=0.2.4, <0.2.10 | >0.2.10, <0.2.11 | >0.2.11, <0.2.12 | >0.2.12)
DEBUG Selecting: pydantic-ai==0.2.9 [compatible] (pydantic_ai-0.2.9-py3-none-any.whl)
```
Which seems like the source of the issue at least? I'm quite sure though 

---

_Comment by @proever on 2025-05-30 20:11_

Ok, the logs were very helpful. 

```
DEBUG Searching for a compatible version of pydantic-ai-slim[a2a] (==0.2.12)
DEBUG Selecting: pydantic-ai-slim==0.2.12 [compatible] (pydantic_ai_slim-0.2.12-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.12: fasta2a>=0.2.12, <0.2.12+
DEBUG Searching for a compatible version of pydantic-ai-slim[anthropic] (==0.2.12)
DEBUG Selecting: pydantic-ai-slim==0.2.12 [compatible] (pydantic_ai_slim-0.2.12-py3-none-any.whl)
DEBUG Adding transitive dependency for pydantic-ai-slim==0.2.12: anthropic>=0.52.0
DEBUG Recording unit propagation conflict of pydantic-ai-slim[anthropic] from incompatibility of (anthropic, pydantic-ai-slim[anthropic])
DEBUG Recording unit propagation conflict of pydantic-ai-slim[anthropic] from incompatibility of (anthropic, pydantic-ai-slim[anthropic])
DEBUG Searching for a compatible version of pydantic-ai-slim[a2a] (>=0.2.12, <0.2.12+)
DEBUG Selecting: pydantic-ai-slim==0.2.12 [compatible] (pydantic_ai_slim-0.2.12-py3-none-any.whl)
DEBUG Searching for a compatible version of pydantic-ai-slim[anthropic] (>0.2.12, <0.2.12+)
DEBUG No compatible version found for: pydantic-ai-slim[anthropic]
DEBUG Recording unit propagation conflict of pydantic-ai-slim[anthropic] from incompatibility of (anthropic, pydantic-ai-slim[anthropic])
DEBUG Recording unit propagation conflict of pydantic-ai-slim[anthropic] from incompatibility of (anthropic, pydantic-ai)
DEBUG Recording unit propagation conflict of pydantic-ai from incompatibility of (anthropic, pydantic-ai)
DEBUG Package anthropic has too many conflicts (culprit), deprioritizing and backtracking
DEBUG Backtracked 1 decisions
```

upgrading `anthropic` at the same time worked:

```
> uv sync -P pydantic-ai -P anthropic                                     
Resolved 290 packages in 528ms
Prepared 2 packages in 0.97ms
Uninstalled 6 packages in 63ms
Installed 6 packages in 48ms
 - anthropic==0.51.0
 + anthropic==0.52.1
 - fasta2a==0.2.9
 + fasta2a==0.2.12
 - pydantic-ai==0.2.9
 + pydantic-ai==0.2.12
 - pydantic-ai-slim==0.2.9
 + pydantic-ai-slim==0.2.12
 - pydantic-evals==0.2.9
 + pydantic-evals==0.2.12
 - pydantic-graph==0.2.9
 + pydantic-graph==0.2.12
```

apologies the for goose chase! It might be very nice to make this information slightly more readily available, in cases like this in particular. Since I was doing `uv sync -P pydantic-ai` (initially) `uv` could infer I might be interested in why the command is "doing nothing".

---

_Closed by @proever on 2025-05-30 20:11_

---
