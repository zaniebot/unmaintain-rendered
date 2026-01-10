---
number: 12875
title: How to install uv system wide?
type: issue
state: open
author: vitrinite
labels:
  - question
assignees: []
created_at: 2025-04-14T09:08:03Z
updated_at: 2025-06-03T03:08:02Z
url: https://github.com/astral-sh/uv/issues/12875
synced_at: 2026-01-10T01:25:26Z
---

# How to install uv system wide?

---

_Issue opened by @vitrinite on 2025-04-14 09:08_

### Question

Hello

I have jupyterhub server and I would like to use uv for all my user. 
After looking at the documentation I try to install uv system wide by  using
curl -LsSf https://astral.sh/uv/install.sh > installuv.sh
export UV_INSTALL_DIR="/usr/local/bin"
sudo -E sh installuv.sh
Everything seems good uv and uvx are in /usr/local/bin/.

But $(uv python dir) and $(uv tool dir) are in /home/adminuser/.local/share/uv/ and there is also a directory in   
/home/adminuser/.config => did not this potentially prevent the other user to use properly uv?
When I try to use uv from the  adminuser account, uv have not the same the behavior as when I install just for one user, it is like the environment was imposed to be the one of the python system install.
For example, init a new project into an empty directory with uv init.
uv pip list inside the directory reply 
 Using Python 3.12.6 environment at: /opt/tljh/user 
 and the list is not empty but correspond to the environment of the user.

What should I do to make uv available to all my users without having each of them to install uv?



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @vitrinite on 2025-04-14 09:08_

---

_Comment by @konstin on 2025-04-14 09:45_

In jupyterhub, are the users system users, or does jupyterhub do separate user management?

You can configure the install directories with `UV_PYTHON_INSTALL_DIR`, `UV_TOOL_DIR` and `UV_TOOL_BIN_DIR` to set it to a global location. Usually though, it's easier to have a uv installation per user, e.g. the user that runs the jupyterhub process; a uv installation is small (just the `uv` and `uvx` binaries), so there is not much overhead re-installing it.

---

_Comment by @vitrinite on 2025-04-18 10:10_

In my jupyterhub, my users are the users of the system.

It seems to me that uv have also a sort of cache, so that the packages are not store multiple time? Should this directories have special permission, so that all the user can use it?
I have set UV_PYTHON_INSTALL_DIR, UV_TOOL_DIR and UV_TOOL_BIN_DIR as you propose. The installation was fine, I can publish environnement with ipykernel fine.
Should I made the environnement variable permanent or it was just for the installation?

Thanks for your reply

---

_Comment by @konstin on 2025-04-22 09:10_

The environment variables are just for installation. To avoid permission problems (uv links files and directories on installation), each user should have their own cache, without sharing it.

---

_Comment by @ThomasHezard on 2025-04-23 15:53_

Hello 👋 

I am also wondering how to install `uv` system-wide.    
We (me and my team) are working on multiple projects with multiple Python versions, and multiple versions of many different dependencies. Hence, our `uv` caches are quite large.   
I was looking for a way to share `uv` and all its cache in order to avoid taking that much space for each user on the team's workstations when I found this discussion.

Two questions
- What is the reason behind your recommandation for each user to have their own `uv` cache?
- Is there a way to share `uv` and all its cache properly on an Ubuntu 24.04 system?

Thanks for you help 🙏

---

_Comment by @konstin on 2025-04-24 08:01_

Even though we generally develop and test for a single user scenario, with permissions set so that all users have read and write access, there is nothing preventing a multi-user scenario with a shared `UV_CACHE_DIR`.

---

_Comment by @angel-devicente on 2025-05-02 06:38_

Hello,

I'm also considering the use of "uv" system-wide, but I was hoping "uv" would have a tiered cache system, so that I could have a global read-only cache with all the common packages, while users could have their own local cache for packages not found in the global cache. Is something like this possible or planned?

Thanks

---

_Comment by @konstin on 2025-05-02 08:08_

We currently don't have any plans for a tiered cache.

---

_Comment by @angel-devicente on 2025-05-02 15:32_

And there is no other way to cater for that scenario, right? (i.e. a read-only selection of packages which users can re-use to create their own virtual environments without wasting space for each user?)

---

_Comment by @oraluben on 2025-06-03 02:43_

> Even though we generally develop and test for a single user scenario, with permissions set so that all users have read and write access, there is nothing preventing a multi-user scenario with a shared `UV_CACHE_DIR`.

Hi @konstin , I found this issue while looking for a global uv cache dir. By default folder created by each user are not writable by other users, so if user A uses `uv pip install` and create a, let's say `/var/cache/uv`, other users are not able to update the cache. Do you have some idea how to workaround this?

---

_Comment by @zanieb on 2025-06-03 03:08_

@oraluben there's some commentary in https://github.com/astral-sh/uv/issues/5611

---
