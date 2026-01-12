```yaml
number: 6775
title: "Fix `uncessary-coding-comment` fix when there's leading content"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: fix/unecessary-coding-comment-fix-space
created_at: 2023-08-22T15:51:46Z
updated_at: 2023-08-23T17:01:59Z
url: https://github.com/astral-sh/ruff/pull/6775
synced_at: 2026-01-12T02:45:38Z
```

# Fix `uncessary-coding-comment` fix when there's leading content

---

_Pull request opened by @zanieb on 2023-08-22 15:51_

Closes https://github.com/astral-sh/ruff/issues/6756

Including whitespace, code, and continuations.

---

_@charliermarsh reviewed on 2023-08-22 15:53_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 15:53_

What happens if the coding comment is preceded by content, like:

```python
print(x) # coding=utf8
print("Hello world")
```

---

_Comment by @github-actions[bot] on 2023-08-22 16:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      4.0±0.18ms    10.1 MB/sec    1.00      3.9±0.05ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   821.9±31.04µs    20.3 MB/sec    1.00   824.6±14.86µs    20.2 MB/sec
formatter/numpy/globals.py                 1.05     85.8±1.85µs    34.4 MB/sec    1.00     81.7±2.80µs    36.1 MB/sec
formatter/pydantic/types.py                1.00  1591.7±49.23µs    16.0 MB/sec    1.00  1590.1±43.83µs    16.0 MB/sec
linter/all-rules/large/dataset.py          1.05     12.1±0.17ms     3.4 MB/sec    1.00     11.5±0.34ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.2±0.03ms     5.2 MB/sec    1.00      3.1±0.09ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.05    457.8±4.09µs     6.4 MB/sec    1.00   438.1±14.17µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.4±0.07ms     4.0 MB/sec    1.00      6.1±0.21ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.12ms     6.5 MB/sec    1.02      6.4±0.13ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1356.7±32.98µs    12.3 MB/sec    1.01  1372.3±33.24µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.9±4.07µs    18.4 MB/sec    1.04    166.2±2.02µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.06ms     9.2 MB/sec    1.04      2.9±0.03ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14      3.0±0.02ms    13.5 MB/sec    1.00      2.7±0.06ms    15.3 MB/sec
formatter/numpy/ctypeslib.py               1.11    577.9±5.41µs    28.8 MB/sec    1.00   522.6±13.40µs    31.9 MB/sec
formatter/numpy/globals.py                 1.07     59.0±0.80µs    50.0 MB/sec    1.00     55.1±2.00µs    53.6 MB/sec
formatter/pydantic/types.py                1.11  1210.6±15.96µs    21.1 MB/sec    1.00  1094.8±26.23µs    23.3 MB/sec
linter/all-rules/large/dataset.py          1.00      9.1±0.08ms     4.5 MB/sec    1.02      9.3±0.09ms     4.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.6±0.02ms     6.5 MB/sec    1.03      2.6±0.03ms     6.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    265.7±4.20µs    11.1 MB/sec    1.03    273.0±5.54µs    10.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.8±0.05ms     5.3 MB/sec    1.03      4.9±0.08ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.1±0.05ms     8.0 MB/sec    1.03      5.2±0.04ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1063.4±9.53µs    15.7 MB/sec    1.02  1087.7±21.93µs    15.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    107.8±1.93µs    27.4 MB/sec    1.02    109.6±1.98µs    26.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.02ms    11.2 MB/sec    1.03      2.3±0.03ms    10.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-08-22 17:38_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 17:38_

That shouldn't match our regex, but I can add test coverage

---

_@zanieb reviewed on 2023-08-22 17:40_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 17:40_

Except it does get removed.. humph will investigate

---

_@charliermarsh reviewed on 2023-08-22 17:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 17:41_

The regular expression is only run over the comment (the range is that of the comment).

---

_@zanieb reviewed on 2023-08-22 17:42_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 17:42_

Yeah I just realized that — this was less clear when the variable was named `line`, pretty obvious now

---

_@zanieb reviewed on 2023-08-22 17:49_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:71 on 2023-08-22 17:49_

[PEP 263](https://peps.python.org/pep-0263/) doesn't let you declare these as comments with other content on the line so I've updated the regex to apply to the full line in https://github.com/astral-sh/ruff/pull/6775/commits/f0db7a43008e3c50c7b20f5d483cd24989c6c43d

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:97 on 2023-08-22 18:16_

What if we're given this:

```python
x = 1 \
  # coding=utf8
x = 2
```

Fixing to:
```ppython
```python
x = 1 \
x = 2
```

Would lead to a syntax error. (I'm not certain that's what's happening, but we have to consider and test these kinds of cases too. We have some logic around that elsewhere via `preceded_by_continuations`.)

---

_@charliermarsh reviewed on 2023-08-22 18:16_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:97 on 2023-08-22 18:21_

Yeesh. Alright will fix this.

---

_@zanieb reviewed on 2023-08-22 18:21_

---

_@charliermarsh reviewed on 2023-08-22 18:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:97 on 2023-08-22 18:23_

If we don't, it will get fuzzed eventually :joy:

---

_Label `bug` added by @zanieb on 2023-08-22 19:07_

---

_Label `fuzzer` added by @zanieb on 2023-08-22 19:07_

---

_Renamed from "Fix `uncessary-coding-comment` fix when there is same-line leading whitespace" to "Fix `uncessary-coding-comment` fix when there leading content" by @zanieb on 2023-08-22 19:23_

---

_Renamed from "Fix `uncessary-coding-comment` fix when there leading content" to "Fix `uncessary-coding-comment` fix when there's leading content" by @zanieb on 2023-08-22 19:23_

---

_@charliermarsh approved on 2023-08-22 20:53_

I think another valid solution here would be: if the comment isn't at the beginning of the line, delete _just_ the comment (not the `locator.full_line_end` of the comment). Possibly a bit simpler since you wouldn't need to expand the line for the regex, wouldn't need to special-case continuations, etc. But defer to you as the author.

---

_Comment by @zanieb on 2023-08-22 21:26_

Hm that seems kind of nice but in the end we probably don't want to retain that extra content. Are there other rules that would cover the removing the remaining whitespace and newlines? I'm tempted to just move on until we see another edge-case in the future.

> wouldn't need to expand the line for the regex

I still think we should be doing this anyway as the regex is designed to match a full line.

---

_Comment by @charliermarsh on 2023-08-22 21:57_

I worry that the regex will open the door to other problems though. How does the regex fare with an example like this?

```python
"""
# coding=utf8""" # empty comment
```

(I hadn't considered this before, this is a new observation.)

---

_Comment by @zanieb on 2023-08-22 22:29_

:sigh: yeah that is broken too. We could address that in the current implementation by making the end of the regex more strict or by applying the regex twice (once to the comment itself and once to the line), but you've convinced me to reconsider the approach.

I'm not sure about just narrowing the match though. Per the PEP

> There must not be any Python statement on the line that contains the encoding declaration. 

we should probably just ignore cases entirely if the line doesn't match the regex. It feels incorrect to delete a comment when it's not a valid coding comment in the first place.



---

_Comment by @charliermarsh on 2023-08-22 22:33_

What about something like:

- If there is any non-whitespace content between the start of the line and the comment, don't flag it.
- If there is whitespace at the start of the line, only remove the comment range (not the entire line) _or_ expand to include the continuation in the deletion.
- Otherwise, remove the entire line.

---

_Comment by @zanieb on 2023-08-23 15:32_

I think applying the regex twice is the simplest way to ensure we're doing the right thing here without rewriting this whole thing to use a tokenizer? I could do that here but I'm leaning towards opening that as a follow-up issue for a contributor and merging this fix.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:66 on 2023-08-23 15:52_

I think I would still recommend doing something like:

```rust
// Check if there's any content preceding the comment.
let line_start = locator.line_start(*range.start());
if !locator
    .slice(TextRange::new(line_start, range.start()))
    .trim()
    .is_empty()
{
    continue;
}
```

Personally, I find that much easier to understand than running the regex twice.

---

_@charliermarsh approved on 2023-08-23 15:53_

---

_Comment by @zanieb on 2023-08-23 16:02_

Thanks for the example — I thought you were suggesting using the tokenizer but that's nice and simple.

I think the only thing that differs now is

> If there is whitespace at the start of the line, only remove the comment range (not the entire line) or expand to include the continuation in the deletion.

Comments following continuations are not valid coding definitions as far as I can tell, we probably shouldn't flag them at all. I think it's kind of academic at this point though.


---

_@charliermarsh reviewed on 2023-08-23 16:42_

This looks great!

---

_Merged by @zanieb on 2023-08-23 17:00_

---

_Closed by @zanieb on 2023-08-23 17:00_

---

_Branch deleted on 2023-08-23 17:00_

---
