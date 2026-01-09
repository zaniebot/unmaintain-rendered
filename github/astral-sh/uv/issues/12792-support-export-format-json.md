---
number: 12792
title: "Support `export --format=json`"
type: issue
state: open
author: InSyncWithFoo
labels:
  - enhancement
assignees: []
created_at: 2025-04-09T21:53:21Z
updated_at: 2025-07-04T07:32:55Z
url: https://github.com/astral-sh/uv/issues/12792
synced_at: 2026-01-07T13:12:18-06:00
---

# Support `export --format=json`

---

_Issue opened by @InSyncWithFoo on 2025-04-09 21:53_

I'm writing [a VS Code extension for uv](https://github.com/InSyncWithFoo/uv-for-vscode). One of the features it supports is editor commands, which helps with running uv commands from the editor. For `uv remove`, the user will choose the packages to remove from a given list.

To generate this list, I have been using `pip list`, but it also lists transitive dependencies and other installed packages. `export` does what I want, but there's only one output format, `requirements.txt`, which is not easily parsable.

`--format=json` should be supported.


---

_Label `enhancement` added by @InSyncWithFoo on 2025-04-09 21:53_

---

_Comment by @charliermarsh on 2025-04-10 14:18_

What would be the difference between parsing this JSON and parsing the `uv.lock` directly?

---

_Comment by @InSyncWithFoo on 2025-04-10 15:51_

That approach has a few drawbacks:

* The extension is written in JS/TS, which has native support for parsing JSON but not TOML.
* To parse `uv.lock`, I will have to find out where it is, then read it from JS/TS code. This, aside from being a less robust reimplementation of something uv can do, is a chance for subtle bugs.
* `uv.lock`'s format is documented as "[specific to uv and not usable by other tools](https://docs.astral.sh/uv/concepts/projects/layout/#the-lockfile)".

---

_Comment by @zanieb on 2025-04-10 15:54_

> uv.lock's format is documented as "[specific to uv and not usable by other tools](https://docs.astral.sh/uv/concepts/projects/layout/#the-lockfile)".

Note this is in reference to other package managers and is targeting explaining the concept to beginners. The lockfile schema is stable and versioned.

---

_Comment by @InSyncWithFoo on 2025-04-10 15:59_

> The lockfile schema is stable and versioned.

Thanks for the correction. Regardless, maintaining a TypeScript mirror won't be easy and I would prefer not having to do so.

---

_Comment by @kiran-4444 on 2025-05-08 05:29_

I'd like to work on this if this is planned.

---

_Comment by @zanieb on 2025-06-13 12:02_

I don't think I'm opposed per say, but want buy-in from other maintainers. @Gankra / @konstin ?

---

_Comment by @konstin on 2025-06-13 12:20_

Which packages `uv remove` can remove is tied to the internals of uv and may change with new versions, an example was adding support for dependency groups. For an IDE extension that has to integrate deeply with uv, I'd recommend reading the toml directly including handling of different format versions. Once you integrate deeply enough, you usually need to go below the main abstraction layer anyway.

If we want to improve IDE integration, I'd go for a dedicated hidden subcommand where we can add hooks, which could also prepare for deeper LSP integration, but that's a big project and need more design upfront than currently feasible.

We could export more dependency graph information, but `uv export` feels like the wrong subcommand for it, maybe `uv tree`? `uv export` currently specifically target format that are read by other tools, while `uv tree` is more general dependency information.

---

_Comment by @InSyncWithFoo on 2025-07-04 07:32_

Now that `uv export` supports `pylock.toml` as a format, this issue has become less relevant. I'll keep it open though, just in case someone feels strongly about it.

---
