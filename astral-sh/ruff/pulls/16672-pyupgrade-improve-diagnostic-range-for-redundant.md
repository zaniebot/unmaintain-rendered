```yaml
number: 16672
title: "[`pyupgrade`]: Improve diagnostic range for `redundant-open-mode` (`UP015`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/unnecessary-mode-argument-range
created_at: 2025-03-12T12:33:49Z
updated_at: 2025-03-13T07:45:24Z
url: https://github.com/astral-sh/ruff/pull/16672
synced_at: 2026-01-12T15:55:55Z
```

# [`pyupgrade`]: Improve diagnostic range for `redundant-open-mode` (`UP015`)

---

_@MichaReiser_

## Summary

This PR stabilizes the behavior change introduced in https://github.com/astral-sh/ruff/pull/15872/

The diagnostic range is now the range of the redundant `mode` argument where it previously was the range of the entire `open` call:

Before:

```
UP015.py:2:1: UP015 [*] Unnecessary mode argument
  |
1 | open("foo", "U")
2 | open("foo", "Ur")
  | ^^^^^^^^^^^^^^^^^ UP015
3 | open("foo", "Ub")
4 | open("foo", "rUb")
  |
  = help: Remove mode argument
```


Now:

```
UP015.py:2:13: UP015 [*] Unnecessary mode argument
  |
1 | open("foo", "U")
2 | open("foo", "Ur")
  |             ^^^^ UP015
3 | open("foo", "Ub")
4 | open("foo", "rUb")
  |
  = help: Remove mode argument
```

This is a breaking change because it may require moving a `noqa` comment onto a different line, e.g if you have

```py
open(
    "foo",
    "Ur",
) # noqa: UP015
```

Needs to be rewritten to 

```py
open(
    "foo",
    "Ur", # noqa: UP015
)
```

There have been now new issues or PRs since the new preview behavior was implemented. It first was released as part of Ruff 0.9.5 on the 5th of Feb (a little more than a month ago)

## Test Plan

I reviewed the snapshot tests


---

_Label `rule` added by @MichaReiser on 2025-03-12 12:34_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 12:34_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 12:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Funnecessary-mode-argument-range)

### Merging #16672 will **degrade performances by 10.59%**

<sub>Comparing <code>micha/unnecessary-mode-argument-range</code> (097563e) with <code>micha/ruff-0.10</code> (0ced1cb)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Funnecessary-mode-argument-range)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.59% |


---

_Comment by @github-actions[bot] on 2025-03-12 12:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+55 -55 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+46 -46 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/check_proxy.py#L161'>check_proxy.py:161:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/check_proxy.py#L161'>check_proxy.py:161:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/check_proxy.py#L191'>check_proxy.py:191:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/check_proxy.py#L191'>check_proxy.py:191:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Conversation_To_File.py#L73'>crazy_functions/Conversation_To_File.py:73:30:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:10:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Conversation_To_File.py#L87'>crazy_functions/Conversation_To_File.py:87:26:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Latex_Project_Polish.py#L66'>crazy_functions/Latex_Project_Polish.py:66:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Latex_Project_Translate_Legacy.py#L46'>crazy_functions/Latex_Project_Translate_Legacy.py:46:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/Markdown_Translate.py#L61'>crazy_functions/Markdown_Translate.py:61:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Analyse.py#L22'>crazy_functions/SourceCode_Analyse.py:22:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Comment.py#L33'>crazy_functions/SourceCode_Comment.py:33:23:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/SourceCode_Comment.py#L79'>crazy_functions/SourceCode_Comment.py:79:76:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/agent_fns/python_comment_agent.py#L207'>crazy_functions/agent_fns/python_comment_agent.py:207:25:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:10:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/ast_fns/comment_remove.py#L48'>crazy_functions/ast_fns/comment_remove.py:48:28:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:10:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L239'>crazy_functions/latex_fns/latex_actions.py:239:24:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L322'>crazy_functions/latex_fns/latex_actions.py:322:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:18:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_actions.py#L358'>crazy_functions/latex_fns/latex_actions.py:358:33:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L294'>crazy_functions/latex_fns/latex_toolbox.py:294:25:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:18:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L321'>crazy_functions/latex_fns/latex_toolbox.py:321:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:22:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/latex_fns/latex_toolbox.py#L386'>crazy_functions/latex_fns/latex_toolbox.py:386:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:18:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L250'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:250:37:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:18:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L269'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:269:37:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L298'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:298:18:</a> UP015 [*] Unnecessary mode argument
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/executions.py#L56'>src/latch/executions.py:56:36:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:34:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:10:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/main.py#L975'>src/latch_cli/main.py:975:57:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:14:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/get_executions.py#L377'>src/latch_cli/services/get_executions.py:377:29:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:10:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/launch.py#L48'>src/latch_cli/services/launch.py:48:28:</a> UP015 [*] Unnecessary mode argument
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/40d8157fd1ec176268c164f4923043288c9a167b/examples/bulk_import/example_bulkwriter.py#L273'>examples/bulk_import/example_bulkwriter.py:273:18:</a> UP015 [*] Unnecessary mode argument
+ <a href='https://github.com/milvus-io/pymilvus/blob/40d8157fd1ec176268c164f4923043288c9a167b/examples/bulk_import/example_bulkwriter.py#L273'>examples/bulk_import/example_bulkwriter.py:273:34:</a> UP015 [*] Unnecessary mode argument
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
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 12:48_

---

_@ntBre approved on 2025-03-12 16:35_

---

_Merged by @MichaReiser on 2025-03-13 07:45_

---

_Closed by @MichaReiser on 2025-03-13 07:45_

---

_Branch deleted on 2025-03-13 07:45_

---
