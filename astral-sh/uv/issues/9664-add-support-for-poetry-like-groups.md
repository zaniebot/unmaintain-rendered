```yaml
number: 9664
title: Add support for Poetry-like groups
type: issue
state: closed
author: fre-edin
labels:
  - question
assignees: []
created_at: 2024-12-05T22:54:27Z
updated_at: 2024-12-06T11:26:31Z
url: https://github.com/astral-sh/uv/issues/9664
synced_at: 2026-01-12T15:59:55Z
```

# Add support for Poetry-like groups

---

_@fre-edin_

Purpose: faster Docker build for increased iteration speed when working with Docker development images.

**Background:**
Resolving package dependencies means looking at all packages to find the correct versions. This is done by having a lock file, which is then updated each time I make a change in the packages. 

When I have a Docker development image I enjoy installing big and slowly changing packages such as torch first, and smaller and more fast-changing packages later. That way, when I add a package my docker layer does not get invalidated and I can rebuild my image much faster, increasing iteration speed.

So how can I resolve my dependencies without invalidating the Docker cached layers?

**Solution:**
Instead of using the lock file itself inside the Docker image, I first export from the lock file using 
`poetry export --without-hashes --with heavy-builds -o requirements-heavy-builds.txt`

And then I have `requirements-heavy-builds.txt` in my Docker file.

That way, unless the versions of the packages in `requirements-heavy-builds.txt` change, the Docker cache does not get invalidated and I keep iteration speed high.

**Request:**
To support this workflow in uv by adding the commands:
- `uv export --without-hashes --with heavy-builds -o requirements-heavy-builds.txt`
- `uv add --group heavy-builds`


---

_Comment by @charliermarsh on 2024-12-05 23:59_

I think all of this is supported today, unless I'm misunderstanding? `uv export --group heavy-builds ...` and `uv add --group` already exists.

---

_Label `question` added by @charliermarsh on 2024-12-05 23:59_

---

_Comment by @fre-edin on 2024-12-06 07:12_

Thank you!

I must somehow have missed this. Thank you for a great product.

Fredrik


fre 6 dec. 2024 kl. 01:00 skrev Charlie Marsh ***@***.***>:

> I think all of this is supported today, unless I'm misunderstanding? uv
> export --group heavy-builds ... and uv add --group already exists.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/9664#issuecomment-2521758012>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/BMJPLYXT6QAASWAZT2RZTE32EDSIHAVCNFSM6AAAAABTDQDLH6VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDKMRRG42TQMBRGI>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Closed by @charliermarsh on 2024-12-06 11:26_

---
