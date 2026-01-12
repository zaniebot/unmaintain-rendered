```yaml
number: 2949
title: "feature: global / user-wide config"
type: issue
state: closed
author: Roguelazer
labels: []
assignees: []
created_at: 2023-02-16T05:20:16Z
updated_at: 2023-02-16T16:27:31Z
url: https://github.com/astral-sh/ruff/issues/2949
synced_at: 2026-01-12T15:54:43Z
```

# feature: global / user-wide config

---

_@Roguelazer_

It would be nice to have a way to configure this tool system-wide; I'm imagining that it could look at the standard XDG configuration sources (`~/.config/ruff.toml` and `/etc/ruff.toml`) in addition to walking up the filesystem from the current directory.

I know it's not quite as pure as having a `pyproject.toml` in every directory hierarchy, but sometimes it's nice to be able to set defaults for one-off scripts.

---

_Comment by @charliermarsh on 2023-02-16 13:26_

I believe we actually do support this -- can you take a look at [this section](https://github.com/charliermarsh/ruff#how-can-i-change-ruffs-default-configuration) in the docs? If it's not quite what you're asking for, LMK and I can reopen!

---

_Closed by @charliermarsh on 2023-02-16 13:26_

---

_Comment by @Roguelazer on 2023-02-16 15:45_

Ah, I was led astray because for some reason dirs decided to look in `~/Application Support/ruff/ruff.toml` on macos instead of `~/.config`; I think this is the only CLI tool I've ever used that does so. I moved my config in there and it works. Obviously that's a `dirs` weirdness, though; nothing on this repo.

Also, fwiw, this functionality isn't mentioned in [the docs page](https://beta.ruff.rs/docs/configuration/), so perhaps that's a process improvement opportunity.

---

_Comment by @charliermarsh on 2023-02-16 16:27_

Ah yeah, it's mentioned in the [FAQ](https://beta.ruff.rs/docs/faq/), but it should at least be mentioned in the configuration section.

---
