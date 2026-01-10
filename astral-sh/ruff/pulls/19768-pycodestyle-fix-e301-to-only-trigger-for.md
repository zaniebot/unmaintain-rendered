```yaml
number: 19768
title: "[`pycodestyle`] Fix `E301` to only trigger for functions immediately within a class"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-19752
created_at: 2025-08-05T18:27:34Z
updated_at: 2025-09-16T15:21:45Z
url: https://github.com/astral-sh/ruff/pull/19768
synced_at: 2026-01-10T17:40:28Z
```

# [`pycodestyle`] Fix `E301` to only trigger for functions immediately within a class

---

_Pull request opened by @danparizher on 2025-08-05 18:27_

## Summary

Fixes #19752


---

_Comment by @github-actions[bot] on 2025-08-05 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -151 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Document_Optimize.py#L331'>crazy_functions/Document_Optimize.py:331:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Latex_Project_Polish.py#L16'>crazy_functions/Latex_Project_Polish.py:16:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Latex_Project_Translate_Legacy.py#L16'>crazy_functions/Latex_Project_Translate_Legacy.py:16:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/Markdown_Translate.py#L19'>crazy_functions/Markdown_Translate.py:19:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/SourceCode_Analyse_JupyterNotebook.py#L18'>crazy_functions/SourceCode_Analyse_JupyterNotebook.py:18:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/latex_fns/latex_actions.py#L184'>crazy_functions/latex_fns/latex_actions.py:184:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/reduce_aigc.py#L390'>crazy_functions/paper_fns/reduce_aigc.py:390:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/pdf_fns/report_gen_html.py#L25'>crazy_functions/pdf_fns/report_gen_html.py:25:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_chatglmonnx.py#L33'>request_llms/bridge_chatglmonnx.py:33:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/com_sparkapi.py#L118'>request_llms/com_sparkapi.py:118:9:</a> E301 [*] Expected 1 blank line, found 0
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -140 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L366'>src/bokeh/client/connection.py:366:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L559'>src/bokeh/model/model.py:559:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L231'>src/bokeh/server/session.py:231:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_plot.py#L76'>tests/integration/models/test_plot.py:76:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_plot.py#L80'>tests/integration/models/test_plot.py:80:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_button.py#L65'>tests/integration/widgets/test_button.py:65:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_button.py#L70'>tests/integration/widgets/test_button.py:70:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_button_group.py#L48'>tests/integration/widgets/test_checkbox_button_group.py:48:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_button_group.py#L53'>tests/integration/widgets/test_checkbox_button_group.py:53:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L66'>tests/integration/widgets/test_checkbox_group.py:66:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L71'>tests/integration/widgets/test_checkbox_group.py:71:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_datepicker.py#L183'>tests/integration/widgets/test_datepicker.py:183:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_datepicker.py#L211'>tests/integration/widgets/test_datepicker.py:211:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dropdown.py#L70'>tests/integration/widgets/test_dropdown.py:70:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dropdown.py#L75'>tests/integration/widgets/test_dropdown.py:75:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_button_group.py#L48'>tests/integration/widgets/test_radio_button_group.py:48:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_button_group.py#L53'>tests/integration/widgets/test_radio_button_group.py:53:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L65'>tests/integration/widgets/test_radio_group.py:65:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L70'>tests/integration/widgets/test_radio_group.py:70:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L223'>tests/integration/widgets/test_select.py:223:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L228'>tests/integration/widgets/test_select.py:228:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_toggle.py#L64'>tests/integration/widgets/test_toggle.py:64:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_toggle.py#L69'>tests/integration/widgets/test_toggle.py:69:13:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L132'>tests/unit/bokeh/application/handlers/test_code_runner.py:132:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L106'>tests/unit/bokeh/application/handlers/test_directory.py:106:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L128'>tests/unit/bokeh/application/handlers/test_directory.py:128:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L147'>tests/unit/bokeh/application/handlers/test_directory.py:147:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L170'>tests/unit/bokeh/application/handlers/test_directory.py:170:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L186'>tests/unit/bokeh/application/handlers/test_directory.py:186:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L205'>tests/unit/bokeh/application/handlers/test_directory.py:205:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L215'>tests/unit/bokeh/application/handlers/test_directory.py:215:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L256'>tests/unit/bokeh/application/handlers/test_directory.py:256:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L281'>tests/unit/bokeh/application/handlers/test_directory.py:281:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L319'>tests/unit/bokeh/application/handlers/test_directory.py:319:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L341'>tests/unit/bokeh/application/handlers/test_directory.py:341:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L362'>tests/unit/bokeh/application/handlers/test_directory.py:362:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L381'>tests/unit/bokeh/application/handlers/test_directory.py:381:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L400'>tests/unit/bokeh/application/handlers/test_directory.py:400:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L418'>tests/unit/bokeh/application/handlers/test_directory.py:418:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L434'>tests/unit/bokeh/application/handlers/test_directory.py:434:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_directory.py#L91'>tests/unit/bokeh/application/handlers/test_directory.py:91:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L59'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:59:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L71'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:71:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_notebook__handlers.py#L82'>tests/unit/bokeh/application/handlers/test_notebook__handlers.py:82:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_script.py#L46'>tests/unit/bokeh/application/handlers/test_script.py:46:9:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_script.py#L61'>tests/unit/bokeh/application/handlers/test_script.py:61:9:</a> E301 [*] Expected 1 blank line, found 0
... 94 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E301 | 151 | 0 | 151 | 0 | 0 |

</p>
</details>




---

_Comment by @whitequark on 2025-08-05 18:40_

Nice, -148 violations!

---

_Label `preview` added by @ntBre on 2025-08-05 18:45_

---

_Label `bug` added by @ntBre on 2025-08-05 18:45_

---

_Label `rule` added by @ntBre on 2025-08-05 18:45_

---

_@MichaReiser reviewed on 2025-08-06 06:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:839 on 2025-08-06 06:22_

Can we add the reasoning why we only trigger for functions directly within a class? 

What about

```py

class Foo:
	if True:
		def test(): pass

```

---

_@danparizher reviewed on 2025-08-06 16:14_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:839 on 2025-08-06 16:14_

I added in some additional details; let me know if it's too verbose—there may be a simpler way to put it.

---

_@ntBre reviewed on 2025-08-06 17:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:839 on 2025-08-06 17:05_

I think we need something more like this example or one of the other checks on `main` will short-circuit:

```py
class Foo:
    if True:
        print("conditional")
        def test():
            pass
```

but this version is also flagged by `pycodestyle` (and `flake8`), so I think we'll want to test that it still is:

```shell
$ pycodestyle try.py --select E301 --show-source
try.py:4:9: E301 expected 1 blank line, found 0
        def test():
        ^
```

I was hopeful that we could detect this a bit more robustly than checking indentation anyway.

---

_Review requested from @ntBre by @danparizher on 2025-08-07 21:52_

---

_Review requested from @MichaReiser by @danparizher on 2025-08-07 21:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E306_E30.py.snap`:252 on 2025-08-14 14:36_

Based on my comment with the pycodestyle results, this is supposed to be getting an E301, but it's still only getting an E306.

I would really prefer if we could handle these checks by updating the `state` tracking in this function rather than trying to compare the indentation. Have you explored that at all?

---

_@ntBre requested changes on 2025-08-14 14:37_

---

_@danparizher reviewed on 2025-08-15 18:46_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E306_E30.py.snap`:252 on 2025-08-15 18:46_

I haven't thought of that, thank you!

---

_Review requested from @ntBre by @danparizher on 2025-08-15 19:48_

---

_Comment by @ntBre on 2025-09-08 21:30_

Thanks. Could you resolve the merge conflicts? They should just be from the new diff output format. It also still looks like the `callback` case is getting an E301 diagnostic in the snapshot. Is that correct? I thought it should only be an E306.

---

_Comment by @danparizher on 2025-09-09 17:58_

Should be all set, let me know!

---

_Comment by @whitequark on 2025-09-14 12:38_

Can this PR get a review please? It's the last thing blocking the adoption of ruff for me: https://github.com/GlasgowEmbedded/glasgow/pull/993

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:1046 on 2025-09-15 20:54_

Do we need to move this code now? I tested locally after putting it back, and it seems fine down here still.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:995 on 2025-09-15 20:56_

I tried a couple more test cases like this:

```py
class Bar:
    def f():
        x = 1
        def g():
            return 1
        return 2

    def f():
        class Baz:
            x = 1
            def g():
                return 1
            return 2
```

with the second `f` being more interesting. We get an `E306` for `Baz.g` even though it's a method, but that's actually consistent with the upstream pycodestyle, so I think it's okay.

It might be worth adding that test case, but the code is handling it correctly.

---

_@ntBre reviewed on 2025-09-15 20:58_

I had one question about the E306 code and a suggestion for one more test case, but this looks like a nice fix. Thank you!

---

_Review requested from @ntBre by @danparizher on 2025-09-15 22:58_

---

_Comment by @ntBre on 2025-09-15 23:04_

I think you may have reverted the wrong piece of code. The ecosystem check is now showing -160 E306 violations instead of -151 E301 violations.

---

_Comment by @danparizher on 2025-09-15 23:16_

Hm, maybe I am confused—Could you point me to what part we were looking to revert? Feel free to commit from your side if it's easier, I know this has been a long PR 🙂

---

_Comment by @ntBre on 2025-09-15 23:20_

I'll push a commit! I may not have phrased it well, but I just wanted to move the E306 code back to where it was originally (a standalone `if` instead of an `else if` on the E301 check).

---

_Comment by @ntBre on 2025-09-15 23:33_

This is what I had in mind. I'll double-check the ecosystem report to make sure it's actually equivalent. The `else if` might be necessary, but it's a really clean diff if not. I'm expecting -151 `E301` in the ecosystem check (I think the ecosystem projects have added 3 new instances of E301 since the initial PR when it was -148).

---

_@ntBre approved on 2025-09-16 14:59_

---

_Merged by @ntBre on 2025-09-16 15:00_

---

_Closed by @ntBre on 2025-09-16 15:00_

---

_Comment by @whitequark on 2025-09-16 15:05_

Thank you! When can I expect the fix to be available in a release (really, I just want it to use with pre-commit.ci)?

---

_Branch deleted on 2025-09-16 15:14_

---

_Comment by @ntBre on 2025-09-16 15:18_

Our weekly releases are usually on Thursday, so it should be out in ~2 days!

---

_Comment by @whitequark on 2025-09-16 15:21_

Thanks!

---
