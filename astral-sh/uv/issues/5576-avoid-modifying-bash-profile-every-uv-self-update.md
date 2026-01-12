```yaml
number: 5576
title: "Avoid modifying `.bash_profile` every `uv self update`"
type: issue
state: closed
author: scienceplease
labels:
  - external
  - releases
assignees: []
created_at: 2024-07-29T21:21:10Z
updated_at: 2024-10-17T01:07:07Z
url: https://github.com/astral-sh/uv/issues/5576
synced_at: 2026-01-12T15:58:57Z
```

# Avoid modifying `.bash_profile` every `uv self update`

---

_@scienceplease_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Currently, the user's `.bash_profile` is modified by appending `. $CARGO_HOME/env` on every `uv self update`. I understand this behavior on initial install for making uv work out of the box for everyone, but `uv` should respect if a user has since (re)moved this line in their BASH configuration when updating. Is it possible to limit this behavior to the initial install and omit it on `uv self update`?

```bash
$ uv --version
uv 0.2.31
```


---

_Label `upstream` added by @charliermarsh on 2024-07-29 21:28_

---

_Comment by @charliermarsh on 2024-07-29 22:04_

It seems like a reasonable request. I will check with the axo team to see what they think.

---

_Comment by @zanieb on 2024-07-30 14:28_

I think the problem is that the installer can't distinguish between a _newly_ supported shell config file and previously supported one so it just adds to _all_ configs. It does seem reasonable to add a flag to opt-out though? 

---

_Comment by @scienceplease on 2024-07-31 20:56_

Can the difference between an update and initial install be detected? If so, implicit/automatic modification to shell config can be reserved for just the initial installation process (just communicate to the user that it was done). A flag could then be used to enable explicit opt-in to shell config modification when updating e.g. `uv self update --config-shell`, or potentially even outside of the update command e.g. `uv self shellconfig`.

---

_Label `releases` added by @zanieb on 2024-09-12 02:18_

---

_Comment by @ilyagr on 2024-09-12 03:28_

If/when https://github.com/axodotdev/axoupdater/issues/174 is implemented, this should become straightforward. 

---

_Comment by @zanieb on 2024-10-17 01:07_

This is now implemented upstream and will be in the next uv release

---

_Closed by @zanieb on 2024-10-17 01:07_

---
