```yaml
number: 3609
title: "Fix infinite loop due to rules `D207` & `W605`"
type: pull_request
state: merged
author: vlindhol
labels:
  - bug
assignees: []
merged: true
base: main
head: issue-3522
created_at: 2023-03-19T15:57:26Z
updated_at: 2023-03-19T18:46:44Z
url: https://github.com/astral-sh/ruff/pull/3609
synced_at: 2026-01-12T04:39:45Z
```

# Fix infinite loop due to rules `D207` & `W605`

---

_Pull request opened by @vlindhol on 2023-03-19 15:57_

Closes #3522

 ### Issue addressed

Given the following example case:

```python
class A:
    """
\ a
    """
```

With the config:

```toml
select = [
  "D207", # bad docstring indentation
  "W605", # invalid escape sequence
]
```

Those two rules clash when autofixing, creating an infinite loop:

1. `W605` schedules an insertion of a `\` at column 0
2. `D207` schedules an insertion of `....` (four spaces) at column 0
3. End result: `\ a` -> `\` + `    ` + `\ a` -> `\    \ a`
4. GOTO 1

 ### Solution

The culprit is the fairly simplistic way that "fix overlaps" are calculated.

1. Firstly, add an explicit ordering of the rules so that they are applied
   in an order that doesn't break the intent (i.e. FIRST indent, THEN fix
   the escape sequence.
2. For future, similar clashes: make `W605` insert the second `\` AFTER the
   first one, rather than BEFORE. This marks one more character in the code
   as "dirty", postponing other fixes in that same column for the next
   autofix iteration.

 ### Test plan

- [ ] ~~Add a new test covering this pathological case. (TODO: how?)~~
- [x] Test manually that the above example now gets fixed correctly.



---

_Marked ready for review by @vlindhol on 2023-03-19 16:01_

---

_Comment by @vlindhol on 2023-03-19 16:01_

I would like to add a test that tests the interplay between these two rules, but not sure where to put it?

---

_@charliermarsh reviewed on 2023-03-19 16:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:119 on 2023-03-19 16:05_

It might _also_ be possible to fix this by giving one rule precedence in `crates/ruff/src/autofix/mod.rs#cmp_fix`, so that we apply the fixes in a specific order even when they're anchored to the same code point. But, what you have here is good too, arguably better.

---

_Comment by @charliermarsh on 2023-03-19 16:06_

> I would like to add a test that tests the interplay between these two rules, but not sure where to put it?

Hmm, yeah, there's not a natural place for this since it cuts across categories. But, the best I can think of is a dedicated test in `crates/ruff/src/rules/ruff/mod.rs`!

---

_Comment by @github-actions[bot] on 2023-03-19 16:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.1±0.55ms     2.1 MB/sec    1.00     19.1±0.42ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.18ms     3.5 MB/sec    1.02      4.9±0.12ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   649.7±17.71µs     4.5 MB/sec    1.03   668.8±26.35µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.20ms     3.1 MB/sec    1.02      8.4±0.18ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.22ms     4.0 MB/sec    1.00     10.2±0.32ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.12ms     7.5 MB/sec    1.03      2.3±0.09ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    259.4±7.65µs    11.4 MB/sec    1.07   278.6±43.87µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.4 MB/sec    1.00      4.7±0.12ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.28ms     2.8 MB/sec    1.03     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.07ms     4.1 MB/sec    1.02      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    524.0±4.28µs     5.6 MB/sec    1.03    538.6±3.44µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.04ms     3.9 MB/sec    1.03      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.05      8.4±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1790.3±14.96µs     9.3 MB/sec    1.04  1867.5±15.72µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.1±2.50µs    14.8 MB/sec    1.07    212.1±8.02µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.03      3.9±0.03ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@vlindhol reviewed on 2023-03-19 16:14_

---

_Review comment by @vlindhol on `crates/ruff/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:119 on 2023-03-19 16:14_

Good callout! `cmp_fix` wasn't there when I forked the repo, I'll make a change there as well, that potentially saves us an autofix iteration. Think I'll still keep my "insert at column 1" change, might help with other rules clashing in a similar way.

I was thinking of how to make the overlap logic more robust, but didn't come up with anything yet. Need to get the hang of the codebase a bit more I think :)

---

_Comment by @vlindhol on 2023-03-19 16:51_

> Hmm, yeah, there's not a natural place for this since it cuts across categories. But, the best I can think of is a dedicated test in `crates/ruff/src/rules/ruff/mod.rs`!

I gave it a shot, but couldn't quite think of an elegant way of testing it without pulling in half the world or reimplementing the rules in the test, which kinda defeats the purpose. My Rust skills are not great, I'm contributing here to get better :)

Anyway, gave up on the test for now! My intuition says that for nice testability it should be easier to compose the rules. E.g. the `indent` rule takes the entire `Checker` object as a param, and checks if the rules are enabled from within the rule body. Itching to refactor that, but then again I'm not confident enough in the codebase to do that, and maybe it kills performance as well?

---

_Review requested from @charliermarsh by @vlindhol on 2023-03-19 17:01_

---

_Comment by @vlindhol on 2023-03-19 17:24_

Not super stoked about the performance hit either... I'm guessing it's the added `match` case in the rule sorting?

---

_@charliermarsh reviewed on 2023-03-19 17:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/mod.rs`:105 on 2023-03-19 17:49_

I think we can omit this for now if we're gonna go with the other fix. I'd like to avoid these special-cases to the degree that we can.

---

_Comment by @charliermarsh on 2023-03-19 17:50_

Sounds good. It's possible that the performance hit is noise, I'm surprised by it.

---

_Review comment by @vlindhol on `crates/ruff/src/autofix/mod.rs`:105 on 2023-03-19 18:19_

Will do!

---

_@vlindhol reviewed on 2023-03-19 18:19_

---

_Review requested from @charliermarsh by @vlindhol on 2023-03-19 18:22_

---

_Label `bug` added by @charliermarsh on 2023-03-19 18:24_

---

_Merged by @charliermarsh on 2023-03-19 18:29_

---

_Closed by @charliermarsh on 2023-03-19 18:29_

---

_Branch deleted on 2023-03-19 18:34_

---

_Comment by @charliermarsh on 2023-03-19 18:35_

Rock on, thank you! If you're interested in more good issues let me know :)

---
