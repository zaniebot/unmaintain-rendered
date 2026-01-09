---
number: 13863
title: Cached venv in script mode
type: issue
state: closed
author: lucharo
labels:
  - question
assignees: []
created_at: 2025-06-05T15:15:26Z
updated_at: 2025-06-26T16:09:17Z
url: https://github.com/astral-sh/uv/issues/13863
synced_at: 2026-01-07T13:12:18-06:00
---

# Cached venv in script mode

---

_Issue opened by @lucharo on 2025-06-05 15:15_

I recently realised I could use `uv` for system scripts with shebangs and I absolutely love it for scripts I'm quickly iterating over, like:


```python
#!/usr/bin/env uv run --script --with typer 

import typer
...
# my typer cli
```

I find this to be a beautiful pattern but I realised that the temporary venv is getting built every time I run the command. I'm just wondering if there's a way to cache the venv (with some TTL maybe) that gets created or a different optimisation I might be missing.

---

_Comment by @zanieb on 2025-06-05 16:15_

We do cache this, if I run with the `-v` flag I can see the cached environment used on a subsequent invocation.

You might also like https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies

---

_Label `question` added by @zanieb on 2025-06-05 16:15_

---

_Comment by @lucharo on 2025-06-05 18:35_

Oh yeah! Of course I could have inline dependencies as well! Ok that's great, thanks @zanieb 👌🏼

---

_Closed by @lucharo on 2025-06-06 06:08_

---

_Comment by @lucharo on 2025-06-09 13:26_

Sorry @zanieb, where actually do you add the `-v` flag? I couldn't see it in the documentation and I'm just getting more verbose output. 

Also if I use the inline dependencies will the dependencies installed in the existing project/venv be used too? 

---

_Comment by @zanieb on 2025-06-09 14:17_

The `-v` flag does just enable verbose output, which enables you to see that that the same environment is used in each invocation.

> Also if I use the inline dependencies will the dependencies installed in the existing project/venv be used too?

No, it'll be isolated from those.



---

_Comment by @lucharo on 2025-06-26 16:09_

Hey @zanieb, I realised they only get cached if I have inline dependencies properly setup, i.e.: 

```sh
#!/usr/bin/env -S uv run --script 
# /// script
# dependencies = [
#   "requests<3",
#   "rich",
# ]
# ///
```

but the venv doesn't seem to get cached if you have just the dependencies in the CLI call, **is that expected behaviour?**

```sh
#!/usr/bin/env -S uv run --script --with rich --with requests
``` 

---
