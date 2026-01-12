```yaml
number: 17247
title: Custom command hook for uv sync/lock/run/... calls
type: issue
state: open
author: danijar
labels:
  - enhancement
assignees: []
created_at: 2025-12-28T20:10:03Z
updated_at: 2026-01-12T18:02:03Z
url: https://github.com/astral-sh/uv/issues/17247
synced_at: 2026-01-12T18:23:48Z
```

# Custom command hook for uv sync/lock/run/... calls

---

_@danijar_

### Summary

We have been exploring uv extensively for our mono repository, and noticed that currently the only way to support our requirements is to wrap all uv commands to generate `pyproject.toml` entries before calling into uv.

Being able to register a custom command with uv for ensuring a consistent environment would make this much easier, allowing our users to still call uv directly (and rely on the uv documentation).

This could be done by specifying an environment variable `UV_PRE_SYNC_COMMAND="shell command"`. uv would execute this command each time it needs to ensure a consistent environment.

### Example

As one example, we want to specify our dependencies as repository-absolute paths like `org-deep-nested-namespace-dependency` instead of relative paths like `../../../namespace/dependency`.

For this, we can add a custom section to the `pyproject.toml` of each package in the monorepo:

```toml
[tool.monorepo]
dependencies = [
  'org-deep-nested-namespace-dependency',
]
```

We then have a custom `sync_deps.sh` script that finds all `pyproject.toml` files in the monorepo, reads the above config block, and generates the `[tool.uv.sources]` block with relatives paths as required by uv:

```toml
# generated
[tool.uv.source]
org-deep-nested-namespace-dependency = { path = "../../../namespace/dependency", editable = true }
```

Currently, we have to manually call `sync_deps.sh` each time we change a `pyproject.toml` file, which slows down developers and is easy to forget. We could also wrap all uv commands to always call `sync_deps.sh` (for the whole repo or just the `pyproject.toml` targeted by the uv command and its recursive dependencies) before forwarding the call to the uv binary, but then our developers can't call the familiar uv binary directly anymore.

With a custom command hook, we'd register `sync_deps.sh` with uv, for example via an environment variable: 

```
export UV_PRE_SYNC_COMMAND="repo/sync_deps.sh"
```

We could then use uv natively and with low maintenance burden.

---

_Label `enhancement` added by @danijar on 2025-12-28 20:10_

---

_Comment by @konstin on 2026-01-06 19:20_

This sounds like https://github.com/astral-sh/uv/issues/11826

---

_Comment by @danijar on 2026-01-12 17:47_

### Commit vs. Runtime Consistency

At first I also thought #11826 was the same feature, but it seems to be more about the pre-commit use case, i.e. enforcing constraints of commits to the repository. They mention husky and lint-staged, which lints files or runs tests. Those tasks just need to happen **at some point before a commit is created**.

My feature request here is about ensuring runtime constraints to the local files, even before they are committed. We want to enforce correct and consistent `uv run` results when sections of `pyproject.toml` are generated. This needs to be enforced **before every uv command that reads `pyproject.toml`**.

There may be one feature that solves both problems, but it's not entirely clear one way or the other yet.

### Task Runners

I also saw your comment about task runners in the other thread, for example using hatch:

```toml
[tool.hatch.envs.default.scripts]
sync = ["sh sync_deps.sh", "uv sync"]
build = ["sh sync_deps.sh", "uv build"]
run = ["sh sync_deps.sh", "uv run"]
# ...
```

The problems here are:

- Have to wrap every single uv command (boilerplate, hassle to maintain)
- Developers can't use familiar `uv ...` commands anymore (cognitive overhead, both when using commands and when copying uv commands from websites, from Claude, etc)
- More verbose to type out and harder to read, e.g. `hatch run run python` instead of `uv run python`
- Forwarding args to uv is possible but needs workarounds
- Makes everything less standard (if there are other scripts, they all need to use the new commands now instead of plain uv)
- uv knows better in what situations it reads `pyproject.toml` than us (and it could change over time)

---
