---
number: 15673
title: "Question: Is there a `--break-system-packages` equivalent to install in the system environemnt inside a Docker?"
type: issue
state: closed
author: turbotimon
labels:
  - question
assignees: []
created_at: 2025-09-04T07:56:21Z
updated_at: 2025-09-06T02:41:51Z
url: https://github.com/astral-sh/uv/issues/15673
synced_at: 2026-01-10T01:25:58Z
---

# Question: Is there a `--break-system-packages` equivalent to install in the system environemnt inside a Docker?

---

_Issue opened by @turbotimon on 2025-09-04 07:56_

### Question

I'm trying to install packages in the system environment in a Docker. According to [uv in Docker](https://docs.astral.sh/uv/guides/integration/docker/#installing-a-package), this can be done with `RUN uv pip install --system ruff`.

However, since it is a Debian-based image, I got this error. The restriction comes from [PEP 668](https://peps.python.org/pep-0668/), but I think it should be safe in a docker.

```
 => ERROR [6/7] RUN uv pip install --system --no-cache --no-managed-python -r requirements.out                                                                                                      0.1s
------                                                                                                                                                                                                   
 > [6/7] RUN uv pip install --system --no-cache --no-managed-python -r requirements.out:
0.122 Using Python 3.11.2 environment at: /usr
0.122 error: The interpreter at /usr is externally managed, and indicates the following:
0.122 
0.122   To install Python packages system-wide, try apt install
0.122   python3-xyz, where xyz is the package you are trying to
0.122   install.
0.122 
0.122   If you wish to install a non-Debian-packaged Python package,
0.122   create a virtual environment using python3 -m venv path/to/venv.
0.122   Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
0.122   sure you have python3-full installed.
0.122 
0.122   If you wish to install a non-Debian packaged Python application,
0.122   it may be easiest to use pipx install xyz, which will manage a
0.122   virtual environment for you. Make sure you have pipx installed.
0.122 
0.122   See /usr/share/doc/python3.11/README.venv for more information.
0.122 
0.122 Consider creating a virtual environment with `uv venv`.
------
```

My dockerfile looks like this:

```
FROM codercom/code-server:latest

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      python3 \
      python3-pip \
      python3-venv \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# install uv globally
ENV UV_INSTALL_DIR=/usr/local/bin
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

COPY requirements/ /requirements/

WORKDIR /requirements

RUN uv pip install --system --no-cache --no-managed-python -r requirements.out
```

My current workaround is:
- Either use regular pip with `--break-system-packages`
- Or disable the protection with `RUN rm /usr/lib/python3*/EXTERNALLY-MANAGED`

I couldn't find a similar `--break-system-packages` flag for uv? I do not want to have a venv and can not inherit form an official python image (which does not have this restriction).

### Platform

docker, debian

### Version

_No response_

---

_Label `question` added by @turbotimon on 2025-09-04 07:56_

---

_Comment by @konstin on 2025-09-04 11:35_

I recommend using a docker image such as `python:3.11-trixie` that does not have this restriction, the generic debian has this restriction in place to note that system packages may also use those site-packages, causing conflicts. We also provide such images (https://docs.astral.sh/uv/guides/integration/docker/#available-images), and there is a variety of similar base images available, or alternatively `codercom/code-server` may be able to lift this restriction.

Apart from that, `uv pip install --break-system-packages` is supported in uv.

---

_Comment by @turbotimon on 2025-09-05 07:02_

> Apart from that, uv pip install --break-system-packages is supported in uv.

@konstin Apologies, I thought I had checked this, but apparently I only read the "uv in docker" page. But wouldn't you agree that it would make sense to mention this option in the [docker docs](https://docs.astral.sh/uv/guides/integration/docker/)? I could do a PR for that

---

_Comment by @konstin on 2025-09-05 07:16_

We don't want to promote `--break-system-packages`, if an environment is set to externally managed, we should respect that. See PEP 668 for the extended rational and details on the `EXTERNALLY-MANAGED` marker.

---

_Comment by @turbotimon on 2025-09-05 08:00_

@konstin I understand that you don't want to promote it in general, but I'm talking about using it in a Docker which should be perfectly fine. Isn't that flag specifically for environments like CI/CD and therefore docker images?

> --break-system-packages is intended for use in continuous integration (CI) environments

In my case, I can not use another base image and have to "break system packages". However, this is safe as nobody will use `apt` later. I was expecting this in the **"uv in Docker"** docs, as it explicitly already talks about [installing in the system environment with `--system`](https://docs.astral.sh/uv/guides/integration/docker/#installing-a-package). What's against placing a note there, informing (not promoting) about it?

---

_Comment by @konstin on 2025-09-05 09:06_

There are other options, such as using venv in docker (https://hynek.me/articles/docker-virtualenv/), so we don't want to go against the PEP (https://peps.python.org/pep-0668/#guide-users-towards-virtual-environments and https://peps.python.org/pep-0668/#use-cases 5.). While we acknowledge it is something necessary to use this flag, we consider it an escape hatch that should only be used when no other option is possible. We want to encourage best practices with our guides, leaving edge cases to the reference documentation.

---

_Closed by @charliermarsh on 2025-09-06 02:41_

---
