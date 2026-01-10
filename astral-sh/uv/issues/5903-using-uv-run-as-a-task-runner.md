```yaml
number: 5903
title: "Using `uv run` as a task runner"
type: issue
state: open
author: my1e5
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-08-08T11:00:55Z
updated_at: 2026-01-05T02:08:40Z
url: https://github.com/astral-sh/uv/issues/5903
synced_at: 2026-01-10T03:11:31Z
```

# Using `uv run` as a task runner

---

_Issue opened by @my1e5 on 2024-08-08 11:00_

For those of us migrating over from Rye, one of its nice features is the built-in task runner using `rye run` and `[tool.rye.scripts]`. For example:
```
[tool.rye.scripts]
hello = "echo Hello from Rye!"
```
```
$ rye run hello
Hello from Rye!
```
It could have some more features - here is a selection of feature requests from the community:
* https://github.com/astral-sh/rye/issues/695
* https://github.com/astral-sh/rye/issues/652
* https://github.com/astral-sh/rye/issues/930
* https://github.com/astral-sh/rye/issues/1243

A lot of these requested features are things that other 3rd party tools currently offer. I thought it might be useful to highlight a few other tools here, in particular because they also integrate with the `pyproject.toml` ecosystem and can be used with uv today.

* ### Poe the Poet

  https://github.com/nat-n/poethepoet
  ```
  uv add --dev poethepoet
  ```
  ```
  [tool.poe.tasks]
  hello = "echo Hello from poe!"
  ```
  ```
  $ uv run poe hello
  Poe => echo Hello from 'poe!'
  Hello from poe!
  ```
* ### taskipy

  https://github.com/taskipy/taskipy
  ```
  uv add --dev taskipy
  ```
  ```
  [tool.taskipy.tasks]
  hello = "echo Hello from taskipy!"
  ```
  ```
  $ uv run task hello
  Hello from taskipy!
  ```
  
Perhaps these can serve as some inspiration for a future `uv run` task runner and also in the meantime offer a solution for people coming over from Rye looking for a way to run tasks.

---

_Comment by @chrisrodrigue on 2024-08-08 21:07_

Relevant comment from another issue: https://github.com/astral-sh/uv/issues/5632#issuecomment-2267115729

---

_Comment by @chrisrodrigue on 2024-08-08 21:08_

PDM supports this: https://pdm-project.org/latest/usage/scripts/

---

_Label `enhancement` added by @charliermarsh on 2024-08-09 02:51_

---

_Label `preview` added by @charliermarsh on 2024-08-09 02:51_

---

_Comment by @charliermarsh on 2024-08-09 02:52_

Yeah we plan to support something like this! We haven't spent time on the design yet.

---

_Comment by @nikhilweee on 2024-08-09 20:45_

The pyproject standard already supports `[project.scripts]`, so uv may not need to use its own table.
https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#creating-executable-scripts

---

_Comment by @charliermarsh on 2024-08-09 21:21_

`[project.scripts]` is a little different -- that's used to expose specific Python functions as executable scripts, and we do support that already.

---

_Comment by @chrisrodrigue on 2024-08-09 23:27_

Perhaps naming this section `tool.uv.tasks` or `tool.uv.aliases` could help disambiguate that. 

---

_Comment by @nikhilweee on 2024-08-10 21:31_

Or maybe `[tool.uv.run]` to be consistent with the command `uv run`. Or we could even think about `[tool.uv.commands]`.
I'm not a big fan of `[tool.uv.scripts]` since it conflicts with `[project.scripts]` and I myself got [confused](https://github.com/astral-sh/uv/issues/5903#issuecomment-2278772262) before.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @cdwilson on 2024-08-23 17:39_

This is the main thing I missed coming from hatch: https://hatch.pypa.io/dev/config/environment/overview/#scripts

---

_Comment by @chrisrodrigue on 2024-08-23 19:10_

+1 to @nikhilweee suggestions. I think “command” reflects the intent/concept.

Hatch has an “environment” concept and supports running commands namespaced to an environment like so

```
hatch run test:cov
```

where “test” is a user-defined environment (with a dependency group) and “cov” is a user-defined command for that environment.

 ```toml
[tool.hatch.envs.test]
dependencies = [
  "pytest",
  "pytest-cov",
  "pytest-mock",
  "freezegun",
]

[tool.hatch.envs.test.scripts]
cov = 'pytest --cov-report=term-missing --cov-config=pyproject.toml --cov=src'

[[tool.hatch.envs.test.matrix]]
python = ["3.8", "3.9", "3.10", "3.11", "3.12"]
```

I would be curious to hear the use cases of nesting dependency groups and commands into “environments” like this rather than defining them at the top-level (i.e. `[tool.uv.commands]`/`[tool.uv.dev-dependencies]`).

---

_Comment by @pietroppeter on 2024-08-28 10:29_

since it has not been mentioned yet, adding as  a possible inspiration for design of tasks also pixi: https://pixi.sh/latest/features/advanced_tasks/



---

_Comment by @metaist on 2024-08-28 23:39_

I happen to be writing a cross-project task runner that supports a bunch of formats (e.g. rye, pdm, package.json, Cargo.toml; even uv's workspace config).

For what it's worth, almost all python runners use `tool.<name>.scripts` for task config (presumably inspired by npm's `package.json` format), so it's somewhat of an easier upgrade path for people coming from other tools.

https://github.com/metaist/ds

---

_Comment by @inoa-jboliveira on 2024-09-08 04:51_

Also related to this thread, I wish uvx was `uv run` instead of `uv tool run` when inside a project.

`uv tool run` seems like something you would do to play with a tool, like ruff. But then you will eventually `uv add ruff --dev` and forever keep writing `uv run ruff check` instead of `uvx ruff check` which won't respect the locked version and will be in different virtualenv. It also means the tool could be running on a different Python (does not apply to ruff, but any other python lib) and all sorts of weird stuff can happen.

I know `uv run stuff` is still short, but it could be 4 keystrokes shorter.

Regarding `uv run` being a task runner it means people will type it waaaaay more often than `uv tool run`. 
I would appreciate a dedicated command like `uvr` 




---

_Comment by @zanieb on 2024-09-08 13:22_

@inoa-jboliveira I opened a dedicated issue for that https://github.com/astral-sh/uv/issues/7186

---

_Comment by @nikhilweee on 2024-09-10 20:48_

Putting together some thoughts about semantics. This issue is about adding support for running arbitrary instructions specified in `pyproject.toml`. I deliberately use the term _instruction_ to avoid using any of the other terms under consideration (command, tool, script, etc). 

### What do we call these instructions?

Lots of existing tools refer to them as "scripts".

1. `npm` and `yarn` have first class support for scripts. Users can define them in `package.json`
1. `composer` also uses the same term. Users can define them in `composer.json`
1. `pdm` takes inspiration from npm and also uses the term `scripts`. Users can define them in `[tool.pdm.scripts]`
1. `rye` follows suit. Custom scripts are defined in `[tool.rye.scripts]`
1. `hatch` also uses the term scripts, although they are tied to environments. Defined in `[tool.hatch.envs.<env>.scripts]`

It seems advantageous to just go with the term "scripts" because it is the de-facto standard. As noted by another user https://github.com/astral-sh/uv/issues/5903#issuecomment-2316413223, this would also reduce friction for users coming to `uv` from other package managers. That said, this approach has a major flaw because it overlaps with the concept of entry points defined in `[project.scripts]`. Entry points expose certain python functions as executable scripts, but do not allow arbitrary commands. Furthermore, `[project.scripts]` has already been established in [PEP-0621](https://peps.python.org/pep-0621/#entry-points), as the official spec. So what about other terms?

Another option is to use the term "tasks"

1. `pixi` uses the term "tasks". Users can define them in the `[tasks]` table in `pixi.toml`
1. `bundler` uses rake tasks. Although it resembles entry points, `sh` is supported.
1. `grunt` and `gulp` also use the term "tasks". Although they are task runners, not package managers.
1. `gradle` uses the term "tasks", defined in `build.gradle`

Another option is to call them "executables". `dart` uses this term in `pubspec.yaml`

We could also use "commands", although I wasn't able to find existing tools which use this term.

After settling on a name, an obvious thing to do is to let users define instructions in the `[tool.uv.<name>]` table.

### How do we invoke these instructions?

There are two options here.

1. Overload existing `uv run <instruction>` (follows from `npm run <script>`)
1. Add a new subcommand `uv invoke` / `uv command` / `uv task`

### How should these instructions be specified?

PDM's [documentation](https://pdm-project.org/latest/usage/scripts/#user-scripts) around user scripts is pretty evolved, with support for a bunch of features.

1. `cmd` mode, `shell` mode, `composite` mode
3. Specify env vars and env files for each script
4. Specify working dir and site packages for each script
5. Specify order of arguments
6. Specify pre and post scripts
7. `call` a function from a python script (entry point)

[Rye](https://rye.astral.sh/guide/pyproject/#toolryescripts) has its own format, which is a subset of PDM features.

1. Specify env vars and env files for each script
2. `chain` multiple scripts one after the other
3. `call` a function from a python script (entry point)

I hope this serves as a starter for discussing additional details for this feature.

---

_Comment by @charliermarsh on 2024-09-10 21:13_

(Nice comment, thank you!)

---

_Comment by @Xdynix on 2024-09-10 21:17_

One nice thing about PDM is that if a command is not recognized as a built-in, it is treated as `pdm run`. Thus, `pdm foobar` would be shorthand for `pdm run foobar`, which executes the command defined in `[tool.pdm.scripts.foobar]`.

---

_Comment by @gdamjan on 2024-09-10 21:25_

IMHO, `pdm` has the most extensive support for these `scripts` and I would personally like to see the same support in `uv` too. And if so, best to support the same `pyproject` section too :scream:. It's especially nice when one doesn't have to use any other tools like Makefiles (ugh).

Alas, one can argue that the `pdm` support for scripts is feature-creep for `uv`, in which case the `rye` model works too :)

---

_Comment by @patrick91 on 2024-09-11 09:52_

> Furthermore, [project.scripts] has already been established in [PEP-0621](https://peps.python.org/pep-0621/#entry-points), as the official spec. So what about other terms?

I wonder if would be a good time to maybe standardise this? I don't know if a pep is required, but since we have some many package managers for python it would be nice if we can have one way to define these instructions

---

_Comment by @stur86 on 2024-09-19 09:49_

I think "tasks" works fine as a name, but this is definitely a needed feature. Otherwise we need to fall back on a Makefile or custom Python/bash scripts, which gets needlessly clumsy.

---

_Comment by @chrisrodrigue on 2024-09-19 12:25_

> I think "tasks" works fine as a name

For reference, vscode uses the “tasks” nomenclature and they are specified in json.

From https://code.visualstudio.com/docs/editor/tasks
```json
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Run tests",
      "type": "shell",
      "command": "./scripts/test.sh",
      "windows": {
        "command": ".\\scripts\\test.cmd"
      },
      "group": "test",
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    }
  ]
}
```

There are a lot of implementations as others in the thread have pointed out. I am partial to PDM’s approach although I think that it can be greatly simplified. For example, specifying the command type as `cmd`, `call`, `composite` seems too granular and this could just be abstracted from the user. If given a string like `uv run ruff check` , assume a command. If given a list of strings, assume a composite command. If given a python module path and function, assume a function call. 

I think that this tasks concept can be aided by the development dependency group concept and should be developed with them in mind. A task is usually a development related action and always relies on dependencies, whether it is an OS built-in such as `echo` or whether it’s a python library like `ruff`, so having a way to specify them for a command (similar to the way dependencies can now be specified in comments for standalone python scripts) should be strongly considered. A way to specify a dependency group, platform, and python version for a named command could add a lot of value.

Supporting arbitrary shell syntax could be tricky because it is not portable and attempts to work around it are usually not DRY. Python directly solves the shell problem by being the cross platform scripting language that Bash and Batch are not, and I think that its use should be encouraged over the latter. 



---

_Comment by @chrisrodrigue on 2024-09-19 12:29_

There is a related discussion here: https://discuss.python.org/t/a-new-pep-to-specify-dev-scripts-and-or-dev-scripts-providers-in-pyproject-toml/11457

Unsure if there are any formal PEPs on the topic yet.

---

_Comment by @Kludex on 2024-09-22 12:27_

This is a good example on how I see as a good practice using `poetry` and [`poe`](https://github.com/nat-n/poethepoet):

https://github.com/copier-org/copier/blob/ee9918957cb2bb2abd19898336382d90078383c8/pyproject.toml#L79-L101

---

_Comment by @seapagan on 2024-09-22 19:33_

To add to @Kludex comment, i have been using `poe` for ages, and still do with `uv`. An example from one of my current projects:

```toml
[tool.poe.tasks]
pre.cmd = "pre-commit run --all-files"
pre.help = "Run pre-commit checks"

mypy.cmd = "mypy . --strict"
mypy.help = "Run mypy checks"
format.help = "Format code with Ruff"
format.cmd = "ruff format ."
ruff.help = "Run Ruff checks"
ruff.cmd = "ruff check --output-format=concise ."

test.help = "Run tests using Pytest"
test.cmd = "pytest"
"test:watch".cmd = "ptw . --now --clear"
"test:watch".help = "Run tests using Pytest in watch mode"

changelog.cmd = "github-changelog-md"
changelog.help = "Generate a changelog"
```

Other projects have more entries for mkdocs and other tools.

Poe is quite well used, it seems that using `[tool.uv.tasks]` and similar task syntax would be elegant.

---

_Comment by @darthtrevino on 2024-09-23 21:15_

I would love to see built-in task management `poethepoet`; one feature I'd love to see would be first-class workspaces support similar to yarn: (https://yarnpkg.com/cli/workspaces/foreach)

e.g. something like

## Top-level toml
```toml
[tool.uv.tasks]
check = "uv workspaces foreach check --topological"
build = "uv workspaces foreach build"
```

or maybe...

```toml
[tool.uv.tasks.check]
foreach_workspace = "check"
workspace_order = "topological"

[tool.uv.tasks.build]
foreach_workspace = "build"
workspace_order = "parallel"
```

## Project Toml
```toml
[tool.uv.tasks]
check = ["_ruff", "_pyright"]
build = ["_generate_stuff", "_build_library"]
_ruff = "..."
_pyright = "..."
_generate_stuff = "..."
_build_library = "..."
```

---

_Comment by @datnguye on 2024-09-28 03:36_

This is a blocker when I tried to move from `poetry`&`poe` to `uv` today. 

Here is what existing in my `pyproject.toml`:

```
[tool.poe.tasks]
git-hooks = { shell = "pre-commit install --install-hooks && pre-commit install --hook-type commit-msg" }
format = [
  {cmd = "autoflake ."},
  {cmd = "black ."},
  {cmd = "isort ."},
]
lint = [
  {cmd = "black --check ."},
  {cmd = "isort --check-only ."},
  {cmd = "flake8 ."},
]
test = [
  {cmd = "pytest . -vv"},
]
test-cov = [
  {cmd = "pytest --version"},
  {cmd = "coverage run -m pytest ."},
  {cmd = "coverage report --show-missing"},
  {cmd = "coverage xml"},
]
build-doc-and-serve = [
  {cmd = "mkdocs build"},
  {cmd = "mkdocs serve"}
]
```

Hopefully to see this feature added soon! Thanks

---

_Comment by @seapagan on 2024-09-28 06:30_

> This is a blocker when I tried to move from `poetry`&`poe` to `uv` today.

Since `poe` is standalone and works very well with `uv` its not exactly a 'blocker' (`poe` is an extra dep even with `Poetry`)
all those scripts will work fine inside your venv, I've recently moved 3 projects from `Poetry` to `uv` and everything just works.

It would be nice to have it supported without an extra dependency for sure, though until then just keep using poe🤷‍♂️.

---

_Comment by @datnguye on 2024-09-28 11:31_

> > This is a blocker when I tried to move from `poetry`&`poe` to `uv` today.
> 
> Since `poe` is standalone and works very well with `uv` its not exactly a 'blocker' (`poe` is an extra dep even with `Poetry`) all those scripts will work fine inside your venv, I've recently moved 3 projects from `Poetry` to `uv` and everything just works.
> 
> It would be nice to have it supported without an extra dependency for sure, though until then just keep using poe🤷‍♂️.

Ah very good point! Totally agreed, thanks for dust 🙏 

---

_Comment by @harkabeeparolus on 2024-10-03 12:02_

Regarding Poe the Poet... I really like how their Poetry plugin allows it to [hook into the builtin poetry commands](https://poethepoet.natn.io/poetry_plugin.html#hooking-into-poetry-commands).
Their specific example is using a "pre_build" hook to "Optimise static assets for inclusion in the build", which sounds super useful.

---

_Comment by @lordmauve on 2024-10-04 16:25_

I've been trying to adopt [thx](https://thx.omnilib.dev/en/latest/) as a multi-version task runner.

The basic premise is that you configure jobs in `pyproject.toml`:

```toml
[tool.thx]
python_versions = ["3.10", "3.11", "3.12"]
requirements = ["requirements-dev.txt"]

[tool.thx.jobs]
pytest = "pytest --cov"
```

Then `thx pytest mypy` builds a venv per interpreter in parallel, installs the package with pinned requirements, and runs `pytest` for each interpreter.

It also has a `--watch` mode that stays running and repeats the operation shortly after anything changes.

I like that it is multi-version by default, parallel by default, and the configuration in pyproject.toml is very simple and flat.

---

_Comment by @AndreuCodina on 2024-10-06 09:38_

For me, it's interesting to run tasks in parallel with a single command (e.g. `uv run task start-development`). For example, I want to run an MLflow server and execute a FastAPI project in development mode, and if I cancel it (Ctrl + C on the console), both are stopped.

```bash
$ uv run mlflow server --host 127.0.0.1 --port 5000

$ fastapi dev src/main.py
```

Another thing it's interesting, when I have a monorepo with N projects with .NET, the IDE allows me to change, with the UI, the projects I want to run, without overwhelming me, but I don't know what it'd be the equivalent with `uv`.

---

_Comment by @Xdynix on 2024-10-06 09:52_

> For me, it's interesting to run tasks in parallel with a single command (e.g. `uv run task start-development`). For example, I want to run an MLflow server and execute a FastAPI project in development mode, and if I cancel it (Ctrl + C on the console), both are stopped.

@AndreuCodina I once had a similar requirement, and I found that there are many things to consider, such as whether to terminate other commands after one command is terminated, and whether to allow graceful termination before hard termination. So the scope might be too broad for `uv`.

If Node is installed in your environment, you can use `npx concurrently "uv run mlflow server --host 127.0.0.1 --port 5000" "fastapi dev src/main.py"`. [Honcho](https://github.com/nickstenning/honcho) and [Supervisor](https://github.com/Supervisor/supervisor) can also be considered. (I ended up reinventing the wheel for a Python version of `concurrently`)

---

_Comment by @elyxlz on 2024-10-16 19:21_

i would love this

---

_Comment by @dpgraham4401 on 2024-10-17 11:11_

I haven't seen it mentioned, so I think it's worth asking, how would these scripts/tasks be handled with workspaces and nested pyproject.toml?

Intuitively, I would think scripts/tasks would be run using the pyproject.toml in the closest parent directory. Scripts are always executed in the context of the directory with the pyproject.toml file. 

For example, running `uv run migrate` in any subdirectory of workspace `A` runs the `migrate` command in workspace `A`'s pyproject.toml. If no `migrate` script is found in `A`'s pyproject.toml, `uv run` complains and errors out (even if there's a `migrate` command in the root pyproject.toml). 

NPM allows users to [issue a command in the context of a workspace](https://docs.npmjs.com/cli/v7/using-npm/workspaces#running-commands-in-the-context-of-workspaces). For example running `npm run test --workspace=a`. I'm not sure how cargo handles this.

I'm not sure what the best way is, but I'm super glad the astral team has plans to add this feature. My team uses pdm's script section heavily and i don't want to give them whiplash going from pdm -> uv + poe -> uv. 

---

_Comment by @phihung on 2024-10-23 14:11_

Hi. I created a small, dependency-free tool to manage scripts directly from pyproject.toml:
https://github.com/phihung/tomlscript

I came across this thread after implementing my own solution… Silly me.
However, there are some differences in the API design between tomlscript and existing solutions, as well as features that may not make it into the official uv implementation.

I hope the following example can provide some useful ideas for the discussion.


### Config Example

```toml
[tool.tomlscript]
# Start dev server ==> this line is the documentation of the command
dev = "uv run uvicorn --port {port:5001} myapp:app --reload"

# Linter and test 
test = """
uv run ruff check
uv run pytest --inline-snapshot=review
"""

# Generate pyi stubs (python function) ==> Execute python function
gen_pyi = "mypackage.typing:generate_pyi"

# Functions defined here can be reused across multiple commands
source = """
say_() {
    echo $1
}
"""
```

### Usage

```
alias tom="uv run tom"  # alias tom="uvx tomlscript"

tom  # list commands
tom dev --port 8000
tom gen_pyi  --mode all  # execute python function with arguments
```


---

_Comment by @strangemonad on 2024-10-23 15:22_

FWIW, I tried to capture some of my wish list here - https://notes.strangemonad.com/Some+thoughts+on+python+workspaces. It's a bit broader in scope than just the task runner aspect but touches on how I'd like to see task runner commands, multi-project workspaces and consistent lifecycle commands work nicely together

---

_Comment by @harkabeeparolus on 2024-10-23 18:13_

> Hi. I created a small, dependency-free tool to manage scripts directly from pyproject.toml: https://github.com/phihung/tomlscript

First of all, I have to say that I appreciate the effort, and the terse config style. 👍🏻  The syntax is perhaps a little _too_ magical for my tastes, but it does look nice.

That said, I admire what I find to be a slightly terrifying YOLO implementation of `_find_doc()` and `is_pyfunc()`. 👀  Heuristics at their finest! Well done. 😁

---

_Comment by @kasvtv on 2024-11-12 13:19_

I believe this is one of the most major features that `uv` is missing that other projects do have.

Adding support for this would bridge a major part of the feature gap!

---

_Comment by @ljtruong on 2024-11-14 00:18_

> To add to @Kludex comment, i have been using `poe` for ages, and still do with `uv`. An example from one of my current projects:
> 
> ```toml
> [tool.poe.tasks]
> pre.cmd = "pre-commit run --all-files"
> pre.help = "Run pre-commit checks"
> 
> mypy.cmd = "mypy . --strict"
> mypy.help = "Run mypy checks"
> format.help = "Format code with Ruff"
> format.cmd = "ruff format ."
> ruff.help = "Run Ruff checks"
> ruff.cmd = "ruff check --output-format=concise ."
> 
> test.help = "Run tests using Pytest"
> test.cmd = "pytest"
> "test:watch".cmd = "ptw . --now --clear"
> "test:watch".help = "Run tests using Pytest in watch mode"
> 
> changelog.cmd = "github-changelog-md"
> changelog.help = "Generate a changelog"
> ```
> 
> Other projects have more entries for mkdocs and other tools.
> 
> Poe is quite well used, it seems that using `[tool.uv.tasks]` and similar task syntax would be elegant.

Thanks! I was using poethepoet for a while with poetry, didn't know I could use it in any pyproject.toml. This is enough for now

---

_Comment by @AtticusZeller on 2024-11-14 00:36_

what's the downside of pure shell instead of task runners such  as poe?

---

_Comment by @Xdynix on 2024-11-14 00:47_

> what's the downside of pure shell instead of task runners such  as poe?

Using pure shell might need to activate the Python environment each time. Like invoking `source ./.venv/bin/activate` each time opening the project. Or have to add `uv run` before many commands like `uv run mypy .`, `uv run pre-commit ...`, which is frustrating.

And using task runners can also create shorthand for long commands. Like using `pdm lint` for `ruff check --fix && ruff format`.

Indeed these can all be done by some other approaches, but these tasks are often project-related and makes more sense to store as part of project config. This also allows them to be shared with other collaborators.

---

_Comment by @AtticusZeller on 2024-11-14 04:22_

Thank you for the comprehensive explanation!

It does help understand the benefits of task runners.

We've been pondering the choice between pure shell and task runners in my [new Python template repo](https://github.com/AtticusZeller/python-uv). Unfortunately, I chose pure shell in the end. I'd like to share my perspective to offer others additional insights:

> Using pure shell might need to activate the Python environment each time (like invoking `source ./.venv/bin/activate`) each time opening the project, or having to add `uv run` before many commands like `uv run mypy .`, `uv run pre-commit ...`, which is frustrating.

1. In local development, terminals opened in VSCode will automatically activate your `venv` after Python environment selection, so `uv run` and `source ...` are not needed in this scenario.

2. For remote environments like GitHub CI, or when we need to manually activate the environment, a single `uv run bash yourshell.sh` is sufficient.

> And using task runners can also create shorthand for long commands. Like using `pdm lint` for `ruff check --fix && ruff format`.

3. Long commands can be auto-completed from your completion suggestions or command history with zsh plugins when running once.

4. Multiple commands look cleaner, are easier to control, and can be extended or combined harmoniously in one shell script (like running `lint` or `mypy` before `pytest`). Writing dynamic execution logic in `pyproject.toml`, which is meant for static configuration, feels weird to me.

So in my opinion, using a task runner feels redundant.

---

_Comment by @Xdynix on 2024-11-14 04:34_

@AtticusZeller Yes, that's true. There is always multiple approaches to make life easier.

What I mind is that these methods require you to set up your environment separately (such as configuring IDE to recognize and automatically activate virtual environments, configuring shell auto-completion, etc.). Not everyone has the same configuration. Using a task runner can standardize it and make it independent of the environment.

This way, when I or anyone else checked out the repository in any environment, they only need to have `uv` installed (which only takes one line of command) to immediately get started with the development process (linting, testing, dev server, etc.). And don't have to copy-paste some long commands first.

Because of these requirements of my use case, the task runner is more attractive to me.

---

_Comment by @AtticusZeller on 2024-11-14 05:20_

@Xdynix Thank you for sharing your perspective! 

I understand your point about standardization and ease of use. However, I'd like to offer a different views.

I believe this might be asking too much from `uv`. Here's why:

1. Basic development skills like environment management, IDE configuration, and shell operations should be fundamental knowledge for team development. These are **not** just Python-specific skills, but **universal** tools that benefit all development work.

2. These common development practices can easily solve the standardization problem: Team documentation for environment setup or  Standard shell scripts
   
3. These **universal** tools and skills are valuable beyond just Python projects, also can be applied to various development scenarios

4. Instead of avoiding these basics and implementing task execution in `pyproject.toml`, wouldn't it be better to focus on building strong **fundamental** skills, or keep tools focused on their primary purposes

The current solution feels like **over-packaging** - binding script execution tightly with `uv` when these problems can be **naturally** solved through **common** development practices. While task runners are powerful, I believe keeping tools focused on their **core** responsibilities (like `uv` on package management) and addressing standardization through team practices is a more sustainable approach.


---

_Comment by @Xdynix on 2024-11-14 05:34_

@AtticusZeller (Just some off-topic chitchat)

> A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.
>                       -- <https://docs.astral.sh/uv/>

I thought that one selling point of `uv` was to solve all the needs of Python environment in one stop, not just package management.

> Basic development skills like environment management, IDE configuration, and shell operations should be fundamental knowledge for team development.

I totally agree. But I don't really want to force other people's toolboxes, especially when it's an informal collaboration, such as an open source project, a lab course, etc. Some people like VS Code, some like PyCharm, not to mention vim users (and notepad users?). And I'm too lazy to maintain a set of documentation for each common tool. Therefore I prefer to provide only minimal but sufficient developer experience support.

---

_Comment by @harkabeeparolus on 2024-11-14 06:55_

> what's the downside of pure shell instead of task runners such as poe?

Here's the kicker for me -- **Windows support**. I'm in a mixed team with about half the people on Windows / Visual Studio (the old one, not VS Code), half on macOS with PyCharm or VS Code, and some funny people using Vim/Neovim on WSL or Linux proper. Sure, I can whip out some Makefiles and bash scripts, but they _will not work reliably_ for all developers, on all environments.

One way to tackle this is with policy and tooling. We could force our team to all use Unix. Or we could use Vagrant.

But a better solution, in my opinion, is Python. It's already portable _by design_. The standard library `pathlib`, `shutil`, and `subprocess.run` are a little more verbose than shell scripts, but not by too much. Writing our project admin tasks in portable Python and baking them into `pyproject.toml` or another task runner _just works_. We've started using [nox](https://nox.thea.codes) to build our projects instead of Makefiles for this reason.

---

_Comment by @sisp on 2024-11-14 09:11_

uv offers a unified experience across all platforms, combining the aforementioned tools in a single binary, which is amazing. But IMHO, a task runner should be language-agnostic, allowing to join forces for its development and maintenance, having the same features in all projects no matter the language, and avoiding the need to learn a new tool (for essentially the same thing) for each language. A popular and powerful cross-platform task runner is [Task](https://taskfile.dev/), and it is also available as a [Python package](https://pypi.org/project/go-task-bin/) (merely shipping the compiled Go binary) to ease its installation in Python projects.

Although it appears convenient to have yet another task runner integrated with uv, I'd vote against it for the mentioned reasons.

---

_Comment by @harkabeeparolus on 2024-11-14 10:03_

> The current solution feels like **over-packaging** - binding script execution tightly with `uv` when these problems can be **naturally** solved through **common** development practices. While task runners are powerful, I believe keeping tools focused on their **core** responsibilities (like `uv` on package management) and addressing standardization through team practices is a more sustainable approach.

I absolutely do not agree that scripts or tasks would be "over-packaging".

* [Rye has scripts built in](https://rye.astral.sh/guide/pyproject/#toolryescripts) (and [uv is the successor/replacement for rye](https://lucumr.pocoo.org/2024/2/15/rye-grows-with-uv/)).
  * In addition, since Rye was intended to be a "cargo for Python", it also has a bunch of pre-defined tasks; `rye fmt`, `rye lint`, `rye test` (in addition, of course, to `rye build` and `rye publish`). This is similar to [cargo's pre-defined build commands](https://doc.rust-lang.org/cargo/commands/build-commands.html).
* [Hatch has scripts built in](https://hatch.pypa.io/latest/config/environment/overview/#scripts).
* [PDM has scripts built in](https://pdm-project.org/en/latest/usage/scripts/#user-scripts).
* Poetry does not have scripts or tasks built in, but it does have a plugin system. This allows [Poe the Poet to be installed as a plugin](https://poethepoet.natn.io/poetry_plugin.html), integrated with Poetry. This allows Poe to perform admin tasks automatically, e.g. when you run `poetry build`. And installing it is as simple as `poetry self add 'poethepoet[poetry_plugin]'`.
  * Speaking of custom actions that run automatically when you build, [cargo also has custom build scripts](https://doc.rust-lang.org/cargo/reference/build-scripts.html) built in.
* Heck, even [Pipenv has scripts](https://pipenv.pypa.io/en/latest/scripts.html) built in.

(See also the [previous overview](https://github.com/astral-sh/uv/issues/5903#issuecomment-2341984468) by @nikhilweee.)

In other words -- scripts or task runner functionality is **not** over-packaging. It's **standard and expected** functionality in every other modern workflow packaging tool for Python. In fact, if **uv** could not offer user-definable tasks, or a plugin system, it would be _worse than every other modern packaging tool_ in this regard.

---

_Comment by @AtticusZeller on 2024-11-14 11:44_

> Rye has scripts built in (and uv is the successor/replacement for rye)... In fact, if uv could not offer user-definable tasks, or a plugin system, it would be worse than every other modern packaging tool in this regard.

I respectfully disagree with this perspective for several key reasons:

1. **Popularity ≠ Necessity**

The prevalence of task runners in existing tools doesn't automatically justify their inclusion in new ones. This appears to fall into the "appeal to popularity" fallacy. Just because Rye, Hatch, PDM, and others implement task runners doesn't mean it's the optimal design choice.and task runner absolutely not denote **modern** 

2. **Unix Philosophy and Practical Value**

The Unix philosophy advocates for tools that do one thing and do it well. When we examine the **practical value** of built-in task runners because most "tasks" are **simply command aliases** that can be handled by shell scripts and when it comes to complex workflows often require proper build tools anyway, 

Adding task running to uv would dilute its core responsibility of package and Project management while providing minimal practical benefit.

3. Python vs Rust Context

> ...since Rye was intended to be a "cargo for Python"

This comparison overlooks crucial differences

Rust's ecosystem is built around a single, standardized toolchain,but Python's ecosystem is intentionally **diverse**, with different tools serving different needs, so forcing Cargo-like standardization onto Python goes against its philosophy of "_we're all consenting adults here_"


---

_Comment by @wu-clan on 2024-11-14 13:49_

People choose and judge tools based on practicality and applicability, rather than opinions. These opinions have no meaning, even if you vote, a very few people would refuse this feature

---

_Comment by @bbkane on 2024-11-14 15:24_

> Rust's ecosystem is built around a single, standardized toolchain,but Python's ecosystem is intentionally diverse, with different tools serving different needs, so forcing Cargo-like standardization onto Python goes against its philosophy of "we're all consenting adults here"

I'm excited about `uv` BECAUSE its one tool to "rule them all" for common Python build needs. I'm so tired of picking five or more tools to lint/format/build/package/etc a new project, and learning how to make them play nicely with each other, all the while knowing that these tools will likely evolve independently and I'll need to re-figure this when I update them. In addition, everyone else will choose a DIFFERENT five tools and if I want to contribute to their project I'll also learn how they do it. Please `uv`, save me from this!

I think running tasks is a common enough need that it would fit well into `uv` and be useful for most projects. Let me just learn `uv` way to do it. Of course, if a project needs a more specialized way to run tasks, they can use something else, but solve the common case of running command aliases...

---

_Comment by @stan-dot on 2024-11-14 17:06_

there are `one tools to rule them all` broader than just for Python is the Task runner mentioned above by @sisp 

> A popular and powerful cross-platform task runner is [Task](https://taskfile.dev/), and it is also available as a [Python package](https://pypi.org/project/go-task-bin/) (merely shipping the compiled Go binary) to ease its installation in Python projects.

also solves the problem of combinatorial explosion when N tools are used, but even more powerfully, as beyond just Python ecosystem

---

_Comment by @harkabeeparolus on 2024-11-14 20:34_

### To Be, Or Not To Be

First, the uv team at Astral have already decided that they are going to add some kind of task runner:

@charliermarsh commented [on Aug 9](https://github.com/astral-sh/uv/issues/5903#issuecomment-2277047047)

> Yeah we plan to support something like this! We haven't spent time on the design yet.

...So I do not think it's very helpful for anyone if we discuss our opinions for or against endlessly. I think the more constructive thing to do would be to help with the design.

### Target Audience

There is indeed a diverse ecosystem of tools for Python, since it's an old language that predates modern workflow tools for other languages, such as cargo, npm, dotnet, et al.

All of this will continue to exist whether uv has a basic task runner or not. Senior Python developers can continue to deploy fancy customized toolchains for their teams or CI/CD.

However, for new Pythonistas, either students, or developers coming from newer language ecosystems such as .NET, Rust, or Javascript, it would be very helpful to have the mythical "Cargo for Python" or "One tool to rule them all". Read e.g. [Rye's Philosophy and Vision](https://rye.astral.sh/philosophy/) for building "the one tool" that the majority of Pythonistas would use most of the time because it's the easy, obvious default. It won't solve every problem and corner case, perhaps, but it will cover most needs for mostly everyone most of the time.

### The Zen Of Python

> There should be one-- and preferably only one --obvious way to do it.

Building "the one tool" or "Cargo for Python" would be the _obvious_ way to develop in Python. Of course there will be a diverse ecosystem of specialized tools for every situation... But the should be _one obvious way_ to start, lint, format, test, build, and publish a normal open source Python project.

I think uv should become "the one tool", and I think that requires the same basic task running capabilities that all the other modern Python workflow tools have. 

---

_Comment by @zanieb on 2024-11-14 22:56_

The open question for me is whether or not we should be building a general purpose task runner, i.e., designing some sort of DSL, or investing specifically in `uv lint`, `uv format`, and `uv test`. I see a general purpose task runner as sort of antithetical towards creating a consistent experience for these core workflows.

---

_Comment by @zanieb on 2024-11-14 23:04_

(While I think @harkabeeparolus has a great point that moving the design forward is the best way to help move this issue forward, I do think discussion on the goals of and motivation for a task runner are reasonable as they will help inform the design)

---

_Comment by @zgana on 2024-11-15 00:05_

I want to build on this point from @chrisrodrigue [earlier in the thread](https://github.com/astral-sh/uv/issues/5903#issuecomment-2307665065).  With Hatch, we can tie scripts to specific sets of dependencies, installed into separate environments.  This can be much more general than `lint`, `format`, `test`.  Two examples from my team's project template:
- A `docs` environment with `sphinx` plus any extensions, and with helper scripts to build and preview the docs.
- An `interactive` environment with an `ipykernel` dependency, with helper scripts to install/uninstall a kernel.

I'm not at all opposed to expressing these sorts of things with plain old shell scripts.  What we're still missing in uv however (unless I am just out of date here) is a way to tie scripts to a specific dependency group or union of dependency groups.

---

_Comment by @appenz on 2024-11-15 04:01_

One more vote that this should be implemented. 

I think there would be a lot of value for being able to define a default target that allows me to start a package without knowing anything about it. Most Node packages can be started with "npm run" and I do not have to know the name of the package or the main javascript file. With package managers likely being the default way how novice users will run python packages in the future, I think ease-of-use matters.

---

_Comment by @inoa-jboliveira on 2024-11-15 20:10_

Hi everyone, this discussion is veering off course. If you’re not interested in the task runner, please consider downvoting the issue and moving on. I’ve explored the workarounds, but unfortunately, each one falls short in some way. Let’s avoid introducing philosophical debates about open source maintenance and stay focused on resolving the issue at hand. Thanks!

---

_Comment by @dbohdan on 2024-11-20 07:07_

I have suggestions conditional on uv implementing a task runner.

## Poe the Poet sequences

I use Poe the Poet as my task runner with both Poetry and uv. Something I really like about Poe is how it handles [sequence tasks](https://poethepoet.natn.io/tasks/task_types/sequence.html). I haven't seen it in other Python task runners. You can have a failure in a sequence abort the sequence immediately, have no effect, or only change the exit code of `poe` later. This is very useful, e.g., for independent checks that can fail.

Here is how I use it in my website project. (The Lua is for Pandoc.)

```toml
[tool.poe.tasks.check]
sequence = ["format", "lint", "type", "format-lua", "lint-lua"]
help = "Format and run all static checks"
ignore_fail = "return_non_zero"
```

## Starting with Poe

If uv implements a task runner, I would like it to have a similar feature to Poe's sequences. More broadly, my impression is that Poe has well-designed features absent in other Python task runners. Poe also seems to be the go-to task runner for Poetry users. Because of this, I think reimplementing Poe the Poet would be a good starting point for uv. The existing design of Poe could be refined as needed in the process, like how it happened with the Ruff formatter.

---

_Comment by @lucabello on 2024-12-10 17:14_

I have my two cents on the task runner implementation.

I use [`casey/just`](https://github.com/casey/just) in my personal projects; it's written in Rust, and I think it does its job as a task runner pretty well. 

IMHO, the syntax is close enough to Makefiles that it's easy to write. It has a lot of nice features, like easily displaying the available commands and being able to write them in any language (by adding a shebang to a recipe)

Would it be a crazy idea to have `uv` use `just` behind the scenes to run tasks? It seems like a very good opportunity to not reinvent the wheel, since I think `just` might already do all you want to implement.

---

_Comment by @DimitarVanguelov on 2024-12-10 20:59_

I like `just`, very clean and easy to use. One might ask, why not just use `just`, but I suppose the benefit here would be specifying the task commands in the pyproject.toml file and having a unified interface in a project where all commands begin with `uv...`?

---

_Comment by @lucabello on 2024-12-11 16:39_

Correct! To give a bit more substance to what I was saying, here's a more concrete example of what I'm imagining.

If we could say "you need to write a `justfile` in your project", things would be extremely easy to explain (and probably not too complex to implement either).

Assuming that having a `justfile` is too big of a request, and that we want to keep everything in `pyproject.toml`, here's an example of what that might look like. 

Don't mind the name/format of the fields, it's mostly an example to provide a better context to my words :)

### The setup

```toml
# pyproject.toml
[uv.tasks]
# Simple recipes could go directly here
lint = "ruff check"
test = "pytest -v"

# More complex recipe with dependencies
[uv.tasks.my-recipe] 
recipe = """
	echo This is a long recipe.
	echo I can do so many things here.
	echo Goodbye $name!
"""
# Run, in order: `lint`, `myrecipe`, `test`
dependencies = ["lint", "&& fmt"]
arguments = ["name"]
options = ["confirm"]
docs = "This is a very fun recipe."

# Tasks written in any language
[uv.tasks.python-recipe]
recipe = """
#!/usr/bin/env python3
print("Hello from Python!")
"""
dependencies = ["lint"]
```

### Internals

In this example, `uv` would essentially assemble the tasks as individual `just` recipes, building an internal representation roughly equivalent to the following justfile:

```make
set export  # could be a default option

lint:
	#!/usr/bin/env -S uv run   # <-- This would be the default shebang
	ruff check

test:
	#!/usr/bin/env -S uv run
	pytest -v

[doc("This is a very fun recipe")]
[confirm]
my-recipe name: lint && test
	echo This is a long recipe.
	echo I can do so many things here.
	echo Goodbye $name!

python-recipe: lint
	#!/usr/bin/env python3
	print("Hello from Python!")
```

### User Experience

I imagine a user could run a `uv task` command which provides a UX similar to `just`; something like:

```bash
∮ uv task lint
All checks passed!
∮ uv task my-recipe Bob
This is a long recipe.
I can do so many things here.
Goodybe Bob!
∮ uv task --list
Available recipes:
    lint
    my-recipe name # This is a very fun recipe
    python-recipe
    test
```

---

I think there's a good opportunity here to build something very powerful and reliable :)

---

_Comment by @astrojuanlu on 2024-12-11 16:49_

What about turning it the other way around? Allowing projects to add a `uv` CLI subcommand?

Similar to how `rustfmt` is distributed separately, and then allows users to do `cargo fmt`

> ## Custom subcommands
> 
> Cargo is designed to be extensible with new subcommands without having to modify
> Cargo itself. This is achieved by translating a cargo invocation of the form
> cargo `(?<command>[^ ]+)` into an invocation of an external tool
> `cargo-${command}`. The external tool must be present in one of the user's
> `$PATH` directories.

https://doc.rust-lang.org/cargo/reference/external-tools.html#custom-subcommands

Then `uv` just needs to add this automatic fallback mechanism. And folks can define their `uv-test`, `uv-fmt`, `uv-lint` CLI endpoints.

---

_Comment by @lordmauve on 2024-12-12 10:36_

If `just` is fine for you then you can disregard this issue.

I toyed with `just` a bit but it doesn't make it easy to support parallel builds/mypy/tests under multiple Python interpreter versions in the way that a Python-specific tool could.

Their docs say ["use GNU parallel"](https://just.systems/man/en/running-tasks-in-parallel.html) but that is not a cross-platform solution.

---

_Comment by @casey on 2024-12-12 19:30_

Heya, author of `just` here. Random thoughts:

- Just is written in Rust and is available as a library. However, the library interface is extremely limited, basically just supporting embedding `just` within another binary, and calling it with some arguments.

- It would probably be hard to do a deep integration where a justfile/recipe is built from a `.toml` file, since `just` makes a bunch of assumptions that it's parsing everything from a `justfile`. For example, even after parsing, there are still a bunch of source tokens with line numbers and offsets in structs, to allow for error reporting which points back to the original justfile, and it would probably be hard to generate these tokens from a toml file. You could generate a `justfile` from the contents of the `.toml` file, and then run `just` on that generated `justfile`, but the error messages would be confusing, since it would reference a file that the user hadn't written.

- There isn't native support for running recipes in parallel. There was (or is?) a PR open adding it, and it wasn't too bad, but it stalled before it got merged.

- Fallback support is really interesting, the user could type `uv-just` and then if `just` noticed it was invoked as `uv-just`, it could possibly do some `uv`-specific things. (Although actually, that might be a bad idea, since if users wrote `justfile`s that depended on these things, they then couldn't run with vanilla `just`, which would be confusing.)

- One thing about `just` that is probably nice as a dependency is that it has extremely strong backwards compatibility guarantees. 1.0 was released in 2022 after about 8 years of development, and even then, the backwards incompatible changes made before 1.0 were pretty minimal. I've publicly committed (perhaps unwisely!) to never making a `just` 2.0, and any backwards incompatible changes will be released on an opt-in basis, so they won't break anyone's justfiles.

- It turns out that there are basically an infinite number of features that people want from a command runner, so it might be good to consider up front the scope of the `uv` task runner. It seems really obvious that super simple python snippets are very useful, but adding all the stuff that people want might make the scope pretty huge.

- Running commands by passing them to `sh -c` would have some cross-platform considerations, since `sh` differs across unices, and isn't present by default on Windows.

One avenue worth considering is writing a python library which allows doing command runner type things very easily, shipping that with uv, and then letting users write a `tasks.py` or `commands.py` file that uses the library. Something like:

```python
import command from uv

@command
def foo():
  # do some useful stuff

@command(confirm = "are you sure?")
def bar():
  # do some scary stuff

@command(dependencies = [foo])
def baz(some_argument):
  # do some stuff that depends on foo
```

And then you would do something like `uv task foo`  or `uv task baz ARG` to invoke the right command.

If there was also a library which allowed for very compact command invocations, so you could call external programs, this might be a very nice solution which kept everything pure python and cross platform. It would be more verbose than shell, but also not subject to a lot of the trickyness of shell.

---

_Comment by @stur86 on 2024-12-12 19:30_

> I like `just`, very clean and easy to use. One might ask, why not just use `just`, but I suppose the benefit here would be specifying the task commands in the pyproject.toml file and having a unified interface in a project where all commands begin with `uv...`?

Also that you'd keep only _one_ non-python dependency (`uv` itself), which makes portability or using pipelines much easier. Otherwise if you want to use tasks in your CI workflows you need all of them to also install `just` or whatever before doing anything. 

---

_Comment by @jmcvetta on 2024-12-12 21:37_

Since they are both Rust, I wonder if it would be possible for `uv` to use `just` as a library. So it would be compiled in, only a single executable to distribute.

---

_Comment by @mattmess1221 on 2024-12-13 14:59_

Using a `tasks.py` to me is really no better than using a dev version of `[project.scripts]` to generate a script for a python function.

I haven't done extensive research on how others would use it, but for me, most scripts I'd define only involve running single or multiple executables with optional arguments (maybe variables), either in series or parallel. Just is a bit overkill for that, but it's useable as a stopgap until uv implements this.

One of my projects defines these simple scripts with pdm.

```toml
[tool.pdm.scripts]
dev = "fastapi dev"
test.composite = [
  "coverage run -m pytest {args}",
  "coverage report",
  "coverage xml",
]
check.composite = [
  "mypy src",
  "ruff check src",
]
lint.composite = [
  "ruff format src",
  "ruff check src --fix",
]

# lifetime event, keeps requirements.txt up to date on install
post_install = "{pdm} export --prod -o requirements.txt --self --without-hashes"
```

---

_Comment by @casey on 2024-12-13 20:16_

I opened a draft PR against a fork of `uv` to show what integration `just` would look like:

https://github.com/casey/uv/pull/1

It includes `just` as a dependency, so it's all still a single pure-Rust binary.

It adds a `uv just` subcommand, which captures all following arguments and forwards them to `just::run`, which is the single entry-point into the `just` library crate.

You can do `uv just COMMAND` to run a recipe, `uv just --list` to list recipes, etc. (`just` is a spiritual descendant of `make`, so commands are called "recipes").

I have no idea if this is actually a good idea, and there are lots of tricky considerations!

- I think using the name `just` for the subcommand is *probably* a good idea. It allows users to find documentation related to `just` syntax, etc.

- Having identical behavior with vanilla `just` is also probably a good idea. It would be confusing if `uv just` worked with a given justfile, but `just` didn't, because of something additional that `uv just` was doing that the `justfile` author relied on, or vice versa.
 
- `just` is basically just me, a couple more regular contributors, as well as a bunch of sporadic contributions. As such, it often moves slowly, especially for large features. Good examples are running recipes in parallel (a very desirable feature, had one PR which stalled), `{{…}}` interpolation in strings and backticks, and making modules feature-complete (you can create submodules in `just`, but can't yet do things like call a recipe in a submodule from a parent module). If there are any must-have features from the perspective of `uv`, it would be a good idea to make sure those were added to `just` before any integration into `uv`, to avoid a situation where `just` is missing something critical.

- One very crufty part of `just` is shell completions. They exist, but they're hacks on top of the `clap` auto-generated clap completions which attempt to add completions of recipe names. They mostly work, but are also kind of awful. I'm sure trying to get them to work with `uv just` would be horrid.

- I'm not a `uv` user, and I have relatively limited bandwidth, so an integration would definitely need someone on the `uv` side who was motivated to do the integration, both on the `uv` side, and with any PRs on `just` to improve the integration.

- `just` ignores `CTRL-C` and `SIGHUP`, to avoid leaving orphaned child processes behind. This means signal handlers on Unix and something similar on Windows. `just` is free to do such monkeying around because it's a standalone binary, but if `uv` also messes with signal handlers, then there could be problems. This however could probably be solved easily.

- A potentially big issue is that `just` relies on a reasonable `sh` being installed, which isn't the case on Windows. This is tough to work around. There isn't really a good cross-platform Bourne shell in Rust.

I think the one major qualm I have is the worry that for some reason, `uv just` and vanilla `just` would start to diverge, which would be an unfortunate tower of babel type situation.

---

_Comment by @inoa-jboliveira on 2024-12-13 20:35_

Hi @casey Very interesting demo! I think it might be in practice very similar to something that already works such as "uvx just" but I believe your proposal is to demo the lib integration and this could be just part of any `uv run`.

> * A potentially big issue is that `just` relies on a reasonable `sh` being installed, which isn't the case on Windows. This is tough to work around. There isn't really a good cross-platform Bourne shell in Rust.

That for me is the reason I don't use just (I tried several task runners) and finally decided for "taskipy" for the time being until uv gives us something baked in. It allows portable commands by using things like "mslex" to construct the correct command for each platform.
It has its own defects that I won't go on, but I would hope for a runner that gives us that one command to rule them all.



---

_Comment by @casey on 2024-12-13 20:48_

@inoa-jboliveira Yah, the lack of cross-platform shell support is a bummer.

I would definitely consider the idea [here](https://github.com/astral-sh/uv/issues/5903#issuecomment-2539847428), some kind of `tasks.py` file, with a uv-provided helper library for providing a consistent interface and command running features.

```python
import task, sh from uv_tasks

@task
def foo():
  sh('echo hi!')
```

- I think one of the turn offs is that it would require using python, as opposed to less verbose shell syntax. However, shell is so full of footguns, limitations, and weird edge cases that the trade off might be worth it. If you're writing simple commands, the overhead isn't so big, and if you're writing complex commands, then the principled-ness of python might be worth it.

- `uv` already provides a cross-platform,  comprehensive Python environment, so leveraging that makes sense.

- `uv` users are likely to already be familiar with Python, so that's also a bonus.

- Command runners have a ton of features, in large part because they're not general purpose programming languages, so often each piece of functionality means a new feature has to be added to the command runner, as opposed to users being able to write their own functionality. Using Python avoids this, since it's a general purpose programming language. It would be easy for the `uv` devs to add new features to the hypothetical `uv_tasks` library, and for end users to work around any limitations or add functionality themselves, since they have full access to Python.

---

_Comment by @chrisrodrigue on 2024-12-14 16:45_

> I would definitely consider the idea [here](https://github.com/astral-sh/uv/issues/5903#issuecomment-2539847428), some kind of `tasks.py` file, with a uv-provided helper library for providing a consistent interface and command running features.

+1 to this! I think the main advantage of Python is that it's a cross-platform scripting language. Built in packages like `subprocess` deal with the nitty-gritty platform specifics so that we don't need to worry about it. The shell is the medium, but Python should be the fluid running through it.

Conventionally, scripts are placed in a `/scripts` directory in a repo, but a top-level `tasks.py` module would be a very visible and sensible location to standardize task definitions. I'd go as far as saying that this should be formalized into a PEP and added into the standard library.


---

_Comment by @Ravencentric on 2024-12-14 17:39_

This sounds very similar to [`nox`](https://github.com/wntrblm/nox).

---

_Comment by @lucabello on 2024-12-14 19:05_

Adding to this comment:

> [@inoa-jboliveira](https://github.com/inoa-jboliveira) Yah, the lack of cross-platform shell support is a bummer.
> 
> I would definitely consider the idea [here](https://github.com/astral-sh/uv/issues/5903#issuecomment-2539847428), some kind of `tasks.py` file, with a uv-provided helper library for providing a consistent interface and command running features.
> 
> ```python
> import task, sh from uv_tasks
> 
> @task
> def foo():
>   sh('echo hi!')
> ```

I'm not sure if a `sh from uv_task` wrapper is even needed. I know a lot of people prefer to run `os.subprocess` directly ([availability](https://docs.python.org/3/library/subprocess.html): not Android, not iOS, not WASI). IMHO, assuming the availability fits the requirements, the following is perfectly fine UX-wise:

```python
import task, os

@task
def foo():
  os.subprocess.run(["echo", "Hi!"])
```

If we could specify external dependencies for tasks in `pyproject.toml` (just normal "extra" dev dependencies), or even [in the `tasks.py` file itself](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies), the task runner would become extremely expressive.

As a basic example, for Unix shells there's a very neat package ([amoffat/sh](https://github.com/amoffat/sh)) which abstracts shell interactions, and it achieves something very similar to your comment.

The `tasks.py` in your example could become something like (assuming the dependency doesn't come from `pyproject.toml`):

```python
# /// script
# dependencies = [
#   "sh",
# ]
# ///
import task, sh

@task
def foo():
  # both commands resolve to "curl http://duckduckgo.com/ -o page.html --silent"
  sh.curl("http://duckduckgo.com/", o="page.html", silent=True)
  sh.curl("http://duckduckgo.com/", "-o", "page.html", "--silent")
```

Maybe this makes things more complicated than they need to be, but it allows for a generic enough and widely accepted way to run shell commands with Python (`os.subprocess`), while still allowing for "power-usage" with external libraries (`sh`, `requests`, etc.).


---

_Comment by @mattmess1221 on 2024-12-14 21:12_

I would rather not have another [bun shell](https://bun.sh/docs/runtime/shell). Writing a Python file to just call subprocess is not appealing to me. And wrapping that subprocess (or reimplementing) with a library/framework is not better. 

---

_Comment by @chrisrodrigue on 2024-12-15 12:38_

@mattmess1221, I respect your opinion but could you please give specific examples on why this is not better? The disadvantages/pitfalls of writing pure shell like `bash` are already well defined.

The shell is an unknown environment that is outside the scope of a Python project. Shell dependencies are baked into the system rather than defined at the project level. (Is `curl` available? If so, is it the Unix binary or the PowerShell cmdlet? What version is it? Am I on Windows or Unix? What options will work? What operators/symbols can I use in the shell? What happens if we try to run these commands/scripts/tasks on another platform?)

Writing pure shell logic is bad for many different reasons, and being that `uv` is a Rust tool that manages *Python* projects and dependencies, it makes sense to approach and discuss things with a Python-first (and Rust-first) mindset.

So far, defining tasks in either (1) `pyproject.toml` or (2) `tasks.py` seems to be leading the discussion.

---

_Comment by @chrisrodrigue on 2024-12-15 13:03_

I myself am leaning toward `tasks.py` rather than `pyproject.toml` because tasks are not configuration. A config file just doesn’t feel like the right place to write logic in Python, shell abstractions, or pure shell.

(I think trying to define tasks in configuration is where a lot of the noted problems start to arise.)

---

_Comment by @chrisrodrigue on 2024-12-15 13:59_

# Implementation thought experiment 

To build on the other awesome ideas in this thread… let’s say we want to make some tasks to check our code with ruff and run pytest to generate a coverage report. 

## Exploratory questions
- Should we specify the dependencies for tasks in `pyproject.toml` or in `tasks.py`? 
- If the former (in `pyproject.toml`), do we have the assurance that `tasks.py` will be run in the project virtual environment scope? 
- If the latter (in `tasks.py`), should we mix multiple tasks and dependencies in the same `tasks.py` module?
- Should we specify task dependencies with a decorator instead of inline script metadata (i.e. `@dependencies`, `@needs`)?
- Should we have the ability to create multiple task modules under `/tasks`, with the name of the module resolving to the task name (i.e. `check.py`, `build.py`, `test.py`, `release.py`)?
- Should the docstring for a task function be printed as a help message for a task? 

## Tasks as file/module

`tasks.py`
```py
from uv import run, task

@task(needs="pytest pytest-cov")
def cov():
  '''Run unit tests and generate a coverage report.'''
  run("pytest --cov-config=pyproject.toml --cov=src tests")

@task(needs="ruff")
def check():
  '''Check Python code for a plethora of problems.'''
  run("ruff check")
```

Notes: A dependency decorator might not be strictly necessary if dependencies are already specified in `pyproject.toml`, but such a decorator would permit lazy dependency resolution for tasks at the function scope rather than the module scope, boosting performance. It could also allow running a task module as a standalone script with uv, independent of the project context. I don’t think that inline script metadata gives you this granularity (not sure if you can you put one of these comments in the function scope), so if you had all tasks in the same module, you would need to grab *all* the dependencies even if you were only running a single task. Helper classes/functions could be included without issues as long as they are not marked with the `@task` decorator (which marks a task entrypoint and gets you a nice CLI).

## Task command-line invocation

```bash
$ uv check --help
usage: uv run tasks.py check [-h]

Check Python code for a plethora of problems.

optional arguments:
  -h, --help            show this help message and exit

$ uv check
Resolved 7 packages in 0.41ms
Audited 5 packages in 0.01ms
All checks passed!
```
## Opinion

We already know that `uv run` *just works*, so providing a Python API for it makes sense to me. It gives developers the flexibility to define long tasks and handle complex logic with the expressiveness and simplicity of pure Python. The shell can still be used for what it is good for—as an execution medium—but we can leave all the heavy lifting and logic to the tools and languages that are within our scope and under our control: uv and Python.

No need to fight with bash/powershell/make. No need to learn or add new dependencies to your project or system. No foreign DSLs, templates, filetypes, or config. No need for you or others on your team to know how to use anything other than uv and Python.

---

_Comment by @ksamuel on 2024-12-15 19:27_

A few thoughts:

- I would avoid `tasks.py` or `task.py` as a name for the file. Queuing libraries tend to use those file names to attempt to load functions to distribute to workers automatically. 

- Providing a Python API is quite dangerous because now you are supporting an API that should work across Python implementations and versions. When UV is upgraded but then run on an old project, it will have to work. Also, people will be tempted to import stuff from their own project in there. This has all kind of implications. UV has been very robust so far because it was totally isolated from the user python setup. If you provide a python API, any user mistake in their Python setup will let them think it's a UV bug. **UV not interacting with the Python code directly has been both a technical and a psychological blessing for the tool adoption, and you could lose that.**

- If you want to go in that direction anyway, there are very different paths you can take. `doit` is by far the best task runner in Python (https://pydoit.org/), but it has a  LOT of features (full DAG, deps, targets, etc) and must provide a clunky API to do so declaratively while supporting good composition. Fabric allows remote execution and is very straightforward (https://www.fabfile.org/) but can break easily and is hard to compose. Typer is not a task runner, but shows the way for how to expose a good arg declaration API (https://typer.tiangolo.com/) which is a weak point of `just`.

Most python-based task runners are too verbose for basic usage, though. If I had to design a Python-based one, I would allow two syntaxes: a basic one for very quick and easy task declaration. And a more complete one for more complex tasks.

Simple syntax:

```python
# everything named TASK_ is a task
workers = 4
# strings are python 3.14 t-string so you get access to variables outside, but can substitute them on the fly
# by arguments passed by cli
TASK_RUNSERVER = t"python manage.py runserver {args}" # full argument proxing
TASK_TEST = t"python -m uvicorn wsgi:app -w {workers:int}" # argument --workers of type int (abusing formatting syntax for typing)
# You can use """ if you want multiple commands
```

Advanced syntax:

```python

from pathlib import Path

# no need for decorators, if it's prefixed with "task", it runs, like in doit
def task_build_frontend(build_dir: Path, clean: bool=True): # build the args parsing from annotations like typer
   # so this allows uv run build_frontend /tmp --no-clean
    """"Build the frontend bundle files

        Params:
            build_dir: a path to ....
            clean: delete....

    """
    # ^ this is the help of the command and document each params as well, something typer does with `Annotated` which is ugly

    # this should allow long running commands (like servers) to be sustained and allow for escape sequences (like colors) 
    # or stdin (like pdb)
    yield "npm run build" # this will run
    yield "cat manifest.json | ./generator.sh" # supports pipping out of the box
    yield t"cp output.js {build_dir}" # still a templated string to provide automatic escaping
    if clean:
        yield task_clean # if you yield a python `task_` func, it runs it, so you don't need to declare dependencies with a decorator

# you can provide optional wrapper objects for those strings for things like capturing output, accepting failure on tasks, 
# disabling color support, etc 
```

But as you can see, even making something simple but useful is a full project on itself.  

I recommend avoiding Python for that, and skipping advanced features for the task runner, while limiting the scope to basically aliasing, arg proxying, and pipping things, keeping tasks in pyproject.toml. A better packages.json if you wish. After all, a user can create a Python script in their code base and call it from uv, or install `doit` and use that. Risking breaking uv stellar robustness  reputation by exposing it to the python user code is not worth it IMO. People will never be satisfied with the scope of the task runner anyway.

Even `just` scope, a tool I use weekly, is probably too large. It supports a WHOLE DSL, which means a parser, syntax errors, error messages for those, and tooling in IDE. Plus it's easy to install, why integrate it? It's not like `uv pip` which solved the bootstrapping problem.

Also if you intend to provide test facilities like the hatch testing matrix, then it's important to iron out UV testing story first before diving into this feature since it will rely on it.

Honestly, keep it simple. Most users just need a way to run common commands in their env with a shorthand. If you need more, there are already plenty of tools to do it, and power users know how to use them. Besides, UV makes it trivial to install and use those.



---

_Comment by @zanieb on 2024-12-15 21:09_

👋 I don't have the time to engage deeply here yet, but I'm watching this discussion and the context is helpful for prioritization and will certainly save us time when we try to reach consensus on a design.

I'd like to point out a couple Python discussions relevant to this thread (which I'm also following):

- [Standardizing test dependency and command specification](https://discuss.python.org/t/pre-pep-standardizing-test-dependency-and-command-specification/71896)
- [Introduce/standardize project development scripts](https://discuss.python.org/t/idea-introduce-standardize-project-development-scripts/67194)

As with dependency groups, if these reach the point where they look like standardization is feasible, we will engage upstream and wait to implement something until the standard is finalized.

---

_Comment by @inoa-jboliveira on 2024-12-16 01:18_

> and wait to implement something until the standard is finalized.

@zanieb Are you sure this is the right move? isn't it time to take the lead on this? Waiting for the Python Council to finalize their standards could take forever. This issue is hot right now, and by acting, you could set the pace for others to follow. We're not worried about how pipx or Poetry will align their tasks down the line; we're focused on how we will use uv for this. If compatibility becomes an issue later, you can always add a layer for that. Right now, your community is eager to see progress. Let's make it happen

You pointed out "dependency groups" and for a month or two uv had its own --dev option pointing to a different thing in pyproject.toml. After a PEP passed you quickly adjusted but gave a valuable tool to the users in the meanwhile. That is some power that Python Council does not have.

---

_Comment by @zanieb on 2024-12-16 03:15_

> Are you sure this is the right move? isn't it time to take the lead on this? Waiting for the Python Council to finalize their standards could take forever. 

I don't think this quite matches what I said. If there's not a trend towards consensus on a standard, then we won't wait for it. I do think it'd be a loss for us to implement a task interface if something different is in the process of being standardized. Unlike defining a local development dependency group, there are many ways to run tasks today and the scope of the problem is much larger. We could easily design something that is fundamentally incompatible with another design. While I understand people are very interested in us building this feature immediately, it's going to be hard to get right and there's other, lower risk work that's still impactful.

If the lack of this feature is _blocking_ you from using uv or accomplishing your work please let us know — it's helpful to understand what problems need to be solved.

Similarly, if there's a way that implementing this directly in uv would drastically change the experience, that's helpful too — e.g., if we just re-implement Poe or Nox, I'm not sure what value we're adding. What strength of uv can be leveraged to build something better? 

---

_Comment by @ehossack on 2024-12-16 04:41_

@zanieb  If you visit the rye repository right now, an announcement indicates that `uv` is the successor project, more actively maintained and feature-complete. 

Yet notably there is a task runner and more holistic project build system feel to rye, I would say in large part due to its scripts. So in that sense I feel actively blocked in adopting `uv` because I cannot migrate from its predecessor. Maybe I'm misunderstanding the goals here, and they are actually different, but it feels like I should be able to finally have my "one python tool to accomplish what a project needs" here- and having to patch in another tool doesn't accomplish that. 

I can think of most major languages these days (JS/Java/etc.) and the default or defacto tool will have a scripts runner just to allow simple commands like tests or packaging. 

So while "blocked" is subjective, I think the stated goals/documentation of `uv` indicate that this is a feature that fits quite cohesively and is of pretty high importance. 
Anything implemented now can serve to validate what's possible, what the community likes, and completely break or get dropped in a 1.0 release. 

---

_Comment by @ksamuel on 2024-12-16 10:28_

I don't think it's fair to attempt to pressure the uv team to deliver what is essentially a "nice-to-have" feature despite the fact they are very transparently expressing why it's sane to wait until they have more information.

I understand that we all want it, but it's not just about implementing it, after that, they are stuck with supporting it for a long time.

---

_Comment by @inoa-jboliveira on 2024-12-16 14:56_

> If the lack of this feature is _blocking_ you from using uv or accomplishing your work please let us know

Like @ehossack above noted: "blocked" is subjective

We can make do with anything. It really just depends on Astral vision here. All the task runners have at least 1 important problem:
They are an extra dependency besides uv, so you need to make them part of the dev group and to have the env synced before they can be used. That forbids them to be used for anything that may need to happen in a non synced environment.

Astral wants everyone using uv run or uvx directly, but it is cumbersome to do `uv run sometaskrunner the-task` there is like 4 layers of understanding here: what is uv? why am i asking it to run something? why there is a taskrunner if uv is running stuff? What does that last bit mean? Is it part of uv, uv run, sometaskrunner or what? Oh it is defined on pyproject, but who is reading the pyproject? both uv and sometaskrunner?

In the end in our projects we decided to never tell our devs to do `uv anything`. We tell people to activate the env as usual and forget uv exists. Then we just do `sometaskrunner the-task` where the task may call uv internally.

> attempt to pressure the uv team to deliver what is essentially a "nice-to-have" feature 

This is really a foundational feature. But also there's no pressure. It is their job to define the priorities, and it is our job to say what we need. They proved over and over that they know what they are doing to the point I trust Astral more to create a decent task runner than the python council to spec it. A PEP is not an implementation.

---

_Comment by @chrisrodrigue on 2024-12-16 17:02_

>Providing a Python API is quite dangerous because now you are supporting an API that should work across Python implementations and versions.

I'm not so sure that providing a Python API for uv is "dangerous." If something breaks, just fix it.

---

_Comment by @chrisrodrigue on 2024-12-16 17:09_

It would be nice if we could do something like `uv <task name>` instead of `uv run tasks.py <task name>` (which is cumbersome, as @inoa-jboliveira points out). 

A workaround would be to automatically compile a rust shim for the uv binary that adds in a task interface, but that would be really annoying for a user to have to do.

---

_Comment by @stur86 on 2024-12-16 18:03_

> I don't think this quite matches what I said. If there's not a trend towards consensus on a standard, then we won't wait for it. I do think it'd be a loss for us to implement a task interface if something different is in the process of being standardized. Unlike defining a local development dependency group, there are many ways to run tasks today and the scope of the problem is much larger. We could easily design something that is fundamentally incompatible with another design. While I understand people are very interested in us building this feature immediately, it's going to be hard to get right and there's other, lower risk work that's still impactful.
> 
> If the lack of this feature is _blocking_ you from using uv or accomplishing your work please let us know — it's helpful to understand what problems need to be solved.
> 
> Similarly, if there's a way that implementing this directly in uv would drastically change the experience, that's helpful too — e.g., if we just re-implement Poe or Nox, I'm not sure what value we're adding. What strength of uv can be leveraged to build something better?

My two cents on this: what are the possible scenarios?

Obviously trying to reimplement CMake or some other uber-elaborate build system in uv is bonkers. It's way out of scope, basically another project of comparable size, and liable to conflicting with later standards and be hard to make compatible, as said here.

I think however the core of the suggestion here is just to be able to do something like `uv run <task name>` or `uv task <task_name>` and get a couple commands executed, like downloading some data with `wget`, or running tests with a bunch of default command line options. That's both very very simple and addresses a good 90% of all the use cases for this functionality. For anyone needing anything more complex, more specialised build systems will still be around; and in fact this could just be used as a shortcut to invoke those systems instead.

Supposing that is implemented with some custom `.tools.uv` section of the `pyproject.toml` file, then, what happens if later on PEP details a standard? Well, I expect the most likely outcome is that they will support some new section of `pyproject.toml` that does just about the same thing, allowing the user to define tasks via commands and possible additional options (e.g. mutual dependency, artifacts to use to check whether the task needs re-running, etc). But if the functionality implemented by uv is minimal, the odds are extremely high that the behaviour doesn't need to change, only expand, while the implementation shifts from looking at one section of the pyproject file to another, seamlessly (and possible with both supported for a little while for backwards compatibility). That doesn't seem like a terribly dangerous scenario. Now of course if uv developed its own complex and opinionated task runner the risk of conflict would be higher, but that's not the only way to go about this.

---

_Comment by @chrisrodrigue on 2024-12-17 04:34_

Opened #9955 (check 'em) for previewing a pythonic uv API and proof of concept task runner. 

It would be cool to investigate dynamically discovering and exposing user-specified tasks as top-level uv commands but I've run out of time tonight.

---

_Comment by @chrisrodrigue on 2024-12-18 15:33_

> Or maybe `[tool.uv.run]` to be consistent with the command `uv run`.

@nikhilweee I like this suggestion. It limits the scope to just the `uv run` command which is quite powerful already.

```toml
[tool.uv.run]
key = "value"
```

The user would execute `uv run <key>` or even `uv <key>`, which should be equivalent to `uv run <value>`. 

I just tested and found that `uv run <arbitrary uv command>` works. `uv run <arbitrary system binary>` also works, so even power users can rejoice.

---

_Comment by @smallstepman on 2024-12-18 17:22_

^ it can cause conflict issues:
```bash
uv add pytest
echo '[tool.uv.run]'  >> pyproject.toml
echo 'pytest = "pytest -vv"' >> pyproject.toml
uv run pytest # should it run pytest binary or pytest -vv?
```

this is adjacent to the issue, but I can feel the general desire to shortify things in this thread, perhaps what `cargo` does can be inspiration here:
```bash
cargo build
# is the same as 
cargo b

# same for run(r) check(c) test(t)
```

so maybe this could be direction potentially worth exploring
```bash
uv task pytest
# or shorter
uv t pytest
```

---

_Comment by @chrisrodrigue on 2024-12-18 17:34_

> ^ it can cause conflict issues:
> ```bash
> uv add pytest
> echo '[tool.uv.run]'  >> pyproject.toml
> echo 'pytest = "pytest -vv"' >> pyproject.toml
> uv run pytest # should it run pytest binary or pytest -vv?
> ```
> 
> this is adjacent to the issue, but I can feel the general desire to shortly things in this thread, perhaps what `cargo` does can be inspiration here:
> ```bash
> cargo build
> # is the same as 
> cargo b
> 
> # same for run(r) check(c) test(t)
> ```
> 
> so maybe this could be direction potentially worth exploring
> ```bash
> uv task pytest
> # or shorter
> uv t pytest
> ```

Named uv run commands in `pyproject.toml` should probably have the lowest execution order priority so that they don't shadow uv commands or executables in the virtual environment. It could resolve this type of conflict and the associated security concerns.

---

_Comment by @ofek on 2024-12-18 17:43_

Scripts/named commands should have the highest execution order priority as this allows maintainers to provide wrappers over command line tools, this is what Hatch does. To provide an escape you can prefix the first word/executable with a character to indicate pass-through.

---

_Comment by @chrisrodrigue on 2024-12-18 18:09_

> Scripts/named commands should have the highest execution order priority as this allows maintainers to provide wrappers over command line tools, this is what Hatch does. To provide an escape you can prefix the first word/executable with a character to indicate pass-through.

This would make wrappers convenient but it could also surprise users ([POLA](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) might apply here).

To reuse @smallstepman's example, if someone ran `uv run pytest` and instead they got `pytest -vv`, that could be a real head scratcher until they eventually discover that the pytest binary in the virtual environment was shadowed by a command defined in the `pyproject.toml`.

---

_Comment by @ofek on 2024-12-18 19:40_

In Hatch there is not much of a surprise because the [`run`](https://hatch.pypa.io/latest/cli/reference/#hatch-run) command has no flags.

---

_Comment by @lucabello on 2024-12-18 20:47_

I feel like if the scope is to simply execute `uv run` commands, the conflict you're talking about would be solved by having a separate `uv task` execute `uv run` (only for commands defined in `pyproject.toml`), and keep the current `uv run` behavior. If we aggregate some suggestions from above, we could have:

```toml
[tool.uv.tasks]
pytest = "pytest -vv"
```

Then you can execute:

```bash
uv run pytest  # runs `pytest`
uv task pytest  # runs `pytest -vv`
```

You get:
- least surprises (you're intentionally using a new command)
- pretty minimal scope (simply providing aliases for long `uv run` commands)

However, I think the scope might be too minimal (for me at least); I'd like to have an "alias" for my `uv lock` command, `uv export`, etc.

---

One idea could be to expand at something in the middle between "only `uv run`" and "arbitrary shell": running arbitrary `uv` commands. Assuming you can only run `uv` commands, `pyproject.toml` could look like this:

```toml
[tool.uv.tasks]
lock = "uv lock --upgrade --no-cache"
lint = """
  uv run --frozen --isolated --extra dev ruff check
  uv run --frozen --isolated --extra dev ruff format --check --diff
""" 
```

And you could run *commands* with:

```bash
uv task lock
uv task lint
```


---

_Comment by @chrisrodrigue on 2024-12-18 21:23_

@lucabello I think that's a happy medium!

The cool part about `uv run` is that it can be used to run anything (even other uv commands).

Assuming `[tool.uv.tasks]` values are passed to `uv run`:

```toml
[tool.uv.tasks]
# "uv task lint" equates to "uv run ruff check"
lint = "ruff check"
# "uv task lintt" equates to "uv run uv run ruff check"
lintt = "uv run ruff check"
# "uv task lock" equates to "uv run uv lock"
lock = "uv lock"
# "uv task clean" equates to "uv run git clean -fdx" 
clean = "git clean -fdx"
# too meta
uv = "uv run uv run uv run uv run uv"

---

_Comment by @ehossack on 2024-12-18 21:27_

Just for a context sake, looking back up this thread, are we reinventing the wheel? The way `rye` works currently, could we start from there? (Project from same umbrella)

---

_Comment by @DimitarVanguelov on 2024-12-18 23:58_

> ^ it can cause conflict issues:
> ```bash
> uv add pytest
> echo '[tool.uv.run]'  >> pyproject.toml
> echo 'pytest = "pytest -vv"' >> pyproject.toml
> uv run pytest # should it run pytest binary or pytest -vv?
> ```
> 
> this is adjacent to the issue, but I can feel the general desire to shortify things in this thread, perhaps what `cargo` does can be inspiration here:
> ```bash
> cargo build
> # is the same as 
> cargo b
> 
> # same for run(r) check(c) test(t)
> ```
> 
> so maybe this could be direction potentially worth exploring
> ```bash
> uv task pytest
> # or shorter
> uv t pytest
> ```

Might as well shorten it to `uvt` (or `uvk` or whatever, since `uvt` might confuse some as standing short for `uv tool ...`). This combined with @lucabello's comment just above makes the most sense (to me) in terms of having an ergonomic command line interface. 

---

_Comment by @casey on 2024-12-19 01:27_

@inoa-jboliveira 

> > * A potentially big issue is that `just` relies on a reasonable `sh` being installed, which isn't the case on Windows. This is tough to work around. There isn't really a good cross-platform Bourne shell in Rust.
> 
> That for me is the reason I don't use just (I tried several task runners) and finally decided for "taskipy" for the time being until uv gives us something baked in. It allows portable commands by using things like "mslex" to construct the correct command for each platform. It has its own defects that I won't go on, but I would hope for a runner that gives us that one command to rule them all.

I started fooling around with a branch of `just` which adds a `[no-shell]` attribute, which can be set on recipes to, instead of running lines with `sh`, run them with a very simple command parser. I'm not sure the juice is worth the squeeze, since there are very few environments where a `sh` truly isn't available. But I thought I would put it up in case the approach was interesting for `uv`. It's woefully incomplete, but it wouldn't be too hard to add things like string parsing, environment variable access, etc.

---

_Comment by @casey on 2024-12-19 02:04_

@inoa-jboliveira Whoops, forgot to link the PR: https://github.com/casey/just/pull/2531

---

_Comment by @smallstepman on 2024-12-19 03:25_

> Might as well shorten it to uvt (or uvk or whatever, since uvt might confuse some as standing short for uv tool ...). This combined with @lucabello's comment just above makes the most sense (to me) in terms of having an ergonomic command line interface.

im not a contributor to uv so I can't make an argument related to how good of an idea would be to deliver another binary on `uv` installation, but personally, I think I'd be ok with `alias ut='uv task'` (or `uvt`) in my .zshrc. I've been running `alias c=cargo` for years and I'm quite happy with it. I find the shorten CLI notation `uv t` (as is in `cargo b`) to be the sweet spot between simplicity and discoverability, at no additional cost of having to modify shell config files or shipping multiple binaries

---

_Comment by @DimitarVanguelov on 2024-12-19 19:24_

> > Might as well shorten it to uvt (or uvk or whatever, since uvt might confuse some as standing short for uv tool ...). This combined with @lucabello's comment just above makes the most sense (to me) in terms of having an ergonomic command line interface.
> 
> im not a contributor to uv so I can't make an argument related to how good of an idea would be to deliver another binary on `uv` installation, but personally, I think I'd be ok with `alias ut='uv task'` (or `uvt`) in my .zshrc. I've been running `alias c=cargo` for years and I'm quite happy with it. I find the shorten CLI notation `uv t` (as is in `cargo b`) to be the sweet spot between simplicity and discoverability, at no additional cost of having to modify shell config files or shipping multiple binaries

Yea that's fair. I'm not a contributor either, just a user hopefully providing some useful feedback. While I use aliases a decent amount, I think that relying on aliases for ergonomic workflows across a team is not ideal, you can't make everyone have the same aliases. 

And since I'm on the topic, just want to thank the Astral team and all contributors for the wonderful work they're doing. Aside from the solid tooling, it's great to see the core devs engage with the community so thoughtfully and carefully. 

---

_Comment by @ceejatec on 2024-12-21 00:46_

I just wanted to offer a wholehearted +1 to @ksamuel 's [comment](https://github.com/astral-sh/uv/issues/5903#issuecomment-2544013475) from a few days ago. At the risk of redundancy, I'm going to quote their conclusion, in case some people missed it after their initial discussion about offering a Python API:

> I recommend avoiding Python for _[tasks]_, and skipping advanced features for the task runner, while limiting the scope to basically aliasing, arg proxying, and pipping things, keeping tasks in pyproject.toml. _[...]_ Risking breaking uv stellar robustness reputation by exposing it to the python user code is not worth it IMO. People will never be satisfied with the scope of the task runner anyway. _[...]_
>Honestly, keep it simple. Most users just need a way to run common commands in their env with a shorthand. If you need more, there are already plenty of tools to do it, and power users know how to use them. Besides, UV makes it trivial to install and use those.

This really is excellent advice. It's clear from reading this thread that once you get beyond the "basics", peoples' ideas about what this system should offer begin to diverge wildly and grow many legs. There's no way to satisfy all those needs while still offering a nice, simple, basic task functionality that will satisfy 95% of use cases. And a cornucopia of tools exist outside of uv to handle the 5% in the wide variety of ways that they need to be accomplished.

My further $.02:

* I like the idea of an explicit subcommand for this. IMHO if uv has a weak point today, it's the heavy overloading of `uv run`. I'd lean towards `uv task` just because it's short and easy to type.
* If at all possible, it should NOT depend on any particular `sh` implementation. Windows matters, whether we like it or not, and even within the *ix world there's a pretty fair amount of variation.
* It seems like a lot of the "legs" I mentioned earlier, parallelism in particular, stem from utilizing task runners for testing purposes. As such it seems like those would be good candidates for excluding from `uv task` and deferring either to a future all-dancing `uv test` solution, or else to existing testing tools like `tox`.

---

_Comment by @lucabello on 2024-12-21 14:29_

@ceejatec I agree, and it seems like we are mostly converging on the example I posted [a few comments up](https://github.com/astral-sh/uv/issues/5903#issuecomment-2552238931): `uv task` providing convenient aliases for `uv run` commands.

As @chrisrodrigue pointed out, this is more powerful than I initially thought, because while you can do:

```toml
[tool.uv.tasks]
lint = "ruff check"
```

and run `uv task lint` (which would run `uv run ruff check`), you could also do:

```toml
[tool.uv.tasks]
lint = "uvx --from=rust-just just lint"
otherlint = "uvx tox -e lint"
```

where `uv task lint` would run `uv run uvx --from=rust-just just lint`, and you get the best of all worlds.

This effectively allows you to use `just`, `tox`, `pydoit`, or any other tool of choice, without requiring the user to install that external dependency, and that is a pretty big advantage IMHO. `uv` becomes the only thing you need to directly interact with, which is pretty neat.

---

While I'm also not a `uv` contributor and just circling ideas around, here's my understanding of this design:
- it's thightly scoped, and pretty simple to implement and maintain;
- it leverages `uv run` and removes extra dependencies from the users;
- it allows to wrap more complex runners (e.g. `just`, `tox`, `pydoit`, etc);
- it satisfies pretty much all "use cases" and "wants" I've seen in this GitHub issue.

Does anyone disagree / have any use case that wouldn't be supported by this design? Or do you think this is the way to go?

---

_Comment by @chrisrodrigue on 2024-12-21 18:01_

Great write-up/summary @lucabello. 

This concept of Tasks is simple, but powerful: aliases for `uv run` defined in `pyproject.toml`.

---

_Comment by @smallstepman on 2024-12-21 18:37_

im prepared 
```console
echo "alias ut='uv task'" >> ~/.zshrc
echo "alias utf='uv task format'" >> ~/.zshrc
echo "alias utt='uv task test'" >> ~/.zshrc
echo "alias utl='uv task lint'" >> ~/.zshrc
echo "alias uts='uv task serve'" >> ~/.zshrc
echo "alias uuu='utf && utt && utl`" >> ~/.zshrc
```
what am I missing?

---

_Comment by @bbkane on 2024-12-21 19:51_

> im prepared
> 
> echo "alias ut='uv task'" >> ~/.zshrc
> echo "alias utf='uv task format'" >> ~/.zshrc
> echo "alias utt='uv task test'" >> ~/.zshrc
> echo "alias utl='uv task lint'" >> ~/.zshrc
> echo "alias uts='uv task serve'" >> ~/.zshrc
> echo "alias uuu='utf && utt && utl`" >> ~/.zshrc
> 
> what am I missing?

Obviously you're missing [`uwu`](https://en.m.wikipedia.org/wiki/Uwu) 😁

Please feel free to remove as this is off topic, I just couldn't help myself

---

_Comment by @chrisrodrigue on 2024-12-21 20:07_

> im prepared 
> ```console
> echo "alias ut='uv task'" >> ~/.zshrc
> echo "alias utf='uv task format'" >> ~/.zshrc
> echo "alias utt='uv task test'" >> ~/.zshrc
> echo "alias utl='uv task lint'" >> ~/.zshrc
> echo "alias uts='uv task serve'" >> ~/.zshrc
> echo "alias uuu='utf && utt && utl`" >> ~/.zshrc
> ```
> what am I missing?

`build`, `doc`, and `publish` are some other development actions that might be valid for a project. It all depends on the specific needs of your project. For example, you might author a package in an internal company repo that never needs publishing, or on your personal computer that never needs documenting (if you have the memory of an elephant).

It's important to distinguish *user* commands (such as an `alias` statement in a `.rc` file) from *project* commands (such as a key under `[tool.uv.task]` in a `pyproject.toml`). The gist of "tasks" here is that they are project-specific, so that authors/maintainers/contributors/users/pipeline robots can all have an abstract and centralized way to run arbitrary project commands without having to hardcode them in a `.bashrc`, copy and paste them from the project documentation, or repeat them in pipeline job configs. 

Even shorter CLI notation (like `uv k` for `uv task`) seems like a different issue/feature request with much a broader scope that would have implications for all `uv` commands instead of just the proposed one. As you already demonstrated... user aliases seem like an appropriate stopgap (but platforms like Windows could be left wanting).

---

_Comment by @stur86 on 2024-12-21 20:13_

> [@ceejatec](https://github.com/ceejatec)
> Does anyone disagree / have any use case that wouldn't be supported by this design? Or do you think this is the way to go?

I'd say this pretty much does everything we might need (outside of very complex tasks that should probably be handled with a dedicated build system).

---

_Comment by @juangelos on 2024-12-26 05:59_

just doit

---

_Comment by @axel-kah on 2024-12-26 14:22_

> just doit

I think @juangelos meant to say: [just](https://github.com/casey/just?tab=readme-ov-file#readme) [doit](https://pydoit.org/).

Which is a clever word play, given the context of this discussion 😸
Regardless, I also think @lucabello distilled the essence of what most of the communities needs and in an elegant way, because it's pragmatic, intuitive and doesn't explode with complexity on the implementation side. Way to go💪 

---

_Comment by @rmorshea on 2025-01-02 00:48_

### A plugin system for task runners

I'm curious to know what the folks here think about the alternative described in https://github.com/astral-sh/uv/issues/10211. In short, the idea is that UV would supply a plugin system for task runners so that projects could use whichever task runner best suited their needs (e.g. [just](https://github.com/casey/just?tab=readme-ov-file#readme), [doit](https://pydoit.org/), [poethepoet](https://github.com/nat-n/poethepoet), [taskipy](https://github.com/taskipy/taskipy), or [invoke](https://www.pyinvoke.org/) just to name a few).

### A quick example

From the user's perspective, usage of this plugin system wouldn't look significantly different from what's been discussed already. So, for example, you'd still be able to run your tests with a command like:

```bash
uv task test
```

A configuration of the plugin system which used [Poe the Poet](https://github.com/nat-n/poethepoet) might then look something like:

```toml
[dependency-groups]
dev = ["poethepoet"]

[tool.uv.task]
command = "poe"

[tool.poe.tasks]
test = "pytest"
```

Given this, `uv task test` would essentially be an alias for:

```bash
uv run poe
```

There are probably some additional configuration options you'd need in order to deal with edge cases I haven't thought of, but hopefully this gets the basic idea across.

### Why prefer a plugin system

1. The UV devs wouldn't need to develop and maintain their own task runner system. One which would receive frequent pressure to grow in scope and complexity.
2. A UV-based task runner, whatever form it might take, is unlikely to be as capable as tools like [Just](https://just.systems/man/en/) which are solely dedicated to that purpose.
3. Because a UV-based task runner is likely to be relatively simple just to keep maintenance costs down, some projects will need to use other tools to manage their tasks anyway. As a workaround, a couple comments have suggested that you might alias usages of those tools within your UV tasks to keep UV as a central entry point for contributors:

   ```toml
   [tool.uv.tasks]
   lint = "uvx --from=rust-just just lint"
   fmt = "uvx --from=rust-just just fmt"
   test = "uvx --from=rust-just just test"
   ```

   In a case like this you end up having to sync two lists of tasks - one in UV and another in your tool of choice. This seems burdensome to maintain. By comparison, with a task runner plugin system you'd configure UV once with something like:

   ```toml
   [tool.uv.task]
   command = "uvx --from=rust-just just"
   ```

---

_Comment by @lucabello on 2025-01-02 10:41_

@rmorshea I'm not a big fan of the "plugin" approach, mainly because it's easily included in the "alias" approach.

About (1), I think there is a general agreement on this issue, but the "alias" approach is not implementing a full-fledged task runner either (i.e., there's no dependency groups) it's just `uv run` aliases so that everyone can run the same `test` or `lint` without installing extra packages. It stil allows power-users to use a separate task runner without needing a separate installation.

About (2) and (3), I think the "alias" approach doesn't require you to maintain two lists of tasks if you don't want to. To exclusively rely on `poe`, you don't need to replicate the task list at all; you could simply have:

```toml
[tool.uv.task]
poe = "poe"  # poe = "uvx poe" is equivalent

[tool.poe.tasks]
test = "pytest"
```

You can then run `uv task poe test` and you're essentially using `poe`. The only difference with the "plugin" approach (which is a bit more complex imho) is running `uv task poe test` instead of `uv task test`. I believe the "alias" approach is also more transparent (other than simpler to implement), as it's always executing `uv run` behind the scenes, and that makes it easier - in my mind at least - to reason about when using simple aliases.

All the task runners you listed are executable via `uv run`, so this applies to all of them. Here's another example for `just` (assuming your actual tasks live in a `justfile`):

```toml
[tool.uv.task]
just = "uvx --from=rust-just just"
```

You can run `uv task just` if you wish, so `uv task just test`, `uv task just lint`, etc.

---

Albeit the "alias" approach is very simple (which is what most people seem to want and need), it allows for all of these power-usage scenarios and essentially *includes* the "plugin" approach, which is why I personally like the "alias" approach better :)

---

_Comment by @tobiasdiez on 2025-01-02 13:30_

In the "alias-approach", how does one specify that a given command has certain external dependencies that should be installed before running it?

---

_Comment by @axel-kah on 2025-01-02 13:46_

> In the "alias-approach", how does one specify that a given command has certain external dependencies that should be installed before running it?

In the end you are "aliasing" uvx invocations and uvx features the `--with` option to [specify arbitrary dependencies](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies).

---

_Comment by @rmorshea on 2025-01-02 14:00_

Maybe I'd need to read the rest of this thread to understand why this isn't the case, but the "alias" approach doesn't seem to provide any benefit over using `uv run` - from a user's perspective you'd be executing `uv task just test` instead of `uv run just test`. If that's true, maybe projects that are interested in using a 3rd party task runner like Just should just keep using `uv run` or contend with maintaining two task lists.

---

_Comment by @tobiasdiez on 2025-01-02 14:47_

> > In the "alias-approach", how does one specify that a given command has certain external dependencies that should be installed before running it?
> 
> In the end you are "aliasing" uvx invocations and uvx features the `--with` option to [specify arbitrary dependencies](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies).

But that get's quite involved, doesn't it? You need to remember which dependencies you need for which task, kind of defeating the purpose of having predefined tasks in the first place.

Alternatively, you would need to define
```toml
[tool.uv.tasks]
lint = "uvx --with ruff ruff check"
test = "uvx --with pytest pytest"
```



---

_Comment by @ksamuel on 2025-01-02 16:20_

Not to mention arg proxying is necessary to allow those aliases to be flexible. 

---

_Comment by @lucabello on 2025-01-04 11:25_

@rmorshea IMHO the benefits of the "alias approach" over simply `uv run` become more apparent when the commands are not as simple as `pytest`.

What if you want to use a specific tool version / always pass some flags / etc? New users would need to read docs on what to `uv run` instead of simply executing `uv task`.

What if:
- you want the project to always be tested with `pytest --tb=short -vv` - new users won't know this
- you want a "test-network" command that only runs tests marked as "network" (`pytest -vv -m network`)
- you want to use tooling the user hasn't necessarily installed, like `doit` or `just`
- you want a specific version of a tool

Sure, you technically *could* document it somewhere and tell everyone: "to run the tests, execute `uv run --from=rust-just just test`" (or `pytest -vv --tb=auto -m production`, or whatever), but it's a lot easier to run `uv task test` and absorb the command complexity into the alias.

---

@tobiasdiez I think you'd use `--with` in the task definition, just as you wrote - note you don't need to specify it for the tool you're using, you can simply `uvx ruff check`. But if your task required an extra package to be present, that's yet another argument in favour of aliases - the user doesn't have to remember that, as the would just execute `uv task test` or whatever.

---

_Comment by @rmorshea on 2025-01-04 16:37_

Couldn't I just include `rust-just` in my `dev` dependency group so that I don't have to use a verbose command like that? That's why I had imagined the comparison would be between `uv task just test` and `uv run just test`.

---

_Comment by @rmorshea on 2025-01-06 17:38_

Now that I think about it, if UV shipped with a `uvr` alias for `uv run`. You could effectively get the exact same experience being described here if you installed `taskipy` in your `dev` dependency group:

```bash
uvr task test
```

```toml
[dependency-groups]
dev = ["taskipy"]

[tool.taskipy.tasks]
test = "pytest -vv"
```

Given that, [as mentioned above](https://github.com/astral-sh/uv/issues/5903#issuecomment-2567572773), `uv task` scripts are "not implementing a full-fledged task runner" and are "just `uv run` aliases", perhaps providing a `uvr` alias and some easily discoverable documentation describing this pattern would satisfy people?

---

_Comment by @ceejatec on 2025-01-06 21:27_

@rmorshea I might be an oddball, but I'd personally prefer not to add another `uvr` binary. IMHO one of the best things about `uv` is its single-binary nature. In most environments I don't even install `uvx`. In your example, that wouldn't matter; I could just run `uv run task test` (and in fact I think I'm going to start doing exactly that - thanks for the pointer!). However, if it became commonplace for people to define tasks or scripts that invoked `uvr` directly, then those would break when `uvr` wasn't available.

`uvx` is kind of understandable as it isolates a whole sub-subcommand that is pretty separable from the rest of the `uv` experience (although as I said, I still kinda don't like it). `uvr` feels like a bridge too far though. It's only saving you three keystrokes and not really simplifying anything - `uv run` is still just as overloaded as it always was.

---

_Comment by @rmorshea on 2025-01-06 22:49_

That makes sense. Users can always define `uvr` as an alias (I already do on my machine). The section documenting this sort of pattern could explicitly suggest doing so for those whoe find `uv run` to be too repetitive.

---

_Comment by @ksamuel on 2025-01-08 11:49_

Most windows user don't know how to define aliases, and you have to
define them in at least two shells. Sometimes it's not even possible
with some company policies.

 Le lundi 6 janvier 2025 à 23:50, Ryan Morshead
***@***.***> a écrit : 

> That makes sense. Users can always define uvr as an alias (I already
> do on my machine). The section documenting this sort of pattern
> could explicitly suggest doing so if you find uv run to be too
> repetitive.
> 
> —
> Reply to this email directly, view it on GitHub
> [https://github.com/astral-sh/uv/issues/5903#issuecomment-2574062506],
> or unsubscribe
> [https://github.com/notifications/unsubscribe-auth/AABWFPW7COUSO7KZXFMXD4T2JMCCLAVCNFSM6AAAAABMGHOKM6VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDKNZUGA3DENJQGY].
> You are receiving this because you were mentioned.Message ID:
>***@***.***>
 


---

_Comment by @rmorshea on 2025-01-08 19:02_

The alias really only saves you three characters which is really just a marginal benefit. Additionally, as mentioned above, UV could explain how to define an alias thus addressing the users who don't know how already.

---

_Comment by @lucabello on 2025-01-08 19:52_

Just so it's not lost in the noise, the comment detailing the approach almost everyone seems to be agreeing on is [here](https://github.com/astral-sh/uv/issues/5903#issuecomment-2558137515).

I'd love to hear some thoughts from the uv maintainers on this!

---

_Comment by @mmerickel on 2025-01-08 21:53_

I think the `[tool.uv.tasks]` and `uv task` strategy is great because it also doesn't clobber the default `$PATH` in the `uv run` subshell. So you can invoke `uv task foo` from `uv task bar` but you have to explicitly call `uv task foo` instead of just `foo`. I think this is a good thing. For example:

```toml
[tool.uv.tasks]
foo = "uv task lint"
lint = "ruff check"
```

Another issue I haven't seen brought up much would be how to invoke tasks defined by other projects in the workspace. You'd need something like `uv task --package subpackage lint` similar to `--package` on other commands. This difference in lookup is a great justification for not auto-injecting the tasks into the default `$PATH` used by `uv run`.

As far as what you can do / supported syntax in in the task definition - that is more controversial. Whether it's some variant of bash, parseable by shlex, or what, because you do need some mechanism to access env vars and chain multiple commands together and in a cross-platform way. This is super common in npm/yarn where you'll do things like:

```json
{
  "scripts": {
    "build:prod": "NODE_ENV=production yarn run build",
    "publish:prod": "...",
    "build-and-publish:prod": "yarn run build:prod && yarn run publish:prod"
  }
}
```

Some dialect has to be decided for `tool.uv.tasks` that can handle cross-platform scenarios.

Don't forget at the very least you will always have an escape-hatch that you can just shell to a cross-platform python script that you write that can do the right thing on each platform, despite everyone saying how much they hate writing python scripts for a python tool :-)

```toml
[tool.uv.tasks]
foo = "python3 scripts/foo.py"
```

Finally, it'd be nice if there were a way for a task to say it depends on a dependency-group so that if it is invoked then that dependency group is automatically installed into the env.

Imagine:

```toml
[[tool.uv.tasks]]
name = "foo"
dependency-groups = ["foo-deps"]
command = "python3 scripts/foo.py"

[[tool.uv.tasks]]
name = "..."
```

or a dict-form which allows you to keep the simple structure of just a string on some commands:

```toml
[tool.uv.tasks]
lint = "ruff check"
bar = {
  command = "tox -e build"
}

[tool.uv.tasks.foo]
dependency-groups = ["foo-deps"]
command = "python3 scripts/foo.py"
```

---

_Comment by @rmorshea on 2025-01-08 22:47_

@lucabello, if it's true that in [your proposal](https://github.com/astral-sh/uv/issues/5903#issuecomment-2558137515) `uv task` scripts are "not implementing a full-fledged task runner" and are "just `uv run` aliases" then it's not clear why it's necessary to implement this feature if you can get a nearly identical user experience with:

```toml
[dependency-groups]
dev = ["taskipy"]

[tool.taskipy.tasks]
test = "pytest -vv"
```

As far as I can tell, the only difference is that you have to type a few more characters:

```bash
uv run task test  # currently supported via taskipy
uv task test      # your proposal
```

It seems to me that, at least until the [ongoing standards discussions](https://github.com/astral-sh/uv/issues/5903#issuecomment-2544067283) have been resolved, the `taskipy` trick is probably acceptable for most use cases.

---

_Comment by @lucabello on 2025-01-09 10:20_

@rmorshea I think that given the scope we're setting, if `uv` wants to be the *one tool* to manage a Python project, having an internal solution is better than relying on a third-party package (that happens to be named `task`): what if it's discontinued, has a bug, etc? I'm not a `uv` maintainer, but I believe the scope of this issue has been made small enough that implementing it from scratch seems the way to go (to me at least).

The internals are also slightly different: `taskipy` is running custom shell commands, while `uv task` would be executing `uv run` behind the scenes; there has been concern in the previous comments about portability of using/writing shell commands.

I agree that the UX is very similar, and I think it's more of a maintenance/reliability/provenance type of problem for me; `taskipy` is good to use, surely, just like other task runners. But I'd argue `uv` has the chance to have their own *task runner* ([this](https://github.com/astral-sh/uv/issues/5903#issuecomment-2558137515) `uv task` thing) that also supports all other pip-installable task runners. The implementation is simple, small, and covers most uses cases - all if you use it in conjunction with another task runner (e.g. `uv task just`, and so on). `uv` isn't relying on an external task runner, but a user can *choose to* do so.

---

_Comment by @zanieb on 2025-01-09 17:20_

I'm working on scheduling design and implementation of this feature on the uv roadmap.

---

_Comment by @birdhackor on 2025-01-10 07:59_

Is there an opportunity to integrate a feature similar to PDM that reads env files based on different CMDs? (PDM documentation is attached below)

[https://pdm-project.org/en/latest/usage/scripts/#env_file](https://pdm-project.org/en/latest/usage/scripts/#env_file)
[https://pdm-project.org/en/latest/usage/scripts/#shared-options](https://pdm-project.org/en/latest/usage/scripts/#shared-options)

Currently, I use the environment variable `UV_ENV_FILE=.env` to automatically read the .env file. However, this feature has a drawback: if there is no .env file, it throws an error. I often encounter this issue when running `uv run ...` for the first time on a new project, and then I manually run `touch .env` to avoid the error.

Having behavior similar to PDM would be much more convenient.

---

_Comment by @lucabello on 2025-01-14 09:07_

Just one small addition, after giving the `uv task` alias approach some thought; I think the missing puzzle piece would be **the ability to specify a default command when you run `uv task`.**

Whatever the syntax in `pyproject.toml` is, being able to alias the base `uv task` command to something allows to fully integrate `uv` with an external task runner by having it list the available commands.

Some examples:

```bash
# Poe the Poet
$ uv task  # aliased to `poe`
[...]  # more output
Configured tasks:
  lint
  test

# Just
$ uv task  # aliased to `just` with a default list recipe, or to `just --list`
Available recipes:
    lint
    test

# pydoit
$ uv task  # aliased to `doit list`
lint   Lint things.
test   Test things.
```

---

A possible way to configure this in `pyproject.toml` would be the way `just` does it, which is using the `default` name to specify the default behavior:

```toml
[tool.uv.tasks]
default = "uvx --from=rust-just just"
lint = "uvx --from=rust-just just lint"
test = "uvx --from=rust-just test"
```

In this example, `uv task` and `uv task default` would be equivalent.

---

This little addition would make the `uv task` integration with an external task runner completely transparent. Obviously, an alternative solution would be to define a separate alias (e.g., `uv task just`) and use *that* to run the commands, but then we're adding one extra words to all commands (which is fine, but maybe not as clean as it could be).

We'd go from:
```bash
uv task just  # shows the list of commands
uv task just lint  # executes `uvx --from=rust-just just lint`
uv task just test  # executes `uvx --from=rust-just just test`
```
to:
```bash
uv task  # shows the list of commands
uv task lint  # executes `uvx --from=rust-just just lint`
uv task test  # executes `uvx --from=rust-just just test`
```

Sorry for the extra noise!

---

_Comment by @gesslerpd on 2025-01-20 22:49_

@zanieb Great that design for this is being scheduled!

Possibly some of this will come for free since `[tool.uv.*]` declarations are supported inside inline script metadata but it'd be great if script-based tasks were supported/considered as part of this.

For example, as of [0.5.17](https://github.com/astral-sh/uv/releases/tag/0.5.17) release this is one method of running `pytest` against a single script with all of it's dependencies (including `pytest`) defined inline.

`uv export --script test_example.py | uv run --with-requirements - pytest <options> test_example.py`


---

_Comment by @tringenbach on 2025-01-21 00:12_

Hi,

I am much more familiar with the NodeJs/Typescript/Webpack world than I am Python at this point in time.

The discussion I see in this Issue looks really encouraging. 

I got excited and typed up my own thoughts, but that probably wasn't needed. 

## What do Python devs expect?
First I wanted to ask, what do Python developers expect? That seems really important, and it's not a question I can answer. How do popular projects solve this problem today? How do devs using Python for their day job expect this to work, and what are they doing today?

## My own use cases

I saw someone mention shell aliases. Those don't really solve my use cases.

- I'm on a team, and I need a bunch other people to all run the same commands
  - Some on Mac, some on Windows, different shells, different IDEs
  - Everyone works on other stuff too
- I want the commands to be discoverable
- I want to reuse some of these commands with CI/CD


## NodeJs stuff

Back to my own thoughts from my NodeJs background.

In `package.json`, these kinds of tasks are defined using the "scripts" section. This automatically makes things confusing since Python has already standardized that word as meaning executables (which `package.json` defines under the `bin` key). Not much you can do about that, besides explain it in the documentation, but please do explain it. 

### "script" is overloaded
Script seems especially overloaded. Looks like at least hatch and PDM have something called scripts that are like these tasks. Python has "scripts" vs "modules" and so "uv run" talks about running "scripts", and the docs has a section on  standalone "scripts" vs "projects", and also a section on "`project.scripts`".  

PDM (which I'm not familiar with, just reading the docs) has a warning block in its documentation about this overloading of the word "scripts": https://pdm-project.org/latest/usage/scripts/#user-scripts It calls them "User Scripts". Looks like it also does the yarn shortcut thing.

### npm / yarn / gulp / the word "run"
`npm` and `yarn` both use the `run` command with these tasks. Yarn even lets you admit the word `run` if the task name doesn't conflict with a yarn subcommand.

For a while the task runner "gulp" was popular, and it did use the word "task", so the word "task" is decent choice. I do worry about having too many words that mean the same thing in plain english to someone new to Python. (task, tool, run, script).

In the nodejs / npm ecosystem, there's a popular package called [cross-env](https://www.npmjs.com/package/cross-env) to solve the problem of setting environment variables in a way that works cross platform. It would be great if a solution to that problem was built in to `uv`'s task runner.

It's not all that popular, but I've seen a couple of projects (such as [Lit](https://github.com/lit/lit)) using [wireit](https://www.npmjs.com/package/wireit), which I thought was neat in its minimalism, while still allowing tasks to depend on one another.

Another task running and workspace manager I've used in the nodejs world is [nx](https://nx.dev/reference/nx-commands#run-tasks). It's more about managing monorepos. Like `uv`, you end up prefixing everything with `nx`.

### IDEs

IDE's usually want to display a list of tasks. If they are being defined in a straight forward way in `pyproject.toml`, that seems easy enough.  But it might still be helpful to include a command to list tasks, or list tasks and their dependencies as a tree.

(I don't work on any IDE, so I don't know what their needs really are, but hopefully that use case is considered.)

### Other features

Some other things that might be nice:

- task dependencies
  - running one task first runs some others
- nested tasks
  -  colons are popular in tasks names, either for workspaces, or for steps or variants
- hidden tasks
  - useful for a shared dependency / an internal task you want to call from other tasks, but don't want the shell to autocomplete
- user defined task descriptions / help text 

---

_Comment by @brandon-avantus on 2025-01-27 23:44_

I started with *Rye*, and was really enjoying its built-in task runner.  But it was missing some features, so I started a Python version to experiment with. Then I made the switch to *uv* and was missing that built-in task runner, so I continued working on, and just published, my Python task runner, [pyproject-runner](/avantus-tech/pyproject-runner), and the related [pyproject-runner-shim](/avantus-tech/pyproject-runner/tree/main/shim).

*pyproject-runner* is similar to Rye's task runner, and should require few changes for those moving to uv from Rye. See the [FAQ](/avantus-tech/pyproject-runner?tab=readme-ov-file#frequently-asked-questions) answering "why not use an existing tool?"

I am open to using *pyproject-runner* as a playground to test features that might go into an official uv task runner.

---

_Comment by @niraj-khatiwada on 2025-01-29 14:33_

Man, I just switched from `pipenv` and was so excited that we finally have a worthy package manager in Python. Just stumbled that we cannot use custom scripts which I miss from `pipenv`. I'll stick with `make` for the time being but I really hope this gets implemented soon. Kudos!

---

_Comment by @haydenk on 2025-02-10 17:24_

I might be missing a key detail which is why it's not working but even being able to run local modules in the project would be nice.

```python 
# tests/runner.py

def hello():
    print('Hello')

def world():
    print('World')
```

Being able to run this as `uv run --module tests.runner:hello` and `uv run --module tests.runner:world` would be nice

---

_Comment by @mrh1997 on 2025-02-22 22:23_

As running tasks is an action was usually has to be done frequently it would be great if typing them can be done fast. Thus for example the widespread nodejs tool [pnpm allows shortcuting the run command](https://pnpm.io/cli/run) be omiting "run" if the task is not in conflict with any uv command.

Thus instead of writing
```
pnpm run task
```

I could write
```
pnpm task
```

In my opinion it would be really great if uv provides a similar DX.

---

_Comment by @d-k-bo on 2025-02-22 22:35_

@mrh1997 This could lead to ambiguous behaviour. If you want to type less, you could just `alias ur="uv run"`.

---

_Comment by @mrh1997 on 2025-02-22 22:47_

The downside of an alias is, that it is not available in every environment. I.e. if I support a collegue or created a new docker container I had to fall back to "uv run".

How about providing this shortcut by uv? Like you did with uvx.

Then one could run for example
```
uvr task
```

---

_Comment by @Xdynix on 2025-02-22 22:55_

@mrh1997 @d-k-bo The simplest way is to treat commands as tasks only if they're not conflicting with `uv`'s built-in commads. So `uv python` will never be expended to  `uv run python` or `uv task python`. This is also how `pdm` implement such feature.

> There is a builtin shortcut making all scripts available as root commands as long as the script does not conflict with any builtin or plugin-contributed command.

---

_Comment by @neutrinoceros on 2025-02-22 23:00_

This way it limits the design space for new uv commands is concerning. I don't see how it could be worth the cost.

---

_Comment by @Xdynix on 2025-02-22 23:05_

@neutrinoceros The shortcut is just an add-on. If a `uv` command with the same name as my task is added in the future, I can just rename my task, or use `uv task foobar`/`uv run foobar` to run the task. I don't see it limiting `uv`.

---

_Comment by @neutrinoceros on 2025-02-23 06:29_

Multiply that experience by the number of users and you're guaranteed to generate friction, confusion and complaints every time a new command is introduced. I simply don't think it's worth it.

---

_Comment by @Xdynix on 2025-02-23 06:38_

You can choose not to use the shortcut feature then, you don't lose anything. The point is to provide an option that trades the benefit of having to type less often, at the expense of the (very small) chance that your shortcut will break.

---

_Comment by @neutrinoceros on 2025-02-23 06:57_

Would you assume that *every* user opting in would necessarily understand the risk around upgrades ? What happens when someone integrates such a command into a CI job with an unpinned `uv` version ? Maybe this person understands the risk, and their reviewers don't.
Is it reasonable to assume that this situation will *never* create unnecessary friction ? How does it fit with astral's extreme caution with breaking updates (see uv 0.6.0's release notes) ?

---

_Comment by @Xdynix on 2025-02-23 07:07_

A reasonable concern. All I can say is that no matter how much design you do, you can never completely prevent users from making mistakes. That will be a tradeoff for Astral to make. I will only express my desire for this feature.

---

_Comment by @mrh1997 on 2025-02-23 10:04_

@neutrinoceros I got your arguement about the risk on breaking changes.

But what do you think about [my proposal](https://github.com/astral-sh/uv/issues/5903#issuecomment-2676435543) of a standardized shortcut like `uvx` for the run command (i.e. `uvr`)?

---

_Comment by @neutrinoceros on 2025-02-23 12:02_

I don't have a strong opinion there.
pros:
- I agree it feels on-brand 
- it doesn't create an enormous pile of technical debt/maintenance burden

cons:
- it creates a small opportunity for confusion (the same goes for `uvx`)
- to me `uv run` is already very minimal typing. Compare with `uvx` which is a shorthand for `uv tool run`, and I'm not sure it's worth it

I would probably use it if it existed, though I'm also happy to live without it.


---

_Comment by @Chewie on 2025-02-24 14:23_

Silly two cents: I tend to always have a dedicated language-agnostic task runner like [Task](https://taskfile.dev/) or [Just](https://github.com/casey/just) and wrap commands with that to also have non-python scripts under a common interface, so the verbosity of adding `run`/`task` to uv isn't a big problem for me.

---

_Comment by @mattmess1221 on 2025-02-24 14:49_

I've recently started using bash scripts with `uv run` in the shebang. It does the normal `uv run` things like automatically installing dependencies and activating the venv. I find it a suitable stopgap until uv gets its own shell agnostic task runner.

`scripts/test.sh`
```bash
#!/usr/bin/env -S uv run --group test bash
# shellcheck shell=bash
set -xeuo pipefail

coverage run -pm pytest
coverage combine
coverage report
```

`pyproject.toml`
```toml
[dependency-groups]
test = [
  "coverage",
  "pytest",
]
```


---

_Comment by @OCopping on 2025-02-27 09:10_

Not sure if this has been suggested before, but it would nice if it could also support chaining scripts such as:
```bash
uv run script1,script2
```

This is similar to how `tox` can run environments, which my company is seriously trying to move away from...
```bash
tox -e env1,env2,env3
```

This would save having to write a separate entry under a `[tool.uv.scripts]` for each individual combo of scripts you could ever want to run.

`hatch` is a nice alternative to `tox`, but one of the downsides of `hatch` envs/scripts in my opinion is that you either have to write a new entry under `[tool.hatch.envs.defaults.scrpts]` or chain whole run commands such as:
```bash
hatch run script1; hatch run script2
```

---

_Comment by @lucabello on 2025-02-27 10:49_

@OCopping interestingly enough, this use case would also be covered by a [`uv task` with a default command](https://github.com/astral-sh/uv/issues/5903#issuecomment-2589378072).

Assuming your `pyproject.toml` contains:
```toml
[tool.uv.tasks]
default = "uvx --from=rust-just just"
lint = "uvx --from=rust-just just lint"
test = "uvx --from=rust-just test"
```

you could then do:

```bash
uv task test lint # internally, this would run: uvx -from=rust-just just test lint
```

---

_Comment by @zgana on 2025-03-02 21:19_

@mattmess1221 wrote:
> I've recently started using bash scripts with `uv run` in the shebang. It does the normal `uv run` things like automatically installing dependencies and activating the venv. I find it a suitable stopgap until uv gets its own shell agnostic task runner.
> 
> `scripts/test.sh`
> 
> #!/usr/bin/env -S uv run --group test bash

This is great.  I prefer to persist per-group environments, similar to Hatch.  This can be achieved with a minor tweak:

```bash
#!/usr/bin/env -S UV_PROJECT_ENVIRONMENT=.venv-test uv run --group=test bash
```

---

_Comment by @humanzz on 2025-03-11 23:57_

Late to this thread, but I really like the idea of `tasks.py` from https://github.com/astral-sh/uv/issues/5903#issuecomment-2539847428 as it's similar to my current usage of https://github.com/pyinvoke/invoke which also uses a `tasks.py` where I run all that I need via
- `inv <task name>` if I wanna run a specific task
- `inv` to run the default task which I typically configure to run the several tasks e.g. installing dependencies (e.g. `uv pip install` as am still using the `pip` interface), linting checks, running the tests, etc.

It'd be great if `uv` can provide some similar functionality, and a command - as suggested in other comments e.g. `uvr` or even `uv run` to allow for similar functionality.

Example snippets from `tasks.py`

```python
...

@task
def lint(ctx):
    """
    Run linting checks using ruff and mypy
    """
    ctx.run("ruff format --check src test tasks.py", echo=True)
    ctx.run("ruff check src test tasks.py", echo=True, pty=True)
    ctx.run("mypy src test tasks.py", echo=True, pty=True)


@task(name="lint:fix")
def lint_fix(ctx):
    """
    Fix linting issues using ruff
    """
    ctx.run("ruff check --fix src test tasks.py", echo=True, pty=True)
    ctx.run("ruff format src test tasks.py", echo=True)

@task
def develop(ctx):
    ctx.run(f"uv pip install -r {LOCK_FILE} -e .", echo=True)

...

@task(default=True, pre=[develop, lint, test, build_docs, publish])
def release(ctx):
    """
    Release the package
    """
    pass
```

---

_Comment by @carlosferreyra on 2025-03-19 22:01_

I've been following this thread, but now I wanted to give a newbie perspective to the community.

I've been playing with npm and having a "scripts" section as an out-of-the-box feature, it's a massive gain in terms of maintainability and ease of development.

analog to this feature, I would agree that uv can add support to something similar to that.

I would follow this example
```toml
[tool.uv.run|aliases|commands]
default = "uvx --from=rust-just just"         # uvr
lint = "uv run scripts/lint.py"               # uvr lint
test = "echo 'hello from uv'"                 # uvr test
```

or maybe even not add a `uvr` but make `uvx` to search aliases first, before fetching from the online packages.

---

_Comment by @Quidge on 2025-03-19 22:08_

I've also been subscribed/following this feature for awhile. I ended up discovering how to install a project as a package, which gives you `[scripts]` support and that solves my primary use case.

But _WOW_ for someone new learning how to set that up it would be rough. There have been so many iterations on python packaging that you really need to understand a lot to get it working. It also seems uncommon to set up python application projects (not distributable libraries) up like this in the first place, so you're not going to see mention of it in various tutorials.

For some skill context, I'd consider myself an intermediate Python developer who has spent the last several years only in web dev projects. No distributable libraries. It's been bizarre to me that I got this far without knowing about packaging, but I wouldn't be surprised if the _large_ majority of Python devs are in the same boat.

The package.json `scripts` type approach is far easier for a newbie to understand IMO. Not sure if it's worth the complexity/scope, but I definitely understand the want.

(side note, I cannot emphasize enough how grateful I am to the work being done on `uv` -- bundling Python installation and dependency management into one tool is an absolute godsend for reducing surface area of issues across teammates)

---

_Comment by @AtticusZeller on 2025-03-20 13:53_

i find this feature request rather misguided. UV is an excellent environment and project management tool, but introducing such trivial functionality is unnecessary and counterproductive.

as people mentioned before, the request aims to address cross-platform issues, but handling these collaboration challenges is a fundamental skill any competent developer should possess. Features that don't bring substantial productivity improvements only burden the maintainers while encouraging poor development practices.

Python is renowned for its philosophy of explicitness—In keeping with Python's commitment to clarity, developers should understand the specific scripts they're working with instead of hiding them behind excessive layers of abstraction.

I'm commenting because I can't stand seeing discussions about features that won't bring actual benefits and only create more work for maintainers. Does this issue really affect your actual efficiency when using the uv tool？

btw, I love uvx ,it does be useful and clean,

uxr is the most stupid thought I've ever seen.sorry bro

uvx is enough man 

be appreciate on the current UV and focus on the actual and practical features please 

---

_Comment by @Lordfirespeed on 2025-03-20 15:36_

> i find this feature request rather misguided

@AtticusZeller you aren't a member of `astral-sh` nor have you contributed to `uv`.
Your comment, in summary: "I don't want this feature, so it would be a pointless burden on the maintainers."

Other users of `uv` have expressed they want this feature.
Other ecosystems have analogous features. 
Maintainers have expressed they are interested in supporting this feature. ([ref](https://github.com/astral-sh/uv/issues/5903#issuecomment-2277047047))

> [this feature] won't bring actual benefits

No benefits *for you* does not imply no benefits for anyone/everyone. That is a fallacy.

Your comment does not meaningfully contribute to the discussion at hand. Please refrain from commenting like this on GitHub.

---

_Comment by @dandavison on 2025-03-20 15:50_

I'd like to add shell completion to this design discussion. There was a quite a bit of discussion earlier in the thread about minimizing typing via aliases and the `uvr` short form, but good shell completion would have an even bigger impact on typing convenience and discoverability.

I'm not sure how it should work, but if it were possible to do something like `eval "$(uv generate-shell-completion --include-uv-run-commands)"` I think that could be very beneficial. Perhaps it would involve having a way for `uv run` command definitions to explicitly declare their own shell completions, or perhaps the contract would be that if a `uv run` command itself supports `generate-shell-completion` then those get incorporated into the tree of completions rooted at `uv run`?

In general I agree that having a task runner built into `uv ` (like `poe` etc, and like `make` when used as a simple task runner via `.PHONY`, but with cross-platform support) seems very attractive.

---

_Comment by @paulovcmedeiros on 2025-04-03 11:54_

In case other people may find it useful, this is how I do it with `uv` and `poe`:

```toml
[project]
name = "myproject"
requires-python = ">=3.12"
authors = [{name = "Author Name", email = "author@domain.com"}]
description = "Example package config file"

[project.scripts]
my_package_entrypoint = "myproject.__main__:main"
task-runner = "poethepoet:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[dependency-groups]
task_runner = ["poethepoet>=0.33.1"]
linting = ["black>=25.1.0", "isort>=6.0.1", "ruff>=0.11.2"]

[tool.uv]
package = true
default-groups = "all"

[tool.poe.tasks]
black = "black ."
isort = "isort ."
ruff = "ruff check -q myproject/"
linters = ["black", "isort", "ruff"]

[tool.ruff]
select = ["ALL"]
```

With this you can run `uv run task-runner linters` (or whatever name you choose instead of `task-runner`). I find this to be enough for what I've needed so far.

---

_Comment by @pot-code on 2025-04-06 06:22_

any updates?

---

_Comment by @kevlarr on 2025-04-11 13:05_

That's a great and well organized workaround @paulovcmedeiros. Especially if `uv` could be updated in the future to natively support defining tasks in the same way that `poe` does and  `tool.poe.tasks` could simply be relabeled as `tool.uv.tasks`. (And then invoked like `uv run task linters` instead of needing to define a separate `task-runner`, etc. script.)

---

_Comment by @amd-isaac on 2025-04-12 00:39_

I also appreciated @paulovcmedeiros' solution, but I hit a snag: it only works for packages, as they are the only types of project that support entrypoints. If you're wanting to use tasks with a non-packaged application, you can't redefine the name of the entrypoint from `poe` to `task-runner` or `task` (which is what I prefer) and need to instead use `uv run poe format` which is less "obvious" to me. 

My solution to this was to use the `taskfile.dev` project, which is a standalone go executable which can be installed as a Python package (`uv add --dev go-task-bin`). This conveniently exposes a `task` executable in the venv without requiring an entrypoint in the `pyproject.toml` file. You can then run `uv run task format` which to me looks better than `uv run poe format` when running in a non-packaged application. The only downside is that you can't define the tasks in the `pyproject.toml` file, but need to instead use a `taskfile.yml` file.

So the `pyproject.toml` file would look like:

```toml
[project]
name = "myproject"
description = "Clever description here"
requires-python = ">=3.10"

[dependency-groups]
dev = [
    "go-task-bin>=3.42.1",
    "ruff>=0.11.5",
]
```

And the `taskfile.yml` file would look like:

```yaml
version: '3'

tasks:
  format:
    desc: Format all files
    cmds:
      - ruff format
```

---

_Comment by @JBloss1517 on 2025-04-14 15:27_

I have been using `poe` with `uv` for a while now without much friction. Instead of using `taskrunner` like @paulovcmedeiros had, I just codify the commands that I would normally run via `uv` into my `[tool.poe.tasks]` section.

 For example:
```toml
[tool.poe.tasks]
dev = "uv run src/main.py"
build = "uv run pyinstaller --clean ./Server.spec"
build-debug = "uv run pyinstaller --clean ./Server-debug.spec"
```
and use it like `poe dev` or `poe build` no problem. 

As strictly a matter of preference for the api for `uv`, I would rather type `uv run dev` than `uv run task dev`. Where the `run` looks for available commands from your defined tasks in something like `[tool.uv.tasks]` and if it doesn't find it there, it looks into your scripts folder for other files for a the same name.

---

_Comment by @Speedlulu on 2025-04-14 15:44_

> and use it like `poe dev` or `poe build` no problem. 

@JBloss1517 you must have `poe` installed globally for that to work, right ?



---

_Comment by @Lordfirespeed on 2025-04-14 15:57_

> [@JBloss1517](https://github.com/JBloss1517) you must have `poe` installed globally for that to work, right ?

```bash
$ uv init
$ uv add --dev poethepoet
$ source .venv/bin/activate
$ poe --version
Poe the Poet - A task runner that works well with poetry.
version 0.33.1
```

No, once you have activated the virtual environment.

Also, the `uv run` prefix in task should be unnecessary: [poe docs](https://github.com/nat-n/poethepoet/blob/main/docs/guides/without_poetry.rst#usage-with-uv). This should be sufficient:

```toml
[tool.uv]  # ensure this table exists somewhere in your pyproject.toml. It can be empty.

[tool.poe.tasks]
dev = "src/main.py"
build = "pyinstaller --clean ./Server.spec"
build-debug = "pyinstaller --clean ./Server-debug.spec"
```

---

_Comment by @JBloss1517 on 2025-04-14 18:46_

@Lordfirespeed great to know! Thanks for the info.

---

_Comment by @AKuederle on 2025-04-16 09:06_

Just as a note, recent versions of poe have [first class support for uv](https://github.com/nat-n/poethepoet/pulls?q=is%3Apr+uv). This means poe will run `uv run` under the hood for any command. With that you don't need to activate the venv and you will get automatic `uv sync` when running the command 

---

_Comment by @phitoduck on 2025-04-17 20:24_

Re: design

Comparison of a few task runners from the perspective of people who know `bash` and use `uv`.

I left blank areas that I was unsure. Feel free to correct me.

| Feature                         | bash script ([example I like](https://github.com/adriancooney/Taskfile)) | [Justfile](https://github.com/casey/just) | Makefile           | [Taskfile](https://taskfile.dev/)       | [Poethepoet](https://poethepoet.natn.io/index.html)               |
| ------------------------------- | ------------ | -------- | ------------------ | -------------- | ------------------------ |
| Near-zero learning curve        | ✅            | ✅        | ❌                  | ❌              | ✅                        |
| Tab autocomplete commands       | ❌            | ✅        | ✅                  | ✅              | ✅                        |
| cwd does not matter             | ❌            | ✅        | ✅                  |                | ✅                        |
| True bash (functions, env vars) | ✅            | ✅        | ❌❌❌                | ❌ (`mvdan/sh`) | ✅                        |
| Bash syntax highlighting in editors       | ✅            | ❌        | ❌                  | ❌              | ❌                        |
| No extra install                | ✅            | ❌        | ✅                  | ❌              | ✅ (via `uv run poe ...`) |
| Can `apt-get install` if needed | n/a          | ✅        | already everywhere |                | n/a                      |
| Cmds can accept arguments           | ✅            | ✅        | ❌                  |                |  ✅                       |
| ^^^ *named* arguments           | ✅            |          | ❌                  |                |                          |
| Tab autocomplete arguments      | ❌            |          | ❌                  |                |                          |

Some notes

- "cwd does not matter" -- refers to how it doesn't matter which current working directory you're in, e.g. when you run `uv add ...`. `uv` is smart enough to figure out where the `pyproject.toml` file is and goes from there. Similarly, you can run `make ...` in any subdirectory of a dir containing `Makefile` and the targets will be discovered. Really handy.

---

_Comment by @my1e5 on 2025-04-17 21:49_

@phitoduck For "No extra install" - Just and Task are as easy to install as Poe the Poet as the binaries are avaliable on PyPi. So they can be used with `uv run` automatically.

* ### Task

    https://taskfile.dev/

    ```
    $ uv add --dev go-task-bin
    ```
    ```
    $ uv run task --init
    Taskfile created: Taskfile.yml
    ```
    ```
    $ uv run task
    Hello, World!
    ```

* ### Just

    https://github.com/casey/just

    ```
    $ uv add --dev just-bin
    ```
    Create a `justfile`
    ```
    hello:
      echo 'Hello from just!'
    ```
    ```
    $ uv run just hello
    echo 'Hello from just!'
    Hello from just!
    ```
    

---

_Comment by @phitoduck on 2025-04-18 00:09_

Whoooooa 🤯. I had no idea! Sadly, you do lose tab-completion if you invoke `just` through `uvx --from just-bin just` or `uv add just-bin; uv run just`. Similar to `poethepoet`. But still, being able pip-install those other ones is huge.

Bringing this back to uv's design--If `uv` could figure out tab completion for their solution when they build it, that'd be sick. It's one of the surprisingly strong reasons my last 2 teams have stuck with Just/Make over pure bash.

Edit: wow, I feel like I have to report back what I found since it relates to some of the previous comments about `poethepoet` (sorry if this is off topic). Look at this!

If you use `poethepoet` in a project to define your tasks in `pyproject.toml`, a newcomer could run:

```bash
# install `poe` globally
uv tool install poethepoet

# make sure ~/.local/bin is in your $PATH
uv tool update-shell
# Output for me: Executable directory /Users/ericriddoch/.local/bin is already in PATH (but if it weren't it'd be added, so this is idempotent!)

# enable tab completion of poe tasks in pyproject.toml; there's a similar cmd for those that don't use OhMyZSH--an onboarding script could just run all of them
# https://poethepoet.natn.io/installation.html#shell-completion
mkdir -p ~/.oh-my-zsh/completions
poe _zsh_completion > ~/.oh-my-zsh/completions/_poe

# reset the shell to reflect all these $PATH changes
eval $SHELL
```

Now typing `poe` and then pressing Tab will autocomplete the tasks. 🎉

As others have pointed out, `poe` automatically uses `uv run` to run tasks so you get all the benefits of `uv`. 🎉

This is exactly what I was looking for as a ML Platform engineer trying to make onboarding to our DS repos simple/consistent. As far as I'm concerned, I'd be thrilled if `uv` essentially gave you all this without requiring all the setup commands first.



---

_Comment by @davidpoblador on 2025-05-05 16:01_

While waiting for native task support in uv, I've been using a [justfile](https://github.com/alltuner/uv-version-bumper/blob/main/justfile) with `uvx --from just-bin just` to handle project automation like version bumping, tagging, and syncing dependencies. It works well as a temporary solution and keeps everything uv-centric. You can see it in action in my [uv-version-bumper project](https://github.com/alltuner/uv-version-bumper), which was built specifically with this limitation in mind. Looking forward to replacing the justfile with native uv tasks once they're ready!

---

_Comment by @lambda-science on 2025-05-22 05:27_

Is this feature on some kind of roadmap ? I like it a lot but it's open since August 2024 so I'm think it's not coming soon 

---

_Comment by @dedebenui on 2025-05-22 10:17_

I like the currently favored, simple approach of tasks being project-defined aliases for `uv run` because it nudges devs towards setting up a project with python-centric tooling that can be installed with `uv`, making it more portable. However, I've seen most examples illustrate it with something like

```toml
[tool.uv.tasks]
test = "pytest -vv" # uv run pytest -vv
```

but I would prefer if `uv` forced developers to write commands as a list of arguments to avoid common issues. I also think that a dictionary would provide a bit more control without broadening the scope:

```toml
[[tool.uv.tasks]]
name = "test"
cmd = ["pytest", "-vv"]

[[tool.uv.tasks]]
name = "clean"
cmd = [ # multiple commands as a list of commands
  ["rm", "-r", ".venv"],
  ["sync"],
]
env = {} # set environment variables ?
```

---

_Comment by @markusschlenker on 2025-07-04 14:30_

Hello guys,

since I did not see it mentioned anywhere: Sth. like this is also available in [pixi](https://pixi.sh/latest/workspace/advanced_tasks/) and I find it quite good.


---

_Comment by @pierrepo on 2025-07-07 10:42_

Maybe an integration of [`just`](https://github.com/casey/just) could be a solution. `just` is a command runner also developed in Rust and available through [PyPI](https://pypi.org/project/rust-just/)

---

_Comment by @JPHutchins on 2025-07-13 19:10_

Presently I am using hatch

```toml
[tool.hatch.envs.default.scripts]
format = "ruff format ."
lint = [
    "ruff check .",
    "mypy ."
]
test = "pytest"
all = [
    "format",
    "lint",
    "test"
]
```

The user interface is gross, but at least there's a cross platform SSOT for both the def and call:

```
uv run hatch run all
cmd [1] | ruff format .
3 files left unchanged
cmd [2] | ruff check .
All checks passed!
cmd [3] | mypy .
Success: no issues found in 3 source files
cmd [4] | pytest
======================== test session starts =========================
platform linux -- Python 3.12.3, pytest-8.4.0, pluggy-1.6.0
rootdir: /home/jp/repos/bix
configfile: pyproject.toml
plugins: mypy-testing-0.1.3, anyio-4.9.0
collected 64 items

tests/test_bix.py ........................................... [ 67%]
.....................                                          [100%]

========================= 64 passed in 3.61s =========================
```

My preferred interface would be:
```
uv run task format
```

Which is close to what would be accomplished with cross-platform/shell aliases as provided by [envr](https://github.com/JPhutchins/envr):

```ini
[ALIASES]
format=ruff format $ENVR_ROOT
```

Interface:
```
format
```

Comparisons like this are useful, but IMO Make and Bash shouldn't even be candidates given their antiquated and error prone syntax and lack of first class support in Windows. Shells can "run programs" - perhaps we do not need to reinvent the wheel.

> Re: design
> 
> Comparison of a few task runners from the perspective of people who know `bash` and use `uv`.
> 
> I left blank areas that I was unsure. Feel free to correct me.
> 
> Feature	bash script ([example I like](https://github.com/adriancooney/Taskfile))	[Justfile](https://github.com/casey/just)	Makefile	[Taskfile](https://taskfile.dev/)	[Poethepoet](https://poethepoet.natn.io/index.html)
> Near-zero learning curve	✅	✅	❌	❌	✅
> Tab autocomplete commands	❌	✅	✅	✅	✅
> cwd does not matter	❌	✅	✅		✅
> True bash (functions, env vars)	✅	✅	❌❌❌	❌ (`mvdan/sh`)	✅
> Bash syntax highlighting in editors	✅	❌	❌	❌	❌
> No extra install	✅	❌	✅	❌	✅ (via `uv run poe ...`)
> Can `apt-get install` if needed	n/a	✅	already everywhere		n/a
> Cmds can accept arguments	✅	✅	❌		✅
> ^^^ _named_ arguments	✅		❌		
> Tab autocomplete arguments	❌		❌		
> Some notes
> 
> * "cwd does not matter" -- refers to how it doesn't matter which current working directory you're in, e.g. when you run `uv add ...`. `uv` is smart enough to figure out where the `pyproject.toml` file is and goes from there. Similarly, you can run `make ...` in any subdirectory of a dir containing `Makefile` and the targets will be discovered. Really handy.



---

_Comment by @codethief on 2025-07-14 07:38_

As mentioned in https://github.com/astral-sh/uv/issues/12533#issuecomment-3048921249, there could be some benefit to integrating uv with [mise](https://mise.jdx.dev/) when it comes to tool installation, and the same probably applies to task running capabilities (which mise [already provides](https://mise.jdx.dev/tasks/), too).


---

_Comment by @wigging on 2025-07-21 17:28_

Just want to show my support for this. It would be a great feature for uv.

---

_Comment by @aleyan on 2025-07-26 16:14_

Hey all, I have [done an analysis](https://aleyan.com/blog/2025-task-runners-census/) of various task runners used by top 100,000 github repos. 

![Image](https://github.com/user-attachments/assets/e9edab66-f2ce-44d2-9b37-afd311316352)

As a package manager, *uv* already dominates dedicated task runners like *just* and *task*. Usage for *mise* and *pdm* was below the threshold of 100 repos to fit on the graph. Thus I don't think it makes sense to integrate with any existing task running standard because *uv* is more popular than any one of them.

My recommendation is for *uv* to come up with its own way of executing arbitrary shell tasks. This could potentially be standardized in a PEP down the road.

---

_Comment by @rectalogic on 2025-07-28 15:16_

In the Rust world, `cargo xtask` is a common pattern, defined here https://github.com/matklad/cargo-xtask
Basically you add a new package named `xtask` to your workspace (or as a directory if not using workspaces), then define a cargo alias that invokes that package. The `xtask` package takes a single subcommand name and arguments with a subcommand, all implemented in Rust and with any custom dependencies (since the `xtask` package has it's own `Cargo.toml`)

The alias is what makes it ergonomic since now users can just run `cargo xtask foo ...` etc.

So if uv supported aliases, then both the workspace or plain directory versions of xtask could work:
```toml
[tool.uv.aliases]
xtask = "uv run --package xtask" # workspace version
xtask = "uv run --directory xtask" # plain directory version
```

The xtask package would have it's own `pyproject.toml` so it can depend on it's own helper packages if needed.

With uv alias support, trivial commands could be directly implemented as aliases, and more complex tooling could be built using the xtask pattern.

---

_Comment by @hasansezertasan on 2025-07-30 12:14_

Even though I already use mise as my task runner and tool manager, I’m still super excited about this! 🚀 Always love seeing new tools and improvements in the ecosystem. 🙌

---

_Comment by @WilliamStam on 2025-08-04 08:20_

defs keen for this. in the interim im using taskipy 

```
[tool.taskipy.tasks]
test = "pytest -vv -s"
lint = "ruff check --fix src"
```

```
uv run task test
uvx task test
```

would be nice to do something like 

```
uv run test
uvx test
```

and it checking the pyproject.toml first, runs that if found. else runs the script from bin / venv wherever else. if the dev wants to be "stupid" by adding tasks that have the same names as real executables files (like cmd / python etc) then the clashing is on them. not the responsibility of the tool. 

---

_Comment by @AmodeusR on 2025-08-05 14:10_

So, is there any estimation to when this could come to uv?

---

_Comment by @toppk on 2025-08-05 20:26_

i'd like to ask for yarn/pdm run shorthand, so instead of running `uv run foo`, you can run just `uv foo` (where builtin uv commands always take precedence). 

---

_Comment by @thomasgl-orange on 2025-08-05 22:17_

I like having tools like `go-task` and `ruff` and others managed by `uv` as dev dependencies, but I don't like typing `uv run ...` to run these tools (mostly because I then miss `task` Bash completion).  So for what it's worth, in some projects I use a short shell wrapper:

```bash
#!/bin/bash
PROG_NAME=${0##*/}
if [[ ${PROG_NAME} = uvr ]] ; then
  exec uv run "${@}"
else
  exec uv run "${PROG_NAME}" "${@}"
fi
```

I save it as `bin/uvr`, alongside a few symlinks:
```
task -> uvr
ruff -> uvr
...
```

And I prepend this `bin` dir to my `$PATH`  when I'm in the project, using an `.envrc` file (loaded by [`direnv`](https://direnv.net/) and relying on `PATH_add` from its standard library, but of course there are other ways):
```
# add an `uv run` wrapper to the $PATH, convenient for completion on `task <TAB>` for instance
PATH_add bin
```

**Warning:** this `bin` dir should stay out of your sources tree (or go under `.git`) in case of a shared project, to avoid external "contributions" adding malicious commands to your `$PATH`. This example was in the context of a private personal project.

And that's it; in the context of such projects, I can simply type `ruff analyze graph` or `task whate<TAB>`, and the actual command which get executed will be `uv run ruff analyze graph` or `uv run task whatever`.

---

_Comment by @Master-Y0da on 2025-08-13 23:33_

@WilliamStam cool solution!! I didn't know about taskipy! super easy to use! thanks!

---

_Comment by @julian-hecker on 2025-08-27 15:10_

this is pretty exciting

---

_Comment by @scottschreckengaust on 2025-08-28 21:30_

Usage perspective: `uv task [-h] [--help] [--list] [name] [args]` "feels" right

---

_Comment by @hasansezertasan on 2025-08-29 11:59_

> Usage perspective: `uv task [-h] [--help] [--list] [name] [args]` "feels" right

And maybe a shorthand like `uvt`?



---

_Comment by @TrevinAvery on 2025-08-30 01:54_

One thing I have always wanted from these quick script features is an automatic check for matching files in a directory.

For example, most of my helper scripts are one liners that i can put in the pyproject.toml no problem. However, there are a few that I need multiple lines for, so I place them in a separate file, typically in a "scripts" directory at the project root. But I don't want to type the full path to run them because I want to access all my quick scripts with the same cli pattern so they are easier to remember. To address this, I add a quick script in the config file that just calls that one file.

But what if we could just add a directory to the config file for the tool to check for scripts? When running a quick script, it first checks the config file, then checks if a file with the same name exists in the scripts directory, and executes it if it matches. Then I don't need this extra boilerplate configuration.

---

_Comment by @leetrout on 2025-08-30 01:58_

@TrevinAvery that is exactly how Mise works if you were not aware. I find it quite helpful as well. 

---

_Comment by @lukasgraf on 2025-08-30 07:05_

@TrevinAvery that should already be covered by the [`projects.scripts` entrypoint](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#creating-executable-scripts), if I understand you use case correctly:

pyproject.toml:
```toml
[project.scripts]
say-hello = "yourpkg.cli:say_hello"
```

src/yourpkg/cli.py:
```python
def say_hello():
    print('Hello')
    # Set up argparse here etc...
```

**Usage:**
```shell
uv run say-hello
```

(Additionally, it will be available as `.venv/bin/say-hello` as well, and therefore available on your $PATH if you decide to activate the venv)


---

_Comment by @Lordfirespeed on 2025-08-30 07:58_

@lukasgraf please reread @TrevinAvery's post, they already know they can do as you described.

---

_Comment by @TrevinAvery on 2025-08-30 11:24_

@lukasgraf project.scripts is totally different.

1. It still requires the boilerplate of adding each script to the config. I’m suggesting we only add a directory and every file in there is available as a script automatically.
2.  It appears to only support Python scripts. I want something that shoppers sent file type. Specifically, bash files.



---

_Comment by @JimDabell on 2025-08-30 13:15_

@TrevinAvery This seems out of scope for uv. It’s not a tool to find arbitrary executables to run. It’s a tool to run Python in environments it manages.

---

_Comment by @stur86 on 2025-08-30 19:57_

> [@TrevinAvery](https://github.com/TrevinAvery) This seems out of scope for uv. It’s not a tool to find arbitrary executables to run. It’s a tool to run Python in environments it manages.

Best you could do that allows it and is sort of potentially useful in general is make a `[tools.uv]` parameter that allows you to set up environmental variables for all `uv run` and eventually `uv task` (or whatever) commands. That way you could put any local folder of binaries in the PATH and call it a day.

---

_Comment by @peacefulotter on 2025-09-04 14:10_

This feature would be a great addition to uv imo! 

---

_Comment by @TrevinAvery on 2025-09-04 15:24_

@stur86 If we modify the PATH, then the scripts will be accessible without the uv prefix, and potentially outside the project directory. The idea here is to scope custom scripts to the project for convenience and consistency.


---

_Comment by @AnthonyGress on 2025-09-04 17:31_

+1 I would also like this feature. My 2 cents is to just get a basic version of a this feature (simple task runner) out. Then iterate on it and add more advanced features after the fact. 

---

_Comment by @stur86 on 2025-09-05 11:57_

> [@stur86](https://github.com/stur86) If we modify the PATH, then the scripts will be accessible without the uv prefix, and potentially outside the project directory. The idea here is to scope custom scripts to the project for convenience and consistency.

You can just add them for the scope of running that specific shell instance in which the command is executed, which won't make them be persistent. That was my line of thinking.

---

_Comment by @adrianovieira on 2025-09-06 22:55_

Awesome!

I am coming from `pipenv`, and this is a feature I also missed.

<details open>
<summary>an excerpt from <code>Pipefile</code></summary>

```toml
[scripts]
dev = "fastapi dev main.py"

tests = """sh -c
        'pytest -p no:cacheprovider $1 $2 $3' $1
        """
```
That can be run like:

```bash
pipenv run dev
```

```shell
pipenv run tests
```

</details>

---

_Comment by @KotlinIsland on 2025-09-10 07:45_

uv should also provide git hook configuration for it's tasks, then we will finally be able to move past pre-commit

---

_Comment by @jacobtomlinson on 2025-09-23 13:59_

<del>For anyone looking for this I made a quick utility called [uv-job](https://github.com/jacobtomlinson/uv-job) for doing this with `uv`. </del>

_Update: Looks like `poe` and `taskipy` do exactly the same thing. Sorry for the noise!_

<details>
<summary>previous comment contents</summary>

For anyone looking for this I made a quick utility called [uv-job](https://github.com/jacobtomlinson/uv-job) for doing this with `uv`.

Add it as a dev dependency
```bash
uv add --dev uv-job
```
Define some jobs in your `pyproject.toml`.
```toml
# pyproject.toml
# ...
[tool.jobs]
test = "pytest -v"
```
Run your job with `uv run job <name>`
```console
$ uv run job test
============ test session starts ============
platform linux -- Python 3.13.0, pytest-8.4.2, pluggy-1.6.0 -- /home/jacob/Projects/jacobtomlinson/uv-job/.venv/bin/python
cachedir: .pytest_cache
rootdir: /home/jacob/Projects/jacobtomlinson/uv-job
configfile: pyproject.toml
...
```

</details>

---

_Comment by @wevertonms on 2025-09-23 14:16_

> ```
> # pyproject.toml
> # ...
> 
> [tool.jobs]
> test = "pytest -v"
> ``` 
> Run your job with `uv run job <name>`
> 
> `$ uv run job test`

@jacobtomlinson That's basiclly the same as [Poe the Poet](https://github.com/nat-n/poethepoet). Besides, poe can activate the venv automatically, so you could run just:

`$ poe test`

---

_Comment by @adrianovieira on 2025-09-23 14:20_

> For anyone looking for this I made a quick utility called [uv-job](https://github.com/jacobtomlinson/uv-job) for doing this with `uv`.
> Feedback welcome!

Congrats. Well done. Thx.

However, there is *[Taskipy](https://pypi.org/project/taskipy/)*, which already provides this resource.

For short:

```bash
uv add --dev taskipy 
```

```toml
[tool.taskipy.tasks]
tests = "pytest -v"
lint = "pylint tests taskipy"
```

```bash
# show all tasks
uv run task --list

# to run a task
uv run task tests
```


---

_Comment by @jacobtomlinson on 2025-09-23 14:24_

Typical, I spent ages searching for a tool to do this and couldn't find anything. Guess I shouldn't have skim read the OP comment 😓 

@wevertonms sweet, I was under the impression Poe was just for poetry. Glad it works with `uv` too.

@adrianovieira I hadn't heard of that one at all!

I'm definitely going to use one of those instead. I don't need any more projects to maintain 😆 

---

_Comment by @adrianovieira on 2025-09-23 15:17_

> Typical, I spent ages searching for a tool to do this and couldn't find anything. Guess I shouldn't have skim read the OP comment 😓
> 
> [@wevertonms](https://github.com/wevertonms) sweet, I was under the impression Poe was just for poetry. Glad it works with `uv` too.
> 
> [@adrianovieira](https://github.com/adrianovieira) I hadn't heard of that one at all!
> 
> I'm definitely going to use one of those instead. I don't need any more projects to maintain 😆

I happens. You're welcome.

The issue opening description https://github.com/astral-sh/uv/issues/5903#issue-2455504094 also provides information about those tools (rye, Poe, and Taskipy).


---

_Comment by @goof-bug on 2025-09-30 17:17_

I'd love to implement this! Reading through the thread it seems like we need to settle the table name for defining the tasks and the uv subcommand used to execute them. However, I feel like most of the implementation can be started on already and whichever table and command we end up choosing can quickly be changed to without too much effort. This would be my first time doing something for this project so if anyone could give me some pointers to start with that would be great, otherwise I'll just figure it out myself 😄. 

---

_Label `needs-design` added by @zanieb on 2025-09-30 17:44_

---

_Comment by @zanieb on 2025-09-30 17:45_

@goof-bug I appreciate the enthusiasm, but this isn't a feature we're going to accept a contribution on (especially not a first contribution to the project) — we need to do a lot of design work first.

---

_Comment by @goof-bug on 2025-09-30 17:48_

> [@goof-bug](https://github.com/goof-bug) I appreciate the enthusiasm, but this isn't a feature we're going to accept a contribution on (especially not a first contribution to the project) — we need to do a lot of design work first.

I understand, I was just really missing this feature myself and thought I'd help make it happen rather than just add another upvote on an issue. 😄 

---

_Comment by @inoa-jboliveira on 2025-10-01 07:36_

There's isn't a lot to design needed here, to be quite honest. You don't need to crack the code the 1st try. Just give us a sane interface via `uv run` on a pyproject.toml section and then you expand on it later. 

I get the Astral team is small and likely have other higher priorities, but you can make 99% of us happy here with maybe a few days of work if you decide to just ship something minimal as opposed to something perfect and highly compliant to whatever non existent PEP specs we've been waiting to appear. There are more people interest enough to help if the design is decided, as you say. 

I'm not being pushy or anything, I'm just really keen to drop many crutches on my projects. We all love `uv` here. 

So here comes some "design" based on rye, poe, taskipy and a sliver of let's get crackin

Basic:
```toml
[tool.uv.tasks]
hello = "echo Hello from uv!"
lint = "ruff check ."
```

Complete:
```toml
[tool.uv.tasks]
test = { 
  cmd = "pytest tests/", 
  description = "Run unit tests with coverage",
  cwd = "src/",  # Change working directory relative to project root
  env = { "ENV_VAR" = "value" },  # Custom environment variables
  depends-on = ["lint"],  # Run these tasks first (sequential)
  pre = ["build-docs"],  # Pre-hooks (run before cmd)
  post = ["clean-up"],  # Post-hooks (run after cmd, even on failure)
  requires = ["pytest-cov"],  # Additional dependencies to install/resolve if missing (as dev deps)
  shell = true,  # Enable shell interpolation/expansion (e.g., for $VAR or wildcards)
  parallel = false,  # If true, run depends-on in parallel (inspired by Poe)
  dotenv = ".env.test"  # Load specific .env file; default to ".env" if true
}

chain-example = [
  "uv run lint",  # Chain commands or other tasks as a list
  "uv run test"
] 
```

You can even start with fewer of these options. I can write a more detailed spec of this if you would like. 



---

_Comment by @doraeric on 2025-10-13 09:54_

I had also been waiting for this feature in uv, but it still hasn’t been released so far. I found another tool called [mise](https://mise.jdx.dev/tasks/) that can work as a task manager, so I’m sharing it here as an alternative.
You’ll need to create a `mise.toml` file like this:
```toml
[env]
# _.file = '.env'
_.python.venv = { path = ".venv" }

[tasks.start]
description = "Run the application"
run = "python3 main.py"
```
Then you can simply run `mise start`.
Hopefully uv will add this feature soon.

---

_Comment by @almereyda on 2025-10-15 15:30_

In your example `.venv` is already populated and `python3` is available.

Is there an option to include support for a transparent `uv sync` before executing the command, in so `.venv` is available in all possible cases of running `mise start`, e.g. directly from a fresh clone? Would replacing `python3` with `uv run` suffice?

---

_Comment by @doraeric on 2025-10-16 02:48_

> Is there an option to include support for a transparent `uv sync` before executing the command, in so `.venv` is available in all possible cases of running `mise start`, e.g. directly from a fresh clone? Would replacing `python3` with `uv run` suffice?

It looks like you can specify dependency of a task \[[ref](https://mise.jdx.dev/tasks/architecture.html#depends-prerequisites)\], so this should work
```toml
[env]
# _.file = '.env'
_.python.venv = { path = ".venv" }

[tasks.install]
run = "uv sync"

[tasks.start]
description = "Run the application"
depends = ["install"]
run = "python3 main.py"
```

Although I personally prefer using separate commands, just like `yarn install` and then `yarn start`.

Since I use uv to create venv and mise would load venv to your shell environment base on `_.python.venv`, `python3` in the start task should be the same as `uv run python3`.

---

_Comment by @God-damnit-all on 2025-10-16 23:46_

The lack of this feature is pretty much why I haven't migrated any of my rye projects yet. There are alternatives, sure, but it feels like setting that up and getting everything converted is just going to amount to wasted time once uv implements its own. I wish it was considered a higher priority.

---

_Comment by @chalbersma on 2025-10-17 00:08_

Is there any version of these scripts that supports running the script with a dependency group specified? For example, running test or lint with dev dependencies or runing a "docs" command with a "docs" dependency group?

---

_Comment by @spacecoffin on 2025-10-17 00:22_

> Is there any version of these scripts that supports running the script with a dependency group specified? For example, running test or lint with dev dependencies or runing a "docs" command with a "docs" dependency group?

The pairing of scripts with environments is a feature that I love from Hatch: https://hatch.pypa.io/latest/config/environment/overview/

---

_Comment by @brandon-avantus on 2025-10-17 16:58_

> The lack of this feature is pretty much why I haven't migrated any of my rye projects yet. There are alternatives, sure, but it feels like setting that up and getting everything converted is just going to amount to wasted time once uv implements its own. I wish it was considered a higher priority.

@God-damnit-all I made [pyproject-runner](https://pypi.org/project/pyproject-runner/) to make transitioning from Rye a breeze. It requires minimal changes to the pyproject.toml. It might be a good intermediate step to uv until a built-in method is implemented.

---

_Comment by @txchen on 2025-10-24 20:52_

I am using https://github.com/pyprojectx/pyprojectx, before uv ships this super important feature.

---

_Comment by @linux-china on 2025-10-25 10:40_

I added "rye scripts" support in my tool [task-keeper](https://github.com/linux-china/task-keeper/releases/tag/v0.30.7) 

Add following code in pyproject.toml: 

```toml
[tool.rye.scripts]
python-version = "python3 --version"
hello-uv = { cmd = "echo 'hello uv'" }
```

and run `tk hello-uv` 


---

_Comment by @ksamuel on 2025-11-20 20:12_

I come back every month or so on this ticket, and it's been a year now. I just want to take the time to say that it's still a very interesting feature, and I already ran uv in 6 projects in which it would have been handy.

I do appreciate that you want to decide on a good design, and that the myriad of different opinions in the thread is not helping.

I post to keep the interest in this alive and not buried at the bottom of the big pile of ponies we all wish from Astral.

Cheers

---

_Comment by @AshciR on 2025-11-21 00:58_

Same @ksamuel . I literally just came here looking for this. I missed this feature from rye

---

_Comment by @duke8585 on 2025-11-24 13:41_

> I come back every month or so on this ticket, and it's been a year now. I just want to take the time to say that it's still a very interesting feature, and I already ran uv in 6 projects in which it would have been handy.
> 
> I do appreciate that you want to decide on a good design, and that the myriad of different opinions in the thread is not helping.
> 
> I post to keep the interest in this alive and not buried at the bottom of the big pile of ponies we all wish from Astral.
> 
> Cheers

still still is :)

---

_Comment by @tuyennv-se on 2025-11-29 01:03_

I use PDM because it has this feature.

---

_Comment by @StevenACoffman on 2025-12-16 12:45_

_**Please**_ choose to delegate any taskfile to some popular alternative like [just](https://github.com/casey/just) (also in Rust! supports Python recipes!) rather than beginning down the road of slowly mutating into a new ad hoc taskfile format that tries to be all things to all people. Providing some convenient passthrough, shortcut, or alias to something external that specializes in task running (and orchestration) would be plenty.

`uv` does what it does so well because of its laser focus on solving its narrowly targetted problem space _well_. `ruff` and `uv` are separate for a reason. Each has a very well-defined boundary to its own value proposition.

If the targeted scope of `uv` expands to include various half-hearted attempts at reinventing solutions for _other_ problem domains _and_ _those do not work out so well,_ it only encourages the community to "make a better kitchen sync" `uv` alternative or "make a simpler" `uv` alternative.

I'm just so incredibly tired of the fractured ecosystem, and `uv` and `ruff` have finally given me new hope. I'm very emotionally invested in this _not_ just going the same way the last few attempts have. 🤞 Thanks for giving me hope. 🫶 

---

_Comment by @mdaeron on 2025-12-16 13:35_

@StevenACoffman, I understand where you're coming from and I want to be convinced. To me (= a researcher interested in reproducible science), everything hinges on being able to promise users that all they need to install is `uv`. Can you point me to a description of what I need to do for users to run `just` recipes, with only `uv` as a requirement? I need to specify some flavor of `just` as a dependency, right? Then users can do `uv run just` transparently? Or is it `uvx just`? If this can be made painless/invisible to the end-user, I think you may have a point.

---

_Comment by @davidpoblador on 2025-12-16 13:52_

> [@StevenACoffman](https://github.com/StevenACoffman), I understand where you're coming from and I want to be convinced. To me (= a researcher interested in reproducible science), everything hinges on being able to promise users that all they need to install is `uv`. Can you point me to a description of what I need to do for users to run `just` recipes, with only `uv` as a requirement? I need to specify some flavor of `just` as a dependency, right? Then users can do `uv run just` transparently? Or is it `uvx just`? If this can be made painless/invisible to the end-user, I think you may have a point.

Chiming in: while I also agree that a setup where `uv` is the *only* required tool would be ideal, the `just` solution doesn’t seem unreasonable—at least as a stop-gap. This is especially true if `just` can be packaged and installed via `uv` in a way that’s effectively invisible to end users—for example via a prebuilt distribution like `just-bin` (<https://pypi.org/project/just-bin/>), which can then be invoked transparently via `uvx --from just-bin just`.

---

_Comment by @mdaeron on 2025-12-16 15:39_

> This is especially true if `just` can be packaged and installed via `uv` in a way that’s effectively invisible to end users—for example via a prebuilt distribution like `just-bin` (https://pypi.org/project/just-bin/), which can then be invoked transparently via `uvx --from just-bin just`.

As a stop-gap, sure.

While in theory I agree that there should be little difference between `uvx --from just-bin just build-docs` and shorter, more expressive forms like `uvx run build-docs`, in practice, from a user experience point of view, `uvx --from just-bin just build-docs` is going to be gibberish to many users, with potential confusion with `uvx --from just just-bin`, while `uv run build-docs` will be much less likely to be garbled.

There's clearly a trade-off here, and we might have different sweet spots for usability vs focus. For me, I'm still of the opinion that `uv` with a built-in "recipe" feature would be incredibly useful for scientific reproducibility.

---

_Comment by @julian-hecker on 2025-12-16 18:22_

Personally, I like having UV as the kitchen sink of Python development. 

In the web development world, npm is bundled with Node and it handles not just installing packages, but also running scripts. You could install additional task runners like Grunt or Gulp, but you can run just about any CLI command with `npm run <script>` from package.json scripts, without needing additional dependencies.

If uv could have functionality of a task runner like [poethepoet](http://poethepoet.natn.io/index.html) which simply lets you define CLI scripts in your pyproject.toml and run them with `poe <script>`, it would be a huge win to not have to install an additional dependency to run your scripts.

Should there be a survey of uv users to see if this is something that they'd like to have?

---

_Comment by @sinokadev on 2025-12-16 19:13_

I want this

---

_Comment by @simonw on 2025-12-16 22:26_

I just found myself wanting this to replace the following pattern:
```bash
cd docs/
make livehtml
```
In many of my projects it currently does this: https://github.com/simonw/s3-credentials/blob/de3b9c14bb8515b26bec6a34ea30d85ab5cbaa8f/docs/Makefile
```makefile
# Minimal makefile for Sphinx documentation
#

# You can set these variables from the command line.
SPHINXOPTS    =
SPHINXBUILD   = sphinx-build
SPHINXPROJ    = sqlite-utils
SOURCEDIR     = .
BUILDDIR      = _build

# Put it first so that "make" without argument is like "make help".
help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.PHONY: help Makefile

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

livehtml:
	sphinx-autobuild -b html "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(0)
```
So I really want to have something in my `pyproject.toml` that lets me run the equivalent of that `livehtml` target and have everything just work.

---

_Comment by @simonw on 2025-12-16 22:42_

... I ended up using [Poe the Poet](https://poethepoet.natn.io/index.html) for the moment:

```toml
[project]
name = "s3-credentials"
version = "0.16.1"
description = "A tool for creating credentials for accessing S3 buckets"
readme = "README.md"
authors = [{name = "Simon Willison"}]
license = {text = "Apache-2.0"}
requires-python = ">=3.10"
dependencies = [
    "click",
    "boto3",
]

[project.urls]
Homepage = "https://github.com/simonw/s3-credentials"
Issues = "https://github.com/simonw/s3-credentials/issues"
CI = "https://github.com/simonw/s3-credentials/actions"
Changelog = "https://github.com/simonw/s3-credentials/releases"

[project.scripts]
s3-credentials = "s3_credentials.cli:cli"

[tool.poe.tasks]
docs = "sphinx-build -M html docs docs/_build"
livehtml = "sphinx-autobuild -b html docs docs/_build"
cog = "cog -r docs/*.md"

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[dependency-groups]
test = [
    "pytest",
    "pytest-mock",
    "cogapp",
    "moto>=5.0.4",
]
docs = [
    "furo",
    "sphinx-autobuild",
    "myst-parser",
    "cogapp",
]
dev = [
    {include-group = "test"},
    {include-group = "docs"},
    "poethepoet>=0.38.0",
]
```
Now I can run:
```bash
uv run poe livehtml
```

---

_Comment by @chrisrodrigue on 2025-12-17 00:14_

It would be awesome if Python formalized `task-backend`. 

Like build backends, task runners are a dime a dozen and range widely in features and complexity. A PEP that formally specs out the minimum requirements for the project task runner interface and defines a namespace in pyproject.toml would be a positive step forward for establishing some consistency and greater separation of concerns.

---

_Comment by @ofek on 2025-12-17 01:04_

@simonw If you're interested in learning new tech (i.e. you ended up not choosing Hatch to define/run scripts) you might be interested in the command runner [just](https://just.systems/man/en/). I recently became a co-maintainer of `msgspec` and revamped the entire [developer experience](https://github.com/jcrist/msgspec/blob/main/.github/CONTRIBUTING.md) to be based on `just` and uv. I [define](https://github.com/jcrist/msgspec/blob/main/.justfile) config that, in a rudimentary way, emulates Hatch environments so that certain scripts are able to target specific virtual environments that are backed by uv.

---

_Comment by @AKuederle on 2025-12-17 16:45_

> ... I ended up using [Poe the Poet](https://poethepoet.natn.io/index.html) for the moment:
> 
> [project]
> name = "s3-credentials"
> version = "0.16.1"
> description = "A tool for creating credentials for accessing S3 buckets"
> readme = "README.md"
> authors = [{name = "Simon Willison"}]
> license = {text = "Apache-2.0"}
> requires-python = ">=3.10"
> dependencies = [
>     "click",
>     "boto3",
> ]
> 
> [project.urls]
> Homepage = "https://github.com/simonw/s3-credentials"
> Issues = "https://github.com/simonw/s3-credentials/issues"
> CI = "https://github.com/simonw/s3-credentials/actions"
> Changelog = "https://github.com/simonw/s3-credentials/releases"
> 
> [project.scripts]
> s3-credentials = "s3_credentials.cli:cli"
> 
> [tool.poe.tasks]
> docs = "sphinx-build -M html docs docs/_build"
> livehtml = "sphinx-autobuild -b html docs docs/_build"
> cog = "cog -r docs/*.md"
> 
> [build-system]
> requires = ["setuptools"]
> build-backend = "setuptools.build_meta"
> 
> [dependency-groups]
> test = [
>     "pytest",
>     "pytest-mock",
>     "cogapp",
>     "moto>=5.0.4",
> ]
> docs = [
>     "furo",
>     "sphinx-autobuild",
>     "myst-parser",
>     "cogapp",
> ]
> dev = [
>     {include-group = "test"},
>     {include-group = "docs"},
>     "poethepoet>=0.38.0",
> ]
> 
> Now I can run:
> 
> uv run poe livehtml

Just as a note to this (I think this is mentioned further up in the comment chain already), `poe` supports uv directly. What this means is that if you install poe (`uv tool install poethepoeth`) and then run `poe <you-task>` within your repo, poe will recognize that you are within a uv project and will actually call the scripts, tasks (or what ever you defined), with `uv run`, ensuring that the venv is active and in sync.

For me that is the perfect combination. For people that just want to work with the project, they only have to install uv (poe is installed via dev dependencies). And for "power users" that get annoyed by typing `uv run poe` all the time, they can run an additional `uv tool install poe` and then use `poe <task>`

---

_Comment by @shoriminimoe on 2025-12-17 17:03_

> Just as a note to this (I think this is mentioned further up in the comment chain already), poe supports uv directly. What this means is that if you install poe (uv tool install poethepoeth) and then run poe <you-task> within your repo, poe will recognize that you are within a uv project and will actually call the scripts, tasks (or what ever you defined), with uv run, ensuring that the venv is active and in sync.

My team uses `poe` as the task runner in our monorepo. We tried to take advantage of the uv support to produce subcommands for the subpackages, but it would leave the virtual environment in an unclean state (transient dependencies left in the environment) when performing tasks in different packages.

We landed on using `uv run --exact --directory ./path/to/package poe` to make sure the environment is correct. At the top level of my repo, I can run `poe package1 test` to run the `test` task in package1:


```toml
[tool.poe]
executor.type = "uv"

[tool.poe.tasks]
package1 = "uv run --exact --directory ./packages/package1 poe"
package2 = "uv run --exact --directory ./packages/package2 poe"
package3 = "uv run --exact --directory ./packages/package3 poe"
package4 = "uv run --exact --directory ./packages/package4 poe"

[tool.poe.tasks.all]
help = "Run a task for all packages i.e. lint, format, etc."
args = [{ name = "task", positional = true, required = true }]
ignore_fail = "return_non_zero"
sequence = [
  "package1 {task}",
  "package2 {task}",
  "package3 {task}",
  "package4 {task}",
]

```

---

_Comment by @God-damnit-all on 2025-12-17 21:05_

> I added "rye scripts" support in my tool [task-keeper](https://github.com/linux-china/task-keeper/releases)
> 
> Add following code in pyproject.toml:
> 
> [tool.rye.scripts]
> python-version = "python3 --version"
> hello-uv = { cmd = "echo 'hello uv'" }
> and run `tk hello-uv`

This is honestly the best solution presented in this issue thus far, you can use the old rye syntax directly without having to adapt to a new format. It's very simple. I really like how this project intelligently determines what to look for.

---

_Comment by @adrianovieira on 2025-12-17 22:17_

Nowadays I am using the Just[^just]

`justfile` sample:

```justfile

# list all available recipes
@a_default:
    just --list

# fastapi app in dev mode
@dev:
   fastapi dev src/api/main.py

# execute package manager (e.g.: uv)
@uv *args='':
    uv $@

# run test framework (pytest)
@test *args='':
    pytest $@

```

So… just

```bash
$ just uv <args>

$ just dev

$ just test [args]
```


[^just]: Just https://just.systems/

---

_Comment by @mathmul on 2026-01-01 08:34_

Not sure if mentioned (Cmd+F "uvx --from=rust-just just" finds nothing), but I found it best to use `just` with the following shebang at the top to avoid any additional dependencies:
```
#!/usr/bin/env -S uvx --from=rust-just just --justfile
```
Except for the very first time, it works super fast.

Caveat: You have to use `./justfile <args>` instead of `just <args>`. (`alias just='./justfile'`?)

---

_Comment by @haydenk on 2026-01-05 02:08_

I wanted this initially so it could match pdm since both tools use `pyproject.toml` but I have since gotten mise tasks to work really well with uv, so I'm good for now.

---
