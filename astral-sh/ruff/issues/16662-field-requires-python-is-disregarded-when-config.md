```yaml
number: 16662
title: "Field `requires-python` is disregarded when config lives outside `pyproject.toml`"
type: issue
state: closed
author: ofek
labels:
  - configuration
assignees: []
created_at: 2025-03-12T05:01:23Z
updated_at: 2025-03-13T19:50:32Z
url: https://github.com/astral-sh/ruff/issues/16662
synced_at: 2026-01-12T15:54:55Z
```

# Field `requires-python` is disregarded when config lives outside `pyproject.toml`

---

_@ofek_

### Summary

Create a directory with the following files:

<table>
<tr><th><code>pyproject.toml</code></th><th><code>ruff.toml</code></th><th><code>test.py</code></th></tr>
<tr>
<td>

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "dda"
version = "0.0.1"
requires-python = ">=3.12"

# Comment this out
[tool.ruff.lint]
select = ["I001"]
ignore = ["T201"]
```

</td>
<td>

```toml
# Uncomment
# [lint]
# select = ["I001"]
# ignore = ["T201"]
```

</td>
<td>

```python
from __future__ import annotations

import tomllib

import requests


def main():
    print(tomllib)
    print(requests)


if __name__ == "__main__":
    main()
```

</td>
</tr>
</table>

Then run `ruff check .` and notice success. Then use the dedicated `ruff.toml` config file instead and notice that it tries to re-sort the imports because it did not properly read the version from `pyproject.toml`.

### Version

0.9.9

---

_Comment by @dhruvmanila on 2025-03-12 05:11_

This is a duplicate of https://github.com/astral-sh/ruff/issues/14813, here's the PR that should fix this https://github.com/astral-sh/ruff/pull/16319.

In the interim, you can include an empty `[tool.ruff]` heading in the `pyproject.toml` file which will prompt Ruff to read the `requires-python` field.

---

_Closed by @dhruvmanila on 2025-03-12 05:11_

---

_Label `question` added by @dhruvmanila on 2025-03-12 05:11_

---

_Comment by @ofek on 2025-03-12 05:17_

That does not work.

---

_Reopened by @dhruvmanila on 2025-03-12 05:21_

---

_Comment by @dhruvmanila on 2025-03-12 05:44_

Sorry, that workaround would not work in this case because you're using both `pyproject.toml` and `ruff.toml`. You can see this in the debug logs:

```sh
# ruff check -v
[2025-03-12][11:08:22][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
```

I think the linked PR should fix this issue but in my testing it doesn't seem to be:
```console
$ ~/work/astral/ruff/target/debug/ruff check -v --no-cache .
[2025-03-12][11:12:07][ruff::resolve][DEBUG] Using configuration file (via parent) at: /private/tmp/ruff-test/ruff.toml
[2025-03-12][11:12:07][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-03-12][11:12:07][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.12
[2025-03-12][11:12:07][ruff_workspace::pyproject][DEBUG] Derived `target-version` from `requires-python` in `pyproject.toml`: Py312
[2025-03-12][11:12:07][ignore::gitignore][DEBUG] opened gitignore file: /Users/dhruv/.ignore
[2025-03-12][11:12:07][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-03-12][11:12:07][ignore::gitignore][DEBUG] opened gitignore file: /private/tmp/ruff-test/.venv/.gitignore
[2025-03-12][11:12:07][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/private/tmp/ruff-test/.venv"
[2025-03-12][11:12:07][ruff_workspace::resolver][DEBUG] Included path via `include`: "/private/tmp/ruff-test/test.py"
[2025-03-12][11:12:07][ruff_workspace::resolver][DEBUG] Included path via `include`: "/private/tmp/ruff-test/pyproject.toml"
[2025-03-12][11:12:07][ignore::gitignore][DEBUG] opened gitignore file: /private/tmp/ruff-test/.ruff_cache/.gitignore
[2025-03-12][11:12:07][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/private/tmp/ruff-test/.ruff_cache"
[2025-03-12][11:12:07][ruff::commands::check][DEBUG] Identified files to lint in: 9.338125ms
[2025-03-12][11:12:07][ruff::diagnostics][DEBUG] Checking: /private/tmp/ruff-test/test.py
[2025-03-12][11:12:07][ruff::diagnostics][DEBUG] Checking: /private/tmp/ruff-test/pyproject.toml
[2025-03-12][11:12:07][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'requests' as Known(ThirdParty) (NoMatch)
[2025-03-12][11:12:07][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tomllib' as Known(ThirdParty) (NoMatch)
[2025-03-12][11:12:07][ruff_linter::rules::isort::categorize][DEBUG] Categorized '__future__' as Known(Future) (Future)
[2025-03-12][11:12:07][ruff::commands::check][DEBUG] Checked 2 files in: 837.5µs
test.py:1:1: I001 [*] Import block is un-sorted or un-formatted
  |
1 | / from __future__ import annotations
2 | |
3 | | import tomllib
4 | |
5 | | import requests
  | |_______________^ I001
  |
  = help: Organize imports
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

cc @dylwil3 

---

_Comment by @dhruvmanila on 2025-03-12 05:48_

The actual workaround would be to use the [`target-version`](https://docs.astral.sh/ruff/settings/#target-version) config which does mean that the information is duplicated but we're working towards a fix for it.

```toml
target-version = "py312"

[lint]
select = ["I001"]
ignore = ["T201"]
```

---

_Label `question` removed by @dhruvmanila on 2025-03-12 05:48_

---

_Label `configuration` added by @dhruvmanila on 2025-03-12 05:48_

---

_Comment by @dylwil3 on 2025-03-12 10:32_

Investigating the issue at the linked PR now. Something must be _resetting_ the `target-version` based on those logs - very glad you caught this! Even more unsettling: if you directly pass the file path in then you get the expected behavior:

```console
❯ ../target/debug/ruff check --no-cache -v test.py
[2025-03-12][05:29:39][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/dmbp/Documents/dev/contrib/ruff/tmp/ruff.toml
[2025-03-12][05:29:39][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-03-12][05:29:39][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.12
[2025-03-12][05:29:39][ruff_workspace::pyproject][DEBUG] Derived `target-version` from `requires-python` in `pyproject.toml`: Py312
[2025-03-12][05:29:39][ruff::commands::check][DEBUG] Identified files to lint in: 6.826875ms
[2025-03-12][05:29:39][ruff::diagnostics][DEBUG] Checking: /Users/dmbp/Documents/dev/contrib/ruff/tmp/test.py
[2025-03-12][05:29:39][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'requests' as Known(ThirdParty) (NoMatch)
[2025-03-12][05:29:39][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tomllib' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-03-12][05:29:39][ruff_linter::rules::isort::categorize][DEBUG] Categorized '__future__' as Known(Future) (Future)
[2025-03-12][05:29:39][ruff::commands::check][DEBUG] Checked 1 files in: 1.247917ms
All checks passed!
```

---

_Closed by @MichaReiser on 2025-03-13 11:51_

---

_Comment by @amoralesc on 2025-03-13 17:01_

Hey @dylwil3. Since you seem to be working on this issue, and seeing your [commit](https://github.com/astral-sh/ruff/commit/15c05356f4631fbc739cbfa552e22a34ef1766cc), I would like to ask about how would the Python version be resolved for Ruff in this situation.

I have a `pyproject.toml` file with the `requires-python` property defined at the root of the project. I then have a `.ruff.toml` configuration file **without** the `target-version` property under a `.config` directory. So essentially:

```plaintext
.
├── .config/
│   └── .ruff.toml
└── pyproject.toml
```

I then run Ruff like this (from the project root):

```bash
ruff --config .config/.ruff.toml check .
```

Your commit suggests that if I specify that the config file is under `.config`, it means that it will look for the `pyproject.toml` under the same directory. However, in this situation, I don't have my custom configuration file under the project root (to not clutter it).

---

_Comment by @dylwil3 on 2025-03-13 17:13_

Hi @amoralesc that's a good question!

If you pass a config file in directly via `--config` then none of the new behavior is triggered, so `ruff` would use the default `target-version` (which is currently 3.9) in this case. This is meant to be more consistent with the current behavior: if a user passes in a config file directly, then we don't do any further config file discovery or hierarchical resolution, we just use the config file passed in and use the current working directory as the "root" of the project.

---

_Comment by @amoralesc on 2025-03-13 17:24_

I know this is probably too much too ask, and perhaps out of scope for Ruff, but could it ever support this behavior where the `requires-python` version lives on the `pyproject.toml`, but the config file is specified by command line AND they don't share the same directory?

And to not break current behavior, perhaps with a command line flag like `--python-version-file` or something of that line?

Of course I would understand if this is out of scope for the project because it is a very specific use-case of having different paths for Python config files.

**EDIT:** My specific use-case is to reduce the places where I have to remember to change the Python version when it is upgraded on a project. It's not a project-ending problem, but it is quite a headache to remember all the places where the Python version gets pinned on a project (config files, version files, Dockerfiles, etc.)

---

_Comment by @dylwil3 on 2025-03-13 17:34_

It's an interesting idea, and I don't know offhand if users would find it more or less confusing to use the behavior you describe.

Could you help me understand a bit more about your situation? In particular, I'm curious why you wouldn't want to put your configuration in the `[tool.ruff]` section of the `pyproject.toml`. That wouldn't clutter your directory. Is the issue that you want to have a local configuration that differs from a shared configuration?

---

_Comment by @amoralesc on 2025-03-13 19:50_

I don't love placing the Ruff configuration in the `[tool.ruff]` section mostly because I like to have separate configuration files for each of the tools that I'm using on my development environment (and if possible and the tool supports it, under a `.config` directory).

Most of the Python tools that I run on a Python project (`uv`, `ruff`, `mypy`) will indeed allow me to configure them directly on the `pyproject.toml`. However, I may use other tools on my env (specially on monorepos), like Prettier, ESLint, ShellCheck, hadolint, just to name a few. All of those tools do use their own configuration file, and they also allow me the flexibility of placing them under a `.config` a directory, just like Ruff also does.

So this is mostly a preference thing. I like having all my development tools configured by their own standalone configuration files. This also makes it easier to document the development tools at the README or wherever I'm documenting the project. For example:

```markdown
- Hey, look at `.config/.ruff.toml` to change how Ruff works!
- You can also change Prettier's behavior at `.config/.prettierrc.yml`!
- `uv` manages Ruff's versioning at `pyproject.toml`, and NPM manages Prettier's versioning at `package.json`.
```

---
