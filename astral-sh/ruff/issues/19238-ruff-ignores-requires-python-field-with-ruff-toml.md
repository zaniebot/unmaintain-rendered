```yaml
number: 19238
title: "Ruff ignores requires-python field with `ruff.toml` in %userprofile% on Windows"
type: issue
state: open
author: orishamir
labels:
  - documentation
assignees: []
created_at: 2025-07-09T16:41:06Z
updated_at: 2025-07-15T18:48:07Z
url: https://github.com/astral-sh/ruff/issues/19238
synced_at: 2026-01-10T11:09:59Z
```

# Ruff ignores requires-python field with `ruff.toml` in %userprofile% on Windows

---

_Issue opened by @orishamir on 2025-07-09 16:41_

### Summary

Hey!
While working on a project on **Windows**, I have come across the following issue:
I have a Python project with e.g. requires-python field `>=3.12` (`uv init --lib -p 3.12`)
Then, I have a `ruff.toml` inside %UserProfile% with only the following contents:
> C:\Users\user\ruff.toml
```toml
[lint]
select = ["I"]
# Notice target-version is not specified
```
Then, running `uvx ruff check --show-settings` gives:
```toml
...
...
analyze.target_version = 3.9
...
```
Which is the default for Ruff.
The bug does not reproduce on Linux:
```Dockerfile
FROM ghcr.io/astral-sh/uv:0.7.19-python3.12-bookworm-slim
WORKDIR /app

RUN uv init --lib -p 3.12

COPY <<EOF /root/.config/ruff/ruff.toml
[lint]
select = [
    "I"
]
EOF

CMD ["/bin/bash", "-c", "uvx ruff check --show-settings | grep analyze.target_version"]
# correctly outputs `analyze.target_version = 3.12`
```
And another note: when placing the `ruff.toml` inside the project directory, the detected Python version is correct

It seems this issue has been solved for linux in https://github.com/astral-sh/ruff/issues/16662
Indeed, adding an empty `[tool.ruff]` as suggested in https://github.com/astral-sh/ruff/issues/16662#issuecomment-2716487692 prompts Ruff to read the `requires-python` field

Thank you for the help, and thank you for the amazing work on Ruff and Ty!!

### Version

ruff 0.12.2

---

_Comment by @ntBre on 2025-07-09 19:09_

Thanks for the report and for the kind words!

I'm not that familiar with config directories on Windows, but based on our [docs](https://docs.astral.sh/ruff/configuration/#config-file-discovery), we should be using [`etcetera`'s native strategy](https://docs.rs/etcetera/latest/etcetera/#native-strategy) for config file discovery, which I think should correspond to `%userprofile%\AppData\Roaming\ruff`, if I'm following the `etcetera` [examples](https://docs.rs/etcetera/latest/etcetera/base_strategy/struct.Windows.html) and the docs on [`home_dir`](https://docs.rs/home/latest/home/fn.home_dir.html) correctly. I think this also corresponds to the `%APPDATA%` environment variable.

---

_Label `question` added by @ntBre on 2025-07-09 19:09_

---

_Comment by @orishamir on 2025-07-09 19:56_

@ntBre 
hmmm. Indeed, when I place `ruff.toml` in `%userprofile%\AppData\Roaming\ruff`, the config file is respected, and specifically the requires-python field gets correctly identified. But when I place it in `%userprofile%`, still the `ruff.toml` gets respected (in terms of `[lint].select` for example), but not the requires-python field. So it's weird, if I put `ruff.toml` in `%userprofile%`, then only part of it is respected?

---

_Comment by @ntBre on 2025-07-09 20:21_

Sorry, I focused on the wrong part of the report! I think I have an idea of what's going on, but maybe @dylwil3 can confirm. Is your project in a subdirectory of `%userprofile%`? I can reproduce this on Linux with the following example:

```shell
$ echo 'lint.select = ["I"]' > ruff.toml
$ uv init example
Initialized project `example` at `/home/clod/example`
$ cd example
$ ruff check -v --show-settings | grep analyze.target_version
[2025-07-09][20:13:42][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/clod/ruff.toml
[2025-07-09][20:13:42][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-07-09][20:13:42][ruff_workspace::pyproject][DEBUG] No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
[2025-07-09][20:13:42][ignore::gitignore][DEBUG] opened gitignore file: /home/clod/example/.gitignore
[2025-07-09][20:13:42][ignore::gitignore][DEBUG] opened gitignore file: /home/clod/example/.git/info/exclude
[2025-07-09][20:13:42][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/example/main.py"
[2025-07-09][20:13:42][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/clod/example/.git"
[2025-07-09][20:13:42][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/example/pyproject.toml"
analyze.target_version = 3.9
```

I think we will check for config files in the parent of the current directory, and that is overriding the `pyproject.toml` file. Your real config dir in `AppData` is treated differently.

That feels like kind of surprising behavior to me, but Dylan will know a lot more about that than I do.

---

_Comment by @dylwil3 on 2025-07-09 21:06_

Yes I think there is an implicit assumption that the user config is not in the same hierarchy as your local project. I'm not sure of a way around this, or what a more ideal behavior might be given that Ruff supports hierachical/cascading configurations?

I'm not able to try to reproduce this right now, but if either of you have a moment could you see whether the target version is resolved as expected for the files _inside_ the project? 

Like, in the setup above, I'd be curious to know the result of

```
ruff check -v --show-settings main.py | grep version
```

---

_Comment by @ntBre on 2025-07-09 22:29_

```
$ ruff check -v --show-settings main.py | grep version
[2025-07-09][22:11:27][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/clod/ruff.toml
[2025-07-09][22:11:27][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-07-09][22:11:27][ruff_workspace::pyproject][DEBUG] No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
linter.unresolved_target_version = none
linter.per_file_target_version = {}
formatter.unresolved_target_version = 3.9
formatter.per_file_target_version = {}
analyze.target_version = 3.9
```

It did feel a bit surprising to ignore the `requires-python` in the `pyproject.toml` in the current directory, but that's consistent with our very first rule of config file discovery:

> 1. In locating the "closest" pyproject.toml file for a given path, Ruff ignores any pyproject.toml files that lack a [tool.ruff] section.

So I think everything is working as expected, and the two fixes mentioned earlier in the thread are still recommended:
- Add an empty `[tool.ruff]` section to `pyproject.toml` so it's not skipped
- Or move `ruff.toml` to the expected global config directory where it's not a parent of the project

---

_Comment by @orishamir on 2025-07-13 21:29_

Perhaps I can add to the documentation a more explicit note about where Linux/Windows configuration files should be located? In uv docs, it's said very explicitly

---

_Comment by @ntBre on 2025-07-14 01:48_

That sounds good to me! I do find it confusing having to follow the link to etcetera and read the crate docs to figure out where config files go. One sentence with actual paths could really help.

---

_Comment by @orishamir on 2025-07-14 20:45_

Great! I will put up a PR soon :)

---

_Comment by @orishamir on 2025-07-15 18:34_

Hey @ntBre ,
It may be best for someone more familiar with Ruff to make that PR, as I'm afraid of misleading someone   
Also I'm not a native English speaker so the PR's review will probably reword my phrasing anyway

---

_Comment by @ntBre on 2025-07-15 18:47_

Okay, no problem :) Thank you for looking into it and reporting the issue! If you change your mind, I'm happy to review and help with phrasing or ideas.

---

_Label `question` removed by @ntBre on 2025-07-15 18:48_

---

_Label `documentation` added by @ntBre on 2025-07-15 18:48_

---
