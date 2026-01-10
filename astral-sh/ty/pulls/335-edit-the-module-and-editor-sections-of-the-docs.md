```yaml
number: 335
title: Edit the module and editor sections of the docs
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-edit
created_at: 2025-05-12T21:21:33Z
updated_at: 2025-05-12T22:40:05Z
url: https://github.com/astral-sh/ty/pull/335
synced_at: 2026-01-10T02:34:10Z
```

# Edit the module and editor sections of the docs

---

_Pull request opened by @zanieb on 2025-05-12 21:21_

_No description provided._

---

_Label `documentation` added by @zanieb on 2025-05-12 21:21_

---

_Review comment by @AlexWaygood on `docs/README.md`:59 on 2025-05-12 21:23_

```suggestion
[`src.root`](./reference/configuration.md#root) in your `pyproject.toml` or `ty.toml`. For example, if your
```

---

_Review comment by @AlexWaygood on `docs/README.md`:60 on 2025-05-12 21:23_

```suggestion
project's code is in an `app/` directory:
```

---

_Review comment by @AlexWaygood on `docs/README.md`:82 on 2025-05-12 21:26_

```suggestion
These are usually declared as project dependencies in a `pyproject.toml` file or similar,
and are installed using a package manager like uv or pip. Examples of popular third-party
libraries are `requests`, `numpy` and `django`.
```

---

_Review comment by @AlexWaygood on `docs/README.md`:97 on 2025-05-12 21:30_

I find the order in which the information is presented here a little confusing because you mention `--python` after `VIRTUAL_ENV` and `.venv`, but the first thing ty checks is whether it was set explicitly using `--python` or the `environment.python` config option. I think it would be easier to follow if we listed the fallbacks in the order ty considers them: first detail the explicit ways of configuring this, then detail the fallbacks it uses.

I think it's also important to state that the `VIRTUAL_ENV` variable will be implicitly set:
- if you're using `uv run` or an equivalent from pdm/poetry/hatch/etc.
- if you explicitly activate the virtual environment in your shell

---

_Review comment by @AlexWaygood on `docs/README.md`:104 on 2025-05-12 21:30_

```suggestion
For example, Python 3.10 introduced support for `match` statements and added the
```

---

_Review comment by @AlexWaygood on `docs/README.md`:106 on 2025-05-12 21:31_

````suggestion
`sys.stdlib_module_names` symbol to the standard library. However, if your project also supports
Python 3.9, you cannot use these new features unless they are inside an `if sys.version_info >= (3, 10)` branch:

```python
import sys

if sys.version_info >= (3, 10):
    print(sys.stdlib_module_names)  # okay whatever your python-version is set to, because of the `sys.version_info` condition
else:
    print(sys.stdlib_module_names)  # ty will emit an error here if the configured `python-version` is <3.10
```
````

---

_Review comment by @AlexWaygood on `docs/README.md`:138 on 2025-05-12 21:35_

it's a bit weird to me that we mention these last, since they're the things that ty checks for first

---

_Review comment by @AlexWaygood on `docs/README.md`:133 on 2025-05-12 21:36_

```suggestion
We plan on adding dedicated options for including and excluding files in future releases.
```

---

_@AlexWaygood approved on 2025-05-12 21:36_

this is excellent, thanks

---

_@zanieb reviewed on 2025-05-12 21:44_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 21:44_

I think the existing structure of covering the default behavior (which is what most people will use) makes sense before discussing the fact that you can override that behavior? I am generally hesitant to cover the explicit options first here.

> I think it's also important to state that the VIRTUAL_ENV variable will be implicitly set:

This makes sense to me.

---

_@zanieb reviewed on 2025-05-12 21:46_

---

_Review comment by @zanieb on `docs/README.md`:82 on 2025-05-12 21:46_

I'm hesitant to go into more detail here, it seems like a bit of a distraction and "or similar" is pretty load bearing. What's they key point we're trying to establish here?

---

_Review comment by @AlexWaygood on `docs/README.md`:82 on 2025-05-12 21:48_

I wasn't really sure what "these are usually dependencies of your project" really meant in your current text. It feels almost tautological -- third-party modules are dependencies, yes, but what's the definition of a dependency? It's a third-party module.

---

_@AlexWaygood reviewed on 2025-05-12 21:48_

---

_@zanieb reviewed on 2025-05-12 21:51_

---

_Review comment by @zanieb on `docs/README.md`:82 on 2025-05-12 21:51_

Haha see, I think the point is to provide that sort of tautological "third-party modules is just a fancy word for dependencies" statement since we're explaining to a beginner what we mean by this concept. I expect people to know what a "dependency" is. I can play with this language though...

---

_Review comment by @AlexWaygood on `docs/README.md`:82 on 2025-05-12 21:54_

> I expect people to know what a "dependency" is

Ah, I do not!! I've seen lots of Python noobs who've been forced to type checkers by the company they're working for, and who know very little at all about Python packaging 😄

---

_@AlexWaygood reviewed on 2025-05-12 21:54_

---

_@zanieb reviewed on 2025-05-12 21:58_

---

_Review comment by @zanieb on `docs/README.md`:82 on 2025-05-12 21:58_

```suggestion
These are usually declared as dependencies in a `pyproject.toml` or `requirements.txt` file
and installed using a package manager like uv or pip. Examples of popular third-party
modules are `requests`, `numpy` and `django`.
```

---

_@AlexWaygood reviewed on 2025-05-12 21:58_

---

_Review comment by @AlexWaygood on `docs/README.md`:97 on 2025-05-12 21:58_

What about something like this:

```md
The Python environment ty should use can be explicitly specified, but ty can also automatically
infer your project's environment in many cases.

ty only supports _automatic_ discovery of virtual environments at this time.

When discovering virtual environments, ty first checks for an active environment using the `VIRTUAL_ENV`
environment variable. If not set, ty will search for a `.venv` folder in the project root or
working directory.

The Python environment may be explicitly specified using the
[`environment.python`](./reference/configuration.md#python) setting or
[`--python`](./reference/cli.md#ty-check--python) flag. When setting
the environment explicitly, non-virtual environments can be provided.
```

So, we still detail the automatic discovery before the explicit config options, but we make it slightly clearer upfront that explicit config is supported and non-virtual environments are supported.

---

_@zanieb reviewed on 2025-05-12 21:59_

---

_Review comment by @zanieb on `docs/README.md`:82 on 2025-05-12 21:59_

That's sort of not the target audience for _this_ documentation, but anyway I'm fine with changing it.

---

_@AlexWaygood reviewed on 2025-05-12 21:59_

---

_Review comment by @AlexWaygood on `docs/README.md`:82 on 2025-05-12 21:59_

that LGTM!

---

_@zanieb reviewed on 2025-05-12 22:00_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:00_

I don't love it :D 

---

_@zanieb reviewed on 2025-05-12 22:06_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:06_

I'm trying to rephrase it for you, but this pattern is used in each section haha

---

_@zanieb reviewed on 2025-05-12 22:07_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:07_

I think if we wanted them to be used by most users, I'd list them first, but the idea is that you don't need the setting.

---

_@zanieb reviewed on 2025-05-12 22:09_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:09_

In the same way that we don't lead with "here's how to use `src.root`"

---

_Review comment by @AlexWaygood on `docs/README.md`:97 on 2025-05-12 22:10_

yeah I get that... maybe something like this...?

```md
In many cases, ty can also automatically infer your project's environment.
ty only supports _automatic_ discovery of virtual environments at this time.

When discovering virtual environments, ty first checks for an active environment using the `VIRTUAL_ENV`
environment variable. If not set, ty will search for a `.venv` folder in the project root or
working directory.

The Python environment can also be explicitly specified using the
[`environment.python`](./reference/configuration.md#python) setting or
[`--python`](./reference/cli.md#ty-check--python) flag. If either of these
are set, they will take precedence over ty's automatic environment discovery.

When setting the environment explicitly, non-virtual environments can be provided.

---

_@AlexWaygood reviewed on 2025-05-12 22:10_

---

_@zanieb reviewed on 2025-05-12 22:12_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:12_

I pushed a tweak, maybe it's a bit better?

I'm trying to use the same language as the rest of the sections, e.g., "By default, ..." 

---

_@AlexWaygood reviewed on 2025-05-12 22:16_

---

_Review comment by @AlexWaygood on `docs/README.md`:97 on 2025-05-12 22:16_

Yeah, the new language looks great, thanks!

---

_Review comment by @AlexWaygood on `docs/README.md`:95 on 2025-05-12 22:17_

think it would still be great to mention that this is set by uv/poetry/hatch/pdm if they're running `uv run` or similar, and that it's set if you activate the environment explicitly in the shell

---

_Review comment by @AlexWaygood on `docs/README.md`:104 on 2025-05-12 22:18_

```suggestion
The Python environment may be explicitly configured using the
[`environment.python`](./reference/configuration.md#python) setting or
[`--python`](./reference/cli.md#ty-check--python) flag. This takes
precedence over the virtual environment discovery detailed above.
```

---

_@AlexWaygood approved on 2025-05-12 22:18_

---

_Review comment by @zanieb on `docs/README.md`:97 on 2025-05-12 22:19_

Thanks for your lenience :) haha

---

_@zanieb reviewed on 2025-05-12 22:19_

---

_@zanieb reviewed on 2025-05-12 22:19_

---

_Review comment by @zanieb on `docs/README.md`:95 on 2025-05-12 22:19_

Yeah working on that 👍 

---

_@zanieb reviewed on 2025-05-12 22:20_

---

_Review comment by @zanieb on `docs/README.md`:104 on 2025-05-12 22:20_

I don't want to add that :p explicit configuration _always_ takes place over defaults. That's the case for all of our options.

---

_@AlexWaygood reviewed on 2025-05-12 22:21_

---

_Review comment by @AlexWaygood on `docs/README.md`:104 on 2025-05-12 22:21_

eh, fair enough

---

_@zanieb reviewed on 2025-05-12 22:22_

---

_Review comment by @zanieb on `docs/README.md`:95 on 2025-05-12 22:22_

I do think this is a little awkward to add though. As before, it's a different target audience than the alpha documentation is for.

---

_@zanieb reviewed on 2025-05-12 22:23_

---

_Review comment by @zanieb on `docs/README.md`:95 on 2025-05-12 22:23_

i.e., https://github.com/astral-sh/ty/pull/335/commits/c97679219ab16ef725e3a0034f58e8b3a2996b71

---

_Review comment by @zanieb on `docs/README.md`:95 on 2025-05-12 22:23_

I think it sort of interrupts the flow more than it helps, but I don't feel strongly.

---

_@zanieb reviewed on 2025-05-12 22:23_

---

_@AlexWaygood reviewed on 2025-05-12 22:30_

---

_Review comment by @AlexWaygood on `docs/README.md`:95 on 2025-05-12 22:30_

I feel pretty strongly that this is important information -- it's part of the theme we're trying to describe here, which is "for most cases, you should (hopefully) not have to provide any configuration at all". If everybody starts setting `VIRTUAL_ENV` environment variables because they don't realise that uv is already going to set it for them (or, if they're using pip, because they don't realise it's already set when they activate the environment), that feels like a bad outcome? 😃

The note you added looks great; I'm happy with the text now!

---

_@zanieb reviewed on 2025-05-12 22:37_

---

_Review comment by @zanieb on `docs/README.md`:95 on 2025-05-12 22:37_

I agree it's important, but I think this is the wrong place for it. Those users need "guide"-style documentation that is explicitly targeting beginners (like we have in uv). This documentation _isn't_ for beginners. It's for people trying an experimental type checker. Catering to two audiences like that is just asking for confusing documentation. Anyway 🤷‍♀️ 

---

_Merged by @zanieb on 2025-05-12 22:38_

---

_Closed by @zanieb on 2025-05-12 22:38_

---

_Branch deleted on 2025-05-12 22:38_

---

_@AlexWaygood reviewed on 2025-05-12 22:40_

---

_Review comment by @AlexWaygood on `docs/README.md`:95 on 2025-05-12 22:40_

that's all fair!

---
