```yaml
number: 3451
title: "Avoid removing un-aliased exceptions in `OSError`-aliased handlers"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up024
created_at: 2023-03-11T20:12:57Z
updated_at: 2023-03-11T20:24:13Z
url: https://github.com/astral-sh/ruff/pull/3451
synced_at: 2026-01-12T04:39:44Z
```

# Avoid removing un-aliased exceptions in `OSError`-aliased handlers

---

_Pull request opened by @charliermarsh on 2023-03-11 20:12_

Closes #3422.

---

_Comment by @github-actions[bot] on 2023-03-11 20:23_

ℹ️ ecosystem check **detected changes**. (+0, -10, 0 error(s))

<details><summary>zulip</summary>
<p>

```diff
- zproject/dev_settings.py:168:29: PLW1508 Invalid type for environment variable default, must be `str` or `none`
```

</p>
</details>
<details><summary>bokeh</summary>
<p>

_No changes detected_.

</p>
</details>
<details><summary>scikit-build</summary>
<p>

_No changes detected_.

</p>
</details>
<details><summary>airflow</summary>
<p>

```diff
- docs/exts/sphinx_script_update.py:53:16: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- setup.py:741:12: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- setup.py:892:12: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- setup.py:896:8: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- tests/system/providers/amazon/aws/utils/__init__.py:230:25: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- tests/system/providers/microsoft/azure/example_azure_service_bus.py:43:25: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- tests/system/providers/microsoft/azure/example_azure_synapse.py:26:25: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- tests/system/providers/microsoft/azure/example_azure_synapse.py:30:20: PLW1508 Invalid type for environment variable default, must be `str` or `none`
- tests/system/providers/microsoft/azure/example_azure_synapse.py:31:42: PLW1508 Invalid type for environment variable default, must be `str` or `none`
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Merged by @charliermarsh on 2023-03-11 20:24_

---

_Closed by @charliermarsh on 2023-03-11 20:24_

---

_Branch deleted on 2023-03-11 20:24_

---
