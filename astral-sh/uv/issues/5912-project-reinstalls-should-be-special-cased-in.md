```yaml
number: 5912
title: Project reinstalls should be special cased in install summary
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-08-08T14:28:20Z
updated_at: 2024-08-31T23:18:21Z
url: https://github.com/astral-sh/uv/issues/5912
synced_at: 2026-01-12T15:59:00Z
```

# Project reinstalls should be special cased in install summary

---

_@zanieb_

e.g.

```
❯ uv init
Initialized project `example`

❯ uv sync
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved 1 package in 3ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 152ms
Installed 1 package in 0.94ms
 + example==0.1.0 (from file:///Users/zb/workspace/example)

❯ uv add anyio
Resolved 4 packages in 5ms
   Built example @ file:///Users/zb/workspace/example
Prepared 4 packages in 151ms
Uninstalled 1 package in 0.34ms
Installed 4 packages in 2ms
 + anyio==4.4.0
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
 + idna==3.7
 + sniffio==1.3.1

❯ uv add anyio
Resolved 4 packages in 2ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 152ms
Uninstalled 1 package in 0.41ms
Installed 1 package in 0.89ms
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)

❯ uv remove anyio
Resolved 1 package in 5ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 152ms
Uninstalled 4 packages in 3ms
Installed 1 package in 0.99ms
 - anyio==4.4.0
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
 - idna==3.7
 - sniffio==1.3.1
```

It's really noisy to see `example` (my own project) being reported as added and removed on every operation.

---

_Label `cli` added by @zanieb on 2024-08-08 14:29_

---

_Label `preview` added by @zanieb on 2024-08-08 14:29_

---

_Comment by @charliermarsh on 2024-08-09 00:19_

Do you think we should omit them? Would it be solved by creating a better UI in general for reinstalls (i.e., one line that summarizes the reinstall, rather than a removal and addition)?

---

_Comment by @zanieb on 2024-08-09 13:18_

I think it's probably safe to omit entirely in this case?

---

_Comment by @charliermarsh on 2024-08-09 13:20_

I worry that's going to cause more confusion. Alternatively we could _not_ reinstall the project in these cases and use a more specific cache key (e.g., the `pyproject.toml` content rather than the timestamp).

---

_Comment by @charliermarsh on 2024-08-09 13:20_

Wait sorry, that wouldn't work either, the `pyproject.toml` _is_ changing.

---

_Comment by @zanieb on 2024-08-09 13:44_

In what case would it be important that the editable install of my package was re-created?

---

_Comment by @zanieb on 2024-08-09 13:45_

Notably, I think we cover "something happens during build" with the "Built example @ file:///Users/zb/workspace/example" line.

---

_Comment by @charliermarsh on 2024-08-09 13:45_

If you defined new entry points, for example. Those need to be added to the bin directory of the virtualenv. 

---

_Comment by @zanieb on 2024-08-09 13:52_

That doesn't seem critical to display on every dependency change though?

---

_Comment by @charliermarsh on 2024-08-09 14:00_

When do we include the project(s) in the output, then? I also want to improve this but I think it's fairly nuanced... If the answer is "we never show it", what happens in the following cases:

- First time you sync a project, and it has no dependencies (so we'd show nothing, even though we installed the project).
- You do a strict sync with `--package`, and _want_ to know which workspace members were added or removed.
- You run with `--reinstall-package my-package`, and want visual confirmation that the package was reintalled.

I also worry about showing it conditionally, since then users will be unsure when / if the project was reinstalled.

Would a better solution be to... rebuild and reinstall the project less often?


---

_Comment by @zanieb on 2024-08-09 14:47_

1) We should show installing the project if it was _not_ installed
2) We should show adding and removing workspace members, but not reinstalling editable workspace members
3) Hm...

I think as a starting point, we could improve the reinstall output so it only shows a single line then see how obtrusive it is?

Reducing the rebuilds and reinstalls seems complicated, I don't see how we can avoid a rebuild when the pyproject.toml changes, e.g., adding a dependency.



---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-13 13:38_

---

_Comment by @charliermarsh on 2024-08-13 13:39_

I'll play around with this today. I know it can be improved, though I'm not sure what the best approach is.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @charliermarsh on 2024-08-31 23:18_

This happened thanks to @zanieb.

---

_Closed by @charliermarsh on 2024-08-31 23:18_

---
