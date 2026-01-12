```yaml
number: 9075
title: Add fix for comment-related whitespace rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: charlie/E26
created_at: 2023-12-09T19:52:26Z
updated_at: 2023-12-09T20:18:08Z
url: https://github.com/astral-sh/ruff/pull/9075
synced_at: 2026-01-12T15:55:27Z
```

# Add fix for comment-related whitespace rules

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/9067.

Closes https://github.com/astral-sh/ruff/issues/9068.

Closes https://github.com/astral-sh/ruff/issues/8119.

---

_Label `autofix` added by @charliermarsh on 2023-12-09 19:52_

---

_Label `preview` added by @charliermarsh on 2023-12-09 19:52_

---

_Comment by @github-actions[bot] on 2023-12-09 20:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +20218 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/opus.py#L76'>disnake/opus.py:76:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/opus.py#L76'>disnake/opus.py:76:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L167'>disnake/types/interactions.py:167:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L167'>disnake/types/interactions.py:167:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L179'>disnake/types/interactions.py:179:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L179'>disnake/types/interactions.py:179:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L217'>disnake/types/interactions.py:217:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L217'>disnake/types/interactions.py:217:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L247'>disnake/types/interactions.py:247:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/types/interactions.py#L247'>disnake/types/interactions.py:247:1:</a> E266 [*] Too many leading `#` before block comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L297'>docs/source/conf.py:297:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L297'>docs/source/conf.py:297:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L299'>docs/source/conf.py:299:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L299'>docs/source/conf.py:299:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L301'>docs/source/conf.py:301:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L301'>docs/source/conf.py:301:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L303'>docs/source/conf.py:303:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/docs/source/conf.py#L303'>docs/source/conf.py:303:5:</a> E265 [*] Block comment should start with `# `
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L100'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:100:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L100'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:100:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L58'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:58:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L58'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:58:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L93'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:93:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L93'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:93:1:</a> E266 [*] Too many leading `#` before block comment
... 1 additional changes omitted for rule E266
- <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/providers/google/cloud/hooks/bigquery.py#L1072'>airflow/providers/google/cloud/hooks/bigquery.py:1072:30:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/providers/google/cloud/hooks/bigquery.py#L1072'>airflow/providers/google/cloud/hooks/bigquery.py:1072:30:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/providers/google/cloud/hooks/mlengine.py#L595'>airflow/providers/google/cloud/hooks/mlengine.py:595:39:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/apache/airflow/blob/6e419001d3dd512cb01bc42bdfa8738c289e2754/airflow/providers/google/cloud/hooks/mlengine.py#L595'>airflow/providers/google/cloud/hooks/mlengine.py:595:39:</a> E262 [*] Inline comment should start with `# `
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +19098 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/box_annotation.py#L17'>examples/basic/annotations/box_annotation.py:17:1:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/box_annotation.py#L17'>examples/basic/annotations/box_annotation.py:17:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/anscombe.py#L52'>examples/basic/layouts/anscombe.py:52:1:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/anscombe.py#L52'>examples/basic/layouts/anscombe.py:52:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/scatters/elements.py#L32'>examples/basic/scatters/elements.py:32:75:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/scatters/elements.py#L32'>examples/basic/scatters/elements.py:32:75:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L44'>examples/integration/widgets/layout_multiple_widgets.py:44:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L44'>examples/integration/widgets/layout_multiple_widgets.py:44:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L45'>examples/integration/widgets/layout_multiple_widgets.py:45:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L45'>examples/integration/widgets/layout_multiple_widgets.py:45:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L46'>examples/integration/widgets/layout_multiple_widgets.py:46:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L46'>examples/integration/widgets/layout_multiple_widgets.py:46:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L47'>examples/integration/widgets/layout_multiple_widgets.py:47:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L47'>examples/integration/widgets/layout_multiple_widgets.py:47:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L49'>examples/integration/widgets/layout_multiple_widgets.py:49:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L49'>examples/integration/widgets/layout_multiple_widgets.py:49:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L50'>examples/integration/widgets/layout_multiple_widgets.py:50:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/widgets/layout_multiple_widgets.py#L50'>examples/integration/widgets/layout_multiple_widgets.py:50:5:</a> E265 [*] Block comment should start with `# `
... 18979 additional changes omitted for rule E265
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/checkout_form.py#L21'>examples/plotting/checkout_form.py:21:62:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/checkout_form.py#L21'>examples/plotting/checkout_form.py:21:62:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/checkout_form.py#L22'>examples/plotting/checkout_form.py:22:55:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/checkout_form.py#L22'>examples/plotting/checkout_form.py:22:55:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/button_server.py#L1'>examples/reference/models/button_server.py:1:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/button_server.py#L1'>examples/reference/models/button_server.py:1:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L1'>examples/reference/models/checkbox_button_server.py:1:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L1'>examples/reference/models/checkbox_button_server.py:1:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L24'>examples/reference/models/checkbox_button_server.py:24:44:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L24'>examples/reference/models/checkbox_button_server.py:24:44:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L26'>examples/reference/models/checkbox_button_server.py:26:5:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L26'>examples/reference/models/checkbox_button_server.py:26:5:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L33'>examples/reference/models/checkbox_button_server.py:33:5:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_button_server.py#L33'>examples/reference/models/checkbox_button_server.py:33:5:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L1'>examples/reference/models/checkbox_server.py:1:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L1'>examples/reference/models/checkbox_server.py:1:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L21'>examples/reference/models/checkbox_server.py:21:37:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L21'>examples/reference/models/checkbox_server.py:21:37:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L23'>examples/reference/models/checkbox_server.py:23:5:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L23'>examples/reference/models/checkbox_server.py:23:5:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L30'>examples/reference/models/checkbox_server.py:30:5:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/checkbox_server.py#L30'>examples/reference/models/checkbox_server.py:30:5:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/data_table_server.py#L1'>examples/reference/models/data_table_server.py:1:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/data_table_server.py#L1'>examples/reference/models/data_table_server.py:1:1:</a> E266 [*] Too many leading `#` before block comment
... 55 additional changes omitted for rule E266
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/dropdown_menu_server.py#L22'>examples/reference/models/dropdown_menu_server.py:22:36:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/dropdown_menu_server.py#L22'>examples/reference/models/dropdown_menu_server.py:22:36:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/multi_select_server.py#L13'>examples/reference/models/multi_select_server.py:13:46:</a> E262 Inline comment should start with `# `
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/multi_select_server.py#L13'>examples/reference/models/multi_select_server.py:13:46:</a> E262 [*] Inline comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/reference/models/multi_select_server.py#L26'>examples/reference/models/multi_select_server.py:26:39:</a> E262 Inline comment should start with `# `
... 19051 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+0 -0 violations, +110 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/logging_extra.py#L184'>common/logging_extra.py:184:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/logging_extra.py#L184'>common/logging_extra.py:184:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/logging_extra.py#L185'>common/logging_extra.py:185:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/logging_extra.py#L185'>common/logging_extra.py:185:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/transformations/camera.py#L5'>common/transformations/camera.py:5:1:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/transformations/camera.py#L5'>common/transformations/camera.py:5:1:</a> E266 [*] Too many leading `#` before block comment
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/transformations/tests/test_orientation.py#L63'>common/transformations/tests/test_orientation.py:63:7:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/common/transformations/tests/test_orientation.py#L63'>common/transformations/tests/test_orientation.py:63:7:</a> E265 [*] Block comment should start with `# `
... 87 additional changes omitted for rule E265
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/ford/carcontroller.py#L47'>selfdrive/car/ford/carcontroller.py:47:5:</a> E266 Too many leading `#` before block comment
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/ford/carcontroller.py#L47'>selfdrive/car/ford/carcontroller.py:47:5:</a> E266 [*] Too many leading `#` before block comment
... 100 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L112'>blinksync/forms.py:112:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L112'>blinksync/forms.py:112:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L16'>blinksync/forms.py:16:9:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L16'>blinksync/forms.py:16:9:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L55'>blinksync/forms.py:55:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L55'>blinksync/forms.py:55:5:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L81'>blinksync/forms.py:81:5:</a> E265 Block comment should start with `# `
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L81'>blinksync/forms.py:81:5:</a> E265 [*] Block comment should start with `# `
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E265 | 19106 | 0 | 0 | 19106 | 0 |
| E266 | 1068 | 0 | 0 | 1068 | 0 |
| E262 | 45 | 0 | 1 | 44 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-12-09 20:18_

---

_Closed by @charliermarsh on 2023-12-09 20:18_

---

_Branch deleted on 2023-12-09 20:18_

---
