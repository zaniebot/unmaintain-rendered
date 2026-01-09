---
number: 12435
title: Allow passing --quiet (and --verbose) via environmental variables
type: issue
state: open
author: liiight
labels:
  - enhancement
assignees: []
created_at: 2025-03-24T15:56:18Z
updated_at: 2025-09-09T07:07:09Z
url: https://github.com/astral-sh/uv/issues/12435
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow passing --quiet (and --verbose) via environmental variables

---

_Issue opened by @liiight on 2025-03-24 15:56_

### Summary

I often tend to chain multiple uvx commands together and add add the `--quiet` flag to them like so:
```shell
uvx --quiet cowsay -t "moo" | uvx --quiet --from terminaltexteffects tte burn
```
Having the ability to set the quiet flag once via an environmental variable would be nice.

Thanks in advance!

### Example

```shell
export UV_OPTION_QUIET=1
uvx cowsay -t "moo" | uvx --from terminaltexteffects tte burn
```
I added the `--verbose` flag to the title even though I have no particular usage for it but I imagine the same treatment can be applied to both cases.

---

_Label `enhancement` added by @liiight on 2025-03-24 15:56_

---

_Comment by @zanieb on 2025-03-24 16:21_

`UV_QUIET` and `UV_VERBOSE` seem fine to me — we just need to make sure like `UV_VERBOSE=3` is equivalent to `uv -vvv` or similar.

---

_Comment by @K-Saikrishnan on 2025-09-09 07:04_

This feature would be much welcomed :)

This is my current workflow:
* Capture all the relevant project cmds in a [.justfile](https://just.systems/) as a transparent documentation in single location
* Establish working commands in the `.justfile`
* To prevent long outputs in CI and docker builds, duplicate each command separately with `-q` flag

It'd be great to prevent that duplication and overhead
We can just toggle the env var and subsequent cmd outputs would become less noisy

---
