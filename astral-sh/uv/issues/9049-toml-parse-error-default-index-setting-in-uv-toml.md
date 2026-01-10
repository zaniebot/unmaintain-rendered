---
number: 9049
title: "TOML parse error: `default-index` setting in uv.toml"
type: issue
state: closed
author: idling-mind
labels:
  - question
assignees: []
created_at: 2024-11-12T09:22:58Z
updated_at: 2024-11-12T18:04:10Z
url: https://github.com/astral-sh/uv/issues/9049
synced_at: 2026-01-10T01:24:36Z
---

# TOML parse error: `default-index` setting in uv.toml

---

_Issue opened by @idling-mind on 2024-11-12 09:22_

Hi,
  Below is my uv.toml file.

```toml
default-index = "https://pypackages:***@tfsprod.internal/DefaultCollection/_packaging/pypackages/pypi/simple/"
index = "https://ett_packages:***@tfsprod.internal/DefaultCollection/ETT/_packaging/ett_packages/pypi/simple/"
native-tls = true
```

when I run any uv command, i throws me the following error.

```
error: Failed to parse: `uv.toml`
  Caused by: TOML parse error at line 1, column 1
  |
1 | default-index = "https://pypackages:***@tfsprod.internal/DefaultCollection/_packaging/pypackages/pypi/simple/"
  | ^^^^^^^^^^^^^
unknown field `default-index`, expected one of `native-tls`, `offline`, `no-cache`, `cache-dir`, `preview`, `python-preference`, `python-downloads`, `concurrent-downloads`, `concurrent-builds`, `concurrent-installs`, `index`, `index-url`, `extra-index-url`, `no-index`, `find-links`, `index-strategy`, `keyring-provider`, `allow-insecure-host`, `resolution`, `prerelease`, `dependency-metadata`, `config-settings`, `no-build-isolation`, `no-build-isolation-package`, `exclude-newer`, `link-mode`, `compile-bytecode`, `no-sources`, `upgrade`, `upgrade-package`, `reinstall`, `reinstall-package`, `no-build`, `no-build-package`, `no-binary`, `no-binary-package`, `publish-url`, `trusted-publishing`, `pip`, `cache-keys`, `override-dependencies`, `constraint-dependencies`, `environments`, `workspace`, `sources`, `managed`, `package`, `default-groups`, `dev-dependencies`
```

However, if I set `UV_DEFAULT_INDEX` env variable, it works fine.

I'm having uv v0.5.1

regards

ps: Thanks for this great tool!!


---

_Comment by @charliermarsh on 2024-11-12 14:58_

The syntax in TOML is like this:

```toml
[[tool.uv.index]]
url = "https://pypackages:***@tfsprod.internal/DefaultCollection/_packaging/pypackages/pypi/simple/"
default = true

[[tool.uv.index]]
url = "https://ett_packages:***@tfsprod.internal/DefaultCollection/ETT/_packaging/ett_packages/pypi/simple/"
```

---

_Label `question` added by @charliermarsh on 2024-11-12 14:58_

---

_Comment by @idling-mind on 2024-11-12 15:09_

Isn't the content you specified for `pyproject.toml`? will that work in `uv.toml`? I was trying to create it in similar way as shown in the example here. https://docs.astral.sh/uv/configuration/files/#configuration-files

```
index-url = "https://test.pypi.org/simple"
```

---

_Comment by @charliermarsh on 2024-11-12 17:15_

In `uv.toml`, you would just drop the `[[tool.uv` piece:

```toml
[[index]]
url = "https://pypackages:***@tfsprod.internal/DefaultCollection/_packaging/pypackages/pypi/simple/"
default = true

[[index]]
url = "https://ett_packages:***@tfsprod.internal/DefaultCollection/ETT/_packaging/ett_packages/pypi/simple/"
```

---

_Comment by @charliermarsh on 2024-11-12 17:15_

Alternatively, you can do:

```toml
index-url = "https://pypackages:***@tfsprod.internal/DefaultCollection/_packaging/pypackages/pypi/simple/"
extra-index-url = "https://ett_packages:***@tfsprod.internal/DefaultCollection/ETT/_packaging/ett_packages/pypi/simple/"
native-tls = true
```

But those fields are considered legacy, and the `[[index]]` syntax is preferred.

---

_Referenced in [astral-sh/uv#9065](../../astral-sh/uv/pulls/9065.md) on 2024-11-12 17:31_

---

_Comment by @idling-mind on 2024-11-12 18:04_

Thanks! using `[[index]]` format worked for me and I prefer that one! :)

---

_Closed by @idling-mind on 2024-11-12 18:04_

---
