```yaml
number: 10318
title: "Implement pylint W1518 method cache max size astral-sh#970"
type: pull_request
state: closed
author: Glyphack
labels: []
assignees: []
base: main
head: pytlint-method-cache-max-size-none-W1518
created_at: 2024-03-09T22:24:56Z
updated_at: 2024-03-14T16:54:21Z
url: https://github.com/astral-sh/ruff/pull/10318
synced_at: 2026-01-10T22:47:01Z
```

# Implement pylint W1518 method cache max size astral-sh#970

---

_Pull request opened by @Glyphack on 2024-03-09 22:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement rule https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/method-cache-max-size-none.html

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-09 22:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7bba05d782a64da0b5cce68ec92ac6f5c7b49221/airflow/providers/google/common/hooks/base_google.py#L319'>airflow/providers/google/common/hooks/base_google.py:319:5:</a> PLW1518 Method decorated with `@lru_cache(maxsize=None)` or `@cache` is never garbage collected.
+ <a href='https://github.com/apache/airflow/blob/7bba05d782a64da0b5cce68ec92ac6f5c7b49221/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L577'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:577:5:</a> PLW1518 Method decorated with `@lru_cache(maxsize=None)` or `@cache` is never garbage collected.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/db0efecaa9bb520ef881f9b9038e96b9ab245f9d/samcli/lib/translate/sam_template_validator.py#L98'>samcli/lib/translate/sam_template_validator.py:98:5:</a> PLW1518 Method decorated with `@lru_cache(maxsize=None)` or `@cache` is never garbage collected.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/61ee7192e00a1671c4a17cb1c0f932c8ffa5d107/indico/cli/watchfiles.py#L94'>indico/cli/watchfiles.py:94:5:</a> PLW1518 Method decorated with `@lru_cache(maxsize=None)` or `@cache` is never garbage collected.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1518 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @Glyphack on 2024-03-10 07:10_

---

_Renamed from "Implement pylint W1518 singledispatch-method astral-sh#970" to "Implement pylint W1518 method cache max size astral-sh#970" by @Glyphack on 2024-03-11 21:33_

---

_Marked ready for review by @Glyphack on 2024-03-13 22:34_

---

_Comment by @charliermarsh on 2024-03-14 15:38_

Unfortunately this may be the same as https://docs.astral.sh/ruff/rules/cached-instance-method/ -- what do you think? Any differences?

---

_Comment by @Glyphack on 2024-03-14 15:48_

Oops did not cross my mind to check other linter rules. Closing this one.

---

_Closed by @Glyphack on 2024-03-14 15:48_

---

_Comment by @charliermarsh on 2024-03-14 15:52_

I'm sorry for the duplicated work :(

What I typically do is: take the snippet, and paste it into the playground with `ALL` enabled. It's a really quick way to see if there are clear conflicts: https://play.ruff.rs/e3ce2844-34b1-43eb-8883-ea803a531861

---

_Branch deleted on 2024-03-14 16:54_

---
