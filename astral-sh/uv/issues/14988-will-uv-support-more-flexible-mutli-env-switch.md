```yaml
number: 14988
title: Will uv support more flexible mutli-env switch  like conda?
type: issue
state: closed
author: FuRuF-11
labels:
  - enhancement
assignees: []
created_at: 2025-07-31T07:48:14Z
updated_at: 2025-07-31T13:43:32Z
url: https://github.com/astral-sh/uv/issues/14988
synced_at: 2026-01-12T16:02:01Z
```

# Will uv support more flexible mutli-env switch  like conda?

---

_@FuRuF-11_

### Summary

After a period of use, I would like to say that uv is the ultimate python package solver.

But I still  feel uncomfortable about the way uv handle with mutli-env.

uv allow user create another venv in workspace, but this new venv will not be directly used by uv even after activated. Unless you specific the python interpreter.

This is a very counter-intuitive setup. Because it's obvious that user want to use the new venv directly after activate it.

By my view, this is because uv insist on  "one project, on env" idea. But, I would still suggest that uv should allow more flexible conda-like venv switch. As this will not only benefits the user, but also will  attract more conda users.

### Example

After  `uv venv env_name` and `env_name/bin/activate`, the `uv run` should directly use new env's python interpreter not the .venv's python. 



---

_Label `enhancement` added by @FuRuF-11 on 2025-07-31 07:48_

---

_Comment by @zanieb on 2025-07-31 11:42_

We're trying to move to ecosystem in away from manually constructing and activating virtual environments. 

What's your use-case for creating a second environment in a project? Have you tried using the `--active` flag?


---

_Comment by @FuRuF-11 on 2025-07-31 12:22_

> We're trying to move to ecosystem in away from manually constructing and activating virtual environments.
> 
> What's your use-case for creating a second environment in a project? Have you tried using the `--active` flag?

I see. And yes, I tried `--active`, but still I would argue this is not a intuitive setup.

I want make a project to do some practice on different RL architectures  (like a RL Gym) and compare those stuff's performance . And I was kind worry about these architectures conflict with each other. So I try to use multi-env with uv. 

And here we are.



---

_Comment by @zanieb on 2025-07-31 12:24_

Conflict in what sense? Why not put them in different dependency groups then use `uv run --group ...` to select them?

---

_Comment by @FuRuF-11 on 2025-07-31 13:43_

> Conflict in what sense? Why not put them in different dependency groups then use `uv run --group ...` to select them?

Thanks for your suggestion.

I realized that I was still using conda-mind. I know how uv handle with these problems now.   

---

_Closed by @FuRuF-11 on 2025-07-31 13:43_

---
