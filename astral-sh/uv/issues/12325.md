```yaml
number: 12325
title: "uv doesn't respect workspace dependences when defined as extras"
type: issue
state: open
author: szaher
labels:
  - needs-mre
assignees: []
created_at: 2025-03-19T23:50:05Z
updated_at: 2025-03-20T00:28:09Z
url: https://github.com/astral-sh/uv/issues/12325
synced_at: 2026-01-10T03:50:31Z
```

# uv doesn't respect workspace dependences when defined as extras

---

_Issue opened by @szaher on 2025-03-19 23:50_

### Summary

When using workspace as extra uv doesn't install/package the workspace dependencies along with the extra.

```bash
uv tree
```
```
Resolved 30 packages in 2ms
kubeflow-sdk v0.1.4
├── optimizer v0.18.0rc0 (extra: all)
│   ├── certifi v2025.1.31
│   ├── grpcio v1.71.0
│   ├── kubeflow-training v1.9.0
│   │   ├── certifi v2025.1.31
│   │   ├── kubernetes v32.0.1
│   │   │   ├── certifi v2025.1.31
│   │   │   ├── durationpy v0.9
│   │   │   ├── google-auth v2.38.0
│   │   │   │   ├── cachetools v5.5.2
│   │   │   │   ├── pyasn1-modules v0.4.1
│   │   │   │   │   └── pyasn1 v0.6.1
│   │   │   │   └── rsa v4.9
│   │   │   │       └── pyasn1 v0.6.1
│   │   │   ├── oauthlib v3.2.2
│   │   │   ├── python-dateutil v2.9.0.post0
│   │   │   │   └── six v1.17.0
│   │   │   ├── pyyaml v6.0.2
│   │   │   ├── requests v2.32.3
│   │   │   │   ├── certifi v2025.1.31
│   │   │   │   ├── charset-normalizer v3.4.1
│   │   │   │   ├── idna v3.10
│   │   │   │   └── urllib3 v2.3.0
│   │   │   ├── requests-oauthlib v2.0.0
│   │   │   │   ├── oauthlib v3.2.2
│   │   │   │   └── requests v2.32.3 (*)
│   │   │   ├── six v1.17.0
│   │   │   ├── urllib3 v2.3.0
│   │   │   └── websocket-client v1.8.0
│   │   ├── retrying v1.3.4
│   │   │   └── six v1.17.0
│   │   ├── setuptools v77.0.1
│   │   ├── six v1.17.0
│   │   └── urllib3 v2.3.0
│   ├── kubernetes v32.0.1 (*)
│   ├── protobuf v4.25.6
│   ├── setuptools v77.0.1
│   ├── six v1.17.0
│   └── urllib3 v2.3.0
├── trainer (extra: all)
│   ├── kubernetes v32.0.1 (*)
│   └── pydantic v2.10.6
│       ├── annotated-types v0.7.0
│       ├── pydantic-core v2.27.2
│       │   └── typing-extensions v4.12.2
│       └── typing-extensions v4.12.2
├── optimizer v0.18.0rc0 (extra: optimizer) (*)
├── trainer (extra: optimizer) (*)
└── trainer (extra: trainer) (*)
(*) Package tree already displayed

```
when I publish the package to pypi uv doesn't include/define neither the trainer nor the optimizer dependencies.
The dependencies are defined in the workspace pyproject.toml files not in the root file to keep them separated and clean.

### Platform

macos 15.3 Darwin 24.3.0 arm64

### Version

uv 0.6.5 (bcbcd0a1e 2025-03-06)

### Python version

Python 3.10.15

---

_Label `bug` added by @szaher on 2025-03-19 23:50_

---

_Comment by @charliermarsh on 2025-03-19 23:52_

Apologies, I don't understand the issue from the above. Can you please include a complete reproduction -- like a GitHub repo we can clone, a series of steps we can run, and the expected vs. actual outcome?

---

_Label `bug` removed by @charliermarsh on 2025-03-19 23:52_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-19 23:52_

---

_Comment by @szaher on 2025-03-19 23:56_

@charliermarsh Thanks for the quick response. 
the complete repo can be found here https://github.com/szaher/kubeflow-sdk-v2
the pypi release can be found here https://pypi.org/project/kubeflow-sdk/0.1.3/

I am trying to create a project with multiple workspaces. we want to release each workspace as separate extra of the main project. 
when we do that as per the repo above, `uv build` ships the workspace without its dependencies which is defined in the workspace `pyproject.toml` file

Thanks

---

_Comment by @charliermarsh on 2025-03-19 23:58_

The release looks correct, though, unless I'm misunderstanding? The extras are defined and point to those packages (which you need to publish separately):

```
Provides-Extra: optimizer
Requires-Dist: optimizer; extra == "optimizer"
Provides-Extra: trainer
Requires-Dist: trainer; extra == "trainer"
```

---

_Comment by @szaher on 2025-03-20 00:02_

Not sure how to do that using uv? Do I have to push the extras separately? if yes, can you please point me to the docs on how to do that using uv?
Thanks

---

_Comment by @szaher on 2025-03-20 00:26_

some how I managed to fix the issue but I don't like the way it's fixed ... (check this commit https://github.com/szaher/kubeflow-sdk-v2/commit/e1b3d1d4ccc9ea427d614faca4dc4b1945d2af24) 

I have root package `kubeflow` with two workspaces `trainer` and `optimizer` which I would like to ship them as extras to the root package.

I defined the `trainer` dependencies in it's own [pyproject.toml](https://github.com/szaher/kubeflow-sdk-v2/blob/master/kubeflow/trainer/pyproject.toml#L27-L30) 
and did the same for the `optimizer` [pyproject.toml](https://github.com/szaher/kubeflow-sdk-v2/blob/master/kubeflow/optimizer/pyproject.toml#L26-L34) 

in the root package if I wanted to define `trainer` and `optimizer` as extras with direct dependency on them as per https://github.com/szaher/kubeflow-sdk-v2/blob/9ebc9d4bcb18612ff1ab2c033eca94173392256b/pyproject.toml#L9-L20 
that works Ok and ships the extra but pip can't find the extra dependency (the code of the trainer and optimizer is already shipped with the root since the root defines both as workspaces) 
anyways, I had to re-add the dependencies of both packages again to root `pyproject.toml` https://github.com/szaher/kubeflow-sdk-v2/blob/master/pyproject.toml#L9-L36 to around that issue and it worked

@charliermarsh is there a way with `uv` to get the workspace dependencies from its `pyproejct.toml` file if it's being used as extras? that would make things cleaner and reduce the duplication ....

Thanks!

---
