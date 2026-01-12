```yaml
number: 9452
title: Before posting in the issue tracker
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2024-11-26T21:15:37Z
updated_at: 2025-10-15T13:01:23Z
url: https://github.com/astral-sh/uv/issues/9452
synced_at: 2026-01-12T15:59:51Z
```

# Before posting in the issue tracker

---

_@zanieb_

👋 Thanks for engaging with uv! As maintainers, we must split our time between solving existing issues (e.g., by adding features and fixing bugs) and responding to new issues. We care a lot about responding to issues promptly. **Help us out by opening high quality issues, identifying related issues, and ensuring that your issue is not a duplicate.**

## When to open a new issue

Before opening a new issue, please **[search for a similar issue](https://github.com/astral-sh/uv/issues?q=is%3Aissue%20%20sort%3Arelevance-desc%20)**. GitHub's search is frequently unreliable, we struggle to find issues we know exist, so we understand if you have a hard time finding an existing issue. The preceding link includes closed issues and sorts by the "best match". We've also included some common issues below. It's also important to **search the [documentation](https://docs.astral.sh/uv/)**; we often see issues that are answered there.

If you find a similar issue, please **confirm that your issue is the same**. If you are not certain if your issue is the same, it is best to create a new issue instead. Including a link to possible existing issues can save us a lot of time. Some error messages can look similar, e.g., during build failure, but we often need more information to help diagnose your issue and it's best to avoid side-tracking an existing discussion.

If your request or problem is the same as an existing issue, please **upvote the original post with a 👍** instead of adding a comment like "+1" or "I'm encountering this too". If you do comment, please make sure you're adding new context to the conversation. Similarly, please do not request status updates on issues — if progress has been made, the issue is usually updated.

If you encountered the same problem or have the same question as someone else, consider creating a new issue with a **suggestion to improve the experience**. For example, uv could show a different error message that would help explain its behavior. In this way, we can collaborate to prevent confusion in the first place and make uv easier to use. 

## What information to include

When reporting a problem or asking a question, **be sure to include the following**:

- The version of uv
- The operating system
- The command that you ran
- The output of the command with the `--verbose` flag

Including this information can save a lot of back and forth and we'll be able to solve your problem faster.

The **best issues include a _minimal reproduction_ of the problem**. A minimal reproduction is a _small_ example that demonstrates the problem. This can take a few forms:

- A repository on GitHub and a command to run
- A Docker image and command
- A list of commands to run, e.g., `uv init example && cd example && uv add ...`

If your project is open source, it can be helpful to link to your _actual_ GitHub repository you are encountering the problem in. However, this is not a _minimal_ example — isolating the issue in a simple case without extraneous details is better. Including both can help us narrow down the problem. **See our [reproducible examples guide](https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/) for examples** and more details.

A few things to keep in mind:

- If you're not on the latest version of uv, **make sure that your issue is still present on the latest version**.
- If the behavior worked on a previous version of uv and _regressed_ in a new version, please share all the versions you have tested. This can help track down the cause of the bug. Regressions are generally given higher priority.
- Please **avoid screenshots**, and use markdown formatted text. Instead, copy the output text from your terminal. This improves discoverability via search and makes it easier for maintainers to interact with the output.
- You can get more logs by repeating the `-v` flag, e.g., `-vvv`. These logs can be very verbose, we recommend putting them in a [`<details>` block](https://gist.github.com/ericclemmons/b146fe5da72ca1f706b2ef72a20ac39d) or a [gist](https://gist.github.com).
- If you're reporting a difference from pip, please be sure to share both the `uv pip` and `pip` commands used and their output. Read the [pip compatibility guide](https://docs.astral.sh/uv/pip/compatibility/) to ensure the difference is not intentional.

See issues with the [`great writeup` label](https://github.com/astral-sh/uv/issues?q=label%3A%22great+writeup%22+) for examples. 

## Popular requests

Check if your request has already been made:

- https://github.com/astral-sh/uv/issues/6265
- https://github.com/astral-sh/uv/issues/1419
- https://github.com/astral-sh/uv/issues/5903
- https://github.com/astral-sh/uv/issues/1404
- https://github.com/astral-sh/uv/issues/1910
- https://github.com/astral-sh/uv/issues/1703
- https://github.com/astral-sh/uv/issues/1495
- https://github.com/astral-sh/uv/issues/6794
- https://github.com/astral-sh/uv/issues/5734

See the [most upvoted issues](https://github.com/astral-sh/uv/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) for more.

## Common duplicates

Check if your problem has been previously discussed:

- https://github.com/astral-sh/uv/issues/1331
- https://github.com/astral-sh/uv/issues/4022
- https://github.com/astral-sh/uv/issues/2252
- https://github.com/astral-sh/uv/issues/2500

See previous [duplicate issues](https://github.com/astral-sh/uv/issues?q=is%3Aissue+label%3Aduplicate) for more.

------------

_Want to provide feedback on this document? See https://github.com/astral-sh/uv/issues/9453_

---

_Locked by @astral-sh on 2024-11-26 21:15_

---
