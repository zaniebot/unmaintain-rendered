```yaml
number: 10948
title: Best Practices for integrating uv with PYTHONPATH
type: issue
state: closed
author: ClarkMyWords
labels:
  - question
assignees: []
created_at: 2025-01-24T22:21:00Z
updated_at: 2025-02-04T17:28:18Z
url: https://github.com/astral-sh/uv/issues/10948
synced_at: 2026-01-12T16:00:24Z
```

# Best Practices for integrating uv with PYTHONPATH

---

_@ClarkMyWords_

### Question

Hello,

I'm working with [ROS 2 Humble](https://docs.ros.org/en/humble/index.html), largely in Python. I'm managing an environment for a few users, and I've chosen `uv` as our scheme for managing virtual environments.

ROS 2 _can_ respect Python virtual environments, but it relies on whatever is set in PYTHONPATH for those. At the moment, I am having to manually set it as such:
`export PYTHONPATH=$PYTHONPATH:$VIRTUAL_ENV/lib/python3.10/site-packages`

From [this issue](https://github.com/astral-sh/uv/issues/9168) I understand that this isn't really functionallity in `uv`'s wheelhouse, but I'm wondering if there's a reasonable way  in `uv` to create some sort of hook that, after sourcing `.venv/bin/activate`, will add anything added by that venv to the PYTHONPATH? (Bonus points if this would also work for something installed via `uv pip install -e ...` because that doesn't place the editable package in the site-packages directory (for obvious reasons)).

I see that for something like `uv run ...` the `--env-file` argument could work, but, because of the constraints of my project, I'm using `uv` only for the virtual environment management, not for actually running anything.

Or, possibly, if there is any other wisdom or best-practices to dynamically set my PYTHONPATH with `uv`.

Thanks!

### Platform

Ubuntu 22.04.5 LTS x86_64

### Version

uv 0.5.24 (42fae925c 2025-01-23)

---

_Label `question` added by @ClarkMyWords on 2025-01-24 22:21_

---

_Closed by @ClarkMyWords on 2025-01-24 22:21_

---

_Reopened by @ClarkMyWords on 2025-01-24 22:41_

---

_Closed by @ClarkMyWords on 2025-01-28 20:28_

---

_Comment by @borismo on 2025-02-04 04:53_

Hi, did you solve this? Curious to know how if you did.

---

_Comment by @ClarkMyWords on 2025-02-04 17:28_

@borismo 

Hi,

So, according to the ROS 2 documentation [here](https://docs.ros.org/en/humble/How-To-Guides/Using-Python-Packages.html), the venv should work out of the box. However, based on all of my testing, as well as [this stackexchange post](https://robotics.stackexchange.com/questions/98214/how-to-use-python-virtual-environments-with-ros2) which first pointed me to `PYTHONPATH`, it is clear this isn't the case.

In my use-case, our system is fully containerized (due to the constraints of our institution, we are using [apptainer](https://apptainer.org/) in lieu of Docker/on top of a docker image (for back-readers, I would be happy to discuss using ROS 2 in apptainer, but that's not specifically relevant to this issue).

Because it is containerized, we have decided that system-wide package installation is our best option, because that works out of the box. This should be fine containerized. I'm still using `uv` to manage the installation, but this way, I never have to source a venv or modify the `PYTHONPATH`.

So, in summary, because we were using a containerized environment anyway, I just ensured that Python packages were installed, with `uv pip`, systemwide. (In my specific case, this is an issue because our apptainer containers are read-only, so we have to rebuild the whole image to add more Python dependencies, but that is an issue we're taking up with our institution's security policy, and the system install would work fine in a Docker container.

---
