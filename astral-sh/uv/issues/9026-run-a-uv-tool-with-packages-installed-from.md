```yaml
number: 9026
title: Run a uv tool with packages installed from pyproject.toml
type: issue
state: open
author: Levelleor
labels: []
assignees: []
created_at: 2024-11-11T18:49:30Z
updated_at: 2025-05-22T22:20:58Z
url: https://github.com/astral-sh/uv/issues/9026
synced_at: 2026-01-12T15:59:40Z
```

# Run a uv tool with packages installed from pyproject.toml

---

_@Levelleor_

If I install packages via:
```shell
uv sync
or
uv pip install -r pyproject.toml
```
Then try to run pytest on the project via a tool
```shell
uv tool run pytest
```
It won't work because:
> ModuleNotFoundError: No module named 'boto3'

Which is part of the main dependencies block in pyproject.toml.

To overcome this I could do either of:

1) 
```shell
uv sync
uv run pytest
```
2) 
```shell
uv tool run --with boto3 pytest
```
3) (i am assuming this will also install dev packages every time a tool is ran)
```shell
uv tool run --with-requirements pyproject.toml pytest
```

The second one isn't worth it in my case due to the number of tools I may need to add, and the last one will re-link (or in my case it copies over the files as a fallback option) the packages on every run for that specific tool.

I'd like to be able to run the tool in the same environment as the `uv run pytest` would run in. This is useful when I want to enforce a tool on users of a CI pipeline without affecting their project dependencies with "add". While I could run something along the lines of `uv sync --with pytest` and then have the ability to execute it I'd like to know whether a simpler solution is available to me. 

I also have found that `tool run` comes with an `--isolated` flag, which I am assuming is on by default from the output of the example above? 
```
--isolated
    Run the tool in an isolated virtual environment, ignoring any already-installed tools
```

So I am not sure if I can de-isolate the tool to reuse the isolated environment of a project .venv instead. 


---

_Comment by @codethief on 2025-05-22 22:20_

I think this is a great suggestion! This could go hand in hand with introducing [project-level tools](https://github.com/astral-sh/uv/issues/3560#issuecomment-2719148617) by adding an option to include the project's "prod" dependencies in a given project tool's isolated venv.

---
