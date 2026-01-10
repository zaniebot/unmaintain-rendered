---
number: 11672
title: "Dynamic dependencies, `cache-keys` and `uv add` inconsistencies ?"
type: issue
state: closed
author: cmayet
labels:
  - question
assignees: []
created_at: 2025-02-20T17:27:41Z
updated_at: 2025-02-21T09:41:51Z
url: https://github.com/astral-sh/uv/issues/11672
synced_at: 2026-01-10T01:25:09Z
---

# Dynamic dependencies, `cache-keys` and `uv add` inconsistencies ?

---

_Issue opened by @cmayet on 2025-02-20 17:27_

### Summary

When using `pyproject.toml`'s dynamic dependencies ( deps in a `requirements.in` file) and `cache-keys = [{ file = "requirements.in" }]`, `uv sync && uv add whatever_new_dep` does not fail as it should.

Dockerfile to build a  python lib project with dynamic dependencies in a requirements.in

*Dockerfile_ok*
```dockerfile
FROM ghcr.io/astral-sh/uv:0.6.1-debian-slim

WORKDIR /mre
RUN uv init --lib /mre

COPY <<EOF /mre/pyproject.toml
[project]
name = "mre"
version = "0.0.1"
readme = "README.md"
requires-python = ">=3.9"
dynamic = ["dependencies"]

[build-system]
requires = ["hatchling", "hatch-requirements-txt"]
build-backend = "hatchling.build"

[tool.hatch.metadata.hooks.requirements_txt]
files = ["requirements.in"]

EOF

COPY <<EOF /mre/requirements.in
pydantic
EOF
```
Build the image and exec the container

```sh
docker build -f Dockerfile_ok -t ok .
docker run -d --name c1 ok sleep infinity
docker exec -ti c1 bash
```
Let's try to add new deps / requirements using uv add. This should fail as build backends do not accept both static and dynamic dependencies (and uv can not handle dynamic dependencies).

```sh
uv sync # runs OK
uv add fastapi # fails as expected
```
`uv add` raises an exception as uv cannot handle dynamic dependencies. The build backend prevents uv from adding new deps. This is the expected outcome (though it would be great if uv could handle this specific use case but this is out of the scope of this ticket).

Now if we add a dependency in `requirements.in`, the documentation (https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) says that nothing should happen as uv doesn't look at requirements.in changes (uv's aggressive caching) : 
```sh
# manually add new dependency in requirements.in
echo "flask" | tee -a requirements.in
#and run uv sync
uv sync
```
but flask gets installed... **WHY !?!**

Now, following the doc's recommendations : (https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) : 

> Similarly, if a project reads from a requirements.txt to populate its dependencies, you can add the following to the project's
> pyproject.toml:
> [tool.uv]
> cache-keys = [{ file = "requirements.txt" }]

let's build a new image with the cache-keys metadata : 
*Dockerfile_bad*
```dockerfile
FROM ghcr.io/astral-sh/uv:0.6.2-debian-slim

WORKDIR /mre
RUN uv init --lib /mre

COPY <<EOF /mre/pyproject.toml
[project]
name = "mre"
version = "0.0.1"
readme = "README.md"
requires-python = ">=3.9"
dynamic = ["dependencies"]

[build-system]
requires = ["hatchling", "hatch-requirements-txt"]
build-backend = "hatchling.build"

## NEW LINES HERE ! 
[tool.uv]
cache-keys = [{ file = "requirements.in" }]
## 

[tool.hatch.metadata.hooks.requirements_txt]
files = ["requirements.in"]

EOF

COPY <<EOF /mre/requirements.in
pydantic
EOF
```

let's build the image and run the container : 
```sh
docker build -f Dockerfile_bad -t bad .
docker run -d --name c2 bad sleep infinity
docker exec -ti c2 bash
```


From inside the new container:
```sh
uv sync #all is OK
uv add fastapi # fails silently
# no exception is raised !!
cat pyproject.toml | grep fastapi   # uv added fastapi in pyproject.toml's dependencies
cat uv.lock | grep fastapi                # but did not sync .lock
uv pip list | grep fastapi                 # nor venv
```

`uv add` (**after a first `uv sync`**) fails silently, the new dependency is added to pyproject.toml but .lock and venv are not updated. 

Sorry for this very long post. Is that a bug or misunderstanding from me ?



### Platform

debian

### Version

0.6.2

### Python version

3.13.2

---

_Label `bug` added by @cmayet on 2025-02-20 17:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-21 05:05_

---

_Comment by @charliermarsh on 2025-02-21 05:20_

The first part can be explained by the fact that `uv add` modifies the `pyproject.toml` (and then we undo those modifications when the operation fails). So the modification time of the `pyproject.toml` changes, which invalidates the cached metadata. That's why the failed `uv add` still causes the subsequent `uv sync` to take affect, despite not including `requirements.in` in the cache keys.

---

_Comment by @charliermarsh on 2025-02-21 05:23_

The second part is explained by the fact that setting:

```toml
[tool.uv]
cache-keys = [{ file = "requirements.in" }]
```

...means that we _don't_ invalidate the metadata when the `pyproject.toml` changes. Setting `cache-keys` is _not_ additive, so by setting it to `"requirements.in"`, you're actually overriding the defaults!

---

_Label `bug` removed by @charliermarsh on 2025-02-21 05:23_

---

_Label `question` added by @charliermarsh on 2025-02-21 05:23_

---

_Comment by @cmayet on 2025-02-21 09:41_

Thank you @charliermarsh for your quick answers !

The first one was a bit tricky, you made it clear that it was a side effect of uv touching pyproject.toml (even though no change remains).
Running the following is indeed much more consistent than my previous test.
```sh
uv sync #runs OK
echo "flask" | tee -a requirements.in     # add a requirement in requirements.in
uv sync  # nothing happens as uv was not told to watch requirements.in
```

For second one, I understand that the modification of `cache-keys` I made **overrides** the default instead of **appending** to it. When running `uv add`, the new dependency is written in `pyproject.toml` but this file is ignored because of my `cache-keys = [{ file = "requirements.in" }`.

The desired outcome was indeed obtained by setting 
```toml
[tool.uv]
cache-keys = [{ file = "requirements.in" },
              { file = "pyproject.toml"}]
```

I think it could be made more obvious in the documentation that cache-keys **overrides** the default instead of **appending** to it.
 The following sentence (from https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) was a bit misleading to me

> To **incorporate other information into the cache key** for a given package, you can **add** cache key entries under tool.uv.cache-keys, which can include both file paths and Git commit hashes. 
 
and could be rephrased as
> To **override the default cache key** for a given package, you can **set** cache key entries under tool.uv.cache-keys, which can include both file paths and Git commit hashes. 

, with -maybe- a small example as 
```toml
[tool.uv]
cache-keys = [
              { file = "pyproject.toml"},
              { file = "setup.py"},
              { file = "setup.cfg"},
              { file = "new_file_to_watch" },
              ]
```
Thank you again :D

---

_Closed by @cmayet on 2025-02-21 09:41_

---
