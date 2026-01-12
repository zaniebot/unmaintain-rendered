```yaml
number: 4701
title: Implement copyright notice detection
type: pull_request
state: merged
author: Ryang20718
labels:
  - plugin
assignees: []
merged: true
base: main
head: implement-flake8-copyright
created_at: 2023-05-29T07:39:31Z
updated_at: 2023-06-20T23:09:14Z
url: https://github.com/astral-sh/ruff/pull/4701
synced_at: 2026-01-12T03:43:29Z
```

# Implement copyright notice detection

---

_Pull request opened by @Ryang20718 on 2023-05-29 07:39_

## Summary

Add copyright notice detection to enforce the presence of copyright headers in Python files.

Configurable settings include: the relevant regular expression, the author name, and the minimum file size, similar to [flake8-copyright](https://github.com/savoirfairelinux/flake8-copyright).

Closes https://github.com/charliermarsh/ruff/issues/3579


---

_Review comment by @Ryang20718 on `crates/ruff/src/checkers/physical_lines.rs`:163 on 2023-05-29 07:46_

Could also do dep injection here and pass regexp as an arg. However, that would essentially put all the logic in physical_lines.rs. Let me know which you would prefer

---

_@Ryang20718 reviewed on 2023-05-29 07:46_

---

_Marked ready for review by @Ryang20718 on 2023-05-29 07:50_

---

_Comment by @github-actions[bot] on 2023-05-29 08:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.09ms     2.7 MB/sec    1.01     15.3±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.10      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    377.8±1.41µs     7.8 MB/sec    1.90    717.4±4.90µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.04      6.6±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.03ms     5.4 MB/sec    1.00      7.5±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1613.6±11.13µs    10.3 MB/sec    1.00   1549.2±5.07µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.06    176.7±0.58µs    16.7 MB/sec    1.00    167.0±0.14µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.00ms     7.5 MB/sec    1.00      3.3±0.04ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.03      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1127.4±0.78µs    14.8 MB/sec    1.03   1161.5±4.55µs    14.3 MB/sec
parser/numpy/globals.py                    1.00    115.2±0.39µs    25.6 MB/sec    1.04    119.8±0.36µs    24.6 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.02      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.22ms     2.4 MB/sec    1.03     17.4±0.44ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.09ms     3.8 MB/sec    1.08      4.7±0.03ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    465.6±8.56µs     6.3 MB/sec    1.85   859.3±15.41µs     3.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.06ms     3.5 MB/sec    1.06      7.6±0.13ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.20ms     4.7 MB/sec    1.01      8.7±0.12ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1822.8±38.48µs     9.1 MB/sec    1.00  1811.9±32.52µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    197.2±1.57µs    15.0 MB/sec    1.00    194.2±7.07µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.03ms     6.6 MB/sec    1.00      3.9±0.05ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.04      7.0±0.03ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1279.3±10.69µs    13.0 MB/sec    1.04  1330.2±14.93µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    131.4±1.49µs    22.4 MB/sec    1.06    139.6±2.17µs    21.1 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.04      3.0±0.02ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Ryang20718 on 2023-05-29 20:40_

Hi, I made a first stab at implementing https://github.com/charliermarsh/ruff/pull/4701 flake8-copyright  

However, it seems like the "pre-commit" check in CI is failing due to a lack of copyright headers on some python files in the scripts directory https://github.com/charliermarsh/ruff/actions/runs/5115146921/jobs/9196121487?pr=4701
Wondering if it would be preferable to add the copyright header, disable the check or add noqa C801?

When I tried adding noqa, when emulating CI and running the command locally, it would overwrite my changes. 

---

_Comment by @Ryang20718 on 2023-05-29 20:43_

Sorry if this was documented somewhere/asked again, but wondering if there's a group of people to assign as reviewers for PR's? 


---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/physical_lines.rs`:179 on 2023-05-30 06:36_

`line.len` gives you the number of bytes on that line, not the number of characters. The two numbers are different for non-ascii strings (e.g. emojis consist of multiple bytes, but represent a single character)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:31 on 2023-05-30 06:37_

We should gracefully handle the case where the `Regex` is invalid rather than just panicking. 

@charliermarsh is there a place where we could do this deserialization outside of the rule (maybe in the configuration to settings deserialization?)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:34 on 2023-05-30 06:39_

Ruff only supports files up to 4GB. Using a u32 should be fine.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:26 on 2023-05-30 06:41_

Nit: I think it would improve readability if this rule returns a custom enum instead of `Option<bool>`

```
#[derive(Copy, Clone, Eq, PartialEq)]
enum CopyrightHeaderKind{
	Missing,
	Present,
	TooSmall
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:22 on 2023-05-30 06:42_

Is it necessary that this rule runs on every line? Could we instead call the rule just once, passing the full source code? Should this rule only run on comments (or doc-strings)? rather than on all source code?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/settings.rs`:52 on 2023-05-30 06:42_

Does the rule use `-1` to indicate no limit? 

I think I would prefer the use of an `Option` instead. 


```suggestion
    pub copyright_min_file_size: Option<u32>,
```



---

_@MichaReiser reviewed on 2023-05-30 06:45_

---

_@Ryang20718 reviewed on 2023-05-30 16:20_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:22 on 2023-05-30 16:20_

![image](https://github.com/charliermarsh/ruff/assets/31294356/f2e95a9d-e4ea-47b5-a580-2f4104645a46)
I initially asked on discord. I think it would more efficient as well to pass the full source code as well.

Is there any rule that currently does this pattern or should I just create a separate portion in check_physical_lines to pass the entire source code?

---

_@Ryang20718 reviewed on 2023-05-30 16:21_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:26 on 2023-05-30 16:21_

Will do!

---

_@Ryang20718 reviewed on 2023-05-30 16:21_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:34 on 2023-05-30 16:21_

Will fix

---

_@MichaReiser reviewed on 2023-05-30 20:27_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:22 on 2023-05-30 20:27_

@charliermarsh what's your take?

---

_@Ryang20718 reviewed on 2023-05-31 06:42_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:31 on 2023-05-31 06:42_

Made it default to known regex string if user specified regex passed in panics for now, but open to better suggestions

---

_@Ryang20718 reviewed on 2023-05-31 06:47_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/settings.rs`:52 on 2023-05-31 06:47_

converted to u32, max will be 1024 chars with default always be. rule doesn't use -1

---

_@charliermarsh reviewed on 2023-06-05 02:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:22 on 2023-06-05 02:27_

Yeah I'd suggest rewriting to call once per file, especially since it's limited to the first N characters anyway.

---

_@charliermarsh reviewed on 2023-06-05 02:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:741 on 2023-06-05 02:29_

Nit: let's use `CPY` here, I'd like to avoid overloading the `C` prefix.

---

_@charliermarsh reviewed on 2023-06-05 02:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_copyright/settings.rs`:12 on 2023-06-05 02:31_

Can we do a brief GitHub Codesearch to see how often (if ever) these options are used? And if they're important to support?

---

_@charliermarsh reviewed on 2023-06-05 02:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_copyright/settings.rs`:1 on 2023-06-05 02:31_

Nit: double comment pragma here.

---

_@charliermarsh reviewed on 2023-06-05 02:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_copyright/rules/copyright_header_absent.rs`:7 on 2023-06-05 02:32_

Nit: if we do need a lazy regex, lets just use `Lazy::new(|| Regex::new(...))`, to avoid adding another dependency.

---

_@Ryang20718 reviewed on 2023-06-05 02:53_

---

_Review comment by @Ryang20718 on `crates/ruff/src/rules/flake8_copyright/settings.rs`:12 on 2023-06-05 02:53_

From a brief text based search on public repos,

Copyright min file size is used in 70+ places. https://github.com/search?q=copyright-min-file-size&type=code
Copyright regexp is used in 370+ places. https://github.com/search?q=copyright-regexp&type=code
Copyright Author is used in 290+ places https://github.com/search?q=%22copyright-author+%3D+%22&type=code

---

_Comment by @Ryang20718 on 2023-06-06 02:19_

Addressed comments. 

This PR should be good for re-review. However, the CI problem still lingers. I tried adding `# noqa: CPY801` to the python files, but autofix removes the line. I could add a line to these scripts such as `# Copyright (c) 2023 ruff` or is there a way we can disable this check when running ruff on this repo for that particular github action?


![image](https://github.com/charliermarsh/ruff/assets/31294356/121b234e-88fd-45c1-9955-6a4d730eace6)


---

_Comment by @charliermarsh on 2023-06-06 02:32_

Sorry, missed that question -- can we add `CPY` to the `ignore` in `scripts/pyproject.toml`?

---

_Comment by @Ryang20718 on 2023-06-06 02:36_

TIL nested pyproject.toml. Neat! 

---

_Comment by @Ryang20718 on 2023-06-07 03:02_

addressed all comments. Wondering if @charliermarsh or @MichaReiser could re-review please?

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-10 01:56_

---

_Renamed from "Implement flake8 copyright" to "Implement copyright notice detection" by @charliermarsh on 2023-06-11 02:07_

---

_Label `plugin` added by @charliermarsh on 2023-06-11 02:09_

---

_Merged by @charliermarsh on 2023-06-11 02:17_

---

_Closed by @charliermarsh on 2023-06-11 02:17_

---

_Comment by @gertvdijk on 2023-06-20 22:19_

Hi! Cool to see a another new rule has landed.

Two comments - as seen in 0.0.237:

1.  The listing of linters appears inconsistent:
    ```
    $ ruff linter
    [...]
     COM flake8-commas
     CPY Copyright-related rules
      C4 flake8-comprehensions
    [...]
    ```
    That line should have been  `CPY flake8-copyright`, I think.

2. [The docs](https://beta.ruff.rs/docs/rules/missing-copyright-notice/) say ...

    > This rule is part of the nursery, a collection of newer lints that are still under development. As such, it must be enabled by explicitly selecting CPY001.

    ... but it's enabled already using `CPY` (broader). Could it be an accidental copy/paste from some E-rules?

---

_Comment by @charliermarsh on 2023-06-20 23:05_

Thanks for this. I had intentionally renamed to "Copyright-related rules" but perhaps that was a mistake.

(I think it's probably a bug that `--select CPY` works... but it's not a big deal in this case since the category only has the one rule, so I might not even bother to fix :joy:)

---

_Branch deleted on 2023-06-20 23:09_

---
