```yaml
number: 7186
title: "Should `uvx` use a project dependency if available?"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
  - uv tool
assignees: []
created_at: 2024-09-08T13:22:20Z
updated_at: 2025-09-15T12:21:30Z
url: https://github.com/astral-sh/uv/issues/7186
synced_at: 2026-01-10T03:23:52Z
```

# Should `uvx` use a project dependency if available?

---

_Issue opened by @zanieb on 2024-09-08 13:22_

Right now, `uvx` will run in an isolated environment even if the target tool is installed in the current project. For example, `uvx ruff` will not use the version of Ruff declared in the project (e.g., with `uv add ruff==0.3.0`). If we're in a project, it might make sense to respect the version there instead of ignoring it?

This may be a reasonable way to support defining tools in projects until we design and implement "project-level tool definitions" per #3560.

Related:

- https://github.com/astral-sh/uv/issues/6377
- https://github.com/astral-sh/uv/issues/5903#issuecomment-2336546258
- #3560 



---

_Label `uv tool` added by @zanieb on 2024-09-08 13:23_

---

_Label `needs-decision` added by @zanieb on 2024-09-08 13:23_

---

_Comment by @helderco on 2024-09-08 14:29_

My preference is to reserve `uvx` for stuff in `.venv/bin` (like npx) since `uv tool install ruff` can create symlinks for the tool's binaries in a place that can be added to the `$PATH` (like pipx) and run with simply `ruff`.

---

_Comment by @zanieb on 2024-09-08 14:44_

> My preference would be to use uv tool run, but place symlinks for the tools in a directory that can be added to the $PATH

Isn't this just `uv tool install`?

> That way, I could install ruff globally with uv tool install ruff, run it with just ruff check, and use uvx for stuff in .venv/bin (like npx with node_modules/bin).

Except `npx` will also run arbitrary versions without installing them, right? I'm not sure I follow.

---

_Comment by @helderco on 2024-09-08 15:06_

Sorry, I'm on my phone so I can't try things. Reworded my previous comment right after posting.

Didn't know npx could run commands without installing. I don't personally use it now but did a few years ago, when npx came out it was used just for easily running installed binaries in the project. 

For global installations I rather use the `$PATH`. I do use `uv run` a lot and would prefer uvx be used to shorten that. As for running without installing, could uvx try these in turn?
- in .venv
- in tools dir
- isolated

---

_Comment by @zanieb on 2024-09-08 17:06_

Yep that's the suggestion this issue tracks.

---

_Comment by @inoa-jboliveira on 2024-09-08 17:07_

Currently `uvx` is really just a shorthand for `uv tool run` but it could be a more intelligent command using context. 

Meaning it will either call `uv run` or `uv tool run` as necessary. And if you actually mean one or the other, you could force it by using the explicit form. If you bake "run" inside "tool run", you lose this ability. 

I don't know if that is feasible as these commands have different options. My original suggestion was to have a 3rd command but I am seeing it as more confusing to the end user than a smarter `uvx`



---

_Comment by @zanieb on 2024-09-08 17:10_

The idea here is that `uv tool run` would check the project for the dependency too. You'd use `--no-project` to ignore the project dependency declaration. I don't think `uvx` will ever have different semantics than `uv tool run` (it'd be too confusing).

---

_Comment by @matterhorn103 on 2024-10-17 08:32_

This actually seems to be two different questions, no?

1. Should `uv tool run ruff` use the version of `ruff` installed in the project, if there is one, rather than the global/tools version?
2. If `uv tool run ruff` uses a project version of `ruff,` should it be run in the project environment or in an isolated one?

Because so far as I can see (with my limited knowledge of the problem) it would be theoretically possible to do 1 but not 2, no? I.e. reuse the installed wheel but still run it in an isolated environment?

While I'm here, my feedback would be that this seems to me on the face of it not such a great idea. The whole "tool" interface is already a little challenging for newcomers conceptually (was for me, anyway). Assuming that both 1) and 2) are being proposed, this proposal would muddy the waters and make it even more confusing, as the behaviour of `uv tool run` would vary, and it might not always be clear what is being invoked. Learning and teaching becomes harder as it is no longer possible to break it down as "`uv tool` uses global/isolated packages and environments and `uv run` uses local/project packages and environments".

Sometimes it makes a big difference whether a tool-like package runs in an environment or not (e.g. `pyinstaller`), and then the clear delineation of the two behaviours is useful and crucial.

If anything I would say that `uvx` having different behaviour to `uv tool run` is *less* confusing than having *uv tool run* show context-dependent behaviour. But both are a little confusing.

If using the project dependency with `uv tool run` was implemented, I think it'd be much more user-friendly if it was off by default and turned on with e.g. `--use-project` rather than the other way round.

---

_Comment by @inoa-jboliveira on 2024-12-13 17:30_

I've been using uv run lately to have tools available in the current env of a project without having it installed as a dependency. Tools such as ones that are useful once in a blue moon such as checking for licenses of libs:

```
uv run --with pip-licenses -- pip-licenses
```

This is what I would hope for uvx to be when running inside a project. Notice this must run in the project env to find the libraries.

Reading the docs: [Relationship to uv run](https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run)

For uv run to behave like uvx, it would require `--no-project` and replace (optional) `--from` with `--with`.

So why can't uvx just have a `--project` flag and a `UVX_PROJECT=true` env var which would behave like `uv run`?

This would be very easy to implement and save us a lot of keystrokes and mental gymnastics to remember the usage.


---

_Comment by @alxmrs on 2025-02-04 23:15_

Hey! I'm trying to use `uv` as my everything dev tool in Python from here on out, and I find `uvx` / `uv run` to be really confusing. Specifically, I want to use `pytype` as a type checker for my project. This tool performs "transitive" type inference (for lack of a better term), meaning it infers types based on my projects code + all of its underlying dependencies. 

In short, I would expect to be able to run: 

```shell
uvx pytype **/*.py
```

But instead, I have to run: 
```shell
uv run --all-extras pytype **/*.py
```

Because the isolated environment in the former case doesn't have the full context of the project, and thus throws import related errors. 

---

_Comment by @zanieb on 2025-02-04 23:40_

Hm. At the very least, it seems like `uvx` could layer the tool environment on top of the project environment? That might fix that use-case, cc @charliermarsh (who has the context on our environment layering impl)

edit: This would be somewhat complicated because then we'd need to sync the project too? But the idea of project-level tools suggests this is okay / `--no-project` or `--isolated` can opt-out?

---

_Comment by @alxmrs on 2025-02-05 01:52_

Hey there, thanks for quickly replying. I think I may have spoken too soon. Whereas I was experiencing the issue in my description yesterday when using the `uv` Github Action, things are working properly now for me in my local environment. I was confused; I am experiencing an unrelated import error related to `pytype`, something like this: https://github.com/google/pytype/issues/455. That lead me to believe that isolation was the culprit. The following 
```shell
uvx pytype **/*.py
```
does in fact work as intended! My apologies -- I'm still doing my best to learn how to use `uv`. 

---

_Comment by @aliwo on 2025-02-13 00:06_

I want this feature! 
I use different mypy versions for different project. And I don't want to always specify the exact version everytime I run mypy

---

_Comment by @JakkuSakura on 2025-02-25 09:00_

I just tested pyright. If I run uvx, it doesn't use my enviornment and reports many dependencies are missing. I have to activate my venv first

---

_Comment by @mstarodub on 2025-02-25 22:06_

> Hm. At the very least, it seems like `uvx` could layer the tool environment on top of the project environment? That might fix that use-case, cc [@charliermarsh](https://github.com/charliermarsh) (who has the context on our environment layering impl)
> 
> edit: This would be somewhat complicated because then we'd need to sync the project too? But the idea of project-level tools suggests this is okay / `--no-project` or `--isolated` can opt-out?

Surprised this isn’t already possible, running an ipython repl in a uv project seems to require `uv add ipython`, which seems unnecessary as it's a dev dep.

---

_Comment by @zanieb on 2025-02-25 22:34_

`uv run --with ipython ipython` works fine, you don't need to add it as a dependency. `uvx` is just sugar to avoid an explicit `--with`.

---

_Comment by @Winand on 2025-04-08 08:33_

> `uv run --with ipython ipython` works fine, you don't need to add it as a dependency.

maybe `uv run` could implicitly add `--with` if a tool is not installed in the environment? Like `uv run ipython` or `uv run ptpython`.

---

_Comment by @dedebenui on 2025-05-22 08:50_

Adding my two cents, I like the present state of things:
- `uvx` never messes with the current project, if one exists
- `uv add --dev ruff==0.9.1` (say you want a specific version for that project), combined with `uv run ruff` lets me run tools in the context of my project

And indeed, if I want to run a one-off command in my project's environment, `uv run --with` is there.  I think the value of shaving a few characters off a command is minimal. I do agree however that `uv run --with ipython ipython` is a bit redundant but I would be wary of adding new syntax and/or implicit behavior. It's already the case that if you `uv tool install ipython`, you can `uv run ipython` inside a project, even if `ipython` is nowhere in `pyproject.toml`, and it will run in your `.venv` with access to your project's dependencies. I think this is good enough and intuitive.

I like `uvx` exactly because I can confidently run it anywhere and know that it's always running the same tool. Making `uvx` behave differently whether it is run in a project or not would require the dev to know which one is desired, at which point they can bother knowing which command to run.

> Hm. At the very least, it seems like `uvx` could layer the tool environment on top of the project environment? That might fix that use-case, cc [@charliermarsh](https://github.com/charliermarsh) (who has the context on our environment layering impl)

I've add bad experiences with conflicting packages in the past, for example with both PySide and PyQT in the same environment. I think that layering environments, even when resolving dependencies correctly, would lead to that kind of problems. Is there any instance of "layering" environments elsewhere?



---

_Comment by @EonesDespero on 2025-06-14 20:17_

> > `uv run --with ipython ipython` works fine, you don't need to add it as a dependency.
> 
> maybe `uv run` could implicitly add `--with` if a tool is not installed in the environment? Like `uv run ipython` or `uv run ptpython`.

Depending on what you mean, this could be a fine idea or a terrible idea, in my opinion.
If I misunderstood your comment, please, then correct me.

I am personally against of any tool installing things on its own without permission or opt-in.
The problem is that uv run is the main way to 

I think that trying to provide too much sugar is bad. If people really need to run the same command constantly, they are better off creating an alias for their shell. Otherwise uv will end up bloated and confusing.
I am against, but I wouldn't worry about a new syntax like "uvxw" that is uv run tool --with,
but I think that making it the default behaviour is a very bad idea.
IMO, uv run ... should be the smart equivalent to python ..., and not take decisions for you unless you explicitly say it.

Alternatively, uv run --tool ipython could be short hand for uv run tool --with ipython ipython (I predict many people searching the difference between uv tool run and uv run --tool, so maybe another name is better)

But again, I think that users creating an alias uvw='uv tool run --with' should suffice.

---

_Comment by @absynce on 2025-09-11 19:33_

Intent: Sharing my experience as a data point, and to help anyone else who finds themself here with a similar confusion. No expectations.

Experience:
As someone coming from a world with Node, this confused me because I cargo-culted my prior assumptions about how [npx](https://docs.npmjs.com/cli/v8/commands/npx) works. 😊

After reading through the docs, I understand the difference. Now I use `uv run` to have it pick up the version from `pyproject.toml`. If I had read more carefully, I would've seen the note in https://docs.astral.sh/uv/guides/tools/#running-tools 

> If you are running a tool in a [project](https://docs.astral.sh/uv/concepts/projects/) and the tool requires that your project is installed, e.g., when using pytest or mypy, you'll want to use [uv run](https://docs.astral.sh/uv/guides/projects/#running-commands) instead of uvx. Otherwise, the tool will be run in a virtual environment that is isolated from your project.
> 
> If your project has a flat structure, e.g., instead of using a src directory for modules, the project itself does not need to be installed and uvx is fine. In this case, using uv run is only beneficial if you want to pin the version of the tool in the project's dependencies.

I know Python handles packages differently from Node, but Python's package/dependency handling still confuses me sometimes. But, it sounds like it might be possible to have `uvx` behave somewhat similarly to `npx`. `ruff` and `ty` are prime examples of tools I want to lock the version for per project. At this point, I don't have a preference whether they are isolated from the project's other dependencies.

I thought I could do:
```shell
uv add --dev ty
uvx ty # This might run a different version from what's specified in `pyproject.toml`.
```

Instead, I need to do:
```shell
uv add --dev ty
uv run ty # Always runs the version in `pyproject.toml`.
``` 

So, yes, to me, Zanie's suggestion makes sense:
> If we're in a project, it might make sense to respect the version there instead of ignoring it?

Hope it helps! 

---

_Comment by @galah92 on 2025-09-15 12:21_

I got a similar incentive: running `ruff`/`ty` for neovim LSP. I currently do:

```lua
vim.lsp.config['ruff'] = {
  cmd = { 'uv', 'run', 'ruff', 'server' },
  filetypes = { 'python' },
  root_markers = { 'pyproject.toml', 'ruff.toml', '.ruff.toml', '.git' },
  settings = {},
}

vim.lsp.config['ty'] = {
  cmd = { 'uv', 'run', 'ty', 'server' },
  filetypes = { 'python' },
  root_markers = { 'pyproject.toml', 'ty.toml', '.git' },
  settings = {},
}
```

Which forces me to install them to the project (a good thing!).

However, it would be helpful to either:
1. Add a flag to this comment to run the global tool in case it isn't installed as part of the project.
2. As the issue suggest: make `uvx` run the local tool in case it is installed.

---
