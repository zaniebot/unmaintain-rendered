```yaml
number: 2559
title: "Does it make sense to have mypy-like `--output-format concise` as the default?"
type: issue
state: open
author: ksnnsr
labels:
  - question
assignees: []
created_at: 2026-01-19T07:33:30Z
updated_at: 2026-01-19T07:33:30Z
url: https://github.com/astral-sh/ty/issues/2559
synced_at: 2026-01-19T08:24:55Z
```

# Does it make sense to have mypy-like `--output-format concise` as the default?

---

_@ksnnsr_

### Question

Hi team,

I just tried out `ty`. To get the output I expect, coming from mypy and having still a few type issues, I had to run:
```
uvx ty check --output-format concise
```

Else, there was a lot of helpful output, but it looked very overwhelming.

To me, the following makes more sense:
- Make `concise` the default
- Add a message after the output, along the lines
> To see a more verbose / detailed output, add `--output-format full` 

Does this sound sensible to you?

### Version

ty 0.0.12

---

_Label `question` added by @ksnnsr on 2026-01-19 07:33_

---
