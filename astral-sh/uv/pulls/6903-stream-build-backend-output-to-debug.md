```yaml
number: 6903
title: "Stream build backend output to `debug!`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - tracing
assignees: []
merged: true
base: main
head: charlie/build-output
created_at: 2024-08-31T23:49:04Z
updated_at: 2024-09-05T11:52:29Z
url: https://github.com/astral-sh/uv/pull/6903
synced_at: 2026-01-12T16:07:35Z
```

# Stream build backend output to `debug!`

---

_@charliermarsh_

## Summary

We need to decide whether we want this in `debug!` or `tracing!`. We also _probably_ (?) want to show this by default in `uv build`.

Closes https://github.com/astral-sh/uv/issues/1567.

Closes https://github.com/astral-sh/uv/issues/5893.


---

_Label `tracing` added by @charliermarsh on 2024-08-31 23:49_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-31 23:49_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-31 23:49_

---

_Review requested from @konstin by @charliermarsh on 2024-08-31 23:49_

---

_Comment by @charliermarsh on 2024-08-31 23:51_

![Screenshot 2024-08-31 at 7 48 30 PM](https://github.com/user-attachments/assets/e984ae52-11c4-45c1-a2be-08b72da96aba)


---

_Comment by @charliermarsh on 2024-09-01 16:19_

Confusingly these failing tests pass on my machine.

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:1031 on 2024-09-02 08:47_

We need to forward that error

---

_@konstin approved on 2024-09-02 08:50_

---

_Comment by @konstin on 2024-09-02 08:51_

Do we need to prefix builds when multiple ones are running in parallel, or generally to identify which one is which?

---

_Comment by @konstin on 2024-09-02 08:52_

Tests are also passing on my linux machine

---

_Comment by @charliermarsh on 2024-09-02 18:22_

> Do we need to prefix builds when multiple ones are running in parallel, or generally to identify which one is which?

I thought about this, but if you run `-vvv`, the tree structure seems fine to me.

---

_Comment by @charliermarsh on 2024-09-02 18:52_

Ok, figured it out -- I was dropping output from one stream as soon as the other completed.

---

_Merged by @charliermarsh on 2024-09-02 19:46_

---

_Closed by @charliermarsh on 2024-09-02 19:46_

---

_Branch deleted on 2024-09-02 19:46_

---

_Comment by @henryiii on 2024-09-03 06:34_

> We also probably (?) want to show this by default in uv build.

Remember, this is the _only_ way for a build backend like scikit-build-core, maturin, hatching, etc. to communicate with the user. Modern build backends are fairly careful with output, trying to just display useful information to the user, like compiler warnings, enabled components, etc. In my opinion, `uv build .` and `uv pip install .` should show output. I even think `uv pip install numpy` should show output **if** it's building a wheel, but that's probably too large a departure from pip and what people know.  (Remember, most of the time it won't build!)

I think the reason it's hidden by default is a) setuptools produces a ton of pretty useless output, like every file it copies, b) pip was designed in a time when pre-compiled wheels for everything didn't exist, which meant every install produced stuff, while today most packages have wheels, and c) dependency output could get confusing. Very little of this makes sense, though, in the modern most-packages-have-wheels world, where most SDist-only packages are compiled packages where output is both helpful and keeps the terminal from just sitting idle for seconds to minutes (to hours in rare cases). But in the case of building from a source directory, I think it's easy to say hiding the build-backend output is never helpful. And again, this is the _only_ way to see the build-backend produce output. You only interact to build backend via `pip install .` and `build .`!

Two issues with this as well, by the way. It looks like every line gets a ~~long~~ prefix with logging info (Correction: it appears to just be "DEBUG", which isn't bad), and it looks like the build backend can't tell things like the terminal width and color support. scikit-build-core, cmake, and most compilers all support color output, but it's gone with `uv`. What about also providing a flag that disables capturing the subprocess output? Pip has the same problem, but it only adds a couple of spaces as a prefix. This is why build's output is so much better than pips, as it doesn't buffer it and modify it.

Here's an example of build's output for boost-histogram, with scikit-build-core's logging enabled:

![Screenshot 2024-09-03 at 2 14 28 AM](https://github.com/user-attachments/assets/6f2284a4-a00c-47dd-b965-07b84f629b42)

Now here's uv's current support:

![Screenshot 2024-09-03 at 2 17 42 AM](https://github.com/user-attachments/assets/0251e5b2-d3d2-46fc-98f4-6d8794578a86)
![Screenshot 2024-09-03 at 2 18 20 AM](https://github.com/user-attachments/assets/23ce0f03-b209-4dc1-a7f1-8f89a873064a)

There's about two pages of uv logging before the build backend info starts, it's got a DEBUG prefix on everything, and all color is gone.

Also, I noticed another problem. Without scikit-build-core's logging enabled, this is the entire output from `pipx run build --wheel`:

![Screenshot 2024-09-03 at 2 32 22 AM](https://github.com/user-attachments/assets/25f303c1-8411-4b1e-926f-3397c5d93868)


Notice that Ninja is rewriting its output if each file is successfully compiled, keeping the log short, because it knows it's writing to a terminal. However, you can't do that when you buffer! Here's the output part from uv:

![Screenshot 2024-09-03 at 2 27 19 AM](https://github.com/user-attachments/assets/9deab271-cde9-4720-949d-3c2737548915)

Having a special flag to disable capturing the output would fix that too.

Edit: https://github.com/astral-sh/uv/pull/6912 looks better, not sure about it supporting terminal detection though.

---

_Comment by @henryiii on 2024-09-05 05:30_

Actually, this is what it looks like for me now with 0.4.5:

![Screenshot 2024-09-05 at 1 28 10 AM](https://github.com/user-attachments/assets/db2c62d0-5270-4524-9030-9c7ea8d45c1b)

Every line is three lines, two of them with DEBUG prefixes.

---

_Comment by @charliermarsh on 2024-09-05 11:52_

That’s strange, I’ll take a look. I don’t see that for other backends.

---
