---
number: 6926
title: Guidance on project management
type: issue
state: open
author: DataEnggNerd
labels:
  - documentation
assignees: []
created_at: 2024-09-02T04:01:02Z
updated_at: 2024-09-05T11:19:48Z
url: https://github.com/astral-sh/uv/issues/6926
synced_at: 2026-01-07T13:12:17-06:00
---

# Guidance on project management

---

_Issue opened by @DataEnggNerd on 2024-09-02 04:01_

For the people who are entering python ecosystem for first time through job, including me, are not clear in managing virtual environments.

There is a legacy application with a clumsy requirements.txt file, which has to be installed in virtual environment. That's all the knowledge about virtual environments. When I am going through documentation of UV, most of the jargons are new and bit confusing when to use what! It's quite embarrassing to be honest. 

I am proposing to create a guideline document, which will help in understanding package management, creating virtual environments, managing requirements, when to use pyproject.toml, why not to use requirements.txt etc. 

This may not be a big ask, but the result would obviously guide many beginners to understand what is the standard way to create, handle and manage packages by UV.

---

_Label `documentation` added by @charliermarsh on 2024-09-02 17:16_

---

_Comment by @DataEnggNerd on 2024-09-03 03:16_

@charliermarsh Do you mind me taking this task up? We can connect over a meet and I shall write up your thoughts. 

---

_Comment by @zanieb on 2024-09-03 17:34_

Let's try to chat through it asynchronously first. I think in part, this would be solved by #5200 but I'd love to hear more about what specifically was confusing.

The `uv pip` guide at https://docs.astral.sh/uv/pip/ should be a good starting place for understanding how to use your `requirements.txt` with a virtual environment.

---

_Comment by @DataEnggNerd on 2024-09-03 18:13_

@zanieb Thanks for reverting quick. I believe there is a communication gap here. I am actually seeking for a "opinionated" standard for managing python projects from UV. It started with [this tweet](https://x.com/tusharisanerd/status/1829284911641252176. ). 

As a developer who have worked based on what it has been a standard at work, most of the time using `requirements.txt`, I am interested in knowing what will be right standard for managing projects? And why? what are the flaws in other approaches? 


---

_Comment by @Cherrymelon on 2024-09-04 03:36_

pip use requirements.txt，but pip just record all dependency pkg and their sub dependency pkg plain, not tree structure,when you work in the huge project,more than one hundred lines pkg will let manage pkg version not friendly,and requirements.txt can not handle version conflict smartly.
In one word, requirements.txt like a log,not a smart package manage tools

---

_Comment by @DataEnggNerd on 2024-09-04 04:18_

@Cherrymelon Completely agree. So what should be the "right" way? 

---

_Comment by @Cherrymelon on 2024-09-05 01:46_

IMO,this topic may be too wide,such as add pkg that include two path,one is  switching from exist project,another one  is start from new project, the former need `uv add -r /path/to/pyproject.toml`  or `uv add -r <requirements.txt` with removing duplicated sub pkg,the latter just run `uv init` or `uv init /path/your_work_pkg_path` for pkg level development in big team.

This just one little part of add pkg,besides, like the deploy action will be different in container and normal server machine , I think considering the most common situation and control  the size of  exposed information  also important and make reading friendly.

Anyway,it is a high-valued work

---

_Comment by @schlich on 2024-09-05 03:55_

The [Python Packaging User Guide](https://packaging.python.org/en/latest/) is a very good resource on this and getting better all the time.  I think a visible reference to this would suffice for most beginners.

---

_Comment by @DataEnggNerd on 2024-09-05 11:19_

@schlich Thanks for the suggestion. For a beginner this can be overwhelming and most of the content seems to be for the python packages, but not the projects we use in our workspace or so. 

---
