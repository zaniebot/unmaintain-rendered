```yaml
number: 11624
title: "Respect `requires-python` upper bounds on first-party packages"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-02-19T15:33:59Z
updated_at: 2025-02-19T17:09:13Z
url: https://github.com/astral-sh/uv/issues/11624
synced_at: 2026-01-10T03:50:31Z
```

# Respect `requires-python` upper bounds on first-party packages

---

_Issue opened by @zanieb on 2025-02-19 15:33_

We ignore `requires-python` upper bounds during resolution for various reasons (discussed much elsewhere), however, it can be confusing when we apply this to a first party package, e.g.,

```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12,<3.13"
dependencies = []
```

```
❯ uv venv -p 3.13
Using CPython 3.13.2
warning: The requested interpreter resolved to Python 3.13.2, which is incompatible with the project's Python requirement: `==3.12.*`
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install -e .
Resolved 1 package in 0.78ms
      Built example @ file:///Users/zb/workspace/uv/example
Prepared 1 package in 548ms
Installed 1 package in 1ms
 + example==0.1.0 (from file:///Users/zb/workspace/uv/example)
```

I think we should consider respecting `requires-python` upper bounds in this context, as an up-front check rather than a resolver behavior. I think we'd apply this to the target package of install operations (including tool installs).

See

- https://github.com/astral-sh/uv/issues/11612#issuecomment-2668992030

---

_Label `needs-decision` added by @zanieb on 2025-02-19 15:34_

---

_Comment by @notatallshaw on 2025-02-19 16:12_

What about throwing an error or a warning when the user puts an upper bound at all? Even if it matches the current Python environment.

I'm about to write a pre-PEP on the semantics of Requires-Python, and I was going to say that tools should error, warn, or ignore upper bounds. It would be good to know if there are edge cases to this.

---

_Comment by @zanieb on 2025-02-19 16:20_

Well, until there's an alternative to `project.requires-python` that allows people to specify a single Python version for their application, it's not reasonable to say you can't use `requires-python = "==3.11.*"` or whatever. People want to be able to encode that their application runs on a single Python version, which I think is quite reasonable. Published packages are a separate problem.

---

_Comment by @notatallshaw on 2025-02-19 16:40_

> Well, until there's an alternative to project.requires-python that allows people to specify a single Python version for their application

Could you make a uv specific option for that?

> , it's not reasonable to say you can't use requires-python = "==3.11.*" or whatever. 

Oh dear, the PEP is going to say exactly that, both it's going to say the version should be on the language level not the release level (i.e. should only be 3.11, not 3.11.*) and the only operators defined will be >, >=, and !=.

> People want to be able to encode that their application runs on a single Python version, which I think is quite reasonable. Published packages are a separate problem.

Understood, but the consensus is, and the direction given by the author of the spec, is that Requires-Python was not intended for that.

I'll consider your feedback when I start writing the PEP (in about a week) and make sure astral are fully in the loop, might not be accepted, but the consensus was pretty strong on the last discussion thread (https://discuss.python.org/t/requires-python-and-pre-release-python-versions/62959).

I'll definitely add this use case as a rejected idea, mostly because it treats Requires-Python as meaning different things on different contexts, and see what the consensus ends up being on that.


---

_Comment by @zanieb on 2025-02-19 17:09_

> Could you make a uv specific option for that?

Certainly, we're considering it. We're dragging our feet because it'd be uv-specific though and it conflicts with  the `.python-version` file which is vaguely respected by other tools.

My point was mostly in response to changing the standard — the standard probably shouldn't change without an alternative? I strongly agree this isn't what `Requires-Python` is for, but that's all users have today. It would resolve all the confusion about the overlap between `Requires-Python` and the `.python-version` files though! which would be nice.




---
