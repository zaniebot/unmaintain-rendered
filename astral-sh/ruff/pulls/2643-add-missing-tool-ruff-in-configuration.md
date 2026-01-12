```yaml
number: 2643
title: "Add missing `tool.ruff` in configuration documentation."
type: pull_request
state: closed
author: Chris-May
labels: []
assignees: []
base: main
head: docs/fix-missing-config
created_at: 2023-02-07T22:45:46Z
updated_at: 2023-02-08T18:41:32Z
url: https://github.com/astral-sh/ruff/pull/2643
synced_at: 2026-01-12T15:55:09Z
```

# Add missing `tool.ruff` in configuration documentation.

---

_@Chris-May_

The documentation includes a configuration example that shows one can configure ruff's per-file ignore patterns by adding the following section to `pyproject.toml`:

```
# Ignore `E402` (import violations) in all `__init__.py` files, and in `path/to/file.py`.
[per-file-ignores]
"__init__.py" = ["E402"]
"path/to/file.py" = ["E402"]
```

It should be `[tool.ruff.per-file-ignores]`

This one caused me a few hours of issues, so I decided to contribute a fix.

---

_Comment by @charliermarsh on 2023-02-07 23:21_

Confusingly, this is actually correct, because that section is talking about `ruff.toml` (see `For example, the
`pyproject.toml` described above would be represented via the following `ruff.toml`...`) in the paragraph leading up to it.

If you scroll up in the docs, you'll see that we do include the `pyproject.toml` version in full:

```toml
[tool.ruff]
# Enable flake8-bugbear (`B`) rules.
select = ["E", "F", "B"]

# Never enforce `E501` (line length violations).
ignore = ["E501"]

# Avoid trying to fix flake8-bugbear (`B`) violations.
unfixable = ["B"]

# Ignore `E402` (import violations) in all `__init__.py` files, and in `path/to/file.py`.
[tool.ruff.per-file-ignores]
"__init__.py" = ["E402"]
"path/to/file.py" = ["E402"]
```

I'll try to make this clearer. I'm really sorry that you lost time to it.

---

_Closed by @charliermarsh on 2023-02-08 02:39_

---

_Comment by @Chris-May on 2023-02-08 14:36_

Ah! Thanks for the clarity!!!

I remember reading about the `ruff.toml` file, but when I was scanning the documentation, those sections ran together.

Would you want to make that more explicit? I can think of two options:

1) Add a subheading

```
#### Configure via `ruff.toml`

As an alternative to pyproject.toml, Ruff will also respect a ruff.toml file, which implements an equivalent schema (though the [tool.ruff] hierarchy can be omitted). For example, the pyproject.toml described above would be represented via the following ruff.toml:

# Enable flake8-bugbear (`B`) rules.
select = ["E", "F", "B"]

# Never enforce `E501` (line length violations).
ignore = ["E501"]

# Avoid trying to fix flake8-bugbear (`B`) violations.
unfixable = ["B"]

# Ignore `E402` (import violations) in all `__init__.py` files, and in `path/to/file.py`.
[per-file-ignores]
"__init__.py" = ["E402"]
"path/to/file.py" = ["E402"]
```


2) Add a comment:


As an alternative to pyproject.toml, Ruff will also respect a ruff.toml file, which implements an equivalent schema (though the [tool.ruff] hierarchy can be omitted). For example, the pyproject.toml described above would be represented via the following ruff.toml:

```
# In ruff.toml

# Enable flake8-bugbear (`B`) rules.
select = ["E", "F", "B"]

# Never enforce `E501` (line length violations).
ignore = ["E501"]

# Avoid trying to fix flake8-bugbear (`B`) violations.
unfixable = ["B"]

# Ignore `E402` (import violations) in all `__init__.py` files, and in `path/to/file.py`.
[per-file-ignores]
"__init__.py" = ["E402"]
"path/to/file.py" = ["E402"]
```


Additionally, the intro paragraph to the section mentions that there are two ways to configure `ruff`, those being `pyproject.toml` and the command line. It seems like the `ruff.toml` would be a third.

If you're open to it, I'd be happy to update this PR with an option. Otherwise, I can happily let you focus on more pressing topics.  😃

---

_Comment by @charliermarsh on 2023-02-08 18:41_

That'd be great! I'd be happy with Option 1 (and adding `ruff.toml` to the intro paragraph).

---
