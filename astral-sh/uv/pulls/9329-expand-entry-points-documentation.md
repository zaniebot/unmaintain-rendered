```yaml
number: 9329
title: Expand entry points documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-projects-entry
created_at: 2024-11-21T17:20:25Z
updated_at: 2024-11-28T02:47:18Z
url: https://github.com/astral-sh/uv/pull/9329
synced_at: 2026-01-10T12:00:00Z
```

# Expand entry points documentation

---

_Pull request opened by @zanieb on 2024-11-21 17:20_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-11-21 17:20_

---

_Comment by @zanieb on 2024-11-22 16:26_

I'm a little hesitant to spend times on the plugin entry-points and gui scripts but it is relevant?

---

_Marked ready for review by @zanieb on 2024-11-22 16:26_

---

_Review requested from @charliermarsh by @zanieb on 2024-11-26 18:36_

---

_Review requested from @konstin by @zanieb on 2024-11-26 18:36_

---

_Comment by @konstin on 2024-11-26 18:55_

Personally, I'm fine with not discussing plugin entrypoints in uv, they may be explained by the app that wants to load that information.

---

_Review comment by @konstin on `docs/concepts/projects/config.md`:67 on 2024-11-26 18:59_

I'd put this front in this section, we can even roll the entire section into a paragraph at the end of the previous paragraph:

> On windows, `[project.scripts]` will create a terminal. If you're writing a GUI application and don't want a terminal on Windows, use `[project.gui-scripts]` instead.

---

_@konstin approved on 2024-11-26 18:59_

---

_Comment by @CarrotManMatt on 2024-11-27 00:46_

I think it would be worthwhile to have a mention (even if it is brief) to application entry-points. Although the more detailed documentation can be found in the pyproject.toml specification & writing guide, I am sure a lot of new users will learn about the features of a pyproject.toml only from uv's documentation. If we can at least say those features are available and signpost where to find out more that would be very useful.

I speak from what I would have found useful when I was learning. Hatch's pyproject.toml documentation is very good (possibly because it makes use of the plugin system in its build backend hatchling), whereas flake8 makes use of the plugin system and has quite outdated and lacking documentation in my opinion.

Perhaps full examples are not necessary, but I think a mention to them would be very valuable

---

_Review comment by @CarrotManMatt on `docs/concepts/projects/config.md`:96 on 2024-11-27 00:49_

I think some grammatical improvements can be made:
```markdown
The `group` key can be an arbitrary value, it does not need to include the package name or
"plugins". However, it is recommended to namespace the key by the package name to avoid collisions
```

---

_@CarrotManMatt requested changes on 2024-11-27 00:49_

---

_Review comment by @zanieb on `docs/concepts/projects/config.md`:96 on 2024-11-27 15:50_

Sorry I read this twice but can't spot the suggestion; what are you thinking?

---

_@zanieb reviewed on 2024-11-27 15:50_

---

_Merged by @zanieb on 2024-11-27 15:53_

---

_Closed by @zanieb on 2024-11-27 15:53_

---

_Branch deleted on 2024-11-27 15:53_

---

_@hutcho reviewed on 2024-11-28 02:23_

---

_Review comment by @hutcho on `docs/concepts/projects/config.md`:96 on 2024-11-28 02:23_

The text in the commit has possible typos such as missing words. Perhaps `it does need` should become `it does **not** need`. Same with `is recommended` to `**it** is recommended`.

---

_@zanieb reviewed on 2024-11-28 02:47_

---

_Review comment by @zanieb on `docs/concepts/projects/config.md`:96 on 2024-11-28 02:47_

Ah I see, thanks!

---
