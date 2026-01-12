```yaml
number: 4166
title: Enable PYI033 for .py files
type: pull_request
state: closed
author: JonathanPlasse
labels:
  - rule
assignees: []
base: main
head: enable-pyi033-for-py-files
created_at: 2023-04-30T21:20:10Z
updated_at: 2023-10-17T11:07:22Z
url: https://github.com/astral-sh/ruff/pull/4166
synced_at: 2026-01-12T15:55:14Z
```

# Enable PYI033 for .py files

---

_@JonathanPlasse_

- Asked in https://github.com/charliermarsh/ruff/issues/980#issuecomment-1529127615
> > It is implemented as PYI033.
> 
> Hm, looks like this is only for stub files (`.pyi`) I cannot get it work for normal python files. man_shrugging

---

_Comment by @github-actions[bot] on 2023-04-30 21:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.06ms     2.9 MB/sec    1.00     14.0±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.2±0.89µs     7.0 MB/sec    1.00    422.3±0.75µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1456.9±4.51µs    11.4 MB/sec    1.00   1460.4±2.37µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.53µs    18.0 MB/sec    1.01    165.4±2.77µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.6 MB/sec    1.00      5.3±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1061.5±0.60µs    15.7 MB/sec    1.00   1051.9±2.31µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.7±0.14µs    27.1 MB/sec    1.00    108.2±0.44µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     26.7±0.63ms  1559.7 KB/sec    1.00     25.3±0.65ms  1647.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      6.6±0.35ms     2.5 MB/sec    1.00      6.2±0.25ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   708.4±39.50µs     4.2 MB/sec    1.01   717.2±47.48µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.7±0.43ms     2.4 MB/sec    1.00     10.5±0.42ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.2±0.49ms     3.3 MB/sec    1.01     12.3±0.38ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.08ms     6.8 MB/sec    1.04      2.5±0.12ms     6.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   301.3±29.78µs     9.8 MB/sec    1.01   303.1±23.68µs     9.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.4±0.18ms     4.7 MB/sec    1.02      5.5±0.33ms     4.6 MB/sec
parser/large/dataset.py                    1.16     11.3±0.50ms     3.6 MB/sec    1.00      9.7±0.31ms     4.2 MB/sec
parser/numpy/ctypeslib.py                  1.16      2.1±0.10ms     8.0 MB/sec    1.00  1804.8±45.80µs     9.2 MB/sec
parser/numpy/globals.py                    1.09   209.1±11.28µs    14.1 MB/sec    1.00    191.5±7.86µs    15.4 MB/sec
parser/pydantic/types.py                   1.15      4.7±0.36ms     5.5 MB/sec    1.00      4.0±0.14ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/type_comment.rs`:17 on 2023-05-01 10:17_

Do we know the exact versions for *recent versions*? E.g. 3.10+? Should we only check for type comments in `py` files if the configured python version is as new as *recent version*?

---

_@MichaReiser reviewed on 2023-05-01 10:17_

---

_@JonathanPlasse reviewed on 2023-05-01 15:59_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_pyi/rules/type_comment.rs`:17 on 2023-05-01 15:59_

Modern type annotations are available since Python 3.6 so all version ruff support should check it.

---

_Comment by @charliermarsh on 2023-05-01 16:56_

I'd been hesitant to enable these rules for `.py` files since it's a deviation from `flake8-pyi`. I know there's value in enabling this, and a few others which aren't `.pyi`-file specific. I wonder if it should be opt-in, since it's a deviation from upstream... What do you think? (I'd be curious if there's documented reasoning for `flake8-pyi` on limiting to `.pyi` files, I bet there is but I'm on airplane Wi-Fi.)

---

_Comment by @JonathanPlasse on 2023-05-01 17:08_

No flake8-pyi rules are activated on .py files.
I think there is an issue on which rules could be enable on .py files.

---

_Comment by @JonathanPlasse on 2023-05-01 17:09_

Here is the list.
https://github.com/qutebrowser/qutebrowser/issues/7098#issuecomment-1109959035

---

_Comment by @AlexWaygood on 2023-05-03 14:59_

> Here is the list. [qutebrowser/qutebrowser#7098 (comment)](https://github.com/qutebrowser/qutebrowser/issues/7098#issuecomment-1109959035)

We've discussed several times at flake8-pyi whether we wanted to support running our checks on `.py` files, as a lot of our checks could indeed apply to `.py` files as well as `.pyi` files. (The list I gave in https://github.com/qutebrowser/qutebrowser/issues/7098#issuecomment-1109959035 is now incomplete, since I wrote that more than a year ago, and we've added lots of checks since!)

The main reason we don't want to support `.py` files at flake8-pyi it is that it would complicate the code base in an annoying way. We'd have to do loads of `if file.suffix == ".pyi"` checks everywhere, and it would be a pain. We're also a little wary of scope creep.

I'm not too familiar with ruff's codebase (and don't know rust 😆), but my impression is that neither of these concerns apply to ruff in the same way. (You obviously have a... much broader scope than flake8-pyi :)

If I were writing flake8-pyi from scratch today, I'd probably separate it into two plugins -- one which specialised in type hints (and could be run on `.py` files and `.pyi` files), and one which specialised in checks that _only_ applied to `.pyi` files. But I'm not writing it from scratch; I'm pretty happy with how it works overall; and separating it into two plugins at this stage sounds like a lot of work that I don't really feel like doing right now 😆

---

_Label `rule` added by @charliermarsh on 2023-05-12 01:07_

---

_Comment by @JonathanPlasse on 2023-05-14 15:12_

Should we go ahead and merge this or do we want to activate all rules that can apply to `.py` files in this PR?

---

_Comment by @nunokaeru on 2023-05-16 17:31_

> Should we go ahead and merge this or do we want to activate all rules that can apply to `.py` files in this PR?

IMO a Issue tracking all the rules that should be enabled in `.py` files from this plugin would be nice.

Then each PR takes it as one rule at a time, which can then be crossed of from the Issue when merged.

---

_Comment by @JonathanPlasse on 2023-05-20 18:24_

What is blocking this PR?

---

_Comment by @charliermarsh on 2023-05-20 18:40_

I think there are two things:

1. I'd like to turn on a bunch of the `flake8-pyi` rules at once, rather than having inconsistency within the rule set.
2. There _are_ reasons to use type comments in `.py` files (see, e.g., https://github.com/charliermarsh/ruff/issues/1619#issuecomment-1529896008) that don't apply to `.pyi` files, which I don't quite know how to resolve.


---

_Comment by @JonathanPlasse on 2023-05-20 18:57_

Is it possible to get the corresponding statement or expression from a `TextSize`?

---

_Comment by @MichaReiser on 2023-05-22 07:49_

> Is it possible to get the corresponding statement or expression from a `TextSize`?

We could do a binary search over the tree (at least after my changes to RustPython merge that include the decorators in the function range) but I would discourage from doing it in general. What are you trying to do?

---

_Comment by @JonathanPlasse on 2023-05-22 08:37_

- I wanted to find the statement corresponding to the type comment.
- But, it is not needed as all type comments can be removed (c.f.: https://github.com/charliermarsh/ruff/issues/1619#issuecomment-1556796646)

---

_Comment by @charliermarsh on 2023-06-12 16:32_

Closing for now, as I want to do this in a consolidated pass for all rules.

---

_Closed by @charliermarsh on 2023-06-12 16:32_

---

_Branch deleted on 2023-10-17 11:07_

---
