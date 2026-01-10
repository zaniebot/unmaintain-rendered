---
number: 2630
title: Should uv pip compile --prerelease=if-necessary-or-explicit work for explicit dependencies of dependencies?
type: issue
state: closed
author: VerdantFox
labels: []
assignees: []
created_at: 2024-03-22T23:57:26Z
updated_at: 2024-03-23T00:13:41Z
url: https://github.com/astral-sh/uv/issues/2630
synced_at: 2026-01-10T01:23:20Z
---

# Should uv pip compile --prerelease=if-necessary-or-explicit work for explicit dependencies of dependencies?

---

_Issue opened by @VerdantFox on 2024-03-22 23:57_

uv version: 0.1.24
Platform: WSL on Windows 11
uv was installed by `pipx`

I'm using `uv pip compile` to generate a `requirements.txt` file from a `pyproject.toml` file. Based on version requirements in the `pyproject.toml` file, one of the dependencies of my dependencies appears to require a prerelease version. I set the `--prerelease=if-necessary-or-explicit` flag (which says it's the default anyway), but the compile fails, saying that `--prerelease=allow` is required. If I use `--prerelease=allow`, the compile does indeed work. However, if I use `--prerelease=allow`, uv eagerly sets other dependencies to prerelease versions that don't need to be prerelease, which I don't want.

1. Is this a bug? Should `--prerelease=if-necessary-or-explicit` resolve dependencies as I'd expect in this case?
2. If it isn't a bug, is there a way to do what I want? i.e., only resolve prerelease versions if it is absolutely needed to resolve my dependencies?

Here's a simplified `pyproject.toml` file that reproduces my problem:

```toml
[project]
name = "example"
requires-python = ">=3.10"
dependencies = [
    "azure-cli>2.50.0",
    "beautifulsoup4"
]
```

Here's the command that produces the error:

```bash
$ uv pip compile --generate-hashes --upgrade --prerelease=if-necessary-or-explicit --output-file=requirements.txt pyproject.toml
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of azure-keyvault-keys==4.8.0b2 and azure-cli>=2.51.0,<=2.53.1 depends on azure-keyvault-keys==4.8.0b2, we can conclude that azure-cli>=2.51.0,<=2.53.1 cannot
      be used.
      And because only the following versions of azure-cli are available:
          azure-cli<=2.50.0
          azure-cli==2.51.0
          azure-cli==2.52.0
          azure-cli==2.53.0
          azure-cli==2.53.1
          azure-cli==2.54.0
          azure-cli==2.55.0
          azure-cli==2.56.0
          azure-cli==2.57.0
          azure-cli==2.58.0
      we can conclude that azure-cli>2.50.0,<2.54.0 cannot be used. (1)

      Because there is no version of azure-keyvault-administration==4.4.0b2 and azure-cli>=2.54.0 depends on azure-keyvault-administration==4.4.0b2, we can conclude that azure-cli>=2.54.0 cannot
      be used.
      And because we know from (1) that azure-cli>2.50.0,<2.54.0 cannot be used, we can conclude that azure-cli>2.50.0 cannot be used.
      And because example depends on azure-cli>2.50.0, we can conclude that the requirements are unsatisfiable.

      hint: azure-keyvault-keys was requested with a pre-release marker (e.g., azure-keyvault-keys==4.8.0b2), but pre-releases weren't enabled (try: `--prerelease=allow`)

      hint: azure-keyvault-administration was requested with a pre-release marker (e.g., azure-keyvault-administration==4.4.0b2), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

Again, with the `--prerelease=allow` flag, the command works, but then `beautfulsoup4` resolves to `beautifulsoup4==4.13.0b2`. Without the `--prerelease=allow` flag beautifulsoup4 resolves to `beautifulsoup4==4.12.3`, the version I would like it to resolve to.


---

_Renamed from "Shouldn't uv pip compile --prerelease=if-necessary-or-explicit work for explicit dependencies of dependencies?" to "Should uv pip compile --prerelease=if-necessary-or-explicit work for explicit dependencies of dependencies?" by @VerdantFox on 2024-03-22 23:57_

---

_Comment by @charliermarsh on 2024-03-22 23:58_

I don't think uv supports what you're looking for. It's described in more detail here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#pre-release-compatibility. The core idea is that we need to know upfront which packages are _allowed_ to use pre-releases, because changing the set of _allowed_ packages over the course of resolution creates a lot of complexity.

---

_Comment by @VerdantFox on 2024-03-23 00:05_

Wow, thanks for the incredibly fast reply (< 2 minutes 🤯) and the link to that documentation. I can work with that. This project and ruff are awesome; keep up the good work! 🙂

---

_Closed by @VerdantFox on 2024-03-23 00:05_

---

_Comment by @charliermarsh on 2024-03-23 00:13_

No prob! I'm hoping to expand pre-release support at some point but it's tricky and the spec needs to evolve a bit to make the expectations clear for tools.

---
