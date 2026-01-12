```yaml
number: 7836
title: "Install dev-dependencies before trying to resolve \"normal\" dependencies"
type: issue
state: open
author: vlad-ivanov-name
labels: []
assignees: []
created_at: 2024-10-01T12:54:58Z
updated_at: 2024-10-01T12:54:58Z
url: https://github.com/astral-sh/uv/issues/7836
synced_at: 2026-01-12T15:59:17Z
```

# Install dev-dependencies before trying to resolve "normal" dependencies

---

_@vlad-ivanov-name_

Consider the following `pyproject.toml`:

```
[project]
name = "uv-reproducer"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
  "my-private-package==1.0.0",
]

[tool.uv]
dev-dependencies = [
  "keyring",
  "keyrings.google-artifactregistry-auth",
]

keyring-provider = "subprocess"
extra-index-url = [
  "https://oauth2accesstoken@us-west1-python.pkg.dev/my-google-project/my-package-repo/simple/"
]
```

In order to install `my-private-package`, one needs access to the extra-index-url and keyring command. I haven't found a way to create a lockfile without installing those dependencies manually when there's no `uv.lock` yet.

Perhaps those kinds of "internal" uv dependencies could be pre-installed, or, to avoid messing with package's own requirements, they can go into their own "dev-tool only" venv, where uv can then pick up the keyring command from?

```
uv --version
uv 0.4.17 (Homebrew 2024-09-27)
```

---
