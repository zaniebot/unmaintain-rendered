```yaml
number: 9645
title: "Post-`uv sync` hook for code generation pre development"
type: issue
state: open
author: galah92
labels:
  - needs-design
assignees: []
created_at: 2024-12-04T18:13:04Z
updated_at: 2025-09-02T23:15:22Z
url: https://github.com/astral-sh/uv/issues/9645
synced_at: 2026-01-10T03:23:53Z
```

# Post-`uv sync` hook for code generation pre development

---

_Issue opened by @galah92 on 2024-12-04 18:13_

Hi guys, this is a feature request.

I have some `.proto` files (Protobuf) I'd like to generate Python code for, at some point in time before the code runs.
A hook after `uv sync` would be a perfect place to run such code generation task.

Is it possible to implement something like that?
If not, do you see any other elegant solution? Ideally it would be something the developers won't need to execute independently before running the actual code.

Thanks.

---

_Comment by @zanieb on 2024-12-04 18:31_

I like the idea, but there's not a clear way to do it right now we'd need to design it.

Related #5903 

---

_Label `needs-design` added by @zanieb on 2024-12-04 18:31_

---

_Comment by @galah92 on 2024-12-04 18:42_

I understand. I'll be happy to contribute something like this myself once there's an agreeable design for it.

---

_Comment by @afeblot on 2024-12-19 12:15_

I would have used such a hook to apply a patch file on a broken dependency installed in the .venv.

---

_Comment by @rhuanbarreto on 2025-04-05 20:42_

Something like the postinstall hook from node would be interesting in uv IMO. We have some tasks that we need to run right after we run `uv sync`

---

_Comment by @dmartin-gh on 2025-08-11 13:22_

+1 on this feature. Having some trouble coming up with a clean solution to injecting our company's self-sign certificates into certifi. Would love to do that as a postinstall hook to `uv sync`. It would allow us to remove a _lot_ of `verify=ctx` SSL context objects we've had to create all over our code when instantiating httpx clients.

---

_Comment by @80avin on 2025-09-02 23:15_

Another very strong usecase is having pre-commit hooks.
If we keep the work of initializing pre-commit hooks manual, most of the contributor will forget it. Therefore, rendering it useless. It is best if such work can be done in a post-install/sync job.

[PDM](https://pdm-project.org/en/latest/usage/scripts/#pre-post-scripts) already has a similar support.

---
