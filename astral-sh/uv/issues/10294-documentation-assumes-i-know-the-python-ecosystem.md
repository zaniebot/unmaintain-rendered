---
number: 10294
title: Documentation assumes I know the Python ecosystem
type: issue
state: open
author: tringenbach
labels:
  - documentation
assignees: []
created_at: 2025-01-03T20:48:07Z
updated_at: 2025-01-03T21:52:03Z
url: https://github.com/astral-sh/uv/issues/10294
synced_at: 2026-01-10T01:24:52Z
---

# Documentation assumes I know the Python ecosystem

---

_Issue opened by @tringenbach on 2025-01-03 20:48_

My more recent background is Javascript, Typescript, and Java, where I mainly use `yarn` as a package manager.  I've touched Python a little over the years, but only recently have really focused on it and needed to care about tools like `uv`.

uv is designed to replace a bunch of existing tools, so it's not surprising that the documentation leans towards assuming I'm familiar with these existing tools. But for the most part, I am not.

I'll try to document in this issue some points that I found confusing, in hopes it can be clarified for everyone.

Just to be clear, when I compare things to other ecosystems, I am NOT saying one ecosystem is better than another. I am only trying to rely how I, with a background in that ecosystem, implicitly expect things to work. The hope is that this information can be used to write better documentation, not to suggest changes to how uv works.

### build system
Build systems in python are not something I've explored yet, so most of the references to them are confusing to me. It sounds like uv wants to eventually also be or include its own build system? Perhaps that will make things less confusing.

https://docs.astral.sh/uv/concepts/projects/config/#build-systems
https://docs.astral.sh/uv/concepts/projects/build/

What does `uv build` actually do? Or put another way, what are uv's responsibilities and what are the build system's responsibilities? 

Here https://docs.astral.sh/uv/guides/publish/#preparing-your-project-for-packaging it says uv won't build without a `[build-system]` but elsewhere it says it defaults to setup-tools for legacy reasons.

In Javascript-land, you might have a build system with `tsc`, `webpack`, `vite`, and maybe some in-repo custom scripting. That is mostly outside of `npm` / `yarn` though. You might invoke it with a `package.json` `scripts` entry, and might configure `npm` to include `dist/` when you publish, but that's about that. You wouldn't normally "install" after "building". (Usually "install" refer to what `uv` calls "sync".) And `dist/` is usually used only for publishing. So I know that `yarn build` just runs whatever I put in `"scripts": { "build": """ }`. For `uv`, it sounds like it does more, but I'm not sure what.


### `[project.scripts]` and "scripts"

- "scripts" https://docs.astral.sh/uv/guides/scripts/ 
- https://docs.astral.sh/uv/concepts/projects/config/#entry-points

This might be my `npm` ecosystem background confusing me. In that world, in `package.json`, there is a `scripts` key. 

I was looking for a way to define a package.json style "script". For example, maybe I want to be able to write `uv run test` and have it run `pytest`, or maybe `ruff` and `pytest`, or maybe I want a separate `uv run lint` for `ruff`. Maybe that's just not something python folks do?

`[project.scripts]` is what I originally thought I wanted, but I didn't have a build system so that was off limits to me. But I am starting to realize that project.script is actually closer to [bin](https://docs.npmjs.com/cli/v11/configuring-npm/package-json#bin) in `package.json`, and so is not what I want for this. (I wouldn't want my `test` script example installed globally in `$PATH`!)

I've noticed that a lot of tools support being configured in `pyproject.toml`. It seems that instead of having a "scripts" that specifies what arguments or chain of commands to run, that in Python-land folks typically just do `pytest` or `uv run pytest`, and have it configured in `pyproject.toml` (or else its own config file).

### the `.venv` folder

Should I prefix all my commands with `uv`, or should I `source .venv/bin/activate`? I didn't really see the latter mentioned, but it occurred to me to do that at some point, so that's what I'm been doing.

Actually, I take that back, I see it's mentioned here: https://docs.astral.sh/uv/guides/projects/#running-commands

Still, I wonder if sourcing it is typical, or if I'm doing something weird, and it matters at all.

I was aware of python virtual environments coming into this. But for some reason I was thinking that people normally created only one or two virtual envs at the user level. I wasn't aware of the `.venv/` convention. I was thinking of virtual environments as a way to avoid stomping on `brew` or `apt`, or for specialized purposes. But it ends up being more like `node_modules/`. That actually makes a lot of sense.

Also, I originally assumed having a `.venv` folder in my project was a uv thing. But then I noticed VsCode picking it up, and some other references to it, so now I suspect it is a common, though not universal, convention. 

### misc

I was surprised reading through https://docs.astral.sh/uv/getting-started/features/ that "projects" is the third thing listed and not the first.

To me, managing the Python version is just a particularly special dependency of my project. (And it's awesome that uv can manage it! In node-land, `nvm` is its own thing, and not integrated into npm/yarn, and much time is wasted as a result of folks running the wrong node version)

The emphasis on "scripts" I don't entirely get either. I guess it must be common in Python-land to have many scripts that depend on a different set of dependencies than the rest of your project? 

I find that confusing, because the docs don't mention why that is important, or when I would use it, or what the community practice is.

---

_Assigned to @zanieb by @zanieb on 2025-01-03 21:37_

---

_Comment by @zanieb on 2025-01-03 21:51_

Thanks for the comprehensive feedback! I wrote the docs — I'll see what I can do to address these. The newcomer perspective is really valuable.

You may find this third-party overview helpful: https://reinforcedknowledge.com/a-comprehensive-guide-to-python-project-management-and-packaging-concepts-illustrated-with-uv-part-i/

> Should I prefix all my commands with uv, or should I source .venv/bin/activate? I didn't really see the latter mentioned, but it occurred to me to do that at some point, so that's what I'm been doing.

We generally try to avoid recommend activating environments, since you can easily leave the current project with the environment still activated. I'd use something like `direnv` to automatically activate the environment if you want that.

See:

- #1910 

> I was looking for a way to define a package.json style "script". For example, maybe I want to be able to write uv run test and have it run pytest, or maybe ruff and pytest, or maybe I want a separate uv run lint for ruff. Maybe that's just not something python folks do?

See:

- #5903 

> What does uv build actually do? Or put another way, what are uv's responsibilities and what are the build system's responsibilities?

`uv build` is a build _frontend_. It invokes the build _backend_ as defined by your project. The build system is primarily for creating distributable artifacts like source distributions and binary distributions (wheels). Unfortunately, a build system is also necessary to install the _project itself_ into the environment. 

See:

- [PEP 517 – A build-system independent format for source trees](https://peps.python.org/pep-0517/)
- #3957 

> Here https://docs.astral.sh/uv/guides/publish/#preparing-your-project-for-packaging it says uv won't build without a [build-system] but elsewhere it says it defaults to setup-tools for legacy reasons.

The nuance here in particular is that we are _required_ to use setuptools to build dependencies if they do not declare a build system (this is part of PEP 517). However, for the project itself we skirt the specification and do not do this by default because the experience is bad. 

> I was surprised reading through https://docs.astral.sh/uv/getting-started/features/ that "projects" is the third thing listed and not the first.
>
> ...
> 
> The emphasis on "scripts" I don't entirely get either. I guess it must be common in Python-land to have many scripts that depend on a different set of dependencies than the rest of your project?

Python beginners often have a progression like...

1. Write code interactively in a `python` REPL
2. Write a script
3. Write a _project_ with modules
4. Distribute a project

The guides are roughly organized to get you going in that order, from simple to advanced use-cases.

Even as an advanced Python user, I do a lot of (1) and (2)



---

_Label `documentation` added by @zanieb on 2025-01-03 21:52_

---
