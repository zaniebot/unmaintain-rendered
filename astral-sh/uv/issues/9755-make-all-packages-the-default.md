---
number: 9755
title: "Make `--all-packages` the default?"
type: issue
state: closed
author: gjoseph92
labels:
  - question
assignees: []
created_at: 2024-12-10T00:09:48Z
updated_at: 2025-06-29T13:48:16Z
url: https://github.com/astral-sh/uv/issues/9755
synced_at: 2026-01-10T01:24:45Z
---

# Make `--all-packages` the default?

---

_Issue opened by @gjoseph92 on 2024-12-10 00:09_

When setting up a uv workspace in a monorepo, I was surprised to find that `uv sync` would change which packages were installed depending on which directory I ran it from. For example:
1. it removed all my transitive dev dependencies when run from the root directory
2. when run within a sub-package, it removed dependencies (required and dev) for all other packages!

Overall, it's confusing that the behavior of `uv sync` changes depending on where you run it from within a workspace that just has one shared environment.

Without `--all-packages`, `uv sync` seems to behave as though the virtual environment belongs only to whichever package you're currently in—which would make sense in non-workspace mode, where each package has its own environment. But in a workspace, the environment is shared between all members, so it feels odd for each member to trample on this shared resource by default.

<details><summary>Some specific repros</summary>

You can recreate these with https://github.com/gjoseph92/uv-monorepro

The transitive dev dependencies aren't available in the base workspace (I'd expect `py-spy` and `pip` to be here):
```
uv-monorepro main*​(uv-monorepro) ❯ uv tree
Resolved 15 packages in 2ms
uv-monorepro v0.1.0
├── needs-things v0.1.0
│   ├── thing1 v0.1.0
│   │   ├── cowsay v6.1
│   │   └── py-spy v0.4.0 (group: dev)
│   └── thing2 v0.1.0
│       ├── trio v0.27.0
│       │   ├── attrs v24.2.0
│       │   ├── idna v3.10
│       │   ├── outcome v1.3.0.post0
│       │   │   └── attrs v24.2.0
│       │   ├── sniffio v1.3.1
│       │   └── sortedcontainers v2.4.0
│       └── pip v24.3.1 (group: dev)
└── thing1 v0.1.0 (*)
(*) Package tree already displayed
uv-monorepro main*​(uv-monorepro) ❯ which pip   
pip not found
```

If I go into a subpackage, `uv sync` uninstalls the _required_ dependency (`cowsay`) of my other subpackage (because it's not a dependency of this subpackage):
```
uv-monorepro main*​(uv-monorepro) ❯ cd packages/thing2
uv-monorepro/packages/thing2 main*​(uv-monorepro) ❯ uv sync
Resolved 15 packages in 2ms
Uninstalled 1 package in 8ms
Installed 1 package in 9ms
 - cowsay==6.1
 + pip==24.3.1
uv-monorepro/packages/thing2 main*​(uv-monorepro) ❯ which pip
/Users/gjoseph/scratch/uv-monorepro/.venv/bin/pip
```

Going to the other subpackage does the inverse:
```
uv-monorepro/packages/thing2 main*​(uv-monorepro) ❯ cd ../thing1      
uv-monorepro/packages/thing1 main*​(uv-monorepro) ❯ uv sync
Resolved 15 packages in 1ms
Uninstalled 7 packages in 97ms
Installed 2 packages in 2ms
 - attrs==24.2.0
 + cowsay==6.1
 - idna==3.10
 - outcome==1.3.0.post0
 - pip==24.3.1
 + py-spy==0.4.0
 - sniffio==1.3.1
 - sortedcontainers==2.4.0
 - trio==0.27.0
uv-monorepro/packages/thing1 main*​(uv-monorepro) ❯ which pip
pip not found
```

Back in the root workspace, `uv sync` again does install the necessary dependencies for the subpackages, but not their dev dependencies (`py-spy` and `pip` are both gone now). Given that the [docs say](https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups) the dev group is included by default, I would have expected and wanted all the dev dependencies of member packages to be installed too.
```
uv-monorepro/packages/thing1 main*​(uv-monorepro) ❯ cd ../..    
uv-monorepro main*​(uv-monorepro) ❯ uv sync
Resolved 15 packages in 2ms
Uninstalled 1 package in 3ms
Installed 6 packages in 5ms
 + attrs==24.2.0
 + idna==3.10
 + outcome==1.3.0.post0
 - py-spy==0.4.0
 + sniffio==1.3.1
 + sortedcontainers==2.4.0
 + trio==0.27.0
uv-monorepro main*​(uv-monorepro) ❯ which pip
pip not found
```

`--all-packages` gets the behavior I want. And most importantly, it always results in the same things being installed no matter which directory I run it from.
```
uv-monorepro main*​(uv-monorepro) ❯ uv sync --all-packages
Resolved 15 packages in 2ms
Installed 2 packages in 8ms
 + pip==24.3.1
 + py-spy==0.4.0
uv-monorepro main*​(uv-monorepro) ❯ which pip
/Users/gjoseph/scratch/uv-monorepro/.venv/bin/pip
```

</details>

I couldn't find other issues requesting this, but apologies if this is a dupe. Maybe somewhat related to https://github.com/astral-sh/uv/issues/9261?

---

_Comment by @charliermarsh on 2024-12-10 00:25_

I think we're somewhat unlikely to change this... I hear your confusion but it may just be a learning curve that comes from working with the workspace concept? Cargo, for example, behaves the same way: the workspace root is a member, just like any other, and by default `cargo run` uses the current directory which can be overridden with `-p`.

In our case, `uv sync` always does a strict sync. You can use `uv sync --inexact` if you only want to make the minimal changes necessary to include all dependencies for the current workspace member. (`uv run` uses these "inexact" semantics by default.)

> But in a workspace, the environment is shared between all members, so it feels odd for each member to trample on this shared resource by default.

Well... I think this isn't quite true. You can have members within a workspace that don't depend on any other members. Those members should declare all the direct dependencies that they need. In that case, running `uv sync` and removing dependencies of _other_ members is totally right.


---

_Label `question` added by @charliermarsh on 2024-12-10 13:29_

---

_Closed by @zanieb on 2025-01-07 19:23_

---

_Closed by @zanieb on 2025-01-07 19:23_

---

_Referenced in [renovatebot/renovate#33874](../../renovatebot/renovate/issues/33874.md) on 2025-05-07 17:07_

---

_Comment by @ntjess on 2025-06-29 13:21_

@charliermarsh would you consider supporting a pyproject configuration option like:

```toml
[tool.uv]
sync-all-packages = true
```

in the monorepo root so that workspaces which _do_ share the same environment can enable this behavior?

Use case: allow multiple devs using the same code base to experience the same CLI behavior running `uv sync` without needing to add bash scripts/etc to the repo

Apologies if this is already possible -- I checked the [docs](https://docs.astral.sh/uv/reference/settings) for configurations and didn't come across it. Also happy to open a new issue if it's preferable


Edit: just came across #5737 which looks like a similar request so I'll subscribe to that issue instead 🙂


---
