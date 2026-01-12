```yaml
number: 1064
title: Allow JSON as the output format
type: issue
state: open
author: Khoyo
labels:
  - cli
assignees: []
created_at: 2025-08-20T14:39:43Z
updated_at: 2025-12-19T20:14:47Z
url: https://github.com/astral-sh/ty/issues/1064
synced_at: 2026-01-12T15:54:24Z
```

# Allow JSON as the output format

---

_@Khoyo_

The current allowed output formats of `ty check` are full and concise, which aren't great if you want to parse it (eg. in CI). Allowing the `json` and `json-lines` output formats that ruff already supports would make it easier.

---

_Label `cli` added by @AlexWaygood on 2025-08-20 14:42_

---

_Label `wish` added by @MichaReiser on 2025-08-20 15:10_

---

_Comment by @MichaReiser on 2025-08-20 15:12_

This is something that's on our mind.

It's unlikely that we'll port the `json` output 1:1. Instead, we might want to have multiple json outputs: a concise version similar to Ruff's output and a full output that captures more diagnostic information (maybe even in its rendered form). 

So there's some design work that we need to do before exposing it to ty. This, otherwise, is very straightforward.

---

_Comment by @MichaReiser on 2025-11-14 08:59_

Some more details in https://github.com/astral-sh/ty/issues/717

---

_Label `wish` removed by @MichaReiser on 2025-11-14 08:59_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:59_

---

_Comment by @spaceone on 2025-12-19 20:14_

For writing a linter `--output-format json-lines` would be nice. With `gitlab` output format I have all available options but must iterate a JSON list.

---
