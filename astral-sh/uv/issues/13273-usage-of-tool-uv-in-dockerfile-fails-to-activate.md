```yaml
number: 13273
title: "Usage of tool `uv` in Dockerfile fails to activate environment"
type: issue
state: open
author: brunolnetto
labels: []
assignees: []
created_at: 2025-05-02T19:55:15Z
updated_at: 2025-05-02T21:15:27Z
url: https://github.com/astral-sh/uv/issues/13273
synced_at: 2026-01-12T16:01:23Z
```

# Usage of tool `uv` in Dockerfile fails to activate environment

---

_@brunolnetto_

Hi all. I want to use uv on my Dockerfile. After more than 3 hours reading and implementing the [documentation](https://docs.astral.sh/uv/guides/integration/docker/#installing-a-project_1), without success to "activate the environment", I came here for help. THe repository is [here](https://github.com/brunolnetto/langgraph-pydantic-mvp). The command to run the application is `docker compose up -d --build`. 

After "succesfully building the application", following error pops up: 

```
   Building landgraph-pydantic-mvp @ file:///app
      Built landgraph-pydantic-mvp @ file:///app
Uninstalled 1 package in 2ms
Installed 1 package in 0.64ms
Bytecode compiled 6046 files in 1.03s
error: Failed to spawn: uvicorn
  Caused by: No such file or directory (os error 2)
```

I am sure, I am using it wrongly, but at this point, I am tired of trying new things out. Therefore, any help is appreciated.

---

_Comment by @charliermarsh on 2025-05-02 20:03_

I cloned the repo and it mostly "just worked" for me? I had to run `uv add langchain-openai`. Then I ran `docker compose up -d --build` and looked at the logs:

```
INFO:     Will watch for changes in these directories: ['/app']
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [131] using WatchFiles
```

(It's immediately failing because I don't have `OPENAI_API_KEY` set, but uvicorn starts without issue.)

---

_Comment by @brunolnetto on 2025-05-02 20:04_

You don't count. Tool `uv` is your favourite child, it will always obey you. 😅 . But it still is not working properly for me, with the same issue. :/

---

_Comment by @zanieb on 2025-05-02 21:01_

You mount your working directory as a volume

```
    volumes:
      - .:/app
```

This can clobber the existing environment as mentioned at https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container

See https://github.com/astral-sh/uv-docker-example/blob/ac09d87bf12d7b2d573a0e37c1d64f791ae31f10/compose.yml#L14-L21 instead

---

_Comment by @brunolnetto on 2025-05-02 21:15_

Thanks, @zanieb. You guys are so amazing, I feel blessed everytime I come here asking for help.

---
