```yaml
number: 11153
title: "How to associate optional `override-dependencies` with a specific `optional-dependencies`"
type: issue
state: open
author: tlambert03
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-02-01T17:39:33Z
updated_at: 2025-09-21T15:13:06Z
url: https://github.com/astral-sh/uv/issues/11153
synced_at: 2026-01-10T03:23:53Z
```

# How to associate optional `override-dependencies` with a specific `optional-dependencies`

---

_Issue opened by @tlambert03 on 2025-02-01 17:39_

### Question

I'm aware that I can use `override-dependencies` to essentially block a transitive dependency.  For example,
if I depend on `'direct-dependency'` which depends on `'preferred-backend'`, even though it
has been tested on and supports an `'alternate-backend'` that I would like to be using

```toml
[project]
dependencies = [
    "direct-dependency",  # this package declares a dependency on "preferred-backend"
    "alternate-backend"
]

[tool.uv]
override-dependencies = ["preferred-backend ;  sys_platform == 'never'"]
```

**my question:**

can I override `preferred-backend` *only* when one of my `optional-dependencies` is included:

```toml
[project]
dependencies = [
    "direct-dependency"  # this package declares a dependency on "preferred-backend"
]

[project.optional-dependencies]
variant = ["alternate-backend"]  # my extra

[tool.uv]
# how to override only when `--extra variant` is expressed
override-dependencies = ["preferred-backend ;  sys_platform == 'never'"]
```

```sh
uv sync --extra variant  # -> uninstall preferred-backend  and install alternate-backend 
```

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @tlambert03 on 2025-02-01 17:39_

---

_Comment by @zanieb on 2025-02-01 17:53_

I was hoping something like this would work

```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["anyio"]

[project.optional-dependencies]
foo = []

[tool.uv]
override-dependencies = ["sniffio ; (extra == 'foo' and sys_platform == 'never') or extra != 'foo'"]
```

but I don't think we respect the `extra == ...` as it's normalized out in the lockfile

```toml
[manifest]
overrides = [{ name = "sniffio", marker = "sys_platform == 'never' or extra != 'foo'" }]

[[package]]
name = "anyio"
version = "4.8.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "idna" },
    { name = "sniffio", marker = "sys_platform == 'never'" },
    { name = "typing-extensions", marker = "python_full_version < '3.13'" },
]
```

---

_Comment by @tlambert03 on 2025-05-05 14:48_

hey @zanieb, gentle ping: have any new clever approaches for allowing either an extra or a dependency group to override a dependency come up in the last few months?

---

_Comment by @alexeldeib on 2025-06-28 19:34_

I also find myself needing this.

I have a set of common dependencies, and some extras. jax is one of the common dependencies with different extras. other direct dependencies pull jax as a transitive dependency. to make matters worse, I have to build some of the extra/jax wheels myself, while some are directly available on pypi.

I need a version of my project that works with jax across all these variations, consumable by other downstream dependencies. I've been trying to split my core deps into multiple extras to deliver this.

I've tried combining `override-dependencies` with `never`, `optional-dependencies`, and `conflicts` in various forms to no avail. my issue is the common deps create a transitive dependency on the wrong wheel (pypi vs private, same package name, different index) for one of the extras, even when the extra specifies the index directly. I can use `override-dependencies` with `never` or e.g. `uv export --no-emit` to solve that, but then I still need to figure out another way for the extra to install the package. And similarly to your comment https://github.com/astral-sh/uv/issues/11153#issuecomment-2629046693 I can't use the extra or index markers in the override-dependencies block to fix it.

The best solution I have found is skip emitting those libraries, uv export into an environment where I manually pip installed the package, and then pip install the export on top of that.

Ideally I could use optional depedencies + per-extra override + per-extra sources to say, "for this extra, this package should come from this index, and override the transitive dependency to use this version as well". I can't seem to find a way to do that and this issue seems like it would do the trick! 

---

_Label `question` removed by @zanieb on 2025-06-28 19:37_

---

_Label `enhancement` added by @zanieb on 2025-06-28 19:37_

---

_Label `needs-design` added by @zanieb on 2025-06-28 19:37_

---

_Comment by @zanieb on 2025-06-28 19:37_

Thanks for sharing another use-case!

This sounds challenging to implement, so we'll need to take a deeper look here but I'm not sure when we'll have the resources to do so.

---

_Comment by @ARKAD97 on 2025-09-21 15:10_

One more use-case.

[axolotl](https://github.com/axolotl-ai-cloud/axolotl) dependencies vary based on the version of torch that is installed in the project environment. Solution proposed in the [documentation](https://docs.astral.sh/uv/concepts/projects/config/#dynamic-metadata) is to specify `extra-build-dependencies`:

```toml
[tool.uv.extra-build-dependencies]
axolotl = ["torch==2.7.1"]
```

However, it doesn't work when you want to support multiple CUDA /torch versions via optional-dependencies as `extra-build-dependencies` don't respect them too. The only axolotl dependencies of axolotl that vary are `xformers` and `vllm`. So I could just override them for each CUDA/torch version if only `override-dependencies` respected optional dependencies.

One solution is to provide metadata for axolotl upfront as proposed in the [documentation](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata). But I'll have to list all dependencies and it's quite messy.

---

_Comment by @zanieb on 2025-09-21 15:12_

I'm not sure I understand the details there, but it sounds like you want `match-runtime = true` instead of pinning the torch version? See https://docs.astral.sh/uv/concepts/projects/config/#augmenting-build-dependencies

---
