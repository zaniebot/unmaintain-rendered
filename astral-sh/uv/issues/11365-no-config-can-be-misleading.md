```yaml
number: 11365
title: "--no-config  can be misleading"
type: issue
state: open
author: KRRT7
labels:
  - documentation
assignees: []
created_at: 2025-02-10T00:40:07Z
updated_at: 2025-08-08T14:12:37Z
url: https://github.com/astral-sh/uv/issues/11365
synced_at: 2026-01-10T03:32:45Z
```

# --no-config  can be misleading

---

_Issue opened by @KRRT7 on 2025-02-10 00:40_

### Summary

uv run --no-config --with rich main.py
error: No `project` table found in: `C:\Users\NuitkaDevOps\Desktop\pyproject.toml`

i thought that --no-config would've prevented the project error

as per the doc:
```
      --no-config                                  Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) [env: UV_NO_CONFIG=]
```

this tells me that it shouldnt even try to find and error after finding the desktop pyproject.toml


but after some help from @zanieb it appears that this behavior can be true or false depending on the context; perhaps the documentation could be enhanced for this situation.

### Example

_No response_

---

_Label `enhancement` added by @KRRT7 on 2025-02-10 00:40_

---

_Comment by @charliermarsh on 2025-02-10 00:48_

You might be looking for `--no-project`?

---

_Comment by @KRRT7 on 2025-02-10 01:24_

> You might be looking for `--no-project`?

I was indeed, zane helped me out in the discord:
https://discord.com/channels/1039017663004942429/1039017663512449056/1338305769723924592

---

_Label `enhancement` removed by @zanieb on 2025-02-10 02:54_

---

_Label `documentation` added by @zanieb on 2025-02-10 02:54_

---

_Comment by @jonas-hagen on 2025-02-25 09:27_

I am too puzzled by that:

```
[user@host]$ ls -a | cat
requirements.txt
.venv  # <-- created by uv venv

[user@host]$ uv pip install -r requirements.txt 
Audited 8 packages in 27ms

[user@host]$ uv lock --no-config
error: No `pyproject.toml` found in current directory or any parent directory

[user@host]$ uv lock --no-project
error: unexpected argument '--no-project' found

  tip: a similar argument exists: '--project'

Usage: uv lock --project <PROJECT>

For more information, try '--help'.

[user@host]$ uv --version
uv 0.6.3
```

Is it in the end not possible to create a `uv.lock` file from just a `requirements.txt` (which contains a few lines, but no dependencies or versions).

The discord link is broken, so I do not understand where the `--no-project` option could be useful in this context.

---

_Comment by @KRRT7 on 2025-02-25 09:29_

@jonas-hagen here's a temp invite while the discord link gets fixed: https://discord.gg/jJD58zhQ

---

_Comment by @jonas-hagen on 2025-02-25 10:06_

Thanks, now I can access discord.

My usecase: I have a bunch of quick and dirty scripts and would like to use uv to manage my environment, because I like the speed and ergonomics. The alternative is a requirements and constraints (or .in and .txt) file and use `uv pip` like I would use pip. This would work perfectly fine, even tough I am a bit sad to miss out on `uv.lock`.

My question basically is: What does the `--no-config` option actually do?

---

_Comment by @zanieb on 2025-02-26 19:13_

It will avoid reading uv settings from `pyproject.toml` and `uv.toml` files when you are not in a project.

---

_Comment by @ahundt on 2025-08-08 04:58_

no config should stop going up the directory tree, in many environments such as security sandboxes going out of the permitted area crashes uv. UV needs to be robust to being denied access to config files, plus UV_NO_CONFIG=1 works but --no-config does not it still searches for configs. it is necessary and fixing --no-config to be consistent with the env var and robust to being denied access to directories when searching for configs would be very helpful.

---

_Comment by @zanieb on 2025-08-08 14:12_

@ahundt please open a new issue for your permission errors.

Separately, `--no-config` and `UV_NO_CONFIG=1` should be exactly equivalent. They're parsed by clap and propagated throughout uv as though they were identical https://github.com/astral-sh/uv/blob/84d57f2ee9bb6f452e3be74c658b2e0096b1dc30/crates/uv-cli/src/lib.rs#L130-L135. Please open a new issue with a clear reproduction of that behavior.

---
