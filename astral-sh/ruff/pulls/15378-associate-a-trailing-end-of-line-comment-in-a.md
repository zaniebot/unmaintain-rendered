```yaml
number: 15378
title: Associate a trailing end-of-line comment in a parenthesized implicit concatenated string with the last literal
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/fix-implicit-concat-string-trailing-part-comment
created_at: 2025-01-09T17:50:53Z
updated_at: 2025-01-10T18:21:36Z
url: https://github.com/astral-sh/ruff/pull/15378
synced_at: 2026-01-10T20:34:00Z
```

# Associate a trailing end-of-line comment in a parenthesized implicit concatenated string with the last literal

---

_Pull request opened by @MichaReiser on 2025-01-09 17:50_

## Summary

This PR implements a fix for the implicit concatenated string formatting. Specifically for strings where the last part has a trailing end-of-line comment and they're in the value position of an assignment (including type alias) or return statement.

For example:

```py
self._attr_unique_id = (
    f"{self._device.temperature.group_address_state}_"
    f"{self._device.target_temperature.group_address_state}_"
    f"{self._device.target_temperature.group_address}_"
    f"{self._device._setpoint_shift.group_address}"  # noqa: SLF001
)
```

got reformatted to 

```py
self._attr_unique_id = (
    f"{self._device.temperature.group_address_state}_"
    f"{self._device.target_temperature.group_address_state}_"
    f"{self._device.target_temperature.group_address}_"
    f"{self._device._setpoint_shift.group_address}"
)  # noqa: SLF001
```

which is undesired because the `noqa` comment is now misplaced. It's also impossible to move it back to the right place because the formatter keeps moving it out of the parentheses. 

The reason this is happening is because we try to match Black's formatting for assignments:

```py
a = (
	"a" "b"  # comment
)

# Get's formatted to 
a = "ab"  # comment
```

Notice how the comment gets moved out. 

The relevant logic for this behavior lives here

https://github.com/astral-sh/ruff/blob/424b720c192f8603123c1a105cf12f343e45de3f/crates/ruff_python_formatter/src/statement/stmt_assign.rs#L349-L449

and `inline_comments` stores all the comments that should either be moved in or outside the parentheses. 

Ideally, we would adjust this logic (and it's counterpart in https://github.com/astral-sh/ruff/blob/424b720c192f8603123c1a105cf12f343e45de3f/crates/ruff_python_formatter/src/statement/stmt_assign.rs#L759-L908) but this is going to be fairly involved and is something I consider too risky for a bugfix release. 

That's why I decided to instead implement it in `place_comment` and associate a comment in a parenthesized f-string expression that's on the same line as the last part with said part, but only if the last part is on its own line. 

This has two downsides:

1. Ideally, we'd collapse the following but we'll no longer be able to do that with the changes from this PR:
    ```py
    a = (
        "a"
        "b" # comment
    )
    
    # could be formatted to
    a = "ab" # comment
    ```

    This used to work before and works for implicit concatenated strings outside of assignment-value positions. I think this is acceptable.
2. This is directly related to the above limitation. I made this behavior specific to implicit concatenated strings in assignment value positions or return statements. That means that the behavior is somewhat inconsistent to how we e.g. handle implicit concatenated strings embedded in another expression (you can see an example in the snapshot changes)

I'm open to changing the behavior to all implicit concatenated strings. It might be the right move, but I'm not sure. There are trade offs ;) 


Fixes https://github.com/astral-sh/ruff/issues/15375

## Test Plan

Added snapshots

The ecosystem changes look good to me. 

---

_Label `bug` added by @MichaReiser on 2025-01-09 17:51_

---

_Label `formatter` added by @MichaReiser on 2025-01-09 17:51_

---

_Comment by @MichaReiser on 2025-01-09 17:54_

It might actually be better to handle this inside `FormatStatementsLastExpression::left_to_right` but I have to take another look how comments in assignments are handled (it's complicated)

---

_Comment by @github-actions[bot] on 2025-01-09 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+12 -12 lines in 6 files in 2 projects; 1 project error; 52 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/facfd42768485d92777608e5d62d705550c1a5c4/libs/core/tests/unit_tests/utils/test_html.py#L166'>libs/core/tests/unit_tests/utils/test_html.py~L166</a>
```diff
         '<a href="https://foobar.com:9999">BAD</a>'
         '<a href="http://foobar.com:9999/">BAD</a>'
         '<a href="https://foobar.com/OK">OK</a>'
-        '<a href="http://foobar.com/BAD">BAD</a>'
-    )  # Change in scheme is not OK here
+        '<a href="http://foobar.com/BAD">BAD</a>'  # Change in scheme is not OK here
+    )
 
     expected = sorted(
         [
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -10 lines across 5 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/history_events.py#L362'>rotkehlchen/db/history_events.py~L362</a>
```diff
         free_base_suffix = (
             f"* FROM (SELECT {base_suffix}) WHERE event_identifier IN ("
             f"SELECT DISTINCT event_identifier FROM history_events {ignore_asset_filter} "
-            "ORDER BY timestamp DESC,sequence_index ASC LIMIT ?)"
-        )  # free query only select the last LIMIT groups  # noqa: E501
+            "ORDER BY timestamp DESC,sequence_index ASC LIMIT ?)"  # free query only select the last LIMIT groups  # noqa: E501
+        )
 
         if has_premium:
             suffix, limit = premium_base_suffix, []
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v42_v43.py#L83'>rotkehlchen/db/upgrades/v42_v43.py~L83</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v43_v44.py#L222'>rotkehlchen/db/upgrades/v43_v44.py~L222</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v44_v45.py#L46'>rotkehlchen/db/upgrades/v44_v45.py~L46</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v45_v46.py#L187'>rotkehlchen/db/upgrades/v45_v46.py~L187</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+12 -12 lines in 6 files in 2 projects; 1 project error; 52 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/facfd42768485d92777608e5d62d705550c1a5c4/libs/core/tests/unit_tests/utils/test_html.py#L158'>libs/core/tests/unit_tests/utils/test_html.py~L158</a>
```diff
         '<a href="https://foobar.com:9999">BAD</a>'
         '<a href="http://foobar.com:9999/">BAD</a>'
         '<a href="https://foobar.com/OK">OK</a>'
-        '<a href="http://foobar.com/BAD">BAD</a>'
-    )  # Change in scheme is not OK here
+        '<a href="http://foobar.com/BAD">BAD</a>'  # Change in scheme is not OK here
+    )
 
     expected = sorted([
         "https://foobar.com/OK",
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -10 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/history_events.py#L362'>rotkehlchen/db/history_events.py~L362</a>
```diff
         free_base_suffix = (
             f"* FROM (SELECT {base_suffix}) WHERE event_identifier IN ("
             f"SELECT DISTINCT event_identifier FROM history_events {ignore_asset_filter} "
-            "ORDER BY timestamp DESC,sequence_index ASC LIMIT ?)"
-        )  # free query only select the last LIMIT groups  # noqa: E501
+            "ORDER BY timestamp DESC,sequence_index ASC LIMIT ?)"  # free query only select the last LIMIT groups  # noqa: E501
+        )
 
         if has_premium:
             suffix, limit = premium_base_suffix, []
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v42_v43.py#L83'>rotkehlchen/db/upgrades/v42_v43.py~L83</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v43_v44.py#L220'>rotkehlchen/db/upgrades/v43_v44.py~L220</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v44_v45.py#L46'>rotkehlchen/db/upgrades/v44_v45.py~L46</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/4a99f573d6b3ada401114f262dd21da14111ed7f/rotkehlchen/db/upgrades/v45_v46.py#L187'>rotkehlchen/db/upgrades/v45_v46.py~L187</a>
```diff
                 "DELETE FROM history_events WHERE identifier IN ("
                 "SELECT H.identifier from history_events H INNER JOIN evm_events_info E "
                 "ON H.identifier=E.identifier AND E.tx_hash IN "
-                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"
-            )  # location 'o' is zksync lite  # noqa: E501
+                "(SELECT tx_hash FROM evm_transactions) AND H.location != 'o')"  # location 'o' is zksync lite  # noqa: E501
+            )
             bindings: tuple = ()
             if customized_events != 0:
                 querystr += " AND identifier NOT IN (SELECT parent_identifier FROM history_events_mappings WHERE name=? AND value=?)"  # noqa: E501
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-01-10 10:24_

---

_Review requested from @charliermarsh by @MichaReiser on 2025-01-10 10:24_

---

_Review request for @charliermarsh removed by @MichaReiser on 2025-01-10 10:24_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-10 10:24_

---

_Review requested from @charliermarsh by @MichaReiser on 2025-01-10 10:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_assignment.py`:346 on 2025-01-10 11:19_

I understand the change but I want to confirm as to why does this comment belongs to the f-string expression. I think the reason is that without that, the formatting might move the last part of the string to it's own line along with the comment which means the comment won't be applicable for the last second part. Is that correct?

---

_@dhruvmanila reviewed on 2025-01-10 11:19_

---

_@dhruvmanila reviewed on 2025-01-10 11:22_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:358 on 2025-01-10 11:22_

Do we need to cover other statements like `raise` and `assert`?

```python
raise Exception(
    "a"
    "b"  # belongs to `b`
)

assert False, (
    "a"
    "b"  # belongs to `b`
)
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_assignment.py`:361 on 2025-01-10 11:28_

It might be useful to shorten these strings (similar to other test cases) just to make sure that the formatter isn't going to collapse them because I think otherwise the line length won't even allow this to be tested.

---

_@dhruvmanila reviewed on 2025-01-10 11:28_

---

_@MichaReiser reviewed on 2025-01-10 12:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:358 on 2025-01-10 12:17_

No, only statements that use `FormatStatementsLastExpression`

---

_@MichaReiser reviewed on 2025-01-10 12:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_assignment.py`:346 on 2025-01-10 12:21_

This is an edge case where there's no "right" solution because the formatter can't tell if the comment belongs to the middle part, the last part, or the entire f-string. 

That's why I decided to associate it with the f-string. This matches the default behavior where we associate a comment with the outer most node that ends right before it. We could also decide to associate it with the last part, if we think this is more appropriate.

---

_@dhruvmanila reviewed on 2025-01-10 13:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_assignment.py`:346 on 2025-01-10 13:09_

Yeah, that seems like a reasonable default. It might be worth adding this as a comment to that case.

---

_@dhruvmanila approved on 2025-01-10 13:13_

---

_Renamed from "Keep trailing part comment associated with its part" to "Associate a trailing end-of-line comment in a parenthesized implicit concatenated string with the last literal" by @MichaReiser on 2025-01-10 13:20_

---

_@charliermarsh approved on 2025-01-10 14:16_

This seems like a wise behavior change. Is the intent that we'd eventually do the thing that you called "too risky for a bugfix release", so that we _can_ collapse these in some cases?

---

_Comment by @MichaReiser on 2025-01-10 16:50_

> This seems like a wise behavior change. Is the intent that we'd eventually do the thing that you called "too risky for a bugfix release", so that we can collapse these in some cases?

Possibly. I think it depends on how often it comes up and if it's worth the extra complexity. A possible other direction is to remove the custom assignment logic altogether.... but that would come at the cost of black compatibility and is something we should consider carefully. 

---

_Merged by @MichaReiser on 2025-01-10 18:21_

---

_Closed by @MichaReiser on 2025-01-10 18:21_

---

_Branch deleted on 2025-01-10 18:21_

---
