```yaml
number: 10070
title: "Ability to add a reason along with `--add-noqa`"
type: issue
state: closed
author: ashrub-holvi
labels:
  - cli
  - help wanted
assignees: []
created_at: 2024-02-21T13:21:03Z
updated_at: 2025-12-23T19:48:47Z
url: https://github.com/astral-sh/ruff/issues/10070
synced_at: 2026-01-12T15:54:49Z
```

# Ability to add a reason along with `--add-noqa`

---

_@ashrub-holvi_

Hi!

[Ruff can automatically add `noqa` directives](https://docs.astral.sh/ruff/linter/#inserting-necessary-suppression-comments), but they don't have any info about how they were added:
```
  # noqa: S308
```
and it may be difficult to distinguish intentionally/manually added `noqa` and those automatically added.

I think, main use case is migration to ruff for existing codebase without fixing everything at once, but it should be clear that those `noqa` added automatically and should be handled at some point, something like that:
```
  # noqa: S308 FIXME when you are here
```
Downside of it - adding longer comment will produce more formatting changes, so, perhaps
```
  # noqa: S308 auto
```
or even ruff may have some argument for provide the text of comment.


---

_Comment by @kaddkaka on 2025-01-23 22:01_

Do you really want to `# noqa` them if you intend to fix them?

In our CI we have say 10000 ruff lint violations, and we accept if this number shrinks but fail if it ever increases. 

---

_Label `wish` added by @MichaReiser on 2025-01-24 08:17_

---

_Comment by @ashrub-holvi on 2025-01-24 08:56_

> Do you really want to `# noqa` them if you intend to fix them?

Yes, first of all it makes it much more visible - when you read the code you see that lines you are touching can be improved - ruff is working on file level, so linter can show issues in lines you are not intended to touch. Also, marking all existing issues as noqa really simplifies to see new issues, approach with number of errors also may work in average, but same time it might be you fixed two old, but introduced a new.
Something like https://github.com/astral-sh/ruff/issues/1149 may also be useful.

---

_Comment by @kaddkaka on 2025-01-26 12:06_

Thanks now I understand the situation much better. For me #1149 would be a more valuable feature. 

If you really want to do this it can be done using ruff+other tool. 

Example on how to achieve this today with ruff+git+vim:
```
$ ruff check --add-noqa
$ git jump diff 
:cdo normal! A (added automatically by ruff) 
```

Perhaps this eliminates the feature need in ruff? 

---

_Renamed from "Additional comment to noqa added by ruff" to "Ability to add a reason along with `--add-noqa`" by @MichaReiser on 2025-11-04 13:21_

---

_Comment by @MichaReiser on 2025-11-04 13:22_

This should be fairly easy to implement and shouldn't even need a new CLI flag if we make `--add-noqa` accept a value. 

---

_Label `cli` added by @MichaReiser on 2025-11-04 13:22_

---

_Label `help wanted` added by @MichaReiser on 2025-11-04 13:22_

---

_Label `wish` removed by @MichaReiser on 2025-11-04 13:22_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-11-06 14:36_

---

_Closed by @MichaReiser on 2025-11-11 13:03_

---

_Comment by @engnatha on 2025-12-23 19:45_

This isn't working for me on `0.14.10`. `--add-noqa` works fine, but providing a reason silently exits without fixing any errors.

Edit: Ah I was missing the `requires_equals`

---
