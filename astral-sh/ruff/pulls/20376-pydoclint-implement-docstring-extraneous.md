```yaml
number: 20376
title: "[`pydoclint`] Implement `docstring-extraneous-parameter` (`DOC102`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: doc102
created_at: 2025-09-13T21:26:46Z
updated_at: 2025-10-25T20:30:03Z
url: https://github.com/astral-sh/ruff/pull/20376
synced_at: 2026-01-10T16:59:49Z
```

# [`pydoclint`] Implement `docstring-extraneous-parameter` (`DOC102`)

---

_Pull request opened by @augustelalande on 2025-09-13 21:26_

## Summary

Implement `docstring-extraneous-parameter` (`DOC102`). This rule checks that all parameters present in a functions docstring are also present in its signature.

Split from #13280, per this [comment](https://github.com/astral-sh/ruff/pull/13280#issuecomment-3280575506).

Part of #12434.

## Test Plan

Test cases added.

---

_Comment by @github-actions[bot] on 2025-09-13 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+66 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/dba1a6223ed2a8d9d699ef86a19c0d8ab1d1c92e/disnake/client.py#L814'>disnake/client.py:814:9:</a> DOC102 Documented parameter `Example` is not in the function's signature
+ <a href='https://github.com/DisnakeDev/disnake/blob/dba1a6223ed2a8d9d699ef86a19c0d8ab1d1c92e/disnake/ext/commands/base_core.py#L855'>disnake/ext/commands/base_core.py:855:5:</a> DOC102 Documented parameter `params` is not in the function's signature
+ <a href='https://github.com/DisnakeDev/disnake/blob/dba1a6223ed2a8d9d699ef86a19c0d8ab1d1c92e/disnake/ext/commands/base_core.py#L895'>disnake/ext/commands/base_core.py:895:5:</a> DOC102 Documented parameter `params` is not in the function's signature
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset-extensions-cli/tests/utils.py#L186'>superset-extensions-cli/tests/utils.py:186:9:</a> DOC102 Documented parameter `name` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/async_events/api.py#L57'>superset/async_events/api.py:57:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/charts/data/api.py#L105'>superset/charts/data/api.py:105:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/charts/data/api.py#L287'>superset/charts/data/api.py:287:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L1711'>superset/dashboards/api.py:1711:11:</a> DOC102 Documented parameter `requestBody` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L1717'>superset/dashboards/api.py:1717:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L408'>superset/dashboards/api.py:408:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L459'>superset/dashboards/api.py:459:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L518'>superset/dashboards/api.py:518:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L716'>superset/dashboards/api.py:716:11:</a> DOC102 Documented parameter `requestBody` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L723'>superset/dashboards/api.py:723:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L800'>superset/dashboards/api.py:800:11:</a> DOC102 Documented parameter `requestBody` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/dashboards/api.py#L807'>superset/dashboards/api.py:807:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/databases/api.py#L1058'>superset/databases/api.py:1058:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/databases/api.py#L1139'>superset/databases/api.py:1139:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/databases/api.py#L1373'>superset/databases/api.py:1373:11:</a> DOC102 Documented parameter `requestBody` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/databases/api.py#L1380'>superset/databases/api.py:1380:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
+ <a href='https://github.com/apache/superset/blob/1617bbbe7133f0a6ec1196637a0fb26287c347a7/superset/databases/api.py#L989'>superset/databases/api.py:989:11:</a> DOC102 Documented parameter `responses` is not in the function's signature
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+26 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L427'>src/bokeh/core/has_props.py:427:13:</a> DOC102 Documented parameter `json` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L429'>src/bokeh/core/has_props.py:429:13:</a> DOC102 Documented parameter `models` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L450'>src/bokeh/core/property/dataspec.py:450:13:</a> DOC102 Documented parameter `name` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L402'>src/bokeh/core/property/descriptors.py:402:13:</a> DOC102 Documented parameter `json` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L404'>src/bokeh/core/property/descriptors.py:404:13:</a> DOC102 Documented parameter `models` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L671'>src/bokeh/core/property/descriptors.py:671:13:</a> DOC102 Documented parameter `new` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L790'>src/bokeh/core/property/descriptors.py:790:13:</a> DOC102 Documented parameter `json` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L69'>src/bokeh/core/query.py:69:9:</a> DOC102 Documented parameter `obj` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/decorators.py#L60'>src/bokeh/core/validation/decorators.py:60:9:</a> DOC102 Documented parameter `code` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L353'>src/bokeh/document/document.py:353:13:</a> DOC102 Documented parameter `patch` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L395'>src/bokeh/document/events.py:395:13:</a> DOC102 Documented parameter `column_source` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L487'>src/bokeh/document/events.py:487:13:</a> DOC102 Documented parameter `column_source` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L585'>src/bokeh/document/events.py:585:13:</a> DOC102 Documented parameter `column_source` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L149'>src/bokeh/driving.py:149:9:</a> DOC102 Documented parameter `x` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L367'>src/bokeh/embed/bundle.py:367:9:</a> DOC102 Documented parameter `objs` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L380'>src/bokeh/embed/bundle.py:380:9:</a> DOC102 Documented parameter `objs` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L442'>src/bokeh/embed/bundle.py:442:9:</a> DOC102 Documented parameter `objs` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_tools.py#L86'>src/bokeh/plotting/_tools.py:86:9:</a> DOC102 Documented parameter `tools_map` is not in the function's signature
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L210'>src/bokeh/protocol/message.py:210:13:</a> DOC102 Documented parameter `buf_header` is not in the function's signature
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/langchain_core/callbacks/file.py#L136'>libs/core/langchain_core/callbacks/file.py:136:13:</a> DOC102 Documented parameter `file` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/langchain_core/outputs/chat_generation.py#L47'>libs/core/langchain_core/outputs/chat_generation.py:47:13:</a> DOC102 Documented parameter `values` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/langchain_core/utils/mustache.py#L451'>libs/core/langchain_core/utils/mustache.py:451:9:</a> DOC102 Documented parameter `partials_path` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/langchain_core/utils/mustache.py#L454'>libs/core/langchain_core/utils/mustache.py:454:9:</a> DOC102 Documented parameter `partials_ext` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/tests/unit_tests/test_tools.py#L1350'>libs/core/tests/unit_tests/test_tools.py:1350:13:</a> DOC102 Documented parameter `banana` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/core/tests/unit_tests/test_tools.py#L1351'>libs/core/tests/unit_tests/test_tools.py:1351:13:</a> DOC102 Documented parameter `monkey` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/langchain/langchain_classic/agents/agent.py#L1098'>libs/langchain/langchain_classic/agents/agent.py:1098:13:</a> DOC102 Documented parameter `values` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/langchain/langchain_classic/agents/agent.py#L833'>libs/langchain/langchain_classic/agents/agent.py:833:13:</a> DOC102 Documented parameter `values` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/langchain/langchain_classic/agents/openai_functions_agent/base.py#L69'>libs/langchain/langchain_classic/agents/openai_functions_agent/base.py:69:13:</a> DOC102 Documented parameter `values` is not in the function's signature
+ <a href='https://github.com/langchain-ai/langchain/blob/2209878f480aeb6cbfa4fb271f632b832b10e85d/libs/langchain/langchain_classic/evaluation/scoring/eval_chain.py#L306'>libs/langchain/langchain_classic/evaluation/scoring/eval_chain.py:306:13:</a> DOC102 Documented parameter `prediction_b` is not in the function's signature
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC102 | 66 | 66 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-09-16 20:39_

---

_Label `preview` added by @ntBre on 2025-09-16 20:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:589 on 2025-09-16 20:51_

This doesn't seem like a big deal either way, but I noticed that `parse_raises_google` doesn't trim indentation first, unlike `parse_raises_numpy`. Could we get away with just an immediate `split_once` call followed by a `trim()` call to strip the whitespace? Again, it seems fine either way, just an idea to more closely mirror the `raises` version.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:153 on 2025-09-16 21:34_

Do you think it would be helpful to show a sub-diagnostic or secondary annotation highlighting the parameter list itself? Obviously they're nearby in the source code, but it might be nice to see them both in the diagnostic. Just an idea since we have support for multiple spans now.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC102_numpy.py`:11 on 2025-09-16 21:38_

Could we add a test case without a type? It looked like the code would handle it correctly, but I didn't see a test for it.

---

_@ntBre reviewed on 2025-09-16 21:43_

Thanks for your work here and for splitting off the PR! This looks great to me overall, just a few minor ideas/suggestions. Did you look into reusing any code from `D417`? That seemed like the only [comment](https://github.com/astral-sh/ruff/pull/13280#issuecomment-2362997178) from the other PR that wasn't fully resolved.

I'd also like @MichaReiser to have a chance to look since he reviewed #13280 originally.

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:153 on 2025-09-17 03:28_

lol the original PR predates multiple spans. Can you suggest an example I can reference?

---

_@augustelalande reviewed on 2025-09-17 03:28_

---

_@augustelalande reviewed on 2025-09-17 03:49_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:589 on 2025-09-17 03:49_

Depends if we care about enforcing indentation. You are right that I'm being inconsistent between raises and parameters. Not sure which is the better approach.

---

_@ntBre reviewed on 2025-09-18 14:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:153 on 2025-09-18 14:30_

Sure, I copied this from another comment I wrote so hopefully the  links aren't too outdated:

The easiest way is  with `secondary_annotation` on the guard returned by `report_diagnostic`:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/checkers/ast/mod.rs#L3309-L3311

Here's the one use of it we've added so far:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/rules/redefined_while_unused.rs#L195-L198

And an example of the type of diagnostic it produces (the `-----` underlining is used for secondary annotations):

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F811_F811_0.py.snap#L4-L17

There are some other options if that doesn't look good, but that's the easiest place to start.

---

_@augustelalande reviewed on 2025-09-19 01:45_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:589 on 2025-09-19 01:45_

I think I prefer enforcing consistent indentation. I would rather update parse_raises in a separate PR

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:589 on 2025-09-19 16:50_

That sounds good to me

---

_@ntBre reviewed on 2025-09-19 16:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_google.py.snap`:40 on 2025-09-19 17:05_

Oh wow, I was actually suggesting to show the parameters in the function definition, which I thought would be a bit easier to implement. Something like:

```
DOC102 Documented parameter `multiplier` is not in the function's signature
  --> DOC102_google.py:21:5
   |
19 |     Multiplies each element in a list by a given multiplier.
20 |
21 |     Args:
   |     ^^^^
22 |         lst (list of int): A list of integers.
23 |         multiplier (int): The multiplier for each element in the list.
24 |
25 |     Returns:
   |
   | ------------------------------------------------------------
   |
16 | # DOC102
17 | def multiply_list_elements(lst):
   |                           ----- parameters defined here
18 | """
   |
help: Remove the extraneous parameter from the docstring
```

Now that you've implemented this, though, should we make it the main diagnostic range instead of the section heading? I saw some previous discussion about this [here](https://github.com/astral-sh/ruff/pull/13280#discussion_r1752784612).

After mocking up my original/intended suggestion above, I think it might have been a bad idea, but using the ranges you're computing now as the main diagnostic ranges would be great. What do you think? At that point we could also emit one diagnostic per parameter as Micha suggested in the other PR.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:678 on 2025-09-19 17:12_

I think we try to avoid `as` casts since they can silently misbehave. This should be safe since the ranges are from within the document, and the document has to be less than `u32::MAX` in length, but let's unwrap in case that assumption ever breaks down:

```suggestion
                            content_start + TextSize::try_from(param_start as u32).unwrap(),
                            content_start + TextSize::try_from(param_end).unwrap(),
```

Ah this is what clippy is complaining about too.

---

_@ntBre reviewed on 2025-09-19 17:27_

Thank you! You went above and beyond my diagnostic suggestion. I think we could use that for the main diagnostic now. The code looks good to me otherwise.

However, I took a quick peek at the ecosystem results, and this one in particular looks pretty suspicious:

[disnake/client.py:805:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L805) DOC102 These documented parameters are not in the function's signature: `Example`, `--------`, `.. code-block`

I know we have some open issues around sphinx docstrings, but ideally we'd eliminate these false positives at least.

This one also looks interesting, I wonder if we should consider skipping `*args` as well as `**kwargs`:

[disnake/ext/commands/context.py:310:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/ext/commands/context.py#L310) DOC102 Documented parameter `entity` is not in the function's signature

This one looks like it should have been straightforward, but we have a false positive on `cls`:

[disnake/ext/commands/flag_converter.py:556:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/ext/commands/flag_converter.py#L556) DOC102 Documented parameter `cls` is not in the function's signature

We'll need to go through all of the ecosystem results, but those are a few I pulled out from the `disnake` run.

---

_Comment by @augustelalande on 2025-09-19 23:14_

I fixed:

[disnake/ext/commands/context.py:310:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/ext/commands/context.py#L310) DOC102 Documented parameter entity is not in the function's signature

and

[disnake/ext/commands/flag_converter.py:556:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/ext/commands/flag_converter.py#L556) DOC102 Documented parameter cls is not in the function's signature

however the problem with

[disnake/client.py:805:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L805) DOC102 These documented parameters are not in the function's signature: Example, --------, .. code-block

is that `Example` is not an official numpy docstring section, where `Examples` is prefered. As defined [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/docstrings/numpy.rs) and [here](https://numpydoc.readthedocs.io/en/latest/format.html#examples)

---

_Comment by @ntBre on 2025-09-23 12:53_

Thanks! Have you looked through the other ecosystem results? It looks like airflow has some YAML blocks in docstrings that we're trying to parse as parameter names (lots of `-` diagnostics), and there are a couple of splitting issues in bokeh like this:

> [src/bokeh/core/has_props.py:435:13:](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L435) DOC102 Documented parameter `setter(ClientSession` is not in the function's signature

I wonder if we should be a little more permissive with the numpy example(s) section. I don't really like the look of emitting diagnostics like this:

+ [disnake/client.py:812:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L812) DOC102 Documented parameter `Example` is not in the function's signature
+ [disnake/client.py:813:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L813) DOC102 Documented parameter `--------` is not in the function's signature
+ [disnake/client.py:815:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L815) DOC102 Documented parameter `.. code-block` is not in the function's signature

It may help in several of these cases to filter out candidates that aren't valid Python identifiers, if we can't avoid checking them in the first place.

---

_@ntBre reviewed on 2025-09-23 12:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-extraneous-parameter_DOC102_numpy.py.snap`:10 on 2025-09-23 12:57_

These new diagnostics look really good! It looks like we're underlining one additional character in some cases, though. The `a` example above in the Google file looks good, but this one has an extra space.

---

_Comment by @augustelalande on 2025-09-23 21:44_

> It may help in several of these cases to filter out candidates that aren't valid Python identifiers, if we can't avoid checking them in the first place.

I have limited the detection of parameters to those beginning with valid characters.

>I wonder if we should be a little more permissive with the numpy example(s) section. I don't really like the look of emitting diagnostics like this:
    [disnake/client.py:812:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L812) DOC102 Documented parameter Example is not in the function's signature
    [disnake/client.py:813:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L813) DOC102 Documented parameter -------- is not in the function's signature
    [disnake/client.py:815:9:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/client.py#L815) DOC102 Documented parameter .. code-block is not in the function's signature

We can, and it is easy to change, but it seems out of scope for this PR

>[src/bokeh/core/has_props.py:435:13:](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L435) DOC102 Documented parameter setter(ClientSession is not in the function's signature

As far as I'm concerned this one is a problem with their formatting, they should have a space between the parameter and the type.

>lots of - diagnostics

This is also a problem with their docstrings, they aren't using google or numpy style, so we can't parse them properly.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:597 on 2025-10-01 18:12_

I imagine that this would cover the vast majority of cases, but it's not strictly correct. We should try to use something like [`is_identifier`](https://github.com/astral-sh/ruff/blob/caf48f4bfc54bb587342bf794a4f5167d855f529/crates/ruff_python_stdlib/src/identifiers.rs#L7) or the helper functions it uses to cover non-ascii cases and any other nuances.

I'm kind of wondering if a regex might be helpful here. Something like `\*{0,2}([\w_]+) *:` seems like it would work? Then we could apply `is_identifier` to the capture group and reject any non-identifiers. Then we can be a bit more flexible with missing spaces and such.

---

_@ntBre reviewed on 2025-10-01 18:32_

Thanks! I've been thinking about how to handle the false positives. I think I agree with you that these are technically issues in the ecosystem docs, but they'll still look like false positives in Ruff, so I'd like to err on the side of filtering them out.

I had a couple of ideas about identifier handling in an inline comment that I think could help with some of them.

I'm somewhat less concerned about the `Example` case and the inline YAML docstrings, but one idea that came to mind is if we can bail out if we're not in a known `Args` or `Parameters` section? That seems like it would filter out at least the YAML cases I looked at, which don't fall under any known section heading as far as I can tell.

---

_Comment by @augustelalande on 2025-10-01 18:51_

> Thanks! I've been thinking about how to handle the false positives. I think I agree with you that these are technically issues in the ecosystem docs, but they'll still look like false positives in Ruff, so I'd like to err on the side of filtering them out.
> 
> I had a couple of ideas about identifier handling in an inline comment that I think could help with some of them.
> 
> I'm somewhat less concerned about the `Example` case and the inline YAML docstrings, but one idea that came to mind is if we can bail out if we're not in a known `Args` or `Parameters` section? That seems like it would filter out at least the YAML cases I looked at, which don't fall under any known section heading as far as I can tell.

If by YAML docstring you are referring to something like this https://github.com/apache/superset/blob/4b71adaa9c89925676e7fed6af0c56c5b0ece863/superset/dashboards/api.py#L1711, the issue is that one of the keys is [`parameters`](https://github.com/apache/superset/blob/4b71adaa9c89925676e7fed6af0c56c5b0ece863/superset/dashboards/api.py#L1705C11-L1705C22) which causes our parser to think it is in a parameters section. We already split the docstring into sections, so we are not processing anything outside of identified sections.

---

_@augustelalande reviewed on 2025-10-01 20:16_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:597 on 2025-10-01 20:16_

Ok I should now only be considering valid identifiers. That should get rid of more false positives.

---

_Comment by @augustelalande on 2025-10-07 20:07_

@ntBre Any further comments?

---

_Comment by @ntBre on 2025-10-07 21:01_

No, I think you've convinced me. I might do one more pass over the code, but I think we should land this.

There are still a few false positives for unusual docstring formats, but the ecosystem check is also showing quite a few true positives. I think it's a helpful rule.

---

_Comment by @augustelalande on 2025-10-15 23:04_

@ntBre Sorry to be pushy, but it's been a year since I made the initial pull request, it's been reviewed 3 or 4 times, and every time the conversation just goes silent with no more comments and no merge. I'm fine with making more changes, and fine with it not being merged, but I would really appreciate some transparency about what is holding things up.

Best,

Auguste

---

_Comment by @ntBre on 2025-10-15 23:06_

Sorry, just a bit behind on my notifications. This is still in my inbox to do another once-over on the code, but I didn't quite get to it today. I'll make this my first review tomorrow.

---

_Comment by @augustelalande on 2025-10-15 23:08_

@ntBre Thanks for the quick reply, and appreciate everything you guys do

---

_@ntBre approved on 2025-10-16 14:46_

Awesome, thanks for all of your work on this and for your patience with all the reviews!

I pushed one commit using `TextSize` a bit more heavily in the parameter parsing functions. The  +1 to the length wasn't quite right for files with `\r\n` line endings, but we can use the `full_line_end` method from the `LineRanges` trait to help with that. Then it became a bit easier to use `TextSize` elsewhere and avoid some unwraps to boot.

I also wrote up a follow-up issue for the last two types of false positives in the ecosystem check: https://github.com/astral-sh/ruff/issues/20923. I think we should go ahead and land this, but I wanted to track those somewhere.

I'll make sure my change doesn't regress the ecosystem check, and then I'll hit merge!

---

_Merged by @ntBre on 2025-10-16 15:26_

---

_Closed by @ntBre on 2025-10-16 15:26_

---

_Branch deleted on 2025-10-25 20:30_

---
