```yaml
number: 3569
title: "ci(check_ecosystem): add PyPa/build"
type: pull_request
state: merged
author: henryiii
labels: []
assignees: []
merged: true
base: main
head: henryiii/ci/build
created_at: 2023-03-17T03:08:32Z
updated_at: 2023-03-18T23:09:23Z
url: https://github.com/astral-sh/ruff/pull/3569
synced_at: 2026-01-12T15:55:13Z
```

# ci(check_ecosystem): add PyPa/build

---

_@henryiii_

Adding PyPA/build.



---

_Comment by @github-actions[bot] on 2023-03-17 03:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.06ms     3.0 MB/sec    1.00     13.8±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.9±1.82µs     5.8 MB/sec    1.00    505.5±1.15µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1688.5±4.52µs     9.9 MB/sec    1.00   1686.5±4.27µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.8±0.61µs    15.3 MB/sec    1.00    192.5±0.44µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.3 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.9±0.27ms     2.6 MB/sec    1.00     15.8±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    544.0±8.03µs     5.4 MB/sec    1.00    542.2±8.34µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.10ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.8±0.09ms     4.6 MB/sec    1.00      8.7±0.13ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1921.3±20.96µs     8.7 MB/sec    1.00  1907.8±21.42µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.2±2.91µs    14.0 MB/sec    1.02    214.9±5.95µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.05ms     6.3 MB/sec    1.01      4.1±0.07ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-17 03:37_

@henryiii - Any ideas on how to resolve the above? 

---

_Comment by @henryiii on 2023-03-17 03:55_

It’s reading the pyproject.toml’s in test, I guess? Those need to be ignored. There are examples of broken toml files. Running it normally doesn’t have this issue.

---

_Comment by @charliermarsh on 2023-03-17 03:57_

I do get it when running locally:

```
❯ ruff .
error: TOML parse error at line 2, column 19
  |
2 | requires = ['bad' 'syntax']
  |                   ^
invalid array
expected `]
```

Does the typical invocation run over `src` specifically, or something like that?

---

_Comment by @henryiii on 2023-03-17 04:28_

Typical invocation is via pre-commit, https://github.com/pypa/build/blob/e496fe6342f0e4729b69d0ea93eef529f27875a6/.pre-commit-config.yaml#L41. It probably passes all Python files explicitly. Maybe it doesn’t pick up nested pyproject.toml files in that case?

---

_Comment by @henryiii on 2023-03-17 04:30_

Will it ignore them if I exclude tests/**/pyproject.toml?

---

_Renamed from "ci(check_ecosystem): add build" to "ci(check_ecosystem): add PyPa/build" by @MichaReiser on 2023-03-17 07:38_

---

_Comment by @henryiii on 2023-03-17 14:06_

```toml
exclude = ["tests/packages/test-bad-syntax/pyproject.toml"]
```

Doesn't seem to get `ruff .` to ignore trying to read this pyproject.toml file.

---

_Comment by @charliermarsh on 2023-03-17 16:08_

I'll take a look, we need to exclude the entire directory so that it doesn't try to find the `pyproject.toml` file for any of the surfaced files.

---

_Comment by @charliermarsh on 2023-03-17 18:41_

Omitting `test` entirely works, but that's not what you want. Omitting _just_ that directory doesn't seem to work. I'll take a look, we might need to fix something in Ruff.

---

_Comment by @henryiii on 2023-03-17 18:41_

Yeah, I tried omitting the directory and the file. Don't want to omit `test`. :)

---

_Comment by @charliermarsh on 2023-03-17 18:44_

Haha yeah I figured :)

We have to load configuration files as we traverse, because the set of exclusions can change as we go deeper into the filesystem and find the relevant configuration files for each directory. So I'm guessing we're loading that config too eagerly somewhere despite the exclusion.

---

_Comment by @henryiii on 2023-03-18 18:49_

I expect the exclusion still needs to be added to build. I can do that in a bit.

---

_Comment by @charliermarsh on 2023-03-18 18:51_

🤦 Right right, sorry. A _release_ isn't necessary, but the exclusion does need to be added.

---

_Comment by @henryiii on 2023-03-18 20:07_

Why is it passing, actually?

---

_Comment by @charliermarsh on 2023-03-18 20:09_

The ecosystem check doesn't fail CI, but the error _is_ being reported in the comment, I think:

![Screen Shot 2023-03-18 at 4 08 32 PM](https://user-images.githubusercontent.com/1309177/226135856-b6514fd6-b520-4981-a950-b59f832e9cb8.png)


---

_Merged by @charliermarsh on 2023-03-18 23:09_

---

_Closed by @charliermarsh on 2023-03-18 23:09_

---
