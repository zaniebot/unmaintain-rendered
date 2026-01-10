---
number: 11645
title: Avoid unnecessary downgrades due to development dependencies
type: issue
state: open
author: staurostriantafyllos
labels:
  - enhancement
assignees: []
created_at: 2025-02-19T22:32:52Z
updated_at: 2025-08-22T17:47:49Z
url: https://github.com/astral-sh/uv/issues/11645
synced_at: 2026-01-10T01:25:08Z
---

# Avoid unnecessary downgrades due to development dependencies

---

_Issue opened by @staurostriantafyllos on 2025-02-19 22:32_

### Summary

Hello **uv** community! 👋

```
Platform
macOS 15.3

Version
uv 0.6.0 (591f38c25 2025-02-14)

Python version
3.11.10
```

While experimenting with **uv**, I noticed that all types of dependencies are tightly coupled. For example, when adding a new dependency, uv tries to find a valid version that satisfies all constraints across all dependency types (project, development, etc.), regardless of whether the library is going to be installed in the environment.

### The issue

If I already have a **development dependency** that requires _mylibrary<=2.7.0_ and I try to add a new project dependency that also uses _mylibrary_, uv forces the installation (downgrade) to at most _mylibrary==2.7.0_, even though the latest available version might be 2.10.6.

This means that in **production**, we may be forced to use an **outdated** and potentially **less secure** version of a library just because of a development dependency that is only meant for local development.

I understand the current approach focuses on performance and syncing, but in production environments where security is a priority—especially in enterprises that undergo CVE audits—convenience and speed alone are not enough. Developers can manually identify and resolve such versioning issues, but this adds extra work. Also, removing development dependencies from pyproject.toml and managing them separately isn’t a viable option for me (and probably not for others either 😛)

### Reproduce issue
**Initial pyproject.toml**

```
dependencies = []

[dependency-groups]
dev = ["pydantic<=2.7.0"]
```

**Running** _uv add_ **command**
```
uv add pydantic
```

**What happens:**
```
dependencies = [
    "pydantic>=2.7.0",
]
```
**Installed versions:**
```diff
Prepared 3 packages in 220ms
Uninstalled 1 package in 5ms
Installed 5 packages in 14ms
 annotated-types==0.7.0
+ pydantic==2.7.0
 pydantic-core==2.18.1
 typing-extensions==4.12.2
```
### Suggested solution

Introduce a _--no-downgrade_ option (default: warning) when adding dependencies:

- `uv add mylibrary --no-downgrade warning` or `uv add mylibrary `
- `uv add mylibrary --no-downgrade error`

(_I’m terrible at naming arguments—open to better suggestions!_ 😅)

**Expected behavior**
- Identify versions that fully satisfy project dependencies first.
- Then try to include development dependencies.
- If a common library (used in both project and development dependencies) requires a downgrade:
    - **--no-downgrade-level warning**: Show a warning explaining which libraries were downgraded and to which version, then proceed.
    - **--no-downgrade-level error**: Show the same warning, but prevent the installation, requiring the developer to first upgrade the development dependency that caused the issue.

### Example

This example uses pydantic, but it could be any library that isn’t explicitly listed in the project. If someone suggests just pinning the version, that may not work because the library could be a transitive dependency, pulled in by another package.

**Initial pyproject.toml**
```
dependencies = []

[dependency-groups]
dev = ["pydantic<=2.7.0"]
```

**Running** _uv add_ **command in** _warning_ **level**
```
uv add pydantic --no-downgrade-level warning
```

**What happens:**
```
dependencies = [
    "pydantic>=2.7.0",
]
```
**but** a warning is also displayed, saying that pydantic could have been added at version 2.10.6 but was downgraded to 2.7.0 due to constraints from development dependencies. Ideally, the warning could also suggest the minimum version required for the development dependency to avoid the downgrade, making it even more useful.

**Running** _uv add_ **command in** _error_ **level**
```
uv add pydantic --no-downgrade-level error
```

**What happens:**

The toml file remains unchanged and a warning is displayed, indicating that the library could have been added with pydantic version 2.10.6 in the project dependencies. However, it was restricted to 2.7.0 due to constraints from development dependencies. The developer is prompted to upgrade the development dependencies first or set _--no-downgrade-level_ to _warning_.



---

_Label `enhancement` added by @staurostriantafyllos on 2025-02-19 22:32_

---

_Renamed from "Avoid unnecessary downgrades due to development dependencies – Add Downgrade Warning" to "Avoid unnecessary downgrades due to development dependencies" by @staurostriantafyllos on 2025-02-19 22:35_

---

_Comment by @zanieb on 2025-02-19 23:08_

It sounds loosely like you want to declare the development dependencies as conflicting with your project so we can solve their constraints independently https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies 

---

_Comment by @staurostriantafyllos on 2025-02-20 00:05_

I’m not talking about conflicts! Conflicting dependencies are fine because they produce an explicit error that the developer can see and fix. (btw `tool.uv -> conflicts` is an interesting feature for other cases)

The issue I’m highlighting is silent version downgrades caused by development dependencies (without any conflict). This happens when a **local tool** (e.g., black, mypy) imposes constraints that indirectly downgrade a **project’s transitive dependencies**, even though these development dependencies aren’t required in production.

This is problematic because:
- The developer isn’t notified that a downgrade occurred in a **transitive** dependency of **project's** dependencies
- It can lead to using **outdated** or insecure dependencies in production due to constraints from tools that are only meant for local development.

What I need is a way to ensure that uv **first** resolves project dependencies independently, then considers development dependencies **without forcing a downgrade** unless explicitly allowed. The --no-downgrade flag I suggested would help developers identify and control these cases rather than dealing with them silently.

If this isn’t clear, let me know—Maybe I can provide a more detailed example to clarify the issue.

---

_Comment by @zanieb on 2025-02-20 00:16_

I'm just not quite sure what you're suggesting is feasible. Resolving packages doesn't quite work that way. I'll give this some thought though.

---

_Comment by @staurostriantafyllos on 2025-02-21 02:19_

Hey Zanie & uv community 👋

Indeed, I have no idea how package resolution works, and I hope I never have to! 😆 
It seems incredibly complex after watching some of Charlie’s presentations.

But I wanted to follow up with **two (2) alternative solutions** that might be much simpler to implement.

### Updates from my last messages:

As I explored more about how uv works, I realized that **all interfaces use the same resolver** logic (i.e., uv add/remove/export/sync/lock). Initially, I thought the issue was tied to sync, so I assumed using the pip interface might solve it.

I tried:
```
uv export --no-dev --format requirements-txt > requirements.txt
uv pip install -r requirements.txt
uv pip install -e .
```
Even with --no-dev, the resolver **still downgraded** the library version of a project dependency.

This means there’s currently **no option to ignore dev dependencies during resolution**, which is quite concerning for production use cases.

### The (hacky) workaround

The only way I found to work around this is:

1. Manually empty dev dependencies in pyproject.toml:
```
[dependency-groups]
dev = []
```
2. Run `uv lock --upgrade`
3. Rename uv.lock to uv.prod.lock
4. Commit the lockfile
5. In CI/CD during the Docker build:
    1. Rename uv.prod.lock to uv.lock
    2. Run `uv sync --frozen`

This works, but obviously, it’s not ideal.

### Proposed Solutions
**Optional 1 : Make `--no-dev` do what it promises** (Preferred option!)
- This flag should completely ignore dev dependencies in resolution, as if they never existed (essentially replicating step 1 of my workaround).
- This would ensure that uv.lock only contains project dependencies.
    - This would also resolve [issue #9967](https://github.com/astral-sh/uv/issues/9967).
- **Bonus**: If uv identifies that dev dependencies need to be installed, it could generate a separate (hidden) .uv.lock file, which could be added to .gitignore.

**Outcome**:
```
uv sync --no-dev  # Completely ignores dev dependencies
```

**Optional 2 : Add `--only-project` and `--lockfile` options**
- Introduce a 4th flag (`--only-project`), similar to `--dev`, `--only-dev`, and `--no-dev`.
- This would install all non-dev dependencies needed for a project. A good aspect of adding this option as an extra is that it will not affect the existing interface.
- Additionally, a `--lockfile` option would allow users to specify a custom lockfile name, which is already being discussed in [issue #6830](https://github.com/astral-sh/uv/issues/6830).

**Outcome**:
```
uv lock --only-project --lockfile uv.project.lock
```

### Why this matters beyond the lab
In my current company, **staying up to date with dependencies is critical** due to security audits and CVE compliance. Even without dev dependencies, we already face challenges managing project dependencies. If dev dependencies introduce additional overhead, it makes things much worse.

Of course, we do run CVE checks in CI/CD, but why should we waste time fixing issues that shouldn’t exist in the first place?

Sorry for the long message guys, and thanks for taking the time to consider this! I really appreciate it. 🙏

---

_Comment by @staurostriantafyllos on 2025-02-24 21:48_

Hey @zanieb and @charliermarsh , just following up to see if my recent update caught your attention.

I proposed two alternative solutions that might make it much easier to handle silent downgrades caused by development dependencies.

Would love to hear your thoughts when you have a chance. Appreciate your time! Thanks

---

_Comment by @konstin on 2025-03-10 22:20_

I want to share two notes on this problem, in hopes of showing which capabilities uv we could extend to better avoid this problem:

In your proposal, you have a `uv.prod.lock` and a `uv.lock`. This looks like a case for conflicting dependencies: The requirements aren't technically conflicting, but you want to have two different versions for the same package that are selected depending on CLI arguments and can't be installed together. Despite the name "conflicting dependencies", it's really more "different candidates for the same package on the same platform that are installed in different scenarios because they can't be installed at the same time".

You mention black as an example. I share your concern that black's dependencies shouldn't influence your production dependencies' versions. Ideally, we'd isolate such tools into their own environment, such as `uv tool` does, even though this currently does not integrate with defining the black version in `pyproject.toml` (see e.g. #5903).

---

_Comment by @dgutson on 2025-08-22 17:47_

FWIW I'm looking forward to this feature, in relation to https://github.com/pypa/pip/issues/10807 . This would be a very good additional migration reason.

---
