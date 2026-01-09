---
number: 8738
title: Local drive environment install with project on network drive
type: issue
state: closed
author: marcelroed
labels:
  - question
assignees: []
created_at: 2024-10-31T23:59:21Z
updated_at: 2024-11-07T04:09:39Z
url: https://github.com/astral-sh/uv/issues/8738
synced_at: 2026-01-07T13:12:18-06:00
---

# Local drive environment install with project on network drive

---

_Issue opened by @marcelroed on 2024-10-31 23:59_

I'm working in an academic machine learning environment, where most users work across many machines, and therefore have their projects on a network drive. In practice this means people either maintain a (horribly slow) environment on the network drive, or they set up an environment manually on each machine they want to use. The benefit of keeping projects on a network drive is that projects can be used across different machines when hardware availability changes, or on multiple machines at once when multi-node runs happen.

Currently, my understanding is that uv only supports keeping the virtualenv in the project-local `.venv` directory, meaning everything needs to be copied or symlinked (I think this means every package is symlinked individually?) to the network drive.

I've found I can get all the benefits of the local cache and a local install by symlinking the `.venv` directory to a virtualenv on the local drive. It's really painful to set this up with current uv, and I propose this should be supported.

Here is my current workaround:

1. Place your project on the network drive
2. Create a symlink from `<project-dir>/.venv/` to a directory on the local drive. The local path will be consistent across every machine.
3. Instantiate an empty virtualenv in that local directory on the machine you want to use. This needs to be repeated on each machine.
4. `uv run ...` in the project directory on the network drive. This will be fast!

What I'm envisioning is support for this without manually creating the symlinks on each machine, or at least without having to instantiate a virtualenv manually in the local drive. The key issue with my current setup is that it doesn't happen automatically when moving to a new machine, meaning I have to set it up for every project on every machine. An ideal implementation here might be something similar to the transient environments created for `uvx` or `uv run --with`.

Is this use-case something the uv team would be interested in supporting? I know this would be very valuable to research labs that operate across many machines.

---

_Comment by @FishAlchemist on 2024-11-01 11:40_

Would setting environment variables solve your problem?
https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path

**Note:** I only contributed to the document. I'm not a member of the UV team.

Edit:
Actually, the temporary virtual environment created by UV doesn't necessarily have a short lifespan, as it caches the virtual environment.

---

_Label `question` added by @charliermarsh on 2024-11-01 13:38_

---

_Comment by @charliermarsh on 2024-11-01 13:39_

Yeah I'm curious if `UV_PROJECT_ENVIRONMENT` solves your problem?

---

_Comment by @FishAlchemist on 2024-11-01 13:45_

> Yeah I'm curious if `UV_PROJECT_ENVIRONMENT` solves your problem?

However, in this context, if using ``UV_PROJECT_ENVIRONMENT`` can solve the problem, can we enforce its setting in the configuration file to prevent accidental creation of virtual environments within the project directory?

---

_Comment by @marcelroed on 2024-11-05 21:45_

Yes! This along with a `.env` file seems like a good approach. It seems like I have to explicitly set the `--env-file` flag from #8811. Is this the intended future behavior also for 0.5 and above?

---

_Closed by @marcelroed on 2024-11-07 04:09_

---
