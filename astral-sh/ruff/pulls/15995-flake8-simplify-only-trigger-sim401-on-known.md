```yaml
number: 15995
title: "[flake8-simplify] Only trigger SIM401 on known dictionaries (SIM401)"
type: pull_request
state: merged
author: junhsonjb
labels:
  - rule
assignees: []
merged: true
base: main
head: jjb-sim401-rule
created_at: 2025-02-06T14:38:47Z
updated_at: 2025-02-07T08:28:52Z
url: https://github.com/astral-sh/ruff/pull/15995
synced_at: 2026-01-12T15:55:53Z
```

# [flake8-simplify] Only trigger SIM401 on known dictionaries (SIM401)

---

_@junhsonjb_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This change resolves #15814 to ensure that `SIM401` is only triggered on known dictionary types. Before, the rule was getting triggered even on types that _resemble_ a dictionary but are not actually a dictionary.

I did this using the `is_known_to_be_of_type_dict(...)` functionality. The logic for this function was duplicated in a few spots, so I moved the code to a central location, removed redundant definitions, and updated existing calls to use the single definition of the function!

## Test Plan

<!-- How was it tested? -->

Since this PR only modifies an existing rule, I made changes to the existing test instead of adding new ones. I made sure that `SIM401` is triggered on types that are clearly dictionaries and that it's not triggered on a simple custom dictionary-like type  (using a modified version of [the code in the issue](#15814))

The additional changes to de-duplicate `is_known_to_be_of_type_dict` don't break any existing tests -- I think this should be fine since the logic remains the same (please let me know if you think otherwise, I'm excited to get feedback and work towards a good fix 🙂).


---

_Comment by @github-actions[bot] on 2025-02-06 14:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/891c67f210ab7c877d1f00ea6ea3d3cdbb0e96ef/providers/src/airflow/providers/databricks/operators/databricks.py#L87'>providers/src/airflow/providers/databricks/operators/databricks.py:87:29:</a> SIM401 Use `error = run_output.get("error", run_state.state_message)` instead of an `if` block
- <a href='https://github.com/apache/airflow/blob/891c67f210ab7c877d1f00ea6ea3d3cdbb0e96ef/providers/src/airflow/providers/databricks/triggers/databricks.py#L107'>providers/src/airflow/providers/databricks/triggers/databricks.py:107:29:</a> SIM401 Use `error = run_output.get("error", run_state.state_message)` instead of an `if` block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/8a0d96afd3c825930e6677961e2e60065be4f663/toolbox.py#L437'>toolbox.py:437:9:</a> SIM401 Use `current = chatbot._cookies.get("files_to_promote", [])` instead of an `if` block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zerver/data_import/mattermost.py#L553'>zerver/data_import/mattermost.py:553:9:</a> SIM401 Use `post_team = post.get("team", team_name)` instead of an `if` block
- <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zproject/computed_settings.py#L1186'>zproject/computed_settings.py:1186:5:</a> SIM401 Use `path = idp_dict.get("x509cert_path", f"/etc/zulip/saml/idps/{idp_name}.crt")` instead of an `if` block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM401 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/891c67f210ab7c877d1f00ea6ea3d3cdbb0e96ef/providers/src/airflow/providers/databricks/operators/databricks.py#L87'>providers/src/airflow/providers/databricks/operators/databricks.py:87:29:</a> SIM401 Use `error = run_output.get("error", run_state.state_message)` instead of an `if` block
- <a href='https://github.com/apache/airflow/blob/891c67f210ab7c877d1f00ea6ea3d3cdbb0e96ef/providers/src/airflow/providers/databricks/triggers/databricks.py#L107'>providers/src/airflow/providers/databricks/triggers/databricks.py:107:29:</a> SIM401 Use `error = run_output.get("error", run_state.state_message)` instead of an `if` block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/8a0d96afd3c825930e6677961e2e60065be4f663/toolbox.py#L437'>toolbox.py:437:9:</a> SIM401 Use `current = chatbot._cookies.get("files_to_promote", [])` instead of an `if` block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zerver/data_import/mattermost.py#L553'>zerver/data_import/mattermost.py:553:9:</a> SIM401 Use `post_team = post.get("team", team_name)` instead of an `if` block
- <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zproject/computed_settings.py#L1186'>zproject/computed_settings.py:1186:5:</a> SIM401 Use `path = idp_dict.get("x509cert_path", f"/etc/zulip/saml/idps/{idp_name}.crt")` instead of an `if` block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM401 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-02-07 08:15_

---

_Comment by @MichaReiser on 2025-02-07 08:16_

Going through the ecosystem results. It's somewhat unfortunate that most of those were true-positives but I still think this is the right trade-off and improvements to the type inference will help to detect the false-negatives in the future.

---

_@MichaReiser approved on 2025-02-07 08:22_

Thanks

---

_Merged by @MichaReiser on 2025-02-07 08:25_

---

_Closed by @MichaReiser on 2025-02-07 08:25_

---
