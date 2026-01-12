```yaml
number: 16993
title: "`uv activate` could print an error message with the appropriate `source .venv/bin/activate` command"
type: issue
state: open
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-12-05T05:10:02Z
updated_at: 2026-01-09T23:04:44Z
url: https://github.com/astral-sh/uv/issues/16993
synced_at: 2026-01-12T16:02:41Z
```

# `uv activate` could print an error message with the appropriate `source .venv/bin/activate` command

---

_@charliermarsh_

A suggestion that came up in an in-person conversation. Even if we can't support `uv activate`, we could at least print a message telling the user how to activate it.

---

_Label `error messages` added by @charliermarsh on 2025-12-05 05:10_

---

_Comment by @nooscraft on 2025-12-05 09:41_

```
$ uv activate
error No virtual environment found in the current directory or any parent directory.

hint Create a virtual environment with: uv venv
```
Something like above ^^ @charliermarsh 

---

_Comment by @nooscraft on 2025-12-05 13:29_

## Example Outputs

### When a virtual environment exists:
```bash
$ uv activate
Activate with: source .venv/bin/activate
```

### When no virtual environment is found:
```bash
$ uv activate
error No virtual environment found in the current directory or any parent directory.

hint Create a virtual environment with: uv venv
```

### Help output:
```bash
$ uv activate --help
Show how to activate a virtual environment

Usage: uv activate [OPTIONS]
...
```

@charliermarsh FYI!

---

_Comment by @zanieb on 2025-12-05 14:01_

This has already come up a few times before and is very similar to https://github.com/astral-sh/uv/issues/14038.

I'm pretty wary to add a top-level command that just prints the activation command.

---

_Comment by @charliermarsh on 2025-12-05 14:03_

I don’t think it has to be command. Can’t it just be a more helpful error message?

---

_Comment by @nooscraft on 2025-12-05 14:31_

There's no command

When uv activate is run and a venv exists:
  ```
$ uv activate  
  Activate with: source .venv/bin/activate
```

When uv activate is run and no venv exists:
```
 $ uv activate  
error No virtual environment found in the current directory or any parent directory.  hint Create a virtual environment with: uv venv
```

---

_Comment by @disouzam on 2025-12-31 00:43_

I'm pretty new to uv. Had looked through the documentation now (and some weeks before) and now in this issue and also in #14038. However, I couldn't answer the question: what do you people use to activate a recently created environment? The old `source {os-dependent path}/activate` (in bash, for example)? Or another command?





---

_Comment by @konstin on 2026-01-06 16:59_

With `uv run`, you don't need to activate environments by hand. Otherwise, the old `source .venv/bin/activate` remains working.

---

_Comment by @disouzam on 2026-01-06 17:09_

Understood, @konstin! Thanks! I ended up creating an alias in bash for the old command. 

---

_Comment by @gregglind on 2026-01-09 21:08_

+1 on having `uv activate` do *something* rather than error. 

That something could be:

* give the source command (as described above) 
* explain `uv run`
* (or both!)

When I `activate`, it's because I want the cli commands and entry points to be available, including any entry points for the project, `ipython`, `python` and others.  I don't want to preface everything with `uv run`.  


---

_Comment by @disouzam on 2026-01-09 23:04_

Yeah, @gregglind ! Although I have learned to internalize those differences and keep moving forward, it would be good to have a uniformity across the ecosystem! I don't fear these frictions anymore while they are still tiresome but for novices / newcomers, this certainly keeps cognitive load at high values! 

Do you have any knowledge on the internals of uv? As far as I know (I also checked on repo's home page), it is written in rust. I don't have any experience right now to try to make a pull request with proposed changes. But I'm ready to test / give opinions if matter.

---
