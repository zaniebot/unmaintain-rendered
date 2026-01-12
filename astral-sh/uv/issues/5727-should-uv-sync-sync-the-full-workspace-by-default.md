```yaml
number: 5727
title: "Should `uv sync` sync the full workspace by default?"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-08-02T14:50:50Z
updated_at: 2025-10-30T06:09:48Z
url: https://github.com/astral-sh/uv/issues/5727
synced_at: 2026-01-12T15:58:58Z
```

# Should `uv sync` sync the full workspace by default?

---

_@charliermarsh_

If you have a virtual workspace, we do this. But for non-virtual workspaces, we sync the workspace root. It's a little bit of a footgun (see, e.g., https://github.com/astral-sh/ruff/pull/12622#issuecomment-2265469492).

---

_Label `preview` added by @charliermarsh on 2024-08-02 14:50_

---

_Comment by @charliermarsh on 2024-08-02 14:58_

There are a few different cases to consider...

- `uv sync` from the workspace root
- `uv sync -p foo` from the workspace root
- `uv sync` from a workspace member (e.g., `./foo`)

---

_Comment by @zanieb on 2024-08-02 14:59_

Does this mean that all workspace requirements would no longer need to be explicit?

---

_Comment by @charliermarsh on 2024-08-02 16:58_

I think it'd be something like:

- `uv sync` (and `uv run` by extension) always syncs the full workspace, regardless of whether you're in the top-level directory or a project.
- `uv sync -p` always syncs the specified package (and it could be the workspace root).


---

_Comment by @bluss on 2024-08-02 17:32_

I have a minor concern that `un run` would uninstall some other part of the workspace that an already running (concurrent) `uv run` is using - that's a drawback of the current behaviour, I mean.

---

_Comment by @charliermarsh on 2024-08-02 17:55_

In general `uv run` won't uninstall (unless your dependencies changed in some way) -- it just makes the changes necessary to get to a sufficient state, but could include some packages that you don't need.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Closed by @charliermarsh on 2024-08-20 20:08_

---

_Reopened by @charliermarsh on 2024-09-03 01:21_

---

_Comment by @charliermarsh on 2024-09-03 01:21_

This has come up a few times so leaving it open for more discussion.

---

_Comment by @jamesbraza on 2024-09-03 01:25_

I guess a good default behavior is to take a light touch, so maybe not `sync`ing the full workspace by default makes sense.

Perhaps we can add a CLI/config arg to `sync`, something like `--full-workspace`, and provide decent docs on the decision

---

_Comment by @vvuk on 2024-10-30 20:43_

>I guess a good default behavior is to take a light touch, so maybe not syncing the full workspace by default makes sense.

Hmm.. I'd suggest the opposite -- that the good default behaviour is the least surprising one. If I've set up a workspace, I've explicitly declared "I'm going to be working with all of these members". It's very strange that `uv sync` sets up a venv, but doesn't install anything (unless my workspace root also happens to be a "real" project vs. just a workspace container) -- even though it does a full resolve and creates a lock file.

---

_Comment by @charliermarsh on 2024-10-30 20:46_

I would argue that it would also be surprising, though, if any package could import any other package regardless of whether it was declared as a dependency, which is what would be enabled if we synced the entire workspace by default. We would lose any sort of enforcement of module boundaries.


---

_Comment by @vvuk on 2024-10-30 20:55_

But don't you end up in that state anyway, once you've synced multiple members of the workspace? e.g. from what you said above -- `uv run` won't uninstall, so you'll have things you don't need available in the environment.

---

_Comment by @charliermarsh on 2024-10-30 20:57_

`uv run --exact` will uninstall packages, as will `uv sync`, but yes `uv run` won't uninstall by default. That's a compromise, though, to make things work as expected with extras -- we don't want to _remove_ extras between `uv run --extra dev` to `uv run` invocation, since _that_ would be surprising to users.

---

_Comment by @vvuk on 2024-10-30 21:10_

I think that makes workspaces much less useful. For example, the comfyui/plugins scenario can't be supported reasonably. Ideally, I'd want to specify extras as a global thing for the workspace. In other words, I wouldn't have the ability to run workspace member `foo` with extra `thing` and workspace member `bar` without extra `thing`. If I want that, why am I in a workspace to begin with? (Or if I _really_ want that, `uv run --isolated` already exists.) Again, totally just my opinion, but to me:

- inside a workspace, there should be one global set of packages (the full resolution), with any extra sets globally applied across all members
- if I want to run a specific project with a different set of packages, then I should need to use `--isolated`

it shouldn't be up to `uv` to enforce perfect dependency declarations; that can be verified with `--isolated`, but otherwise it's a python ecosystem limitation that you can't really do this.

Edit: at the very least, an option to do this (sync everything) that can be set in the workspace root would be helpful. Then the actual default doesn't matter much.

---

_Comment by @vvuk on 2024-10-30 22:58_

I completely missed the mention of "virtual workspace" vs. project in the initial post here. (There's also no mention of "virtual workspace" on https://docs.astral.sh/uv/concepts/workspaces/) If I just remove the `[project]` section (which was a dummy section -- I thought I needed one!) then I think I'm getting the behaviour that I want, but I'm currently fighting with pytorch still.

---

_Comment by @charliermarsh on 2024-10-31 04:30_

The “virtual workspace” concept isn’t present in the docs because they’re considered legacy. We removed them in favor of non-packaged projects (projects without a build system), which you can find in the docs.

---

_Comment by @vvuk on 2024-10-31 05:12_

Ah ok. Please don't remove them even if legacy until there's another way to get the full-workspace sync behaviour -- it's the only way to make this work right now. non-packaged projects trigger the "sync just the project" behaviour.

---

_Comment by @charliermarsh on 2024-11-02 02:37_

You can now use `uv sync --all-packages` (same on `uv run`, `uv export`, etc.) to sync the full workspace.

---

_Closed by @charliermarsh on 2024-11-02 02:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-02 02:37_

---

_Label `enhancement` added by @charliermarsh on 2024-11-02 02:37_

---

_Comment by @vvuk on 2024-11-06 17:43_

Great, thank you! Is there a way to set this in the workspace pyproject file, so that it doesn't need to be passed in as a command line argument?

---

_Comment by @m-dziuba on 2025-01-29 07:39_

You could use direnv so that the command is ran automatically every time you enter the the main directory. Or set up an alias like `usa` for `uv sync --all-packages`.

---

_Comment by @krpatter-intc on 2025-02-13 19:34_

I see this is released and that the `uv sync --all-packages` runs. However, I can't tell the behavior change. It still seems that `uv sync --all-packages` will install/uninstall based on the current-working-dir. For example: running it in a workspace followed by running it in the root, will uninstall the dependencies of the workgroups.

---

_Comment by @charliermarsh on 2025-02-13 19:36_

@krpatter-intc -- Please create a separate issue with a clear reproduction that we can run ourselves, rather than commenting on a closed issue.

---

_Comment by @rosmur on 2025-10-29 04:11_

> You can now use uv sync --all-packages (same on uv run, uv export, etc.) to sync the full workspace.

requesting confirmation that this is still the correct method and the default `uv sync` behavior is not changed? Claude Code seems to think that it did

---

_Comment by @konstin on 2025-10-29 11:40_

Please check our documentation on workspaces, it is more accurate that LLms: https://docs.astral.sh/uv/concepts/projects/workspaces/

---

_Comment by @rosmur on 2025-10-30 06:09_

@konstin I did. nowhere in that page (or anywhere else in the docs) is this information stated cleanly/explicitly. 

Please understand that people are swtiching to using AI as the front end for docs - and this is arguably a sensible/pragmatic approach as reading docs requires time investment that many devs dont have/cant prioritize. It then behooves maintainers to ensure that a) docs are replete b) human and machine friendly. 

---
