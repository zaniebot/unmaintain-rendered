```yaml
number: 4691
title: add dummy-import-rgx option
type: pull_request
state: closed
author: IamRezaMousavi
labels: []
assignees: []
base: main
head: feature-import-ignores
created_at: 2023-05-28T11:50:59Z
updated_at: 2023-07-30T18:30:03Z
url: https://github.com/astral-sh/ruff/pull/4691
synced_at: 2026-01-12T02:58:30Z
```

# add dummy-import-rgx option

---

_Pull request opened by @IamRezaMousavi on 2023-05-28 11:50_

## Summary
check #4250 


---

_Comment by @github-actions[bot] on 2023-05-28 12:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.08ms     2.9 MB/sec    1.01     14.1±0.18ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.8±0.69µs     6.9 MB/sec    1.00    426.3±0.76µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.05ms     4.4 MB/sec    1.01      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1500.1±2.31µs    11.1 MB/sec    1.00   1496.3±6.21µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.0±0.40µs    17.4 MB/sec    1.00    169.7±1.06µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.16      6.0±0.00ms     6.8 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.13   1152.7±0.81µs    14.4 MB/sec    1.00   1020.4±0.87µs    16.3 MB/sec
parser/numpy/globals.py                    1.10    115.7±0.90µs    25.5 MB/sec    1.00    105.3±0.15µs    28.0 MB/sec
parser/pydantic/types.py                   1.14      2.5±0.00ms    10.1 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     18.0±1.05ms     2.3 MB/sec     1.19     21.5±0.87ms  1933.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.24ms     3.3 MB/sec     1.05      5.3±0.29ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   564.2±36.42µs     5.2 MB/sec     1.14   643.4±21.25µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.69ms     2.9 MB/sec     1.02      9.0±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.52ms     4.4 MB/sec     1.21     11.1±0.50ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1947.4±113.22µs     8.6 MB/sec    1.22      2.4±0.06ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.3±25.54µs    11.6 MB/sec     1.12   282.5±16.28µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.19ms     6.1 MB/sec     1.21      5.0±0.23ms     5.1 MB/sec
parser/large/dataset.py                    1.06      8.5±0.26ms     4.8 MB/sec     1.00      8.0±0.30ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.03  1583.1±72.90µs    10.5 MB/sec     1.00  1543.0±81.12µs    10.8 MB/sec
parser/numpy/globals.py                    1.00   142.9±12.61µs    20.6 MB/sec     1.18    169.2±6.17µs    17.4 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.24ms     7.6 MB/sec     1.10      3.7±0.13ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/defaults.rs`:37 on 2023-05-30 06:59_

Nit: Can we use a single regex here (assuming they are indeed the same)
```suggestion
pub static DUMMY_VARIABLE_OR_IMPORT_RGX: Lazy<Regex> =
    Lazy::new(|| Regex::new("^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$").unwrap());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:5141 on 2023-05-30 07:00_

Nit: I recommend moving the line below the line 5152. The `ImportFrom` check should be cheaper than testing the name.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:5143 on 2023-05-30 07:01_

Do we need to add an entry to the `ignored` vec @charliermarsh ?

---

_@MichaReiser reviewed on 2023-05-30 07:01_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-30 07:01_

---

_@IamRezaMousavi reviewed on 2023-05-30 11:32_

---

_Review comment by @IamRezaMousavi on `crates/ruff/src/settings/defaults.rs`:37 on 2023-05-30 11:32_

It's not compatible

---

_@IamRezaMousavi reviewed on 2023-05-30 11:48_

---

_Review comment by @IamRezaMousavi on `crates/ruff/src/checkers/ast/mod.rs`:5141 on 2023-05-30 11:48_

This is a good idea

But what if I want import like this?
If I just want to ignore _Name:

```python
from ImportFrom import Name as _Name
```
or
```python
from ImportFrom import OtherName, _Name
```



---

_@MichaReiser reviewed on 2023-05-30 12:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/defaults.rs`:37 on 2023-05-30 12:53_

Are you saying they are not the same or is there another reason, why they are not compatible?

---

_@IamRezaMousavi reviewed on 2023-05-30 12:59_

---

_Review comment by @IamRezaMousavi on `crates/ruff/src/settings/defaults.rs`:37 on 2023-05-30 12:59_

My mistake, I thought you wanted to change `dummy_variable_rgx` option to `dummy-variable-or-import-rgx`

---

_Comment by @charliermarsh on 2023-06-12 17:21_

I have two hesitations here that I'm not certain how to address:

1. What rules should this affect? Right now, it only affects `unused_import`. But the setting is highly generic (`dummy-import-rgx`). In theory, should it also effect all other rules that flag imports? Like the wildcard import rules (`from resources import *`)? If not, we need a more deliberate name, like `side_effect_imports` or `effectful_modules` etc.

1. Should this be a regex, or should it instead be a list of module paths? `dummy-variable-rgx` makes sense because it's matching on arbitrary identifiers. But here, we're matching against specific modules, like `foo.bar.baz`. I would prefer, I think, to require the user to pass in a list of fully-qualified module paths.




---

_Comment by @IamRezaMousavi on 2023-06-12 23:59_

Can I share an idea?


> 1. What rules should this affect? Right now, it only affects `unused_import`. But the setting is highly generic (`dummy-import-rgx`). In theory, should it also effect all other rules that flag imports? Like the wildcard import rules (`from resources import *`)? If not, we need a more deliberate name, like `side_effect_imports` or `effectful_modules` etc.

I think `dummy-import-rgx` look like `dummy-variable-rgx` which the simple way to ignored all rules about this import name 
ps1: Can we merge the two together?
ps2: I think we mustn't delete `dummy-variable-rgx` for compatible reason, but can use that for ignore import names. What you think?

> 2. Should this be a regex, or should it instead be a list of module paths? `dummy-variable-rgx` makes sense because it's matching on arbitrary identifiers. But here, we're matching against specific modules, like `foo.bar.baz`. I would prefer, I think, to require the user to pass in a list of fully-qualified module paths.

This is another option. This option look like `per-file-ignores`, we can implement this and give users the option to decide which import rules to ignore for which package

---

_Comment by @charliermarsh on 2023-07-21 04:08_

I think I'm going to close this one out for now (in shame, since I never got back to you on your follow-up ideas). My current feeling is that I'm hesitant to add more configuration options without thinking through configuration more holistically, which is something we have on the roadmap. I'm sorry for both (1) not resolving your immediate problem and (2) being be a better maintainer / partner on this one.

---

_Closed by @charliermarsh on 2023-07-21 04:08_

---

_Branch deleted on 2023-07-30 18:30_

---
