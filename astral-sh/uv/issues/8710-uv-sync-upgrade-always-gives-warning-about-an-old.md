```yaml
number: 8710
title: "`uv sync --upgrade` always gives warning about an old extra for typer"
type: issue
state: closed
author: taranlu-houzz
labels:
  - needs-mre
assignees: []
created_at: 2024-10-30T21:44:18Z
updated_at: 2024-11-05T19:55:19Z
url: https://github.com/astral-sh/uv/issues/8710
synced_at: 2026-01-12T15:59:32Z
```

# `uv sync --upgrade` always gives warning about an old extra for typer

---

_@taranlu-houzz_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Previously, I was using an older version of `typer` where you needed to add the `typer[all]` extra to get full functionality. I deleted that extra since it is no longer needed, but for some reason, whenever I run `uv sync --upgrade`, I get warning messages about it. Seems like maybe this old dep config is sticking around in a cache somewhere?

```
❯ uv sync --upgrade
Resolved 79 packages in 956ms
warning: The package `typer==0.12.5` does not have an extra named `all`
Audited 75 packages in 0.28ms
```

```toml
[project]
dependencies = [
  .
  .
  .
  "sqlmodel>=0.0.22",
  "stamina>=24.3.0",
  "tenacity>=9.0.0",
  "typer>=0.12.5",
  "ujson>=5.10.0",
  "uvicorn[standard]>=0.32.0",
]
```

---

_Comment by @samypr100 on 2024-10-31 23:32_

Hello, do you have a way to reproduce? I wasn't able to reproduce. 

I don't see an extra in [typer ](https://github.com/fastapi/typer/blob/master/pyproject.toml) called `all` in 0.12.5.
It seems [0.11.x](https://github.com/fastapi/typer/blob/0.11.1/pyproject.toml) did have it.

When I installed 0.11.x with all and upgraded it to 0.12.5 I didn't get a warning unless `all` was still referenced in pyproject.toml.

Lastly, which version of `uv` are you using?

---

_Comment by @taranlu-houzz on 2024-10-31 23:41_

I need to try a minimal example as well, but what you are describing is more or less what I believe I did. I do have other deps that also rely on `typer` and it may be possible that they had `typer[all]` in their `pyproject.toml`, but I also did not see anything that looked like an extra related to `typer` anywhere in my project.

---

_Label `needs-mre` added by @charliermarsh on 2024-11-01 13:40_

---

_Renamed from "uv sync --upgrade always gives warning about an old extra for typer" to "`uv sync --upgrade` always gives warning about an old extra for typer" by @charliermarsh on 2024-11-01 13:40_

---

_Comment by @zanieb on 2024-11-01 13:49_

It seems likely the request comes from a transitive dependency?

We just display this warning whenever we look at a package request https://github.com/astral-sh/uv/blob/5ab860be20e75451c3c9c779c5f552b3fd0cb2ad/crates/uv-resolver/src/resolution/graph.rs#L347-L354

I don't think it'd be easy for us to display where the request came from in this context.

---

_Comment by @seapagan on 2024-11-05 08:47_

I was getting this exact warning today when i was upgrading a FastAPI app from Poetry to uv. Turns out I had forgotten to delete the OLD `.venv` from Poetry which still had an older version of **something** that wanted `typer[all]`. Deleted the `.venv` and issue was gone. 

EDIT: actually turns out it was another external library (ironically one of my own) that still used the `typer[all]` dep. Fixed this and all good so it's prob similar for your case

---

_Comment by @taranlu-houzz on 2024-11-05 18:07_

@zanieb @seapagan Okay, that makes sense. Thanks for the confirmation!

---

_Closed by @zanieb on 2024-11-05 19:55_

---
