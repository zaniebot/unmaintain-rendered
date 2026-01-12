```yaml
number: 3662
title: "[`pylint`]: Implement `while-used` (`W0149`)"
type: pull_request
state: closed
author: latonis
labels: []
assignees: []
base: main
head: pylint-while-used
created_at: 2023-03-22T02:52:39Z
updated_at: 2023-03-22T03:53:11Z
url: https://github.com/astral-sh/ruff/pull/3662
synced_at: 2026-01-12T15:55:13Z
```

# [`pylint`]: Implement `while-used` (`W0149`)

---

_@latonis_

Rule reference: https://pylint.pycqa.org/en/latest/user_guide/messages/warning/while-used.html

Implemented for #970 

Implemented exactly as Pylint does, which is to flag every single `while` loop if this is enabled :smile_cat: 

---

_Renamed from "`pylint`: Implement `while-used` (`W0149`)" to "[`pylint`]: Implement `while-used` (`W0149`)" by @latonis on 2023-03-22 02:54_

---

_Comment by @charliermarsh on 2023-03-22 02:55_

One challenge with this rule is that in Pylint, you typically have to enable it explicitly (I think?) by enabling the `pylint.extensions.while_used` checker. We don't have a great mechanic for that, so all users that have Pylint enabled would be opted into this 🤔 

---

_Comment by @latonis on 2023-03-22 03:02_

> One challenge with this rule is that in Pylint, you typically have to enable it explicitly (I think?) by enabling the `pylint.extensions.while_used` checker. We don't have a great mechanic for that, so all users that have Pylint enabled would be opted into this thinking

Ah fair point @charliermarsh, could we add an config option for Pylint like `while_used` to be set to true that way it only runs when the user has it enabled, similar to how the user would be enabling it with `extensions.while_used` in standard Pylint?

---

_Comment by @latonis on 2023-03-22 03:20_

@charliermarsh, I've added a config option via Pylint's `Options` struct to try to simulate the `pylint.extension` needing to be enabled too. If this route isn't the way you'd like to head and this rule is too opinionated I understand not implementing :smile_cat: 

---

_Comment by @charliermarsh on 2023-03-22 03:23_

That's a clever solution... Even _with_ that, I think I'm gonna err on the side of skipping this one. The issue is we now have two ways to "turn on" the rule -- e.g., if a user does `--select PLW0149`, the rule actually wouldn't turn on, which feels unintuitive. But it _also_ feels unintuitive for that to turn on the rule despite the presence of this extension setting!

---

_Closed by @charliermarsh on 2023-03-22 03:23_

---

_Comment by @charliermarsh on 2023-03-22 03:25_

BTW -- you're definitely welcome to keep picking off Pylint or other rules, but if it'd be ever helpful to get alignment before putting in the work, I'm always happy to chime in and confirm or discuss etc., either via comments in any relevant issue or on Discord. It's totally up to you, it just always hurts to close people's PRs or reject proposals after folks have put in good work!

---

_Comment by @latonis on 2023-03-22 03:28_

> BTW -- you're definitely welcome to keep picking off Pylint or other rules, but if it'd be ever helpful to get alignment before putting in the work, I'm always happy to chime in and confirm or discuss etc., either via comments in any relevant issue or on Discord. It's totally up to you, it just always hurts to close people's PRs or reject proposals after folks have put in good work!

I appreciate that, I've been jumping in head first into the Ruff ecosystem and it's allowing me to learn Rust and be able to contribute to a great community and project, so I've just started working on them first and have no hard feelings on closing a PR :smile: . However, will definitely ping you in the future if the rule seems very opinionated or noisy on how you'd like to proceed with it :smile: 

---

_Branch deleted on 2023-03-22 03:28_

---

_Comment by @charliermarsh on 2023-03-22 03:30_

For sure, I've really appreciated all your contributions this far! I hope it's been a good experience!

(FWIW, in general, I'm happy to add anything on the Pylint list, but am wary of adding those rules that are part of Pylint _extensions_ which is typically buried at the bottom of the page for the relevant rule.)

---

_Comment by @github-actions[bot] on 2023-03-22 03:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     13.5±0.07ms     3.0 MB/sec    1.00     13.3±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.1±1.50µs     6.0 MB/sec    1.00    487.8±1.09µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1607.5±3.46µs    10.4 MB/sec    1.01   1629.4±3.71µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.0±0.75µs    16.5 MB/sec    1.01    181.3±1.49µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.00ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     24.8±1.33ms  1682.9 KB/sec    1.02     25.3±1.37ms  1646.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.5±0.46ms     2.6 MB/sec    1.06      6.9±0.91ms     2.4 MB/sec
linter/all-rules/numpy/globals.py          1.04   807.3±85.21µs     3.7 MB/sec    1.00   774.5±63.32µs     3.8 MB/sec
linter/all-rules/pydantic/types.py         1.00     11.0±1.39ms     2.3 MB/sec    1.02     11.2±0.96ms     2.3 MB/sec
linter/default-rules/large/dataset.py      1.00     13.2±0.94ms     3.1 MB/sec    1.14     15.0±0.94ms     2.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.8±0.23ms     6.0 MB/sec    1.04      2.9±0.13ms     5.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   328.8±38.24µs     9.0 MB/sec    1.04   343.4±28.36µs     8.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.9±0.47ms     4.3 MB/sec    1.10      6.5±0.38ms     3.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
