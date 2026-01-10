---
number: 16621
title: Can i set a default config file path?
type: issue
state: closed
author: Ada-lave
labels:
  - question
assignees: []
created_at: 2025-03-11T11:11:58Z
updated_at: 2025-03-18T07:44:49Z
url: https://github.com/astral-sh/ruff/issues/16621
synced_at: 2026-01-10T01:22:57Z
---

# Can i set a default config file path?

---

_Issue opened by @Ada-lave on 2025-03-11 11:11_

### Question

Is there any way I can set the path to the default config file if it doesn't find it locally?

### Version

ruff 0.9.10

---

_Label `question` added by @Ada-lave on 2025-03-11 11:11_

---

_Comment by @dhruvmanila on 2025-03-11 11:36_

Refer to point (3) in https://docs.astral.sh/ruff/configuration/#config-file-discovery.

---

_Comment by @Ada-lave on 2025-03-12 03:58_

If I am using Ubuntu, the ${config_dir}/ruff/pyproject.toml file will be located at /etc/ruff/pyproject.toml.


---

_Comment by @ntBre on 2025-03-12 21:04_

`/etc` is usually only for default config files, and at least on Arch, the ruff package doesn't put anything there. I'm not sure about Ubuntu. 

You should also be able to put a config file in `$XDG_CONFIG_HOME/ruff/pyproject.toml`, where `$XDG_CONFIG_HOME` should also default to `$HOME/.config` if it's not set. So for me that would be `/home/brent/.config/ruff/pyproject.toml`.

This should also work for `ruff.toml` and `.ruff.toml` as the docs say a little farther down.

Another option that might help here is that you can specify the path to any config file with the `--config` command line option.

---

_Closed by @Ada-lave on 2025-03-18 07:44_

---
