```yaml
number: 3741
title: Implement flake8-i18n
type: pull_request
state: merged
author: leiserfg
labels:
  - rule
assignees: []
merged: true
base: main
head: implement-flake8-i18n
created_at: 2023-03-26T15:03:29Z
updated_at: 2023-03-28T21:44:21Z
url: https://github.com/astral-sh/ruff/pull/3741
synced_at: 2026-01-12T04:39:45Z
```

# Implement flake8-i18n

---

_Pull request opened by @leiserfg on 2023-03-26 15:03_

- Add skelleton code
- Add fixtures for flake8_i18n
- Add tests
- Add implementation and insta snapshots
- Add license entry

Closes https://github.com/charliermarsh/ruff/issues/3294.


---

_Comment by @github-actions[bot] on 2023-03-26 15:19_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>zulip (+1, -0)</summary>
<p>

```diff
+ corporate/lib/stripe.py:1010:17: INT001 f-string is resolved before function call; consider `_("string %s") % arg`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.08ms     2.8 MB/sec    1.00     14.5±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.03ms     4.4 MB/sec    1.00      3.8±0.02ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.3±1.56µs     6.8 MB/sec    1.00    433.0±1.58µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.02ms     4.0 MB/sec    1.02      6.5±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.06ms     5.2 MB/sec    1.00      7.8±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1712.6±9.61µs     9.7 MB/sec    1.00   1694.7±2.78µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.8±0.89µs    16.8 MB/sec    1.01    177.0±0.44µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.13ms     2.6 MB/sec    1.00     15.6±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    463.1±7.69µs     6.4 MB/sec    1.00    458.3±8.02µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.06ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1785.6±17.06µs     9.3 MB/sec    1.01  1794.8±17.34µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.5±2.06µs    15.9 MB/sec    1.00    185.5±6.65µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.03ms     6.6 MB/sec    1.00      3.9±0.03ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2917 on 2023-03-27 07:16_

Nit: 
```suggestion
						self.diagnostics.extend(flake8_i18n::rules::f_string_in_i18n_func_call(args))
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_i18n/settings.rs`:40 on 2023-03-27 07:19_

Can we use `Vec<String>` directly or is it important to know whether the settings where empty vs `None`

---

_@MichaReiser reviewed on 2023-03-27 07:19_

---

_Review comment by @leiserfg on `crates/ruff/src/rules/flake8_i18n/settings.rs`:40 on 2023-03-27 07:50_

True, it's fine to use a regular `Vec` here.

---

_@leiserfg reviewed on 2023-03-27 07:50_

---

_@leiserfg reviewed on 2023-03-27 08:39_

---

_Review comment by @leiserfg on `crates/ruff/src/rules/flake8_i18n/settings.rs`:40 on 2023-03-27 08:39_

After this change tests are failing in CI but no idea why, 'cause they pass locally.

---

_Review comment by @mvaled on `crates/ruff/resources/test/fixtures/flake8_i18n/INT001.py`:1 on 2023-03-27 08:50_

`f"{'value'}"` is just the string "value".  I'm not sure if the AST will pick up that.  In general, any f-string inside a i18n function might be problematic. 

```suggestion
_(f"{value}")
```

---

_@mvaled reviewed on 2023-03-27 08:50_

---

_@leiserfg reviewed on 2023-03-27 08:52_

---

_Review comment by @leiserfg on `crates/ruff/resources/test/fixtures/flake8_i18n/INT001.py`:1 on 2023-03-27 08:52_

Yes, it will, any f-string is a JoinedString for the parser.

---

_@leiserfg reviewed on 2023-03-27 08:53_

---

_Review comment by @leiserfg on `crates/ruff/src/rules/flake8_i18n/settings.rs`:40 on 2023-03-27 08:53_

I see, some changes in main, I will rebase to fix them.

---

_Label `rule` added by @charliermarsh on 2023-03-27 17:45_

---

_Comment by @charliermarsh on 2023-03-27 17:45_

Nice work here. I made a few minor but otherwise merging as-is. Thanks for getting involved :)

---

_@charliermarsh reviewed on 2023-03-27 17:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_i18n/settings.rs`:61 on 2023-03-27 17:49_

I changed this to use an iterator.

---

_Merged by @charliermarsh on 2023-03-27 18:03_

---

_Closed by @charliermarsh on 2023-03-27 18:03_

---

_@AA-Turner reviewed on 2023-03-27 20:37_

---

_Review comment by @AA-Turner on `LICENSE`:198 on 2023-03-27 20:37_

@charliermarsh -- I'm not certain on this but wanted to ask the question -- does including a GPL tool in Ruff mean that (all/part of) Ruff would be considered a derivative work for the purposes of GPL, and therefore that Ruff would need to be re-licenced (or this PR reverted or etc)? This is the first GPL tool to be integrated, and I wanted to flag the concern as early as possible. Thanks, Adam

---

_@charliermarsh reviewed on 2023-03-27 20:43_

---

_Review comment by @charliermarsh on `LICENSE`:198 on 2023-03-27 20:43_

Lemme look into it.

---

_@leiserfg reviewed on 2023-03-27 20:49_

---

_Review comment by @leiserfg on `LICENSE`:198 on 2023-03-27 20:49_

There are already other parts including GPL3 (flake8-django for instance).
* edit: well only that one, but I think GPL3 is even more infectious than GPL2.

---

_@charliermarsh reviewed on 2023-03-27 20:57_

---

_Review comment by @charliermarsh on `LICENSE`:198 on 2023-03-27 20:57_

I'll have to get some more informed opinions on this, I'm not sure myself on whether these constitute derivative works.

If they do, and we'd have to re-license under GPL, then I'd err on the side of removing them, or going back to the authors and asking if they'd consider re-licensing under MIT or a dual license.

`flake8-django` is confusing because on PyPI it appears dual-licensed, and it includes the MIT trove classifiers:

![Screen Shot 2023-03-27 at 4 54 36 PM](https://user-images.githubusercontent.com/1309177/228064736-bfddd03b-9cee-4047-b4f4-8624716fba08.png)
![Screen Shot 2023-03-27 at 4 55 30 PM](https://user-images.githubusercontent.com/1309177/228064738-3c54e798-6c2e-437a-ac26-32fada358e54.png)


---

_Comment by @leiserfg on 2023-03-27 21:04_

@charliermarsh  should I talk to @vharitonsky to see if he can change the license for us or will you do it?  

---

_Comment by @charliermarsh on 2023-03-27 21:05_

@leiserfg - If you're willing to that'd be great.


---

_@AA-Turner reviewed on 2023-03-27 21:30_

---

_Review comment by @AA-Turner on `LICENSE`:198 on 2023-03-27 21:30_

> I'll have to get some more informed opinions on this, I'm not sure myself on whether these constitute derivative works.

Sorry to cause the trouble and thank you both for looking in to this.

> `flake8-django` is confusing because on PyPI it appears dual-licensed, and it includes the MIT trove classifiers:

It seems this has been noted already: https://github.com/rocioar/flake8-django/issues/101 and https://github.com/rocioar/flake8-django/pull/102. Sorry not to have flagged this at the time for `flake8-django`, but it seems the `MIT` trove classifier may have been a mistake when that project [adopted Poetry](https://github.com/rocioar/flake8-django/commit/bb9dcc881c36d47db932f91993015fe15c447ff3#diff-50c86b7ed8ac2cf95bd48334961bf0530cdc77b5a56f852c5c61b89d735fd711).

A

---

_@charliermarsh reviewed on 2023-03-27 21:39_

---

_Review comment by @charliermarsh on `LICENSE`:198 on 2023-03-27 21:39_

No, thank _you_ for calling this out! It's important to get right. I'll try to reach out to the `flake8-django` maintainer.


---

_Comment by @leiserfg on 2023-03-28 07:01_

To be honest, I don't think this can be considered derivative work as we barely used some of their ideas, and ideas can be patented but not copyrighted.

---

_@mvaled reviewed on 2023-03-28 07:02_

---

_Review comment by @mvaled on `LICENSE`:198 on 2023-03-28 07:02_

Hum... This is such a common "idea", I've [done it before](https://gitlab.merchise.org/merchise/merchise.lint/-/blob/master/merchise/lint/checkers/odoo_i18n.py) without even knowing about flake8-i18n.   We were working in a project and switched to Python 3.6 and many members of a the team started to use f-strings inside `_`.  I became such a common mistake we had to "invent" our checker.

I'm not a lawyer, but I don't think this can be gathered as derivative work.

---

_Comment by @leiserfg on 2023-03-28 19:24_

@charliermarsh  I found another [implementation](https://github.com/cielavenir/flake8_gettext) that does basically the same and is licensed under BSD, is it fine if we reference that one instead? 


---

_Comment by @charliermarsh on 2023-03-28 19:58_

@leiserfg - Yeah that looks fine to me. I do think we could in theory include these rules without referencing `flake8-i18n`, especially since the issue that was raised on Ruff pre-dated any knowledge of that plugin. But it seems most straightforward to base it on `flake8-gettext`. Want to put up a PR?

---

_Comment by @charliermarsh on 2023-03-28 19:59_

Let's continue to use the `INT` codes (which I think differs from either of those plugins).

---

_Comment by @leiserfg on 2023-03-28 21:06_

https://github.com/charliermarsh/ruff/pull/3785


---

_Comment by @charliermarsh on 2023-03-28 21:44_

Thx!

---
