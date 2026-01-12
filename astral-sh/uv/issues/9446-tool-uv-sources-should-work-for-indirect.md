```yaml
number: 9446
title: "`tool.uv.sources` should work for indirect dependencies"
type: issue
state: open
author: zeevro
labels:
  - needs-decision
assignees: []
created_at: 2024-11-26T17:35:25Z
updated_at: 2025-03-06T14:01:38Z
url: https://github.com/astral-sh/uv/issues/9446
synced_at: 2026-01-12T15:59:50Z
```

# `tool.uv.sources` should work for indirect dependencies

---

_@zeevro_

Currently (version 0.5.4), `uv` ignores `tool.uv.sources` if a package is not a direct dependency (i.e. listed in the project's `pyproject.toml`). I think `tool.uv.sources` should apply for all dependencies.

---

_Comment by @charliermarsh on 2024-11-26 17:41_

If you're going to define a source, you should just make it a direct dependency -- what's the issue with that? You're already encoding that your project depends on a certain package.

---

_Comment by @zeevro on 2024-11-26 18:16_

If I'm using a package from a private index, and that package has further dependencies on said private index, I don't want to have to add all of these indirect dependencies to as direct dependencies. It is a bit weird, though, since my private direct dependencies need packages from PyPI apart from the private ones, so I guess how to deal with that is also a question.

---

_Comment by @zanieb on 2024-11-26 18:20_

Sort of related https://github.com/astral-sh/uv/issues/8148#issuecomment-2499547638

---

_Comment by @konstin on 2024-11-27 13:06_

Does it work if you make the private index first priority and pypi the second priority?

For applying extra information to indirect dependencies, you can use [constraints](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies), though you may need https://github.com/astral-sh/uv/pull/9455 for that first.

---

_Comment by @smackesey on 2024-12-22 14:57_

Strong agree with this.

I find it very unintuitive that `tool.uv.sources` does not work for indirect dependencies. It's also not mentioned in the [docs](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources).

The main problem is that it forces you to declare second-order-plus dependencies as `dependencies` if you want to use `tool.uv.sources` for them. This is a non-standard use of `dependencies`, which typically only contains packages actually imported by your code. The situation where it is convenient to use `sources` for indirect dependencies can easily come up in monorepo development, where you have many interdependent packages.

### More details

I work in the context of a large monorepo, [Dagster](https://github.com/dagster-io/dagster). We have ~100 python packages defined in the repo with various interdependencies.

If I am setting up a new package that pulls in some of our integration libraries, it might look like this:

```
[project]
name = "my_example"
requires-python = ">=3.9,<3.13"
version = "0.1.0"
dependencies = [
    "dagster",
    "dagster-components[dbt]",
]
```

But remember that there are many interdependencies among packages. `dagster` has a dependency on another library named `dagster-pipes` and `dagster-components[dbt]` has a dependency on `dagster-dbt`.

If a user were setting up `my_example`, they wouldn't need to specify these packages in dependencies, since they are 2nd-order dependencies that would be pulled in from PyPI like any other.

But in many development contexts, we want to make sure our packages resolve internal dependencies (i.e. dependencies on other dagster packages) against the versions from the same monorepo commit. So we will tack a big list of uv sources onto `pyproject.toml` like this below. Basically this acts as a pseudo-index-- the purpose is to make sure that _any_ internal package is resolved against the local version of the monorepo instead of downloaded from PyPI.

```
[tool.uv.sources]
dagster-test = { path = "/Users/smackesey/stm/code/elementl/oss/python_modules/dagster-test", editable = true }
dagster-graphql = { path = "/Users/smackesey/stm/code/elementl/oss/python_modules/dagster-graphql", editable = true }
dagster = { path = "/Users/smackesey/stm/code/elementl/oss/python_modules/dagster", editable = true }
dagster-pipes = { path = "/Users/smackesey/stm/code/elementl/oss/python_modules/dagster-pipes", editable = true }
dagster-webserver = { path = "/Users/smackesey/stm/code/elementl/oss/python_modules/dagster-webserver", editable = true }
...
```

But, because `tool.uv.sources` does not work for indirect dependencies, we have to remember to specify any nth-order internal dependency as a direct dependency:

```
[project]
name = "my_example"
requires-python = ">=3.9,<3.13"
version = "0.1.0"
dependencies = [
    "dagster",
    "dagster-components[dbt]",
    "dagster-dbt",  # have to add this...
    "dagster-pipes",  # have to add this...
]
```

So now in a dev context we have to chase down 2nd-order-plus deps and have a different package metadata than a user would have because of details of package resolution tooling.

---

_Label `needs-decision` added by @zanieb on 2025-01-07 19:57_

---

_Comment by @zmeir on 2025-02-10 20:51_

Just adding my 2 cents here. I often have the following use-case:  
Package `c` depends on package `b`, which in turn depends on package `a`, but package `c` does not directly depend on `a`. Then, I want to implement something in `a` and see how it affects `c`. Being able to set `a` as an editable local path for `c` via `[tool.uv.sources]` would simplify this flow, especially since I already do the same when implementing something in `b` and testing how it affects `c`.

---

_Comment by @zmeir on 2025-03-06 14:01_

If this helps anyone, I found out that in most cases I can use [dependency override](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides) for indirect dependencies.

One downside with this approach is that I have to specify package extras for the indirect dependency.

For example, let's say my project depends on the package `direct`, and `direct` depends on `indirect[extra]`, then this should work:
```toml
[project]
name = "test-uv-override"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "direct",
]

[tool.uv]
override-dependencies = [
    "indirect[extra]@file:///path/to/indirect",
]
```

Of course this will not install `indirect` as an editable package, but I use this mostly for CI so it's less of an issue for me.

---
