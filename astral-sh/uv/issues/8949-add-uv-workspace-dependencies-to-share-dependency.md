---
number: 8949
title: Add uv.workspace.dependencies to share dependency versions across workspace members
type: issue
state: open
author: BaxHugh
labels:
  - enhancement
assignees: []
created_at: 2024-11-08T16:07:31Z
updated_at: 2025-09-12T13:57:40Z
url: https://github.com/astral-sh/uv/issues/8949
synced_at: 2026-01-10T01:24:35Z
---

# Add uv.workspace.dependencies to share dependency versions across workspace members

---

_Issue opened by @BaxHugh on 2024-11-08 16:07_

Support cargo like workspace.dependencies.

We used cargo workspace.dependencies to ensure all our crates within a monorepo share the same versions of common dependencies. There version is specified in the root Cargo.toml file, and the project crates specify that they should use the dependency as specified in the workspace root's Cargo.toml
Here's the [Cargo Book documentation](https://doc.rust-lang.org/cargo/reference/workspaces.html#the-dependencies-table) on it.

For the python projects in our monorepo, we'd like the same functionality across python projects, so that we know all the packages in the monorepo have the same version specification for i.e. numpy etc.

I've had a dig through the docs, source code, and existing issues, and I don't think this has been proposed yet.

Here's my suggestion of what I think it'd look like, i.e.:
add a `tool.uv.workspace.dependencies` for the version spec at the workspace level, key and `tool.uv.dependencies` to specify dependencies of packages which would use the spec from the workspace.

### root pyproject.toml
```toml
[project]
name = "root"
version = "0"
requires-python = ">=3.12"

[tool.uv]
# Package false because this is to manage a monorepo, not a package with multiple subpackages
package = false

[tool.uv.workspace]
members = ["packages/*"]
# These are the dependency versions that workspace members will use
# since this is a package = false file, they don't go into any actual package as defined here
dependencies = [
    "tqdm >=4.66.2,<5"
]
```

### packages/bird-feeder/pyproject.toml
```toml
[project]
name = "bird-feeder"
requires-python = ">=3.12"
# Normally we'd have a dependencies field here,
# but it's unlikely that there's a PEP 508 compliant way of storing dependency strings
# with some workspace = true fields

# specify workspace dependencies
# in a field which allows inheriting the versions from the workspace root's dependency versions
[tool.uv.dependencies]
tqdm = { workspace = true }
anyio = ">=4.3.0,<5"
```

---

_Label `enhancement` added by @charliermarsh on 2024-11-09 02:30_

---

_Comment by @charliermarsh on 2024-11-09 02:30_

Yeah we don't support this today -- thanks for filing. We'd need to come up with a clever strategy to ensure that it remains standards-compliant.

---

_Comment by @jeamland on 2024-11-14 04:09_

Is it worth trying to turn the broader workspace stuff into a PEP? It'd be awesome if this became a general part of Python packaging.

---

_Comment by @feici02 on 2024-11-15 02:52_

@charliermarsh For a monorepo of multiple microservices, I'd like to manage them as members of a workspace, should the dependencies of all these services be declared altogether at the root level pyproject.toml? Is there a better solution now?

---

_Comment by @charliermarsh on 2024-11-15 03:00_

@feici02 -- You can either declare them as dependencies of the root or use `uv sync --all-packages` to sync the entire workspace at once, if that's what you need. I'd generally recommend making them dependencies of the root, though.

---

_Comment by @feici02 on 2024-11-15 05:58_

@charliermarsh Thank you for the quick reply. `uv sync --all-packages` is just what I want.
My intention here is to define common dependencies at the root level, and member-specific dependencies at the member level. In this way, it is clearer to know a certain dependency is introduced by which member.

---

_Comment by @Jonatha1983 on 2024-12-07 12:19_

@charliermarsh So just a clarification on this topic, if I declare a dependency in the root all members are getting it right? 
What about groups? Same behavior? optionals? 

Also what happens if a member a declare it needs member b from dependencies point of view? 
If member b declare a dependency on an external package is it become transitive to a? 

---

_Comment by @BaxHugh on 2024-12-07 12:52_

@Jonatha1983 

> if I declare a dependency in the root all members are getting it right?
I don't think so, the pyproject.toml for each package still declares it's own package metadata in a PEP compliant way.
i.e., if in the root, I specify ;
```toml
dependencies = ["numpy", "matplotlib"]
```
and in a workspace member, I specify
```toml
dependencies = ["pandas"]
```

Then uv tree shows:
```sh
bar v0
└── pandas v2.2.3
    ├── numpy v2.1.3
    ├── python-dateutil v2.9.0.post0
    │   └── six v1.17.0
    ├── pytz v2024.2
    └── tzdata v2024.2
root v0
├── matplotlib v3.9.3
│   ├── contourpy v1.3.1
│   │   └── numpy v2.1.3
│   ├── cycler v0.12.1
│   ├── fonttools v4.55.2
│   ├── kiwisolver v1.4.7
│   ├── numpy v2.1.3
│   ├── packaging v24.2
│   ├── pillow v11.0.0
│   ├── pyparsing v3.2.0
│   └── python-dateutil v2.9.0.post0 (*)
└── numpy v2.1.3
```

However, if you're doing
```
uv sync --all-packages
```
Then your dev environment will get all of that.

```
uv sync
```
in just the workspace root, that will give you just the dependencies, specified in the root.

I don't know if that fully answers your question.

---

_Comment by @Jonatha1983 on 2024-12-07 13:08_

@BaxHugh thanks for the quick reply. 

> I don't know if that fully answers your question.

I don't know either 😄

I guess the fact I am coming from Java and that I am new to Python (and so new to UV) is not helping...

If we take your example, both the root and the member defined only dependencies and not the dev group, so when you say:

> Then your dev environment will get all of that.

Do you mean that I will be able to write:
`from matplotlib import...`
In all workspace members? Even if it doesn't declare that?
Does the same goes for test files? 

What if a member A declare 
```dependencies = ["temporalio"]```

and I run:
```uv sync --all-packages```
Will I be able to do this in member B source files:
```from temporalio import activity```

Even if it didn't declare it in its pyproject.toml? 

Mine may be very elementary questions. In that case, maybe you could provide me with some good documentation on the relation between member dependencies (including group dependencies)

Thanks!!!  



---

_Comment by @charliermarsh on 2024-12-07 13:22_

> So just a clarification on this topic, if I declare a dependency in the root all members are getting it right? What about groups? Same behavior? optionals?

In a formal sense, no. If `root` depends on `temporalio`, and then you have a workspace member called `member` that doesn't depend on the root, if you run `uv sync --package member`, you won't be able to `import temporalio`.

_However_, lets say that instead, `root` depends on `temporalio`, and `root` depends on `member`. If you `uv sync` (to sync `root`, by default), you actually _will_ be able to `import temporalio` from files in `member`, even though you shouldn't. Python doesn't have explicit module visibility, unlike Java, so all installed dependencies are equally accessible from all other modules.

In other words: uv will only install the dependency tree necessary when you sync a package. But the Python runtime itself doesn't prevent you from importing transitive dependencies.

If you're importing `temporalio` from a workspace member, that member should declare it as a dependency.


---

_Comment by @Jonatha1983 on 2024-12-07 14:04_

Thanks, I start getting it (I think and hope)

```
root/
|-- packages/
|   |-- lib/
|   |   |-- src/
|   |   |-- tests/
|   |   |-- README.md
|   |   |-- pyproject.toml
|   |-- app/
|       |-- src/
|       |-- tests/
|       |-- README.md
|       |-- pyproject.toml
|-- README.md
|-- pyproject.toml
|-- .python-version
```

In the root pyproject.toml:
```[tool.uv.workspace]
    members = ["packages/*"]
```
So if I want to work on both lib and app packages, I can just run:
```uv sync --all-packages```

And with that, I will have all the dependencies needed for both, correct?

So if that is ok, here are some more questions: 

Now if lib import ```temporalio``` and **app don't** after running the sync all-packages, I can (mistakenly) import temporalio in the app files right?

If in app pyproject.toml I have:

```
[tool.uv.sources]
lib = { workspace = true }
```

I still need to declare the `temporalio`, or will the fact that the app depends on lib make it available to the app?
If I do need to declare, what is the right approach to help me define the version of the external third package in one place?
If I have 20 members and 10 need `temporalio` should I declare in 10 projects the temporal dependency?

 
Thanks again!!!

---

_Comment by @pnasrat on 2024-12-07 14:24_

Reading this discussion and I'm wondering if for the monorepo/workspace case that uv could support using `dynamic` for dependencies  - my reading of PEP 621 https://peps.python.org/pep-0621/#dynamic is that  would allow for a monorepo workspace setup to define the versions in the root then propagate them to the wheel metadata.

For this case generation of sdists might need to generate a package uv.lock file that uv would use to enable rebuildabilitly of an individual component from sdist but I think that is how other tools handle centralized deps

One example that does dynamic dependencies using hatchling is airflow which is a monorepo that provides many plugins

https://github.com/apache/airflow/blob/c09e9b5202ce4f1c6ba02381c0284cb4cd7a84e7/pyproject.toml#L65

---

_Comment by @pnasrat on 2024-12-07 14:26_

See also https://github.com/astral-sh/uv/issues/6422

---

_Referenced in [astral-sh/uv#9811](../../astral-sh/uv/issues/9811.md) on 2024-12-12 03:46_

---

_Referenced in [astral-sh/uv#11250](../../astral-sh/uv/issues/11250.md) on 2025-03-04 08:32_

---

_Comment by @zeel-04 on 2025-04-10 20:24_

> If I have 20 members and 10 need `temporalio` should I declare in 10 projects the temporal dependency?

Very similar to this, I have root and 5 members, and 3 need the same dependency(let's say `streamlit`)

@charliermarsh How to handle this kind of situation. I know that if I declare `streamlit` in root do `uv sync --all-packages` I can access it in any members. but you told

> However, lets say that instead, root depends on temporalio, and root depends on member. If you uv sync (to sync root, by default), you actually will be able to import temporalio from files in member, even though you shouldn't

Is there any legit way to do it.

---

_Comment by @timos-flwls on 2025-06-24 08:35_

It would be great to have the current behaviour of root dependencies documenting on the "Using workspaces" page. Currently trying to infer it from the discussion in this thread.

My current understanding:
- `uv sync`: installs root dependencies.
- `uv sync --package foo`: installs only foo's dependencies.
- `uv sync --all-packages`: installs root and all members dependencies.

However, there is no way to say "install foo and root's dependencies".

To make matters worse, it seems like `uv.sources` (which is documented on the "Using workspaces" page) propagates to workspace members, whilst root project dependencies don't which is counter-intuitive.

---

_Comment by @zanieb on 2025-07-09 16:59_

As noted at https://github.com/astral-sh/uv/issues/8949#issuecomment-2465998739 — the challenge here is maintaining standards-compliance for the `project.dependencies` table. That's not a problem for `tool.uv.sources`, which is why that's easier to propagate throughout the workspace.

---

_Comment by @zanieb on 2025-07-09 17:00_

> there is no way to say "install foo and root's dependencies".

I presume `uv sync && uv sync --inexact --package foo` would work? We can probably find a better way to express that though.

---

_Comment by @ahmdatef on 2025-07-23 17:48_

The ways I imagine it is when run `uv add --project X dep-1` uv will be smart enough to check the dependencies of other members, and if it exists in other members it will be appended to the root TOML, else it will be appended to project X's TOML. 

---

_Referenced in [astral-sh/uv#15808](../../astral-sh/uv/issues/15808.md) on 2025-09-12 13:05_

---

_Comment by @sylbeth on 2025-09-12 13:27_

As I understand it, the steps needed to implement this are:

- Define exactly where and how workspace dependencies are configured.

I believe the most sensible to use `tool.uv.workspace.dependencies` for definition and `tool.uv.sources` for usage. That way, it stays PEP compliant without further trouble. For example, for a workspace with a root package that requires numpy and uses ruff and a package named `project` that requires both numpy and scipy and uses ruff, the following would be used.

```toml
# /pyproject.toml
[project]
dependencies = ["numpy"]

[dependency-groups]
dev = ["ruff"]

[tool.uv.sources]
ruff.workspace-dep = true
numpy.workspace-dep = true

[tool.uv.workspace]
members = ["project"]
dependencies = [
    "numpy>=2.3.3",
    "scipy>=1.16.2",
    "ruff>=0.12.7",
]
```

```toml
# /project/pyproject.toml
[project]
dependencies = ["numpy", "scipy"]

[dependency-groups]
dev = ["ruff"]

[tool.uv.sources]
scipy.workspace-dep = true
ruff.workspace-dep = true
numpy.workspace-dep = true
```

(Here either workspace or workspace-dep would be used, but then the question arises on what to do with workspace members. Workspace members could implicitly be workspace dependencies, and that way only workspace = true would be needed)

- Change the configuration parsing to read these new options.
- Change `tool.uv.sources` resolution to do so.
- Change the `uv add` command to allow for a `--workspace` flag to add to the workspace dependencies (and maybe other commands that would make sense)
- Change the way `uv add` resolution works by first looking through workspace dependencies before deciding whether to add a dependency with a specified version or the workspace dependency.
- Document the changes (+ CLI help message).

Did I leave anything out?

(Also, maybe if workspace and package are specified in a `uv add`, it could be added as a workspace dependency and then added properly to the given package, reducing the need for `uv add` calls?)

---

_Comment by @zanieb on 2025-09-12 13:34_

@sylbeth the blocker to implementing this is https://github.com/astral-sh/uv/issues/8949#issuecomment-2465998739 — note your example drops the constraints from the `project.dependencies` table which isn't desirable.

---

_Comment by @sylbeth on 2025-09-12 13:41_

> The blocker to implementing this is https://github.com/astral-sh/uv/issues/8949#issuecomment-2465998739

Is my proposed solution not standard-compliant? Afaik workspaces themselves aren't standard.

> note your example drops the constraints from the `project.dependencies` table which isn't desirable.

Isn't that exactly what is desired here? Having to maintain the constraints is the problem, and afaik the standards don't allow other restrictions there. One way to maintain the contraints would be to just add them. If `tool.uv.sources` has the dependency specified, it checks if the constraints of the package and the workspace are the same, if not it updates it. That would keep the restriction in place but need no manual maintenance. If instead of a change to a more restrictive version (e.g. >=1.0 to >=2.0) is a change to a less restrictive version (e.g. >=2.0 to >=1.0) the user could always be prompted to allow for it, unless a force flag is used.


I don't understand what the problem is exactly, as `tool.uv.sources` already exists and afaik it leaves version constraints empty. If easy migration to other project managers is desired, an option to migrate workspace dependencies can always be added, but the automaintained semver feels enough. It could be only changed on dependency updates and addition, if changes to pyproject on a sync / run / any other command where this could be checked is undesired.

---

_Referenced in [astral-sh/uv#13540](../../astral-sh/uv/issues/13540.md) on 2025-09-23 06:36_

---

_Referenced in [BERDataLakehouse/spark_notebook#43](../../BERDataLakehouse/spark_notebook/pulls/43.md) on 2025-11-11 22:00_

---
