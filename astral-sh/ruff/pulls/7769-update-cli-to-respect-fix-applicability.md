```yaml
number: 7769
title: Update CLI to respect fix applicability
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: main
head: zanie/applicability
created_at: 2023-10-02T22:26:57Z
updated_at: 2023-10-06T03:53:47Z
url: https://github.com/astral-sh/ruff/pull/7769
synced_at: 2026-01-12T02:32:41Z
```

# Update CLI to respect fix applicability

---

_Pull request opened by @zanieb on 2023-10-02 22:26_

Rebase of https://github.com/astral-sh/ruff/pull/5119 authored by @evanrittenhouse with additional refinements.

## Changes

- Adds `--unsafe-fixes` / `--no-unsafe-fixes` flags to `ruff check`
    - Violations with unsafe fixes are not shown as fixable unless opted-in
- Fix applicability is respected now
    - `Applicability::Never` fixes are no longer applied
    - `Applicability::Sometimes` fixes require opt-in
    - `Applicability::Always` fixes are unchanged
 - Hints for availability of `--unsafe-fixes` added to `ruff check` output

## Examples

Check hints at hidden unsafe fixes
```
❯ ruff check example.py --no-cache --select F601,W292
example.py:1:14: F601 Dictionary key literal `'a'` repeated
example.py:2:15: W292 [*] No newline at end of file
Found 2 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

We could add an indicator for which violations have hidden fixes in the future.

Check treats unsafe fixes as applicable with opt-in
```
❯ ruff check example.py --no-cache --select F601,W292 --unsafe-fixes
example.py:1:14: F601 [*] Dictionary key literal `'a'` repeated
example.py:2:15: W292 [*] No newline at end of file
Found 2 errors.
[*] 2 fixable with the --fix option.
```

Also can be enabled in the config file

```
❯ cat ruff.toml
unsafe-fixes = true
```

And opted-out per invocation

```
❯ ruff check example.py --no-cache --select F601,W292 --no-unsafe-fixes
example.py:1:14: F601 Dictionary key literal `'a'` repeated
example.py:2:15: W292 [*] No newline at end of file
Found 2 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Diff does not include unsafe fixes
```
❯ ruff check example.py --no-cache --select F601,W292 --diff
--- example.py
+++ example.py
@@ -1,2 +1,2 @@
 x = {'a': 1, 'a': 1}
-print(('foo'))
+print(('foo'))
\ No newline at end of file

Would fix 1 error.
```

Unless there is opt-in
```
❯ ruff check example.py --no-cache --select F601,W292 --diff --unsafe-fixes
--- example.py
+++ example.py
@@ -1,2 +1,2 @@
-x = {'a': 1}
-print(('foo'))
+x = {'a': 1, 'a': 1}
+print(('foo'))
\ No newline at end of file

Would fix 2 errors.
```

https://github.com/astral-sh/ruff/pull/7790 will improve the diff messages following this pull request

Similarly, `--fix` and `--fix-only` require the `--unsafe-fixes` flag to apply unsafe fixes.

## Related

Replaces #5119
Closes https://github.com/astral-sh/ruff/issues/4185
Closes https://github.com/astral-sh/ruff/issues/7214
Closes https://github.com/astral-sh/ruff/issues/4845
Closes https://github.com/astral-sh/ruff/issues/3863
Addresses https://github.com/astral-sh/ruff/issues/6835
Addresses https://github.com/astral-sh/ruff/issues/7019
Needs follow-up https://github.com/astral-sh/ruff/issues/6962
Needs follow-up https://github.com/astral-sh/ruff/issues/4845
Needs follow-up https://github.com/astral-sh/ruff/issues/7436
Needs follow-up https://github.com/astral-sh/ruff/issues/7025
Needs follow-up https://github.com/astral-sh/ruff/issues/6434
Follow-up #7790 
Follow-up https://github.com/astral-sh/ruff/pull/7792

---

_@zanieb reviewed on 2023-10-02 22:30_

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:82 on 2023-10-02 22:30_

```suggestion
    /// Attempt to fix both automatic and suggested lint violations.
```

---

_@zanieb reviewed on 2023-10-02 22:31_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:292 on 2023-10-02 22:31_

Revert this change (view with whitespace ignored, change to notebook iteration?)

---

_@zanieb reviewed on 2023-10-02 22:32_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:409 on 2023-10-02 22:32_

Investigate avoiding this clone

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:441 on 2023-10-02 22:32_

Why the change in `transformed.write`?

---

_@zanieb reviewed on 2023-10-02 22:32_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:247 on 2023-10-02 22:33_

What does this refer to?

---

_@zanieb reviewed on 2023-10-02 22:33_

---

_@zanieb reviewed on 2023-10-02 22:33_

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:188 on 2023-10-02 22:33_

Why `contains` instead of `intersects`?

---

_@zanieb reviewed on 2023-10-02 22:34_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:805 on 2023-10-02 22:34_

This seems incorrect, `success: false` seems to track this.

---

_@zanieb reviewed on 2023-10-02 22:35_

---

_Review comment by @zanieb on `crates/ruff_diagnostics/src/fix.rs`:22 on 2023-10-02 22:35_

We should probably drop `Unspecified` as it's only there for transition purposes?

---

_Comment by @github-actions[bot] on 2023-10-02 23:00_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+34056, -34004, 0 error(s))

<details><summary>disnake (+6, -5)</summary>
<p>

<pre>
+ 5 hidden fixes can be enabled with the `--unsafe-fixes` option.
+ <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L756'>disnake/enums.py:756:30:</a> RUF100 Unused `noqa` directive (unused: `RUF001`)
- <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L756'>disnake/enums.py:756:30:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
+ <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L764'>disnake/enums.py:764:25:</a> RUF100 Unused `noqa` directive (unused: `RUF001`)
- <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L764'>disnake/enums.py:764:25:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
+ <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L810'>disnake/enums.py:810:31:</a> RUF100 Unused `noqa` directive (unused: `RUF001`)
- <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/enums.py#L810'>disnake/enums.py:810:31:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
+ <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/opus.py#L237'>disnake/opus.py:237:47:</a> RUF100 Unused `noqa` directive (unused: `PLW0603`)
- <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/opus.py#L237'>disnake/opus.py:237:47:</a> RUF100 [*] Unused `noqa` directive (unused: `PLW0603`)
+ <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/opus.py#L242'>disnake/opus.py:242:42:</a> RUF100 Unused `noqa` directive (unused: `PLW0603`)
- <a href='https://github.com/DisnakeDev/disnake/blob/1f6104dcc86c38f391ba4dfee515b1b0929655f2/disnake/opus.py#L242'>disnake/opus.py:242:42:</a> RUF100 [*] Unused `noqa` directive (unused: `PLW0603`)
</pre>

</p>
</details>
<details><summary>HouseWatch (+1, -0)</summary>
<p>

<pre>
+ [*] 11 fixable with the `--fix` option.
</pre>

</p>
</details>
<details><summary>rasa (+48, -47)</summary>
<p>

<pre>
+ [*] 108 fixable with the `--fix` option (47 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/examples/concertbot/actions/actions.py#L59'>examples/concertbot/actions/actions.py:59:9:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/examples/concertbot/actions/actions.py#L59'>examples/concertbot/actions/actions.py:59:9:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/examples/rules/actions/actions.py#L22'>examples/rules/actions/actions.py:22:9:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/examples/rules/actions/actions.py#L22'>examples/rules/actions/actions.py:22:9:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/api.py#L143'>rasa/api.py:143:95:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/api.py#L143'>rasa/api.py:143:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/api.py#L144'>rasa/api.py:144:95:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/api.py#L144'>rasa/api.py:144:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/action.py#L1026'>rasa/core/actions/action.py:1026:101:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/action.py#L1026'>rasa/core/actions/action.py:1026:101:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/action.py#L952'>rasa/core/actions/action.py:952:95:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/action.py#L952'>rasa/core/actions/action.py:952:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/two_stage_fallback.py#L184'>rasa/core/actions/two_stage_fallback.py:184:105:</a> RUF100 Unused blanket `noqa` directive
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/actions/two_stage_fallback.py#L184'>rasa/core/actions/two_stage_fallback.py:184:105:</a> RUF100 [*] Unused blanket `noqa` directive
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/agent.py#L422'>rasa/core/agent.py:422:93:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/agent.py#L422'>rasa/core/agent.py:422:93:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/featurizers/single_state_featurizer.py#L119'>rasa/core/featurizers/single_state_featurizer.py:119:97:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/featurizers/single_state_featurizer.py#L119'>rasa/core/featurizers/single_state_featurizer.py:119:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L301'>rasa/core/test.py:301:92:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L301'>rasa/core/test.py:301:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L308'>rasa/core/test.py:308:92:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L308'>rasa/core/test.py:308:92:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L462'>rasa/core/test.py:462:97:</a> RUF100 Unused blanket `noqa` directive
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/core/test.py#L462'>rasa/core/test.py:462:97:</a> RUF100 [*] Unused blanket `noqa` directive
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/nlu/extractors/crf_entity_extractor.py#L111'>rasa/nlu/extractors/crf_entity_extractor.py:111:89:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/nlu/extractors/crf_entity_extractor.py#L111'>rasa/nlu/extractors/crf_entity_extractor.py:111:89:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/nlu/utils/hugging_face/registry.py#L28'>rasa/nlu/utils/hugging_face/registry.py:28:77:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/nlu/utils/hugging_face/registry.py#L28'>rasa/nlu/utils/hugging_face/registry.py:28:77:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/events.py#L88'>rasa/shared/core/events.py:88:109:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/events.py#L88'>rasa/shared/core/events.py:88:109:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/slots.py#L301'>rasa/shared/core/slots.py:301:95:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/slots.py#L301'>rasa/shared/core/slots.py:301:95:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/trackers.py#L278'>rasa/shared/core/trackers.py:278:111:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/trackers.py#L278'>rasa/shared/core/trackers.py:278:111:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L220'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py:220:107:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L220'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py:220:107:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/training_data/visualization.py#L454'>rasa/shared/core/training_data/visualization.py:454:111:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/core/training_data/visualization.py#L454'>rasa/shared/core/training_data/visualization.py:454:111:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/nlu/training_data/entities_parser.py#L27'>rasa/shared/nlu/training_data/entities_parser.py:27:145:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/shared/nlu/training_data/entities_parser.py#L27'>rasa/shared/nlu/training_data/entities_parser.py:27:145:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/endpoints.py#L23'>rasa/utils/endpoints.py:23:82:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/endpoints.py#L23'>rasa/utils/endpoints.py:23:82:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/tensorflow/data_generator.py#L220'>rasa/utils/tensorflow/data_generator.py:220:97:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/tensorflow/data_generator.py#L220'>rasa/utils/tensorflow/data_generator.py:220:97:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/tensorflow/model_data.py#L106'>rasa/utils/tensorflow/model_data.py:106:108:</a> RUF100 Unused `noqa` directive (unused: `E501`)
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/rasa/utils/tensorflow/model_data.py#L106'>rasa/utils/tensorflow/model_data.py:106:108:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L1'>scripts/evaluate_release_tag.py:1:1:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L1'>scripts/evaluate_release_tag.py:1:1:</a> D200 [*] One-line docstring should fit on one line
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L25'>scripts/evaluate_release_tag.py:25:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L25'>scripts/evaluate_release_tag.py:25:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L38'>scripts/evaluate_release_tag.py:38:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L38'>scripts/evaluate_release_tag.py:38:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L42'>scripts/evaluate_release_tag.py:42:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L42'>scripts/evaluate_release_tag.py:42:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L47'>scripts/evaluate_release_tag.py:47:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/evaluate_release_tag.py#L47'>scripts/evaluate_release_tag.py:47:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/release.py#L226'>scripts/release.py:226:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/scripts/release.py#L226'>scripts/release.py:226:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/aio_pika/robust_channel.pyi#L15'>stubs/aio_pika/robust_channel.pyi:15:18:</a> RUF013 PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/aio_pika/robust_channel.pyi#L15'>stubs/aio_pika/robust_channel.pyi:15:18:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/aio_pika/robust_channel.pyi#L19'>stubs/aio_pika/robust_channel.pyi:19:20:</a> RUF013 PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/aio_pika/robust_channel.pyi#L19'>stubs/aio_pika/robust_channel.pyi:19:20:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/socketio.pyi#L51'>stubs/socketio.pyi:51:64:</a> RUF013 PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/stubs/socketio.pyi#L51'>stubs/socketio.pyi:51:64:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/actions/test_forms.py#L1887'>tests/core/actions/test_forms.py:1887:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/actions/test_forms.py#L1887'>tests/core/actions/test_forms.py:1887:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/featurizers/test_single_state_featurizers.py#L276'>tests/core/featurizers/test_single_state_featurizers.py:276:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/featurizers/test_single_state_featurizers.py#L276'>tests/core/featurizers/test_single_state_featurizers.py:276:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/test_broker.py#L309'>tests/core/test_broker.py:309:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/test_broker.py#L309'>tests/core/test_broker.py:309:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/test_channels.py#L601'>tests/core/test_channels.py:601:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/core/test_channels.py#L601'>tests/core/test_channels.py:601:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L111'>tests/nlu/featurizers/test_lm_featurizer.py:111:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L111'>tests/nlu/featurizers/test_lm_featurizer.py:111:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L576'>tests/nlu/featurizers/test_lm_featurizer.py:576:9:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L576'>tests/nlu/featurizers/test_lm_featurizer.py:576:9:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L598'>tests/nlu/featurizers/test_lm_featurizer.py:598:9:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L598'>tests/nlu/featurizers/test_lm_featurizer.py:598:9:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L624'>tests/nlu/featurizers/test_lm_featurizer.py:624:9:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L624'>tests/nlu/featurizers/test_lm_featurizer.py:624:9:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L90'>tests/nlu/featurizers/test_lm_featurizer.py:90:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/nlu/featurizers/test_lm_featurizer.py#L90'>tests/nlu/featurizers/test_lm_featurizer.py:90:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/core/test_domain.py#L1833'>tests/shared/core/test_domain.py:1833:5:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/core/test_domain.py#L1833'>tests/shared/core/test_domain.py:1833:5:</a> D200 [*] One-line docstring should fit on one line
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/core/test_domain.py#L1833'>tests/shared/core/test_domain.py:1833:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/core/test_domain.py#L1833'>tests/shared/core/test_domain.py:1833:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/nlu/training_data/test_features.py#L179'>tests/shared/nlu/training_data/test_features.py:179:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/shared/nlu/training_data/test_features.py#L179'>tests/shared/nlu/training_data/test_features.py:179:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/utils/test_plotting.py#L20'>tests/utils/test_plotting.py:20:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/utils/test_plotting.py#L20'>tests/utils/test_plotting.py:20:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
+ <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/utils/test_plotting.py#L30'>tests/utils/test_plotting.py:30:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/RasaHQ/rasa/blob/f5fed3a25aa208a6e172670c0ec7c0e134f59bd6/tests/utils/test_plotting.py#L30'>tests/utils/test_plotting.py:30:5:</a> D415 [*] First line should end with a period, question mark, or exclamation point
</pre>

</p>
</details>
<details><summary>snowcli (+1, -0)</summary>
<p>

<pre>
+ [*] 116 fixable with the `--fix` option.
</pre>

</p>
</details>
<details><summary>airflow (+6435, -6434)</summary>
<p>

<pre>
+ [*] 15859 fixable with the `--fix` option (6473 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/__init__.py#L93'>airflow/__init__.py:93:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/__init__.py#L93'>airflow/__init__.py:93:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/__init__.py#L98'>airflow/__init__.py:98:5:</a> SIM108 Use ternary operator `val = getattr(mod, attr_name) if attr_name else mod` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/__init__.py#L98'>airflow/__init__.py:98:5:</a> SIM108 [*] Use ternary operator `val = getattr(mod, attr_name) if attr_name else mod` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/__init__.py#L33'>airflow/api/__init__.py:33:5:</a> SIM105 Use `contextlib.suppress(AirflowConfigException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/__init__.py#L33'>airflow/api/__init__.py:33:5:</a> SIM105 [*] Use `contextlib.suppress(AirflowConfigException)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/auth/backend/kerberos_auth.py#L67'>airflow/api/auth/backend/kerberos_auth.py:67:9:</a> ANN204 Missing return type annotation for special method `__init__`
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/auth/backend/kerberos_auth.py#L67'>airflow/api/auth/backend/kerberos_auth.py:67:9:</a> ANN204 [*] Missing return type annotation for special method `__init__`
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/api_client.py#L27'>airflow/api/client/api_client.py:27:9:</a> ANN204 Missing return type annotation for special method `__init__`
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/api_client.py#L27'>airflow/api/client/api_client.py:27:9:</a> ANN204 [*] Missing return type annotation for special method `__init__`
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L65'>airflow/api/client/local_client.py:65:32:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L65'>airflow/api/client/local_client.py:65:32:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L73'>airflow/api/client/local_client.py:73:37:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L73'>airflow/api/client/local_client.py:73:37:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L76'>airflow/api/client/local_client.py:76:37:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L76'>airflow/api/client/local_client.py:76:37:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L80'>airflow/api/client/local_client.py:80:37:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L80'>airflow/api/client/local_client.py:80:37:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L92'>airflow/api/client/local_client.py:92:16:</a> RET504 Unnecessary assignment to `lineage` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/client/local_client.py#L92'>airflow/api/client/local_client.py:92:16:</a> RET504 [*] Unnecessary assignment to `lineage` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/airflow_health.py#L89'>airflow/api/common/airflow_health.py:89:12:</a> RET504 Unnecessary assignment to `airflow_health_status` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/airflow_health.py#L89'>airflow/api/common/airflow_health.py:89:12:</a> RET504 [*] Unnecessary assignment to `airflow_health_status` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/delete_dag.py#L60'>airflow/api/common/delete_dag.py:60:32:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/delete_dag.py#L60'>airflow/api/common/delete_dag.py:60:32:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/delete_dag.py#L63'>airflow/api/common/delete_dag.py:63:27:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/delete_dag.py#L63'>airflow/api/common/delete_dag.py:63:27:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/__init__.py#L36'>airflow/api/common/experimental/__init__.py:36:27:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/__init__.py#L36'>airflow/api/common/experimental/__init__.py:36:27:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L39'>airflow/api/common/experimental/pool.py:39:33:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L39'>airflow/api/common/experimental/pool.py:39:33:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L43'>airflow/api/common/experimental/pool.py:43:28:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L43'>airflow/api/common/experimental/pool.py:43:28:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L60'>airflow/api/common/experimental/pool.py:60:33:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L60'>airflow/api/common/experimental/pool.py:60:33:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L65'>airflow/api/common/experimental/pool.py:65:33:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L65'>airflow/api/common/experimental/pool.py:65:33:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L70'>airflow/api/common/experimental/pool.py:70:33:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L70'>airflow/api/common/experimental/pool.py:70:33:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L91'>airflow/api/common/experimental/pool.py:91:33:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L91'>airflow/api/common/experimental/pool.py:91:33:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L94'>airflow/api/common/experimental/pool.py:94:33:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L94'>airflow/api/common/experimental/pool.py:94:33:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L98'>airflow/api/common/experimental/pool.py:98:28:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/experimental/pool.py#L98'>airflow/api/common/experimental/pool.py:98:28:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L122'>airflow/api/common/mark_tasks.py:122:26:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L122'>airflow/api/common/mark_tasks.py:122:26:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L125'>airflow/api/common/mark_tasks.py:125:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L125'>airflow/api/common/mark_tasks.py:125:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L129'>airflow/api/common/mark_tasks.py:129:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L129'>airflow/api/common/mark_tasks.py:129:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L132'>airflow/api/common/mark_tasks.py:132:26:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L132'>airflow/api/common/mark_tasks.py:132:26:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L137'>airflow/api/common/mark_tasks.py:137:26:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L137'>airflow/api/common/mark_tasks.py:137:26:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:12:</a> RET504 Unnecessary assignment to `qry_sub_dag` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:12:</a> RET504 [*] Unnecessary assignment to `qry_sub_dag` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L206'>airflow/api/common/mark_tasks.py:206:12:</a> RET504 Unnecessary assignment to `qry_dag` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L206'>airflow/api/common/mark_tasks.py:206:12:</a> RET504 [*] Unnecessary assignment to `qry_dag` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L303'>airflow/api/common/mark_tasks.py:303:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L303'>airflow/api/common/mark_tasks.py:303:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L307'>airflow/api/common/mark_tasks.py:307:5:</a> SIM108 Use ternary operator `start_date = dag.start_date if dag.start_date else execution_date` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L307'>airflow/api/common/mark_tasks.py:307:5:</a> SIM108 [*] Use ternary operator `start_date = dag.start_date if dag.start_date else execution_date` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L336'>airflow/api/common/mark_tasks.py:336:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L336'>airflow/api/common/mark_tasks.py:336:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L408'>airflow/api/common/mark_tasks.py:408:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L408'>airflow/api/common/mark_tasks.py:408:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L411'>airflow/api/common/mark_tasks.py:411:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L411'>airflow/api/common/mark_tasks.py:411:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L414'>airflow/api/common/mark_tasks.py:414:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L414'>airflow/api/common/mark_tasks.py:414:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L461'>airflow/api/common/mark_tasks.py:461:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L461'>airflow/api/common/mark_tasks.py:461:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L464'>airflow/api/common/mark_tasks.py:464:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L464'>airflow/api/common/mark_tasks.py:464:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L468'>airflow/api/common/mark_tasks.py:468:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L468'>airflow/api/common/mark_tasks.py:468:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L553'>airflow/api/common/mark_tasks.py:553:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L553'>airflow/api/common/mark_tasks.py:553:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L556'>airflow/api/common/mark_tasks.py:556:30:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L556'>airflow/api/common/mark_tasks.py:556:30:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L559'>airflow/api/common/mark_tasks.py:559:26:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/mark_tasks.py#L559'>airflow/api/common/mark_tasks.py:559:26:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L122'>airflow/api/common/trigger_dag.py:122:27:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L122'>airflow/api/common/trigger_dag.py:122:27:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L55'>airflow/api/common/trigger_dag.py:55:27:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L55'>airflow/api/common/trigger_dag.py:55:27:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L60'>airflow/api/common/trigger_dag.py:60:26:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L60'>airflow/api/common/trigger_dag.py:60:26:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L69'>airflow/api/common/trigger_dag.py:69:17:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api/common/trigger_dag.py#L69'>airflow/api/common/trigger_dag.py:69:17:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L123'>airflow/api_connexion/endpoints/config_endpoint.py:123:17:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L123'>airflow/api_connexion/endpoints/config_endpoint.py:123:17:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L43'>airflow/api_connexion/endpoints/config_endpoint.py:43:12:</a> RET504 Unnecessary assignment to `config` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L43'>airflow/api_connexion/endpoints/config_endpoint.py:43:12:</a> RET504 [*] Unnecessary assignment to `config` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L88'>airflow/api_connexion/endpoints/config_endpoint.py:88:28:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/config_endpoint.py#L88'>airflow/api_connexion/endpoints/config_endpoint.py:88:28:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L136'>airflow/api_connexion/endpoints/connection_endpoint.py:136:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L136'>airflow/api_connexion/endpoints/connection_endpoint.py:136:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L69'>airflow/api_connexion/endpoints/connection_endpoint.py:69:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L69'>airflow/api_connexion/endpoints/connection_endpoint.py:69:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L83'>airflow/api_connexion/endpoints/connection_endpoint.py:83:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/connection_endpoint.py#L83'>airflow/api_connexion/endpoints/connection_endpoint.py:83:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L129'>airflow/api_connexion/endpoints/dag_endpoint.py:129:24:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L129'>airflow/api_connexion/endpoints/dag_endpoint.py:129:24:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L192'>airflow/api_connexion/endpoints/dag_endpoint.py:192:24:</a> EM102 Exception must not use an f-string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L192'>airflow/api_connexion/endpoints/dag_endpoint.py:192:24:</a> EM102 [*] Exception must not use an f-string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_endpoint.py:58:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L58'>airflow/api_connexion/endpoints/dag_endpoint.py:58:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L68'>airflow/api_connexion/endpoints/dag_endpoint.py:68:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L68'>airflow/api_connexion/endpoints/dag_endpoint.py:68:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L92'>airflow/api_connexion/endpoints/dag_endpoint.py:92:9:</a> SIM108 Use ternary operator `dags_query = dags_query.where(DagModel.is_paused) if paused else dags_query.where(~DagModel.is_paused)` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_endpoint.py#L92'>airflow/api_connexion/endpoints/dag_endpoint.py:92:9:</a> SIM108 [*] Use ternary operator `dags_query = dags_query.where(DagModel.is_paused) if paused else dags_query.where(~DagModel.is_paused)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_run_endpoint.py#L108'>airflow/api_connexion/endpoints/dag_run_endpoint.py:108:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_run_endpoint.py#L108'>airflow/api_connexion/endpoints/dag_run_endpoint.py:108:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_run_endpoint.py#L134'>airflow/api_connexion/endpoints/dag_run_endpoint.py:134:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_run_endpoint.py#L134'>airflow/api_connexion/endpoints/dag_run_endpoint.py:134:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_source_endpoint.py#L40'>airflow/api_connexion/endpoints/dag_source_endpoint.py:40:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dag_source_endpoint.py#L40'>airflow/api_connexion/endpoints/dag_source_endpoint.py:40:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dataset_endpoint.py#L56'>airflow/api_connexion/endpoints/dataset_endpoint.py:56:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/dataset_endpoint.py#L56'>airflow/api_connexion/endpoints/dataset_endpoint.py:56:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/event_log_endpoint.py#L49'>airflow/api_connexion/endpoints/event_log_endpoint.py:49:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/event_log_endpoint.py#L49'>airflow/api_connexion/endpoints/event_log_endpoint.py:49:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L59'>airflow/api_connexion/endpoints/extra_link_endpoint.py:59:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L59'>airflow/api_connexion/endpoints/extra_link_endpoint.py:59:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L64'>airflow/api_connexion/endpoints/extra_link_endpoint.py:64:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L64'>airflow/api_connexion/endpoints/extra_link_endpoint.py:64:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L75'>airflow/api_connexion/endpoints/extra_link_endpoint.py:75:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L75'>airflow/api_connexion/endpoints/extra_link_endpoint.py:75:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L83'>airflow/api_connexion/endpoints/extra_link_endpoint.py:83:12:</a> RET504 Unnecessary assignment to `all_extra_links` before `return` statement
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/extra_link_endpoint.py#L83'>airflow/api_connexion/endpoints/extra_link_endpoint.py:83:12:</a> RET504 [*] Unnecessary assignment to `all_extra_links` before `return` statement
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/import_error_endpoint.py#L49'>airflow/api_connexion/endpoints/import_error_endpoint.py:49:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/import_error_endpoint.py#L49'>airflow/api_connexion/endpoints/import_error_endpoint.py:49:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L102'>airflow/api_connexion/endpoints/log_endpoint.py:102:9:</a> SIM105 Use `contextlib.suppress(TaskNotFound)` instead of `try`-`except`-`pass`
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L102'>airflow/api_connexion/endpoints/log_endpoint.py:102:9:</a> SIM105 [*] Use `contextlib.suppress(TaskNotFound)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L70'>airflow/api_connexion/endpoints/log_endpoint.py:70:30:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L70'>airflow/api_connexion/endpoints/log_endpoint.py:70:30:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L83'>airflow/api_connexion/endpoints/log_endpoint.py:83:26:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/log_endpoint.py#L83'>airflow/api_connexion/endpoints/log_endpoint.py:83:26:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L44'>airflow/api_connexion/endpoints/task_endpoint.py:44:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L44'>airflow/api_connexion/endpoints/task_endpoint.py:44:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L49'>airflow/api_connexion/endpoints/task_endpoint.py:49:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L49'>airflow/api_connexion/endpoints/task_endpoint.py:49:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L63'>airflow/api_connexion/endpoints/task_endpoint.py:63:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_endpoint.py#L63'>airflow/api_connexion/endpoints/task_endpoint.py:63:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L100'>airflow/api_connexion/endpoints/task_instance_endpoint.py:100:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L100'>airflow/api_connexion/endpoints/task_instance_endpoint.py:100:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L103'>airflow/api_connexion/endpoints/task_instance_endpoint.py:103:24:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L103'>airflow/api_connexion/endpoints/task_instance_endpoint.py:103:24:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L106'>airflow/api_connexion/endpoints/task_instance_endpoint.py:106:13:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L106'>airflow/api_connexion/endpoints/task_instance_endpoint.py:106:13:</a> EM101 [*] Exception must not use a string literal, assign to variable first
+ <a href='https://github.com/apache/airflow/blob/59ed537fb10095176ae5bf70a19b7b48ab0850cf/airflow/api_connexion/endpoints/task_instance_endpoint.py#L147'>airflow/api_connexion/endpoints/task_instance_endpoint.py:147:24:</a> EM101 Exception must not use a string liter

---

_@zanieb reviewed on 2023-10-03 01:21_

---

_Review comment by @zanieb on `crates/ruff_diagnostics/src/fix.rs`:22 on 2023-10-03 01:21_

Let's do this in a follow-up pull request to keep the diff small.

---

_Label `breaking` added by @zanieb on 2023-10-03 17:22_

---

_Comment by @codspeed-hq[bot] on 2023-10-03 17:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/applicability)

### Merging #7769 will **not alter performance**

<sub>Comparing <code>zanie/applicability</code> (859ac7d) with <code>main</code> (1dd5deb)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @zanieb on 2023-10-03 18:04_

Rebased @evanrittenhouse to be the author of the first commit — I am leaning towards using this pull request since I've got details in the body.

---

_@zanieb reviewed on 2023-10-03 18:26_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:409 on 2023-10-03 18:26_

I avoided this but now pass a reference?

---

_Marked ready for review by @zanieb on 2023-10-03 19:22_

---

_Review comment by @konstin on `crates/ruff_cli/src/args.rs`:79 on 2023-10-04 15:06_

Personal preference:

```suggestion
    /// Apply fixes to resolve lint violations.
```

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:258 on 2023-10-04 15:07_

nit: could you give this a temporary variable

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:441 on 2023-10-04 15:12_

Either looks fine to me

---

_Review comment by @konstin on `crates/ruff_cli/src/printer.rs`:452 on 2023-10-04 15:14_

i'd match here

---

_@charliermarsh reviewed on 2023-10-04 15:15_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:85 on 2023-10-04 15:15_

What was the reasoning behind using `--fix-suggested` instead of `--unsafe`? Could we also consider `--suggested` or `--with-suggested` or `--include-suggested`? I don't want to derail the PR, but the confusion for me (IIRC) is that it's similar to `--fix` but conceptually independent.

For example, you're now supposed to do `--fix --fix-suggested` (`--fix-suggested` alone as no effect, IIUC), but `--fix-suggested` feels like it should imply `--fix`.

Similarly, you need to do `--diff --fix-suggested` whereas `--diff --fix` would be invalid.

---

_Review comment by @konstin on `crates/ruff_cli/tests/integration_test.rs`:896 on 2023-10-04 15:22_

That's confusing, the user just passed `--fix-suggested` and now we're telling them to use `--fix-suggested`, also shouldn't we get the fixed source back?

---

_@charliermarsh reviewed on 2023-10-04 15:25_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:38 on 2023-10-04 15:25_

Having played around with it in the terminal, I think I prefer `^` over `**`, it feels cleaner to me. But I recognize that it's somewhat subjective.

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:85 on 2023-10-04 15:25_

Ah gee... I intended for `--fix-suggested` to imply` --fix` but that gets weird with `--diff` which I guess is why Evan went with this approach?

There's lots of discussion elsewhere about why not `--unsafe` but basically the idea was that we wanted consistency between the internal "Suggested" level (which is also going to be displayed in the output eventually) and the user facing concept. Also "Manual" fixes will not be fixed.

---

_@zanieb reviewed on 2023-10-04 15:25_

---

_Review comment by @konstin on `crates/ruff_diagnostics/src/fix.rs`:30 on 2023-10-04 15:25_

nit: Grammar and consistency in these docstrings

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:85 on 2023-10-04 15:26_

My qualms with `--suggested` / `--with-suggested` / `--include-suggested` is that it is not clear that they refer to fixes specifically. 

---

_@zanieb reviewed on 2023-10-04 15:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/text.rs`:144 on 2023-10-04 15:27_

Can we use an `if-let` here instead of `.unwrap()`? We already test `.is_some()` above, we should be able to do `if let Some(fix) = message.fix.as_ref()` instead of `.is_some()` + `.unwrap()`.

---

_@charliermarsh reviewed on 2023-10-04 15:27_

---

_@charliermarsh reviewed on 2023-10-04 15:28_

I haven't debugged it, but locally, passing `--fix-suggested` is preventing Ruff from applying any fixes for me.

E.g., I have:

```python
getattr(x, "y")

import os
```

And I see:

```
❯ cargo run -p ruff_cli -- check foo.py --select B --fix --fix-suggested -n --isolated
    Finished dev [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff check foo.py --select B --fix --fix-suggested -n --isolated`
foo.py:1:1: B009 [^] Do not call `getattr` with a constant attribute value. It is not any safer than normal property access.
Found 1 error.
[^] 1 potentially fixable with the --fix-suggested option.
```

---

_@zanieb reviewed on 2023-10-04 15:28_

---

_Review comment by @zanieb on `crates/ruff_linter/src/message/text.rs`:144 on 2023-10-04 15:28_

Yeah I'm actually in the middle of changing this already :)

---

_Review comment by @konstin on `crates/ruff_linter/src/settings/flags.rs`:6 on 2023-10-04 15:29_

it feels like this should be a struct with two enums, but not critical to merging

---

_Review comment by @konstin on `crates/ruff_cli/tests/integration_test.rs`:856 on 2023-10-04 15:30_

formatting

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:439 on 2023-10-04 15:31_

Nit: `#[derive(Debug)]`

---

_@zanieb reviewed on 2023-10-04 15:33_

---

_Review comment by @zanieb on `crates/ruff_diagnostics/src/fix.rs`:38 on 2023-10-04 15:33_

I liked it in the terminal but it looked bad when copied into Discord :/

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:480 on 2023-10-04 15:33_

Should this instead implement `Display`? (See: `impl Display for FormatResultSummary` as an example.) That way, you could do:

```rust
writeln!(writer, "{fixables}")?;
```

---

_@konstin approved on 2023-10-04 15:35_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:501 on 2023-10-04 15:37_

This case combined with `is_empty` feels slightly off to me, since if someone is calling `violation_string` on an empty set, it's probably a mistake? I could see a few alternatives...

1. `fn new` could be `fn try_from` or something like that, and it could return `Option<Self>` (`None` when there are no fixes), so that it's impossible to represent an empty set of fixes.
2. This method could return an `Option` (and `is_empty` could be removed).

(1) feels the most natural to me. What do you think?

---

_@charliermarsh reviewed on 2023-10-04 15:37_

---

_@charliermarsh reviewed on 2023-10-04 15:38_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:38 on 2023-10-04 15:38_

I like it in the terminal too. I think the highlighting we apply helps a bit?

---

_@charliermarsh reviewed on 2023-10-04 15:39_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:896 on 2023-10-04 15:39_

Hmm yeah, what's going on here? :)

---

_@charliermarsh reviewed on 2023-10-04 15:40_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:246 on 2023-10-04 15:40_

I feel like passing `--fix-suggested` without any of `--fix`, `--diff`, etc. should raise an error on the CLI. Can we enforce that somehow?

---

_@evanrittenhouse reviewed on 2023-10-04 21:26_

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/args.rs`:85 on 2023-10-04 21:26_

Yeah, I went with unsafe originally to expand it to all commands (like --diff --fix-suggested makes less sense than --diff ---unsafe). Maybe --include-unsafe?

IMHO it should bias towards external users - we can change the internal Suggested variant to Unsafe, or just document that unsafe refers to "Suggested" fixes

---

_@zanieb reviewed on 2023-10-05 04:36_

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:85 on 2023-10-05 04:36_

I think we're going to go with `--unsafe-fixes` and change the internal names somehow (e.g. #7819)

---

_@zanieb reviewed on 2023-10-05 20:55_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:249 on 2023-10-05 20:55_

I reverted some changes around here to minimize the diff

---

_@zanieb reviewed on 2023-10-05 21:40_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:258 on 2023-10-05 21:40_

I'm not sure what you mean here, but I'd like to address this separately to keep the diff clear (I reverted some of Evan's changes here to keep the diff simpler already).

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:81 on 2023-10-05 21:44_

Should we say "potentially unsafe fixes"? (Defer to you.)

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:257 on 2023-10-05 21:45_

I think Konsti's suggestion (which it's totally up to you whether you take) was to do, like:

```
let var = lint_fix(...);
if let Ok(FixerResult { ... }) = var {
  ....
}
```

...to avoid having a long expanded call on the RHS of the `if-let`. Again, completely up to you.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/check.rs`:130 on 2023-10-05 21:49_

I think accessing from `settings` here could lead to inconsistencies, since the Printer and Emitter and such will use the "global" value, but accessing from `settings` here will read the `unsafe-fixes` from a nested configuration file -- if I understand correctly.

For example... if you had `unsafe-fixes = true` in a `pyproject.toml` at the top of the project, and then `unsafe-fixes = false` in a `pyproject.toml` in a subdirectory, wouldn't the CLI _think_ you ran with `unsafe-fixes`, where internally you would _not_ run with `unsafe-fixes` when analyzing files in the subdirectory?

I suspect this should be passed down from the top-level like `fix_mode` for consistency throughout the execution.

---

_@charliermarsh approved on 2023-10-05 21:50_

Nice! I have one question (or concern, depending on the response) but overall looks great.

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/check.rs`:130 on 2023-10-05 21:53_

Hm okay let me investigate that

---

_@zanieb reviewed on 2023-10-05 21:53_

---

_@zanieb reviewed on 2023-10-05 21:53_

---

_Review comment by @zanieb on `crates/ruff_cli/src/diagnostics.rs`:257 on 2023-10-05 21:53_

👍 I'll do that separately. I want to use `match` instead of `if matches! / else` just outside this too.

---

_@zanieb reviewed on 2023-10-05 21:55_

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:81 on 2023-10-05 21:55_

Hm.. you were the one that told me unsafe is an all or nothing concept haha I'll ponder this as well.

Should it be "potentially incorrect fixes"? Fwiw I feel like `--unsafe-fixes` is the place for a more detailed description.

I also considered "to include additional fixes"

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:81 on 2023-10-05 22:02_

Haha. I guess the way I thought about it was that a given fix is either safe or not. The collection of fixes may thereby contain unsafe changes.

---

_@charliermarsh reviewed on 2023-10-05 22:02_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/check.rs`:130 on 2023-10-06 03:33_

Yeah you're right! Good catch — I misunderstood that call hierarchy.

---

_@zanieb reviewed on 2023-10-06 03:33_

---

_Merged by @zanieb on 2023-10-06 03:41_

---

_Closed by @zanieb on 2023-10-06 03:41_

---

_Branch deleted on 2023-10-06 03:41_

---
