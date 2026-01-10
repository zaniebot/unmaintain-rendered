```yaml
number: 12282
title: "Use `space` separator before parenthesiszed expressions in comprehensions with leading comments."
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: if-leading-comment
created_at: 2024-07-10T20:47:56Z
updated_at: 2024-07-11T20:38:13Z
url: https://github.com/astral-sh/ruff/pull/12282
synced_at: 2026-01-10T21:47:02Z
```

# Use `space` separator before parenthesiszed expressions in comprehensions with leading comments.

---

_Pull request opened by @MichaReiser on 2024-07-10 20:47_

## Summary

Keeps the parentheses on the same line as the preceding keyword if the following expression has leading comments. 


Fixes https://github.com/astral-sh/ruff/issues/12280

```python
y = [
    a
    for (
        # comment
        a
    ) in (
        # comment
        x
    )
    if (
        # asdasd
        "askldaklsdnmklasmdlkasmdlkasmdlkasmdasd"
        != "as,mdnaskldmlkasdmlaksdmlkasdlkasdm"
        and "zxcm,.nzxclm,zxnckmnzxckmnzxczxc" != "zxcasdasdlmnasdlknaslkdnmlaskdm"
    )
    if (
        # comment
        x
    )
]


```


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I added three new integration tests. 

The changes are gated behind preview because this isn't a fix addressing a stability issue or because the formatted produced invalid sytnax.

---

_Comment by @github-actions[bot] on 2024-07-10 20:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Label `formatter` added by @MichaReiser on 2024-07-10 21:08_

---

_Label `preview` added by @MichaReiser on 2024-07-11 06:50_

---

_Renamed from "Use `space` separator between `if` and `(` in comprehensions" to "Use `space` separator before parenthesiszed expressions in comprehensions with leading comments." by @MichaReiser on 2024-07-11 06:52_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-07-11 06:55_

---

_Marked ready for review by @MichaReiser on 2024-07-11 06:55_

---

_@charliermarsh approved on 2024-07-11 14:16_

---

_Merged by @MichaReiser on 2024-07-11 20:38_

---

_Closed by @MichaReiser on 2024-07-11 20:38_

---

_Branch deleted on 2024-07-11 20:38_

---
