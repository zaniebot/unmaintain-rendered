---
number: 8523
title: uv tool upgrade - problem with private indexes due to short living credentials
type: issue
state: open
author: delicb
labels:
  - bug
  - help wanted
  - uv tool
assignees: []
created_at: 2024-10-24T11:02:46Z
updated_at: 2025-07-01T07:45:40Z
url: https://github.com/astral-sh/uv/issues/8523
synced_at: 2026-01-07T13:12:17-06:00
---

# uv tool upgrade - problem with private indexes due to short living credentials

---

_Issue opened by @delicb on 2024-10-24 11:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I am using `uv tool install` to install some CLI tools from private index (CodeArtifact). 

When new version is released, `uv tool upgrade`  generally fails. I have a mechanism that refreshes CodeArtifact index URL with new credentials and stores them in `~/.config/uv/uv.toml`, but when tools are installed, index used is stored in `uv-receipt.toml` as far as I can tell and `upgrade` is trying to use that again. It usually fails, because those credentials are good only for couple hours. 

Workaround is `uv tool uninstall <tool> && uv tool install <tool>`. 

Should upgrade use globally defined index if stored one fails? Maybe only if domain is the same (but ignore credentials)? Not sure what the best approach is here, but it would be good for upgrade to work. 

Happy to provide any additional details if needed.

```shell
❯ uv --version
uv 0.4.26 (1b9b9d56b 2024-10-23)
```

---

_Label `bug` added by @zanieb on 2024-10-24 14:41_

---

_Label `uv tool` added by @zanieb on 2024-10-24 14:41_

---

_Comment by @zanieb on 2024-10-24 14:41_

Thanks for the report! I'm not sure what the best solution here is, seems tricky to know which credentials we should prefer.

---

_Comment by @delicb on 2024-10-24 20:16_

Maybe do what `pipx` does, which is to use `pip` config at the moment of command invocation (I haven't read the code, but behaviour seems like that, since updating `pip.conf` has that effect on `pipx` as well). 

In this case, its not even a different tool (like `pip` and `pipx`), its all `uv` and `uv add` (for example) works with values from config file while `uv tool` only sometimes uses same values. I am not sure what is the purpose of storing index in `receipt.toml`, so I am not sure if suggestion make sense at all. 

---

_Comment by @delicb on 2024-11-09 13:01_

My current workaround is this script:

```python
#!/usr/bin/env uv run
# /// script
# requires-python = ">=3.13"
# dependencies = [
#    "tomlkit",
# ]
# ///

from pathlib import Path

import tomlkit


def main() -> None:
    tools = Path("~/.local/share/uv/tools").expanduser()
    recipes = tools.glob("*/uv-receipt.toml")
    current_index_url = get_current_index()

    for r in recipes:
        with open(r, "rb") as f:
            rec = tomlkit.parse(f.read())

        rec.setdefault("tool", {}).setdefault("options", {})
        rec["tool"]["options"]["index-url"] = current_index_url

        with open(r, "w") as f:
            tomlkit.dump(rec, f)


def get_current_index():
    with open(Path("~/.config/uv/uv.toml").expanduser(), "rb") as f:
        uv_config = tomlkit.parse(f.read())
    return uv_config["index-url"]


if __name__ == "__main__":
    main()
```

Actual `get_current_index` is a bit more complicated, because it generates URL by generating authorization token from AWS and constructs URL, like I do to update `~/.config/uv/uv.toml` as well, but for me that is part of larger CLI, this is good enough approximation. 

---

_Comment by @zanieb on 2024-11-11 15:02_

>  I am not sure what is the purpose of storing index in receipt.toml, so I am not sure if suggestion make sense at all.

We store the index in the receipt so that if you do something like `uv tool install --index-url ...` a subsequent `uv tool upgrade` operation will use the correct index.

---

_Comment by @delicb on 2024-11-11 15:32_

Ah, ok. In that case, maybe it can be stored only if one from config file was overridden via CLI flag? Not sure, but it seems wrong to store credentials there anyway, especially ones that have short lifespan. 

---

_Comment by @zanieb on 2024-11-11 16:12_

Unfortunately it's a pain to distinguish between values from the CLI and config file (due to the way we've implemented it), but yeah I think that would make sense.

---

_Comment by @charliermarsh on 2024-11-11 16:13_

Indexes are actually the one thing where we _can_ differentiate between CLI and elsewhere, we track the origin.

---

_Comment by @zanieb on 2024-11-11 16:14_

Ah great, I knew you added that for something :)

---

_Label `help wanted` added by @zanieb on 2024-11-11 16:14_

---

_Comment by @charliermarsh on 2024-11-11 16:18_

(N.B. that I didn't read the full discussion here before commenting, just your last comment.)

---

_Comment by @idan-rahamim-lendbuzz on 2025-07-01 07:45_

Hi, is there any built-in workaround for this now, or is uninstall + install still the recommended way to handle upgrades with expiring credentials?

---
