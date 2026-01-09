---
number: 9536
title: "bug: explicit-preview-rules enables rule from ALL"
type: issue
state: closed
author: nstarman
labels:
  - needs-decision
assignees: []
created_at: 2024-01-15T18:54:45Z
updated_at: 2024-01-15T22:50:29Z
url: https://github.com/astral-sh/ruff/issues/9536
synced_at: 2026-01-07T13:12:15-06:00
---

# bug: explicit-preview-rules enables rule from ALL

---

_Issue opened by @nstarman on 2024-01-15 18:54_

In https://github.com/astropy/astropy/pull/15889 I tried turning on `preview` mode with `explicit-preview-rules`. From https://docs.astral.sh/ruff/preview/#selecting-single-preview-rules I would expect that running `ruff lint` (really `pre-commit run ruff -a`) shouldn't do anything since no rules are explicitly selected (we use `select = ["ALL"]`). However ruff finds and fixes `SIM910`.
Is this a bug?

---

_Comment by @charliermarsh on 2024-01-15 18:56_

It does sound like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-15 19:59_

---

_Label `bug` added by @charliermarsh on 2024-01-15 19:59_

---

_Comment by @charliermarsh on 2024-01-15 20:06_

Ahh I see... That rule _isn't_ in preview, but we changed the behavior of the rule such that in preview, it flags cases like:

```python
ages = {"Tom": 23, "Maria": 23, "Dog": 11}
age = ages.get("Cat", None)
```

Whereas in stable, it will only flag `.get` calls on literals, like:

```python
{"Tom": 23, "Maria": 23, "Dog": 11}.get("Cat", None)
```

I think this behavior is correct (you're enabling the rule, and `--preview` is modifying its behavior) albeit a bit unclear.


---

_Comment by @charliermarsh on 2024-01-15 20:06_

\cc @zanieb for visibility, if you're interested

---

_Label `bug` removed by @charliermarsh on 2024-01-15 20:06_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-15 20:06_

---

_Comment by @zanieb on 2024-01-15 21:02_

We should probably document preview behaviors better per rule, but I think this is fair expected behavior.

---

_Comment by @charliermarsh on 2024-01-15 21:04_

Yeah. It actually _is_ mentioned in the [rule documentation](https://docs.astral.sh/ruff/rules/dict-get-with-none-default/), but perhaps it should be in a `## Preview` section.

---

_Closed by @charliermarsh on 2024-01-15 21:04_

---

_Comment by @nstarman on 2024-01-15 21:30_

Would it be better if when `explicit-preview-rules` is on that even *existing* rules like SIM910 would need to be explicitly selected to turn on their preview mode?

IMO *preview* option is for things still in development (otherwise it would just be released?), so it would be nice to have granular control over the previews even for *existing* rules.

SIM910 preview is on
```
[tool.ruff.lint]
preview = true
select = [SIM910]
```

SIM910 preview is on
```
[tool.ruff.lint]
preview = true
explicit-preview-rules = true
select = [SIM910]
```

SIM910 is on, it's preview is OFF
```
[tool.ruff.lint]
preview = true
explicit-preview-rules = true
select = [SIM]
```

SIM910 is on, it's preview is OFF
```
[tool.ruff.lint]
preview = true
explicit-preview-rules = true
select = [ALL]
```


---

_Comment by @zanieb on 2024-01-15 22:50_

> Would it be better if when explicit-preview-rules is on that even existing rules like SIM910 would need to be explicitly selected to turn on their preview mode?

_Maybe_ but it adds a lot of complexity and it sounds just as confusing.

> preview option is for things still in development (otherwise it would just be released?), so it would be nice to have granular control over the previews even for existing rules.

Preview is for things that we want to get feedback on before we stabilize to reduce the number of changes that users in our stable track need to deal with. It's explicitly _not_ intended for things still in development and it is explicitly designed to be a relatively _global_ toggle to minimize the overhead of maintenance for us.

---
