```yaml
number: 13245
title: "`uv run` from multiple requirements"
type: issue
state: open
author: patrick-kidger
labels:
  - question
assignees: []
created_at: 2025-04-30T22:46:37Z
updated_at: 2025-05-02T12:52:03Z
url: https://github.com/astral-sh/uv/issues/13245
synced_at: 2026-01-10T03:41:47Z
```

# `uv run` from multiple requirements

---

_Issue opened by @patrick-kidger on 2025-04-30 22:46_

### Question

I've done a search through the issue tracker and not found any good matches. Apologies if this has come up somewhere already but I couldn't find it.

First the ask: is it possible to have `uv run` consume multiple requirements files as input to its locking+syncing? Analogous to how `uv pip compile pyproject.toml foo.txt -o requirements.txt` consumes both `pyproject.toml` and `foo.txt`, not just `pyproject.toml`.

Notably this is *not* the same as `uv run --with-requirements foo.txt`, as this creates and installs into an ephemeral venv. This breaks tooling (e.g. pytest can no longer find its plugins), and noticeably increases startup time. Although if you like this question could be reinterpeted as 'can `--with` install into the current venv', but I'm guessing that would be considered unsafe.

Nor is this the same as `uv sync --inexact; uv pip install -r foo.txt`, as this will not uninstall old dependencies.

Nor is this the same as `UV_PROJECT_ENVIRONMENT=.venv uv run --with foo.txt`, as unfortunately it seems that `--with` takes precedence and the environment variable goes ignored.

---

With all of that out the way, to avoid an X-Y problem, here is the use-case: our team have a library which specifies its dependencies as a `pyproject.toml` file. We use the high-level `uv run ...` which works as normal. However each developer individually also has additional idiosyncratic developer tooling that they'd like installed into their environment: `jupyter`, custom debuggers like `pudb`, the LSP from `ruff`, this kind of thing. These can't be expressed in the `pyproject.toml` file as they shouldn't be shared with other developers and in fact may refer to local-only unpublished packages.

Ideally we'd be able to do something like `uv run pyproject.toml ~/.config/uv/requirements.txt <actual python command here>` (dressed up in a shell alias for a minimum of typing).

Many thanks in advance for any help :)

### Platform

MacOS

### Version

uv 0.7.2 (481d05d8d 2025-04-30)

---

_Label `question` added by @patrick-kidger on 2025-04-30 22:46_

---

_Comment by @konstin on 2025-05-02 10:13_

The answer to your question depends on if those tools should be installed in the same venv (e.g., ruff doesn't need to, but ipykernel does) and if you want those dependencies locked together with the project dependencies, potentially influencing the resolved version. 

Some of these sound like cases for the task runner (https://github.com/astral-sh/uv/issues/5903) or tool installations. For others, dependency groups may be the right solution: Dependency groups are not installed by default (expect `dev`) and are not published with a package, but can be selected with `--group`. They are resolved with the project dependencies though and may influence the resolved versions. 

For jupyter specifically, we have a guide (https://docs.astral.sh/uv/guides/integration/jupyter/). There, the `ipykernel` package needs to be installed with the project, while jupyter can be installed and launched separately.

---

_Comment by @patrick-kidger on 2025-05-02 11:39_

Thanks for the response!

> The answer to your question depends on if those tools should be installed in the same venv

Yup, in general this is important. (E.g. pytest plugins)

> if you want those dependencies locked together with the project dependencies, potentially influencing the resolved version.

Ideally the extra dev dependencies would not influence resolution. But as a practical matter for us this isn't super important, as CI ensures consistent good behaviour across all developers.

> groups

These aren't really practical for the reasons described in my original post. ('These can't be expressed in the `pyproject.toml` file ...')

---

Anyway, since opening this issue I've actually found a different workaround:
```
uv run fish -c 'uv pip install foo bar && actual command here'
```

This isn't perfect as there's no way to express a constraint against the existing `uv.lock` (I've commented on https://github.com/astral-sh/uv/issues/9774 that it'd be nice to have this), but as above probably good enough for practical use cases!

I hope the above trick helps anyone else looking to support the same feature of per-developer dependencies.

---

_Comment by @konstin on 2025-05-02 12:52_

While there's definitely room for improvement, in my workflows I made good experiences with a `uv sync` initially and then `uv pip install`ing additional dev packages, including jupyter. I can still use `uv run` afterwards since it doesn't remove any of the packages, and `uv pip install` uses the installed packages as preferences.

---
