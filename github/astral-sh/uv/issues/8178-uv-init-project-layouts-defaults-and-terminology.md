---
number: 8178
title: "uv init: Project layouts, defaults and terminology"
type: issue
state: open
author: MikeHart85
labels:
  - needs-decision
assignees: []
created_at: 2024-10-14T17:19:54Z
updated_at: 2025-11-20T18:09:43Z
url: https://github.com/astral-sh/uv/issues/8178
synced_at: 2026-01-07T13:12:17-06:00
---

# uv init: Project layouts, defaults and terminology

---

_Issue opened by @MikeHart85 on 2024-10-14 17:19_

Thank you for working on this wonderful tool.

I'd like to suggest several changes to `uv init` arguments and defaults, with the following goals:

- Use consistent and established terminology
- Options to support common usage patterns, so seasoned Python developers can have things their way
- Defaults which guide Python newcomers towards best practices, hint at available features

Much of this revolves around separating the concept of project directory structure ("layout" in Python) from project purpose and contents ("library", "application", etc), as well as disambiguating the overloaded term "package". Currently these terms are conflated and inconsistent in `uv`.

Apologies in advance for the massive wall of text.


## Current behavior

"Application" (default):
```sh
$ uv init [--app] project-name
$ tree project-name/
project-name/
├── hello.py
├── pyproject.toml  # only [project] (builds don't work due to misnamed hello.py)
└── README.md
```

"Library":
```sh
$ uv init --lib project-name
project-name/
├── pyproject.toml  # [project], [build-system]
├── README.md
└── src
    └── project_name
        ├── __init__.py  # contains "hello" code
        └── py.typed
```

"Application Package":
```sh
$ uv init [--app] --package project-name  # --app appears to do nothing here, in spite of docs suggesting it
project-name/
├── pyproject.toml  # [project], [project.scripts], [build-system]
├── README.md
└── src
    └── project_name
        └── __init__.py  # contains "hello" code
```


## Suggested behavior

Src layout (default):
```sh
$ uv init [--layout=src] project-name
project-name/
├── pyproject.toml  # [project], [project.scripts], [build-system]
├── README.md
└── src
    └── project_name
        └── __init__.py  # empty
        └── example.py   # def say_hello(): print("...")
```

Flat layout:
```sh
$ uv init --layout=flat project-name
project-name/
├── pyproject.toml  # [project], [project.scripts], [build-system]
├── README.md
└── project_name
    └── __init__.py  # empty
    └── example.py   # def say_hello(): print("...")
```

Single module layout:
```sh
$ uv init --layout=single project-name
project-name/
├── pyproject.toml  # [project], [project.scripts], [build-system]
├── README.md
└── project_name.py  # def say_hello(): print("...") 
                     # NOTE: file name MUST be project_name.py (it replaces the package)
```

Bare / no layout:
```sh
$ uv init --layout=<bare|none> project-name  # maybe pick one rather than allowing either one
project-name/
├── pyproject.toml  # [project]
└── README.md
```

- Add `--no-entrypoint` to suppress `[project.scripts]`
  - Consider adding `--entrypoint=name` to customize key name under `[project.scripts]`, defaulting to `project-name`
- Add `--build-system=<hatchling|...|none>`, with `hatchling` being default, and `none` to suppress `[build-system]`
  - Consider adding `--no-build-system`, synonymous with `--build-system=none`, for symmetry
  - Remove `--no-package`
- Add `--typed` to create a `py.typed`
- Remove `--package`
- Remove `--app`, or:
  - Make synonymous with `--entrypoint=project-name`
  - Consider generating example code to be more "app-like"
  - Consider `__main__.py` [[1]](https://docs.python.org/3/library/__main__.html#main-py-in-python-packages) instead of `example.py`, but that may unnecessarily confuse newcomers
- Remove `--lib`, or:
  - Make synonymous with `--no-entrypoint`
  - Consider generating example code to be more "lib-like"
  - Consider implying `--typed` (IMO explicit `uv init --lib --typed` would be better)

I would just remove `--lib` and `--app` to keep things simple.


## Reasoning for suggestions

- "Entry point" is the established term for shortcuts to functions that ultimately become commands when installed [[2]](https://packaging.python.org/en/latest/specifications/entry-points/)
- "Layout" is the established term for project directory structure [[3]](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#src-layout), [[4]](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)
  - Src layout should be default
    - It is the most robust, avoids import conflicts, most suitable for serious projects
    - Prevents needing to restructure when project outgrows other layouts
    - Uv eliminates its main downside (very easy to get up-to-date REPL):
    -  `uv run python`
    -  `>>> import project_name`
  - Flat layout is also quite popular, and currently not available in `uv init`
  - Single module (current `--app`, default) layout is the most limited
    - It should most certainly not be the default
    - Has issues and limitations which will confuse newcomers
    - Should really only be used for single file projects, if at all
    - Currently broken for builds due to example file always being `hello.py`
    - Changing file to `project_name.py` allows builds to work
  - `--layout=none` for custom layouts, notebook projects, etc
- Project layout is not related to whether it is an "app" or "lib"
  - Any layout can be an application or a library
  - Project contents and usage determine whether it is an application, or library, or both
  - Not as clear cut in Python as "executable" vs "library" projects in compiled languages, shouldn't attempt to force that pattern since it doesn't fit
  - "App" and "Application" terms should probably be reserved for possible integration with tools like PyInstaller, cxfreeze, etc (IE, generating a self-contained binary executable for distribution)
  - A library isn't required to have [PEP 561](https://peps.python.org/pep-0561/) compliant `py.typed`
- Current use of `--package` conflates it with build-system/distribution, src layout, and absence of `py.typed`
  - A "package" in Python is simply a directory with an `__init__.py` [[5]](https://docs.python.org/3/glossary.html#term-regular-package)
  - Ideally, the term "package" should not be used for anything else, to avoid confusion
  - A package doesn't have to be distributable / have a build-system
  - Single module (IE, non-package) distributions are a thing (hence `--layout=single` with build-system)
    - A prominent example would be `six`, which is a single module library: [PyPI](https://pypi.org/project/six/) [GitHub](https://github.com/benjaminp/six)
- `__init__.py` should not contain general purpose code
  - `__init__.py` is a special purpose file, and putting arbitrary code there can easily have unintended side effects
  - Its meant for initializing packages, making symbols available at package level, `__all__`, etc [[6]](https://docs.python.org/3/tutorial/modules.html#packages)
  - Putting the hello world example there can mislead newcomers into thinking this is where all their code belongs
- Don't see a reason not to include entry point and build system by default in all layouts which support it
  - Can easily be removed / ignored / switched off if not needed
  - Default presence shows newcomers what is possible / where it belongs
  - Reduces friction when a project graduates from hobby to distribution
- Layouts are mutually exclusive, hence grouping them under one argument with a value
  - Less room for confusion than having multiple differently named options, which may or may not be mixable
- All three layouts (src, flat, single) work with `uv run project-name` and `uv build` as shown
  - Only `uv init` needs to change, to support generating them

## Context

- Src layout used to be the default until: https://github.com/astral-sh/uv/pull/6689
- "App", "lib" and "package" terminology introduced around here: https://github.com/astral-sh/uv/pull/6585

Discussion in the latter floated using "distribution" (ala PDM) instead of "package" (ala Poetry) in some contexts, but it seems "package" won out. I would argue "distribution" would have been the correct choice. As mentioned, "package" has a very specific meaning in Python, not strictly related to whether the project is built for distribution.

Thanks for considering.

---

_Comment by @Weilet on 2024-10-24 15:10_

I think `uv init --layout=<bare|none>` is very useful for me when it comes to creating a Django app. I am searching for a way to initialize the project with only a `pyproject.toml`, so that I can use `django-admin` to manage my project without too much unnecessary stuff. 


---

_Assigned to @zanieb by @zanieb on 2024-10-24 15:38_

---

_Label `needs-decision` added by @zanieb on 2024-10-24 15:38_

---

_Comment by @zanieb on 2024-10-24 15:40_

Thanks for your thoughts! I'll own responding to this and seeing what we can integrate.

---

_Comment by @drcongo on 2024-11-02 20:12_

I've only ended up in this thread because I was confused about the default behaviour here - to me it feels like `uv init` with no options should do the equivalent of `uv init --layout=<bare|none>` mentioned above and create nothing but a `pyproject.toml`. I can't imagine ever needing uv to create the `.gitignore`, `.git/` and `hello.py` so for that to be the default seems kinda wild.

Feel free to ignore me though, I realise I'm a data point of one and maybe those do somehow make sense to other devs. 

---

_Comment by @thcrt on 2024-11-06 13:25_

I think both the options available and their documentation are somewhat confusing at present. There are multiple different decisions in determining how a project should be structured, but those decisions seem to be split between `uv init`'s sub-optimal presets and further configuration options in `pyproject.toml` that need to be set manually.

Often, I might want to create a standalone application, such as a Flask webapp. This doesn't need to expose a Python API or be imported into other projects, so I would assume I don't need a build system to turn my code into a package. I'll probably just write a Dockerfile that will produce a container image with my dependencies installed, ready to run my code directly.

In terms of layout, I probably want a ['flat layout'](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/), where my code is in `./myapp`. This is what Flask, for instance, [recommends](https://flask.palletsprojects.com/en/stable/tutorial/layout/). That's not available from `uv init` -- I have to choose between:
- a 'Packaged Application' 
  This configures a build system I don't need and nests all my code in `./src/myapp`.
- a 'Library'
  This has all the downsides of a 'Packaged Application', and also doesn't specify a command-line entry point.
- an 'Application'
  This puts my code in `.`, the project root, which will get cluttered quickly if I want to build anything bigger than a single-file script.

None of these do what I want! No matter what I choose, I'll have to move things around and change `pyproject.toml` to account for the changes. 

This is addressed in #6460, which seems to be resolved by #6585, adding 'Virtual Projects', which don't get built or installed as packages. But the only place this is documented is [under the setting that enables it](https://docs.astral.sh/uv/reference/settings/#package), where I can read a brief summary of what a virtual project _is_, but not why I might want to use one. #6585 states that a project will be treated as 'virtual' merely by the lack of a `[build-system]` in `pyproject.toml`, but this isn't documented there. The [documentation on `uv init`](https://docs.astral.sh/uv/concepts/projects/#creating-projects) doesn't mention 'virtual projects' at all.

A brief aside regarding 'virtual projects' and `uv init`: it seems like  `uv init --virtual` creates a virtual project, but the `--virtual` flag can't be used with `--package` (for a 'Packaged Application') or with `--lib` (for a 'Library'). And `uv init --virtual` produces exactly the same result as `uv init` without the `--virtual` flag, not even adding `package = false` to `pyproject.toml`. So it's not clear to me what the flag is supposed to do.

I love `uv` as a whole, but this is extremely confusing to me. Maybe I'm mistaken on some of this, and I'm doing something wrong, in which case please feel free to correct me.

---

_Comment by @junapur on 2024-11-19 04:48_

I agree. As someone migrating from Poetry to uv, uv's project layout options were a lot harder for me to understand.

---

_Comment by @jromal on 2024-12-08 09:48_

> I've only ended up in this thread because I was confused about the default behaviour here - to me it feels like `uv init` with no options should do the equivalent of `uv init --layout=<bare|none>` mentioned above and create nothing but a `pyproject.toml`. I can't imagine ever needing uv to create the `.gitignore`, `.git/` and `hello.py` so for that to be the default seems kinda wild.

I can follow the logic for not wanting a "hello.py", but I can fully understand that an example (in the terms mentioned by the request) is a good thing for most of the people. I cannot support not creating the ".git" and ".gitignore" files. All amateurs (which I count myself) should have it, but many do not know about it. Nothing serious should be done without them.

What we need is a revamp on the "layout" structure. Which is confusing. I had to test all the options to understant what is meant. With the "--layout" option is clear and does what people do. .

---

_Comment by @clbarnes on 2025-01-22 11:01_

> I can't imagine ever needing uv to create the .gitignore, .git/ and hello.py so for that to be the default seems kinda wild.

Counterpoint: I can't imagine ever needing to start a python project without a `.gitignore` file, and it's annoying to go and fetch the exact same file every time, when uv aims to be a one-stop shop. Of course, a minority of people don't use git for version control, although a host of other dev tools respect .gitignore when it comes to ignoring files to search/ index/ include so arguably it's useful to have even if you're not using git.

I think that `hello.py`, while deleted/ renamed immediately by developers who start new projects regularly, acts as a useful marker for where your functional code should go. Between script, app, and library projects, flat or src layouts, multi-package projects etc., that may not be clear to someone less experienced, or even an experienced developer who hasn't followed the breakneck pace of the evolution of python's best practices in the last few years.

---

_Comment by @drcongo on 2025-01-22 13:24_

@clbarnes Not everyone is starting a new project, nor running uv where their .gitignore, .git repo or README lives - think mono-repos, projects where uv is inside docker, projects where you're in a git submodule etc. - in some of these cases, creating a .git repo is a positively hostile thing to do as you can suddenly end up with nested repositories being pushed and then someone (me) has to untangle it.

---

_Comment by @allenc97 on 2025-01-24 05:38_

Lots of insightful thoughts. I would also argue for project layout to be separated from rather the project is intended to be an application or a python package. In fact, it is not uncommon for python applications to also use a src layout. This is especially useful with containers as mounting all source code files is just a singular mount, or for standardized use in CI/CD.

---

_Comment by @Bunker-D on 2025-01-28 21:16_

I second the idea of a more customized option for `uv init`. I would also like the option to setup a _**personal default configuration**_.

Additionally, it would be neat to be able to **customize the starting `.gitignore`** (at least, setting up a _**personal default `.gitignore`**_).

Typically, I would systematically exclude `.*_cache` (`.pytest_cache` and `.ruff_cache`) and `.venv`, and I'm pretty sure developers' needs would change based on their favorite tools.

---

_Comment by @Bunker-D on 2025-01-28 21:47_

Also, while I don't disagree with @clbarnes 's justification of `hello.py` (making me neutral about the creation of `hello.py`), I think `src/<…>/__init__.py` should not be initialized with code inside it. My big issue with this behavior is that this dummy code not visible at first sight when looking at the project. Worse than having to clean it up, devs might miss it,

---

_Referenced in [astral-sh/uv#11192](../../astral-sh/uv/pulls/11192.md) on 2025-02-03 19:55_

---

_Comment by @chrish42 on 2025-02-11 16:25_

Also coming here because I love the `uv` experience overall... except for its package layout creation options, which I found really confusing. I wanted to create a src-layout for a library that was going to be packaged and released eventually, and by far most of my time struggling with `uv` was spent trying to figure out how to get that config.

The `--library`, etc. options are not great currently, because they bundle too many things together. It's totally fine to have defaults for new users (and please do that). But if I want a src-layout for my project (whatever it is), it would be so much easier to just ask explicitly ("better than implicit") for it by tacking on a `--layout=src` option to my `uv init` invocation.

Also, I was kind of expecting this choice of layout to be also reflected in the section for `uv` of the `pyproject.toml` file, at least, so all `uv` invocations know about it. Right now, I have a makefile whose main function is to tack on `uv run --project src` to all the commands I need to run during development.

For me, this is the only thing making me hesitate to recommend `uv` more broadly to my team.

---

_Comment by @zanieb on 2025-02-11 16:29_

> The --library, etc. options are not great currently, because they bundle too many things together. It's totally fine to have defaults for new users (and please do that). But if I want a src-layout for my project (whatever it is), it would be so much easier to just ask explicitly ("better than implicit") for it by tacking on a --layout=src option to my uv init invocation.

What things did you not want?

---

_Comment by @zanieb on 2025-02-11 16:30_

> Right now, I have a makefile whose main function is to tack on uv run --project src to all the commands I need to run during development.

I'm a little confused by this, you want to run with your working directory as `src/`?

---

_Comment by @hunter-gatherer8 on 2025-02-20 02:26_

How about allowing users to provide their own configuration template in some uv config directory? Or even several templates one could choose with something like `uv init --template=my-named-preset`?

---

_Comment by @zanieb on 2025-02-20 03:35_

@hunter-gatherer8 please see https://github.com/astral-sh/uv/issues/9754

---

_Comment by @ivan-kleshnin on 2025-02-27 11:05_

Can someone take a little time to elaborate the reasoning behind "src" layout for non-Pythonistas 🙏 

With "flat" layout:

```
project_name/
  project_name/
```

we already have a folder-level isolation. It does look "nested" to a TypeScript-first version of me 😄 I get that the project can have multiple packages and those packages will, supposedly, have common files under `project_name/src`. But is there any benefit for simple (and probably more frequent) *one-package-per-project* cases?

Quoting the article, that everyone refers to:
https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/

> The src layout requires installation of the project to be able to run its code, and the flat layout does not.

I don't get this point. With Poetry I had to install my library package, and it definitely has the "flat" layout.

> The src layout helps prevent accidental usage of the in-development copy of the code.

This point is more clear. But it's a "protection by convention", not the strongest argument. We can still accidentally put a non-public data or code in `src`. So, again, I would like to learn pros/cons of each layout better, maybe someone can suggest another article or video?

---

_Comment by @astrojuanlu on 2025-02-27 11:23_

> Can someone take a little time to elaborate the reasoning behind "src" layout for non-Pythonistas 🙏

`import package_name` may pick up the code from your source checkout and not the installed one depending on your current working directory. src prevents this

---

_Comment by @konstin on 2025-02-27 15:28_

https://hynek.me/articles/testing-packaging/ and https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure have some discussion of the topic.

---

_Comment by @clbarnes on 2025-02-28 15:03_

> Can someone take a little time to elaborate the reasoning behind "src" layout for non-Pythonistas 🙏

Python was originally designed as a scripting language, more akin to bash or perl than Java. As such, it's pretty liberal about sweeping local directories for python code to import and execute. However, there are a few different ways of executing python code and that module discovery happens differently in different execution contexts. Some tools designed with that style in mind (e.g. testing utilities) also do things under the hood to make things "easier", which actually makes module discovery even more complicated.

As python has been used for bigger, more complicated projects, this module discovery complexity has been found to add little utility and a lot of headaches. Isolating your code in a `src` folder prevents it from being auto-discovered in most cases, which improves consistency, puts the developer in control of what is imported and how, and makes your development environment behave more similarly to your deployment environment.

Python as a language has changed to support bigger, more complicated projects (e.g. `typing`), as has the tooling around it (e.g. `uv`!).

Also it helps separate the actual module from general project-related cruft like CI configuration, scripts, infrastructure setup, docs etc..

---

_Comment by @matthias-busch on 2025-03-04 15:44_

Coming from using `poetry` I also find the `uv init` options a bit confusing and convoluted. 

I personally would prefer the pretty simple/bareback approach of poetry's `poetry new some-name` for a project skeleton that looks like this:
```
❯ tree bar-delete-later
bar-delete-later
├── README.md
├── bar_delete_later
│   └── __init__.py
├── pyproject.toml
└── tests
    └── __init__.py
```

and `poetry new --src some-name` resulting in:
```
❯ tree foobar-delete-later
foobar-delete-later
├── README.md
├── pyproject.toml
├── src
│   └── foobar_delete_later
│       └── __init__.py
└── tests
    └── __init__.py
```

This might be a simple case of personal preference and I totally (and welcome) uv's intent to being as user-friendly as possible but I personally think pruning back on the options and just giving options for the src/flat layout would suffice and more straightforward. 

I would also love to have the `tests` folder generated by default  for both approaches.

---

_Comment by @jlcheng on 2025-03-07 16:50_

I am grateful that this conversation is happening here. As someone who recently needed to setup a Python project, I ran into the flat vs src decision myself. In the end, I decided that I would use the src layout as my default, as well as making the choice to use `uv` as my preferred tool. But the src-vs-flat choice in Python will probably never go away and I think it won't be easy for uv to, for the lack of a better phrase, "make the _right_ choice." There won't be a perfect answer.

Having said that, I think simply discussing this matter in the uv documentation will go a long way to helping the community, especially people new to Python project layouts like me. Today, the uv documentation (https://docs.astral.sh/uv/concepts/projects/init/) discusses the difference between "Application", "Packaged Application", and "Library". This page could be a good place to discuss why the uv team chose Application as the default and what the implications are (e.g., perhaps referencing https://hynek.me/articles/testing-packaging/). 

This is akin to the practice of keeping architectural decision records (https://adr.github.io/). The goal is not to justify that the current decision to make Application the default is the right one--people will always debate such a decision. The goal is to give users context about how the decision was made _at the time_, which empowers users to better evaluate the alternatives given their own situations and future evolutions in the Python community.

Thanks again for considering this topic. And many thanks to @MikeHart85 for his thoughtful and intelligent proposal.

---

_Comment by @zanieb on 2025-03-07 17:47_

>  This page could be a good place to discuss why the uv team chose Application as the default and what the implications are (e.g., perhaps referencing https://hynek.me/articles/testing-packaging/).

I think this is summarized in https://github.com/astral-sh/uv/issues/3957#issuecomment-2657825266 — we don't want this to be the default longterm.

We intend to revisit this entire interface once our build backend is ready.

---

_Comment by @carlosferreyra on 2025-03-19 21:34_

Thank you for pointing this proposal!
I've been following this thread, but I'm a noob in the open source community, so a little shy to give any opinion.

# Proposal

following with @zanieb ideas and the philosophy of uv, for becoming the cargo of python (or npm/bunof python):
Having uv as a tool for scaffolding an initial template is a must.

What it goes in my mind, is that you can set a basic "__PEP compliant version of project initialization__" for `uv init` (specially when it comes matching [pyproject specs.](https://packaging.python.org/en/latest/specifications/), and use `uvx` for anything that has to do with custom templates (including those that work with [cookiecutter](https://github.com/cookiecutter/cookiecutter) or [copier](https://github.com/copier-org/copier). This follows 3 goals:

- keeps a clean workflow for the uv client, knowing that creating a project is a *one time action* during the lifetime of the project itself.
- Maintain strong compliance with PEP without sacrificing out-of-the-box features from uv.
- Make a equivalent use of what it was`npx create-react-app`|`npx create-expo-app` and would be `uvx create-{some-python-template}`

Also, you could use the proposal as a way to migrate from current out the box options out there in the python community, to a more *uv compliant* version of it. Benefits:

- Decrease of python community friction, when migrating to the uv ecosystem.
- Leave `uvx` as an entrypoint for extended scaffolding models (i.e, even one for cloud-based or AI projects), and even using the community popular choices as a possible builtin-template for uv.

An example of this was the `uvx migrate-to-uv` [script](https://github.com/mkniewallner/migrate-to-uv) which allows to migrate from poetry to uv, but this very concept could also be applied for scaffolding templates from scratch.

## Possible Drawbacks

- Dealing with a new set of standards when designing a new template through `uvx` and have the community to adhere to those standards
- Increased complexity in maintaining two separate but related commands (`uv init` and `uvx`).
- Potential confusion for new users about when to use `uv init` versus `uvx`.
- The need for significant effort to define and maintain the "PEP compliant" template for *uv init*, ensuring it remains up-to-date with evolving Python standards.
- Reliance on the community to create and maintain high-quality templates for `uvx`. If the community uptake is slow, the uvx ecosystem might lack useful options.
- Overlap in functionality with existing tools like `cookiecutter` or `copier` might lead to fragmentation or resistance from users already comfortable with those tools.

### Basic Structure

The `uv init` purpose should be to set up the least amount of settings for the project, since even when you need scaffolding, you also need to keep the option for a full customization from scratch. having said that, I agree with the initial proposal of the issue, but with a small caveat: __all templates, no matter if flat,lib, or package__ should have the `/src`  folder, since its a good widely accepted standard for the dev community, and also separates the project configuration from any project implementation *(even for frontend devs)*.

#### Easter Egg

the team could even create a dedicated package for using the `uvx` client, to build templates (`uvx start-project --with cookiecutter|copier|[uv]`)... and those templates can be selected by the uv team to comply with the uv ecosystem.


---

_Comment by @tmct on 2025-04-10 11:10_

uv is a great tool for managing virtual environments, even outside of packages, so I'm wondering: should there be a template for e.g. `--venv`, which just gives you all you need to start managing a virtual environment?

```
.git
.gitignore
pyproject.toml  # (with no [build-system])
.python-version
```

Currently, `uv init --no-readme --no-package` gets you close to this but creates an undesired "main.py", and `uv init --bare` is good but misses .python-version, and the git files. So - potentially a `--no-main`  option, or allowing `--vcs git` etc to work with `--bare` would achieve this - but perhaps this use-case is worthy of its own template?

---

_Comment by @Spenhouet on 2025-08-04 06:43_

We started using a monorepo structure and we'd still like to have "apps" which do not need a build system but now we have many and the flat layout used by `--app` leads to import conflicts. We'd like to have apps with a src directory, but I could not find an option to initialize this via uv.

It feels like `uv init` is trying to hard to think for the user and does a lot of presets/magic. Adding a `--layout` option to give the user the flexibility to choose the project structure themself would be great.
In addition a `--build-backend none` option could also help to disable adding a build-backend.

---

_Referenced in [astral-sh/uv#13466](../../astral-sh/uv/issues/13466.md) on 2025-08-15 15:10_

---

_Comment by @heyitsaamir on 2025-11-20 18:06_

+1 on the whole discussion here. It'd be great if uv could support templates out of the box especially. We use cookiecutter in our project (an SDK), but telling our customers to use cookiecutter for starter templates seems ugly.

Another idea is, if uv doesn't want to support this OOB, if there was a plugin system which would allow hooks like cookiecutter etc to be developed as plugins, then we could ask folks that they need to install uv with cookiecutter plugin, and then those systems could work without being included officially in uv core.

EDIT: I see that https://github.com/astral-sh/uv/issues/8178#issuecomment-2738176730 this comment has a great suggestion using uvx!

---
