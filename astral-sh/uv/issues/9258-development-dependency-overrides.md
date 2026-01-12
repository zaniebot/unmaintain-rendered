```yaml
number: 9258
title: Development Dependency Overrides
type: issue
state: open
author: iloveitaly
labels:
  - question
assignees: []
created_at: 2024-11-20T00:10:49Z
updated_at: 2025-10-02T19:26:52Z
url: https://github.com/astral-sh/uv/issues/9258
synced_at: 2026-01-12T15:59:45Z
```

# Development Dependency Overrides

---

_@iloveitaly_

One of the things I've always missed from ruby is development dependency overrides. Here's the situation:

* You have a dependency in one of your projects that is published on pypi
* Something is broken in it. You clone it with hopes of fixing it locally before getting the patch merged upstream.
* You want to temporarily override the pypi reference to point to a path on your local filesystem _without_ modifying `pyproject.toml`

Ruby allows you to do this with:

```
bundle config set local.rack ~/Work/git/rack
```

It would be great if uv provided similar sort of functionality.


---

_Comment by @charliermarsh on 2024-11-20 13:31_

I believe this is possible today with:

```toml
[tool.uv]
override-dependencies = ["dependency @ file:///path/to/dependency"]
```

---

_Label `question` added by @charliermarsh on 2024-11-20 13:31_

---

_Comment by @ReinforcedKnowledge on 2024-11-22 02:24_

I think you can also run code with the `--with` option of `uv run` command, or `--with-requirements` if you have many. That won't modify the `pyproject.toml` and the code will only run in an ephemeral virtual environment. 

---

_Comment by @iloveitaly on 2024-11-22 15:57_

This is awesome. Will try this out and report back. Thanks for all of the help.

---

_Comment by @iloveitaly on 2024-11-25 22:52_

This does not work:

```
override-dependencies = ["activemodel @ file://./pypi/activemodel"]
```

And results in the following error:

```
relative path without a working directory: ./pypi/activemodel
```

But an absolute path does work. Am I missing some obvious configuration option here?

Also, it looks like the package is not installed as editable. What's the best way to do that? It looks like [this syntax](https://github.com/Zipstack/unstract/blob/78ae55ea51892b7afc868b831e1d88f895ae4e4d/pyproject.toml#L45) should work, but it doesn't:

---

_Comment by @ReinforcedKnowledge on 2024-11-26 00:23_

Hi!

I will let the experts say why it didn't work because I don't know.

Relative paths with `override-dependencies` aren't working for me either, but I generally don't expect relative paths to work in that fashion, they're always a hassle no matter the tool. If you want to avoid hardcoding the path you can use `${PROJECT_ROOT}`, like 
```toml
[tool.uv]
override-dependencies = ["activemodel @ file:///${PROJECT_ROOT}/pypi/activemodel"]
```

As for the editable install, there is no current way for specifying editable installs in an `override-dependencies`. But at the same time, I don't think that's what you need here.

Here's how you can use your local package as editable:
```
[tool.uv.sources]
activemodel = { path = "./pypi/activemodel", editable = true }
```

Sources in `uv` will always take precedence over version contraints.

As for why `override-dependencies` are not what you need, I think, from my modest beginner understanding of the tool, it's something that you'd use to override versions of transitive dependencies, or to force a particular version constraint in a way that no matter the current or the future resolutions of `uv` (due to a change in `uv` itself or in your dependencies), that package's version is going to stay the same.

I hope someone more knowledgeable can read this and correct it if I'm wrong.

---

_Comment by @zanieb on 2024-11-26 00:41_

There's discussion about this same problem over in https://github.com/astral-sh/uv/issues/8148#issuecomment-2496127365 that may provide some more context.

---

_Comment by @ReinforcedKnowledge on 2024-11-26 01:03_

Oh thanks! I'll read through the thread!

---

_Comment by @iloveitaly on 2024-11-26 01:13_

```
[tool.uv.sources]
activemodel = { path = "./pypi/activemodel", editable = true }
```

This works for local development, but doesn't solve my problem:

1. When developing locally, I want to use a local copy of the package
2. In production, I want to reference a production package
3. I don't want to have to keep a mutated copy of `pyproject.toml` locally to reference the local package

Ruby does this with:

```
bundle config set local.rack ~/Work/git/rack
```

Which does not involve mutating your Gemfile but enables you to easily debug a package with a local development override.

---

_Comment by @zanieb on 2024-11-26 01:17_

We want to build a dedicated "patch" workflow, but we haven't designed that yet. Does using `uv run --with-editable` not work for you?

---

_Comment by @charliermarsh on 2024-11-26 02:52_

You can run with `--no-sources` in production if you want to disable that local path.

---

_Comment by @iloveitaly on 2024-11-27 17:47_

I wasn't aware of `uv run --with-editable` or `--no-sources`, let me try those out and report back.

---

_Comment by @iloveitaly on 2024-11-28 15:45_

Can you specify either of these options with an ENV var?

I'm using [nixpacks](https://github.com/railwayapp/nixpacks) to build my containers and I'd rather not have to customize the install scripts and instead specify ENV configuration which uv can pick up.

---

_Comment by @rpmcginty on 2025-01-13 23:36_

> We want to build a dedicated "patch" workflow, but we haven't designed that yet. Does using `uv run --with-editable` not work for you?

@zanieb is there an issue tracking this, or is it too early?



---

_Comment by @zanieb on 2025-01-13 23:46_

It's sort of tracked in https://github.com/astral-sh/uv/issues/7454

I'm doing some roadmap work and will post here / there if I create a dedicated issue.

---

_Comment by @iloveitaly on 2025-01-15 18:54_

I've been iterating on a script which is working well for me:

https://github.com/iloveitaly/uv-development-toggle

---

_Comment by @Avasam on 2025-03-07 04:18_

I'd like to stay compatible with `pip` as much as possible, but the following doesn't solve the problem I outlined in https://github.com/astral-sh/uv/issues/9683#issuecomment-2705480073

```toml
[dependency-groups]
dev = [
    "ts_utils @ file:lib", # pip compatible
]

[tool.uv]
override-dependencies = [
  "ts_utils @ file:${PROJECT_ROOT}/lib", # uv won't accept relative paths
]
```
```
  × Failed to build `typeshed @ file:///E:/Users/Avasam/Documents/Git/typeshed`
  ├─▶ Failed to parse entry in group `dev`: `ts_utils @ file:lib`
  ╰─▶ relative path without a working directory: lib
      ts_utils @ file:lib
```

`uv 0.6.5 (bcbcd0a1e 2025-03-06)`

---

_Comment by @watsonjj on 2025-04-23 17:14_

> This works for local development, but doesn't solve my problem:
> 
> 1. When developing locally, I want to use a local copy of the package
> 2. In production, I want to reference a production package
> 3. I don't want to have to keep a mutated copy of `pyproject.toml` locally to reference the local package

I have a similar problem, and a partial solution - create a "local" and a "remote" group (with `[dependency-groups]`) or extra (with `[project.optional-dependencies]`). The corresponding source in `[tool.uv.sources]` can be marked with the group/extra it targets. The `conflicts` table is critical for this to work. The group/extra can then be selected during commands, e.g.:
```bash
$ uv sync --extra dev-local

Resolved 11 packages in 517ms
Uninstalled 1 package in 17ms
Installed 2 packages in 29ms
 + pygments==2.19.1
 - pytest==8.3.5
 + pytest==8.4.0.dev459+g33b26e429 (from file:///Users/Jason/Software/uv_issue/pytest)
```

```bash
$ uv sync --group dev-remote

Resolved 11 packages in 6ms
Uninstalled 1 package in 1ms
Installed 1 package in 9ms
 - pytest==8.4.0.dev459+g33b26e429 (from file:///Users/Jason/Software/uv_issue/pytest)
 + pytest==8.4.0.dev459+g33b26e429 (from git+https://github.com/pytest-dev/pytest@33b26e429817935af8eb0202aa1e5ef0476e1cad)
```

The problems with this solution are:
- The version specifier in the `dependencies` is overwritten. In some cases maybe this is desired...
- How to additionally specify a source for a default call (`uv sync`)? The `dev` group is active by default, which at first I thought could be utilised, but then a conflict occurs when trying to use `dev-local` or `dev-remote`.
- How to additionally specify a source for production? There is no way I know of to add this case to the conflict table.

Here is a simple pyproject.toml demonstration:

```toml
[project]
name = "foo"
version = "0.0.1"
requires-python = ">=3.9"
dependencies = ["pytest~=8.3"]

[dependency-groups]
dev-local = ["pytest"]
dev-remote = ["pytest"]

[project.optional-dependencies]
dev-local = ["pytest"]
dev-remote = ["pytest"]

[tool.uv]
conflicts = [
    [
        { extra = "dev-remote" },
        { extra = "dev-local" },
        { group = "dev-remote" },
        { group = "dev-local" },
    ]
]

[tool.uv.sources]
"pytest" = [
    {path = "../pytest", editable = true, extra = "dev-local"},
    {path = "../pytest", editable = true, group = "dev-local"},
    {git = "https://github.com/pytest-dev/pytest", branch = "main", extra = "dev-remote"},
    {git = "https://github.com/pytest-dev/pytest", branch = "main", group = "dev-remote"},
]
```

Some other issues with a similar topic:
#12539 #11941 #9537 #7945 #11632 #12681 #12013

---

_Comment by @lordmauve on 2025-06-30 09:21_

Going back to

> _without_ modifying pyproject.toml

the issue for me is that adding `sources` in `pyproject.toml` is a change that will be tracked by git, so it becomes easy to accidentally commit them. `uv.toml` could be `.gitignored` but if it exists it shadows anything in `pyproject.toml`. It would be cool if there was a third project-local config file like `uv.local.toml` that can be `.gitignored`.

---

_Comment by @iloveitaly on 2025-06-30 15:58_

@lordmauve yeah, totally agree. This the biggest issue with [my tool](https://github.com/iloveitaly/uv-development-toggle) is it requires you to clear out all local references before committing.

---

_Comment by @osamimi on 2025-09-20 06:15_

Perhaps I am missing something, but couldn't one just create a dev only 'meta project' that does an editable install of the cloned packages and use that as the dev environment?

Say we have project_A which depends on project_B. One could make clone both projects under ./meta_project and then create a ./meta_project/pyproject.toml with something like: 

```
[project]
name = "meta_project"
dependencies = [
    "projectA",
    "projectB"
]

[tool.uv.sources]
projectA = { path = "./projectA", editable = true }
projectB = { path = "./projectB", editable = true }
````

meta_project could even be a repo itself with projectA/B as git submodules (if you wanna get fancy). 

---

_Comment by @osamimi on 2025-09-20 18:33_

^ I just actually tried the above, and it seems to work but you also need to specify 'override-dependencies'. I'll play around with it to see if there are any gotchas. @zanieb do you see anything wrong with this setup?

Directory structure: 
```
├── meta_project
├── projectA 
├── projectB 
```

meta_project/pyproject.toml

```
[project]
name = "meta_project"
dependencies = [
    "projectA",
    "projectB",
]

[tool.uv.sources]
projectA = { path = "../projectA", editable = true }
projectB = { path = "../projectB", editable = true }

[tool.uv]
override-dependencies = [
  "projectA @ file:${PROJECT_ROOT}/../projectA", 
  "projectB @ file:${PROJECT_ROOT}/../projectB", 
]
```
projectA/pyproject.toml
```
[project]
name = "projectA"
dependencies = [
    "projectB",
]

[tool.uv.sources]
projectB = { git = "ssh://git@github.com/osamimi/projectB" , tag = "1.0.0"}  

```

projectB/pyproject.toml

```
[project]
name = "projectB"
```

---

_Comment by @osamimi on 2025-10-02 19:26_

Here is a suggestion to make this more straightforward : Allow the user to specify both a normal (pypi/git/...) and a path dependency and prioritize using the path dependency if it exists (e.g. there is a pyproject.toml in that path), and fall back to pypi/git/... if not. 

---
