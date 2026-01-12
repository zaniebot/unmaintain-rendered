```yaml
number: 8293
title: Implement multiline dictionary and list hugging for preview style
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: charlie/hug
created_at: 2023-10-28T03:19:35Z
updated_at: 2023-12-05T18:08:58Z
url: https://github.com/astral-sh/ruff/pull/8293
synced_at: 2026-01-10T23:40:55Z
```

# Implement multiline dictionary and list hugging for preview style

---

_Pull request opened by @charliermarsh on 2023-10-28 03:19_

## Summary

This PR implement's Black's new single-argument hugging for lists, sets, and dictionaries under preview style.

For example, this:

```python
foo(
    [
        1,
        2,
        3,
    ]
)
```

Would instead now be formatted as:

```python
foo([
    1,
    2,
    3,
])
```

A couple notes:

- This doesn't apply when the argument has a magic trailing comma.
- This _does_ apply when the argument is starred or double-starred.
- We don't apply this when there are comments before or after the argument, though Black does in some cases (and moves the comments outside the call parentheses).

It doesn't say it in the originating PR (https://github.com/psf/black/pull/3964), but I think this also applies to parenthesized expressions? At least, it does in my testing of preview vs. stable, though it's possible that behavior predated the linked PR.

See: #8279.

## Test Plan

Before:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99963 |             10596 |               146 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               322 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                21 |

After:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99963 |             10596 |               146 |
| poetry         |           0.96215 |               317 |                34 |
| transformers   |           0.99967 |              2657 |               322 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                21 |

---

_Label `formatter` added by @charliermarsh on 2023-10-28 03:19_

---

_Label `preview` added by @charliermarsh on 2023-10-28 03:19_

---

_Comment by @github-actions[bot] on 2023-10-28 03:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_Comment by @MichaReiser on 2023-10-28 05:41_

Hmm, does it also apply for maybe parenthesize expressions? What parts are missing after this PR? 

---

_Comment by @charliermarsh on 2023-10-28 11:20_

I think perhaps in `parenthesize_if_expands`? Will test that.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-28 21:07_

---

_Review requested from @konstin by @charliermarsh on 2023-10-28 21:07_

---

_Comment by @charliermarsh on 2023-10-28 21:09_

(Added `parenthesize_if_expands`.)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/hug.py`:85 on 2023-10-30 00:29_

Can we add a test verifying that calls with more than one argument are not hugged?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:137 on 2023-10-30 00:31_

Do your tests cover the `format_with_parentheses_comments` case? If not, could you add tests for it as well?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:1202 on 2023-10-30 00:32_

Interesting that tuples are excluded. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:1132 on 2023-10-30 00:33_

Should we move the preview check into `is_huggable` to avoid the repetition?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:429 on 2023-10-30 00:38_

Nit: this becomes somewhat repetitive. Is it worth introducing a `parenthesize_expression_if_expands` helper?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:210 on 2023-10-30 00:40_

Can we add a test case for `magic-trailing-comma=ignore` covering this check

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:422 on 2023-10-30 00:44_

Can you verify if it is necessary to set `indent` for all these cases? 

At least for `if [aaa, b]`, `can_omit_optional_parentheses` should always return `true` and I think the same is true for all other parenthesized expressions. Meaning, this change seems unnecessary. 

Can you test which changes are necessary and a) either remove the change or b) add (if not already) test cases proving that they are necessary



---

_@MichaReiser reviewed on 2023-10-30 00:45_

Please update your PR summary with the new similarity index and can we get results on our new ecosystem check?




---

_Comment by @charliermarsh on 2023-10-30 00:51_

> Please update your PR summary with the new similarity index and can we get results on our new ecosystem check?

Just to be clear, we expect no change in the similarity index -- and I don't know if the ecosystem checks include preview style yet, do they @zanieb? The PR shows no change.

---

_Comment by @MichaReiser on 2023-10-30 00:55_

> > Please update your PR summary with the new similarity index and can we get results on our new ecosystem check?
> 
> Just to be clear, we expect no change in the similarity index -- and I don't know if the ecosystem checks include preview style yet, do they @zanieb? The PR shows no change.

Is it because the projects included in our similarity index using preview-style don't use the new formatting yet?

It seems we need to handle match/case as well

[Black](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFEAGRdAD2IimZxl1N_WmBasjhgIoo1dFgnEH19ELkVU7Zs1tOr_y7iwpTk9TJX2EgkSoZ2FqGN6L3khtFjMSEjkmwuAMiQm0Ik9Ysf2H26EPPHEyOszlfB3ByyaKb5HTA0rJfyte-cfQAACmM1itE96ooAAYABxQIAAO8jkqKxxGf7AgAAAAAEWVo=)

And can you add tests for delete:  `delete ([aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa, bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb])`


---

_Comment by @konstin on 2023-10-30 10:12_

> and I don't know if the ecosystem checks include preview style yet,

poetry is formatted with preview style, for the table at https://github.com/astral-sh/ruff/actions/runs/6691118897#summary-18177750624

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/hug.py`:66 on 2023-10-30 10:15_

nit: Use `**{}` with dict contents here ("TypeError: <...> argument after ** must be a mapping, not list")

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:1202 on 2023-10-30 10:19_

I guess
```python
f((
    1,
    2,
    3,
))
```
is too hard to spot

---

_@konstin approved on 2023-10-30 10:21_

---

_@charliermarsh reviewed on 2023-10-30 14:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:422 on 2023-10-30 14:57_

Only one of these cases is necessary, but it felt odd to be inconsistent between branches -- like, we're then encoding details about which combinations of values are actually possible in practice, aren't we?

---

_Comment by @charliermarsh on 2023-10-30 15:12_

Poetry's similarity index went down, but I assume that's because this isn't even out in Black yet, so their preview style won't include this.

---

_Comment by @zanieb on 2023-10-30 15:27_

> and I don't know if the ecosystem checks include preview style yet, do they @zanieb? The PR shows no change.

They did not; see #8358.

---

_Comment by @MichaReiser on 2023-11-28 06:04_

@charliermarsh what do you plan to do with this PR?

---

_Comment by @charliermarsh on 2023-11-28 06:13_

@MichaReiser - Waiting to see if they decide to stabilize or postpone in https://github.com/psf/black/issues/4042. I am also interested in our stance on preview in general -- Black seems to suggest that preview features won't always graduate to stable and may be reverted, but our preview mode was generally intended to be features in which we have high confidence.

---

_Comment by @MichaReiser on 2023-11-29 01:47_

I'm okay with landing this now but would recommend documenting the preview checks with the corresponding black preview-code to make it easier to remove the right preview style checks. 

There's a risk that this might never be stabilized. But for now, the intention is to ship it, just maybe not as part of 2024.

---

_Comment by @charliermarsh on 2023-11-29 03:32_

> I'm okay with landing this now but would recommend documenting the preview checks with the corresponding black preview-code to make it easier to remove the right preview style checks.

Can you expand on this a bit more? (Not disagreeing, just didn't fully understand the request.)

---

_@charliermarsh reviewed on 2023-11-29 03:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:1202 on 2023-11-29 03:36_

They re-added them in a later PR (https://github.com/psf/black/pull/4012), so I'm changing it here too.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:1132 on 2023-11-29 03:42_

Will do.

---

_@charliermarsh reviewed on 2023-11-29 03:42_

---

_Comment by @MichaReiser on 2023-11-29 03:45_

It would be good to add comments in the code where you check if preview style is enabled that mentions the `hug_parens_with_braces....` code. It will allow us to easily search for the future when promoting the new style to stable (or to not remove the check if it shouldn't be promoted). 


---

_Comment by @charliermarsh on 2023-11-29 03:49_

👍 Makes sense, will do.

---

_Comment by @charliermarsh on 2023-11-29 03:50_

Heads up that Black now collapses these recursively, i.e.:

```python
# Input
foo([([
    [
        1,
        2,
        3,
    ]
])])

# Black
foo([([[
    1,
    2,
    3,
]])])
```

---

_Converted to draft by @charliermarsh on 2023-11-29 03:57_

---

_Comment by @github-actions[bot] on 2023-11-29 03:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+41688 -48548 lines in 1289 files in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+37 -45 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/client.py#L128'>disnake/client.py~L128</a>
```diff
         if task.cancelled():
             continue
         if task.exception() is not None:
-            loop.call_exception_handler(
-                {
-                    "message": "Unhandled exception during Client.run shutdown.",
-                    "exception": task.exception(),
-                    "task": task,
-                }
-            )
+            loop.call_exception_handler({
+                "message": "Unhandled exception during Client.run shutdown.",
+                "exception": task.exception(),
+                "task": task,
+            })
 
 
 def _cleanup_loop(loop: asyncio.AbstractEventLoop) -> None:
```
<a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/embeds.py#L305'>disnake/embeds.py~L305</a>
```diff
         return total
 
     def __bool__(self) -> bool:
-        return any(
-            (
-                self.title,
-                self.url,
-                self.description,
-                self._colour,
-                self._fields,
-                self._timestamp,
-                self._author,
-                self._thumbnail,
-                self._footer,
-                self._image,
-                self._provider,
-                self._video,
-            )
-        )
+        return any((
+            self.title,
+            self.url,
+            self.description,
+            self._colour,
+            self._fields,
+            self._timestamp,
+            self._author,
+            self._thumbnail,
+            self._footer,
+            self._image,
+            self._provider,
+            self._video,
+        ))
 
     def __eq__(self, other: Any) -> bool:
         if not isinstance(other, Embed):
```
<a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/guild.py#L4866'>disnake/guild.py~L4866</a>
```diff
             overwrites_data: List[PermissionOverwritePayload] = []
             for target, perm in overwrites.items():
                 allow, deny = perm.pair()
-                overwrites_data.append(
-                    {
-                        "allow": str(allow.value),
-                        "deny": str(deny.value),
-                        "id": target,
-                        # can only set overrides for roles here
-                        "type": abc._Overwrites.ROLE,
-                    }
-                )
+                overwrites_data.append({
+                    "allow": str(allow.value),
+                    "deny": str(deny.value),
+                    "id": target,
+                    # can only set overrides for roles here
+                    "type": abc._Overwrites.ROLE,
+                })
             data["permission_overwrites"] = overwrites_data
 
         if category is not MISSING:
```
<a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/state.py#L166'>disnake/state.py~L166</a>
```diff
         _log.exception("Exception occurred during %s", info)
 
 
-_SELECT_COMPONENT_TYPES = frozenset(
-    (
-        ComponentType.string_select,
-        ComponentType.user_select,
-        ComponentType.role_select,
-        ComponentType.mentionable_select,
-        ComponentType.channel_select,
-    )
-)
+_SELECT_COMPONENT_TYPES = frozenset((
+    ComponentType.string_select,
+    ComponentType.user_select,
+    ComponentType.role_select,
+    ComponentType.mentionable_select,
+    ComponentType.channel_select,
+))
 
 
 class ConnectionState:
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+26 -38 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L71'>housewatch/api/analyze.py~L71</a>
```diff
         )
         normalized_query = query_details[0]["normalized_query"]
 
-        return Response(
-            {
-                "query": normalized_query,
-            }
-        )
+        return Response({
+            "query": normalized_query,
+        })
 
     @action(detail=True, methods=["GET"])
     def query_metrics(self, request: Request, pk: str):
```
<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L94'>housewatch/api/analyze.py~L94</a>
```diff
         )
         cpu = run_query(QUERY_CPU_USAGE_SQL, {"days": days, "conditions": conditions})
 
-        return Response(
-            {
-                "execution_count": execution_count,
-                "memory_usage": memory_usage,
-                "read_bytes": read_bytes,
-                "cpu": cpu,
-            }
-        )
+        return Response({
+            "execution_count": execution_count,
+            "memory_usage": memory_usage,
+            "read_bytes": read_bytes,
+            "cpu": cpu,
+        })
 
     @action(detail=True, methods=["GET"])
     def query_explain(self, request: Request, pk: str):
```
<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L111'>housewatch/api/analyze.py~L111</a>
```diff
         example_queries = query_details[0]["example_queries"]
         explain = run_query(EXPLAIN_QUERY, {"query": example_queries[0]})
 
-        return Response(
-            {
-                "explain": explain,
-            }
-        )
+        return Response({
+            "explain": explain,
+        })
 
     @action(detail=True, methods=["GET"])
     def query_examples(self, request: Request, pk: str):
```
<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L124'>housewatch/api/analyze.py~L124</a>
```diff
         )
         example_queries = query_details[0]["example_queries"]
 
-        return Response(
-            {
-                "example_queries": [{"query": q} for q in example_queries],
-            }
-        )
+        return Response({
+            "example_queries": [{"query": q} for q in example_queries],
+        })
 
     @action(detail=False, methods=["GET"])
     def query_graphs(self, request: Request):
```
<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L141'>housewatch/api/analyze.py~L141</a>
```diff
         )
         read_bytes = run_query(QUERY_READ_BYTES_SQL, {"days": days, "conditions": ""})
         cpu = run_query(QUERY_CPU_USAGE_SQL, {"days": days, "conditions": ""})
-        return Response(
-            {
-                "execution_count": execution_count,
-                "memory_usage": memory_usage,
-                "read_bytes": read_bytes,
-                "cpu": cpu,
-            }
-        )
+        return Response({
+            "execution_count": execution_count,
+            "memory_usage": memory_usage,
+            "read_bytes": read_bytes,
+            "cpu": cpu,
+        })
 
     @action(detail=False, methods=["POST"])
     def logs(self, request: Request):
```
<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/api/analyze.py#L295'>housewatch/api/analyze.py~L295</a>
```diff
                 )
                 i += 1
                 sleep(0.5)
-            return Response(
-                {
-                    "is_result_equal": is_result_equal,
-                    "benchmarking_result": benchmarking_result,
-                }
-            )
+            return Response({
+                "is_result_equal": is_result_equal,
+                "benchmarking_result": benchmarking_result,
+            })
         except Exception as e:
             return Response(
                 status=418, data={"error": str(e), "error_location": error_location}
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1186 -1337 lines across 49 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/brokers/pika.py#L238'>rasa/core/brokers/pika.py~L238</a>
```diff
             self.exchange_name, type=aio_pika.ExchangeType.FANOUT
         )
 
-        await asyncio.gather(
-            *[
-                self._bind_queue(queue_name, channel, exchange)
-                for queue_name in self.queues
-            ]
-        )
+        await asyncio.gather(*[
+            self._bind_queue(queue_name, channel, exchange)
+            for queue_name in self.queues
+        ])
 
         return exchange
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/channels/hangouts.py#L70'>rasa/core/channels/hangouts.py~L70</a>
```diff
                 )
                 return None
 
-            hangouts_buttons.append(
-                {
-                    "textButton": {
-                        "text": b_txt,
-                        "onClick": {"action": {"actionMethodName": b_pl}},
-                    }
+            hangouts_buttons.append({
+                "textButton": {
+                    "text": b_txt,
+                    "onClick": {"action": {"actionMethodName": b_pl}},
                 }
-            )
+            })
 
         card = {
             "cards": [
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/evaluation/marker_base.py#L665'>rasa/core/evaluation/marker_base.py~L665</a>
```diff
         """
         with path.open(mode="w") as f:
             table_writer = csv.writer(f)
-            table_writer.writerow(
-                [
-                    "sender_id",
-                    "session_idx",
-                    "marker",
-                    "event_idx",
-                    "num_preceding_user_turns",
-                ]
-            )
+            table_writer.writerow([
+                "sender_id",
+                "session_idx",
+                "marker",
+                "event_idx",
+                "num_preceding_user_turns",
+            ])
             for sender_id, results_per_session in results.items():
                 for session_idx, session_result in enumerate(results_per_session):
                     Marker._write_relevant_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/evaluation/marker_base.py#L686'>rasa/core/evaluation/marker_base.py~L686</a>
```diff
     ) -> None:
         for marker_name, meta_data_per_relevant_event in session.items():
             for event_meta_data in meta_data_per_relevant_event:
-                writer.writerow(
-                    [
-                        sender_id,
-                        str(session_idx),
-                        marker_name,
-                        str(event_meta_data.idx),
-                        str(event_meta_data.preceding_user_turns),
-                    ]
-                )
+                writer.writerow([
+                    sender_id,
+                    str(session_idx),
+                    marker_name,
+                    str(event_meta_data.idx),
+                    str(event_meta_data.preceding_user_turns),
+                ])
 
 
 class OperatorMarker(Marker, ABC):
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/evaluation/marker_stats.py#L279'>rasa/core/evaluation/marker_stats.py~L279</a>
```diff
             value_str = str(np.nan)
         else:
             value_str = np.round(statistic_value, 3)
-        table_writer.writerow(
-            [
-                str(item)
-                for item in [
-                    sender_id,
-                    session_idx,
-                    marker_name,
-                    statistic_name,
-                    value_str,
-                ]
+        table_writer.writerow([
+            str(item)
+            for item in [
+                sender_id,
+                session_idx,
+                marker_name,
+                statistic_name,
+                value_str,
             ]
-        )
+        ])
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/featurizers/tracker_featurizers.py#L143'>rasa/core/featurizers/tracker_featurizers.py~L143</a>
```diff
         """
         # store labels in numpy arrays so that it corresponds to np arrays of input
         # features
-        return ragged_array_to_ndarray(
-            [
-                np.array(
-                    [domain.index_for_action(action) for action in tracker_actions]
-                )
-                for tracker_actions in trackers_as_actions
-            ]
-        )
+        return ragged_array_to_ndarray([
+            np.array([domain.index_for_action(action) for action in tracker_actions])
+            for tracker_actions in trackers_as_actions
+        ])
 
     def _create_entity_tags(
         self,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/migrate.py#L70'>rasa/core/migrate.py~L70</a>
```diff
             updated_mappings.append(mapping_copy)
 
     for mapping in new_mappings:
-        mapping.update(
-            {
-                "conditions": [
-                    _get_updated_mapping_condition(condition, mapping, slot_name)
-                ]
-            }
-        )
+        mapping.update({
+            "conditions": [
+                _get_updated_mapping_condition(condition, mapping, slot_name)
+            ]
+        })
         updated_mappings.append(mapping)
 
     return updated_mappings
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/migrate.py#L178'>rasa/core/migrate.py~L178</a>
```diff
         elif key == KEY_FORMS:
             new_domain.update({key: new_forms})
         elif key == "version":
-            new_domain.update(
-                {key: DoubleQuotedScalarString(LATEST_TRAINING_DATA_FORMAT_VERSION)}
-            )
+            new_domain.update({
+                key: DoubleQuotedScalarString(LATEST_TRAINING_DATA_FORMAT_VERSION)
+            })
         else:
             new_domain.update({key: value})
     return new_domain
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/migrate.py#L234'>rasa/core/migrate.py~L234</a>
```diff
 
         if KEY_SLOTS not in original_content and KEY_FORMS not in original_content:
             if isinstance(original_content, dict):
-                original_content.update(
-                    {
-                        "version": DoubleQuotedScalarString(
-                            LATEST_TRAINING_DATA_FORMAT_VERSION
-                        )
-                    }
-                )
+                original_content.update({
+                    "version": DoubleQuotedScalarString(
+                        LATEST_TRAINING_DATA_FORMAT_VERSION
+                    )
+                })
 
             # this is done so that the other domain files can be moved
             # in the migrated directory
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/policies/ted_policy.py#L525'>rasa/core/policies/ted_policy.py~L525</a>
```diff
         model_data = RasaModelData(label_key=LABEL_KEY, label_sub_key=LABEL_SUB_KEY)
 
         if label_ids is not None and encoded_all_labels is not None:
-            label_ids = np.array(
-                [np.expand_dims(seq_label_ids, -1) for seq_label_ids in label_ids]
-            )
+            label_ids = np.array([
+                np.expand_dims(seq_label_ids, -1) for seq_label_ids in label_ids
+            ])
             model_data.add_features(
                 LABEL_KEY,
                 LABEL_SUB_KEY,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/policies/ted_policy.py#L555'>rasa/core/policies/ted_policy.py~L555</a>
```diff
 
         # add the dialogue lengths
         attribute_present = next(iter(list(attribute_data.keys())))
-        dialogue_lengths = np.array(
-            [
-                np.size(np.squeeze(f, -1))
-                for f in model_data.data[attribute_present][MASK][0]
-            ]
-        )
+        dialogue_lengths = np.array([
+            np.size(np.squeeze(f, -1))
+            for f in model_data.data[attribute_present][MASK][0]
+        ])
         model_data.data[DIALOGUE][LENGTH] = [
             FeatureArray(dialogue_lengths, number_of_dimensions=1)
         ]
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/policies/ted_policy.py#L1300'>rasa/core/policies/ted_policy.py~L1300</a>
```diff
         # Disable input dropout in the config to be used if this is a label attribute.
         if is_label_attribute:
             config_to_use = self.config.copy()
-            config_to_use.update(
-                {SPARSE_INPUT_DROPOUT: False, DENSE_INPUT_DROPOUT: False}
-            )
+            config_to_use.update({
+                SPARSE_INPUT_DROPOUT: False,
+                DENSE_INPUT_DROPOUT: False,
+            })
         else:
             config_to_use = self.config
         # Attributes with sequence-level features also have sentence-level features,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/processor.py#L651'>rasa/core/processor.py~L651</a>
```diff
     @staticmethod
     def _log_slots(tracker: DialogueStateTracker) -> None:
         # Log currently set slots
-        slot_values = "\n".join(
-            [f"\t{s.name}: {s.value}" for s in tracker.slots.values()]
-        )
+        slot_values = "\n".join([
+            f"\t{s.name}: {s.value}" for s in tracker.slots.values()
+        ])
         if slot_values.strip():
             structlogger.debug(
                 "processor.slots.log", slot_values=copy.deepcopy(slot_values)
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/test.py#L859'>rasa/core/test.py~L859</a>
```diff
 
             if action_executed_result.action_targets:
                 tracker_eval_store.merge_store(action_executed_result)
-                tracker_actions.append(
-                    {
-                        "action": action_executed_result.action_targets[0],
-                        "predicted": action_executed_result.action_predictions[0],
-                        "policy": prediction.policy_name,
-                        "confidence": prediction.max_confidence,
-                    }
-                )
+                tracker_actions.append({
+                    "action": action_executed_result.action_targets[0],
+                    "predicted": action_executed_result.action_predictions[0],
+                    "policy": prediction.policy_name,
+                    "confidence": prediction.max_confidence,
+                })
         elif use_e2e and isinstance(event, UserUttered):
             # This means that user utterance didn't have a user message, only intent,
             # so we can skip the NLU part and take the parse data directly.
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/core/tracker_store.py#L606'>rasa/core/tracker_store.py~L606</a>
```diff
         for new_event in tracker.events:
             # Event subclasses implement `__eq__` method that make it difficult
             # to compare events. We use `as_dict` to compare events.
-            if all(
-                [
-                    new_event.as_dict() != existing_event.as_dict()
-                    for existing_event in merged.events
-                ]
-            ):
+            if all([
+                new_event.as_dict() != existing_event.as_dict()
+                for existing_event in merged.events
+            ]):
                 merged.update(new_event)
 
         return merged
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/engine/recipes/default_recipe.py#L339'>rasa/engine/recipes/default_recipe.py~L339</a>
```diff
                     cli_parameters,
                 )
 
-            if component.types.intersection(
-                {
-                    self.ComponentType.MESSAGE_TOKENIZER,
-                    self.ComponentType.MESSAGE_FEATURIZER,
-                }
-            ):
+            if component.types.intersection({
+                self.ComponentType.MESSAGE_TOKENIZER,
+                self.ComponentType.MESSAGE_FEATURIZER,
+            }):
                 last_run_node = self._add_nlu_process_node(
                     train_nodes,
                     component.clazz,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/engine/recipes/default_recipe.py#L731'>rasa/engine/recipes/default_recipe.py~L731</a>
```diff
                     config=config,
                 )
 
-            if component.types.intersection(
-                {
-                    self.ComponentType.MESSAGE_TOKENIZER,
-                    self.ComponentType.MESSAGE_FEATURIZER,
-                }
-            ):
+            if component.types.intersection({
+                self.ComponentType.MESSAGE_TOKENIZER,
+                self.ComponentType.MESSAGE_FEATURIZER,
+            }):
                 last_run_node = self._add_nlu_predict_node_from_train(
                     predict_nodes,
                     component_name,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/engine/recipes/default_recipe.py#L745'>rasa/engine/recipes/default_recipe.py~L745</a>
```diff
                     config,
                     from_resource=component.is_trainable,
                 )
-            elif component.types.intersection(
-                {
-                    self.ComponentType.INTENT_CLASSIFIER,
-                    self.ComponentType.ENTITY_EXTRACTOR,
-                }
-            ):
+            elif component.types.intersection({
+                self.ComponentType.INTENT_CLASSIFIER,
+                self.ComponentType.ENTITY_EXTRACTOR,
+            }):
                 if component.is_trainable:
                     last_run_node = self._add_nlu_predict_node_from_train(
                         predict_nodes,
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/engine/validation.py#L510'>rasa/engine/validation.py~L510</a>
```diff
         for component_type, node_names in unmet_requirements_for_target.items():
             unmet_requirements.setdefault(component_type, set()).update(node_names)
     if unmet_requirements:
-        errors = "\n".join(
-            [
-                f"The following components require a {component_type.__name__}: "
-                f"{', '.join(sorted(required_by))}. "
-                for component_type, required_by in unmet_requirements.items()
-            ]
-        )
+        errors = "\n".join([
+            f"The following components require a {component_type.__name__}: "
+            f"{', '.join(sorted(required_by))}. "
+            for component_type, required_by in unmet_requirements.items()
+        ])
         num_nodes = len(
             set(
                 node_name
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/graph_components/validators/default_recipe_validator.py#L156'>rasa/graph_components/validators/default_recipe_validator.py~L156</a>
```diff
                 docs=DOCS_URL_COMPONENTS,
             )
 
-        if training_data.entity_examples and self._component_types.isdisjoint(
-            {DIETClassifier, CRFEntityExtractor}
-        ):
+        if training_data.entity_examples and self._component_types.isdisjoint({
+            DIETClassifier,
+            CRFEntityExtractor,
+        }):
             if training_data.entity_roles_groups_used():
                 rasa.shared.utils.io.raise_warning(
                     f"You have defined training data with entities that "
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/graph_components/validators/default_recipe_validator.py#L172'>rasa/graph_components/validators/default_recipe_validator.py~L172</a>
```diff
                     docs=DOCS_URL_COMPONENTS,
                 )
 
-        if training_data.regex_features and self._component_types.isdisjoint(
-            [RegexFeaturizer, RegexEntityExtractor]
-        ):
+        if training_data.regex_features and self._component_types.isdisjoint([
+            RegexFeaturizer,
+            RegexEntityExtractor,
+        ]):
             rasa.shared.utils.io.raise_warning(
                 f"You have defined training data with regexes, but "
                 f"your NLU configuration does not include a 'RegexFeaturizer' "
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/graph_components/validators/default_recipe_validator.py#L186'>rasa/graph_components/validators/default_recipe_validator.py~L186</a>
```diff
                 docs=DOCS_URL_COMPONENTS,
             )
 
-        if training_data.lookup_tables and self._component_types.isdisjoint(
-            [RegexFeaturizer, RegexEntityExtractor]
-        ):
+        if training_data.lookup_tables and self._component_types.isdisjoint([
+            RegexFeaturizer,
+            RegexEntityExtractor,
+        ]):
             rasa.shared.utils.io.raise_warning(
                 f"You have defined training data consisting of lookup tables, but "
                 f"your NLU configuration does not include a featurizer "
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/graph_components/validators/default_recipe_validator.py#L362'>rasa/graph_components/validators/default_recipe_validator.py~L362</a>
```diff
             and not node_name.startswith("e2e")
         ]
 
-        Featurizer.raise_if_featurizer_configs_are_not_compatible(
-            [schema_node.config for schema_node in featurizers]
-        )
+        Featurizer.raise_if_featurizer_configs_are_not_compatible([
+            schema_node.config for schema_node in featurizers
+        ])
 
     def _validate_core(self, story_graph: StoryGraph, domain: Domain) -> None:
         """Validates whether the configuration matches the training data.
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/classifiers/diet_classifier.py#L1463'>rasa/nlu/classifiers/diet_classifier.py~L1463</a>
```diff
 
             # disable input dropout applied to sparse and dense label features
             label_config = self.config.copy()
-            label_config.update(
-                {SPARSE_INPUT_DROPOUT: False, DENSE_INPUT_DROPOUT: False}
-            )
+            label_config.update({
+                SPARSE_INPUT_DROPOUT: False,
+                DENSE_INPUT_DROPOUT: False,
+            })
 
             self._tf_layers[
                 f"feature_combining_layer.{self.label_name}"
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/featurizers/dense_featurizer/lm_featurizer.py#L543'>rasa/nlu/featurizers/dense_featurizer/lm_featurizer.py~L543</a>
```diff
         for index, embedding in enumerate(sequence_embeddings):
             embedding_size = embedding.shape[-1]
             if actual_sequence_lengths[index] > self.max_model_sequence_length:
-                embedding = np.concatenate(
-                    [
-                        embedding,
-                        np.zeros(
-                            (
-                                actual_sequence_lengths[index]
-                                - self.max_model_sequence_length,
-                                embedding_size,
-                            ),
-                            dtype=np.float32,
+                embedding = np.concatenate([
+                    embedding,
+                    np.zeros(
+                        (
+                            actual_sequence_lengths[index]
+                            - self.max_model_sequence_length,
+                            embedding_size,
                         ),
-                    ]
-                )
+                        dtype=np.float32,
+                    ),
+                ])
             reshaped_sequence_embeddings.append(embedding)
         return ragged_array_to_ndarray(reshaped_sequence_embeddings)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L437'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L437</a>
```diff
         # To train a shared vocabulary, we use TEXT as the
         # attribute for which a combined vocabulary is built.
         if not self.finetune_mode:
-            self.vectorizers = self._create_shared_vocab_vectorizers(
-                {
-                    "strip_accents": self.strip_accents,
-                    "lowercase": self.lowercase,
-                    "stop_words": self.stop_words,
-                    "min_ngram": self.min_ngram,
-                    "max_ngram": self.max_ngram,
-                    "max_df": self.max_df,
-                    "min_df": self.min_df,
-                    "max_features": self.max_features,
-                    "analyzer": self.analyzer,
-                }
-            )
+            self.vectorizers = self._create_shared_vocab_vectorizers({
+                "strip_accents": self.strip_accents,
+                "lowercase": self.lowercase,
+                "stop_words": self.stop_words,
+                "min_ngram": self.min_ngram,
+                "max_ngram": self.max_ngram,
+                "max_df": self.max_df,
+                "min_df": self.min_df,
+                "max_features": self.max_features,
+                "analyzer": self.analyzer,
+            })
             self._fit_vectorizer_from_scratch(TEXT, combined_cleaned_texts)
         else:
             self._fit_loaded_vectorizer(TEXT, combined_cleaned_texts)
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L460'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L460</a>
```diff
     ) -> None:
         """Constructs the vectorizers and train them with an independent vocab."""
         if not self.finetune_mode:
-            self.vectorizers = self._create_independent_vocab_vectorizers(
-                {
-                    "strip_accents": self.strip_accents,
-                    "lowercase": self.lowercase,
-                    "stop_words": self.stop_words,
-                    "min_ngram": self.min_ngram,
-                    "max_ngram": self.max_ngram,
-                    "max_df": self.max_df,
-                    "min_df": self.min_df,
-                    "max_features": self.max_features,
-                    "analyzer": self.analyzer,
-                }
-            )
+            self.vectorizers = self._create_independent_vocab_vectorizers({
+                "strip_accents": self.strip_accents,
+                "lowercase": self.lowercase,
+                "stop_words": self.stop_words,
+                "min_ngram": self.min_ngram,
+                "max_ngram": self.max_ngram,
+                "max_df": self.max_df,
+                "min_df": self.min_df,
+                "max_features": self.max_features,
+                "analyzer": self.analyzer,
+            })
         for attribute in self._attributes:
             if self._attribute_texts_is_non_empty(attribute_texts[attribute]):
                 if not self.finetune_mode:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L199'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L199</a>
```diff
             `self.config` should be checked
         """
         self._feature_to_idx_dict = feature_to_idx_dict
-        self._number_of_features = sum(
-            [
-                len(feature_values.values())
-                for feature_values in self._feature_to_idx_dict.values()
-            ]
-        )
+        self._number_of_features = sum([
+            len(feature_values.values())
+            for feature_values in self._feature_to_idx_dict.values()
+        ])
         if check_consistency_with_config:
             known_features = set(self._feature_to_idx_dict.keys())
             not_in_config = known_features.difference(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/nlu/test.py#L1116'>rasa/nlu/test.py~L1116</a>
```diff
     from rasa.nlu.extractors.crf_entity_extractor import CRFEntityExtractor
     from rasa.nlu.classifiers.diet_classifier import DIETClassifier
 
-    return not extractors.isdisjoint(
-        {CRFEntityExtractor.__name__, DIETClassifier.__name__}
-    )
+    return not extractors.isdisjoint({
+        CRFEntityExtractor.__name__,
+        DIETClassifier.__name__,
+    })
 
 
 def align_entity_predictions(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/server.py#L691'>rasa/server.py~L691</a>
```diff
     @app.get("/version")
     async def version(request: Request) -> HTTPResponse:
         """Respond with the version number of the installed Rasa."""
-        return response.json(
-            {
-                "version": rasa.__version__,
-                "minimum_compatible_version": MINIMUM_COMPATIBLE_VERSION,
-            }
-        )
+        return response.json({
+            "version": rasa.__version__,
+            "minimum_compatible_version": MINIMUM_COMPATIBLE_VERSION,
+        })
 
     @app.get("/status")
     @requires_auth(app, auth_token)
     @ensure_loaded_agent(app)
     async def status(request: Request) -> HTTPResponse:
         """Respond with the model name and the fingerprint of that model."""
-        return response.json(
-            {
-                "model_file": app.ctx.agent.processor.model_filename,
-                "model_id": app.ctx.agent.model_id,
-                "num_active_training_jobs": app.ctx.active_training_processes.value,
-            }
-        )
+        return response.json({
+            "model_file": app.ctx.agent.processor.model_filename,
+            "model_id": app.ctx.agent.model_id,
+            "num_active_training_jobs": app.ctx.active_training_processes.value,
+        })
 
     @app.get("/conversations/<conversation_id:path>/tracker")
     @requires_auth(app, auth_token)
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L151'>rasa/shared/core/events.py~L151</a>
```diff
 
     message_from_md = entities_parser.parse_training_example(text, intent)
     deserialised_entities = deserialise_entities(entities)
-    return TrainingDataWriter.generate_message(
-        {"text": message_from_md.get(TEXT), "entities": deserialised_entities}
-    )
+    return TrainingDataWriter.generate_message({
+        "text": message_from_md.get(TEXT),
+        "entities": deserialised_entities,
+    })
 
 
 def split_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L583'>rasa/shared/core/events.py~L583</a>
```diff
 
     def as_dict(self) -> Dict[Text, Any]:
         _dict = super().as_dict()
-        _dict.update(
-            {
-                "text": self.text,
-                "parse_data": self.parse_data,
-                "input_channel": getattr(self, "input_channel", None),
-                "message_id": getattr(self, "message_id", None),
-                "metadata": self.metadata,
-            }
-        )
+        _dict.update({
+            "text": self.text,
+            "parse_data": self.parse_data,
+            "input_channel": getattr(self, "input_channel", None),
+            "message_id": getattr(self, "message_id", None),
+            "metadata": self.metadata,
+        })
         return _dict
 
     def as_sub_state(self) -> Dict[Text, Union[None, Text, List[Optional[Text]]]]:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L1165'>rasa/shared/core/events.py~L1165</a>
```diff
 
     def __hash__(self) -> int:
         """Returns unique hash for event."""
-        return hash(
-            (
-                self.intent,
-                self.entities,
-                self.trigger_date_time.isoformat(),
-                self.kill_on_user_message,
-                self.name,
-            )
-        )
+        return hash((
+            self.intent,
+            self.entities,
+            self.trigger_date_time.isoformat(),
+            self.kill_on_user_message,
+            self.name,
+        ))
 
     def __eq__(self, other: Any) -> bool:
         """Compares object with other object."""
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L1327'>rasa/shared/core/events.py~L1327</a>
```diff
 
     def as_story_string(self) -> Text:
         """Returns text representation of event."""
-        props = json.dumps(
-            {"name": self.name, "intent": self.intent, "entities": self.entities}
-        )
+        props = json.dumps({
+            "name": self.name,
+            "intent": self.intent,
+            "entities": self.entities,
+        })
         return f"{self.type_name}{props}"
 
     @classmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L1639'>rasa/shared/core/events.py~L1639</a>
```diff
     def as_dict(self) -> Dict[Text, Any]:
         """Returns serialized event."""
         d = super().as_dict()
-        d.update(
-            {
-                "name": self.action_name,
-                "policy": self.policy,
-                "confidence": self.confidence,
-                "action_text": self.action_text,
-                "hide_rule_turn": self.hide_rule_turn,
-            }
-        )
+        d.update({
+            "name": self.action_name,
+            "policy": self.policy,
+            "confidence": self.confidence,
+            "action_text": self.action_text,
+            "hide_rule_turn": self.hide_rule_turn,
+        })
         return d
 
     def as_sub_state(self) -> Dict[Text, Text]:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/events.py#L1994'>rasa/shared/core/events.py~L1994</a>
```diff
     def as_dict(self) -> Dict[Text, Any]:
         """Returns serialized event."""
         d = super().as_dict()
-        d.update(
-            {
-                "name": self.action_name,
-                "policy": self.policy,
-                "confidence": self.confidence,
-            }
-        )
+        d.update({
+            "name": self.action_name,
+            "policy": self.policy,
+            "confidence": self.confidence,
+        })
         return d
 
     def apply_to(self, tracker: "DialogueStateTracker") -> None:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/generator.py#L598'>rasa/shared/core/generator.py~L598</a>
```diff
         """Add unused end checkpoints
         if they were never encountered as start checkpoints.
         """
-        return unused_checkpoints.union(
-            {
-                start_name
-                for start_name in start_checkpoints
-                if start_name not in used_checkpoints
-            }
-        )
+        return unused_checkpoints.union({
+            start_name
+            for start_name in start_checkpoints
+            if start_name not in used_checkpoints
+        })
 
     @staticmethod
     def _filter_active_trackers(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L751'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L751</a>
```diff
                 entity_values = [entity_values]
 
             for entity_value in entity_values:
-                entities.append(
-                    {
-                        ENTITY_ATTRIBUTE_TYPE: entity_type,
-                        ENTITY_ATTRIBUTE_VALUE: entity_value,
-                        ENTITY_ATTRIBUTE_START: match.start(ENTITIES),
-                        ENTITY_ATTRIBUTE_END: match.end(ENTITIES),
-                        **default_properties,
-                    }
-                )
+                entities.append({
+                    ENTITY_ATTRIBUTE_TYPE: entity_type,
+                    ENTITY_ATTRIBUTE_VALUE: entity_value,
+                    ENTITY_ATTRIBUTE_START: match.start(ENTITIES),
+                    ENTITY_ATTRIBUTE_END: match.end(ENTITIES),
+                    **default_properties,
+                })
         return entities
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L180'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L180</a>
```diff
             `True` if the `stories` contain at least one active loop.
             `False` otherwise.
         """
-        return any(
-            [
-                [event for event in story_step.events if isinstance(event, ActiveLoop)]
-                for story_step in stories
-            ]
-        )
+        return any([
+            [event for event in story_step.events if isinstance(event, ActiveLoop)]
+            for story_step in stories
+        ])
 
     @staticmethod
     def process_user_utterance(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L225'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L225</a>
```diff
                                     )
                                 )
                                 if commented_entity:
-                                    entity_map = CommentedMap(
-                                        [(entity["entity"], entity["value"])]
-                                    )
+                                    entity_map = CommentedMap([
+                                        (entity["entity"], entity["value"])
+                                    ])
                                     entity_map.yaml_add_eol_comment(
                                         commented_entity, entity["entity"]
                                     )
                                     entities.append(entity_map)
                                 else:
                                     entities.append(
-                                        OrderedDict(
-                                            [(entity["entity"], entity["value"])]
-                                        )
+                                        OrderedDict([
+                                            (entity["entity"], entity["value"])
+                                        ])
                                     )
                     else:
                         entities.append(
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L327'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L327</a>
```diff
         for checkpoint in checkpoints:
             if checkpoint.name == STORY_START:
                 continue
-            next_checkpoint: OrderedDict[Text, Any] = OrderedDict(
-                [(KEY_CHECKPOINT, checkpoint.name)]
-            )
+            next_checkpoint: OrderedDict[Text, Any] = OrderedDict([
+                (KEY_CHECKPOINT, checkpoint.name)
+            ])
             if checkpoint.conditions:
                 next_checkpoint[KEY_CHECKPOINT_SLOTS] = [
                     {key: value} for key, value in checkpoint.conditions.items()
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L346'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L346</a>
```diff
         Returns:
             Dict with converted user utterances.
         """
-        return OrderedDict(
-            [
-                (
-                    KEY_OR,
-                    [
-                        self.process_user_utterance(utterance, self._is_test_story)
-                        for utterance in utterances
-                    ],
-                )
-            ]
-        )
+        return OrderedDict([
+            (
+                KEY_OR,
+                [
+                    self.process_user_utterance(utterance, self._is_test_story)
+                    for utterance in utterances
+                ],
+            )
+        ])
 
     @staticmethod
     def process_active_loop(event: ActiveLoop) -> OrderedDict:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/importers/importer.py#L413'>rasa/shared/importers/importer.py~L413</a>
```diff
             retrieval_intents
         )
 
-        return Domain.from_dict(
-            {
-                KEY_INTENTS: retrieval_intent_properties,
-                KEY_RESPONSES: responses,
-                KEY_ACTIONS: action_names,
-            }
-        )
+        return Domain.from_dict({
+            KEY_INTENTS: retrieval_intent_properties,
+            KEY_RESPONSES: responses,
+            KEY_ACTIONS: action_names,
+        })
 
     def get_stories(self, exclusion_percentage: Optional[int] = None) -> StoryGraph:
         """Retrieves training stories / rules (see parent class for full docstring)."""
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/importers/importer.py#L481'>rasa/shared/importers/importer.py~L481</a>
```diff
 
         additional_e2e_action_names = set()
         for story_step in stories.story_steps:
-            additional_e2e_action_names.update(
-                {
-                    event.action_text
-                    for event in story_step.events
-                    if isinstance(event, ActionExecuted) and event.action_text
-                }
-            )
+            additional_e2e_action_names.update({
+                event.action_text
+                for event in story_step.events
+                if isinstance(event, ActionExecuted) and event.action_text
+            })
 
         return Domain.from_dict({KEY_E2E_ACTIONS: list(additional_e2e_action_names)})
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/nlu/training_data/formats/readerwriter.py#L179'>rasa/shared/nlu/training_data/formats/readerwriter.py~L179</a>
```diff
         if use_short_syntax:
             return f"({entity_type})"
         else:
-            entity_dict = OrderedDict(
-                [
-                    (ENTITY_ATTRIBUTE_TYPE, entity_type),
-                    (ENTITY_ATTRIBUTE_ROLE, entity_role),
-                    (ENTITY_ATTRIBUTE_GROUP, entity_group),
-                    (ENTITY_ATTRIBUTE_VALUE, entity_value),
-                ]
-            )
-            entity_dict = OrderedDict(
-                [(k, v) for k, v in entity_dict.items() if v is not None]
-            )
+            entity_dict = OrderedDict([
+                (ENTITY_ATTRIBUTE_TYPE, entity_type),
+                (ENTITY_ATTRIBUTE_ROLE, entity_role),
+                (ENTITY_ATTRIBUTE_GROUP, entity_group),
+                (ENTITY_ATTRIBUTE_VALUE, entity_value),
+            ])
+            entity_dict = OrderedDict([
+                (k, v) for k, v in entity_dict.items() if v is not None
+            ])
 
             return f"{json.dumps(entity_dict)}"
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/shared/nlu/training_data/formats/readerwriter.py#L212'>rasa/shared/nlu/training_data/formats/readerwriter.py~L212</a>
```diff
             ]
             return (
                 f"[{entity_text}]["
-                + ", ".join(
-                    [
-                        TrainingDataWriter.generate_entity_attributes(
-                            text=entity_text, entity=e, short_allowed=False
-                        )
-                        for e in entity
-                    ]
-                )
+                + ", ".join([
+                    TrainingDataWriter.generate_entity_attributes(
+                        text=entity_text, entity=e, short_allowed=False
+                    )
+                    for e in entity
+                ])
                 + "]"
             )
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/utils/tensorflow/model_data.py#L540'>rasa/utils/tensorflow/model_data.py~L540</a>
```diff
 
             if features.number_of_dimensions == 4:
                 lengths = FeatureArray(
-                    ragged_array_to_ndarray(
-                        [
-                            # add one more dim so that dialogue dim
-                            # would be a sequence
-                            np.array([[[x.shape[0]]] for x in _features])
-                            for _features in features
-                        ]
-                    ),
+                    ragged_array_to_ndarray([
+                        # add one more dim so that dialogue dim
+                        # would be a sequence
+                        np.array([[[x.shape[0]]] for x in _features])
+                        for _features in features
+                    ]),
                     number_of_dimensions=4,
                 )
             else:
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/rasa/utils/tensorflow/model_data_utils.py#L234'>rasa/utils/tensorflow/model_data_utils.py~L234</a>
```diff
         The default features
     """
     example_features = next(
-        iter(
-            [
-                list_of_features
-                for list_of_list_of_features in all_features
-                for list_of_features in list_of_list_of_features
-                if list_of_features is not None
-            ]
-        )
+        iter([
+            list_of_features
+            for list_of_list_of_features in all_features
+            for list_of_features in list_of_list_of_features
+            if list_of_features is not None
+        ])
     )
 
     # create fake_features for Nones
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/featurizers/test_single_state_featurizers.py#L138'>tests/core/featurizers/test_single_state_featurizers.py~L138</a>
```diff
     encoded_actions = f.encode_all_labels(domain, precomputations=precomputations)
 
     assert len(encoded_actions) == len(domain.action_names_or_texts)
-    assert all(
-        [
-            ACTION_NAME in encoded_action and ACTION_TEXT not in encoded_action
-            for encoded_action in encoded_actions
-        ]
-    )
+    assert all([
+        ACTION_NAME in encoded_action and ACTION_TEXT not in encoded_action
+        for encoded_action in encoded_actions
+    ])
 
 
 #
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/featurizers/test_single_state_featurizers.py#L327'>tests/core/featurizers/test_single_state_featurizers.py~L327</a>
```diff
     # (only `encode_entities` does that).
     units = 300
     precomputations = MessageContainerForCoreFeaturization()
-    precomputations.add_all(
-        [
-            Message(
-                data={TEXT: text, TOKENS_NAMES[TEXT]: tokens},
-                features=[
-                    dummy_features(
-                        fill_value=11,
-                        units=units,
-                        attribute=TEXT,
-                        type=SENTENCE,
-                        is_sparse=True,
-                    ),
-                    dummy_features(
-                        fill_value=12,
-                        units=units,
-                        attribute=TEXT,
-                        type=SEQUENCE,
-                        is_sparse=False,
-                    ),
-                    # Note: sparse sequence feature is last here
-                    dummy_features(
-                        fill_value=13,
-                        units=units,
-                        attribute=TEXT,
-                        type=SEQUENCE,
-                        is_sparse=True,
-                    ),
-                ],
-            ),
-            Message(data={INTENT: intent}),
-            Message(
-                data={ACTION_TEXT: action_text},
-                features=[
-                    dummy_features(
-                        fill_value=1,
-                        units=units,
-                        attribute=ACTION_TEXT,
-                        type=SEQUENCE,
-                        is_sparse=True,
-                    )
-                ],
-            ),
-        ]
-    )
+    precomputations.add_all([
+        Message(
+            data={TEXT: text, TOKENS_NAMES[TEXT]: tokens},
+            features=[
+                dummy_features(
+                    fill_value=11,
+                    units=units,
+                    attribute=TEXT,
+                    type=SENTENCE,
+                    is_sparse=True,
+                ),
+                dummy_features(
+                    fill_value=12,
+                    units=units,
+                    attribute=TEXT,
+                    type=SEQUENCE,
+                    is_sparse=False,
+                ),
+                # Note: sparse sequence feature is last here
+                dummy_features(
+                    fill_value=13,
+                    units=units,
+                    attribute=TEXT,
+                    type=SEQUENCE,
+                    is_sparse=True,
+                ),
+            ],
+        ),
+        Message(data={INTENT: intent}),
+        Message(
+            data={ACTION_TEXT: action_text},
+            features=[
+                dummy_features(
+                    fill_value=1,
+                    units=units,
+                    attribute=ACTION_TEXT,
+                    type=SEQUENCE,
+                    is_sparse=True,
+                )
+            ],
+        ),
+    ])
     if action_name is not None:
         precomputations.add(Message(data={ACTION_NAME: action_name}))
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/policies/test_ted_policy.py#L273'>tests/core/policies/test_ted_policy.py~L273</a>
```diff
 
         # check that ranking length is applied - without normalization
         if trained_policy.config[RANKING_LENGTH] == 0:
-            assert sum(
-                [confidence for confidence in prediction.probabilities]
-            ) == pytest.approx(1)
+            assert sum([
+                confidence for confidence in prediction.probabilities
+            ]) == pytest.approx(1)
             assert all(confidence > 0 for confidence in prediction.probabilities)
         else:
             assert (
                 sum([confidence > 0 for confidence in prediction.probabilities])
                 == trained_policy.config[RANKING_LENGTH]
             )
-            assert sum(
-                [confidence for confidence in prediction.probabilities]
-            ) != pytest.approx(1)
+            assert sum([
+                confidence for confidence in prediction.probabilities
+            ]) != pytest.approx(1)
 
     def test_label_data_assembly(
         self, trained_policy: TEDPolicy, default_domain: Domain
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/policies/test_ted_policy.py#L373'>tests/core/policies/test_ted_policy.py~L373</a>
```diff
             and batch_action_name_mask.shape[0] == batch_size
         )
         # some features might be "fake" so there sequence is `0`
-        seq_len = max(
-            [
-                batch_intent_sentence_shape[1],
-                batch_action_name_sentence_shape[1],
-                batch_entities_sentence_shape[1],
-                batch_slots_sentence_shape[1],
-            ]
-        )
+        seq_len = max([
+            batch_intent_sentence_shape[1],
+            batch_action_name_sentence_shape[1],
+            batch_entities_sentence_shape[1],
+            batch_slots_sentence_shape[1],
+        ])
         assert (
             batch_intent_sentence_shape[1] == seq_len
             or batch_intent_sentence_shape[1] == 0
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/policies/test_ted_policy.py#L432'>tests/core/policies/test_ted_policy.py~L432</a>
```diff
             and batch_dialogue_length.shape[0] == batch_size
         )
         # some features might be "fake" so there sequence is `0`
-        seq_len = max(
-            [
-                batch_intent_sentence_shape[1],
-                batch_action_name_sentence_shape[1],
-                batch_entities_sentence_shape[1],
-                batch_slots_sentence_shape[1],
-            ]
-        )
+        seq_len = max([
+            batch_intent_sentence_shape[1],
+            batch_action_name_sentence_shape[1],
+            batch_entities_sentence_shape[1],
+            batch_slots_sentence_shape[1],
+        ])
         assert (
             batch_intent_sentence_shape[1] == seq_len
             or batch_intent_sentence_shape[1] == 0
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/policies/test_unexpected_intent_policy.py#L181'>tests/core/policies/test_unexpected_intent_policy.py~L181</a>
```diff
     def test_similarities_collection_for_label_ids(self):
         label_ids = np.array([[0, 1], [1, -1], [2, -1]])
         outputs = {
-            "similarities": np.array(
-                [[[1.2, 0.3, 0.2]], [[0.5, 0.2, 1.6]], [[0.01, 0.1, 1.7]]]
-            )
+            "similarities": np.array([
+                [[1.2, 0.3, 0.2]],
+                [[0.5, 0.2, 1.6]],
+                [[0.01, 0.1, 1.7]],
+            ])
         }
         label_id_similarities = UnexpecTEDIntentPolicy._collect_label_id_grouped_scores(
             outputs, label_ids
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/policies/test_unexpected_intent_policy.py#L801'>tests/core/policies/test_unexpected_intent_policy.py~L801</a>
```diff
         # `-1` is used as padding label id. The embedding for it
         # will be the same as `label_id=0`
         expected_extracted_label_embeddings = tf.constant(
-            np.concatenate(
-                [
-                    all_label_embeddings[2],
-                    all_label_embeddings[0],
-                    all_label_embeddings[1],
-                    all_label_embeddings[2],
-                    all_label_embeddings[0],
-                    all_label_embeddings[0],
-                ]
-            ).reshape((3, 2, 20)),
+            np.concatenate([
+                all_label_embeddings[2],
+                all_label_embeddings[0],
+                all_label_embeddings[1],
+                all_label_embeddings[2],
+                all_label_embeddings[0],
+                all_label_embeddings[0],
+            ]).reshape((3, 2, 20)),
             dtype=tf.float32,
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions.py#L347'>tests/core/test_actions.py~L347</a>
```diff
         ],
     }
 
-    nlg = TemplatedNaturalLanguageGenerator(
-        {"utter_ask_cuisine": [{"text": "what dou want to eat?"}]}
-    )
+    nlg = TemplatedNaturalLanguageGenerator({
+        "utter_ask_cuisine": [{"text": "what dou want to eat?"}]
+    })
     with aioresponses() as mocked:
         mocked.post("https://example.com/webhooks/actions", payload=response)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions.py#L1375'>tests/core/test_actions.py~L1375</a>
```diff
     form_name = "test_form"
     entity_name = "some_slot"
 
-    domain = Domain.from_dict(
-        {
-            "intents": ["greet"],
-            "entities": ["some_slot"],
-            "slots": {entity_name: {"type": "text", "mappings": [slot_mapping]}},
-            "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
-        }
-    )
+    domain = Domain.from_dict({
+        "intents": ["greet"],
+        "entities": ["some_slot"],
+        "slots": {entity_name: {"type": "text", "mappings": [slot_mapping]}},
+        "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
+    })
 
     tracker = DialogueStateTracker.from_events(
         "default",
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions.py#L1493'>tests/core/test_actions.py~L1493</a>
```diff
     form_name = "some_form"
     entity_name = "some_slot"
 
-    domain = Domain.from_dict(
-        {
-            "slots": {entity_name: {"type": "text", "mappings": [slot_mapping]}},
-            "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
-        }
-    )
+    domain = Domain.from_dict({
+        "slots": {entity_name: {"type": "text", "mappings": [slot_mapping]}},
+        "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
+    })
 
     tracker = DialogueStateTracker.from_events(
         "default",
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions.py#L1773'>tests/core/test_actions.py~L1773</a>
```diff
         intent=mapping_intent,
         not_intent=mapping_not_intent,
     )
-    domain = Domain.from_dict(
-        {
-            "entities": ["some_entity"],
-            "slots": {"some_slot": {"type": "any", "mappings": [mapping]}},
-            "forms": {form_name: {REQUIRED_SLOTS_KEY: ["some_slot"]}},
-        }
-    )
+    domain = Domain.from_dict({
+        "entities": ["some_entity"],
+        "slots": {"some_slot": {"type": "any", "mappings": [mapping]}},
+        "forms": {form_name: {REQUIRED_SLOTS_KEY: ["some_slot"]}},
+    })
 
     tracker = DialogueStateTracker.from_events(
         "default",
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions.py#L1897'>tests/core/test_actions.py~L1897</a>
```diff
     entity_name = "some_slot"
     slot_filled_by_trigger_mapping = "other_slot"
 
-    domain = Domain.from_dict(
-        {
-            "slots": {
-                entity_name: {
-                    "type": "text",
-                    "mappings": [
-                        {
-                            "type": "from_entity",
-                            "entity": entity_name,
-                            "intent": "some_intent",
-                        }
-                    ],
-                },
-                slot_filled_by_trigger_mapping: {
-                    "type": "text",
-                    "mappings": [trigger_slot_mapping],
-                },
+    domain = Domain.from_dict({
+        "slots": {
+            entity_name: {
+                "type": "text",
+                "mappings": [
+                    {
+                        "type": "from_entity",
+                        "entity": entity_name,
+                        "intent": "some_intent",
+                    }
+                ],
             },
-            "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
-        }
-    )
+            slot_filled_by_trigger_mapping: {
+                "type": "text",
+                "mappings": [trigger_slot_mapping],
+            },
+        },
+        "forms": {form_name: {REQUIRED_SLOTS_KEY: [entity_name]}},
+    })
 
     tracker = DialogueStateTracker.from_events(
         "default",
```
<a href='https://github.com/RasaHQ/rasa/blob/444c197625f614fbf1d6c7bed5dfe8add8400e14/tests/core/test_actions...*[Comment body truncated]*

---

_Comment by @zanieb on 2023-11-29 04:10_

Looking through the ecosystem changes is interesting. I feel like it's fine until you have something like this

```diff
                                     )
                                 )
                                 if commented_entity:
-                                    entity_map = CommentedMap(
-                                        [(entity["entity"], entity["value"])]
-                                    )
+                                    entity_map = CommentedMap([(
+                                        entity["entity"],
+                                        entity["value"],
+                                    )])
                                     entity_map.yaml_add_eol_comment(
                                         commented_entity, entity["entity"]
                                     )
                                     entities.append(entity_map)
                                 else:
                                     entities.append(
-                                        OrderedDict(
-                                            [(entity["entity"], entity["value"])]
-                                        )
+                                        OrderedDict([(
+                                            entity["entity"],
+                                            entity["value"],
+                                        )])
                                     )
                     else:
                         entities.append(
```

I guess maybe I'm not into the recursive hugging, it makes it less clear what's going on.

---

_Comment by @MichaReiser on 2023-11-29 04:13_

> I guess maybe I'm not into the recursive hugging, it makes it less clear what's going on.

Yeah, the recursive hugging took me by surprise. I don't mind hugging the first list, but recursively feels a bit excessive. 

---

_Comment by @charliermarsh on 2023-11-29 04:14_

What is "the first list"? Like, a list within a list, but no deeper?

---

_Comment by @MichaReiser on 2023-11-29 04:17_

> What is "the first list"? Like, a list within a list, but no deeper?

Just the top-level list, similar to what Prettier does (yeah, I'm a prettier fan-boy)

```
foo([
  [[1, 2, 3, aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa]],
])
```

---

_Comment by @zanieb on 2023-11-29 04:18_

Yeah I agree. Perhaps we should take that feedback over to Black?

---

_@charliermarsh reviewed on 2023-11-29 04:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:137 on 2023-11-29 04:23_

Yes, I believe this path isn't necessary since we don't want to hug when the expression has leading or trailing comments.

---

_Comment by @charliermarsh on 2023-11-29 04:23_

Ok, I can back out the recursive cases for now (I only added for lists anyway).

---

_Marked ready for review by @charliermarsh on 2023-11-29 04:51_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-11-29 04:51_

---

_Comment by @charliermarsh on 2023-11-29 04:58_

Trying to understand why Poetry's similarity index changed here. Ohh, it looks like we read the Black options from `pyproject.toml` in `format-dev`? So we _are_ running with our `--preview` behavior. That makes more sense.

---

_Comment by @charliermarsh on 2023-11-29 17:08_

@MichaReiser - Let me know if you'd like to review.

---

_@MichaReiser approved on 2023-12-01 01:59_

This seems a clear improvement to me. Let's keep the preview style issue open for now and let's document that we don't yet support Black's recursive collapsing.

---

_Merged by @charliermarsh on 2023-12-01 02:11_

---

_Closed by @charliermarsh on 2023-12-01 02:11_

---

_Branch deleted on 2023-12-01 02:11_

---

_Comment by @charliermarsh on 2023-12-01 02:12_

Thanks @MichaReiser! I left the issue open, will defer to you if you choose to close.

---

_Comment by @ofek on 2023-12-05 18:08_

This looks fantastic, thank you! https://github.com/pypa/hatch/pull/1085

---
