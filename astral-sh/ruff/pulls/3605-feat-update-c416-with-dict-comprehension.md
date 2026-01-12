```yaml
number: 3605
title: "feat: update C416 with dict comprehension (autofixable)"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/dict-comprehension
created_at: 2023-03-19T09:40:42Z
updated_at: 2023-03-20T03:35:04Z
url: https://github.com/astral-sh/ruff/pull/3605
synced_at: 2026-01-12T04:39:45Z
```

# feat: update C416 with dict comprehension (autofixable)

---

_Pull request opened by @dhruvmanila on 2023-03-19 09:40_

Update C416 with dict comprehensions which is auto-fixable.

~This required changing the function signature of `unnecessary_comprehension` to take a vector of elements as a dict comprehension has 2 of them (key, value).~ _Look at review comments_

resolves: #3598 

---

_Comment by @github-actions[bot] on 2023-03-19 09:54_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+10, -0, 0 error(s))

<details><summary>airflow (+7, -0)</summary>
<p>

```diff
+ airflow/api/common/experimental/get_lineage.py:50:25: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ airflow/models/dagrun.py:281:16: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ airflow/operators/python.py:386:37: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ airflow/www/decorators.py:103:26: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ docs/conf.py:410:29: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ docs/conf.py:411:15: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ docs/conf.py:413:39: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
```

</p>
</details>
<details><summary>cibuildwheel (+1, -0)</summary>
<p>

```diff
+ test/test_dependency_versions.py:48:12: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
```

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

```diff
+ zerver/lib/onboarding.py:26:18: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
+ zerver/management/commands/makemessages.py:280:27: C416 [*] Unnecessary `dict` comprehension (rewrite using `dict()`)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.53ms     2.7 MB/sec    1.00     14.9±0.59ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.12ms     4.4 MB/sec    1.07      4.1±0.13ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   544.2±33.72µs     5.4 MB/sec    1.04   563.7±26.56µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.27ms     3.8 MB/sec    1.03      7.0±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.22ms     5.1 MB/sec    1.10      8.7±0.39ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1932.2±79.25µs     8.6 MB/sec    1.00  1847.5±51.59µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    212.0±9.44µs    13.9 MB/sec    1.07    225.8±8.72µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.20ms     6.5 MB/sec    1.03      4.1±0.25ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.12ms     2.8 MB/sec    1.00     14.6±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    520.1±5.45µs     5.7 MB/sec    1.00   520.0±10.88µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.00      6.5±0.08ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.03      8.3±0.21ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.8±22.65µs     9.4 MB/sec    1.02  1811.9±32.87µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.2±2.94µs    15.0 MB/sec    1.00    196.9±2.54µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-03-19 10:36_

There are a few cases which I've missed, let me look into them.

---

_Converted to draft by @dhruvmanila on 2023-03-19 10:36_

---

_Marked ready for review by @dhruvmanila on 2023-03-19 11:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:49 on 2023-03-19 15:26_

Can we try to split this into two separate cases: `List` and `Set` vs. `Dict`? Since for `List` and `Set`, I believe we _always_ want a `Name` here, whereas for `Dict`, we _always_ want a length-two tuple. (Looking at the [flake8-comprehensions implementation](https://github.com/adamchainz/flake8-comprehensions/pull/490/files#diff-2fafc059a4a4fef0240cd305df8b843c6937ce529a7905567860392cd1cc76d4R311) for reference.) Right now, those expectations are all implicit.


---

_@charliermarsh reviewed on 2023-03-19 15:26_

---

_@charliermarsh reviewed on 2023-03-19 15:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:37 on 2023-03-19 15:26_

I'm tempted to say that the change in signature here means we should use two separate functions entirely -- what do you think?

---

_Comment by @Skylion007 on 2023-03-19 16:52_

Thanks for doing, I gave a pass at adding it myself, but I don't have a lot of experience with RustLang or the Ruff codebase.

---

_@dhruvmanila reviewed on 2023-03-19 17:20_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:37 on 2023-03-19 17:20_

Actually, I did start by defining a separate function but then most of the code remained the same. So, then I thought to combine them. Separate functions do have a pro of having a simpler body to understand and will be the same as the reference implementation. 

Now that I think of it, post reading your previous comment as well, I believe separate functions will be better to avoid any implicit expectations.

---

_Comment by @dhruvmanila on 2023-03-19 17:47_

* I've updated with separating out the `dict` and `list`/`set` part with a helper function to add a diagnostic. 
* The name of the existing function has been changed to `unnecessary_list_set_comprehension` to better reflect the purpose.
* Add documentation for the rule. Will it be auto-generated for the website?

I'm not sure about the merge strategy used here, so should I rebase and squash relevant commits together?

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-03-19 17:48_

---

_Comment by @charliermarsh on 2023-03-19 17:48_

Great, thanks! Will take a look today.

> Add documentation for the rule. Will it be auto-generated for the website?

Yup!


---

_Label `rule` added by @charliermarsh on 2023-03-19 18:31_

---

_Merged by @charliermarsh on 2023-03-19 18:37_

---

_Closed by @charliermarsh on 2023-03-19 18:37_

---

_Comment by @charliermarsh on 2023-03-19 18:59_

Thanks for this! Sorry, I missed your last question. We almost always squash-and-merge to maintain a 1:1 PR-to-commit equivalence, but GitHub does it for us, so no effort needed on your end.

---

_Comment by @dhruvmanila on 2023-03-20 00:45_

Thanks for your feedback and merging! I hope to contribute more as I go deep into Rust and the codebase 😄 

---

_Branch deleted on 2023-03-20 00:52_

---
