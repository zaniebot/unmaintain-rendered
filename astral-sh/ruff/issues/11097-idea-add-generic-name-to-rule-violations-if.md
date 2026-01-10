```yaml
number: 11097
title: "idea: add generic name to rule violations if `message` contains formatted strings when output in `--statistics`"
type: issue
state: closed
author: diceroll123
labels: []
assignees: []
created_at: 2024-04-23T02:23:02Z
updated_at: 2024-06-26T09:02:08Z
url: https://github.com/astral-sh/ruff/issues/11097
synced_at: 2026-01-10T11:09:53Z
```

# idea: add generic name to rule violations if `message` contains formatted strings when output in `--statistics`

---

_Issue opened by @diceroll123 on 2024-04-23 02:23_

This is probably a bit of a stretch to ask, but running `ruff check --select=ANN201 --statistics` for example will show something like

```
25445	ANN201	Missing return type annotation for public function `THING`
```

Which obviously does not _actually_ apply to all 25445 instances of the ANN201 violations.

So I propose some generic analogue to `Violation::message` but for the basic violation. In this case, `Missing return type annotation for public function`.

Of course, this suggestion applies to all violations with formatted strings.

---

_Comment by @MichaReiser on 2024-04-23 07:06_

This sounds reasonable, although adding one more field to all violations is a rather involved change (and having it on `Message` also has a performance cost). I wonder if we should just show the rule name instead with a link or change the titles of our rules to never include dynamic parts?

---

_Comment by @charliermarsh on 2024-06-01 23:39_

Let's change this to use the rule name rather than the violation.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-02 18:18_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-06-02 18:29_

---

_Closed by @MichaReiser on 2024-06-26 09:02_

---
