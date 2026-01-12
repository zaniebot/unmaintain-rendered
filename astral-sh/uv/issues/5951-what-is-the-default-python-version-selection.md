```yaml
number: 5951
title: "What is the default Python version selection strategy for ``uvx``?"
type: issue
state: closed
author: FishAlchemist
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-09T07:33:07Z
updated_at: 2025-10-27T13:11:27Z
url: https://github.com/astral-sh/uv/issues/5951
synced_at: 2026-01-12T15:59:00Z
```

# What is the default Python version selection strategy for ``uvx``?

---

_@FishAlchemist_

How does ``uvx`` determine which Python version to use when ``--python`` is not explicitly specified?
I had a quick look through the document, but I couldn't find anything about it.

**Note:** The document version I'm looking for is the one generated using commit 21408c1f351b159a9b590bcfefcfe645838e33c1

---

_Comment by @zanieb on 2024-08-09 13:42_

We follow the default Python discovery rules, so we'll use the first version we find either

1) The newest installed managed Python version
2) The first system Python installation on the PATH regardless of version



---

_Label `documentation` added by @zanieb on 2024-08-09 13:42_

---

_Label `question` added by @zanieb on 2024-08-09 13:42_

---

_Assigned to @zanieb by @zanieb on 2024-08-09 13:42_

---

_Comment by @FishAlchemist on 2024-08-09 17:31_

> We follow the default Python discovery rules, so we'll use the first version we find either
> 
> 1. The newest installed managed Python version
> 2. The first system Python installation on the PATH regardless of version

If UV first analyze the requirements to determine the supported Python version range, and then run the default Python discovery rules based on the results, would it be a better way to find the Python version?

However, it seems like Pip can still download packages even if they aren't explicitly categorized for a specific Python version. For instance, MonkeyType's highest categorized version is Python 3.10, but it can be installed in a Python 3.12 environment.

Despite the questions I raised about this find strategy, it seems to have had no practical impact.
### [MonkeyType - PyPI](https://pypi.org/project/MonkeyType/)

---

_Comment by @zanieb on 2024-08-09 17:34_

The supported Python range of... all possible versions of the requirements? Then we try to find out if you have that installed? I don't think that's really tractable, but we have a bigger problem: we need a Python interpreter for markers to figure out what your platform is — we usually need a Python interpreter before we can start resolution for an installation.

We ignore upper bounds on package's Python requirements, generally.

---

_Comment by @FishAlchemist on 2024-08-09 17:42_

> The supported Python range of... all possible versions of the requirements? Then we try to find out if you have that installed? I don't think that's really tractable, but we have a bigger problem: we need a Python interpreter for markers to figure out what your platform is — we usually need a Python interpreter before we can start resolution for an installation.
> 
> We ignore upper bounds on package's Python requirements, generally.

Explicitly specifying --python for this issue might be the best solution. 
Therefore, should the document mention that uvx uses Python discovery rules? 
If a specific version is required, --python should be explicitly specified.

---

_Comment by @zanieb on 2024-08-09 17:46_

I guess I feel like that's implicitly documented? Nearly every command uses Python discovery rules. I can try to add some clarity about that to the documentation though.

---

_Comment by @FishAlchemist on 2024-11-21 01:21_

Discovery of Python versions:
https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions

---

_Closed by @FishAlchemist on 2024-11-21 01:21_

---

_Comment by @raffaem on 2025-10-27 13:11_

You can specify it on the command line:

```
uv tool run --python 3.13 ocrmypdf
```

---
