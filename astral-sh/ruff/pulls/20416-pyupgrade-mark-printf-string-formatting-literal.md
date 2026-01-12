```yaml
number: 20416
title: "[`pyupgrade`] Mark `printf-string-formatting` literal fixes as safe (`UP031`)"
type: pull_request
state: open
author: auscompgeek
labels:
  - fixes
  - preview
assignees: []
base: main
head: printf-string-formatting-safe-fixes
created_at: 2025-09-15T13:04:35Z
updated_at: 2025-11-21T08:00:29Z
url: https://github.com/astral-sh/ruff/pull/20416
synced_at: 2026-01-12T15:57:01Z
```

# [`pyupgrade`] Mark `printf-string-formatting` literal fixes as safe (`UP031`)

---

_@auscompgeek_

## Summary

This marks the subset of fixes emitted by UP031, where the RHS operand is a literal (see https://docs.astral.sh/ruff/rules/printf-string-formatting/#fix-safety), as safe.

## Test Plan

`cargo test`, and inspect snapshot diff to see the new safe fixes

---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-15 13:22_

---

_Comment by @dylwil3 on 2025-09-15 13:24_

Thanks for working on this! Would you mind gating this change behind preview? You'll want to add a function to https://github.com/astral-sh/ruff/blob/e8b445012507762e794f6a5f6247856e093e53e9/crates/ruff_linter/src/preview.rs with a comment linking to this PR and then call that function in the module implementing the rule

---

_Comment by @ntBre on 2025-09-15 13:25_

Thanks for working on this, but I'm not sure we want to make this change. The fix has always been unsafe, even before we started handling variables (https://github.com/astral-sh/ruff/pull/11019), so I think there could be a good reason. For example, I think [this comment](https://github.com/astral-sh/ruff/issues/15555#issuecomment-3282365625) is relevant here too:

> `%s` is not equivalent to passing `None` to `__format__` except by convention.

As an example of this:

```pycon
>>> class C:
...     def __format__(*args, **kwargs):
...         print("called my format!")
...         return ""
...
>>> "{}".format(C())
called my format!
''
>>> "%s" % C()
'<__main__.C object at 0x7fd0f5757250>'
```

So I think this fix should remain unsafe, even if this is pretty reliable in practice.

---

_Comment by @auscompgeek on 2025-09-15 14:33_

> As an example of this:

Ah, that's a fun example. I think there shouldn't be any examples of this being unsafe for a literal RHS though, right?

---

_Comment by @ntBre on 2025-09-15 16:42_

Hmm, yeah it's probably safe for literals. But it also seems relatively uncommon to pass a literal to a formatting call instead of using a literal string. I'd probably just lean toward leaving the whole thing unsafe for the sake of simplicity, but I'm open to other thoughts. Making the fix safe for literals in preview, as Dylan said, sounds technically okay.

---

_Review request for @dylwil3 removed by @dylwil3 on 2025-09-15 21:01_

---

_Renamed from "[`pyupgrade`] Mark `printf-string-formatting` fixes as safe (`UP031`)" to "[`pyupgrade`] Mark `printf-string-formatting` literal fixes as safe (`UP031`)" by @auscompgeek on 2025-09-16 02:26_

---

_Comment by @auscompgeek on 2025-09-16 02:33_

I agree that formatting only a single literal on the RHS would be quite uncommon. I can see it being useful with multiple values though.

I've also added a comment about `__format__` to the fix safety docs for the rule, in case that question comes up again :smile:

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:79 on 2025-09-17 14:15_

Not sure exactly the best phrasing here, maybe something like this?

```suggestion
/// Additionally, while `__format__` typically calls `__str__`, this is not always
/// the case. If `__format__` has been overridden, replacing `%s` with the
/// empty `format` placeholder will change the program's behavior:
```

I also think we should mention explicitly when the fix _is_ safe. To me that feels most natural right at the beginning of the section ("The fix is only marked as safe when the value being formatted is a literal"). We should also try to work `In preview, ...` in there somewhere.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:480 on 2025-09-17 14:17_

It looks like `is_constant` already works for tuples:

https://github.com/astral-sh/ruff/blob/d8b4b38d0bae57e72bf7c59eef68e40c5ffafa76/crates/ruff_python_ast/src/helpers.rs#L661-L663

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:498 on 2025-09-17 14:21_

I think we've already checked the keys in `clean_params_dictionary`. We're transforming a dict literal into keyword arguments like `key=value`, and we bail out without a `params_string` if the key isn't a string literal. But we do need to check the values with `is_constant`; it doesn't handle `Expr::Dict`s automatically like tuples.

---

_@ntBre reviewed on 2025-09-17 14:24_

Thanks, this looks reasonable to me. I had a few minor comments, and I think we should add some test cases showing the safe fix. It seems all of the existing tests are still unsafe, or at least they aren't being run with preview enabled.

---

_Comment by @github-actions[bot] on 2025-09-17 14:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+151 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Document_Optimize.py#L331'>crazy_functions/Document_Optimize.py:331:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Latex_Project_Polish.py#L16'>crazy_functions/Latex_Project_Polish.py:16:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Latex_Project_Translate_Legacy.py#L16'>crazy_functions/Latex_Project_Translate_Legacy.py:16:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Markdown_Translate.py#L19'>crazy_functions/Markdown_Translate.py:19:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/SourceCode_Analyse_JupyterNotebook.py#L18'>crazy_functions/SourceCode_Analyse_JupyterNotebook.py:18:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/latex_fns/latex_actions.py#L184'>crazy_functions/latex_fns/latex_actions.py:184:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/reduce_aigc.py#L390'>crazy_functions/paper_fns/reduce_aigc.py:390:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/pdf_fns/report_gen_html.py#L25'>crazy_functions/pdf_fns/report_gen_html.py:25:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_chatglmonnx.py#L33'>request_llms/bridge_chatglmonnx.py:33:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/com_sparkapi.py#L118'>request_llms/com_sparkapi.py:118:9:</a> E301 [*] Expected 1 blank line, found 0
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+140 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L366'>src/bokeh/client/connection.py:366:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L559'>src/bokeh/model/model.py:559:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L231'>src/bokeh/server/session.py:231:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_plot.py#L76'>tests/integration/models/test_plot.py:76:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_plot.py#L80'>tests/integration/models/test_plot.py:80:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_button.py#L65'>tests/integration/widgets/test_button.py:65:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_button.py#L70'>tests/integration/widgets/test_button.py:70:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_button_group.py#L48'>tests/integration/widgets/test_checkbox_button_group.py:48:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_button_group.py#L53'>tests/integration/widgets/test_checkbox_button_group.py:53:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L66'>tests/integration/widgets/test_checkbox_group.py:66:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L71'>tests/integration/widgets/test_checkbox_group.py:71:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_datepicker.py#L183'>tests/integration/widgets/test_datepicker.py:183:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_datepicker.py#L211'>tests/integration/widgets/test_datepicker.py:211:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dropdown.py#L70'>tests/integration/widgets/test_dropdown.py:70:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dropdown.py#L75'>tests/integration/widgets/test_dropdown.py:75:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_button_group.py#L48'>tests/integration/widgets/test_radio_button_group.py:48:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_button_group.py#L53'>tests/integration/widgets/test_radio_button_group.py:53:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L65'>tests/integration/widgets/test_radio_group.py:65:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L70'>tests/integration/widgets/test_radio_group.py:70:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L223'>tests/integration/widgets/test_select.py:223:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L228'>tests/integration/widgets/test_select.py:228:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_toggle.py#L64'>tests/integration/widgets/test_toggle.py:64:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_toggle.py#L69'>tests/integration/widgets/test_toggle.py:69:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L132'>tests/unit/bokeh/application/handlers/test_code_runner.py:132:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L106'>tests/unit/bokeh/application/handlers/test_directory.py:106:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L128'>tests/unit/bokeh/application/handlers/test_directory.py:128:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L147'>tests/unit/bokeh/application/handlers/test_directory.py:147:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L170'>tests/unit/bokeh/application/handlers/test_directory.py:170:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L186'>tests/unit/bokeh/application/handlers/test_directory.py:186:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L205'>tests/unit/bokeh/application/handlers/test_directory.py:205:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L215'>tests/unit/bokeh/application/handlers/test_directory.py:215:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L256'>tests/unit/bokeh/application/handlers/test_directory.py:256:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L281'>tests/unit/bokeh/application/handlers/test_directory.py:281:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L319'>tests/unit/bokeh/application/handlers/test_directory.py:319:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L341'>tests/unit/bokeh/application/handlers/test_directory.py:341:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L362'>tests/unit/bokeh/application/handlers/test_directory.py:362:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L381'>tests/unit/bokeh/application/handlers/test_directory.py:381:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L400'>tests/unit/bokeh/application/handlers/test_directory.py:400:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L418'>tests/unit/bokeh/application/handlers/test_directory.py:418:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L434'>tests/unit/bokeh/application/handlers/test_directory.py:434:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L91'>tests/unit/bokeh/application/handlers/test_directory.py:91:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L59'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:59:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L71'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:71:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L82'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:82:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_script.py#L46'>tests/unit/bokeh/application/handlers/test_script.py:46:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_script.py#L61'>tests/unit/bokeh/application/handlers/test_script.py:61:9:</a> E301 [*] Expected 1 blank line, found 0
... 94 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E301 | 151 | 151 | 0 | 0 | 0 |

</p>
</details>




---

_Label `fixes` added by @ntBre on 2025-09-17 16:12_

---

_Label `preview` added by @ntBre on 2025-09-17 16:12_

---

_Comment by @MichaReiser on 2025-11-21 08:00_

@auscompgeek do you think you'll have time in the near future to address the PR feedback. If not, that's fine. I think we could follow up with the feedback ourselves to get this landed.

---
