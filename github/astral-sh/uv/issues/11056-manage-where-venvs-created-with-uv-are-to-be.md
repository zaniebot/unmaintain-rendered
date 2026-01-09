---
number: 11056
title: manage where venvs created with uv are to be stored
type: issue
state: closed
author: krauspe
labels:
  - question
assignees: []
created_at: 2025-01-29T11:06:03Z
updated_at: 2025-07-05T08:50:00Z
url: https://github.com/astral-sh/uv/issues/11056
synced_at: 2026-01-07T13:12:18-06:00
---

# manage where venvs created with uv are to be stored

---

_Issue opened by @krauspe on 2025-01-29 11:06_

### Question

Hi guys,

I need to determine where venvs are located in the file system. 
I don't want python venvs to get installed in any users home directory, because I need to package them in a rpm file including my application, which has to be globally accessible for multiple users.
I can't find a proper way to do this in the documentation and if this is possible at all.

please help

### Platform

AlmaLinux 9.x, CentOS 7.9

### Version

0.5.24

---

_Label `question` added by @krauspe on 2025-01-29 11:06_

---

_Comment by @charliermarsh on 2025-01-29 13:55_

Are you looking for [`UV_PROJECT_ENVIRONMENT`](https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path)?

---

_Comment by @zanieb on 2025-01-29 14:12_

See also https://github.com/astral-sh/uv/issues/1495

---

_Comment by @krauspe on 2025-02-01 11:27_

> Are you looking for [`UV_PROJECT_ENVIRONMENT`](https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path)?

I'm not sure if that solves my problem.

But however I managed to create an environment in my  application path (so far not using the concept of a 'project' in the sense of uv concepts.)
But the python executable are always symbolic links to a python binary in a uv related path in the users home dir.(sorry don't remember exactly since beeing not in y office) 
Maybe --relocatable is my solution, but I'm not sure.
Is there a step by step intro to create an isolated venv starting from scratch which, when ready does not need any internet access ?

---

_Comment by @krauspe on 2025-02-01 11:31_

> See also https://github.com/astral-sh/uv/issues/1495

No, what i want is exactly the opposite. I want everything included in my project folder. Not even symlinks to any directory outside that folder. This is what I always got using uv so far.

---

_Comment by @zanieb on 2025-02-01 17:36_

> But the python executable are always symbolic links to a python binary in a uv related path in the users home dir.(sorry don't remember exactly since beeing not in y office)

This is how Python virtual environments work. They link to the Python installation on the system. You shouldn't need the internet. What do you mean?

Sounds like you want #7865 

---

_Closed by @zanieb on 2025-02-03 21:34_

---

_Comment by @krauspe on 2025-07-05 08:50_

@zanieb, sorry for my extremely late answer and yes that's exactly what I was looking for. 👍

---
