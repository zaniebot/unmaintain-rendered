```yaml
number: 11641
title: Integrate with pre-commit (or replace it)
type: issue
state: closed
author: cliebBS
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-02-19T21:08:42Z
updated_at: 2025-11-29T14:16:22Z
url: https://github.com/astral-sh/uv/issues/11641
synced_at: 2026-01-12T16:00:42Z
```

# Integrate with pre-commit (or replace it)

---

_@cliebBS_

### Summary

We currently use Poetry (though this applies to `uv` as well) for managing our dependencies along with `pre-commit` for hooking into Git to perform various code lints.  This generally works fine, but for `mypy` it becomes a huge pain since we need to have our dependencies installed as part of the virtualenv that mypy is installed into.  This results in us needing to replicate our frozen dependencies into the `additional_dependencies` section for `mypy` and to keep it in sync with what we have locked by Poetry.

This was painful enough that we created a script that scrapes the `poetry.lock` file, sees what packages are actually used in our code (to filter out transitive dependencies), tries to resolve type stub packages when the package itself doesn't provide type information, then checks the `.pre-commit-config.yaml` file to see if it matches what we want.  We run this as part of our CI to prevent Poetry and mypy from diverging in what dependencies we're using, and also have a mode that allows us to run the script and auto-update the `.pre-commit-config.yaml` file on a dev machine.

It would be amazing if there was a way that `uv` could provide the packages for tools like `mypy` run by `pre-commit` so that we could delete that script.  While `additional_dependencies` is actually just an argument list to `pip install`, `pre-commit` specifically needs to have the dependencies listed directly within its config file (instead of pulled in with `-r requirements.txt`) so that it knows when to create a new virtualenv with the updated contents since it doesn't understand `-r` or `-c` pull in external resources that it needs to cache in order to detect changes.  Additionally, the `pre-commit` devs refuse to add any kind of improvement in this area, wanting to keep configuration isolated to just their config file (not an unreasonable stance).

In order to fix this, we would either need some kind of `uv` pre-commit task that wraps other pre-commit plugins and that does environment setup itself, or we would need a way to run `uv` in "pre-commit emulation" mode where it can operate on `.pre-commit-config.yaml` files, but where an additional option like `install-uv-deps: true` or `install-requirements: [file_path]` is available on hooks that trigger a `uv sync` or `uv pip install -r ...` during environment setup.

As a bonus, a "pre-commit emulation" mode would allow all of the virtualenvs to be created using `uv`, greatly improving the speed at which environment setup occurs :D

### Example

`uv` adds a `pre-commit` emulation mode

1. In a repo working copy, run `uv pre-commit init`
2. `uv` installs relevant scripts into `.git/hooks` that invoke `uv pre-commit run [hook_name] [hook_args...]`
3. User runs `git commit`
4. Git runs the `pre-commit` hook, which invokes `uv pre-commit run pre-commit ...`
5. `uv` parses `.pre-commit-config.yaml`

```yaml
default_stages: [pre-commit]
default_install_hook_types:
  - pre-commit
default_language_version:
  python: python3.12
repos:
- repo: https://github.com/pre-commit/mirrors-mypy
  rev: v1.15.0
  hooks:
    - id: mypy
      use_uv_sync: true
```

6. `uv` downloads Python 3.12 using `uv python install 3.12` if version is not already present
7. Create new `uvx` environment named after the hook name, version, and hash of `uv.lock` file
8. Checkout `https://github.com/pre-commit/mirrors-mypy` at tag `v1.15.0`
9. Run `uv pip install setup.py`, targeting install at the tool venv
10. Run `uv sync`, targeting install at the tool venv
12. `uv` invokes mypy using command in `.pre-commit-hooks.yaml` file from `mirrors-mypy` repo
13. Hook completes successfully, allowing Git to commit the changes
14. User makes random changes, but does not change mypy or `uv.lock`
15. User runs `git commit`
16. `uv` detects that venv already exists for mypy with correct dependencies installed, invokes mypy
17. User makes dependency changes, generating changes to `uv.lock`
18. User runs `git commit`
19. `uv` generates new tool venv name due to changed hash of `uv.lock`, then builds it like specified above

---

_Label `enhancement` added by @cliebBS on 2025-02-19 21:08_

---

_Comment by @chrisrodrigue on 2025-02-19 21:44_

This could also be an opportunity to improve the [out-of-the-box hooks](https://github.com/pre-commit/pre-commit-hooks) for pre-commit. Using all of them can be quite slow and lead to developers minimizing the number that they use... seems like a great job for Rust. Zero(-ish) cost hooks for instant commits.

We might even squash the `.pre-commit-config.yaml` into the `pyproject.toml` under `[tool.uv.hooks]` while we're at it.

Also, IMO, pre-commit is somewhat of a misnomer because [one can actually use it for hooks other than `pre-commit`](https://pre-commit.com/#supported-git-hooks), such as `commit-msg` or `pre-push`, so maybe there could be a better name for this subcommand. 

Maybe [`uv hooks`](https://pre-commit.com/#command-line-interface)?
```console
uv hooks auto-update [options]
uv hooks clean [options]
uv hooks gc [options]
uv hooks init-templatedir DIRECTORY [options]
uv hooks install [options]
uv hooks install-hooks [options]
uv hooks migrate-config [options]
uv hooks run [hook-id] [options]
uv hooks sample-config [options]
uv hooks try-repo REPO [options]
uv hooks uninstall [option]
```

---

_Label `wish` added by @zanieb on 2025-02-19 21:50_

---

_Comment by @cliebBS on 2025-02-20 14:52_

I like the idea of moving the configuration into `pyproject.toml`, though we'd need to think about how to handle the duality of `.pre-commit-config.yaml` (configure hooks to run against this repository) and `.pre-commit-hooks.yaml` (hooks provided by this repository).

Also, 100% agree with this misnomer.  It makes documentation hard to search for as well, especially when you are trying to build hooks for phases other than pre-commit.  Perhaps `uv vcs-hooks` might be more informative since `hooks` on its own feels a little vague as to what you are hooking, and calling it `git-hooks` feels like it would be limiting from a design space if you wanted to support other VCS's in the future.

---

_Comment by @rafalkrupinski on 2025-03-27 14:26_

pre-commit does two main things:
1. manages tools
2. runs them with git hooks

uv already has tool manager, but AFAIK only for python tools, and there are other tools that handle git hooks, like lefthook.

My solution that I'm working on is lefthook + mise. Only problem I had so far is configuring lefthook to either include changes done by tools or fail commit when a change appears (work in progress)

---

_Comment by @cliebBS on 2025-09-11 15:31_

Looks like someone beat us to it.  There's a relatively new project called [prek](https://github.com/j178/prek) that is a drop-in replacement for `pre-commit`, but that is written in Rust and that uses `uv` under the covers to install tools instead of `pip`.

---

_Closed by @cliebBS on 2025-09-11 15:31_

---

_Comment by @rafalkrupinski on 2025-09-11 15:43_

so it's also python-only?

---

_Comment by @cliebBS on 2025-09-11 15:46_

It appears to support *some* other languages, just not everything that `pre-commit` supports currently.  We only use Python, so the lack of support for other languages doesn't impact us, though the shared workspace does for our mono-repo projects that also use MyPy, though apparently multi-workspace support will be added in 0.2.0.

---

_Comment by @rafalkrupinski on 2025-09-11 15:49_

hmm

> 🔄 Fully compatible with the original pre-commit configurations and hooks.

👍

---

_Comment by @Jamie-BitFlight on 2025-11-27 04:33_

This pain point resonates - keeping `additional_dependencies` in sync with your lockfile is tedious.

For the specific case of linting scripts with PEP 723 inline dependencies (or just wanting mypy/pyright to have access to project deps), I built a workaround tool called `pep723-loader`.

It wraps any linter and auto-installs dependencies before execution:

```yaml
- repo: local
  hooks:
    - id: mypy
      name: mypy
      entry: uv run -q --no-sync --with pep723-loader --with mypy pep723-loader mypy
      language: system
      types: [python]
      pass_filenames: true
```

How it works:
1. Scans Python files for PEP 723 `# /// script` metadata
2. Runs `uv export --script` to extract dependencies  
3. Installs them via `uv pip install`
4. Executes the wrapped linter

This doesn't solve the full "sync pre-commit deps from lockfile" problem you're describing, but it does eliminate the `additional_dependencies` dance for PEP 723 scripts and lets type checkers see the deps they need.

PyPI: `pip install pep723-loader`
Repo: https://github.com/bitflight-devops/pre-commit-pep723-linter-wrapper

---
