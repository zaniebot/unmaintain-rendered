---
number: 1390
title: Need improved handling of virtual environments
type: issue
state: closed
author: eabase
labels:
  - question
assignees: []
created_at: 2024-02-15T23:47:15Z
updated_at: 2024-07-01T21:49:09Z
url: https://github.com/astral-sh/uv/issues/1390
synced_at: 2026-01-07T13:12:16-06:00
---

# Need improved handling of virtual environments

---

_Issue opened by @eabase on 2024-02-15 23:47_

Hi, 

I think this package could benefit greatly by improving the handling of virtual environments. 

There are already a great simple solution available, called [venvlink](https://github.com/fohrloop/venvlink). It also has automatic venv *dis/activation* with powershell integration. 

However, that project is no longer maintained and need either a new maintainer or be integrated with something that is maintained.

The main issues with all other available package managers is that they don't separate the venv packages in a sensible manner, and insists on bloating the project directory with these entire package installation structures, while not making it easy to share, reuse or copy environments without duplicating data on disk. Because of this, they also don't keep track of what virtual environments have already been created, or what environments are available on the system.

`venvlink` solves all that!


---

_Comment by @zanieb on 2024-02-15 23:57_

Hi!

Thanks for engaging with the project.

We're already de-duplicating installed packages (on supported platforms). We're planning to tackle a full "workflow" around environment management in the future as well. This will have a much more opinionated workflow than manually creating and activating environments.

---

_Comment by @vtnate on 2024-02-16 23:17_

I love what Astral is doing for python tooling! For me the killer feature of pyenv is the automatic activation/de-activation of venvs as I move around in the terminal. That is the only item keeping me on pyenv at this point, tbh. https://rye-up.com/ appears to be a parallel attempt at fixing the pip/venv experience and making a universal solution.

I'm here to applaud your work and request something like pyenv's seamless venv experience, once venvs have been created.

---

_Comment by @mjclarke94 on 2024-02-18 20:20_

@vtnate I'd suggest checking out [python launcher for unix](https://python-launcher.app). I moved away from pyenv as it was slowing my terminal down massively. This tool replicates the `py` command for windows on unix/mac.

---

_Label `question` added by @zanieb on 2024-02-18 20:30_

---

_Comment by @zanieb on 2024-07-01 21:49_

We're continuing to explore this and will probably have automated activation of a sort in the near future.

---

_Closed by @zanieb on 2024-07-01 21:49_

---
