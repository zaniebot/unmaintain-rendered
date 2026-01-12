```yaml
number: 12249
title: "option to set `output-format` grouped by rule"
type: issue
state: closed
author: DaniBodor
labels: []
assignees: []
created_at: 2024-07-08T17:20:16Z
updated_at: 2024-07-09T06:30:59Z
url: https://github.com/astral-sh/ruff/issues/12249
synced_at: 2026-01-12T15:54:51Z
```

# option to set `output-format` grouped by rule

---

_@DaniBodor_

I think it would be useful if `ruff check` could group the output by rule violated, perhaps even with the number of instances explicitly given.
This can be useful for cases where the violation is not easily fixable to decide whether to supress it case-by-case or turn it off altogether.

Currently, there is a "grouped" option, but this groups violations per file, not per rule.

---

_Renamed from "option to make `output-format` grouped by rule" to "option to set `output-format` grouped by rule" by @DaniBodor on 2024-07-08 17:20_

---

_Comment by @MichaReiser on 2024-07-08 17:32_

does the  --statistics option give you the output you're looking for?

---

_Comment by @DaniBodor on 2024-07-08 18:31_

It almost does. I had missed that option :). 
It still would be nice to allow to show each instance grouped by the rule violation. But it's definitely good enough for the issue I mentioned above.

It is a bit confusing that it is separate from `--output-format`, as it appears to be just another output format :)

---

_Comment by @dhruvmanila on 2024-07-09 03:49_

> It is a bit confusing that it is separate from `--output-format`, as it appears to be just another output format :)

I think the `--statistics` flag was adopted from `flake8` which is why it is separate. There's also [this comment](https://github.com/astral-sh/ruff/issues/4140#issuecomment-1527807554) for context.

---

_Closed by @charliermarsh on 2024-07-09 03:49_

---

_Comment by @MichaReiser on 2024-07-09 06:30_

The other reason why it's different is because `ruff check --statistics --output-format=json` emits json. 

---
