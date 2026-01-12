```yaml
number: 5267
title: Use issue forms instead of an issue template
type: pull_request
state: closed
author: namurphy
labels: []
assignees: []
base: main
head: issue-forms
created_at: 2023-06-21T19:52:51Z
updated_at: 2023-06-29T17:18:24Z
url: https://github.com/astral-sh/ruff/pull/5267
synced_at: 2026-01-12T15:55:18Z
```

# Use issue forms instead of an issue template

---

_@namurphy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`ruff` has been using an [issue template](https://github.com/astral-sh/ruff/blob/main/.github/ISSUE_TEMPLATE.md) which shows up as commented out text when creating a new issue.  This PR proposes the alternative of using [issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms).

This PR removes the issue template and adds issue forms.  So far this is in the draft stage.  To the core maintainers, is this something that you are interested in adding?  If so, I can try adapting the forms to be more specific to `ruff` (such as by incorporating what was in the the issue template).

I'm basing this off of the [`.github/ISSUE_TEMPLATE`](https://github.com/PlasmaPy/PlasmaPy/tree/main/.github/ISSUE_TEMPLATE) of PlasmaPy, which leads to [a page that allows users to select issues](https://github.com/PlasmaPy/PlasmaPy/issues/new/choose) that is essentially a preview of this pull request.

## Test Plan

I'll probably push these changes to the `main` branch of my fork, and add a link here to show a preview of the form.  

---

_Comment by @github-actions[bot] on 2023-06-21 20:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.30ms     5.0 MB/sec    1.18      9.6±0.33ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1687.2±70.04µs     9.9 MB/sec    1.14  1930.0±82.42µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00   169.4±10.24µs    17.4 MB/sec    1.11   188.0±12.21µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.19ms     7.4 MB/sec    1.13      3.9±0.14ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.42ms     2.5 MB/sec    1.06     17.4±0.62ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.1 MB/sec    1.06      4.3±0.34ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   536.5±24.50µs     5.5 MB/sec    1.02   547.2±27.51µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.21ms     3.6 MB/sec    1.07      7.7±0.31ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.20ms     4.9 MB/sec    1.15      9.5±0.39ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1788.2±60.97µs     9.3 MB/sec    1.09  1954.2±80.04µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   214.5±24.91µs    13.8 MB/sec    1.03    221.4±9.67µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.10ms     6.8 MB/sec    1.12      4.2±0.15ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     10.1±0.23ms     4.0 MB/sec    1.00      9.3±0.22ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.0±0.07ms     8.2 MB/sec    1.00  1940.4±90.61µs     8.6 MB/sec
formatter/numpy/globals.py                 1.05   203.3±19.54µs    14.5 MB/sec    1.00   192.9±13.33µs    15.3 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.13ms     6.2 MB/sec    1.00      4.0±0.14ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.4±0.33ms     2.2 MB/sec    1.01     18.5±0.29ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.11ms     3.4 MB/sec    1.00      4.9±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   609.5±22.62µs     4.8 MB/sec    1.00   597.7±17.05µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.19ms     3.1 MB/sec    1.01      8.3±0.19ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.18ms     4.3 MB/sec    1.00      9.5±0.18ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.09ms     8.0 MB/sec    1.00      2.1±0.06ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   240.8±12.77µs    12.3 MB/sec    1.00   239.2±13.37µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.11ms     5.8 MB/sec    1.00      4.3±0.11ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @namurphy on 2023-06-21 21:07_

Looking at #5269, I'm wondering if it would be worth adding a form about performance issues & optimizations.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-26 17:43_

---

_Comment by @charliermarsh on 2023-06-29 00:07_

Thank you for this! And sorry that it took me so long to chime in.

So, we did consider adopting issue forms once before, and came to the conclusion that -- for now at least -- we want to keep the barrier to interaction as low as we can, and that a lightweight template requires less activation energy (for contributors / issue authors) than an issue form. As-is, I'm okay with the amount of information we get from issues, and I'd rather not ask more of issue authors.

We'll definitely revisit that decision in the future, since (1) that's not always the right tradeoff, and (2) I can imagine a world in which an issue form is more helpful for users than our current template (the forms you've setup here are really nice, I clicked through and I genuinely like them a lot). But, for now, I think we'll stick with our template.


---

_Closed by @charliermarsh on 2023-06-29 00:07_

---

_Branch deleted on 2023-06-29 17:18_

---
