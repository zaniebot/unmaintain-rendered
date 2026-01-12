```yaml
number: 12533
title: Add support for declarative tool installations (e.g., like Brewfile)
type: issue
state: open
author: michaeltinsley
labels:
  - enhancement
assignees: []
created_at: 2025-03-28T15:18:46Z
updated_at: 2026-01-08T15:05:07Z
url: https://github.com/astral-sh/uv/issues/12533
synced_at: 2026-01-12T16:01:05Z
```

# Add support for declarative tool installations (e.g., like Brewfile)

---

_@michaeltinsley_

### Summary

It would be great if uv had a way to store all installed tool installations into a standard file, similar to a homebrew brewfile.

My use case is I want to have tools installed with `uv tool install ...` added to a config file that I can track in git along with my brew installed libs in a brewfile, and my other dotfiles.

### Example

_No response_

---

_Label `enhancement` added by @michaeltinsley on 2025-03-28 15:18_

---

_Renamed from "Add a uv tool equivalent of a Brewfile" to "Add support for declarative tool installations (e.g., like Brewfile)" by @zanieb on 2025-04-01 19:44_

---

_Comment by @zanieb on 2025-04-01 19:45_

Yeah; this is on the roadmap, though it's not a high priority right now.

---

_Comment by @itcarroll on 2025-05-02 17:12_

@michaeltinsley Would you be okay with using `pyproject.toml` for this (see my near duplicate at #13268) or do you need to keep this separate from any project?

---

_Comment by @michaeltinsley on 2025-05-02 17:29_

@itcarroll - I think a pyproject.toml would work for my use case. I could dump that in `~/.config/uv/pyproject.toml`. 

For _my_ ideal workflow, I would have something like:

```toml
# ~/.config/uv/pyproject.toml
[dependency-groups]
tools = [
    "ruff",
]
```

which would give something like:
```shell
> uv tool list
ruff v0.11.8
- ruff
```

Then if I do `uv tool add black`, it would automatically add it
```shell
> cat ~/.config/uv/pyproject.toml
...
[dependency-groups]
tools = [
    "ruff",
    "black",
]
```

That would be my ideal flow - I guess I'm more interested in `uv tool add ...` being automatically persisted to a file than anything, if that makes sense

---

_Comment by @codethief on 2025-05-22 22:28_

@michaeltinsley While I agree that `pyproject.toml` would be the appropriate place to persist tools, I'm not sure about filing them under `[dependency-groups]`. Keep in mind that each tool is meant to live in a separate venv, so they are somewhat different from other dependencies.


---

_Comment by @itcarroll on 2025-05-23 13:42_

@codethief The `[dependency-groups]` table doesn't have any requirement that all packages listed in a group be installed in one environment, does it? Using a shared location for the list of tools seems better to me than putting it somewhere under `[tools.uv]`. Eventually `pipx` might pick up the behavior of installing tools from a given dependency group, for instance.

---

_Comment by @rabader on 2025-05-29 20:15_

If I could chime in, it'd be great if we could also specify if it is implied that with a "uv sync" command, "uv tool sync" runs first. In a project that requires the keyrings tool today, my environment fails to install/sync unless I have first installed those tools with:

`uv tool install keyring --with keyrings.google-artifactregistry-auth`

Even if those packages are defined as dependencies, "uv sync" fails unless I first have those tools installed in a separate command. It'd be nice to be able to define that in the pyproject.toml file. Unless there's already a way to do that and I just missed it.

---

_Comment by @amardeep on 2025-07-08 03:47_

My usecase is also similar.

I am working in a monorepo, with no top level pyproject.toml as of now. I would like to declaratively configure tools required for the project - for eg. sqlmesh / dbt / sqlfluff etc. These should be version locked, so other team members working on the project get the exactly same versions. At the same time, I don't want to be forced to activate a virtual environment, as it is a monorepo, and a developer would mostly we working in another virtual environment related to his work.

Ideally, I would like:
- Declarative config for monorepo tools that can be checked in as part of the monorepo
- No venv activation needed to use the tools
- Tools get installed in some bin directory, which developers can add to their path (or use direnv to add it)
- Tools remain in sync. For eg if i pull latest main branch, where the lockfile has changed, uv automatically updates the installed version
- Tools can be invoked directly, without requiring to use something like "uv run"



---

_Comment by @codethief on 2025-07-08 13:13_

@amardeep My use case is very similar to yours; however, now that you spelled out your ideal solution, to me it sounds very much like what [mise](https://mise.jdx.dev/) already provides. In fact, mise can already install tools from pip packages using uv, provided that `uv` is installed globally, see https://mise.jdx.dev/dev-tools/backends/pipx.html (in particular, the section on the `pipx.uvx` setting).

Of course there is something to be said about uv providing everything out of the box and not having to install mise separately. Then again, even in a monorepo with only Python projects it might very well be that people want to use tools not written in Python / not available as pip package. In other words: In an ideal world, uv would provide similar features as mise. Maybe it would make sense for uv & mise to collaborate on this topic instead of uv reinventing the wheel? (FWIW, mise [already integrates quite well](https://mise.jdx.dev/lang/python.html#mise-uv) with uv.)


---

_Comment by @amardeep on 2025-07-09 14:49_

Thank you for the mise recommendation @codethief. It does look like it might support my use case.

---

_Comment by @zacker150 on 2026-01-07 22:52_

Instead of `[dependency-groups]` why not `[tool.uv.tools]`? 

---

_Comment by @itcarroll on 2026-01-08 14:33_

> Instead of `[dependency-groups]` why not `[tool.uv.tools]`?

I [commented](https://github.com/astral-sh/uv/issues/12533#issuecomment-2904470469) in favor of `[dependency-groups]`, which already exists and serves the purpose fine: PyPA [says](https://packaging.python.org/en/latest/specifications/dependency-groups/#installing-dependency-groups-extras) "There is no syntax or specification-defined interface for installing or referring to dependency groups."

Do you have an argument for creating a new table at `[tool.uv.tools]` that would justify it being private to `uv`?

---

_Comment by @konstin on 2026-01-08 15:05_

The problem we need to address is that same tools need to be installed in the same venv (such as mypy), while other should be in an isolated environment with separate resolution that doesn't influence the production dependencies (such as black).

---
