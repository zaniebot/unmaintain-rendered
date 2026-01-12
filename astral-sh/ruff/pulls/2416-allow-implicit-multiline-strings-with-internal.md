```yaml
number: 2416
title: Allow implicit multiline strings with internal quotes to use non-preferred quote
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quotes
created_at: 2023-01-31T21:10:48Z
updated_at: 2023-01-31T21:27:16Z
url: https://github.com/astral-sh/ruff/pull/2416
synced_at: 2026-01-12T04:52:00Z
```

# Allow implicit multiline strings with internal quotes to use non-preferred quote

---

_Pull request opened by @charliermarsh on 2023-01-31 21:10_

As an example, if you have `single` as your preferred style, we'll now allow this:

```py
assert s.to_python(123) == (
    "123 info=SerializationInfo(include=None, exclude=None, mode='python', by_alias=True, exclude_unset=False, "
    "exclude_defaults=False, exclude_none=False, round_trip=False)"
)
```

Previously, the second line of the implicit string concatenation would be flagged as invalid, despite the _first_ line requiring double quotes. (Note that we'll accept either single or double quotes for that second line.)

Mechanically, this required that we process sequences of `Tok::String` rather than a single `Tok::String` at a time. Prior to iterating over the strings in the sequence, we check if any of them require the non-preferred quote style; if so, we let _any_ of them use it.

Closes #2400.

---

_Renamed from "Allow implicit multiline strings with internal quotes to use any non-preferred quote style" to "Allow implicit multiline strings with internal quotes to use non-preferred quote style" by @charliermarsh on 2023-01-31 21:12_

---

_Renamed from "Allow implicit multiline strings with internal quotes to use non-preferred quote style" to "Allow implicit multiline strings with internal quotes to use non-preferred quote" by @charliermarsh on 2023-01-31 21:14_

---

_Merged by @charliermarsh on 2023-01-31 21:27_

---

_Closed by @charliermarsh on 2023-01-31 21:27_

---

_Branch deleted on 2023-01-31 21:27_

---
