```yaml
number: 4158
title: Allow linking to individual rules
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: link-individual-rules
created_at: 2023-04-30T10:25:50Z
updated_at: 2023-05-10T14:55:13Z
url: https://github.com/astral-sh/ruff/pull/4158
synced_at: 2026-01-12T03:56:38Z
```

# Allow linking to individual rules

---

_Pull request opened by @calumy on 2023-04-30 10:25_

Adds an id to each rule to allow linking directly to the rule.

Closes #3420

---

_Comment by @github-actions[bot] on 2023-04-30 10:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.04ms     2.9 MB/sec    1.00     13.9±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     5.0 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    417.8±3.04µs     7.1 MB/sec    1.01    422.3±0.87µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.01      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1491.1±2.13µs    11.2 MB/sec    1.00   1498.2±3.45µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.7±0.84µs    17.6 MB/sec    1.01    168.6±0.32µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.05ms     7.5 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1062.1±0.66µs    15.7 MB/sec    1.00   1063.1±0.62µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    109.0±0.19µs    27.1 MB/sec    1.00    108.4±0.43µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.5±0.56ms  1936.7 KB/sec    1.01     21.7±0.70ms  1919.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.5±0.26ms     3.0 MB/sec    1.00      5.3±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   624.4±33.61µs     4.7 MB/sec    1.00   611.6±21.71µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.33ms     2.8 MB/sec    1.00      9.1±0.38ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.9±0.38ms     3.7 MB/sec    1.00     10.7±0.34ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.10ms     7.3 MB/sec    1.00      2.3±0.09ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   252.8±14.94µs    11.7 MB/sec    1.04   262.3±20.06µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.19ms     5.3 MB/sec    1.02      4.9±0.19ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.5±0.31ms     4.8 MB/sec    1.02      8.6±0.26ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1606.9±59.36µs    10.4 MB/sec    1.03  1654.2±70.76µs    10.1 MB/sec
parser/numpy/globals.py                    1.00    164.2±8.82µs    18.0 MB/sec    1.02   166.8±10.61µs    17.7 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.10ms     7.2 MB/sec    1.05      3.7±0.30ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `crates/ruff_dev/src/generate_rules_table.rs`:25 on 2023-04-30 11:01_

```suggestion
            "| <div id='{0}{1}'>{0}{1}</div> | {2} | {3} | {4} |",
```
You can remove the next lines by using this.

---

_@JonathanPlasse reviewed on 2023-04-30 11:01_

---

_@calumy reviewed on 2023-04-30 11:09_

---

_Review comment by @calumy on `crates/ruff_dev/src/generate_rules_table.rs`:25 on 2023-04-30 11:09_

Thanks! Updated in 50b70c686cceae67c61c6fca93e6fa2fe78c5c67

---

_Comment by @JonathanPlasse on 2023-04-30 15:03_

You could also link the individual rules mentioned elsewhere to the links you created.
In [`task-tags`](https://beta.ruff.rs/docs/settings/#task-tags) setting, `ERA` and `E501` are mentioned and could link back to the links you added in this PR.
They all seem to follow the regex ``\(`[A-Z]{1,3}[0-9]{0,4}`\)``.

---

_Comment by @calumy on 2023-04-30 16:00_

> You could also link the individual rules mentioned elsewhere to the links you created. In [`task-tags`](https://beta.ruff.rs/docs/settings/#task-tags) setting, `ERA` and `E501` are mentioned and could link back to the links you added in this PR. They all seem to follow the regex `` \(`[A-Z]{1,3}[0-9]{0,4}`\) ``.

Thanks for the suggestion @JonathanPlasse. I wonder if doing this automatically might lead to quite a few false positives and, therefore, if it might be better to do this manually in the specific documentation sections. 

An example of a false positive could be if `V101` in the [external settings section](https://beta.ruff.rs/docs/settings/#external) was linked to `https://beta.ruff.rs/docs/rules/#V101`, which is currently not valid a valid rule section. Another example that matches your provided regex is "(`key`)" in the [invalid-envvar-value (PLE1507)](https://beta.ruff.rs/docs/rules/invalid-envvar-value/) rules; this would direct users to `https://beta.ruff.rs/docs/rules/#key`, which is also not currently a valid rule section. 



---

_Comment by @JonathanPlasse on 2023-04-30 16:09_

> > You could also link the individual rules mentioned elsewhere to the links you created. In [`task-tags`](https://beta.ruff.rs/docs/settings/#task-tags) setting, `ERA` and `E501` are mentioned and could link back to the links you added in this PR. They all seem to follow the regex `` \(`[A-Z]{1,3}[0-9]{0,4}`\) ``.
> 
> Thanks for the suggestion @JonathanPlasse. I wonder if doing this automatically might lead to quite a few false positives and, therefore, if it might be better to do this manually in the specific documentation sections.
> 
> An example of a false positive could be if `V101` in the [external settings section](https://beta.ruff.rs/docs/settings/#external) was linked to `https://beta.ruff.rs/docs/rules/#V101`, which is currently not valid a valid rule section. Another example that matches your provided regex is "(`key`)" in the [invalid-envvar-value (PLE1507)](https://beta.ruff.rs/docs/rules/invalid-envvar-value/) rules; this would direct users to `https://beta.ruff.rs/docs/rules/#key`, which is also not currently a valid rule section.

To avoid false positives like `V101` we could check if the rule exists.
``(`key`)`` should not match the regex as it uses lower cases.

---

_Comment by @calumy on 2023-04-30 16:27_

Checking the rule exists seems reasonable. 

To be honest, this might be beyond my Rust skills. @JonathanPlasse, would you consider working on this, or should I open an issue as a feature request? 

---

_Comment by @JonathanPlasse on 2023-04-30 16:45_

I will open an issue.
I may work on it if I have some time.


---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_rules_table.rs`:25 on 2023-05-01 10:10_

You can use `mkdocs` [attribute-lists](https://python-markdown.github.io/extensions/attr_list/#inline) markdown extension to set the id without the need for a wrapper `div`
```suggestion
            "| {0}{1} {{ #{0}{1} }} | {2} | {3} | {4} |",
```

---

_@MichaReiser reviewed on 2023-05-01 10:10_

---

_@MichaReiser approved on 2023-05-01 10:47_

---

_Comment by @calumy on 2023-05-01 10:50_

Tests are failing due to not being able to install "black[d]"'s dependencies. Would it be possible to rerun to check that this is not just a random error? 

---

_Comment by @MichaReiser on 2023-05-01 10:54_

> Tests are failing due to not being able to install "black[d]"'s dependencies. Would it be possible to rerun to check that this is not just a random error?

I triggered a re-run of the failing tests.

---

_Comment by @charliermarsh on 2023-05-02 00:57_

Should we include some kind of visual indicator, e.g., make it a link? (Is that even possible?) I'm also wondering if we should use the human-readable rule name for the URL, rather than the code, but then it becomes kind of duplicative with the standalone rule pages, like:

<img width="1624" alt="Screen Shot 2023-05-01 at 5 57 17 PM" src="https://user-images.githubusercontent.com/1309177/235557093-2c2cb1a4-d5eb-4c4e-a87c-d173456de73e.png">


---

_Comment by @MichaReiser on 2023-05-02 07:04_

> I'm also wondering if we should use the human-readable rule name for the URL, rather than the code, but then it becomes kind of duplicative with the standalone rule pages, like:

I consider this a workaround for the time being until we have standalone rule pages for every rule.

---

_Merged by @charliermarsh on 2023-05-04 17:43_

---

_Closed by @charliermarsh on 2023-05-04 17:43_

---

_Label `documentation` added by @charliermarsh on 2023-05-04 17:43_

---

_Branch deleted on 2023-05-10 14:55_

---
