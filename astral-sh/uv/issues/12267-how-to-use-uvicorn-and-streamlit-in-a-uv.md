```yaml
number: 12267
title: How to use uvicorn and streamlit in a UV environment?
type: issue
state: open
author: dyuzhou
labels:
  - needs-mre
  - external
assignees: []
created_at: 2025-03-18T09:50:56Z
updated_at: 2025-04-14T08:24:07Z
url: https://github.com/astral-sh/uv/issues/12267
synced_at: 2026-01-12T16:00:59Z
```

# How to use uvicorn and streamlit in a UV environment?

---

_@dyuzhou_

### Summary

I use uvicorn and streamlit in my project. The command is started normally when using the caonda environment. However, the following error is thrown when using UV. I don't know if it is a problem with my usage or something else? How should I use uv run uvicorn app:app --workers 2?

Magic number 'UVSC' or 'UVPY' not found at the end of the file. Did you append the magic number, the length and the path to the python executable at the end of the file?



### Platform

Windows 11 X86

### Version

uv 0.6.2

### Python version

Python 3.10

---

_Label `bug` added by @dyuzhou on 2025-03-18 09:50_

---

_Comment by @samypr100 on 2025-03-19 11:33_

I tried it out and I can run uvicorn fine on windows, can you share the command steps by step that led you there?

---

_Label `bug` removed by @konstin on 2025-03-19 11:33_

---

_Label `needs-mre` added by @konstin on 2025-03-19 11:33_

---

_Comment by @johannesloibl on 2025-03-27 10:36_

We got the same error but in a Hatch context (Hatch v1.14.0):

![Image](https://github.com/user-attachments/assets/6d3990ea-7437-431a-9874-276fdd9dabd1)

FYI @ofek 

---

_Comment by @ofek on 2025-03-27 12:17_

Can't help without a reproducible example.

---

_Comment by @johannesloibl on 2025-03-27 13:00_

> Can't help without a reproducible example.

Sry this is also not reproducible on my PC but only occurring for a colleague. I hoped you were maybe seeing this error message somewhere in the code, but i expect it has nothing to to with Hatch, just wanted you to be aware.

Weirdly calling ``hatch build`` has no problem, but running it through a hatch script 

![Image](https://github.com/user-attachments/assets/b92ca1b7-8236-4da4-aa77-215b97d33867) named "publish" it fails on ``hatch build``.

---

_Label `external` added by @zanieb on 2025-04-01 19:46_

---

_Comment by @dyuzhou on 2025-04-14 02:16_

I think I found this problem very easily using it on docker.
In docker, use uv add streamlit, then use streamlit run app.py

---

_Comment by @konstin on 2025-04-14 08:24_

Can you share your Dockerfile and a set of minimal files you add during the docker build?

---
