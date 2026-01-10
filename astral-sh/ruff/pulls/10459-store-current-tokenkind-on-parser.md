```yaml
number: 10459
title: "Store current `TokenKind` on `Parser`"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - parser
assignees: []
draft: true
base: dhruv/parser
head: dhruv/current-kind
created_at: 2024-03-18T14:51:00Z
updated_at: 2024-04-17T15:08:57Z
url: https://github.com/astral-sh/ruff/pull/10459
synced_at: 2026-01-10T22:37:01Z
```

# Store current `TokenKind` on `Parser`

---

_Pull request opened by @dhruvmanila on 2024-03-18 14:51_

## Summary

This PR updates the `Parser` struct to store the current `TokenKind` along with the `Spanned` (token and it's range). The token kind is being used in all of the parsing functions and it's beneficial to have it around instead of computing it every time.


---

_Label `parser` added by @dhruvmanila on 2024-03-18 14:51_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-18 14:51_

---

_Comment by @github-actions[bot] on 2024-03-18 15:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-03-18 15:52_

> and it's beneficial to have it around instead of computing it every time.

Can you expand on why it's beneficial? Does it improve performance? 

---

_Comment by @dhruvmanila on 2024-03-19 02:18_

> Does it improve performance?

Not really. Benchmarking doesn't really show much difference:

```console
$ critcmp main-parser current-kind                                                                
group                         current-kind                           main-parser
-----                         ------------                           -----------
parser/large/dataset.py       1.01    634.8±2.94µs    64.1 MB/sec    1.00    627.6±2.09µs    64.8 MB/sec
parser/numpy/ctypeslib.py     1.00    108.2±0.42µs   153.9 MB/sec    1.02    109.9±0.49µs   151.5 MB/sec
parser/numpy/globals.py       1.01      8.7±0.04µs   340.1 MB/sec    1.00      8.5±0.04µs   345.1 MB/sec
parser/pydantic/types.py      1.01    254.0±1.54µs   100.4 MB/sec    1.00    252.7±1.32µs   100.9 MB/sec
parser/unicode/pypinyin.py    1.01     33.4±0.15µs   125.9 MB/sec    1.00     32.9±0.15µs   127.7 MB/sec
```

Nor does the hyperfine benchmarks.

Although the amount of time we do the conversion is reduced by around 80% which is due to the fact that we do this conversion only once when the parser moves on to the next token (sometimes when we peek) instead of every time we want the token kind.

---

_Comment by @MichaReiser on 2024-03-19 08:44_

> Not really. Benchmarking doesn't really show much difference:

I assume that is because we now trade computation with additional read/write instructions. I would hold off with this PR unless it improves performance because it introduces a potential source of inconsistency bugs (failing to update `token_kind` but moving to the next token). You could also try and see if https://github.com/astral-sh/ruff/pull/9537 helps with perf. 

---

_Comment by @dhruvmanila on 2024-03-19 08:48_

I see, thanks for the linked PR. I'll check it out soon. Putting this PR on draft for the time being.

---

_Converted to draft by @dhruvmanila on 2024-03-19 08:48_

---

_Comment by @dhruvmanila on 2024-04-07 11:24_

I'll close this for now, can pick it up later.

---

_Closed by @dhruvmanila on 2024-04-07 11:24_

---
