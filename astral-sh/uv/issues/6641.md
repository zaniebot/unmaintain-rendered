```yaml
number: 6641
title: Cannot solve an environment with platform_system == Emscripten on Linux/OSX
type: issue
state: closed
author: maartenbreddels
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-08-26T10:28:52Z
updated_at: 2024-08-26T23:58:18Z
url: https://github.com/astral-sh/uv/issues/6641
synced_at: 2026-01-10T04:45:09Z
```

# Cannot solve an environment with platform_system == Emscripten on Linux/OSX

---

_Issue opened by @maartenbreddels on 2024-08-26 10:28_

Given https://github.com/posit-dev/py-shiny/blob/77a679271ee94a64af597ad1159f6ab6bdeb1b51/pyproject.toml
The following requirements file should be solvable for `"platform_system == 'Emscripten'"`
```
shiny>=1.0
watchfiles < 0.18
```

Since one of its dependencies is:
```
"watchfiles>=0.18.0;platform_system!='Emscripten'",
```

$ uv pip compile requirements-conflict.txt correctly gives:
```
uv pip compile requirements-conflict.txt            ✘ 2 feat_improve_resolver ✭ ✱ ◼
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of watchfiles{platform_system != 'Emscripten'} are available:
          watchfiles{platform_system != 'Emscripten'}<=0.18.0
          watchfiles{platform_system != 'Emscripten'}==0.18.1
          watchfiles{platform_system != 'Emscripten'}==0.19.0
          watchfiles{platform_system != 'Emscripten'}==0.20.0
          watchfiles{platform_system != 'Emscripten'}==0.21.0
          watchfiles{platform_system != 'Emscripten'}==0.22.0
          watchfiles{platform_system != 'Emscripten'}==0.23.0
      and shiny==1.0.0 depends on watchfiles{platform_system != 'Emscripten'}>=0.18.0, we can conclude that
      shiny==1.0.0 depends on watchfiles>=0.18.0.
      And because only shiny<=1.0.0 is available, we can conclude that shiny>=1.0.0 depends on
      watchfiles>=0.18.0.
      And because you require shiny>=1.0 and watchfiles<0.18, we can conclude that your requirements are
      unsatisfiable.
```

On Linux or OSX.


The seemingly only way to do 'cross-solving' would be to use the `--python-platform` flag, however, this is limited only to a particular set of values.

If it was possible to override the values for the environment marker (and ENV_VAR would suffice) it would allow cross solving for non-supported platforms. 

My questions are:
 * Can I do what I want with the current version (0.3.3) of uv?
 * If not, is there a plan for, or interest in, allowing users to tweak/change the environment marker values, like `platform_system`?

---

_Comment by @charliermarsh on 2024-08-26 12:19_

This is doable in `uv lock`, but not `uv pip compile` right now. With `uv lock`, you can hint uv to explicitly solve for certain environments, like:

```toml
[tool.uv]
environments = ["platform_system == 'Emscripten'", "platform_system != 'Emscripten'"]
```

There's a hack you can use right now to unblock, but it's a little ridiculous. If you just repeat the dependency, it _should_ work -- as in, just put `"watchfiles>=0.18.0;platform_system!='Emscripten'"` in there twice. That's because the resolver "forks" if it identifies multiple declared dependencies for a given package, and within each "fork", we correctly respect the platform markers. It's something we're working on improving.


---

_Label `bug` added by @charliermarsh on 2024-08-26 12:20_

---

_Label `resolver` added by @charliermarsh on 2024-08-26 12:20_

---

_Comment by @charliermarsh on 2024-08-26 12:20_

(It's a known issue but I'll mark it as a bug and keep it open.)

---

_Comment by @maartenbreddels on 2024-08-26 14:02_

Thanks for the quick reply.

*(Note that I am not per-se interested in solving this environment, it's a more general issue with solving emscripten environment at https://py.cafe)*

I've played around a bit, and here is what I found:
I started with just the shiny dependency
```
[project]
name = "conflict"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "shiny>=1.0.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
environments = ["platform_system == 'Emscripten'", "platform_system != 'Emscripten'"]
```

Adding the watchfiles dependency two different ways fails:
```
$ uv add "watchfiles<0.18" 
Ignoring existing lockfile due to change in supported environments
  × No solution found when resolving dependencies for split (platform_system != 'Emscripten'):
  ╰─▶ Because only the following versions of watchfiles{platform_system != 'Emscripten'} are available:
          watchfiles{platform_system != 'Emscripten'}<=0.18.0
          watchfiles{platform_system != 'Emscripten'}==0.18.1
          watchfiles{platform_system != 'Emscripten'}==0.19.0
          watchfiles{platform_system != 'Emscripten'}==0.20.0
          watchfiles{platform_system != 'Emscripten'}==0.21.0
          watchfiles{platform_system != 'Emscripten'}==0.22.0
          watchfiles{platform_system != 'Emscripten'}==0.23.0
      and shiny==1.0.0 depends on watchfiles{platform_system != 'Emscripten'}>=0.18.0, we can conclude that
      shiny==1.0.0 depends on watchfiles>=0.18.0.
      And because only shiny<=1.0.0 is available, we can conclude that shiny>=1.0.0 depends on
      watchfiles>=0.18.0.
      And because your project depends on shiny>=1.0.0 and watchfiles<0.18, we can conclude that your
      project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```

Same here:
```
$ uv add "watchfiles<0.18;platform_system!='Emscripten'"
  × No solution found when resolving dependencies for split (platform_system != 'Emscripten'):
  ╰─▶ Because only shiny<=1.0.0 is available and shiny==1.0.0 depends on watchfiles{platform_system !=
      'Emscripten'}>=0.18.0, we can conclude that shiny>=1.0.0 depends on watchfiles{platform_system !=
      'Emscripten'}>=0.18.0.
      And because your project depends on shiny>=1.0.0 and watchfiles{platform_system != 'Emscripten'}<0.18,
      we can conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```

No problem for us, because that is now how we will be using uv, but  that might be information that relevant for a bugfix/issue in `uv add`.

The way we'll probably use this, is to write out all dependencies in one go: 

```
[project]
name = "conflict"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "shiny>=1.0.0",
    "watchfiles<0.18"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
environments = ["platform_system == 'Emscripten'"]
```
*(Note that I only have 1 environment, which seems to work)*

On this will, we can run `uv lock`, successfully 🎉 .

However, if we do a requirement.in -> pyproject.toml -> uv.lock, we are left with a non-standard file (ideally, it's a requirements file format). Is there an easy to transform/parse the uv.lock file so we can pull out the data?

Or do you see the support for this also coming to `uv pip compile`?

---

_Comment by @charliermarsh on 2024-08-26 14:09_

Oh sorry, you _only_ want to solve for Emscripten. Then yes, I think what you're doing is correct. The requirements you describe aren't actually solvable on other platforms, IIUC, since any other platform needs `watchfiles < 0.18` and `watchfiles >= 0.18`.

---

_Comment by @charliermarsh on 2024-08-26 14:09_

> However, if we do a requirement.in -> pyproject.toml -> uv.lock, we are left with a non-standard file (ideally, it's a requirements file format). Is there an easy to transform/parse the uv.lock file so we can pull out the data?

We're considering enabling export to `requirements.txt`. But it would also be reasonable for us to respect the `environment` settings in `uv pip compile` -- we probably should.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-26 14:28_

---

_Comment by @maartenbreddels on 2024-08-26 14:55_

I think that with both, it would be a holy grail of cross-solving environments.

PS: Having the lock file in JSON or YAML would also work for us since it would require more advanced post-processing if/when needed.

---

_Comment by @charliermarsh on 2024-08-26 16:21_

The lockfile is valid TOML so you can post-process it yourself if you want :) Though agree we should support the above.

---

_Comment by @maartenbreddels on 2024-08-26 17:28_

Ah, I actually read the documentation on this part, but for some reason missed that it was TOML, so ignore the JSON/YAML request. I think the part in the documentation "There is no Python standard for lockfiles at this time, so the format of this file is specific to uv and not usable by other tools." reset my expectations that it was possible.

But this is great news, that probably means we can do everything we want. Excellent tool, thanks :)

---

_Closed by @charliermarsh on 2024-08-26 23:58_

---

_Closed by @charliermarsh on 2024-08-26 23:58_

---
