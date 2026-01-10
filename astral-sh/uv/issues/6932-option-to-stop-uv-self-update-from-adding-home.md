```yaml
number: 6932
title: "Option to stop `uv self update` from adding `. $HOME/.local/share/cargo/env`?"
type: issue
state: closed
author: baggiponte
labels:
  - documentation
  - external
  - releases
assignees: []
created_at: 2024-09-02T09:49:07Z
updated_at: 2024-09-06T09:59:38Z
url: https://github.com/astral-sh/uv/issues/6932
synced_at: 2026-01-10T04:45:10Z
```

# Option to stop `uv self update` from adding `. $HOME/.local/share/cargo/env`?

---

_Issue opened by @baggiponte on 2024-09-02 09:49_

Ciao! With rust on my machine already, my shell already sources the `cargo/env` script. Every time that I upgrade uv to a newer version with `uv self update`, the line is added to my zshrc and fish conf dir (which I do not use). This is absolutely nothing high priority, but I was wondering if you would consider setting an option to disable this behaviour.

---

_Comment by @AbdealiLoKo on 2024-09-02 12:59_

I think you may be looking for `--no-modify-path` ?

---

_Comment by @baggiponte on 2024-09-02 14:53_

> I think you may be looking for `--no-modify-path` ?

Interesting. I don't see it as an option for `uv self update` though.

---

_Renamed from "Option to stop `uv` from adding `. $HOME/.local/share/cargo/env`?" to "Option to stop `uv self update` from adding `. $HOME/.local/share/cargo/env`?" by @baggiponte on 2024-09-02 14:53_

---

_Label `upstream` added by @charliermarsh on 2024-09-02 17:18_

---

_Label `release` added by @charliermarsh on 2024-09-02 17:18_

---

_Comment by @tpgillam on 2024-09-04 08:03_

Seconded - I upgraded from `uv` 0.4.3 -> 0.4.4 just now with `uv self update`, and it appended `~/.bashrc` with:
```

. "$HOME/.cargo/env"
```

I would certainly prefer if the `self update` command by default did not modify any configuration files. My reasoning is that, if updating, the user has presumably already set their paths in the way that they want (in my case I want my `.bashrc` portable between different machines, so I don't put any paths directly in that file).

Only a small niggle though. I just have to do a `git restore` on it every time I upgrade, which is mildly inconvenient at worst!

---

_Comment by @baggiponte on 2024-09-04 08:14_

Maybe one could do `uv self update --ensurepath` or a flag like that and have this behaviour to be opt-in.

---

_Comment by @zanieb on 2024-09-04 12:55_

Looking into upstream support for this — I don't think we can add it until they do.

---

_Comment by @zanieb on 2024-09-05 14:09_

Can you try setting `INSTALLER_NO_MODIFY_PATH=1` during the update?

---

_Comment by @tpgillam on 2024-09-05 15:30_

I'm on a different machine today (Fedora 39), where it doesn't try to modify `~/.bashrc`, but instead creates or modifies `~/.profile`

> Can you try setting INSTALLER_NO_MODIFY_PATH=1 during the update?

Yes - this seems to do the trick (installed 0.4.4 and tested on `uv self upgrade` thereafter).

---

_Comment by @zanieb on 2024-09-05 15:35_

Great — we can document that.

---

_Label `documentation` added by @zanieb on 2024-09-05 15:35_

---

_Closed by @zanieb on 2024-09-05 16:15_

---

_Comment by @baggiponte on 2024-09-06 09:59_

Confirm too, setting `INSTALLER_NO_MODIFY_PATH=1` works :)

---
