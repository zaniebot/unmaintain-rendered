```yaml
number: 8676
title: Fix instability with await fluent style
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix-8644
created_at: 2023-11-14T14:33:13Z
updated_at: 2023-11-17T17:24:21Z
url: https://github.com/astral-sh/ruff/pull/8676
synced_at: 2026-01-10T23:40:55Z
```

# Fix instability with await fluent style

---

_Pull request opened by @konstin on 2023-11-14 14:33_

Fix an instability where await was followed by a breaking fluent style expression:

```python
test_data = await (
    Stream.from_async(async_data)
    .flat_map_async()
    .map()
    .filter_async(is_valid_data)
    .to_list()
)
```

Note that this technically a minor style change (see ecosystem check)

---

_Label `formatter` added by @konstin on 2023-11-14 14:33_

---

_Marked ready for review by @konstin on 2023-11-14 14:33_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 14:33_

This is arguably correct for ruff

---

_@konstin reviewed on 2023-11-14 14:33_

---

_Comment by @github-actions[bot] on 2023-11-14 14:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+9 -5 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+9 -5 lines across 1 file)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L880'>rasa/server.py~L880</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
-                    conversation_id
+                tracker = await (
+                    app.ctx.agent.processor.fetch_tracker_and_update_session(
+                        conversation_id
+                    )
                 )
 
                 output_channel = _get_output_channel(request, tracker)
```
<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L931'>rasa/server.py~L931</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
-                    conversation_id
+                tracker = await (
+                    app.ctx.agent.processor.fetch_tracker_and_update_session(
+                        conversation_id
+                    )
                 )
                 output_channel = _get_output_channel(request, tracker)
                 if intent_to_trigger not in app.ctx.agent.domain.intents:
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+9 -5 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+9 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L880'>rasa/server.py~L880</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
-                    conversation_id
+                tracker = await (
+                    app.ctx.agent.processor.fetch_tracker_and_update_session(
+                        conversation_id
+                    )
                 )
 
                 output_channel = _get_output_channel(request, tracker)
```
<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L931'>rasa/server.py~L931</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
-                    conversation_id
+                tracker = await (
+                    app.ctx.agent.processor.fetch_tracker_and_update_session(
+                        conversation_id
+                    )
                 )
                 output_channel = _get_output_channel(request, tracker)
                 if intent_to_trigger not in app.ctx.agent.domain.intents:
```

</p>
</details>




---

_Review comment by @zanieb on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 14:47_

This seems like really weird formatting. Why is this correct?

---

_@zanieb reviewed on 2023-11-14 14:47_

---

_@konstin reviewed on 2023-11-14 14:50_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 14:50_

Note that we don't expand, we just keep
```python
async def main():
    await asyncio.sleep(1)  # Hello
    await (
        asyncio.sleep(1)  # Hello
    )
```
as-is

---

_@zanieb reviewed on 2023-11-14 14:53_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 14:53_

Okay that's good to know. I guess that's okay. A little weird but fits with previous choices?

---

_@charliermarsh reviewed on 2023-11-14 15:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 15:50_

Should this be `IfBreaksOrIfRequired`?

---

_@charliermarsh reviewed on 2023-11-14 15:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 15:51_

Why does this work with `IfBreaks`?

---

_@charliermarsh reviewed on 2023-11-14 15:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 15:52_

In yield we use `Parenthesize::Optional`...

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__remove_await_parens.py.snap`:110 on 2023-11-14 15:58_

I tried that too for that same reason, but it introduced new black deviations:
```diff
 # Remove brackets for short coroutine/task
 async def main():
-    await asyncio.sleep(1)
+    await (asyncio.sleep(1))
 
 
 async def main():
-    await asyncio.sleep(1)
+    await (asyncio.sleep(1))
 
 
 async def main():
-    await asyncio.sleep(1)
+    await (asyncio.sleep(1))
```

---

_@konstin reviewed on 2023-11-14 15:58_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 16:01_

That one would regress `asyncio.gather` 
```diff
 # Long lines
 async def main():
-    await asyncio.gather(
-        asyncio.sleep(1),
-        asyncio.sleep(1),
-        asyncio.sleep(1),
-        asyncio.sleep(1),
-        asyncio.sleep(1),
-        asyncio.sleep(1),
-        asyncio.sleep(1),
+    await (
+        asyncio.gather(
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+            asyncio.sleep(1),
+        )
     )
```
(i'm happy too implement a different solution btw, this was just the solution that i found to work best by trial and error)

---

_@konstin reviewed on 2023-11-14 16:01_

---

_@charliermarsh reviewed on 2023-11-14 16:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 16:39_

Ahh yeah, I agree that that's worse. I feel like I need to have a better understanding of why these `Parenthesize` values are leading to these changes before I'd be comfortable approving. Why does `IfBreaks` break that way? Will using `IfBreaks` ever lead to syntax errors?

---

_@konstin reviewed on 2023-11-14 18:19_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 18:19_

No, that's handled by `OptionalParentheses::Multiline`

---

_@konstin reviewed on 2023-11-14 18:21_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 18:21_

I'll make some better examples why we get this behavior

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-14 18:25_

Thank you, I'm trying to make sure I play Micha and really understand the impact of formatting changes before approving.

---

_@charliermarsh reviewed on 2023-11-14 18:25_

---

_@konstin reviewed on 2023-11-15 09:16_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_await.rs`:23 on 2023-11-15 09:16_

From the perspective of the await, it needs `OptionalParentheses::Never` because the child is already parenthesized, and we're preferring those parentheses over adding them around the await. For the await's child, we have four options:

* Optional: Add parentheses if the child breaks, which we want e.g. for `asyncio.gather`. Keeps the child's parentheses from the source code, which we want to remove.
* IfBreaks: Add parentheses if the child breaks, which we want e.g. for `asyncio.gather`. Discards child parentheses, which we want.
* IfRequired: Discards child parentheses, which we want. Also discards them when the child breaks but has its own inner parentheses, which would lead to e.g.
    ```python
    await asyncio.gather(
        fut1,
        fut2,
    )
    ```
    , which don't want.
* IfBreaksOrIfRequired: Special case for return type annotations.
 
I added comments to the parentheses types to make them better understandable in the future

---

_Review requested from @charliermarsh by @konstin on 2023-11-17 09:07_

---

_@charliermarsh approved on 2023-11-17 17:19_

---

_Merged by @charliermarsh on 2023-11-17 17:24_

---

_Closed by @charliermarsh on 2023-11-17 17:24_

---

_Branch deleted on 2023-11-17 17:24_

---
