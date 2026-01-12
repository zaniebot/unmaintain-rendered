```yaml
number: 8648
title: Bump pyproject-toml from 0.8.0 to 0.8.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/pyproject-toml-0.8.1
created_at: 2023-11-13T08:09:01Z
updated_at: 2023-11-13T15:04:13Z
url: https://github.com/astral-sh/ruff/pull/8648
synced_at: 2026-01-12T15:55:26Z
```

# Bump pyproject-toml from 0.8.0 to 0.8.1

---

_@dependabot_

Bumps [pyproject-toml](https://github.com/PyO3/pyproject-toml-rs) from 0.8.0 to 0.8.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/PyO3/pyproject-toml-rs/blob/main/Changelog.md">pyproject-toml's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/ee007c243a352accf34d1797950979e91a53f9dc"><code>ee007c2</code></a> Bump version to 0.8.1</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/f64fad67507a959af07a89db59df7451992b082b"><code>f64fad6</code></a> Merge pull request <a href="https://redirect.github.com/PyO3/pyproject-toml-rs/issues/16">#16</a> from PyO3/alex-patch-1</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/bc7df6cc0e7fde14d943c56f80f06310d430acf9"><code>bc7df6c</code></a> Update toml dependency to 0.8</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/9543160eb6027cfd75c9615e1954147a184848e6"><code>9543160</code></a> Update version to 0.8 in README.md</li>
<li>See full diff in <a href="https://github.com/PyO3/pyproject-toml-rs/compare/v0.8.0...v0.8.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyproject-toml&package-manager=cargo&previous-version=0.8.0&new-version=0.8.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot merge` will merge this PR after your CI passes on it
- `@dependabot squash and merge` will squash and merge this PR after your CI passes on it
- `@dependabot cancel merge` will cancel a previously requested merge and block automerging
- `@dependabot reopen` will reopen this PR if it is closed
- `@dependabot close` will close this PR and stop Dependabot recreating it. You can achieve the same result by closing it manually
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)


</details>

---

_Label `internal` added by @dependabot[bot] on 2023-11-13 08:09_

---

_Comment by @github-actions[bot] on 2023-11-13 08:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -95 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -95 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1006'>pandas/tests/io/json/test_ujson.py:1006:9:</a> PLR6301 Method `test_decode_with_trailing_whitespaces` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1009'>pandas/tests/io/json/test_ujson.py:1009:9:</a> PLR6301 Method `test_decode_with_trailing_non_whitespaces` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1014'>pandas/tests/io/json/test_ujson.py:1014:9:</a> PLR6301 Method `test_decode_array_with_big_int` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1036'>pandas/tests/io/json/test_ujson.py:1036:9:</a> PLR6301 Method `test_decode_floating_point` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1042'>pandas/tests/io/json/test_ujson.py:1042:9:</a> PLR6301 Method `test_encode_big_set` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1051'>pandas/tests/io/json/test_ujson.py:1051:9:</a> PLR6301 Method `test_encode_empty_set` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1054'>pandas/tests/io/json/test_ujson.py:1054:9:</a> PLR6301 Method `test_encode_set` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1076'>pandas/tests/io/json/test_ujson.py:1076:9:</a> PLR6301 Method `test_encode_timedelta_iso` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L1083'>pandas/tests/io/json/test_ujson.py:1083:9:</a> PLR6301 Method `test_encode_periodindex` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L113'>pandas/tests/io/json/test_ujson.py:113:9:</a> PLR6301 Method `test_encode_string_conversion` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L141'>pandas/tests/io/json/test_ujson.py:141:9:</a> PLR6301 Method `test_double_long_numbers` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L148'>pandas/tests/io/json/test_ujson.py:148:9:</a> PLR6301 Method `test_encode_non_c_locale` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L159'>pandas/tests/io/json/test_ujson.py:159:9:</a> PLR6301 Method `test_decimal_decode_test_precise` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L165'>pandas/tests/io/json/test_ujson.py:165:9:</a> PLR6301 Method `test_encode_double_tiny_exponential` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L176'>pandas/tests/io/json/test_ujson.py:176:9:</a> PLR6301 Method `test_encode_dict_with_unicode_keys` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L183'>pandas/tests/io/json/test_ujson.py:183:9:</a> PLR6301 Method `test_encode_double_conversion` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L188'>pandas/tests/io/json/test_ujson.py:188:9:</a> PLR6301 Method `test_encode_with_decimal` could be a function, class method, or static method
... 75 additional changes omitted for rule PLR6301
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L55'>pandas/tests/io/json/test_ujson.py:55:1:</a> PLR0904 Too many public methods (56 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L735'>pandas/tests/io/json/test_ujson.py:735:35:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L901'>pandas/tests/io/json/test_ujson.py:901:22:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/8df36a2f9f787af7fcc08326c6d7b3456b3ff0b7/pandas/tests/io/json/test_ujson.py#L905'>pandas/tests/io/json/test_ujson.py:905:24:</a> PLR6201 Use a `set` literal when testing for membership
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 91 | 0 | 91 | 0 | 0 |
| PLR6201 | 3 | 0 | 3 | 0 | 0 |
| PLR0904 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2023-11-13 14:53_

---

_Closed by @charliermarsh on 2023-11-13 14:53_

---

_Branch deleted on 2023-11-13 14:53_

---
