```yaml
number: 14849
title: Binary installation Path for uv / uvx breaking with snap updates for vs code on Ubuntu 24
type: issue
state: open
author: gorbinphilip
labels:
  - bug
  - external
assignees: []
created_at: 2025-07-23T16:12:21Z
updated_at: 2026-01-09T10:58:16Z
url: https://github.com/astral-sh/uv/issues/14849
synced_at: 2026-01-12T16:01:57Z
```

# Binary installation Path for uv / uvx breaking with snap updates for vs code on Ubuntu 24

---

_@gorbinphilip_

### Summary

I installed uv with the [standalone](https://docs.astral.sh/uv/getting-started/installation/#standalone-installer) installer using the command below
```
curl -LsSf https://astral.sh/uv/install.sh | sh
```
I believe i did this from vscode terminal. Everything worked fine until snap updated vscode couple of times, which eventually removed the version specific path values written into ~/.bashrc & ~/.profile
```
. "$HOME/snap/code/[version]/.local/share/../bin/env"
```
leading to profile loading error on login & no uv, uvx binaries in path.

Binaries seem to have got installed in the following vs code location
```
/home/[user]/snap/code/[version]/.local/share/../bin/
```
 (version value present was109 in my case)

I ended up changing the path values written in the following way for it to consistently work
```
. "$HOME/snap/code/current/.local/share/../bin/env"
```

### Platform

Ubuntu 24.04.2 LTS (Linux 6.14.0-24-generic x86_64 GNU/Linux)

### Version

uv 0.7.19

### Python version

Python 3.12.3

---

_Label `bug` added by @gorbinphilip on 2025-07-23 16:12_

---

_Comment by @zanieb on 2025-07-23 16:14_

I'm not sure it's in scope for us to deal with snap isolation. I'd recommend not installing uv from inside a snap :)

---

_Comment by @zanieb on 2025-07-23 16:15_

I guess it's plausible we could detect we're in a snap path and set `current` appropriately? but it sounds like snap is setting a variable to a versioned directory and I think it's correct for us to respect that.

---

_Comment by @gorbinphilip on 2025-07-23 18:15_

If it is snap itself setting up this trap, a warning during installation attempt could be useful.

---

_Label `needs-design` added by @zanieb on 2025-07-24 21:20_

---

_Label `needs-decision` added by @zanieb on 2025-07-24 21:20_

---

_Comment by @pwilkin on 2025-08-20 21:03_

Please disallow installing uv via snap. Print an error or something. 

I just spent six totally miserable hours of my programming life trying to figure out why the hell a `uv run` command, which ran perfectly fine from the shell, started crashing with an immediate signal 120 as soon as I ran it from within a spawn_process call in TypeScript.

Well, I still don't know why exactly. But I did find out that once I replaced my snap-installed uv with a normal uv installed in ~/.local/bin, everything started working fine.

---

_Comment by @zanieb on 2025-08-20 23:35_

Hey @lengau — would you be interested in exploring a fix for this with us?

---

_Comment by @Logan-Pageler on 2025-08-22 22:18_

I hit this issue too, but it doesn't seem to be uv issue, or snap issue, but more of a vscode issue. Apparently vscode configures `XDG_DATA_HOME` and uv rightfully uses it for its install. There is already an [open issue](https://github.com/microsoft/vscode/issues/237608) about this on the vscode github if you want to read more about it.

The TLDR of the issue is: Fix this by setting
```json
"terminal.integrated.env.linux": {
    "XDG_DATA_HOME": "${env:HOME}/.local/share"
  }
```
in ur vscode `settings.json` and then you can install uv and other tools properly from within vscode.

Its a sad day to be a vscode user :(

Maybe its still worth warning people about this tho since I know vscode is very popular, so theres a good chance many people will hit this?


---

_Comment by @abulygin on 2026-01-08 22:30_

I have got the same issue and the solution, proposed by the author worked for me. This happened after the recent snap and vscode update

---

_Comment by @konstin on 2026-01-09 10:57_

A simple improvement is to show a warning when we see `$HOME/snap/code/[version]/` in the path, telling users that this will break with the next version update and that they should use a non-snap installer.

The proper fix would be on the VS Code or snap side.

---

_Label `needs-design` removed by @konstin on 2026-01-09 10:57_

---

_Label `needs-decision` removed by @konstin on 2026-01-09 10:57_

---

_Label `external` added by @konstin on 2026-01-09 10:57_

---
