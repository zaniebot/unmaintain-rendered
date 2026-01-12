```yaml
number: 7214
title: Add excluded dependencies
type: issue
state: closed
author: MrPandir
labels:
  - enhancement
  - wish
  - needs-design
assignees: []
created_at: 2024-09-09T11:01:23Z
updated_at: 2026-01-03T07:05:34Z
url: https://github.com/astral-sh/uv/issues/7214
synced_at: 2026-01-12T15:59:11Z
```

# Add excluded dependencies

---

_@MrPandir_

I want to request functionality to exclude dependencies, even if it's a sub dependency.

I would like to see the `--excluded` flag in the `uv add` command, and `excluded-dependencies` in `pyproject.toml`.

This functionality is in <samp>rye</samp>, but not in <samp>uv</samp> for some reason. Is there any reason why it's not there, or are there other ways to avoid installing sub-dependencies? Let me know.

I have seen the `uv sync --no-install-package` parameter, but this solution only affects the virtual environment, and is not locked in `pyproject.toml`.

---

_Comment by @zanieb on 2024-09-09 11:42_

Can you explain your use-case some more? 

Is this a duplicate of https://github.com/astral-sh/uv/issues/4422?

---

_Comment by @elohmeier on 2024-09-09 12:52_

I think the use case is to prevent installing transitive dependencies. My use case for this is very similar. I need to use torch, which I've added to my `project.dependencies`. Since I'm not using GPU-acceleration, I would like to exclude its `nvidia` dependency to shave off 2.6 GiB of the resulting venv size.

---

_Label `question` added by @charliermarsh on 2024-09-09 13:18_

---

_Comment by @MrPandir on 2024-09-10 09:47_

> Can you explain your use-case some more?

The example given by @elohmeier perfectly illustrates one of the scenarios where this functionality would be useful. In my case, I primarily need this feature to reduce the size of Docker images. It's unnecessary to download and store libraries that will never be used in the project.

> Is this a duplicate of https://github.com/astral-sh/uv/issues/4422?


Regarding whether this is a duplicate of #4422, I'm not certain it is, as this issue proposes a different approach to excluding dependencies.

I've come across one solution that currently works:

```toml
[tool.uv]
override-dependencies = [
    "polyfactory ; sys_platform == 'never'",
    "jinja2 ; sys_platform == 'never'",
]
```

However, this solution is not currently documented, and I find it not very intuitive. Moreover, it doesn't provide a convenient CLI solution for adding or removing items from this list.

When I first saw the `sys_platform == 'never'` construct, I was quite confused about its purpose. Without additional explanation, this construction is not obvious. It would be much clearer to have a list like:

```toml
[tool.uv]
excluded-dependencies = [
    "polyfactory",
    "typing-extensions",
    "faker",
    "jinja2",
]
```

This format is more straightforward and easier to understand at a glance. It would also be beneficial to have corresponding CLI commands to manage this list easily.

---

_Label `question` removed by @zanieb on 2024-10-21 21:44_

---

_Label `enhancement` added by @zanieb on 2024-10-21 21:44_

---

_Label `wish` added by @zanieb on 2024-10-21 21:44_

---

_Comment by @zanieb on 2024-10-21 21:44_

I still think this is roughly a duplicate of #4422 but I'll leave it open for now.

---

_Label `needs-design` added by @zanieb on 2024-10-21 21:44_

---

_Comment by @dsully on 2024-11-08 21:49_

This doesn't appear to work when running `uv lock` or `uv sync`.

```toml
[tool.uv]
override-dependencies = [
    "futures; sys_platform == 'never'",
]
```

```shell
$ uv sync
⠦ futures==3.3.0
  × Failed to download and build `futures==3.3.0`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

      [stderr]
      This backport is meant only for Python 2.
      It does not work on Python 3, and Python 3 users do not need it as the concurrent.futures package is available in the standard library.
      For projects that work on both Python 2 and 3, the dependency needs to be conditional on the Python version, like so:
      extras_require={':python_version == "2.7"': ['futures']}
```

What's the right work around here? Thanks

---

_Comment by @zanieb on 2024-11-08 22:14_

Can you share a minimal `pyproject.toml`?

---

_Comment by @dsully on 2024-11-09 03:54_

I think I got it sorted. PEBKAC.

---

_Comment by @andreaimprovised on 2025-01-22 03:51_

This would be really useful when there are transitive dependencies that are not copyright compatible and not necessary for your project to run.

---

_Comment by @zanieb on 2025-01-22 06:21_

We already support this; per the linked issue.

---

_Closed by @zanieb on 2025-01-22 06:21_

---

_Comment by @andreaimprovised on 2025-01-22 06:51_

Acknowledged.

That said, we're not currently using pip (or uv pip) to install dependencies in our target environments: local and docker. I'd like to avoid doing so if possible.

---

_Comment by @MrPandir on 2025-01-23 15:20_

@zanieb I don't think this solved the issue.

Perhaps I didn't emphasize this enough in my messages, but I still wanted to see a way to specify a list of excluded dependencies in the `pyproject.toml` file.

One of the possible designs for implementing this feature already exists in rye, which suits me perfectly. It's this specific design that I would like to see in uv.


---

_Comment by @ion-elgreco on 2025-01-31 13:23_

@zanieb is `--no-install-package` a uv sync only feature? Seems it can be also useful for a simple uv pip install

---

_Comment by @charliermarsh on 2025-10-31 14:18_

We just added support for excluding dependencies entirely by name: https://github.com/astral-sh/uv/pull/16528 (available in the next release)

---

_Comment by @futurewasfree on 2026-01-03 07:05_

I'm trying to catch up on this thread and was wondering what the resolution was regarding the CLI key or environment variable. Why not support that as well?
This could be useful in a CI context as well

---
