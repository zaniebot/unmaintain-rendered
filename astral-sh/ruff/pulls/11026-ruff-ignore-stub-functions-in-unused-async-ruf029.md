```yaml
number: 11026
title: "[`ruff]` Ignore stub functions in `unused-async` (`RUF029`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2024-04-19T03:49:33Z
updated_at: 2024-04-19T04:03:53Z
url: https://github.com/astral-sh/ruff/pull/11026
synced_at: 2026-01-10T22:37:01Z
```

# [`ruff]` Ignore stub functions in `unused-async` (`RUF029`)

---

_Pull request opened by @charliermarsh on 2024-04-19 03:49_

## Summary

We should ignore methods that appear to be stubs, e.g.:

```python
async def foo() -> int: ...
```

Closes https://github.com/astral-sh/ruff/issues/11018.


---

_Label `bug` added by @charliermarsh on 2024-04-19 03:49_

---

_Review requested from @plredmond by @charliermarsh on 2024-04-19 03:49_

---

_Comment by @github-actions[bot] on 2024-04-19 04:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -103 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -29 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/disnake/utils.py#L522'>disnake/utils.py:522:11:</a> RUF029 Function `_assetbytes_to_base64_data` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/disnake/utils.py#L527'>disnake/utils.py:527:11:</a> RUF029 Function `_assetbytes_to_base64_data` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/autocomplete.py#L31'>examples/interactions/autocomplete.py:31:11:</a> RUF029 Function `languages_1` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/autocomplete.py#L46'>examples/interactions/autocomplete.py:46:11:</a> RUF029 Function `languages_2` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/converters.py#L16'>examples/interactions/converters.py:16:11:</a> RUF029 Function `clean_command` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/converters.py#L31'>examples/interactions/converters.py:31:11:</a> RUF029 Function `command_with_avatar` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/converters.py#L50'>examples/interactions/converters.py:50:11:</a> RUF029 Function `command_with_clsmethod` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/converters.py#L72'>examples/interactions/converters.py:72:11:</a> RUF029 Function `command_with_convmethod` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/injections.py#L125'>examples/interactions/injections.py:125:11:</a> RUF029 Function `implicit_injection` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/injections.py#L58'>examples/interactions/injections.py:58:11:</a> RUF029 Function `injected1` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/injections.py#L72'>examples/interactions/injections.py:72:11:</a> RUF029 Function `injected2` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/param.py#L23'>examples/interactions/param.py:23:11:</a> RUF029 Function `simple` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/param.py#L35'>examples/interactions/param.py:35:11:</a> RUF029 Function `other_types` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/examples/interactions/param.py#L47'>examples/interactions/param.py:47:11:</a> RUF029 Function `description` is declared `async`, but doesn't `await` or use `async` features.
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L106'>rasa/core/channels/console.py:106:11:</a> RUF029 Function `_get_user_input` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L111'>rasa/core/channels/console.py:111:11:</a> RUF029 Function `_get_user_input` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/cli/test_rasa_export.py#L243'>tests/cli/test_rasa_export.py:243:15:</a> RUF029 Function `close` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/channels/test_slack.py#L932'>tests/core/channels/test_slack.py:932:11:</a> RUF029 Function `fake_on_new_message` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/channels/test_telegram.py#L16'>tests/core/channels/test_telegram.py:16:11:</a> RUF029 Function `noop` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_broker.py#L39'>tests/core/test_broker.py:39:15:</a> RUF029 Function `connect` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_channels.py#L38'>tests/core/test_channels.py:38:11:</a> RUF029 Function `noop` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/00b90fa776154b76aec7f3c8075f53ac6d30d2e8/tests/providers/google/cloud/hooks/test_dataproc.py#L68'>tests/providers/google/cloud/hooks/test_dataproc.py:68:11:</a> RUF029 Function `mock_awaitable` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -65 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/staggered.pyi#L8'>stdlib/asyncio/staggered.pyi:8:11:</a> RUF029 Function `staggered_race` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L29'>stdlib/asyncio/streams.pyi:29:15:</a> RUF029 Function `open_connection` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L37'>stdlib/asyncio/streams.pyi:37:15:</a> RUF029 Function `start_server` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L48'>stdlib/asyncio/streams.pyi:48:15:</a> RUF029 Function `open_connection` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L57'>stdlib/asyncio/streams.pyi:57:15:</a> RUF029 Function `start_server` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L70'>stdlib/asyncio/streams.pyi:70:19:</a> RUF029 Function `open_unix_connection` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L73'>stdlib/asyncio/streams.pyi:73:19:</a> RUF029 Function `start_unix_server` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L77'>stdlib/asyncio/streams.pyi:77:19:</a> RUF029 Function `open_unix_connection` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/streams.pyi#L80'>stdlib/asyncio/streams.pyi:80:19:</a> RUF029 Function `start_unix_server` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L104'>stdlib/asyncio/subprocess.pyi:104:15:</a> RUF029 Function `create_subprocess_shell` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L135'>stdlib/asyncio/subprocess.pyi:135:15:</a> RUF029 Function `create_subprocess_exec` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L168'>stdlib/asyncio/subprocess.pyi:168:15:</a> RUF029 Function `create_subprocess_shell` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L199'>stdlib/asyncio/subprocess.pyi:199:15:</a> RUF029 Function `create_subprocess_exec` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L38'>stdlib/asyncio/subprocess.pyi:38:15:</a> RUF029 Function `create_subprocess_shell` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/subprocess.pyi#L70'>stdlib/asyncio/subprocess.pyi:70:15:</a> RUF029 Function `create_subprocess_exec` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L342'>stdlib/asyncio/tasks.pyi:342:15:</a> RUF029 Function `sleep` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L344'>stdlib/asyncio/tasks.pyi:344:15:</a> RUF029 Function `sleep` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L345'>stdlib/asyncio/tasks.pyi:345:15:</a> RUF029 Function `wait_for` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L350'>stdlib/asyncio/tasks.pyi:350:15:</a> RUF029 Function `sleep` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L352'>stdlib/asyncio/tasks.pyi:352:15:</a> RUF029 Function `sleep` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L353'>stdlib/asyncio/tasks.pyi:353:15:</a> RUF029 Function `wait_for` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L357'>stdlib/asyncio/tasks.pyi:357:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L361'>stdlib/asyncio/tasks.pyi:361:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L367'>stdlib/asyncio/tasks.pyi:367:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L371'>stdlib/asyncio/tasks.pyi:371:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L377'>stdlib/asyncio/tasks.pyi:377:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/tasks.pyi#L385'>stdlib/asyncio/tasks.pyi:385:15:</a> RUF029 Function `wait` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/asyncio/threads.pyi#L9'>stdlib/asyncio/threads.pyi:9:11:</a> RUF029 Function `to_thread` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stdlib/builtins.pyi#L1252'>stdlib/builtins.pyi:1252:15:</a> RUF029 Function `anext` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stubs/aiofiles/aiofiles/os.pyi#L100'>stubs/aiofiles/aiofiles/os.pyi:100:11:</a> RUF029 Function `rmdir` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python/typeshed/blob/d26f1251600d3eea24fcc035742a665249958fb1/stubs/aiofiles/aiofiles/os.pyi#L103'>stubs/aiofiles/aiofiles/os.pyi:103:11:</a> RUF029 Function `removedirs` is declared `async`, but doesn't `await` or use `async` features.
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/342a9bd5cd923bd423e2add867b60f2ccf7be62d/zerver/lib/push_notifications.py#L183'>zerver/lib/push_notifications.py:183:15:</a> RUF029 Function `err_func` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF029 | 103 | 0 | 103 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-19 04:03_

---

_Closed by @charliermarsh on 2024-04-19 04:03_

---

_Branch deleted on 2024-04-19 04:03_

---
