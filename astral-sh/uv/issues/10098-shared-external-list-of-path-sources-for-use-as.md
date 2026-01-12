```yaml
number: 10098
title: "Shared external list of path sources for use as an \"index\" analogue"
type: issue
state: open
author: smackesey
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-12-22T16:55:55Z
updated_at: 2025-01-25T16:51:18Z
url: https://github.com/astral-sh/uv/issues/10098
synced_at: 2026-01-12T16:00:06Z
```

# Shared external list of path sources for use as an "index" analogue

---

_@smackesey_

I'd like the ability to provide a list of sources in a file outside the package `pyproject.toml` to `uv pip install`. The reason the sources should be in an external file is that these sources will contain paths for all the packages in a monorepo, and the same file will be used whenever a package is installed in a test environment to ensure dependencies within the monorepo are resolved within the checked-out commit of the monorepo.

### Details

In monorepo development, often one has many interdependent packages in the same repo. Let's call a dependency across packages within the monorepo an "internal dependency".

When testing, we'd typically like to resolve internal dependencies against the checked out version of the code.

Consider a toy version of the much more complicated scenario we have in [Dagster](https://github.com/dagster-io/dagster). Suppose we have three packages in the monorepo: `dagster-foo` `dagster-bar`, and `dagster-baz`. `dagster-foo` depends on `dagster-bar`, and `dagster-bar` depends on `dagster-baz`:

```
### dagster-foo/pyproject.toml

[project]
...
name = "dagster-foo"
dependencies = ["dagster-bar"]
```

```
### dagster-bar/pyproject.toml

[project]
...
name = "dagster-bar"
dependencies = ["dagster-baz"]
```

When I install `dagster-foo` into a test environment from a local path, I want to resolve both it's first order dep `dagster-bar` and second-order dep `dagster-baz` to the local versions within the monorepo.

One way to do this is to use `[tool.uv.sources]`. For that to work, we'd have to do this:

```
### dagster-foo/pyproject.toml

[project]
...
name = "dagster-foo",
dependencies = [
  "dagster-bar",
  "dagster-baz",  # have to include this now because tool.uv.sources won't work for 2nd-order deps
]

[tool.uv.sources]
dagster-bar = { path = "/path/to/dagster/python_modules/dagster-bar", editable = true }
dagster-baz = { path = "/path/to/dagster/python_modules/dagster-baz", editable = true }
```

This is not ideal for a few reasons:

- I have to list indirect dependencies under `project.dependencies` to make them work with `uv.tool.sources`
- I need to manage the `[tool.uv.sources]` separately for every `dagster-*` package.

What would be better is if I could somehow maintain a _single_ list of sources in some external file and link that file when installing any `dagster-*` into a test environment. This would effectively function like an "index" that shadows PyPI:

```
### SOURCES.toml

dagster-foo = { path = "/path/to/dagster/python_modules/dagster-foo", editable = true }
dagster-bar = { path = "/path/to/dagster/python_modules/dagster-bar", editable = true }
dagster-baz = { path = "/path/to/dagster/python_modules/dagster-baz", editable = true }
# ... any other dagster-* packages
```

Then when installing `dagster-foo`, I would not have any `tool.uv.sources` listed in its pyproject.toml, but would just do:

```
uv pip install -e path/to/dagster-foo --sources SOURCES.toml

# analogous to the below command, _if_ we had a private index https://dagster.monorepo.index that somehow contained the 
# local versions of all dagster-* packages (which we don't)
uv pip install -e path/to/dagster-foo --index https://dagster.monorepo.index
```

---

Is this functionality already supported through some pattern I haven't thought of? If not, consider this a feature request.



---

_Label `enhancement` added by @charliermarsh on 2024-12-26 14:30_

---

_Label `configuration` added by @charliermarsh on 2024-12-26 14:30_

---

_Comment by @smackesey on 2025-01-25 16:51_

Just wanted to check in on this issue. This would solve a lot of our monorepo headaches so I would love to know if it's in the cards.

---
