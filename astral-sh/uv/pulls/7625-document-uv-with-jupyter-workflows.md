```yaml
number: 7625
title: Document uv-with-Jupyter workflows
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/jupyter
created_at: 2024-09-22T17:56:29Z
updated_at: 2025-01-21T18:30:26Z
url: https://github.com/astral-sh/uv/pull/7625
synced_at: 2026-01-10T11:44:28Z
```

# Document uv-with-Jupyter workflows

---

_Pull request opened by @charliermarsh on 2024-09-22 17:56_

## Summary

This is a work-in-progress as I actually want to change some behaviors here.

Closes https://github.com/astral-sh/uv/issues/6329.


---

_Label `documentation` added by @charliermarsh on 2024-09-22 17:56_

---

_@charliermarsh reviewed on 2024-09-22 18:03_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:36 on 2024-09-22 18:03_

Actually, it looks like this is solvable using kernel `ipython kernel install` (https://github.com/astral-sh/uv/issues/6329#issuecomment-2302252543).

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:36 on 2024-09-22 18:15_

I sort of think we should _not_ be discovering the tool environment here, though. Modifying the project environment isn't great, but modifying the tool environment is even worse! It means subsequent users of the tool have these same modifications. I'd propose that we skip tool and ephemeral environments when considering active virtual environments. \cc @zanieb 

---

_@charliermarsh reviewed on 2024-09-22 18:15_

---

_@charliermarsh reviewed on 2024-09-22 19:17_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:69 on 2024-09-22 19:17_

It would be nice if we could find a way to make this work without requiring that you install `ipykernel` into the project environment itself. (We don't want users to have to add `ipykernel` as a project dependency just to interact with their project in a notebook.)

---

_@bluss reviewed on 2024-09-22 22:40_

---

_Review comment by @bluss on `docs/guides/integration/jupyter.md`:17 on 2024-09-22 22:40_

I'm sure there are various opinions are about this, but `jupyter notebook` is the legacy interface and jupyterlab is the current interface. I don't know what relation other people have to this, would it give a better impression to use jupyterlab here? Command `jupyter lab`.

---

_@charliermarsh reviewed on 2024-09-22 23:36_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:17 on 2024-09-22 23:36_

Yeah I went back and forth. I'm fine to use `jupyter lab`, no big deal.

---

_@bluss reviewed on 2024-09-23 09:59_

---

_Review comment by @bluss on `docs/guides/integration/jupyter.md`:69 on 2024-09-23 09:59_

Why is that not wanted?

But the 'Uv Run' kernelspec I posted in the other issue seems to be the most normal way to achieve exactly that - it runs the kernel using `uv run --with ipykernel`. Maybe a vscode extension is a better user interface, most of my questions are around the interface and if it's worth it (Because vscode already picks up `.venv` natively, is it worth the fuss with kernelspecs or other ways to do it - or can those external configuration ways of working be put on the long term plan, that is working on better uv + vscode integration to make these cases easy or automatic in the long run.)

---

_@charliermarsh reviewed on 2024-09-23 12:42_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:69 on 2024-09-23 12:42_

Well, if I'm working on a project, and I want to play with that project in a notebook, I don't want to add notebook dependencies as dependencies to the _project_. I don't want them recorded in the lock, etc.

---

_Comment by @radames on 2024-09-23 18:26_

Just one note, following a fresh new setup with vscode
```
uv init my-notebook
cd my-notebook
uv add --dev ipykernel
uv run ipython kernel install --user --name=uv
code .
```
```
!uv pip install diffusers
/usr/bin/sh: 1: uv: not found
```
The notebook kernel couldn't find the correct env path for `uv` nor `pip` , not sure if it's specific to my env however `uv` is set correct via my `.zshrc` and `.profile` `. "$HOME/.cargo/env"`. Doing the same steps with `pip` doesn't lead to an issue 


---

_Comment by @charliermarsh on 2024-09-23 18:28_

Good call, I experienced the same (though I don't see that when running Jupyter directly).

---

_Review requested from @zanieb by @charliermarsh on 2024-09-23 18:34_

---

_Review comment by @bluss on `docs/guides/integration/jupyter.md`:139 on 2024-09-23 18:36_

s/device/dev/

I like the new updates :slightly_smiling_face: 

---

_Comment by @radames on 2024-09-23 18:45_

indeed `uv run --with jupyter jupyter lab` works, however it looks like there are still some PATH issues. `pyproject.toml` is not an error, just started it from a fresh dir

<img width="710" alt="image" src="https://github.com/user-attachments/assets/0eef1b7b-34b8-48c1-8708-db1e369ea66b">


---

_@bluss reviewed on 2024-09-23 18:46_

---

_Comment by @charliermarsh on 2024-09-23 18:46_

Do you mean that `!pip list` and `!nvidia-smi` are missing?

---

_Comment by @charliermarsh on 2024-09-23 18:55_

Does `!nvidia-smi` work in other Jupyter notebooks for you?

---

_Comment by @radames on 2024-09-23 19:12_

> Does `!nvidia-smi` work in other Jupyter notebooks for you?

yes, starting a jupyter lab from a new conda env, the env path are working correctly

```
conda create -n temp python=3.10 
pip install jupyterlab
jupyter lab  
```


---

_Comment by @charliermarsh on 2024-09-23 23:03_

I haven't been able to reproduce that unfortunately.

---

_@bluss reviewed on 2024-09-25 18:48_

---

_Review comment by @bluss on `docs/guides/integration/jupyter.md`:17 on 2024-09-25 18:48_

I think that's the best, it's what jupyter promotes as `#1`

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:9 on 2024-09-25 23:20_

Just personal preference 
```suggestion
If you're working within a [project](../../concepts/projects.md), you can start a Jupyter server
```

---

_@zanieb reviewed on 2024-09-25 23:20_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:13 on 2024-09-25 23:22_

Future: I wonder if we want `uvx --with-project jupyter lab`?

---

_@zanieb reviewed on 2024-09-25 23:22_

---

_@zanieb reviewed on 2024-09-25 23:24_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:16 on 2024-09-25 23:24_

"Within": Where's the notebook? Can we provide a link to the localhost URL where the notebook server starts by default then say "Within _a_ notebook".

Maybe instead of "any other `uv run` invocation" we should say "any other file in the project"?

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:21 on 2024-09-25 23:25_

```suggestion
more to it. However, if you need to install additional packages from within the notebook, there are a few extra
```

---

_@zanieb reviewed on 2024-09-25 23:25_

---

_@zanieb reviewed on 2024-09-25 23:27_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:47 on 2024-09-25 23:27_

Could be cool to have a command that emits the project name, so you could do `--name=$(uv ...)` as a one-liner that actually works in any project.

---

_@zanieb reviewed on 2024-09-25 23:27_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:53 on 2024-09-25 23:27_

For consistency: "When creating a new notebook, select the `project` kernel from the dropdown"

---

_@zanieb reviewed on 2024-09-25 23:28_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:54 on 2024-09-25 23:28_

"Then, you can use" or "Then use"

---

_@zanieb reviewed on 2024-09-25 23:29_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:91 on 2024-09-25 23:29_

```suggestion
start a Jupyter server at any time with `uv tool run jupyter lab`. This will run a Jupyter
```

---

_@zanieb reviewed on 2024-09-25 23:30_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:148 on 2024-09-25 23:30_

```suggestion
`!uv pip install pydantic` to install `pydantic` into the project's virtual environment without
updating the project's dependencies.
```

---

_@zanieb approved on 2024-09-25 23:31_

Small comments

---

_@charliermarsh reviewed on 2024-09-26 00:51_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:13 on 2024-09-26 00:51_

Yeah I kinda like that. The repeated `jupyter` would be nice to avoid.

---

_Merged by @charliermarsh on 2024-09-26 00:55_

---

_Closed by @charliermarsh on 2024-09-26 00:55_

---

_Branch deleted on 2024-09-26 00:55_

---

_Comment by @akilicarslan on 2025-01-18 15:34_

I am struggling to run black code formatter for jupyter file with uv and vscode. Format notebook or format cell does not work entirely.
Can you help me please? What am I missing?

---

_Comment by @zanieb on 2025-01-21 18:30_

@akilicarslan please open a new issue as described in #9452 

---
