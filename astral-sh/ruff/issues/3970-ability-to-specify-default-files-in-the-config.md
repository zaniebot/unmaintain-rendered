```yaml
number: 3970
title: (🎁) Ability to specify default files in the config
type: issue
state: closed
author: KotlinIsland
labels:
  - configuration
assignees: []
created_at: 2023-04-14T01:24:13Z
updated_at: 2023-11-21T17:34:23Z
url: https://github.com/astral-sh/ruff/issues/3970
synced_at: 2026-01-10T11:09:46Z
```

# (🎁) Ability to specify default files in the config

---

_Issue opened by @KotlinIsland on 2023-04-14 01:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I want to be able to specify the default files within the configuration file so that I don't need to duplicate this config in different places.

```toml
[tool.ruff]
files = ["src", "scripts"]
```

Then it is possible to invoke ruff without passing any paths:
```
👉 ruff
yada yada yada
```

---

_Label `configuration` added by @charliermarsh on 2023-04-14 01:33_

---

_Comment by @charliermarsh on 2023-04-14 01:33_

I think this is pretty reasonable, Black supports this too and it's useful. @MichaReiser - any thoughts here?

---

_Comment by @KotlinIsland on 2023-04-14 05:40_

@charliermarsh How do you do it in Black? I can't find it anywhere...

---

_Comment by @charliermarsh on 2023-04-14 12:45_

I was mixed up -- Black doesn't support this, but [Mypy does](https://mypy.readthedocs.io/en/stable/config_file.html#confval-files).

---

_Comment by @JSchoeck on 2023-06-05 13:49_

[Edit]
I think what I wanted is already possible, I just did't get it when I read it the first time, see the [docs](https://beta.ruff.rs/docs/configuration/#pyprojecttoml-discovery)
For Windows, the $(config_dir) is equivalent to: %userprofile%\AppData\Roaming\
So putting a a pyproject.toml into %userprofile%\AppData\Roaming\ruff should set a global default and I confirm it works on my PC.

---

Do I understand the described use case correctly, that it should be possible to declare a pyproject.toml file as the global default, which overrides the built-in ruff default settings (but does not override project-specific pyproject.toml files)?

In that case, I agree very much that this feature is very helpful, even needed.

PS: Thanks for your development efforts, ruff is great!

---

_Closed by @zanieb on 2023-11-21 17:34_

---
