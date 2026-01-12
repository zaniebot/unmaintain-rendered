```yaml
number: 15163
title: "Switch in `pyproject.toml` to use the project name as the venv name - or setting to specify `UV_PROJECT`"
type: issue
state: open
author: dmrauch
labels:
  - enhancement
assignees: []
created_at: 2025-08-08T09:56:54Z
updated_at: 2025-10-15T17:49:25Z
url: https://github.com/astral-sh/uv/issues/15163
synced_at: 2026-01-12T16:02:05Z
```

# Switch in `pyproject.toml` to use the project name as the venv name - or setting to specify `UV_PROJECT`

---

_@dmrauch_

### Summary

Hi all,

I am aware of some related feature requests and also of some of the counter arguments (see below). Bearing those in mind, I'd like to formulate this proposal.

## Motivation

My personal use case is having multiple repos, each with its own venv, and many of them containing Jupyter notebooks that I work on in VS Code using workspaces (i.e. multiple repos in a single VS Code window). My issue with a fixed venv folder name is that it is displayed as the Jupyter kernel name in VS Code. Therefore, it is hard to impossible to be sure which exact venv is used in a given notebook. Hence, I'd like to give a specific name to each venv, depending on the project to which it belongs.

I am aware of the `UV_PROJECT` and `UV_PROJECT_ENVIRONMENT` env vars, but setting env variables per folder in various terminals is tedious. The way of automating env vars per folder that I am aware of seems quite hacky and suboptimal.

## Proposed Solution

I could imagine two ways of addressing this
- Add a setting to use the project name for the venv folder as well - with a leading point. So
    ```toml
    [project]
    name = "my-project"

    [tool.uv]
    default-env-name = false  # default: true (use .venv)
    ```
    would create the venv in a folder called `.my-project`. Or instead of a boolean switch, it could of course also be a string enum.
- As an alternative that would be more flexible and - I'd argue - clearer, 
    ```toml
    [project]
    name = "my-project"

    [tool.uv]
    environment = ".my-project"  # default: ".venv"
    ```
    would also create the venv in a folder called `.my-project`.

## Related Feature Requests

- Setting for `UV_PROJECT_ENVIRONMENT` in `pyproject.toml` or `uv.toml`: #12587 and #13931
  - Counterargument: Syncing to arbitrary locations on your machine
- Central location for venvs: #1495
  - I see other peoples use cases - but I wouldn't like to have to use this mechanism for fixing the notebook kernel names because it is a different issue and because I find that local venvs are better handled by IDEs
  - Plus, depending on the final mechanism used to decide on the venv folder names, I'd perhaps even like to avoid those names (e.g. if they don't contain but are completely made of hashes)

Many thanks!

### Example

_No response_

---

_Label `enhancement` added by @dmrauch on 2025-08-08 09:56_

---

_Comment by @zanieb on 2025-08-08 11:37_

> My issue with a fixed venv folder name is that it is displayed as the Jupyter kernel name in VS Code

Can this otherwise be customized?

---

_Comment by @zanieb on 2025-08-08 11:39_

How is the kernel started / installed?

As a note, #14937 would solve this issue.

---

_Comment by @dmrauch on 2025-08-08 12:39_

> Can this otherwise be customized?

No, unfortunately not afaik.

> How is the kernel started / installed?

As far as I can tell, VS Code seems to distinguish between different "discovery" mechanisms. The ones that I am aware of are:
- Python environments
  - local venvs
  - conda envs
  - poetry envs
  - pyenv envs
  - global envs in certain other places
- Registered Jupyter kernels
- Jupyter servers

The discovery of local venvs as kernels works like a charm - with the one exception that it is not even possible to see the absolute path of the venv folder - hence the wish to customize the venv folder name.

#14937 def sounds very interesting and I have subscribed to it - nonetheless, it would mean abandoning the local venv. In my experience from using Jupyter notebooks from conda envs with VS Code, the discovery works much less reliably with non-local envs and I'd often have to find out the absolute path of the venv and then point VS Code to it manually.

---

_Comment by @zanieb on 2025-08-08 12:41_

https://github.com/astral-sh/uv/pull/14937 doesn't mean you'd have to abandon the local venv. `UV_PROJECT_ENVIRONMENT={project_name}` would be relative to your project still.

---

_Comment by @zanieb on 2025-08-08 12:42_

I think we should make a feature request to VSCode or Jupyter to respect the virtual environment prompt or some other metadata instead of just using the name of the directory.

---

_Comment by @dmrauch on 2025-08-08 12:54_

> UV_PROJECT_ENVIRONMENT={project_name} would be relative to your project still.

Ok, that sounds interesting indeed. The downside I can see with this is that it induces an env var that has to be coordinated and shared between developers working on the same project. And neither I myself nor the others might be in a position that this configuration holds globally on our systems. So to me the scope of the project seems to be the best possible place in which to specify the venv name.

> I think we should make a feature request to VSCode or Jupyter to respect the virtual environment prompt or some other metadata instead of just using the name of the directory.

At least VS Code is doing this for the registered Jupyter kernels. However, like with the central venvs, this means having a central location where venvs (or rather kernel definitions in this case) are stored, registered and collected. It is manageable indeed - but nothing beats the local auto-discovery in terms of comfort and ease of use ;-) 

---

_Comment by @zanieb on 2025-08-08 13:57_

I don't think I get what you're saying at the end. Why can't VS Code be changed to have better name inference for environments in projects?

---

_Comment by @dmrauch on 2025-08-08 17:22_

Let me rephrase - for uv-created venvs, there are 2 ways that VS Code can detect them as potential notebook kernels:

- the local venv folder discovery
- the list of registered Jupyter kernels

The first mechanism is the one I am using and the one that suffers from the issue described in the OP. However, it is the fastest and most reliable discovery mechanism, so I'd like to keep using it and that's why I'm thinking of ways of how to improve the experience. Now I'm no VS Code dev, but since it's a folder-based discovery of a "standard" python venv, I don't see which information VS Code could possibly use to automatically set a reasonable kernel name - other than the name of the folder containing the venv (which is what it does). Therefore, my line of thought is that it makes most sense to name the venv folder adequately.

The second mechanism, in contrast, involves the additional manual step of me registering the currently active venv as a Jupyter kernel in the central kernel list - e.g.
```sh
source .venv/bin/activate
ipython kernel install --user --name=my-kernel-123
```
Then, VS Code will additionally offer me to select the registered kernel called `my-kernel-123` (which points to the venv `.venv`). The kernel definitions are collected centrally in `~/Library/Jupyter/kernels/` (on MacOS) when the `--user` argument is specified and in some system path otherwise.

Ideally, I would like to avoid this second mechanism because it involves the manual kernel registration - and deletion once a kernel is no longer needed.

---

_Comment by @zanieb on 2025-08-08 17:47_

>  Now I'm no VS Code dev, but since it's a folder-based discovery of a "standard" python venv, I don't see which information VS Code could possibly use to automatically set a reasonable kernel name - other than the name of the folder containing the venv (which is what it does). 

I mean, they could just use the parent directory for a `.venv` directory? Or they could look in the `pyproject.toml` next to the virtual environment? Or we could write some data to the standard `pyvenv.cfg` file that they could read? There's plenty information they could use here.

We're trying to move towards standardizing virtual environment names in projects to help with editor discovery. Adding more ways to change the virtual environment name feels counterproductive. We're not the only ones that are pushing for `.venv` in projects either. I would rather they invest in better name inference since it will help more users by having a better default, rather than adding workarounds that motivated users can use.

---

_Comment by @geophpherie on 2025-10-15 17:20_

If i'm understanding this correctly it appears to be something that _should_ be solved if you set the `prompt` for the venv, but seemingly only on windows in my brief testing. If I do these steps:

`uv init my_proj`
`cd my_proj`
`uv venv --prompt my_prompt` <- force creation of the venv with a prompt

... uv add etc

then open a notebook in vscode - on windows the kernel discovery shows in the list as 
`my_prompt (Python 3.13.7) .venv\Scripts\python.exe`
but on Mac it doesn't pull the prefix and only says `.venv` as i think you describe.

So it's possible that a potential fix is through a bugfix / feature in VSCode i guess ....

If that were to be the case and VSCode ended up fixing it to show the prefix on both platforms - i could see the opportunity for an optimization around setting the prompt prefix w/o the extra `uv venv --prompt` step, but still preserve the `.venv` folder name.

EDIT: Actually i don't even think setting the prompt manually matters, on Windows it'll show the name of the project folder next to the environment anyway

---
