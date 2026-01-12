```yaml
number: 14502
title: "doc(B024): #14455 add annotated but unassgined class variables"
type: pull_request
state: merged
author: cmp0xff
labels:
  - documentation
assignees: []
merged: true
base: main
head: hotfix/cmp0xff/#14455-b024-class-variable
created_at: 2024-11-20T20:50:38Z
updated_at: 2024-11-21T15:12:43Z
url: https://github.com/astral-sh/ruff/pull/14502
synced_at: 2026-01-12T15:55:48Z
```

# doc(B024): #14455 add annotated but unassgined class variables

---

_@cmp0xff_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

# Summary

Closes #14455, migrated from https://github.com/astral-sh/docs/pull/106.

# Test Plan

Updating the documentation probably does not need a test.


---

_Renamed from "doc(B024): #14455 add annotated but unassgined class variables astral-sh/docs/#106" to "doc(B024): #14455 add annotated but unassgined class variables https://github.com/astral-sh/docs/pull/106" by @cmp0xff on 2024-11-20 20:51_

---

_Renamed from "doc(B024): #14455 add annotated but unassgined class variables https://github.com/astral-sh/docs/pull/106" to "doc(B024): #14455 add annotated but unassgined class variables" by @cmp0xff on 2024-11-20 20:51_

---

_Comment by @github-actions[bot] on 2024-11-20 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods or properties
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/440c224af5592f9007eef43d1dbe9025aa34e177/airflow/secrets/base_secrets.py#L26'>airflow/secrets/base_secrets.py:26:7:</a> B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods
+ <a href='https://github.com/apache/airflow/blob/440c224af5592f9007eef43d1dbe9025aa34e177/airflow/secrets/base_secrets.py#L26'>airflow/secrets/base_secrets.py:26:7:</a> B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods or properties
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B024 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods or properties
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/440c224af5592f9007eef43d1dbe9025aa34e177/airflow/secrets/base_secrets.py#L26'>airflow/secrets/base_secrets.py:26:7:</a> B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods
+ <a href='https://github.com/apache/airflow/blob/440c224af5592f9007eef43d1dbe9025aa34e177/airflow/secrets/base_secrets.py#L26'>airflow/secrets/base_secrets.py:26:7:</a> B024 `BaseSecretsBackend` is an abstract base class, but it has no abstract methods or properties
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B024 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2024-11-20 21:15_

---

_Label `documentation` added by @MichaReiser on 2024-11-20 21:15_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:15 on 2024-11-21 02:07_

How about:

```diff
- Checks for abstract classes without abstract methods or abstract class variables.
+ Checks for abstract classes without abstract methods or properties.
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:64 on 2024-11-21 02:08_

Similarly we can make this a little more concise:

```diff
- format!("`{name}` is an abstract base class, but it has neither abstract methods nor abstract class variables")
+ format!("`{name}` is an abstract base class, but it has no abstract methods or properties")
```

---

_Review comment by @dylwil3 on `scripts/generate_mkdocs.py`:114 on 2024-11-21 02:10_

Wow, nice grep! Remember to update this again after modifying the docstring.

---

_@dylwil3 requested changes on 2024-11-21 02:10_

This looks great, thank you! Minor nit and then we'll merge it in.

---

_@cmp0xff reviewed on 2024-11-21 11:50_

---

_Review comment by @cmp0xff on `scripts/generate_mkdocs.py`:114 on 2024-11-21 11:50_

https://github.com/cmp0xff/astral-sh-ruff/commit/680b19969fd234a6cb706bb4e68bdeff103a51b1

---

_@cmp0xff reviewed on 2024-11-21 11:50_

---

_Review comment by @cmp0xff on `crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:64 on 2024-11-21 11:50_

https://github.com/cmp0xff/astral-sh-ruff/commit/680b19969fd234a6cb706bb4e68bdeff103a51b1

---

_@cmp0xff reviewed on 2024-11-21 11:50_

---

_Review comment by @cmp0xff on `crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:15 on 2024-11-21 11:50_

https://github.com/cmp0xff/astral-sh-ruff/commit/680b19969fd234a6cb706bb4e68bdeff103a51b1

---

_Review requested from @dylwil3 by @MichaReiser on 2024-11-21 12:23_

---

_@dylwil3 approved on 2024-11-21 15:03_

LGTM! Thank you so much for the contribution! (I'm gonna merge this right after the 0.8 release so I don't mess up the changelog 😄 )

---

_Comment by @MichaReiser on 2024-11-21 15:06_

@dylwil3 it's fine to merge documentation changes because we rarely mention them in the changelog :sweat_smile: 

---

_Merged by @dylwil3 on 2024-11-21 15:08_

---

_Closed by @dylwil3 on 2024-11-21 15:08_

---

_Branch deleted on 2024-11-21 15:12_

---
