```yaml
number: 215
title: Use a platform-appropriate location for user configuration
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: config_dir
created_at: 2022-09-16T23:48:38Z
updated_at: 2022-09-17T19:38:46Z
url: https://github.com/astral-sh/ruff/pull/215
synced_at: 2026-01-12T15:55:04Z
```

# Use a platform-appropriate location for user configuration

---

_@andersk_

See https://docs.rs/dirs/4.0.0/dirs/fn.config_dir.html.

---

_@charliermarsh reviewed on 2022-09-17 00:02_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 00:02_

Do you think it's more typical for users to write to the config dir than the home dir? To me the config dir paths seem more like internal than user-facing config (e.g., `/Users/Alice/Library/Application Support`).

I think the ideal IMO is to mimic Black's behavior:

<img width="750" alt="Screen Shot 2022-09-16 at 5 59 47 PM" src="https://user-images.githubusercontent.com/1309177/190831916-f0467973-1753-4734-bbfd-8cc225af592b.png">


---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 00:03_

(I would also be comfortable getting rid of the global config and only supporting local `pyproject.toml`, if this is causing problems.)

---

_@charliermarsh reviewed on 2022-09-17 00:03_

---

_@andersk reviewed on 2022-09-17 00:17_

---

_Review comment by @andersk on `src/pyproject.rs`:68 on 2022-09-17 00:17_

I did some investigation into Black’s behavior. psf/black#1899 says it was inherited from pycodestyle. PyCQA/pycodestyle@b1ada24d5d6f837ebaa226a57fce20621048a199 says it fixed a `TypeError` on Windows (PyCQA/pycodestyle#95)—but the part that actually fixed the `TypeError` was switching from `os.getenv("HOME")` to `os.expanduser('~')`, so that doesn’t explain the point of using `~\.pep8` on Windows.

I can say as a Linux user that an [XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) path `$XDG_CONFIG_HOME/foo` or `~/.config/foo` is more idiomatic for modern tools than `~/.foo`. Windows users don’t expect to use dotfiles at all—they aren’t hidden and they’re awkward to deal with in the UI, so `~\.foo` seems like a very weird choice. I’m not sure what macOS users expect.

---

_Review comment by @andersk on `src/pyproject.rs`:68 on 2022-09-17 00:41_

If we want `~/.config/ruff` on all platforms, we could use `xdg::BaseDirectories::new().ok().and_then(|dirs| dirs.find_config_file("ruff"))` from the [`xdg` crate](https://docs.rs/xdg/2.4.1/xdg/struct.BaseDirectories.html#method.find_config_file).

---

_@andersk reviewed on 2022-09-17 00:41_

---

_@charliermarsh reviewed on 2022-09-17 03:38_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 03:38_

I think for macOS I'd expect `~/.ruff`, so if we can do that and then `~/.config/ruff` for Linux (and then `~/ruff` or whatever you like for Windows), that would be awesome.

---

_@andersk reviewed on 2022-09-17 05:11_

---

_Review comment by @andersk on `src/pyproject.rs`:68 on 2022-09-17 05:11_

Locations preferred by other modern Python tools:

Tool | Linux | macOS | Windows
--- | --- | --- | ---
[Black](https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#where-black-looks-for-the-file) | XDG `~/.config` | XDG `~/.config` | `~`
[conda](https://conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html) | XDG `~/.config` | XDG `~/.config` | XDG `~/.config`
[Flake8](https://flake8.pycqa.org/en/latest/release-notes/4.0.0.html#backwards-incompatible-changes) | *removed* | *removed* | *removed*
[Hatch](https://hatch.pypa.io/latest/config/hatch/) | XDG `~/.config` | `~/Library/Preferences` | `%APPDATA%`
[mypy](https://mypy.readthedocs.io/en/stable/config_file.html) | XDG `~/.config` | XDG `~/.config` | XDG `~/.config`
[PDM](https://pdm.fming.dev/latest/usage/project/#configure-the-project) | XDG `~/.config` | `~/Library/Preferences` | `%APPDATA%`
[pip](https://pip.pypa.io/en/stable/topics/configuration/#location) | XDG `~/.config` | `~/Library/Application Support` | `%APPDATA%`
[Poetry](https://python-poetry.org/docs/configuration/) | XDG `~/.config` | `~/Library/Preferences` | `%APPDATA%`
[virtualenv](https://virtualenv.pypa.io/en/latest/cli_interface.html) | XDG `~/.config` | `~/Library/Preferences` | `%APPDATA%`
[YAPF](https://github.com/google/yapf) | XDG `~/.config` | XDG `~/.config` | XDG `~/.config`

Many of these tools now delegate this question to the [platformdirs](https://platformdirs.readthedocs.io/) module, whose [issues](https://github.com/platformdirs/platformdirs/issues) have some relevant discussion. But just about everyone seems to agree that storing configuration directly in `~` is at best a legacy option these days.

🤷

---

_@charliermarsh reviewed on 2022-09-17 15:30_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 15:30_

Wow great collation! Thanks for doing that. It seems like in each case, they're storing a configuration file _within_ those directories rather than as a dotfile? I.e., there's a `config.toml` or `config.ini` or whatever within the directory. So maybe we do this?

- Linux: `XDG ~/.config/ruff/pyproject.toml`? (Would be nice if this was called `config.toml` I guess in the global case but I don't feel strongly.)
- macOS: `~/Library/Application Support/ruff/pyproject.toml`? (I'd prefer `~/Library/Preferences` but I don't feel strongly enough to workaround the `dirs` module.)
- Windows: `%APPDATA%\ruff\pyproject.toml`?

What do you think?


---

_@andersk reviewed on 2022-09-17 19:27_

---

_Review comment by @andersk on `src/pyproject.rs`:68 on 2022-09-17 19:27_

Sounds good to me. Updated.

An advantage of the `pyproject.toml` name is that it makes it obvious that the format is the same (e.g. a `[tool.ruff]` section is still expected).

---

_Merged by @charliermarsh on 2022-09-17 19:29_

---

_Closed by @charliermarsh on 2022-09-17 19:29_

---

_Branch deleted on 2022-09-17 19:30_

---

_@charliermarsh reviewed on 2022-09-17 19:31_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 19:31_

Agreed!

---

_@charliermarsh reviewed on 2022-09-17 19:31_

---

_Review comment by @charliermarsh on `src/pyproject.rs`:68 on 2022-09-17 19:31_

And portable (can copy into projects if needed).

---

_@andersk reviewed on 2022-09-17 19:38_

---

_Review comment by @andersk on `src/pyproject.rs`:68 on 2022-09-17 19:38_

By the way, I noticed that `virtualenv --help` prints its config file location, which is a nice discoverability touch. I tried to do this with clap’s `after_help`, but it seems you can only use an owned `String` there in the Git version of clap with the `string` feature, so we should revisit that when clap 4 is released.

---
