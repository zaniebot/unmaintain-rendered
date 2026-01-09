---
number: 13644
title: no versions of psycopg3???
type: issue
state: closed
author: MalikRumi
labels:
  - question
assignees: []
created_at: 2025-05-25T19:38:33Z
updated_at: 2025-05-26T13:56:02Z
url: https://github.com/astral-sh/uv/issues/13644
synced_at: 2026-01-07T13:12:18-06:00
---

# no versions of psycopg3???

---

_Issue opened by @MalikRumi on 2025-05-25 19:38_

### Question

While trying to create a new pyproject.toml in a pre-existing project, I decided to add the latest version of the PostgreSQL adapter, which would be psycopg3. However, I got this instead:

╰─▶ Because there are no versions of psycopg3 and you require psycopg3>=1.0, we can conclude that your 
requirements are unsatisfiable.

What do you mean 'no versions'? I don't understand this at all. psycopg3 has been out for a while. It's on pypi.  They are up to version 3.2.9. Or did this mean there were no versions on my machine? But that doesn't make sense either, because this was a request to put it on my machine.

I just put >=1.0 because that was before I looked it up, and I figured 1.0 was a safe guess

Your clarifying words of wisdom greatly appreciated.


--




### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @MalikRumi on 2025-05-25 19:38_

---

_Comment by @charliermarsh on 2025-05-25 20:26_

Are you looking for a different package? I only see one version, published in 2013, for https://pypi.org/project/psycopg3/.

---

_Comment by @charliermarsh on 2025-05-25 20:26_

I think it's published under `psycopg`: https://pypi.org/project/psycopg/

---

_Comment by @jules-ch on 2025-05-26 11:57_

Yes you're looking at `psycopg` package, it was renamed for the v3.

---

_Closed by @charliermarsh on 2025-05-26 13:56_

---
