```yaml
number: 4321
title: "With rye {root:uri}/../../packages/ seems to traverse more than 2 folders up"
type: issue
state: open
author: ShravanSunder
labels:
  - needs-mre
assignees: []
created_at: 2024-06-14T00:14:31Z
updated_at: 2024-06-14T12:04:52Z
url: https://github.com/astral-sh/uv/issues/4321
synced_at: 2026-01-10T05:31:37Z
```

# With rye {root:uri}/../../packages/ seems to traverse more than 2 folders up

---

_Issue opened by @ShravanSunder on 2024-06-14 00:14_

i'm trying to use rye sync with a python monorepo and root:uri

the resolution of folders doesn't seem to work.  for example
```
    "askluna-dotenv @ {root:uri}/../../packages/dotenv" => error: Distribution not found at: file:///Users/shravansunder/Documents/dev/project-dev/askluna-project/packages/dotenv
error: could not write production lockfile for workspace

    "askluna-dotenv @ {root:uri}/../packages/dotenv" =>error: Distribution not found at: file:///Users/shravansunder/Documents/dev/project-dev/askluna-project/askluna/apps/packages/dotenv

```

So for some reason, `../../` seems to traverse by more than 2 folders up instead of exactly 2 folders.   And `../` only traverses 1 folder up

---

_Comment by @charliermarsh on 2024-06-14 00:18_

Isn't `{root:uri}` a Hatch thing? What does it resolve to?

---

_Comment by @charliermarsh on 2024-06-14 00:23_

I don't know that uv has any control over how that URL gets resolved. I believe we just ask Hatch for the metadata, and it returns the resolved file path.

---

_Label `needs-mre` added by @charliermarsh on 2024-06-14 04:02_

---

_Comment by @ShravanSunder on 2024-06-14 12:04_

i'll create a issue in their repo as well.  https://github.com/pypa/hatch/issues/1570

Related question @charliermarsh so with uv (and ruff) whats the recommended mechanism to point to a local package in a monorepo.  I've searched all the issues and unable to find a solution that doesn't use absolute paths from user folder.  

I can make a related discussion if that's better for this

---
