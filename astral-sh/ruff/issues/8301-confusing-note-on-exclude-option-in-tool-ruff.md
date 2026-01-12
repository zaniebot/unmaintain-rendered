```yaml
number: 8301
title: "Confusing note on `exclude` option in `tool.ruff.format`"
type: issue
state: closed
author: Giuzzilla
labels: []
assignees: []
created_at: 2023-10-28T09:06:53Z
updated_at: 2023-10-28T13:08:26Z
url: https://github.com/astral-sh/ruff/issues/8301
synced_at: 2026-01-12T15:54:48Z
```

# Confusing note on `exclude` option in `tool.ruff.format`

---

_@Giuzzilla_

I was checking the various options for `tool.ruff.format` and in particular I wanted to add additional exclusions to the formatter (in addition to the exclusions in `tool.ruff`.

The documentation of this option can be found at: https://docs.astral.sh/ruff/settings/#format-exclude

The documentation correctly says 

> A list of file patterns to exclude from formatting in addition to the files excluded globally.

I had two doubts about this:

1. The use of the keyword `exclude` instead of `extend-exclude` might make you think that this list is not "additive" / on top of the global one. (changing this would be a kind of breaking change though and the current behaviour is documented)
2. The doc within the `tool.ruff.format` section of `exclude` says:  

> Note that you'll typically want to use [extend-exclude](https://docs.astral.sh/ruff/settings/#extend-exclude) to modify the excluded paths.

However, no such option is available for the formatter so one might confuse the `tool.ruff.format.exclude` behavior with the behaviour of the global `tool.ruff.exclude` (which is not additive). This text seems to be a copy-paste from the global [`exclude` option documentation](https://docs.astral.sh/ruff/settings/#exclude)



---

_Closed by @Giuzzilla on 2023-10-28 09:22_

---

_Comment by @Giuzzilla on 2023-10-28 09:22_

Maybe it's better to discuss point (1) on the relevant discussion of the formatter: https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7410341

---

_Renamed from "Confusing note on `exclude` option in `tool.ruff.format`?" to "Confusing note on `exclude` option in `tool.ruff.format`" by @Giuzzilla on 2023-10-28 09:35_

---

_Reopened by @Giuzzilla on 2023-10-28 09:36_

---

_Closed by @Giuzzilla on 2023-10-28 13:08_

---
