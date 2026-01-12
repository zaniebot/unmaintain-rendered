```yaml
number: 7586
title: Use the new f-string tokens in string formatting
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/formatter-fstring
created_at: 2023-09-22T03:30:26Z
updated_at: 2023-09-28T03:58:57Z
url: https://github.com/astral-sh/ruff/pull/7586
synced_at: 2026-01-12T15:55:24Z
```

# Use the new f-string tokens in string formatting

---

_@dhruvmanila_

## Summary

This PR updates the string formatter to account for the new f-string tokens.

The formatter uses the full lexer to handle comments around implicitly concatenated strings. The reason it uses the lexer is because the AST merges them into a single node so the boundaries aren't preserved.

For f-strings, it creates some complexity now that it isn't represented as a `String` token. A single f-string will atleast emit 3 tokens (`FStringStart`, `FStringMiddle`, `FStringEnd`) and if it contains expressions, then it'll emit the respective tokens for them. In our case, we're currently only interested in the outermost f-string range for which I've introduced a new `FStringRangeBuilder` which keeps builds the outermost f-string range by considering the start and end tokens and the nesting level.

Note that this doesn't support in any way nested f-strings which is out of scope for this PR. This means that if there are nested f-strings, especially the ones using the same quote, the formatter will escape the inner quotes:

```python
f"hello world {
    x
        +
            f\"nested {y}\"
}"
```

## Test plan

```
cargo test --package ruff_python_formatter
```

---

_Label `formatter` added by @dhruvmanila on 2023-09-22 03:30_

---

_Label `python312` added by @dhruvmanila on 2023-09-22 03:30_

---

_Comment by @dhruvmanila on 2023-09-22 03:33_

Hmm, I've rebased the stack on the latest main so not sure why does `mdformat` wants to format those markdown files.

---

_Comment by @dhruvmanila on 2023-09-22 03:36_

(Looking into the formatter ecosystem failures)

---

_Converted to draft by @dhruvmanila on 2023-09-22 03:36_

---

_Comment by @charliermarsh on 2023-09-22 03:37_

Hm, I wonder if this is related to https://github.com/astral-sh/ruff/pull/7538?

---

_Renamed from "Add support for the new f-string tokens per PEP 701 (#6659)" to "Use the new f-string tokens in string formatting" by @dhruvmanila on 2023-09-22 03:44_

---

_Comment by @dhruvmanila on 2023-09-22 03:47_

> Hm, I wonder if this is related to #7538?

Are you referring to the ecosystem checks or pre-commit (`mdformat`)?

---

_Comment by @charliermarsh on 2023-09-22 03:48_

The `mdformat` change. Those files should be in the `.gitignore`, right?

---

_Comment by @dhruvmanila on 2023-09-22 03:55_

> The `mdformat` change. Those files should be in the `.gitignore`, right?

Oh, my bad. A few files got added when I was exploring `mkdocs-material` 😅 

---

_Comment by @dhruvmanila on 2023-09-22 05:14_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7586?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-09-22 05:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/formatter-fstring)

### Merging #7586 will **not alter performance**

<sub>Comparing <code>dhruv/formatter-fstring</code> (c9cf545) with <code>dhruv/pep-701</code> (5e35a55)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Marked ready for review by @dhruvmanila on 2023-09-22 05:34_

---

_@MichaReiser approved on 2023-09-22 06:06_

---

_Merged by @dhruvmanila on 2023-09-22 09:24_

---

_Closed by @dhruvmanila on 2023-09-22 09:24_

---

_Branch deleted on 2023-09-22 09:25_

---
