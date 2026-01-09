---
number: 10282
title: "`uv tool install` fails to download newer Python when `pip` is installed in system python"
type: issue
state: closed
author: ceejatec
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-01-03T00:15:01Z
updated_at: 2025-01-13T09:29:45Z
url: https://github.com/astral-sh/uv/issues/10282
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv tool install` fails to download newer Python when `pip` is installed in system python

---

_Issue opened by @ceejatec on 2025-01-03 00:15_

I have a PyPI project named `cbdep` where all versions depend on at least Python 3.10. If I run `uv tool install cbdep` in a Linux environment where the system Python is < 3.10 _and_ `pip` is installed, `uv` decides not to download a newer Python and instead fails with the error

    × No solution found when resolving dependencies:
    ╰─▶ Because the current Python version (3.8.10) does not satisfy
         Python>=3.10 and all versions of cbdep depend on Python>=3.10, we can
         conclude that all versions of cbdep cannot be used.
         And because you require cbdep, we can conclude that your requirements
         are unsatisfiable.

Setting `--python-preference=only-managed` allows this to work, but that means it will install Python even in an environment where Python 3.10+ is available which isn't optimal.

Here is a Dockerfile which demonstrates the problem, included the rather bizarre fact that it only occurs when the `python3-pip` package is installed:

    FROM ubuntu:20.04 AS base

    RUN apt-get update && apt-get install -y python3 curl
    RUN useradd -m testuser
    USER testuser
    RUN curl -LsSf https://astral.sh/uv/install.sh | sh
    ENV PATH="/home/testuser/.local/bin:${PATH}"

    # This path works as expected
    FROM base AS successful

    RUN uv tool install cbdep
    RUN ls -l ~/.local/share/uv/python
    RUN touch /tmp/foo

    # This path fails because uv decides not to download python
    FROM base AS failure

    USER root
    RUN apt-get install -y python3-pip

    # This line is only here to make `docker build` build the `successful` path
    COPY --from=successful /tmp/foo /tmp/foo

    USER testuser
    RUN RUST_LOG=trace uv tool install cbdep
    RUN ls -l ~/.local/share/uv/python


I wonder if this might be related in some way to https://github.com/astral-sh/uv/issues/5144 ?


---

_Comment by @zanieb on 2025-01-03 00:22_

I think this is a duplicate of https://github.com/astral-sh/uv/issues/6381

I don't think the presence of `pip` should make any difference. I presume the difference is that the older Python version is placed on your `PATH`?

---

_Comment by @zanieb on 2025-01-03 00:23_

You can bypass this by explicitly requesting 3.10+, e.g., `--python ">=3.10"`

---

_Comment by @ceejatec on 2025-01-03 08:57_

> I think this is a duplicate of https://github.com/astral-sh/uv/issues/6381

Yep, that does look like basically the same issue. I did search for relevant issues before filing this one but somehow missed that one.

> I don't think the presence of pip should make any difference.

I completely agree! 😄 

> I presume the difference is that the older Python version is placed on your PATH?

No, in both cases it's python 3.8 from Ubuntu's repositories, and it's on PATH in both cases. The only difference is the installation of the `python3-pip` Ubuntu package in the failing example. It is true that I was assuming the existence of the `pip` binary is playing a role here; it could equally be something else that is brought in by the `python3-pip` package, which is quite a lot of stuff. But I can say that in both cases, `type python` returns "not found", while `type python3` returns `/usr/bin/python3`.

I do notice that after installing `python3-pip`, both `pip` and `pip3` are on the PATH; is it weird that there's a `pip` but no `python`?

> You can bypass this by explicitly requesting 3.10+, e.g., --python ">=3.10"

Thanks; that's a slightly better workaround than using `--python-preference=only-managed` since it _will_ use the OS-provided Python if it matches the requirement, rather than unambiguously downloading a Python installation every time. It's still not a general solution though, of course, since it requires you to have a priori knowledge of the Python requirement of the tool you're installing.

---

_Comment by @zanieb on 2025-01-03 09:30_

>  I did search for relevant issues before filing this one but somehow missed that one.

No problem, GitHub's search is bad. Thanks for making the effort though.

> I do notice that after installing python3-pip, both pip and pip3 are on the PATH; is it weird that there's a pip but no python?

I'm not sure — it's probably a Python 2 compatibility quirk. 

> No, in both cases it's python 3.8 from Ubuntu's repositories, and it's on PATH in both cases. The only difference is the installation of the python3-pip Ubuntu package in the failing example.

Interesting. I'll take a look at the reproduction. We don't ever invoke or inspect pip.

> It's still not a general solution though, of course, since it requires you to have a priori knowledge of the Python requirement of the tool you're installing.

Definitely! We'd love to fix this by solving for the necessary Python version, we just haven't gotten to it yet.

---

_Label `great writeup` added by @zanieb on 2025-01-03 09:31_

---

_Label `bug` added by @charliermarsh on 2025-01-03 15:31_

---

_Comment by @charliermarsh on 2025-01-05 23:41_

I'm going to try to fix this.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-05 23:41_

---

_Referenced in [astral-sh/uv#10401](../../astral-sh/uv/pulls/10401.md) on 2025-01-08 17:14_

---

_Closed by @charliermarsh on 2025-01-08 17:38_

---

_Closed by @charliermarsh on 2025-01-08 17:38_

---

_Comment by @cb-robot on 2025-01-13 09:29_

Verified working with uv 0.5.18 - thank you @charliermarsh !

---
