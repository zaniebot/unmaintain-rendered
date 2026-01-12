```yaml
number: 15872
title: "[`pyupgrade`] Better messages and diagnostic range (`UP015`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - preview
  - diagnostics
assignees: []
merged: true
base: main
head: UP015
created_at: 2025-02-01T15:30:09Z
updated_at: 2025-02-05T17:11:30Z
url: https://github.com/astral-sh/ruff/pull/15872
synced_at: 2026-01-12T15:55:53Z
```

# [`pyupgrade`] Better messages and diagnostic range (`UP015`)

---

_@InSyncWithFoo_

## Summary

Resolves #15863.

In preview, diagnostic ranges will now be limited to that of the argument. Rule documentation, variable names, error messages and fix titles have all been modified to use "argument" consistently.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-02-01 15:31_

Is it necessary to add a "Preview" section to the rule documentation?

---

_Comment by @github-actions[bot] on 2025-02-01 15:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+55 -55 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+46 -46 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L161'>check_proxy.py:161:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L161'>check_proxy.py:161:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L191'>check_proxy.py:191:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L191'>check_proxy.py:191:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:10:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:10:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:10:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:18:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:18:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:22:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:22:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:18:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:18:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L298'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:298:18:</a> UP015 [*] Unnecessary mode argument
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:10:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:14:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:10:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:10:</a> UP015 [*] Unnecessary open mode parameters
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/c5a3c18be2fb4dde06167f05af2761456af0e2d4/examples/bulk_import/example_bulkwriter.py#L263'>examples/bulk_import/example_bulkwriter.py:263:18:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/milvus-io/pymilvus/blob/c5a3c18be2fb4dde06167f05af2761456af0e2d4/examples/bulk_import/example_bulkwriter.py#L263'>examples/bulk_import/example_bulkwriter.py:263:18:</a> UP015 [*] Unnecessary open mode parameters
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP015 | 110 | 55 | 55 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+55 -55 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+46 -46 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L161'>check_proxy.py:161:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L161'>check_proxy.py:161:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L191'>check_proxy.py:191:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/check_proxy.py#L191'>check_proxy.py:191:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:30:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:26:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:76:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:25:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:28:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:24:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:33:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:25:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:22:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:37:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:37:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/0458590a776616112191752f267cf5acd7791298/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L298'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:298:18:</a> UP015 [*] Unnecessary open mode parameters
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/executions.py#L56'>src/latch/executions.py:56:36:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:34:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:57:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:14:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:10:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:28:</a> UP015 [*] Unnecessary mode argument
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/c5a3c18be2fb4dde06167f05af2761456af0e2d4/examples/bulk_import/example_bulkwriter.py#L263'>examples/bulk_import/example_bulkwriter.py:263:18:</a> UP015 [*] Unnecessary open mode parameters
+ <a href='https://github.com/milvus-io/pymilvus/blob/c5a3c18be2fb4dde06167f05af2761456af0e2d4/examples/bulk_import/example_bulkwriter.py#L263'>examples/bulk_import/example_bulkwriter.py:263:34:</a> UP015 [*] Unnecessary mode argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP015 | 110 | 55 | 55 | 0 | 0 |

</p>
</details>




---

_Label `preview` added by @MichaReiser on 2025-02-03 09:35_

---

_Label `diagnostics` added by @MichaReiser on 2025-02-03 09:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-03 09:37_

```suggestion
            format!("Unnecessary open mode argument, use `{replacement}`")
```

Should we use the singular form here too or are there cases where there are multiple possible open modes?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:129 on 2025-02-03 09:38_

Maybe for another PR, should this use our generic `remove_argument` helper instead of rolling its own function for this?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:51 on 2025-02-03 09:39_

I find the `open mode` hard to read. Maybe simplify to "Remove `mode` argument"

---

_@MichaReiser approved on 2025-02-03 09:40_

Thanks. A few small nits but overall this looks good

---

_@InSyncWithFoo reviewed on 2025-02-03 15:28_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-03 15:28_

There are: `rUb` (read, unicode, binary) can be simplified to `rb`.

---

_@MichaReiser reviewed on 2025-02-04 09:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-04 09:07_

That's fair. I still think singular makes sense here because even with `rb`, there's only a single `mode` argument (the mode might be multiple characters but it's always a single argument)

---

_@InSyncWithFoo reviewed on 2025-02-04 19:49_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-04 19:49_

"Unnecessary open mode argument" has the implication that the entire argument is unnecessary, but that's not correct. Only parts of it are.

---

_@MichaReiser reviewed on 2025-02-05 08:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-05 08:10_

I'm then leaning towards removing the `argument` everywhere and instead just use `Unnecessary open mode, use {replacement}`

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-05 08:23_

@MichaReiser These are the four messages at stake:

* Error message when the entire argument is unnecessary: `Unnecessary mode argument`
* Error message when some modes are unnecessary: <code>Unnecessary modes, use \`{replacement}\`</code>
* Fix title when the entire argument is unnecessary: `Remove mode argument`
* Fix title when some modes are unnecessary: <code>Replace with \`{replacement}\`</code>

Am I correct that the following is what you prefer?

* Error message when the entire argument is unnecessary: `Unnecessary mode`
* Error message when some modes are unnecessary: <code>Unnecessary modes, use \`{replacement}\`</code>
* Fix title when the entire argument is unnecessary: `Remove mode`
* Fix title when some modes are unnecessary: <code>Replace with \`{replacement}\`</code>

---

_@InSyncWithFoo reviewed on 2025-02-05 08:23_

---

_@MichaReiser reviewed on 2025-02-05 08:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-05 08:33_

That's what I had in mind, based on your explanation but please let me know if you've concerns. I'm sure you know the rule much better by now than I do

---

_@InSyncWithFoo reviewed on 2025-02-05 08:40_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs`:44 on 2025-02-05 08:40_

I prefer to keep the current messages, since whether or not "argument" is included clearly reflects what should be removed.

---

_@MichaReiser approved on 2025-02-05 08:44_

---

_Merged by @MichaReiser on 2025-02-05 08:44_

---

_Closed by @MichaReiser on 2025-02-05 08:44_

---

_Branch deleted on 2025-02-05 17:11_

---
