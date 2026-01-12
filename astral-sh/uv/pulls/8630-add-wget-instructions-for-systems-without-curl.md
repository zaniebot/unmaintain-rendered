```yaml
number: 8630
title: Add wget instructions for systems without curl
type: pull_request
state: merged
author: joshmcorreia
labels:
  - documentation
assignees: []
merged: true
base: main
head: feature/add_wget_instructions
created_at: 2024-10-28T04:56:20Z
updated_at: 2024-11-13T17:51:31Z
url: https://github.com/astral-sh/uv/pull/8630
synced_at: 2026-01-12T16:08:24Z
```

# Add wget instructions for systems without curl

---

_@joshmcorreia_

## Summary
Adds wget instructions for linux installations that don't come with `curl`.

## Test Plan
This was tested on Ubuntu 20.04, Ubuntu 22.04, Ubuntu 24.04, and Debian 11.


---

_Comment by @zanieb on 2024-10-28 13:53_

We want to keep the README as small as possible here. Maybe in the documentation Installation guide instead? I'm a bit hesitant though.

---

_Label `documentation` added by @zanieb on 2024-10-28 13:53_

---

_Comment by @zanieb on 2024-10-28 13:53_

Thanks for contributing to the project :)

---

_Comment by @joshmcorreia on 2024-10-28 18:31_

> Maybe in the documentation Installation guide instead? I'm a bit hesitant though.

Is there a reason you're hesitant to add it to the installation guide?

---

_Comment by @zanieb on 2024-10-28 18:51_

It's just more content for people to read and it hasn't come up so far.

---

_Comment by @joshmcorreia on 2024-10-28 19:16_

It came up for me on a docker container without curl and I created the PR because there wasn't any existing documentation on it. Up to you on if you want to close it or not - at least if someone else faces the same problem as me they'll be able to search "wget" and find this solution.

---

_Comment by @samypr100 on 2024-11-04 02:20_

@joshmcorreia Indeed, I think it's still worth adding possibly as a note in [installer.md](https://github.com/astral-sh/uv/blob/main/docs/configuration/installer.md) instead of the repo's [README](https://github.com/astral-sh/uv/blob/main/README.md).

---

_Merged by @zanieb on 2024-11-13 17:51_

---

_Closed by @zanieb on 2024-11-13 17:51_

---
