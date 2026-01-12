```yaml
number: 5614
title: "`[DLINT]` Add Dlint plugin and first rule DUO138"
type: pull_request
state: closed
author: qdegraaf
labels: []
assignees: []
draft: true
base: main
head: feature/dlint
created_at: 2023-07-08T14:54:41Z
updated_at: 2023-07-20T09:43:58Z
url: https://github.com/astral-sh/ruff/pull/5614
synced_at: 2026-01-12T15:55:19Z
```

# `[DLINT]` Add Dlint plugin and first rule DUO138

---

_@qdegraaf_

## Summary

Adds Dlint plugin port and a first rule `DUO138` `catastrophic_backtracking_regular_expression` which checks for regex statements which can potentially lead to catastrophic backtracking and hence are vulnerable to ReDoS attacks. 

Upstream implementation uses `sre.parse`. We do not have this so we use the `regex_syntax` crate. We cannot directly copy the logic from dlint due to this so we have to find our own implementation

TODOs:

- [x] Implement unbounded repetition within repetition logic for catastrophic backtracking in `detect_redos()`
- [ ] Implement alternation with overlap logic for catastrophic backtracking in `detect_redos()`

## Test Plan

Fixture added with many examples of catastrophic backtracking

## Issue links

Refers: https://github.com/astral-sh/ruff/issues/4479 


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/catastrophic_re_use.rs`:45 on 2023-07-10 07:57_

Nit: I'm not sure if we need the word `dangerous` here, especially because it isn't entirely clear to what dangerous refers (I assume it is dangerous because of the performance).

How about:
`Regular expression with potential catastrophic backtracking`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/catastrophic_re_use.rs`:40 on 2023-07-10 07:58_

Maybe `CatastrophicBacktrackingRegularExpression`

---

_@MichaReiser reviewed on 2023-07-10 07:59_

---

_@qdegraaf reviewed on 2023-07-10 09:06_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/catastrophic_re_use.rs`:45 on 2023-07-10 09:06_

The danger lies in the potential for ReDoS: https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS 

I'll change the message anyway, or specify "vulnerable to ReDoS" to be more specific 

---

_Comment by @qdegraaf on 2023-07-10 11:20_

Seeing as you already had a look I will pose the following question to you @MichaReiser .

If I understand the rule correctly to cover the same heuristic ground as the upstream implementation we would have to check two scenarios: 
1. Unbounded repetition within repetition
2. Overlapping alternation within repetition

The current code should cover scenario 1. I have been trying to get scenario 2 to work but I think we are limited by the `regex_syntax` parser here. As for example the following two test cases from dlint:

```python
re.search("([a-c]|[c-e])+")  # DUO138

re.search("([a-c]|[d-e])+")  # OK
```

Both share the same HIR representation:

```
Repetition(
    Repetition {
        min: 1,
        max: None,
        greedy: true,
        sub: Capture(
            Capture {
                index: 1,
                name: None,
                sub: Class(
                    {
                        'a'..='e',
                    },
                ),
            },
        ),
    },
)
Repetition(
    Repetition {
        min: 1,
        max: None,
        greedy: true,
        sub: Capture(
            Capture {
                index: 1,
                name: None,
                sub: Class(
                    {
                        'a'..='e',
                    },
                ),
            },
        ),
    },
)
```

So it would not be possible for us to determine overlap based on the HIR. The alternative is to write a custom parser or use the underlying AST of regex_syntax to detect this, which is outside of my current knowledge and skill level at the moment I think. 

There's a couple things I could do with the PR I was wondering if you had any preference:

1. We implement the rule with just a check for scenario 1 for now (cleaning up and thorough reviewing etc. in the process) and leave scenario 2 as a TODO for the future
    - `+` Potentially justifiable as vulnerable regex can come in many forms and catching all of them is outside the scope of upstream plugin too. Covering some cases is arguably better than covering no cases if we are clear about the coverage of the rule and minimise the risk of false positives.
    - `-` might be confusing and sloppy
    - `-` Judging by the benchmark, current implementation is quite slow, which might be due to the HIR parser. Meaning even if we are ok with the coverage of the rule, the performance might be undesirable.
2. Refactor the PR to use a different or custom regex parser and cover both scenarios
    - `+` Best implementation of the rule, parity with upstream.
    - `-` Outside of my capabilities in the short term, will either need a lot of help/input and/or a lot of time
    - `-` Performance might still be an issue
3. Close the PR 
    - If current implementation is subpar and scenario 2 is not feasible right now, probably best to close this and come back to it later

Let me know which option is preferred from a maintainer point-of-view.


---

_Comment by @bluetech on 2023-07-10 15:08_

I don't think the regex syntax implemented by `regex-syntax ` is fully compatible with Python's regex syntax, so it might parse invalid Python regex, or inversely fail to parse valid Python regex. That might be a problem?

Relatedly, the code currently does `parser.parse(regex_string).unwrap()`. I think that `unwrap` shouldn't be used here, because it's possible for invalid regex to appear in Python source code and Ruff shouldn't crash due to this (I recommend adding a few tests with invalid regex syntax). As for what to do in this case, I think the only thing that can be done is to just skip. Possibly can be a separate "invalid regex" lint rule.

---

_Comment by @qdegraaf on 2023-07-10 15:38_

> I don't think the regex syntax implemented by `regex-syntax ` is fully compatible with Python's regex syntax, so it might parse invalid Python regex, or inversely fail to parse valid Python regex. That might be a problem?
> 
> Relatedly, the code currently does `parser.parse(regex_string).unwrap()`. I think that `unwrap` shouldn't be used here, because it's possible for invalid regex to appear in Python source code and Ruff shouldn't crash due to this (I recommend adding a few tests with invalid regex syntax). As for what to do in this case, I think the only thing that can be done is to just skip. Possibly can be a separate "invalid regex" lint rule.

Yeah that's another point of concern indeed. I have yet to find an overview of the differences between Rust and Python regex and thereby estimate the size of potential incompatibility. As for the `unwrap()` I'll handle that properly if this PR is worth continuing (skipping probably being the best option, though this could add false negatives to an already flaky implementation). I am leaning more towards closing this and looking at how we want to handle regex related rules holistically as I think on it though.

---

_Comment by @qdegraaf on 2023-07-20 09:43_

Closing this for now. I think this is best picked up when Ruff has a more general plan for how to handle Python regex statements.

---

_Closed by @qdegraaf on 2023-07-20 09:43_

---
