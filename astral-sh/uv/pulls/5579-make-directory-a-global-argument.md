```yaml
number: 5579
title: "Make `--directory` a global argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/directory
created_at: 2024-07-29T23:30:48Z
updated_at: 2024-07-29T23:58:18Z
url: https://github.com/astral-sh/uv/pull/5579
synced_at: 2026-01-12T16:06:54Z
```

# Make `--directory` a global argument

---

_@charliermarsh_

## Summary

Cargo makes this global (and uses the same technique). It's still hidden so we can always decide to remove it.


---

_Label `cli` added by @charliermarsh on 2024-07-29 23:31_

---

_Label `preview` added by @charliermarsh on 2024-07-29 23:31_

---

_Marked ready for review by @charliermarsh on 2024-07-29 23:33_

---

_Merged by @charliermarsh on 2024-07-29 23:43_

---

_Closed by @charliermarsh on 2024-07-29 23:43_

---

_Branch deleted on 2024-07-29 23:43_

---

_Comment by @bluss on 2024-07-29 23:44_

Changing directory is a different thing and makes uv run harder to use like that (if you pass relative paths to an uv run command, it might not work so well.)

---

_Comment by @charliermarsh on 2024-07-29 23:45_

What's harder about it? Why is it wrong to resolve paths relative to the provided directory?

---

_Comment by @charliermarsh on 2024-07-29 23:45_

(I mean, it's easy to change -- we just track a current directory globally in `uv-fs`, add a setter, and always read from there rather than using `CWD`.)

---

_Comment by @bluss on 2024-07-29 23:51_

I'm thinking that you have something like

```bash
$ uv --directory path/to/proj run the_command --flag ./myfile.txt
# <------- uv run part ---------> <-- regular command part  ---->
```

Ideally the uv run part should just be transparent. It brings forth the environment in where the program executes. The regular command line part of the command shouldn't have to change.

One reason why it's wrong to resolve myfile.txt relative to the given directory is that the user probably autocompleted it from their current directory. It's hard to use interactively (and hard to use in scripts as well, except there you'd probably just have to remember to abspath everything.)

---

_Comment by @charliermarsh on 2024-07-29 23:58_

That makes sense. I can change it up.

---
