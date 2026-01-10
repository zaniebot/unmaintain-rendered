---
number: 16994
title: Better UX and/or documentation to avoid dev dependencies in production
type: issue
state: open
author: jayqi
labels:
  - documentation
assignees: []
created_at: 2025-12-05T05:18:18Z
updated_at: 2025-12-16T12:06:38Z
url: https://github.com/astral-sh/uv/issues/16994
synced_at: 2026-01-10T01:26:12Z
---

# Better UX and/or documentation to avoid dev dependencies in production

---

_Issue opened by @jayqi on 2025-12-05 05:18_

### Summary

I just realized that I have been running my dev dependency group in production for my Django application for the last several months because I do:

```
uv sync --no-dev
uv run manage.py collectstatic --no-input
uv run manage.py migrate --no-input
uv run -m gunicorn project.wsgi:application -c scripts/gunicorn_config.py
```

It turns out `uv run` will implicitly do a sync and so my dev dependencies end up getting installed. From a quick search through the issues, I've found at least three other issues where others report running into the same thing.

- https://github.com/astral-sh/uv/issues/13694
- https://github.com/astral-sh/uv/issues/12558
- https://github.com/astral-sh/uv/issues/16071

I understand why it works this way, but I think this is clearly a gotcha that has potential security implications. It's not obvious that a command named `run` will sync dependencies (even though it's actually very nice for development that it does). Looking through the documentation, I don't see anywhere that calls this out as a pitfall, and I don't see any documentation specifically geared towards best practices on managing environments for production application deployments. I imagine I can't be the only one with a production application with dev dependencies active which can introduce security vulnerabilities.


---

_Label `enhancement` added by @jayqi on 2025-12-05 05:18_

---

_Label `enhancement` removed by @konstin on 2025-12-16 12:06_

---

_Label `documentation` added by @konstin on 2025-12-16 12:06_

---
