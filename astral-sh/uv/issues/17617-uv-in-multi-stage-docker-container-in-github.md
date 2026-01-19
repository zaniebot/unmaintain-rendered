```yaml
number: 17617
title: uv in multi-stage docker container in github workflow
type: issue
state: closed
author: pstokkink
labels:
  - question
assignees: []
created_at: 2026-01-19T17:02:41Z
updated_at: 2026-01-19T17:12:09Z
url: https://github.com/astral-sh/uv/issues/17617
synced_at: 2026-01-19T17:26:21Z
```

# uv in multi-stage docker container in github workflow

---

_@pstokkink_

### Question

I'm switching a project over to uv and overall it is working great! 👏 

We have a github action that spins up a few containers through a compose file and then runs tests inside the container (we need to have access to a postgres database, which runs in one of the containers). The application container (`web`) has a multi-stage build and sets the user to something other than root. 

I chowned the entire venv and application folders in the container to the non-root user and its group.

```
RUN chown user:user /app
COPY --from=builder --chown=user:user /app /app
```

Now when I do the following in my github workflow:
``` 
- docker compose -f "docker-compose.yml" up -d --build
- docker compose exec -T web uv run pytest
```
The latter fails with: 
`error: failed to remove file `/app/.venv/lib/python3.14/site-packages/package-name-0.1.dist-info/INSTALLER`: Permission denied (os error 13)`

When I build it locally, it is owned by the user until I do a `RUN uv run src/manage.py` and afterwards it is owned by root again.  

I've tried the `--no-project` and `--no-build` flags to no avail. What am I doing wrong? I can't find a clear answer in the documentation on this. 

### Platform

ubuntu-latest

### Version

0.9.25

---

_Label `question` added by @pstokkink on 2026-01-19 17:02_

---

_Closed by @pstokkink on 2026-01-19 17:11_

---

_Comment by @pstokkink on 2026-01-19 17:12_

I was my own rubber duck here. Running the manage.py with --no-project --no-build resolved the issue

---
