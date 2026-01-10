---
number: 3097
title: "Add strict command mode to `uv run`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - help wanted
  - needs-design
assignees: []
created_at: 2024-04-17T16:44:28Z
updated_at: 2025-02-18T15:20:36Z
url: https://github.com/astral-sh/uv/issues/3097
synced_at: 2026-01-10T01:23:24Z
---

# Add strict command mode to `uv run`

---

_Issue opened by @zanieb on 2024-04-17 16:44_

This would only allow you to run a command provided by a package in the current environment.

Maybe we'd want to use a new flag `--strict` or re-use the `--isolated` flag.

---

_Label `enhancement` added by @zanieb on 2024-04-17 16:44_

---

_Assigned to @zanieb by @zanieb on 2024-04-17 16:44_

---

_Comment by @zanieb on 2024-04-17 22:48_

We could also enable this by default and consider a relaxed mode to allow targets outside the requirements.

Poetry's cool with running something else:
```
❯ poetry run echo test
test
```

---

_Comment by @patrick-kidger on 2024-04-18 08:56_

If you'll permit me to, I would like to jump in here with a suggestion.

I would **like** to think of `poetry run` as meaning "please act exactly like the shell I am used to, but in addition with the Python venv activated."

First of all, I think this is by far the most intuitive CLI experience. Second, it would also be consistent with the user experience of`source .venv/bin/activate`

_Unfortunately_, poetry doesn't do this: it doesn't use my current shell (either the login `$SHELL` or the one that I am currently using), and it necessarily also doesn't offer any of the aliases I have set up! (Super inconvenient.)

For this reason I actually have my own CLI tool to fix this. Extracting the relevant code, it basically looks like this:
```python
def run(commands: list[str]):  # e.g. running `mytool run foo bar` on the CLI will produce `commands = ["foo", "bar"]`
    shell_path = os.environ.get("SHELL", "/bin/sh")
    shell_name = pathlib.Path(shell_path).stem
    command, *args = commands
    if shell_name in {"sh", "bash", "zsh"}:
        # This `&& echo` is quite a subtle point.
        #
        # bash and zsh have some optimizations (and different optimizations to each other!) in which if they detect that
        # you're running something sufficiently simple, then they will skip creating a subshell and directly execute the
        # provided command. (Here's a reference: https://stackoverflow.com/a/56620122)
        #
        # In practice the speed difference is going to be small, and this optimization sometimes makes our `bash`
        # instance below (created in `subprocess.run`) visible to the end user. That is, their preferred shell might
        # *not* be the PPID of the command they run, if that shell optimizes itself out!
        #
        # We would prefer that this not happen: things occuring inside `mytool run` should feel the same as if you were
        # on your existing command line.
        command = command + ' "$@" && echo'
    elif shell_name == "fish":
        command = command + " $argv"
    else:
        raise Exception(f"Unsupported shell {shell_name} at {shell_path}")
    process = subprocess.run(
        [
            "bash",
            "-c",
            'source "$1" && shift && "$@"',
            "--",
            f"{_get_venv_path()}/bin/activate",
            shell_path,
            "-c",
            command,
            "--",
            *args,
        ],
        cwd=pathlib.Path.cwd(),
        check=False,
    )
    sys.exit(process.returncode)
```
I expect that probably still takes a bit of work to get portable across Windows, non-{zsh,bash,fish} shells, etc, but you see the essence of it.

This is now much more useful. For example, this means that I can now easily start my `$EDITOR` inside the correct venv, which is what is needed for the LSP to operate correctly.

Do you think it would make sense for `uv run` to operate under this model instead?

---

_Comment by @samypr100 on 2024-04-24 01:25_

@patrick-kidger That aligns with your statement here https://github.com/astral-sh/uv/issues/1910#issuecomment-2060719717. What I'm not entirely sure if it simplies implementation to do this on `uv run` or `uv shell` either way. One can argue `uv shell` is an alias to `uv run $SHELL` from this perspective and you'd end up spawning a new shell, and having to do the same level of plumbing `uv shell` would need.

---

_Comment by @zanieb on 2024-04-24 13:04_

Very interesting, looks _hard_ . We might want to track that separately from this issue as it's a very different idea. I could see a flag to enable / disable that behavior?

---

_Comment by @patrick-kidger on 2024-04-24 18:59_

I'm glad it seems interesting!

Which bit would be tricky -- the handling of shells that aren't on the blessed list?

IIUC this is already a limitation of `uv` when `activate`ing -- c.f. the need for a separate `activate.fish` -- so I don't think this would impose any new restriction on compatibility.

---

_Comment by @Vigilans on 2024-05-25 10:48_

I am expecting `uv run` to work more like `conda run`, just as what @patrick-kidger describes:

> I would like to think of poetry run as meaning "please act exactly like the shell I am used to, but in addition with the Python venv activated."
> 
> First of all, I think this is by far the most intuitive CLI experience. Second, it would also be consistent with the user experience ofsource .venv/bin/activate

The ability to use aliases does not matter much to me.

Currently, `uv run` forcibly requires `pyproject.toml` to exist:
```
$ uv run ls
warning: `uv run` is experimental and may change without warning.
error: No `pyproject.toml` found in current directory or any parent directory
```
Where I have a `.venv` folder in the same directory, and `uv pip install` just works. It is very counter-intuitive that a `pyproject.toml` is required to run `uv run`: I want to run command as in the specified venv, why do I need a project for it? 

E.g. A virtual env may be created for a collection of user-wide terminal cli binaries, and I want to use `uv run` in the folder containing `.venv` to manage in the venv. Here the concept of `project` is naturally absent.

---

_Comment by @charliermarsh on 2024-05-25 12:52_

> It is very counter-intuitive that a pyproject.toml is required to run uv run: I want to run command as in the specified venv, why do I need a project for it?

Not speaking to what we'll eventually ship here, but just to be clear, `uv run` isn't intended to be used with `uv pip install`. It's part of a totally distinct set of APIs that are actively being worked on, and those APIs are _all_ oriented around the concept of a project and workspace. In that context, it is a lot more intuitive that a project is required -- _none_ of the APIs make sense outside of a project.

The initial thinking behind this issue was that we might want to provide something that's separate from that project API, to let users run a command in an existing virtual environment without activating it. Conceptually, that could be thought of as `uv venv run` or something, though I'm not actually proposing that as the API.


---

_Comment by @charliermarsh on 2024-05-25 12:54_

To be clear, I _do_ actually think we should have some form of `uv run` that works with the `uv pip` APIs (I don't know whether it's a flag, or a separate command, or just ignoring the project if there's no `pyproject.toml`), but `uv run` itself in its current form is not intended to be used alongside the `uv pip` APIs, which is where that confusion is coming from.


---

_Comment by @Vigilans on 2024-05-25 13:11_

> In that context, it is a lot more intuitive that a project is required -- none of the APIs make sense outside of a project.

So, now we have following venv managers:
* conda: `conda run`
* pyenv: `pyenv exec`
* [nvm](https://github.com/nvm-sh/nvm) (nodejs): `nvm run` / `nvm exec`
* [goenv](https://github.com/go-nv/goenv) (golang): `goenv exec`

They're all capable of:
* Install specified version of python/nodejs/go
* Have first-class `run`/`exec` that runs a command in specified environment without requirement for a project

And here we have `poetry` and `uv` where they:
* Can only work with a pre-existing installation of python
* Requires a project to use `run` command

For people who thought `uv` to be a venv manager analog to `conda`, `pyenv`, `nvm` and `goenv`, now they should actually regard `uv` as alternative for `poetry` as project manager and shall not expect a first-class `run` command that works like all above venv managers?

---

_Comment by @charliermarsh on 2024-05-25 13:24_

> For people who thought uv to be a venv manager analog to conda, pyenv, nvm and goenv, now they should actually regard uv as alternative for poetry as project manager and shall not expect a first-class run command that works like all above venv managers?

No, this isn't quite the right framing. We plan to support _both_ workflows -- that's why we shipped `uv venv` and `uv pip`. `uv` will be capable of installing specified versions of Python, and we'll probably also add a first-class `run` or `exec` command that works with an existing environment without _managing_ it. You'll have all the tools you need to install Python, manage Python versions, create virtual environments, manipulate them, etc., just as you would with `virtualenv`, `pip`, `pyenv`, etc. You can and will be able to use `uv` as an alternative to those other tools you mentioned.

Separately, though, we want to ship higher-level APIs (at a similar level of abstraction to Poetry) with a more opinionated interface, for users and projects that don't want to operate at that low-level of environment management. The existing `uv run` was implemented as part of that API. So if we want a first-class `run` or `exec` command to execute in an environment without a project (i.e., as an alternative to `source .venv/bin/activate`), that's totally fine, I'm just explaining that the existing `uv run` (which is genuinely work-in-progress) was not intended for that use-case.


---

_Comment by @charliermarsh on 2024-05-25 13:33_

For now though I'll take a look at enabling `uv run` to work outside of an environment. I think it used to do that and I changed it.

---

_Comment by @Vigilans on 2024-05-25 13:46_

> You'll have all the tools you need to install Python, manage Python versions, create virtual environments, manipulate them, etc., just as you would with virtualenv, pip, pyenv, etc. You can and will be able to use uv as an alternative to those other tools you mentioned.

That is what I initially thought `uv` would be and why opt-in it. By comparing the behavior of `run` in `conda` and `poetry`, I was trying to model the user's expectation towards a manager, because `uv run` is a top-level command. Unlike `uv venv run`, the behavior of `uv run` may impose a stronger implication of what this tool focuses. 

Personally I think `uv run` can work in both scope (project or venv). 
* It can accept a venv folder to run in a specified environment.
* When executing without venv, it may keep the current behavior to search for a python project and works the project way (`--strict` or `isolated` can apply). 
* Or furthermore, if no `pyproject.toml` found but `.venv` folder exists in current directory, use it as specified venv, just like `uv pip` does.

---

_Comment by @zanieb on 2024-05-25 14:08_

I appreciate the conversation, but could you describe the behavior you're looking for in a separate issue please? This issue is supposed to be focused quite narrowly on a `uv run` mode in which commands that are not in the environment are not allowed.

For what it's worth: I think you're suggesting things that we're already planning on doing (and have done previously), but, as Charlie mentioned, `uv run` is in the middle of being built and is changing all the time as we explore the best way to implement things.

---

_Referenced in [astral-sh/uv#3836](../../astral-sh/uv/issues/3836.md) on 2024-05-25 15:02_

---

_Referenced in [astral-sh/uv#6711](../../astral-sh/uv/pulls/6711.md) on 2024-08-28 13:53_

---

_Unassigned @zanieb by @zanieb on 2025-02-18 15:20_

---

_Label `help wanted` added by @zanieb on 2025-02-18 15:20_

---

_Label `needs-design` added by @zanieb on 2025-02-18 15:20_

---
