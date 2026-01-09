---
number: 989
title: Choose a badge for Ruff
type: issue
state: closed
author: ofek
labels:
  - documentation
assignees: []
created_at: 2022-12-01T23:25:30Z
updated_at: 2023-01-06T05:33:08Z
url: https://github.com/astral-sh/ruff/issues/989
synced_at: 2026-01-07T13:12:14-06:00
---

# Choose a badge for Ruff

---

_Issue opened by @ofek on 2022-12-01 23:25_

Like https://hatch.pypa.io/dev/next-steps/#community

---

_Label `documentation` added by @charliermarsh on 2022-12-02 18:25_

---

_Comment by @ofek on 2022-12-14 00:19_

Any thoughts? 🙂 

---

_Comment by @smackesey on 2022-12-14 19:46_

When there is a badge/logo/icon, it would also be nice to have it added to [Buildkite Emojis](https://github.com/buildkite/emojis#contributing-new-emoji) for Buildkite CI.

---

_Comment by @charliermarsh on 2022-12-17 04:20_

I want this but I need a logo first!

---

_Referenced in [astral-sh/ruff#1535](../../astral-sh/ruff/issues/1535.md) on 2023-01-01 21:38_

---

_Comment by @charliermarsh on 2023-01-01 21:39_

@ofek - Do you have any pointers on how you went about creating a badge? Any docs or references you could share?

---

_Comment by @ofek on 2023-01-01 22:35_

Only https://github.com/pypa/hatch/issues/370

---

_Comment by @saadmk11 on 2023-01-02 14:08_

@charliermarsh This is an example from black https://github.com/psf/black#show-your-style

### Ruff Example:

**For README.md:**

```md
[![Linter: Ruff](https://img.shields.io/badge/Linter-Ruff-brightgreen?style=flat-square)](https://github.com/charliermarsh/ruff)
```

**For README.rst:**

```
.. image:: https://img.shields.io/badge/Linter-Ruff-brightgreen?style=flat-square
    :target: https://github.com/charliermarsh/ruff
```

**Looks like this:**

[![Linter: Ruff](https://img.shields.io/badge/Linter-Ruff-brightgreen?style=flat-square)](https://github.com/charliermarsh/ruff)

### Add Custom Logo:

> `?logo=data:image/png;base64,…`
> 
> Insert custom logo image (≥ 14px high). There is a limit on the total size of request headers we can accept (8192 bytes). From a practical perspective, this means the base64-encoded image text is limited to somewhere slightly under 8192 bytes depending on the rest of the request header.

**Source:** https://shields.io/category/build

---

_Comment by @charliermarsh on 2023-01-04 04:19_

This is what I'm considering:

[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/charliermarsh/ruff/main/assets/badge/v1.json)](https://github.com/charliermarsh/ruff)


---

_Referenced in [astral-sh/ruff#1642](../../astral-sh/ruff/pulls/1642.md) on 2023-01-04 22:26_

---

_Closed by @charliermarsh on 2023-01-04 22:27_

---

_Comment by @ofek on 2023-01-04 23:09_

Is there a way you can encode that in a single URL so it doesn't have to hit an endpoint every time?

---

_Comment by @charliermarsh on 2023-01-04 23:20_

I think shields.io caches the call?

---

_Comment by @charliermarsh on 2023-01-04 23:20_

I thought this was preferable to base64-encoding the SVG logo in the URL.

---

_Comment by @charliermarsh on 2023-01-06 04:01_

Just confirming that these are cached on the CDN, so there shouldn't be any endpoint penalty AFAIU:

<img width="375" alt="Screen Shot 2023-01-05 at 11 00 35 PM" src="https://user-images.githubusercontent.com/1309177/210927502-c1c00bae-315c-4a08-80a2-349e3dca090f.png">


---

_Comment by @ofek on 2023-01-06 05:33_

Awesome!

---
