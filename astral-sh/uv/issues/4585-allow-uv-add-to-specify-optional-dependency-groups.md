---
number: 4585
title: "Allow `uv add` to specify optional dependency groups"
type: issue
state: closed
author: blueraft
labels:
  - enhancement
  - needs-design
  - preview
assignees: []
created_at: 2024-06-27T11:45:21Z
updated_at: 2024-06-28T01:24:23Z
url: https://github.com/astral-sh/uv/issues/4585
synced_at: 2026-01-10T01:23:39Z
---

# Allow `uv add` to specify optional dependency groups

---

_Issue opened by @blueraft on 2024-06-27 11:45_

I didn't see an issue related to this so figured I'd create one.

`pdm` allows extra dependency groups by  by providing -G/--group <name> option, similar functionality for `uv` would be nice to have.


[pdm reference](https://pdm-project.org/en/latest/usage/dependency/#add-dependencies)
 


---

_Comment by @charliermarsh on 2024-06-27 17:40_

I think you should rephrase the issue around supporting dependency groups if that's what you're looking for. We started with a single dev dependency group for simplicity (though internally it's implemented more generically) and `uv add` already supports `--dev`. So the request here is really around supporting multiple dependency groups. We want to get more signal on why that's necessary before increasing the user complexity.

---

_Comment by @blueraft on 2024-06-27 19:19_

I think this is not really about dependency groups as in poetry, if that's what you're referring to here.

This is about adding additional dependencies to extra groups defined in `optional-dependencies`. 

```toml
[project]
name = "foo"
version = "0.1.0"
dependencies = ["requests"]

[project.optional-dependencies]
scraping = ["beautifulsoup4"]
```

In the above case `pdm add -G scraping selenium` adds `selenium` to the scraping group.

```toml
[project]
name = "foo"
version = "0.1.0"
dependencies = ["requests"]

[project.optional-dependencies]
scraping = [
    "beautifulsoup4",
    "selenium>=4.22.0",
]
```

Of course it's fine not to support that, I can manually add it in the toml file.


---

_Comment by @charliermarsh on 2024-06-27 19:31_

Oh sorry, I misunderstood -- you want to add to an extra group! Yes this should be supported. I thought it was but looks like not yet.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2024-06-27 19:32_

---

_Label `enhancement` added by @charliermarsh on 2024-06-27 19:34_

---

_Label `preview` added by @charliermarsh on 2024-06-27 19:34_

---

_Comment by @blueraft on 2024-06-27 19:38_

All good, perhaps `--group/-G` is not the best name for this but I just went with the `pdm` way  🤷‍♂️

---

_Label `needs-design` added by @zanieb on 2024-06-27 19:55_

---

_Referenced in [astral-sh/uv#4607](../../astral-sh/uv/pulls/4607.md) on 2024-06-28 00:01_

---

_Closed by @ibraheemdev on 2024-06-28 01:24_

---

_Closed by @ibraheemdev on 2024-06-28 01:24_

---
