```yaml
number: 12489
title: Is there a way to set uv installation Path?
type: issue
state: closed
author: Crefok
labels:
  - question
assignees: []
created_at: 2025-03-26T15:07:35Z
updated_at: 2025-03-26T15:53:03Z
url: https://github.com/astral-sh/uv/issues/12489
synced_at: 2026-01-12T16:01:04Z
```

# Is there a way to set uv installation Path?

---

_@Crefok_

### Question

Hey there,
we currently think about going from rye to uv. We face the Problem that not every PC which will use uv will have Internet Access. So with Rye we set up a sync Process to do self update and sync to some other pcs.
We share some Servers without Internet Accsess. So that not every Person has to setup his own sync, we share the rye executable as well as he python versions. We got that working.

we are currently in the setup with uv. But the self update of uv reports 
`warning: Self-update is only available for uv binaries installed via the standalone installation scripts.

The current executable is at `/shared/path/on/systems` but the standalone installer was used to install uv to `$HOME/.local/bin`. Are multiple copies of uv installed?`

is there a easyier way of fixing it then remove links under $HOME/.local/bin, copy files from /shared/path/on/systems, perform the update, move the files back and create the links after the update?

### Platform

different linux X86 with 64Bit

### Version

uv 0.6.9/0.6.10

---

_Label `question` added by @Crefok on 2025-03-26 15:07_

---

_Comment by @charliermarsh on 2025-03-26 15:41_

Are you looking for https://docs.astral.sh/uv/configuration/environment/#uv_install_dir?

---

_Comment by @Crefok on 2025-03-26 15:53_

yeah thats exactly what I am looking for, thanks!

---

_Closed by @Crefok on 2025-03-26 15:53_

---
