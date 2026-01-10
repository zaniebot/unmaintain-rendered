---
number: 12668
title: Using streamlit watch in uv workspaces
type: issue
state: open
author: NixBiks
labels:
  - question
assignees: []
created_at: 2025-04-04T11:25:33Z
updated_at: 2025-04-04T11:25:33Z
url: https://github.com/astral-sh/uv/issues/12668
synced_at: 2026-01-10T01:25:23Z
---

# Using streamlit watch in uv workspaces

---

_Issue opened by @NixBiks on 2025-04-04 11:25_

### Question


I've made this [repo](https://github.com/NixBiks/uv-workspaces-with-streamlit) is to investigate how to make [`uv workspaces`](https://docs.astral.sh/uv/concepts/projects/workspaces/) work properly with `streamlit`s hot reload feature.

### Project structure

```
├── apps
│   └── streamlit-app
│       ├── main.py
│       └── pyproject.toml
├── packages
│   └── hello-world
│       ├── pyproject.toml
│       └── src
│           └── hello_world
│               ├── __init__.py
│               └── py.typed
├── pyproject.toml
└── uv.lock
```


### The problem

If I run my streamlit app

```
uv run streamlit run apps/streamlit-app/main.py
```

then the hot reload works just fine if I make changes in `apps/streamlit-app`. However any changes to `packages/hello-world` is not picked up.


Streamlit suggests to update [`PYTHONPATH`](https://docs.streamlit.io/knowledge-base/using-streamlit/streamlit-watch-changes-other-modules-importing-app) but that's not working together with uv workspaces AFAIK.

Note I have asked the same question in [streamlit issues](https://github.com/streamlit/streamlit/issues/11037) since I'm not sure where this belongs.

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.12 (e4e03833f 2025-04-02)

---

_Label `question` added by @NixBiks on 2025-04-04 11:25_

---
