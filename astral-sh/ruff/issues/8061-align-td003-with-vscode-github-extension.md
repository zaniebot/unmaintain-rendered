```yaml
number: 8061
title: "Align `TD003` with VSCode github extension automatic issue creation"
type: issue
state: closed
author: MartinBernstorff
labels:
  - bug
assignees: []
created_at: 2023-10-19T09:19:24Z
updated_at: 2025-01-15T23:47:34Z
url: https://github.com/astral-sh/ruff/issues/8061
synced_at: 2026-01-12T15:54:47Z
```

# Align `TD003` with VSCode github extension automatic issue creation

---

_@MartinBernstorff_

Rule: https://docs.astral.sh/ruff/rules/missing-todo-link/#missing-todo-link-td003

In VSCode, if I have a TODO without an issue number, it suggests creating an issue in the quick fix menu:

![image](https://github.com/astral-sh/ruff/assets/8526086/75e94f65-b515-48dc-94b1-593721501ada)

However, the syntax does not match with TD003.

![image](https://github.com/astral-sh/ruff/assets/8526086/21424bcc-4d60-4f39-8d21-65f798fc632d)

![image](https://github.com/astral-sh/ruff/assets/8526086/6cb41656-22fc-4460-9129-1f5ff59eb99f)

Would be super nice if those two were aligned!

---

_Comment by @MartinBernstorff on 2023-10-19 09:26_

It's a different type of rule than the currently implemented ones, since this numbering is on the same line as the TODO. 

I imagine it'd be a separate branch in the control-flow in todos here. 

https://github.com/astral-sh/ruff/blob/67b043482a5d2f34ee57d7947482ee3429084d54/crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs#L234-L295

With a regex rule like `# TODO: #[0-9]+ `.

It does diverge from flake8-todos, but I'd find it super valuable, and imagine other users would as well.


---

_Renamed from "Align TD003 with VSCode github extension automatic issue creation" to "Align `TD003` with VSCode github extension automatic issue creation" by @MartinBernstorff on 2023-10-20 11:55_

---

_Label `bug` added by @charliermarsh on 2023-10-20 23:25_

---

_Comment by @charliermarsh on 2023-10-20 23:25_

I think we can support this.

---

_Comment by @a-recknagel on 2023-10-23 14:12_

Could this be supported through a setting like
```toml
[tool.ruff.flake8-todos]
link-regex = [
    ".*\n\s*# https?://[\S]+^",  # link in comment on next line
    ".*\(ABC[\d]+\)^",  # what I'd like to write, so that I can have # TODO(arne): improve code (ABC123)
]
```
where the regex is applied on the line where the TODO appears?

---

_Comment by @MartinBernstorff on 2023-11-10 14:18_

@charliermarsh I might have some time over the weekend to implement this. Opinions on a link-regex option in toml vs. an adaptation of TD003? :-)

---

_Comment by @charliermarsh on 2023-11-10 14:36_

@MartinBernstorff - Nice! I think my preference would be to just adapt the logic to accept this for now, and parameterize later if we see more need for it. I prefer avoiding settings if we can just make the defaults better.

---

_Closed by @dylwil3 on 2025-01-15 23:47_

---
