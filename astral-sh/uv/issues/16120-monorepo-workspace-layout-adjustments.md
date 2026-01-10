```yaml
number: 16120
title: Monorepo/Workspace Layout Adjustments
type: issue
state: open
author: ZachHandley
labels:
  - enhancement
assignees: []
created_at: 2025-10-03T20:31:28Z
updated_at: 2025-10-08T06:22:03Z
url: https://github.com/astral-sh/uv/issues/16120
synced_at: 2026-01-10T03:23:54Z
```

# Monorepo/Workspace Layout Adjustments

---

_Issue opened by @ZachHandley on 2025-10-03 20:31_

### Summary

Hi!

I love the project, and I just wanted to say thank you, first and foremost

I want to be able to define workspaces like, normally. I really hate how Python makes you do e.g.

```
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```

Is there any way we could (even with a symlink behind the scenes or something?) do more like

```
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       ├── __init__.py
│   │       └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│             ├── __init__.py
│             └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
        └─ main.py
```

IMO this is cleaner, and makes more sense to my brain, but maybe that's me --

### Example

Well, you'd just specify the workspace packages, and uv would automatically know that the `pythonpath` and the `pyproject.toml` specifies the package, so if I have in the bird-feeder pyproject.toml that it's called `bird-feeder`, I don't quite get why that isn't just resolved automatically?

I get that python has standards, and maybe it goes against them, but I would prefer to at least have this as an option.

---

_Label `enhancement` added by @ZachHandley on 2025-10-03 20:31_

---

_Comment by @woutervh on 2025-10-05 16:16_

The whole point of a src-folder is that it is not importable.
A src-folder should only contain one or more directories.
It should never contain *.py files directly.

What you are doing, is basically creating a project called "src" 3x times.



For a workspace mono, I would put all packages in src:

```
albatross
├── src
│   ├──  albatross
│   │    └── main.py
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
```

in pyproject.toml
```
[tool.uv.workspace]
members = ["src/*"]
exclude = ["src/albatross"]
```

---

_Comment by @konstin on 2025-10-06 09:39_

See https://github.com/astral-sh/uv/pull/11325 for this being proposed and the rebuttal.

---

_Comment by @ZachHandley on 2025-10-06 20:24_

interesting, okay, I'll try that! I still think it's not really clear, because in TypeScript for instance, I have

basically what I mean is: imagine Python supported monorepos like TypeScript with pnpm workspaces.

so instead of one big `src/`, you'd do this kind of layout:

```
root
├── README.md
├── pyproject.toml
├── uv.lock
└── packages
    ├── albatross
    │   ├── pyproject.toml
    │   └── src/main.py
    ├── bird-feeder
    │   ├── pyproject.toml
    │   └── src/foo.py
    ├── seeds
    │   ├── pyproject.toml
    │   └── src/bar.py
    └── seagull
        ├── pyproject.toml
        └── src/wind.py
```

and then the root `pyproject.toml` acts like a workspace manager, same as a pnpm monorepo:

```
[tool.uv.workspace]
members = [
  "packages/albatross",
  "packages/bird-feeder",
  "packages/seeds",
  "packages/seagull",
]
```

and each subpackage just declares editable path deps:

```
[tool.uv.sources]
seeds = { path = "../seeds", editable = true }

[tool.uv]
dependencies = ["seeds"]
```

basically — this would give Python a pnpm-style workspace, single `uv.lock`, and linked editable packages by default.

Granted, I know this would move away from Python standards a bit, which is probably averse to the goal, I just think it would make it a lot easier to use, IMO. I don't know, I just don't love having multiple sub-folders for seemingly not a ton of reason?


---

_Comment by @seffs on 2025-10-08 06:22_

> this would give Python a pnpm-style workspace, single `uv.lock`, and linked editable packages by default.

If I fully understood what you're looking for, I think this is already the case. Consider the following structure:

```
 backend/
  ├── pyproject.toml       # workspace root
  ├── uv.lock              # single lock file for all packages
  ├── libs/
  │   └── core/
  │       ├── pyproject.toml
  │       └── src/
  └── services/
      ├── api/
      │   ├── pyproject.toml
      │   └── src/
      ├── messaging/
      │   ├── bridge/
      │   │   ├── pyproject.toml
      │   │   └── src/
      │   └── relay/
      │       ├── pyproject.toml
      │       └── src/
      └── middleware/
          └── orchestrator/
              ├── pyproject.toml
              └── src/
```

`backend` acts as our root. In order to serve as the "workspace manager" you need to define:

```toml
# backend/pyproject.toml
[tool.uv.workspace]
members = [
    "libs/*",
    "services/api",
    "services/messaging/*",
    "services/middleware/*",
]

# No [build-system] defined!
```

For each (sub)component, e.g. the API service:

```toml
  # Individual package (services/api/pyproject.toml)
  [project]
  name = "my-api-gateway"
  dependencies = [
      "my-core",
      "fastapi[all]>=0.115.8",
      # ... other deps
  ]

  [tool.uv.sources]
  my-core = { workspace = true }  # editable workspace dep
```

Executing `uv sync --all-packages` at the root (`backend/`) takes care of the rest.

## Bonus

I found an elegant (or rather lazy) way to take care of complex, nested layouts as above. You could do something like this:

```toml
[tool.uv.workspace]
members = [
    "[!.]*/**" # Traverse all visible directories right under backend/
]

exclude = [
    "**/src", # Exclude all src/ dirs
    "**/src/**",  # Exclude all src/ subdirs
    # Exclude non-python folders
    "docs",
    # Define the immediate parent of nested projects
    "services/messaging",
    "services/middleware",
    "services/x/y/z",
    # ...
]
```

I like this approach better as it will raise an error on undefined nested paths instead of being permissive and "failing" silently.

---
