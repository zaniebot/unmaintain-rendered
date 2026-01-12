```yaml
number: 12098
title: cannot install vllm
type: issue
state: closed
author: da-the-dev
labels:
  - question
assignees: []
created_at: 2025-03-10T16:16:57Z
updated_at: 2025-03-14T10:25:42Z
url: https://github.com/astral-sh/uv/issues/12098
synced_at: 2026-01-12T16:00:55Z
```

# cannot install vllm

---

_@da-the-dev_

### Summary

here's my `pyproject.toml`

```toml
[project]
name = "itmo-x5-agent"
version = "0.1.0"
description = "X5 case solution"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pandas>=2.2.3",
    "ipykernel>=6.29.5",
    "jupyter>=1.1.1",
    "matplotlib>=3.10.1",
    "numpy>=2.2.3",
    "pandas>=2.2.3",
    "seaborn>=0.13.2",
    "pydantic-settings>=2.8.1",
    "langchain>=0.3.20",
    "langchain-openai>=0.3.8",
    "langgraph>=0.3.5",
    "langchain-community>=0.3.19",
    "vllm>=0.1.2",
]
```

when i run `uv sync` i expect that vllm would be installed, and i would be able to run `vllm --help`.

but instead i get `command not found: vllm`

here's `uv sync -v`

```
DEBUG uv 0.6.1 (c91ee82a8 2025-02-17)
DEBUG Found project root: `/home/sv-cheats-1/Documents/itmo-x5-agent`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/sv-cheats-1/Documents/itmo-x5-agent`
DEBUG Reading Python requests from version file at `/home/sv-cheats-1/Documents/itmo-x5-agent/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `/tmp/uv-26cbf5c4c0794eaa.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: itmo-x5-agent @ file:///home/sv-cheats-1/Documents/itmo-x5-agent
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 199 packages in 9ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: ipykernel==6.29.5
DEBUG Requirement already installed: jupyter==1.1.1
DEBUG Requirement already installed: langchain==0.3.20
DEBUG Requirement already installed: langchain-community==0.3.19
DEBUG Requirement already installed: langchain-openai==0.3.8
DEBUG Requirement already installed: langgraph==0.3.5
DEBUG Requirement already installed: matplotlib==3.10.1
DEBUG Requirement already installed: numpy==2.2.3
DEBUG Requirement already installed: pandas==2.2.3
DEBUG Requirement already installed: pydantic-settings==2.8.1
DEBUG Requirement already installed: seaborn==0.13.2
DEBUG Requirement already installed: vllm==0.1.2
DEBUG Requirement already installed: comm==0.2.2
DEBUG Requirement already installed: debugpy==1.8.13
DEBUG Requirement already installed: ipython==9.0.2
DEBUG Requirement already installed: jupyter-client==8.6.3
DEBUG Requirement already installed: jupyter-core==5.7.2
DEBUG Requirement already installed: matplotlib-inline==0.1.7
DEBUG Requirement already installed: nest-asyncio==1.6.0
DEBUG Requirement already installed: packaging==24.2
DEBUG Requirement already installed: psutil==7.0.0
DEBUG Requirement already installed: pyzmq==26.2.1
DEBUG Requirement already installed: tornado==6.4.2
DEBUG Requirement already installed: traitlets==5.14.3
DEBUG Requirement already installed: ipywidgets==8.1.5
DEBUG Requirement already installed: jupyter-console==6.6.3
DEBUG Requirement already installed: jupyterlab==4.3.5
DEBUG Requirement already installed: nbconvert==7.16.6
DEBUG Requirement already installed: notebook==7.3.2
DEBUG Requirement already installed: langchain-core==0.3.43
DEBUG Requirement already installed: langchain-text-splitters==0.3.6
DEBUG Requirement already installed: langsmith==0.3.13
DEBUG Requirement already installed: pydantic==2.10.6
DEBUG Requirement already installed: pyyaml==6.0.2
DEBUG Requirement already installed: requests==2.32.3
DEBUG Requirement already installed: sqlalchemy==2.0.38
DEBUG Requirement already installed: aiohttp==3.11.13
DEBUG Requirement already installed: dataclasses-json==0.6.7
DEBUG Requirement already installed: httpx-sse==0.4.0
DEBUG Requirement already installed: tenacity==9.0.0
DEBUG Requirement already installed: openai==1.65.5
DEBUG Requirement already installed: tiktoken==0.9.0
DEBUG Requirement already installed: langgraph-checkpoint==2.0.18
DEBUG Requirement already installed: langgraph-prebuilt==0.1.2
DEBUG Requirement already installed: langgraph-sdk==0.1.55
DEBUG Requirement already installed: contourpy==1.3.1
DEBUG Requirement already installed: cycler==0.12.1
DEBUG Requirement already installed: fonttools==4.56.0
DEBUG Requirement already installed: kiwisolver==1.4.8
DEBUG Requirement already installed: pillow==11.1.0
DEBUG Requirement already installed: pyparsing==3.2.1
DEBUG Requirement already installed: python-dateutil==2.9.0.post0
DEBUG Requirement already installed: pytz==2025.1
DEBUG Requirement already installed: tzdata==2025.1
DEBUG Requirement already installed: python-dotenv==1.0.1
DEBUG Requirement already installed: fastapi==0.115.11
DEBUG Requirement already installed: fschat==0.2.36
DEBUG Requirement already installed: ninja==1.11.1.3
DEBUG Requirement already installed: ray==2.43.0
DEBUG Requirement already installed: sentencepiece==0.2.0
DEBUG Requirement already installed: torch==2.6.0
DEBUG Requirement already installed: transformers==4.49.0
DEBUG Requirement already installed: uvicorn==0.34.0
DEBUG Requirement already installed: xformers==0.0.29.post3
DEBUG Requirement already installed: decorator==5.2.1
DEBUG Requirement already installed: ipython-pygments-lexers==1.1.1
DEBUG Requirement already installed: jedi==0.19.2
DEBUG Requirement already installed: pexpect==4.9.0
DEBUG Requirement already installed: prompt-toolkit==3.0.50
DEBUG Requirement already installed: pygments==2.19.1
DEBUG Requirement already installed: stack-data==0.6.3
DEBUG Requirement already installed: platformdirs==4.3.6
DEBUG Requirement already installed: jupyterlab-widgets==3.0.13
DEBUG Requirement already installed: widgetsnbextension==4.0.13
DEBUG Requirement already installed: async-lru==2.0.4
DEBUG Requirement already installed: httpx==0.28.1
DEBUG Requirement already installed: jinja2==3.1.6
DEBUG Requirement already installed: jupyter-lsp==2.2.5
DEBUG Requirement already installed: jupyter-server==2.15.0
DEBUG Requirement already installed: jupyterlab-server==2.27.3
DEBUG Requirement already installed: notebook-shim==0.2.4
DEBUG Requirement already installed: setuptools==76.0.0
DEBUG Requirement already installed: beautifulsoup4==4.13.3
DEBUG Requirement already installed: bleach==6.2.0
DEBUG Requirement already installed: defusedxml==0.7.1
DEBUG Requirement already installed: jupyterlab-pygments==0.3.0
DEBUG Requirement already installed: markupsafe==3.0.2
DEBUG Requirement already installed: mistune==3.1.2
DEBUG Requirement already installed: nbclient==0.10.2
DEBUG Requirement already installed: nbformat==5.10.4
DEBUG Requirement already installed: pandocfilters==1.5.1
DEBUG Requirement already installed: jsonpatch==1.33
DEBUG Requirement already installed: typing-extensions==4.12.2
DEBUG Requirement already installed: orjson==3.10.15
DEBUG Requirement already installed: requests-toolbelt==1.0.0
DEBUG Requirement already installed: zstandard==0.23.0
DEBUG Requirement already installed: annotated-types==0.7.0
DEBUG Requirement already installed: pydantic-core==2.27.2
DEBUG Requirement already installed: certifi==2025.1.31
DEBUG Requirement already installed: charset-normalizer==3.4.1
DEBUG Requirement already installed: idna==3.10
DEBUG Requirement already installed: urllib3==2.3.0
DEBUG Requirement already installed: greenlet==3.1.1
DEBUG Requirement already installed: aiohappyeyeballs==2.5.0
DEBUG Requirement already installed: aiosignal==1.3.2
DEBUG Requirement already installed: attrs==25.1.0
DEBUG Requirement already installed: frozenlist==1.5.0
DEBUG Requirement already installed: multidict==6.1.0
DEBUG Requirement already installed: propcache==0.3.0
DEBUG Requirement already installed: yarl==1.18.3
DEBUG Requirement already installed: marshmallow==3.26.1
DEBUG Requirement already installed: typing-inspect==0.9.0
DEBUG Requirement already installed: anyio==4.8.0
DEBUG Requirement already installed: distro==1.9.0
DEBUG Requirement already installed: jiter==0.8.2
DEBUG Requirement already installed: sniffio==1.3.1
DEBUG Requirement already installed: tqdm==4.67.1
DEBUG Requirement already installed: regex==2024.11.6
DEBUG Requirement already installed: msgpack==1.1.0
DEBUG Requirement already installed: six==1.17.0
DEBUG Requirement already installed: starlette==0.46.1
DEBUG Requirement already installed: markdown2==2.5.3
DEBUG Requirement already installed: nh3==0.2.21
DEBUG Requirement already installed: rich==13.9.4
DEBUG Requirement already installed: shortuuid==1.0.13
DEBUG Requirement already installed: click==8.1.8
DEBUG Requirement already installed: filelock==3.17.0
DEBUG Requirement already installed: jsonschema==4.23.0
DEBUG Requirement already installed: protobuf==6.30.0
DEBUG Requirement already installed: fsspec==2025.3.0
DEBUG Requirement already installed: networkx==3.4.2
DEBUG Requirement already installed: nvidia-cublas-cu12==12.4.5.8
DEBUG Requirement already installed: nvidia-cuda-cupti-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cuda-nvrtc-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cuda-runtime-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cudnn-cu12==9.1.0.70
DEBUG Requirement already installed: nvidia-cufft-cu12==11.2.1.3
DEBUG Requirement already installed: nvidia-curand-cu12==10.3.5.147
DEBUG Requirement already installed: nvidia-cusolver-cu12==11.6.1.9
DEBUG Requirement already installed: nvidia-cusparse-cu12==12.3.1.170
DEBUG Requirement already installed: nvidia-cusparselt-cu12==0.6.2
DEBUG Requirement already installed: nvidia-nccl-cu12==2.21.5
DEBUG Requirement already installed: nvidia-nvjitlink-cu12==12.4.127
DEBUG Requirement already installed: nvidia-nvtx-cu12==12.4.127
DEBUG Requirement already installed: sympy==1.13.1
DEBUG Requirement already installed: triton==3.2.0
DEBUG Requirement already installed: huggingface-hub==0.29.2
DEBUG Requirement already installed: safetensors==0.5.3
DEBUG Requirement already installed: tokenizers==0.21.0
DEBUG Requirement already installed: h11==0.14.0
DEBUG Requirement already installed: parso==0.8.4
DEBUG Requirement already installed: ptyprocess==0.7.0
DEBUG Requirement already installed: wcwidth==0.2.13
DEBUG Requirement already installed: asttokens==3.0.0
DEBUG Requirement already installed: executing==2.2.0
DEBUG Requirement already installed: pure-eval==0.2.3
DEBUG Requirement already installed: httpcore==1.0.7
DEBUG Requirement already installed: argon2-cffi==23.1.0
DEBUG Requirement already installed: jupyter-events==0.12.0
DEBUG Requirement already installed: jupyter-server-terminals==0.5.3
DEBUG Requirement already installed: overrides==7.7.0
DEBUG Requirement already installed: prometheus-client==0.21.1
DEBUG Requirement already installed: send2trash==1.8.3
DEBUG Requirement already installed: terminado==0.18.1
DEBUG Requirement already installed: websocket-client==1.8.0
DEBUG Requirement already installed: babel==2.17.0
DEBUG Requirement already installed: json5==0.10.0
DEBUG Requirement already installed: soupsieve==2.6
DEBUG Requirement already installed: webencodings==0.5.1
DEBUG Requirement already installed: tinycss2==1.4.0
DEBUG Requirement already installed: fastjsonschema==2.21.1
DEBUG Requirement already installed: jsonpointer==3.0.0
DEBUG Requirement already installed: mypy-extensions==1.0.0
DEBUG Requirement already installed: latex2mathml==3.77.0
DEBUG Requirement already installed: wavedrom==2.0.3.post3
DEBUG Requirement already installed: markdown-it-py==3.0.0
DEBUG Requirement already installed: jsonschema-specifications==2024.10.1
DEBUG Requirement already installed: referencing==0.36.2
DEBUG Requirement already installed: rpds-py==0.23.1
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG Requirement already installed: argon2-cffi-bindings==21.2.0
DEBUG Requirement already installed: python-json-logger==3.3.0
DEBUG Requirement already installed: rfc3339-validator==0.1.4
DEBUG Requirement already installed: rfc3986-validator==0.1.1
DEBUG Requirement already installed: svgwrite==1.4.3
DEBUG Requirement already installed: mdurl==0.1.2
DEBUG Requirement already installed: cffi==1.17.1
DEBUG Requirement already installed: fqdn==1.5.1
DEBUG Requirement already installed: isoduration==20.11.0
DEBUG Requirement already installed: uri-template==1.3.0
DEBUG Requirement already installed: webcolors==24.11.1
DEBUG Requirement already installed: pycparser==2.22
DEBUG Requirement already installed: arrow==1.3.0
DEBUG Requirement already installed: types-python-dateutil==2.9.0.20241206
Audited 194 packages in 0.27ms
```

looks like everything is installed. but if i run `vllm --help`, it does not work

in addition, when i run `pip show vllm` i get `WARNING: Package(s) not found: vllm`

but if i run `uv pip show vllm` that package exists

### Platform

Linux 6.13.2-arch1-1 x86_64 GNU/Linux

### Version

uv 0.6.1 (c91ee82a8 2025-02-17)

### Python version

Python 3.12.6

---

_Label `bug` added by @da-the-dev on 2025-03-10 16:16_

---

_Comment by @zanieb on 2025-03-10 18:31_

Regarding `pip show`: sounds like you're using pip from another environment?

Use `uv pip install pip` or `uv venv --seed` to populate `pip`.

---

_Comment by @zanieb on 2025-03-10 18:32_

> when i run uv sync i expect that vllm would be installed, and i would be able to run vllm --help.

We don't active the environment, so you need to do `uv run vllm --help` or use `source .venv/bin/activate` first.

---

_Label `bug` removed by @zanieb on 2025-03-10 18:32_

---

_Label `question` added by @zanieb on 2025-03-10 18:32_

---

_Closed by @charliermarsh on 2025-03-12 23:06_

---

_Comment by @da-the-dev on 2025-03-14 10:25_

the issue was that i had pyenv installed that managed the env. uv and pyenv dont mix. i uninstalled pyenv and everything works as it should

---
