```yaml
number: 19900
title: "[`pyflakes`] Add secondary annotation showing previous definition (`F811`)"
type: pull_request
state: merged
author: ntBre
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: brent/redefined-while-unused
created_at: 2025-08-13T16:27:17Z
updated_at: 2025-08-14T17:23:45Z
url: https://github.com/astral-sh/ruff/pull/19900
synced_at: 2026-01-10T17:52:17Z
```

# [`pyflakes`] Add secondary annotation showing previous definition (`F811`)

---

_Pull request opened by @ntBre on 2025-08-13 16:27_

## Summary

This is a second attempt at a first use of a new diagnostic feature after #19886. I'll blame rustc for this one because it also has a similar diagnostic:

<img width="735" height="335" alt="image" src="https://github.com/user-attachments/assets/572fe1c3-1742-4ce4-b575-1d9196ff0932" />

We end up with a very similar diagnostic:

<img width="764" height="401" alt="image" src="https://github.com/user-attachments/assets/01eaf0c7-2567-467b-a5d8-a27206b2c74c" />

## Test Plan

New snapshots and manual tests above


---

_Comment by @github-actions[bot] on 2025-08-13 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+38 -38 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/commands/deploy/test_deploy_context.py#L11'>tests/unit/commands/deploy/test_deploy_context.py:11:47:</a> F811 [*] Redefinition of unused `DeployFailedError` from line 9
+ <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/commands/deploy/test_deploy_context.py#L11'>tests/unit/commands/deploy/test_deploy_context.py:11:47:</a> F811 [*] Redefinition of unused `DeployFailedError` from line 9: `DeployFailedError` redefined here
- <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 [*] Redefinition of unused `parameterized` from line 5
+ <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 [*] Redefinition of unused `parameterized` from line 5: `parameterized` redefined here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+35 -35 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/Rag_Interface.py#L8'>crazy_functions/Rag_Interface.py:8:41:</a> F811 [*] Redefinition of unused `validate_path_safety` from line 4
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/Rag_Interface.py#L8'>crazy_functions/Rag_Interface.py:8:41:</a> F811 [*] Redefinition of unused `validate_path_safety` from line 4: `validate_path_safety` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/agent_fns/python_comment_agent.py#L268'>crazy_functions/agent_fns/python_comment_agent.py:268:9:</a> F811 Redefinition of unused `dedent` from line 5
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/agent_fns/python_comment_agent.py#L268'>crazy_functions/agent_fns/python_comment_agent.py:268:9:</a> F811 Redefinition of unused `dedent` from line 5: `dedent` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/AI_review_doc.py#L10'>crazy_functions/doc_fns/AI_review_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/AI_review_doc.py#L10'>crazy_functions/doc_fns/AI_review_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9: `Inches` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/batch_file_query_doc.py#L10'>crazy_functions/doc_fns/batch_file_query_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/batch_file_query_doc.py#L10'>crazy_functions/doc_fns/batch_file_query_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9: `Inches` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:48:</a> F811 [*] Redefinition of unused `gen_time_str` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:48:</a> F811 [*] Redefinition of unused `gen_time_str` from line 3: `gen_time_str` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:62:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:62:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 3: `trimmed_format_exc` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L5'>crazy_functions/gen_fns/gen_fns_shared.py:5:51:</a> F811 [*] Redefinition of unused `get_log_folder` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L5'>crazy_functions/gen_fns/gen_fns_shared.py:5:51:</a> F811 [*] Redefinition of unused `get_log_folder` from line 3: `get_log_folder` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/latex_fns/latex_actions.py#L392'>crazy_functions/latex_fns/latex_actions.py:392:16:</a> F811 [*] Redefinition of unused `os` from line 348
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/latex_fns/latex_actions.py#L392'>crazy_functions/latex_fns/latex_actions.py:392:16:</a> F811 [*] Redefinition of unused `os` from line 348: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf.py#L4'>crazy_functions/pdf_fns/parse_pdf.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf.py#L4'>crazy_functions/pdf_fns/parse_pdf.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_grobid.py#L4'>crazy_functions/pdf_fns/parse_pdf_grobid.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_grobid.py#L4'>crazy_functions/pdf_fns/parse_pdf_grobid.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_legacy.py#L3'>crazy_functions/pdf_fns/parse_pdf_legacy.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_legacy.py#L3'>crazy_functions/pdf_fns/parse_pdf_legacy.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L3'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:3:21:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L3'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:3:21:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/report_gen_html.py#L52'>crazy_functions/pdf_fns/report_gen_html.py:52:29:</a> F811 [*] Redefinition of unused `get_log_folder` from line 1
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/report_gen_html.py#L52'>crazy_functions/pdf_fns/report_gen_html.py:52:29:</a> F811 [*] Redefinition of unused `get_log_folder` from line 1: `get_log_folder` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/vector_fns/vector_database.py#L4'>crazy_functions/vector_fns/vector_database.py:4:8:</a> F811 [*] Redefinition of unused `os` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/vector_fns/vector_database.py#L4'>crazy_functions/vector_fns/vector_database.py:4:8:</a> F811 [*] Redefinition of unused `os` from line 3: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量总结PDF文档.py#L5'>crazy_functions/批量总结PDF文档.py:5:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量总结PDF文档.py#L5'>crazy_functions/批量总结PDF文档.py:5:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L3'>crazy_functions/批量翻译PDF文档_NOUGAT.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L3'>crazy_functions/批量翻译PDF文档_NOUGAT.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L99'>crazy_functions/批量翻译PDF文档_NOUGAT.py:99:12:</a> F811 [*] Redefinition of unused `copy` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L99'>crazy_functions/批量翻译PDF文档_NOUGAT.py:99:12:</a> F811 [*] Redefinition of unused `copy` from line 9: `copy` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm.py#L23'>request_llms/bridge_chatglm.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm.py#L23'>request_llms/bridge_chatglm.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm3.py#L23'>request_llms/bridge_chatglm3.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm3.py#L23'>request_llms/bridge_chatglm3.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:21:</a> F811 [*] Redefinition of unused `get_conf` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:21:</a> F811 [*] Redefinition of unused `get_conf` from line 18: `get_conf` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:31:</a> F811 [*] Redefinition of unused `update_ui` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:31:</a> F811 [*] Redefinition of unused `update_ui` from line 18: `update_ui` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:42:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:42:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 18: `trimmed_format_exc` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_internlm.py#L53'>request_llms/bridge_internlm.py:53:56:</a> F811 [*] Redefinition of unused `AutoTokenizer` from line 4
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_internlm.py#L53'>request_llms/bridge_internlm.py:53:56:</a> F811 [*] Redefinition of unused `AutoTokenizer` from line 4: `AutoTokenizer` redefined here
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/3a44401d580b378395aa066fddec76f494a80af8/tests/test_ls.py#L6'>tests/test_ls.py:6:13:</a> F811 Redefinition of unused `test_account_jwt` from line 3
+ <a href='https://github.com/latchbio/latch/blob/3a44401d580b378395aa066fddec76f494a80af8/tests/test_ls.py#L6'>tests/test_ls.py:6:13:</a> F811 Redefinition of unused `test_account_jwt` from line 3: `test_account_jwt` redefined here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 76 | 38 | 38 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+38 -38 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/commands/deploy/test_deploy_context.py#L11'>tests/unit/commands/deploy/test_deploy_context.py:11:47:</a> F811 [*] Redefinition of unused `DeployFailedError` from line 9
+ <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/commands/deploy/test_deploy_context.py#L11'>tests/unit/commands/deploy/test_deploy_context.py:11:47:</a> F811 [*] Redefinition of unused `DeployFailedError` from line 9: `DeployFailedError` redefined here
- <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 [*] Redefinition of unused `parameterized` from line 5
+ <a href='https://github.com/aws/aws-sam-cli/blob/ba82b4752d2a0a2bbb807e971a9a5d12ecc79590/tests/unit/local/docker/test_lambda_image.py#L8'>tests/unit/local/docker/test_lambda_image.py:8:27:</a> F811 [*] Redefinition of unused `parameterized` from line 5: `parameterized` redefined here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+35 -35 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/Rag_Interface.py#L8'>crazy_functions/Rag_Interface.py:8:41:</a> F811 [*] Redefinition of unused `validate_path_safety` from line 4
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/Rag_Interface.py#L8'>crazy_functions/Rag_Interface.py:8:41:</a> F811 [*] Redefinition of unused `validate_path_safety` from line 4: `validate_path_safety` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/agent_fns/python_comment_agent.py#L268'>crazy_functions/agent_fns/python_comment_agent.py:268:9:</a> F811 Redefinition of unused `dedent` from line 5
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/agent_fns/python_comment_agent.py#L268'>crazy_functions/agent_fns/python_comment_agent.py:268:9:</a> F811 Redefinition of unused `dedent` from line 5: `dedent` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/AI_review_doc.py#L10'>crazy_functions/doc_fns/AI_review_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/AI_review_doc.py#L10'>crazy_functions/doc_fns/AI_review_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9: `Inches` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/batch_file_query_doc.py#L10'>crazy_functions/doc_fns/batch_file_query_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/doc_fns/batch_file_query_doc.py#L10'>crazy_functions/doc_fns/batch_file_query_doc.py:10:39:</a> F811 [*] Redefinition of unused `Inches` from line 9: `Inches` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:48:</a> F811 [*] Redefinition of unused `gen_time_str` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:48:</a> F811 [*] Redefinition of unused `gen_time_str` from line 3: `gen_time_str` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:62:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L4'>crazy_functions/gen_fns/gen_fns_shared.py:4:62:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 3: `trimmed_format_exc` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L5'>crazy_functions/gen_fns/gen_fns_shared.py:5:51:</a> F811 [*] Redefinition of unused `get_log_folder` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/gen_fns/gen_fns_shared.py#L5'>crazy_functions/gen_fns/gen_fns_shared.py:5:51:</a> F811 [*] Redefinition of unused `get_log_folder` from line 3: `get_log_folder` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/latex_fns/latex_actions.py#L392'>crazy_functions/latex_fns/latex_actions.py:392:16:</a> F811 [*] Redefinition of unused `os` from line 348
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/latex_fns/latex_actions.py#L392'>crazy_functions/latex_fns/latex_actions.py:392:16:</a> F811 [*] Redefinition of unused `os` from line 348: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf.py#L4'>crazy_functions/pdf_fns/parse_pdf.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf.py#L4'>crazy_functions/pdf_fns/parse_pdf.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_grobid.py#L4'>crazy_functions/pdf_fns/parse_pdf_grobid.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_grobid.py#L4'>crazy_functions/pdf_fns/parse_pdf_grobid.py:4:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_legacy.py#L3'>crazy_functions/pdf_fns/parse_pdf_legacy.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_legacy.py#L3'>crazy_functions/pdf_fns/parse_pdf_legacy.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L3'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:3:21:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L3'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:3:21:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/report_gen_html.py#L52'>crazy_functions/pdf_fns/report_gen_html.py:52:29:</a> F811 [*] Redefinition of unused `get_log_folder` from line 1
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/pdf_fns/report_gen_html.py#L52'>crazy_functions/pdf_fns/report_gen_html.py:52:29:</a> F811 [*] Redefinition of unused `get_log_folder` from line 1: `get_log_folder` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/vector_fns/vector_database.py#L4'>crazy_functions/vector_fns/vector_database.py:4:8:</a> F811 [*] Redefinition of unused `os` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/vector_fns/vector_database.py#L4'>crazy_functions/vector_fns/vector_database.py:4:8:</a> F811 [*] Redefinition of unused `os` from line 3: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量总结PDF文档.py#L5'>crazy_functions/批量总结PDF文档.py:5:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量总结PDF文档.py#L5'>crazy_functions/批量总结PDF文档.py:5:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 3: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L3'>crazy_functions/批量翻译PDF文档_NOUGAT.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L3'>crazy_functions/批量翻译PDF文档_NOUGAT.py:3:44:</a> F811 [*] Redefinition of unused `promote_file_to_downloadzone` from line 2: `promote_file_to_downloadzone` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L99'>crazy_functions/批量翻译PDF文档_NOUGAT.py:99:12:</a> F811 [*] Redefinition of unused `copy` from line 9
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/crazy_functions/批量翻译PDF文档_NOUGAT.py#L99'>crazy_functions/批量翻译PDF文档_NOUGAT.py:99:12:</a> F811 [*] Redefinition of unused `copy` from line 9: `copy` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm.py#L23'>request_llms/bridge_chatglm.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm.py#L23'>request_llms/bridge_chatglm.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm3.py#L23'>request_llms/bridge_chatglm3.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_chatglm3.py#L23'>request_llms/bridge_chatglm3.py:23:16:</a> F811 [*] Redefinition of unused `os` from line 22: `os` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:21:</a> F811 [*] Redefinition of unused `get_conf` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:21:</a> F811 [*] Redefinition of unused `get_conf` from line 18: `get_conf` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:31:</a> F811 [*] Redefinition of unused `update_ui` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:31:</a> F811 [*] Redefinition of unused `update_ui` from line 18: `update_ui` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:42:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 18
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_claude.py#L25'>request_llms/bridge_claude.py:25:42:</a> F811 [*] Redefinition of unused `trimmed_format_exc` from line 18: `trimmed_format_exc` redefined here
- <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_internlm.py#L53'>request_llms/bridge_internlm.py:53:56:</a> F811 [*] Redefinition of unused `AutoTokenizer` from line 4
+ <a href='https://github.com/binary-husky/gpt_academic/blob/65a4cf59c26d307c35d9f032eca851e428e7b58f/request_llms/bridge_internlm.py#L53'>request_llms/bridge_internlm.py:53:56:</a> F811 [*] Redefinition of unused `AutoTokenizer` from line 4: `AutoTokenizer` redefined here
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/3a44401d580b378395aa066fddec76f494a80af8/tests/test_ls.py#L6'>tests/test_ls.py:6:13:</a> F811 Redefinition of unused `test_account_jwt` from line 3
+ <a href='https://github.com/latchbio/latch/blob/3a44401d580b378395aa066fddec76f494a80af8/tests/test_ls.py#L6'>tests/test_ls.py:6:13:</a> F811 Redefinition of unused `test_account_jwt` from line 3: `test_account_jwt` redefined here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 76 | 38 | 38 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @ntBre on 2025-08-13 16:56_

---

_Review requested from @carljm by @ntBre on 2025-08-13 16:56_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-13 16:56_

---

_Review requested from @sharkdp by @ntBre on 2025-08-13 16:56_

---

_Review requested from @dcreager by @ntBre on 2025-08-13 16:56_

---

_Review request for @dcreager removed by @ntBre on 2025-08-13 16:56_

---

_Review request for @carljm removed by @ntBre on 2025-08-13 16:56_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-13 16:56_

---

_Label `diagnostics` added by @ntBre on 2025-08-13 16:56_

---

_Renamed from "Add sub-diagnostic to F811" to "[`pyflakes`] Add sub-diagnostic (`F811`)" by @ntBre on 2025-08-13 16:57_

---

_Comment by @AlexWaygood on 2025-08-13 17:10_

We have a similar diagnostic in ty for detecting when symbols declared as `Final` are overridden:

<img width="1428" height="614" alt="image" src="https://github.com/user-attachments/assets/09f82ef0-f0ea-476e-acca-0397e436fbe6" />

<br><br>

Although... now that I look at it, it's a bit weird that the "Symbol later reassigned here" annotation is rendered _before_ the "Symbol declared as `Final` here" annotation -- it doesn't really read cleanly as a pair of sentences anymore. Did that change recently with one of your diagnostics refactors? It doesn't look like that's how we originally had them 😆 you can see the screenshot of what they originally looked like in https://github.com/astral-sh/ruff/pull/19214#issue-3213382071

---

_Comment by @ntBre on 2025-08-13 17:14_

Oh interesting. I don't think that should have changed. Are those two screenshots from the same input? I think the rendering depends on exactly how many lines are between the two snippets.

---

_Comment by @AlexWaygood on 2025-08-13 17:18_

> Are those two screenshots from the same input? I think the rendering depends on exactly how many lines are between the two snippets.

Great point, they're not! Here's a screenshot on `main` using the same input as [the screenshot on the original PR](https://github.com/astral-sh/ruff/pull/19214#issue-3213382071) -- that still looks great:

<img width="1598" height="538" alt="image" src="https://github.com/user-attachments/assets/53009229-d7e2-4f63-b13a-c5b03ad99928" />

<br><br>

I guess I should file a bug report at ty to improve that diagnostic if there are many lines between the two snippets (which is probably the common case!)

---

_Comment by @ntBre on 2025-08-13 17:27_

This might be the relevant code, but I haven't tested it:

https://github.com/astral-sh/ruff/blob/ab685c16a8c01bb33f054698d2f32bb023d07429/crates/ruff_db/src/diagnostic/render.rs#L372-L373

We sort the annotations by their start range earlier in this function but then sort the snippets to put the primary one first, which I think aligns with the behavior in your first screenshot.

---

_Comment by @MichaReiser on 2025-08-14 06:42_

Nice!

I'm not entirely sure I follow

> They appear to be using a secondary annotation. I kind of prefer a sub-diagnostic because it seems to ensure that the additional info is after the main diagnostic:

Or, maybe it's just that I dislike that part :) @BurntSushi will know better but I think we try to avoid having info/help text between sub diagnostics with snippets because it looks somewhat odd. 

Either way, I do prefer what we have in ty. It's more concise because it doesn't need the extra info text to explain the annotation; the explanation is right in the annotation. 

---

_@MichaReiser reviewed on 2025-08-14 06:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3353 on 2025-08-14 06:44_

I'm not sure about this. I think it's important that a lint rule author is in full control about how a diagnostic is rendered. If they decide that a sub diagnostic with lower severity should come first, than so be it. I'm sure there's a reason for it. 

---

_Comment by @ntBre on 2025-08-14 12:18_

I like the ty example too, and it will avoid needing to deal with the sub-diagnostic sorting, at least for now. I'll switch to a secondary annotation!

---

_@ntBre reviewed on 2025-08-14 12:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3353 on 2025-08-14 12:26_

I know what you mean, I was just trying to work around the automatic `help` sub-diagnostic for every rule that has a fix as easily as possible. I'm assuming we don't want lint authors to have to control _that_, but maybe we can just expose the sub-diagnostic `Vec` or an API that lets them insert additional sub-diagnostics wherever they want. Or another level of `DiagnosticGuard` that pushes the `help`/`fix_title` last instead of immediately when the `Diagnostic` is created from a `Violation`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3353 on 2025-08-14 12:32_

A more strimlined version of `report_diagnostic`, e.g. `report_custom` or similar, seems the most reasonable to me. Ideally, we'd just remove `fix` and `help`, but I believe those fields are used on the website or other output formats? If not, that would be the simplest: just remove `help` and `fix_title` and set them manually

---

_@MichaReiser reviewed on 2025-08-14 12:32_

---

_Comment by @ntBre on 2025-08-14 13:18_

How do y'all feel about the primary annotation (`bar redefined here`)? I kind of like adding one, like rustc and ty:

<img width="764" height="401" alt="image" src="https://github.com/user-attachments/assets/90707a3e-a351-40be-ae5a-67fc4f5b48c9" />

However, this has the downside of changing our `concise` diagnostic for F811. We use `Diagnostic::concise_message`, which concatenates the main diagnostic message and the primary annotation, if both are set. I'm sure we can work around that, but I didn't want to bother if we don't like the primary annotation anyway.

Adding the secondary annotation doesn't really make sense in the concise diagnostic:

```
F811_0.py:10:5: F811 Redefinition of unused `bar` from line 6: `bar` redefined here
```

However, that is exactly what rustc does:

```
$ rustc try.rs --error-format=short
try.rs:5:1: error[E0428]: the name `foo` is defined multiple times: `foo` redefined here
```

I guess ty must do the same too.

---

_Comment by @AlexWaygood on 2025-08-14 13:25_

I think ideally the concise diagnostic message would not include the text of the primary annotation in _this_ case (since it kinda duplicates the information already in the diagnostic summary). However, I don't think this is _awful_:

```
F811_0.py:10:5: F811 Redefinition of unused `bar` from line 6: `bar` redefined here
```

I'd say it's fine for now!

---

_Comment by @AlexWaygood on 2025-08-14 13:25_

(and yeah, I think ty does do similarly right now!)

---

_Renamed from "[`pyflakes`] Add sub-diagnostic (`F811`)" to "[`pyflakes`] Add secondary annotation showing previous definition (`F811`)" by @ntBre on 2025-08-14 13:27_

---

_Comment by @MichaReiser on 2025-08-14 13:29_

A thought triggered by your message

> Adding the secondary annotation doesn't really make sense in the concise diagnostic:

It might be a nice addition for our testing framework to at least capture some concise diagnostics because, unlike you, I don't think many folks (myself included) verify the output manually as you did.

---

_Comment by @ntBre on 2025-08-14 13:32_

This actually did appear in a CLI snapshot I added in the past, in this case, so I got a bit lucky :) But yes, that sounds like a good idea in general!

---

_Comment by @ntBre on 2025-08-14 13:36_

We could potentially just dump a second rendering of the diagnostics in `concise` form in `print_messages`:

https://github.com/astral-sh/ruff/blob/22c284aefd8bb2a77a25e43eeb00023fcb129d1f/crates/ruff_linter/src/test.rs#L360-L363

I don't think that would be too bad to look at. It reminds me a bit of the parser tests that print the AST.

---

_Comment by @MichaReiser on 2025-08-14 14:17_

I guess we have some coverage for concise in the ecosystem checks. It only means that we'd rely on the issue being present in at least one of our ecosystem projects which might be a bit too fragile

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:272 on 2025-08-14 15:21_

Out of curiosity, what is the motivation for this change?

---

_@BurntSushi reviewed on 2025-08-14 15:21_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:272 on 2025-08-14 15:26_

Just to match the path in `from_diagnostic` above. This isn't used in this PR now that we're using a secondary annotation, so I can revert it!

I'll double-check if it's actually needed for full diagnostics too.

---

_@ntBre reviewed on 2025-08-14 15:26_

---

_@ntBre reviewed on 2025-08-14 15:30_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:272 on 2025-08-14 15:30_

Using `path` instead of `relative_path` changes some of our CLI snapshots for the main diagnostic, so I think we'll need this for sub-diagnostics too, once we use any sub-diagnostics with paths in Ruff.

```
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
        0     0 │ success: false
        1     1 │ exit_code: 1
        2     2 │ ----- stdout -----
        3     3 │ F401 [*] `bar` imported but unused
        4       │- --> bar.py:2:8
              4 │+ --> /tmp/.tmpJqw1Qi/bar.py:2:8
        5     5 │   |
        6     6 │ 2 | import bar   # unused import
        7     7 │   |        ^^^
        8     8 │   |
        9     9 │ help: Remove unused import: `bar`
       10    10 │ 
       11    11 │ F401 [*] `foo` imported but unused
       12       │- --> foo.py:2:8
             12 │+ --> /tmp/.tmpJqw1Qi/foo.py:2:8
       13    13 │   |
       14    14 │ 2 | import foo   # unused import
       15    15 │   |        ^^^
       16    16 │   |
    ────────────┴───────────────────────────────────────────────────────────────────
```

---

_@MichaReiser approved on 2025-08-14 15:44_

---

_Comment by @MichaReiser on 2025-08-14 15:45_

This is a nice find for a rule that can make use of the new features because it is one that's very commonly used.

---

_@ntBre reviewed on 2025-08-14 16:41_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:272 on 2025-08-14 16:41_

(I think I'll go ahead and leave this instead of reverting, since it seems like it will be useful in the future)

---

_Merged by @ntBre on 2025-08-14 17:23_

---

_Closed by @ntBre on 2025-08-14 17:23_

---

_Branch deleted on 2025-08-14 17:23_

---
