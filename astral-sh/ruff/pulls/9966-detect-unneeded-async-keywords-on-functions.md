```yaml
number: 9966
title: "Detect unneeded `async` keywords on functions"
type: pull_request
state: merged
author: plredmond
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: issue9951-spurious_async
created_at: 2024-02-13T00:00:59Z
updated_at: 2024-04-18T18:34:01Z
url: https://github.com/astral-sh/ruff/pull/9966
synced_at: 2026-01-12T15:55:30Z
```

# Detect unneeded `async` keywords on functions

---

_@plredmond_

## Summary

This change adds a rule to detect functions declared `async` but lacking any of `await`, `async with`, or `async for`. This resolves #9951.

## Test Plan

This change was tested by following https://docs.astral.sh/ruff/contributing/#rule-testing-fixtures-and-snapshots and adding positive and negative cases for each of `await` vs nothing, `async with` vs `with`, and `async for` vs `for`.

## Questions/todo

* [x] I'm unsure whether the rule should also check for `yield` appearing in functions. My understanding is that there has been several rounds of iteration in how python async is supposed to be done, and that `yield` is not considered current, and so I haven't added a case for that. Therefore an `async` function containing only `yield` will cause this rule to fire.
  * resolved by "async doesn't require yield" so the rule will fire when an async function's only async feature is yield
  * https://github.com/astral-sh/ruff/pull/9966#issuecomment-2044527147
  * https://github.com/astral-sh/ruff/pull/9966#pullrequestreview-2002255367
* [x] I was unsure how to access just the range of the function declaration (without the body), so the rule currently indicates the whole function declaration & body as the problem area.
* ~~[ ] Needs review by @zanieb~~

---

_Comment by @plredmond on 2024-02-13 00:05_

Sorry, I neglected formatting and linting of the rust code. I'll do that now.

---

_Comment by @github-actions[bot] on 2024-02-13 00:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+314 -0 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+65 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/disnake/ext/commands/core.py#L1722'>disnake/ext/commands/core.py:1722:19:</a> RUF029 Function `wrapper` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/disnake/utils.py#L522'>disnake/utils.py:522:11:</a> RUF029 Function `_assetbytes_to_base64_data` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/disnake/utils.py#L527'>disnake/utils.py:527:11:</a> RUF029 Function `_assetbytes_to_base64_data` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/basic_bot.py#L81'>examples/basic_bot.py:81:11:</a> RUF029 Function `on_ready` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/basic_voice.py#L136'>examples/basic_voice.py:136:11:</a> RUF029 Function `on_ready` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/converters.py#L77'>examples/converters.py:77:11:</a> RUF029 Function `on_ready` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/edit_delete.py#L52'>examples/edit_delete.py:52:11:</a> RUF029 Function `on_ready` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/guessing_game.py#L39'>examples/guessing_game.py:39:11:</a> RUF029 Function `on_ready` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/autocomplete.py#L25'>examples/interactions/autocomplete.py:25:11:</a> RUF029 Function `autocomplete_langs` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/autocomplete.py#L31'>examples/interactions/autocomplete.py:31:11:</a> RUF029 Function `languages_1` is declared `async`, but doesn't `await` or use `async` features.
... 55 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+168 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/botframework.py#L293'>rasa/core/channels/botframework.py:293:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/callback.py#L68'>rasa/core/channels/callback.py:68:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L106'>rasa/core/channels/console.py:106:11:</a> RUF029 Function `_get_user_input` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L111'>rasa/core/channels/console.py:111:11:</a> RUF029 Function `_get_user_input` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/facebook.py#L359'>rasa/core/channels/facebook.py:359:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/facebook.py#L363'>rasa/core/channels/facebook.py:363:19:</a> RUF029 Function `token_verification` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/hangouts.py#L295'>rasa/core/channels/hangouts.py:295:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/mattermost.py#L208'>rasa/core/channels/mattermost.py:208:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/rest.py#L138'>rasa/core/channels/rest.py:138:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/rocketchat.py#L148'>rasa/core/channels/rocketchat.py:148:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/slack.py#L503'>rasa/core/channels/slack.py:503:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/socketio.py#L198'>rasa/core/channels/socketio.py:198:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/socketio.py#L202'>rasa/core/channels/socketio.py:202:19:</a> RUF029 Function `connect` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/socketio.py#L220'>rasa/core/channels/socketio.py:220:19:</a> RUF029 Function `disconnect` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/telegram.py#L202'>rasa/core/channels/telegram.py:202:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/twilio.py#L126'>rasa/core/channels/twilio.py:126:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/twilio_voice.py#L233'>rasa/core/channels/twilio_voice.py:233:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/webexteams.py#L102'>rasa/core/channels/webexteams.py:102:19:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/jobs.py#L13'>rasa/core/jobs.py:13:11:</a> RUF029 Function `scheduler` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/run.py#L129'>rasa/core/run.py:129:15:</a> RUF029 Function `configure_async_logging` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/run.py#L292'>rasa/core/run.py:292:11:</a> RUF029 Function `create_connection_pools` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L1630'>rasa/core/training/interactive.py:1630:15:</a> RUF029 Function `ignore_404s` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/server.py#L1264'>rasa/server.py:1264:15:</a> RUF029 Function `tracker_predict` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/server.py#L1365'>rasa/server.py:1365:15:</a> RUF029 Function `unload_model` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/server.py#L1376'>rasa/server.py:1376:15:</a> RUF029 Function `get_domain` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/server.py#L403'>rasa/server.py:403:11:</a> RUF029 Function `authenticate` is declared `async`, but doesn't `await` or use `async` features.
... 142 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/apache/livy/hooks/test_livy.py#L508'>tests/providers/apache/livy/hooks/test_livy.py:508:19:</a> RUF029 Function `mock_fun` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/apache/livy/hooks/test_livy.py#L531'>tests/providers/apache/livy/hooks/test_livy.py:531:19:</a> RUF029 Function `mock_fun` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/apache/livy/hooks/test_livy.py#L556'>tests/providers/apache/livy/hooks/test_livy.py:556:19:</a> RUF029 Function `mock_fun` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/apache/livy/hooks/test_livy.py#L597'>tests/providers/apache/livy/hooks/test_livy.py:597:19:</a> RUF029 Function `mock_fun` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/apache/livy/hooks/test_livy.py#L616'>tests/providers/apache/livy/hooks/test_livy.py:616:19:</a> RUF029 Function `mock_fun` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/cncf/kubernetes/triggers/test_pod.py#L76'>tests/providers/cncf/kubernetes/triggers/test_pod.py:76:15:</a> RUF029 Function `mock_read_namespaced_pod` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/google/cloud/hooks/test_cloud_batch.py#L322'>tests/providers/google/cloud/hooks/test_cloud_batch.py:322:19:</a> RUF029 Function `_get_job` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/google/cloud/hooks/test_dataproc.py#L68'>tests/providers/google/cloud/hooks/test_dataproc.py:68:11:</a> RUF029 Function `mock_awaitable` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/google/cloud/triggers/test_cloud_batch.py#L137'>tests/providers/google/cloud/triggers/test_cloud_batch.py:137:19:</a> RUF029 Function `_mock_job` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/apache/airflow/blob/4a288460a501364cd228c4b2f7a24401c4c4e992/tests/providers/google/cloud/triggers/test_cloud_run.py#L109'>tests/providers/google/cloud/triggers/test_cloud_run.py:109:19:</a> RUF029 Function `_mock_operation` is declared `async`, but doesn't `await` or use `async` features.
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+65 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/staggered.pyi#L8'>stdlib/asyncio/staggered.pyi:8:11:</a> RUF029 Function `staggered_race` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L29'>stdlib/asyncio/streams.pyi:29:15:</a> RUF029 Function `open_connection` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L37'>stdlib/asyncio/streams.pyi:37:15:</a> RUF029 Function `start_server` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L48'>stdlib/asyncio/streams.pyi:48:15:</a> RUF029 Function `open_connection` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L57'>stdlib/asyncio/streams.pyi:57:15:</a> RUF029 Function `start_server` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L70'>stdlib/asyncio/streams.pyi:70:19:</a> RUF029 Function `open_unix_connection` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L73'>stdlib/asyncio/streams.pyi:73:19:</a> RUF029 Function `start_unix_server` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L77'>stdlib/asyncio/streams.pyi:77:19:</a> RUF029 Function `open_unix_connection` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/streams.pyi#L80'>stdlib/asyncio/streams.pyi:80:19:</a> RUF029 Function `start_unix_server` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/asyncio/subprocess.pyi#L104'>stdlib/asyncio/subprocess.pyi:104:15:</a> RUF029 Function `create_subprocess_shell` is declared `async`, but doesn't `await` or use `async` features.
... 55 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/16a08a8b6b35f11cdc27410ef80043a2b8025563/zerver/lib/push_notifications.py#L183'>zerver/lib/push_notifications.py:183:15:</a> RUF029 Function `err_func` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/zulip/zulip/blob/16a08a8b6b35f11cdc27410ef80043a2b8025563/zerver/lib/push_notifications.py#L188'>zerver/lib/push_notifications.py:188:15:</a> RUF029 Function `make_apns` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/zulip/zulip/blob/16a08a8b6b35f11cdc27410ef80043a2b8025563/zerver/tornado/event_queue.py#L687'>zerver/tornado/event_queue.py:687:11:</a> RUF029 Function `setup_event_queue` is declared `async`, but doesn't `await` or use `async` features.
+ <a href='https://github.com/zulip/zulip/blob/16a08a8b6b35f11cdc27410ef80043a2b8025563/zerver/tornado/views.py#L42'>zerver/tornado/views.py:42:15:</a> RUF029 Function `wrapped` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF029 | 314 | 314 | 0 | 0 | 0 |

</p>
</details>




---

_@zanieb reviewed on 2024-02-13 00:48_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:4 on 2024-02-13 00:48_

Usually we'd indicate this with something like 

```suggestion
async def pass_case(): # OK: awaits a coroutine
```

---

_@zanieb reviewed on 2024-02-13 00:48_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:4 on 2024-02-13 00:48_

It's not really standardized though, so it's okay either way.

---

_Comment by @zanieb on 2024-02-13 00:52_

The `rasa` ecosystem checks look like a bunch of false positives due to methods that are overriding an [abstract method which must be async](https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L241-L263). This common enough that we should attempt to avoid it, but it requires multifile analysis in most cases which we do not yet support yet. There are some other issues like this, but I can't recall them — perhaps @charliermarsh knows. We could consider just not applying this to methods in classes with base classes for now?

---

_Comment by @charliermarsh on 2024-02-13 00:54_

Ah yeah, the thing we often do there (at the very least) is check if the method has an `@override` decorator (can grep for `is_override`), since that's used to hint to static analysis tools that the method doesn't have control over its own signature. So users at least have a way to opt-out of these kinds of rules entirely for methods that override a parent method. I would be fine omitting this entirely for classes with base classes though (or even classes at all?).

---

_Comment by @zanieb on 2024-02-13 00:55_

Here's another interesting edge-case false positive https://github.com/zulip/zulip/blob/35098f49597895718343091881fbd6198bd2022d/zerver/tornado/views.py#L35 — this one I'm less sure we can/should do anything about.

---

_@zanieb reviewed on 2024-02-13 01:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:16 on 2024-02-13 01:00_

I think we'll want to include an example here too

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:49 on 2024-02-13 01:02_

Do you mind running `ruff format` over this file, just to standardize the comment spacing, lines between functions, etc.? (We don't _always_ run it over fixtures, since fixtures sometimes include intentionally-odd comments and syntax, but this one doesn't, so seems like a nice improvement.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:370 on 2024-02-13 01:03_

For newer rules, we're trying to pass the entire node (like `function_def`, above), to ensure that callers can't pass in "incorrect" data. Do you mind revising the signature?

---

_@zanieb reviewed on 2024-02-13 01:04_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:16 on 2024-02-13 01:04_

You can also probably say "will limit the contexts" instead of "may artificially limit".

There's also something to be said about it actually being _bad_ to say that your function is async without it containing a yield as it's breaking the fundamental promise of the async label. Some more context on this idea [in the trio documentation](https://trio.readthedocs.io/en/stable/reference-core.html#checkpoints). I'm not sure if we can get into that without confusing users though.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:80 on 2024-02-13 01:04_

Ahh, if you have the `function_def: &ast::StmtFunctionDef` as an argument to this method, you can use `function_def.identifier()` to get _just_ the range of the function name (per your PR summary).

---

_@charliermarsh reviewed on 2024-02-13 01:05_

Nice!

---

_Comment by @zanieb on 2024-02-13 01:15_

Perhaps @AlexWaygood can elucidate whether or not this should trigger for functions with `yield`.

https://peps.python.org/pep-0525 may be helpful.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:32 on 2024-02-13 16:28_

Nit: I first assumed that the visitor only searches for `yield` expressions because of its name and the variable's name. Maybe consider renaming the visitor and the variable field. 
 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:35 on 2024-02-13 16:30_

This visitor is somewhat expensive because it has to traverse the entire sub-tree of the function to the end, even when it has already found a `yield` or `await`able. 

You could consider implementing `PreorderVisitor` instead. It differs from `Visitor` that it visits the fields in the source order instead of semantical ordering (not important in this case) and that it has a `enter_node` function which can either return `TraverseSignal::Traverse` to continue traversing or `TraverseSignal::Skip` to stop traversing, which we could return here when `found_yield` is `true`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:70 on 2024-02-13 16:30_

Nit: You could derive `Default` for `YieldingExprVisitor` so that this becomes
```suggestion
        let mut visitor = YieldingExprVisitor::default();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-02-13 16:33_

This is a Python noob question from a JS engineer. How does Python handle default arguments? Can you use `await` in a default initializer? Does this count as async call (because it pauses the function execution) or not because defaults are initialized once during declaration and not when calling the function. 

---

_@MichaReiser reviewed on 2024-02-13 16:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-02-13 16:40_

That's a fun question that I'd never considered before.

`await` isn't allowed at the top-level in a usual Python file (only inside async functions), so normally this isn't really something that would ever come up, since function defaults are evaluated in the scope the function is in rather than the scope the function creates:

```pycon
>>> import asyncio
>>> async def foo(x=await asyncio.sleep(0.1)): ...
...
  File "<stdin>", line 1
SyntaxError: 'await' outside function
```

But theoretically you could use `await` in a function default argument if it was an inner function inside an async function:

```pycon
>>> import asyncio
>>> async def foo():
...     def bar(x=await asyncio.sleep(0.1)): ...
...     return bar
...
>>> bar = asyncio.run(foo())
>>> import inspect
>>> inspect.iscoroutinefunction(bar)
False
>>> type(bar)
<class 'function'>
```

As you can see from that REPL snippet, using `await` inside the default argument for the inner function doesn't impact whether or not the inner function is considered to be a coroutine function, since the `await` in the function's default argument is evaluated before the function has even finished being defined.

---

_@AlexWaygood reviewed on 2024-02-13 16:40_

---

_Comment by @AlexWaygood on 2024-02-13 16:48_

> Perhaps @AlexWaygood can elucidate whether or not this should trigger for functions with `yield`.
> 
> [peps.python.org/pep-0525](https://peps.python.org/pep-0525) may be helpful.

Yeah, async functions that contain `yield` are async generators that can be used in `async for` loops by other async functions. So I don't think ruff should emit this diagnostic for async functions that have a `yield` in them:

```pycon
>>> import asyncio
>>> async def x():
...     yield 42
...     yield 56
...
>>> x
<function x at 0x00000274CDC52200>
>>> x()
<async_generator object x at 0x00000274CC36BE20>
>>> async def y():
...     async for number in x():
...         print(number)
...
>>> asyncio.run(y())
42
56
```

---

_@MichaReiser reviewed on 2024-02-13 17:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-02-13 17:10_

Thanks for the detailed explanation.  

I tested if `foo` is considered a co-routine and that seems to be the case. Do you know if that's only because the function is marked as `async`? 

```python
>>> import asyncio
>>> async def foo():
...     def bar(x=await asyncio.sleep(0.1)): ...
...     return bar
... 
>>> import inspect
>>> inspect.iscoroutinefunction(foo)
True
```

I'm trying to understand if the use of `async` on `foo` is something this lint rule should warn about or not. 

---

_@AlexWaygood reviewed on 2024-02-13 17:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-02-13 17:16_

`foo` is considered a coroutine function because it's a function that returns a coroutine ;)

```pycon
>>> foo
<function foo at 0x00000274CBC2A160>
>>> foo()
<coroutine object foo at 0x00000274CDC19300>
```

But the short answer to your question is "yes" -- all `async def` functions are "coroutine functions" -- functions that return coroutine objects that can then be `await`ed inside other coroutine functions.

The use of `async` on `foo` should not be something that this lint rule warn about, because the `await` expression in the default for the `x` parameter of `bar` is executed within the scope of `foo`. So removing the `async` on `foo` would mean that that `await` expression would no longer be found in an async context (== illegal syntax).

---

_@MichaReiser reviewed on 2024-02-13 17:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-02-13 17:20_

Perfect. That's what this lint rule should support already today. It might be worth to copy the example as a test case.

---

_@plredmond reviewed on 2024-02-13 18:37_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:32 on 2024-02-13 18:37_

I originally used "yielding_expression". Does that name seem acceptable?

---

_@plredmond reviewed on 2024-02-13 18:38_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:70 on 2024-02-13 18:38_

Yes, I followed that pattern in an earlier draft when the field was a vector of yielding-expression-TextRanges, but reverted to explicit construction when I changed it to a boolean. Is the convention to always prefer default where applicable?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:70 on 2024-02-13 18:40_

There's no strong convention for private types. For public types, yes, I would say we prefer using `::default` or `::new` to encapsulate the initialization logic. Here I would do it because it's less to type but that's also why it is a Nit. It's up to you what you prefer.

---

_@MichaReiser reviewed on 2024-02-13 18:40_

---

_@MichaReiser reviewed on 2024-02-13 18:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:32 on 2024-02-13 18:46_

Good question. `yield` or `yielding` is too close to the `yield` expression semantics. I would probably be explicit and call it `awaits_or_yields` or `found_await_or_yield`


---

_Comment by @MichaReiser on 2024-02-13 18:48_

I think there's one issue that we might need to look into and it's that the visitor will cross function boundaries when searching for `await` and `yield`, resulting in false negatives

```python
async def unnecessary_async():
	async def inner():
		await x

async def unnecessary_async2():
	class Inner:
		async def inner():	
			await x


 ```

I don't think the rule flags `unnecessary_async` or `unnecessary_async2`. 


---

_Comment by @plredmond on 2024-02-13 18:50_

Thanks for your review! I've looked over the comments above and can work on the changes later today. I need to devote this morning to a different task.

> Ah yeah, the thing we often do there (at the very least) is check if the method has an `@override` decorator (can grep for `is_override`), since that's used to hint to static analysis tools that the method doesn't have control over its own signature. So users at least have a way to opt-out of these kinds of rules entirely for methods that override a parent method. I would be fine omitting this entirely for classes with base classes though (or even classes at all?).

@charliermarsh should I omit this check in both the cases (1) in the context of a class or (2) when the override decorator is present? Based on your description, it seems preferable to limit case (1) to classes with a base class, since they're actually likely to be overriding things, though.

---

_Comment by @plredmond on 2024-02-13 19:05_

> I don't think the rule flags `unnecessary_async` or `unnecessary_async2`.

@MichaReiser These examples make the problem a bit harder. :slightly_smiling_face: I need to check if the visitor has a callback for when an inner-function ends; I could use that to track which scope I'm in when detecting yielding-expressions to attribute them correctly.

---

_@plredmond reviewed on 2024-02-13 19:07_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:35 on 2024-02-13 19:07_

Combined with your point about nested-functions (e.g. `unnecessary_async2`) from [this comment](https://github.com/astral-sh/ruff/pull/9966#issuecomment-1942180352), I need to rethink my approach a bit.

---

_Comment by @MichaReiser on 2024-02-13 20:36_

@plredmond I think there are only two cases where this is relevant: classes and functions. You could extend your visitor to handle `visit_stmt explicitly` and traverse classes and functions manually (don't call `walk_stmt`). This allows you to skip traversing the body (you still want to traverse decorators and the function header)

---

_Comment by @plredmond on 2024-03-05 05:56_

Hi! Sorry to let this PR languish. I have a paper deadline just before I start my astral internship, so I have to put this on ice until then.

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:28_

---

_Comment by @MichaReiser on 2024-04-05 10:29_

IMO, `unused-async` is easier to understand than spurious (which I mainly associate with threading). It also fits well into other rules that catch "unused" syntax. 

---

_Comment by @AlexWaygood on 2024-04-09 09:16_

> Yeah, async functions that contain `yield` are async generators that can be used in `async for` loops by other async functions. So I don't think ruff should emit this diagnostic for async functions that have a `yield` in them:

So, I chatted to @carljm about this offline... and he persuaded me that the rationale I gave in https://github.com/astral-sh/ruff/pull/9966#issuecomment-1941990446 for excluding async generators from this check _basically_ makes no sense :-)

It's true that removing the `async` keyword from a function with a `yield` statement in it will mean that it will become a sync generator function rather than an async generator function. That means that it will only be usable in `for` loops, not `async for` loops, which means callsites of the function will have to be rewritten to use `for` loops rather than `async for` loops -- potentially a sweeping and disruptive refactor. _However_... that's not really materially different from the changes this rule recommends to non-generator functions. Removing redundant `async` keywords from non-generator functions that don't have any `await`s in them will _also_ necessitate callsites being refactored: you'll have to do this refactor everywhere:

```diff
- x = await function_in_question()
+ x = function_in_question()
```

As such, I'm now persuaded that there's no reason to exclude async generator functions from this check. (Thanks @carljm!)

---

_Comment by @plredmond on 2024-04-15 16:45_

Picking this up today. Sorry for the delay.

---

_Comment by @plredmond on 2024-04-15 19:19_

> IMO, `unused-async` is easier to understand than spurious (which I mainly associate with threading). It also fits well into other rules that catch "unused" syntax.

Saving this rename for last b/c it'll disrupt the above comments on diffs.

---

_@plredmond reviewed on 2024-04-15 21:34_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:80 on 2024-04-15 21:34_

Is it desirable for the error message to only indicate the name, given that the problem is to the left of the name (the use of the `async` keyword) and to the right of it (the function body lacking a reason for the use of the `async` keyword)?

---

_Comment by @plredmond on 2024-04-15 21:49_

I've made a bunch of smaller/cosmetic changes per the comments above. Additionally, I've added four new test cases (two pass, and two fail). **Note, that the tests do not pass right now, because i need to implement the changes.** The remaining work is:

* make the new test cases pass
  * ~~change the rule to *not* fire when an `async` function contains a `yield`~~ (this was switched to fire, per @carljm & @AlexWaygood)
  * change the rule to fire when an `async` function using no async features contains an `async` function which does
  * change the rule to fire when an `async` function using no async features contains a class with an `async` function which does
  * skip applying this rule to class methods entirely and make an issue to track doing so in a limited way (per @zanieb)
* fix the visitor to stop traversing early when it finds a problem
* rename "spurious async" to "unused async" throughout

Could somebody take a look at the two pass-cases and fail-cases that I just added and confirm that this is the desired behavior?

---

_@plredmond reviewed on 2024-04-15 21:54_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:4 on 2024-04-15 21:54_

I've made this change below.

---

_@plredmond reviewed on 2024-04-15 21:55_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:16 on 2024-04-15 21:55_

I've made this change below.

---

_@plredmond reviewed on 2024-04-15 21:56_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:49 on 2024-04-15 21:56_

Formatted!

---

_@plredmond reviewed on 2024-04-15 21:56_

---

_Review comment by @plredmond on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:370 on 2024-04-15 21:56_

I've made this change below.

---

_@plredmond reviewed on 2024-04-15 21:56_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:32 on 2024-04-15 21:56_

I've made this rename change below.

---

_@plredmond reviewed on 2024-04-15 21:57_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:70 on 2024-04-15 21:57_

I've made this change below.

---

_@plredmond reviewed on 2024-04-15 21:59_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:65 on 2024-04-15 21:59_

I've added a test case (which should pass, without causing the rule to fire) where an async function defines an inner function by setting the inner function's default argument value to the result of an await expression, below. Implementing it is todo.

---

_Review comment by @carljm on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:45 on 2024-04-15 22:02_

For the reasons mentioned in  @AlexWaygood's most recent comment, this case should fire `RUF029` -- yielding a value does not require an async function.

---

_Review comment by @carljm on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029.py`:56 on 2024-04-15 22:03_

and "or yield" should be removed from the comments here

---

_Review comment by @carljm on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:38 on 2024-04-15 22:04_

and "yield or" should be removed from this error message

---

_@carljm reviewed on 2024-04-15 22:05_

The test cases look right to me, apart from the inline comments about `yield`

---

_Comment by @charliermarsh on 2024-04-16 02:19_

Many reviewers on here -- who wants to own the final review + approval? \cc @carljm @AlexWaygood @zanieb @MichaReiser 

---

_@plredmond reviewed on 2024-04-16 07:03_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/spurious_async.rs`:35 on 2024-04-16 07:03_

Implemented this below.

---

_Comment by @plredmond on 2024-04-16 07:06_

Sorry for the excessive number of commits here. I've had a bit of churn in the test cases because I found them confusing and reorganized them. The real work is these three commits:

* [switch to preorder visitor](https://github.com/astral-sh/ruff/pull/9966/commits/1cb7765b62046fe32050d08671e08d32a8839a4c), [1cb7765](https://github.com/astral-sh/ruff/pull/9966/commits/1cb7765b62046fe32050d08671e08d32a8839a4c)
* [skip the rest of traversal after finding an async/await](https://github.com/astral-sh/ruff/pull/9966/commits/41f0ac5bfaaf5ca939be84e003f9fe7d008b7b9c), [41f0ac5](https://github.com/astral-sh/ruff/pull/9966/commits/41f0ac5bfaaf5ca939be84e003f9fe7d008b7b9c)
* [don't traverse the body of inner-functions or inner-classes](https://github.com/astral-sh/ruff/pull/9966/commits/b714e808d8f403bdecbaa9a16df0f4b548261d33), [b714e80](https://github.com/astral-sh/ruff/pull/9966/commits/b714e808d8f403bdecbaa9a16df0f4b548261d33)

I believe this covers the test cases @MichaReiser shared [above](https://github.com/astral-sh/ruff/pull/9966#issuecomment-1942180352) with the nested asyncs using his [suggested](https://github.com/astral-sh/ruff/pull/9966#issuecomment-1942437370) solution (partially reimplementing traversal of function defs and class defs), as well as his [suggested](https://github.com/astral-sh/ruff/pull/9966#discussion_r1488203954) optimization to stop traversing once some evidence that the function is actually async has been found.

However, this _doesn't_ cover @charliermarsh's [suggestion](https://github.com/astral-sh/ruff/pull/9966#issuecomment-1939953539) to just omit checking methods in classes. I'm not sure how to approach that (and the test case is commented out). The rule is currently written in terms of a function definition; to omit the rule for methods, we'd need to know whether that function definition is a method. I didn't see a way to do that in `analyze/statement.rs`.

Except for @charliermarsh's suggestion, the remaining work is just to rename spurious->unused.

---

_Comment by @plredmond on 2024-04-16 07:16_

Ok, I think everything is done except for this:

> However, this _doesn't_ cover @charliermarsh's [suggestion](https://github.com/astral-sh/ruff/pull/9966#issuecomment-1939953539) to just omit checking methods in classes. I'm not sure how to approach that (and the test case is commented out). The rule is currently written in terms of a function definition; to omit the rule for methods, we'd need to know whether that function definition is a method. I didn't see a way to do that in `analyze/statement.rs`.

[EDIT: I'll look at the test failure in the morning. I don't see one when I test locally.]

---

_Comment by @MichaReiser on 2024-04-16 07:51_

@plredmond I'm not entirely sure if that's what you mean but you can test if you're inside a class by using 

```
    // Assignments in class bodies are attributes (e.g., `x = x` assigns `x` to `self.x`, and thus
    // is not a self-assignment).
    if checker.semantic().current_scope().kind.is_class() {
        return;
    }
```

You can also use the `kind` to retrieve more information about the enclosing class (e.g. if it has any base class or whatnot)

---

_Comment by @kkom on 2024-04-16 09:38_

Came across this PR when looking at #9951 – thank you for adding this rule, very excited about it!

Regarding the latest discussion – I'm curious why not omit it for method marked with `@override` and continue linting methods? This is analogous to how https://docs.astral.sh/ruff/rules/no-self-use/ works – and that rule has worked really well for us in that way.

---

_Comment by @zanieb on 2024-04-16 14:16_

@kkom we're just cutting scope from the initial rule. We could consider it in the future.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unused_async.rs`:38 on 2024-04-16 16:42_

Nit: wrap await in backticks here for consistency with async?

---

_Comment by @charliermarsh on 2024-04-16 16:42_

This looks good to me, but I suggest adding the snippet that Micha posted above.

---

_@charliermarsh reviewed on 2024-04-16 16:42_

---

_@plredmond reviewed on 2024-04-16 16:55_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/ruff/rules/unused_async.rs`:38 on 2024-04-16 16:55_

Implemented below.

---

_Comment by @charliermarsh on 2024-04-16 17:05_

@plredmond - Try running `cargo dev generate-all` -- you need to regenerate the JSON Schema since we added a new rule.

---

_Comment by @charliermarsh on 2024-04-16 17:05_

(I believe that's the cause of your failing test.)

---

_Comment by @plredmond on 2024-04-16 17:08_

```sh-session
$ cargo dev generate-all
...
$ git diff
diff --git a/ruff.schema.json b/ruff.schema.json
index 871cce40b..90582fc67 100644
--- a/ruff.schema.json
+++ b/ruff.schema.json
@@ -3951,4 +3951,4 @@
       ]
     }
   }
-}
+}
\ No newline at end of file


---

_Comment by @charliermarsh on 2024-04-16 17:09_

Is your editor adding a newline?

---

_Comment by @plredmond on 2024-04-16 17:09_

I also tried fetching and merging in the latest `main` in. Still no success. Strangely, I'm not seeing this locally.

I'll look at the CI output.

---

_Comment by @plredmond on 2024-04-16 17:10_

> Is your editor adding a newline?

It was modified by `cargo dev generate-all` afaik. My editor is vim and I didn't modify that file.

---

_Comment by @charliermarsh on 2024-04-16 17:11_

We do _expect_ the Schema to change on this branch.

---

_Comment by @plredmond on 2024-04-16 17:14_

I was wrong. I *did* edit it, back in feb, and that seems to have lead to the CI complaining about the newline. It should be fixed now.

---

_@charliermarsh reviewed on 2024-04-16 17:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:965 on 2024-04-16 17:16_

Sorry, annoying, but do you mind flipping the order here so that this rule comes last (after `RUF028`)?

---

_@charliermarsh approved on 2024-04-16 17:16_

---

_@plredmond reviewed on 2024-04-16 17:18_

---

_Review comment by @plredmond on `crates/ruff_linter/src/codes.rs`:965 on 2024-04-16 17:18_

Done

---

_Merged by @plredmond on 2024-04-16 17:32_

---

_Closed by @plredmond on 2024-04-16 17:32_

---

_Branch deleted on 2024-04-16 17:32_

---

_Label `rule` added by @dhruvmanila on 2024-04-18 18:34_

---

_Label `preview` added by @dhruvmanila on 2024-04-18 18:34_

---
