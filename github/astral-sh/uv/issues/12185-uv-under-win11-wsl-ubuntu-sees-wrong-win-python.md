---
number: 12185
title: uv under Win11 WSL-ubuntu sees wrong (win) python references
type: issue
state: open
author: krstp
labels:
  - needs-mre
assignees: []
created_at: 2025-03-15T12:28:52Z
updated_at: 2025-04-11T11:00:49Z
url: https://github.com/astral-sh/uv/issues/12185
synced_at: 2026-01-07T13:12:18-06:00
---

# uv under Win11 WSL-ubuntu sees wrong (win) python references

---

_Issue opened by @krstp on 2025-03-15 12:28_

Here is an interesting turn of things... in the past days I had to work with Win11 box... as unusable as Windows-box is, it made me install virtual Ubuntu shell via WSL...

Long story short, when installing uv under virtual shell, that by default runs bash changed to zsh, some strange reference base python thing happens that instead of seeing *nix python versions, it sees the underlying Win11 pythons that clearly make it unusable or with some gimmicks will allow Win11 layer pythons usage which is super inefficient.

The core issue was, whenever I tried to instantiate uv venv, uv would not find  the right python references, uv simply could not see and recognize the *nix layer pythons!

What did came with a rescue was pyenv, that carries correct local zsh references. So running pyenv and then uv for pip sync etc works just fine.

This also shows the inner-works of core uv for python env instantiation.. such as it runs on diff python references than follow up commands, which makes sense. However, relying only on uv under WSL-ubuntu for venv was not finalizing with success, by ingesting windows python references ¯\_(ツ)_/¯ or rather unable to even correctly ingest those producing an error, which also makes sense.

This just shows uv runs better natively. And yes I did reinstall uv under WSL, it would still find win11 references.

Also, atm, I just want to give context why combination of pyenv with uv might be better in such scenario, where pyenv-venv yields some more env stability. Also this is not the first time pyenv-venv comes to the rescue, I am mostly mac and *nix user, and whatever hurdles I was dealing with in those systems in local development, the virtual env layer with pyenv-venv came each time to the rescue - if you prefer blame it on early uv releases or just my user error, whichever suits better; atm I am unable to reference the issues from the past I had, but there were some. 

Thank you for working on such wonderful tool as uv! It really makes our lives easier. o7

_Originally posted by @krstp in https://github.com/astral-sh/uv/issues/1495#issuecomment-2726485796_

---

_Referenced in [astral-sh/uv#1495](../../astral-sh/uv/issues/1495.md) on 2025-03-15 12:31_

---

_Comment by @konstin on 2025-04-11 11:00_

Can you share a list of setup steps and commands to reproduce your problem, and verbose logs for the cases where commands failed?

---

_Label `needs-mre` added by @konstin on 2025-04-11 11:00_

---
