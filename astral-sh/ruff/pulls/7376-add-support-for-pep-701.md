```yaml
number: 7376
title: Add support for PEP 701
type: pull_request
state: merged
author: dhruvmanila
labels:
  - core
  - python312
assignees: []
merged: true
base: main
head: dhruv/pep-701
created_at: 2023-09-14T02:22:57Z
updated_at: 2023-10-02T18:00:19Z
url: https://github.com/astral-sh/ruff/pull/7376
synced_at: 2026-01-12T15:55:23Z
```

# Add support for PEP 701

---

_@dhruvmanila_

## Summary

This PR adds support for PEP 701 in Ruff. This is a rollup PR of all the other individual PRs. The separate PRs were created for logic separation and code reviews. Refer to each pull request for a detail description on the change.

### Pull requests

#### Parser

- #6659 
- #7041 
- #7211 
- #7263 
- #7331
- #7378 
- #7667

#### Linter

- #7325 
- #7326 
- #7327 
- #7328 
- #7329
- #7387
- #7477 
- #7515 
- #7588 
- #7589
- #7690

#### Formatter

- #7586 
- #7597 

## Test Plan

### Formatter ecosystem checks

Explanation for the change in ecosystem check: https://github.com/astral-sh/ruff/pull/7597#issue-1908878183

#### `main`

```
| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |
```

#### `dhruv/pep-701`

```
| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76051 |              1789 |              1632 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |
```

---

_Comment by @dhruvmanila on 2023-09-14 02:29_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7376?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-09-14 02:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/pep-701)

### Merging #7376 will **degrade performances by 5.74%**

<sub>Comparing <code>dhruv/pep-701</code> (204a62c) with <code>main</code> (78b8741)</sub>



### Summary

`❌ 10` regressions
`✅ 15` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/pep-701)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/pep-701` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[unicode/pypinyin.py]` | 591.9 µs | 617.5 µs | -4.14% |
| ❌ | `lexer[pydantic/types.py]` | 4 ms | 4.2 ms | -5.74% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 6.2 ms | 6.4 ms | -2.09% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 37.4 ms | 38.4 ms | -2.4% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.9 ms | 35.7 ms | -2.26% |
| ❌ | `lexer[numpy/globals.py]` | 228.7 µs | 240.2 µs | -4.8% |
| ❌ | `lexer[large/dataset.py]` | 9 ms | 9.5 ms | -4.67% |
| ❌ | `lexer[numpy/ctypeslib.py]` | 1.9 ms | 2 ms | -3.97% |
| ❌ | `linter/all-rules[large/dataset.py]` | 164.5 ms | 168.4 ms | -2.37% |
| ❌ | `linter/default-rules[large/dataset.py]` | 90.8 ms | 95.7 ms | -5.18% |


---

_Comment by @MichaReiser on 2023-09-25 07:47_

Would you mind running ` ./scripts/formatter_ecosystem_checks.sh` before merging and ensuring the numbers are identical to `main`?

---

_Comment by @github-actions[bot] on 2023-09-27 08:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+31, -0, 0 error(s))

<details><summary>content (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/MicroFocusSMAX/Integrations/MicroFocusSMAX/MicroFocusSMAX.py#L116'>Packs/MicroFocusSMAX/Integrations/MicroFocusSMAX/MicroFocusSMAX.py:116:73:</a> UP034 [*] Avoid extraneous parentheses
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/MicroFocusSMAX/Integrations/MicroFocusSMAX/MicroFocusSMAX.py#L119'>Packs/MicroFocusSMAX/Integrations/MicroFocusSMAX/MicroFocusSMAX.py:119:73:</a> UP034 [*] Avoid extraneous parentheses
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py#L3067'>Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py:3067:44:</a> UP034 [*] Avoid extraneous parentheses
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py#L3180'>Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py:3180:47:</a> UP034 [*] Avoid extraneous parentheses
</pre>

</p>
</details>
<details><summary>mlflow (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/f29e6145e8925a92916aaa5420132bcf60a9e27c/mlflow/models/evaluation/base.py#L499'>mlflow/models/evaluation/base.py:499:59:</a> UP034 [*] Avoid extraneous parentheses
</pre>

</p>
</details>
<details><summary>rotki (+11, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/accounting/export/csv.py#L248'>rotkehlchen/accounting/export/csv.py:248:84:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/accounting/export/csv.py#L248'>rotkehlchen/accounting/export/csv.py:248:89:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/accounting/export/csv.py#L249'>rotkehlchen/accounting/export/csv.py:249:75:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/accounting/export/csv.py#L249'>rotkehlchen/accounting/export/csv.py:249:80:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/chain/ethereum/modules/nft/nfts.py#L239'>rotkehlchen/chain/ethereum/modules/nft/nfts.py:239:33:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/chain/ethereum/modules/nft/nfts.py#L240'>rotkehlchen/chain/ethereum/modules/nft/nfts.py:240:33:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/db/addressbook.py#L155'>rotkehlchen/db/addressbook.py:155:92:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/db/dbhandler.py#L957'>rotkehlchen/db/dbhandler.py:957:44:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/db/ens.py#L52'>rotkehlchen/db/ens.py:52:102:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/fval.py#L159'>rotkehlchen/fval.py:159:27:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/rotki/rotki/blob/f59390915cb8d03c10a0490f6fff99789c89ddac/rotkehlchen/globaldb/upgrades/v2_v3.py#L361'>rotkehlchen/globaldb/upgrades/v2_v3.py:361:82:</a> E226 Missing whitespace around arithmetic operator
</pre>

</p>
</details>
<details><summary>sphinx (+15, -0)</summary>
<p>

<pre>
+ 
+     = help: Remove trailing comma
+     |
+     |                                                                                   ^^ E226
+     |                                                                    ^ COM819
+ 431 |             if not entries:
+ 432 |                 return
+ 433 |             self.body.append(f'\n{self.escape(self.node_names[name], )}\n\n')
+ 434 |             self.add_menu_entries(entries)
+ 435 |             for subentry in entries:
+ 752 |     """Return an RFC 3339 formatted string representing the given timestamp."""
+ 753 |     seconds, fraction = divmod(timestamp, 10**6)
+ 754 |     return time.strftime("%Y-%m-%d %H:%M:%S", time.gmtime(seconds)) + f'.{fraction//1_000}'
+ <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/environment/__init__.py#L754'>sphinx/environment/__init__.py:754:83:</a> E226 Missing whitespace around arithmetic operator
+ <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/writers/texinfo.py#L433'>sphinx/writers/texinfo.py:433:68:</a> COM819 [*] Trailing comma prohibited
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| E226 | 12 | 12 | 0 |
| UP034 | 5 | 5 | 0 |
| COM819 | 1 | 1 | 0 |



---

_Marked ready for review by @dhruvmanila on 2023-09-28 06:09_

---

_Label `core` added by @dhruvmanila on 2023-09-28 06:14_

---

_Label `python312` added by @dhruvmanila on 2023-09-28 06:14_

---

_Comment by @dhruvmanila on 2023-09-29 02:45_

Ugh, I messed up a rebase and removed the logical line rules PR commit 😓 

I've manually merged the branch locally and pushed it but the details can be found in the original PR: #7690

---

_Comment by @dhruvmanila on 2023-09-29 02:48_

The ecosystem checks for the linter looks good as we now detect violations inside f-strings as well. We'll avoid the ones for which it's hard to distinguish between usages like `:` for format spec vs dictionary.

The formatter ecosystem checks are unchanged except for one file which we're able to parse now (`test_fstring.py` in CPython repository) as it contained PEP 701 syntax.

---

_Merged by @dhruvmanila on 2023-09-29 02:55_

---

_Closed by @dhruvmanila on 2023-09-29 02:55_

---

_Branch deleted on 2023-09-29 02:55_

---

_Comment by @hugovk on 2023-09-29 05:53_

Thank you for all your work on this!

---

_Comment by @konstin on 2023-09-29 08:03_

:tada: 

---

_Comment by @MichaReiser on 2023-09-29 08:26_

Congrats on landing this huge change 🎸 🎸 🎸 

---

_Comment by @dhruvmanila on 2023-10-02 18:00_

Thank you all for your comments! 😄

---
