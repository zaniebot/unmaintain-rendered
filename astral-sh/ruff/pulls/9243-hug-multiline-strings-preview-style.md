```yaml
number: 9243
title: Hug multiline-strings preview style
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: multiline-string
created_at: 2023-12-22T09:19:43Z
updated_at: 2024-01-10T12:37:51Z
url: https://github.com/astral-sh/ruff/pull/9243
synced_at: 2026-01-12T15:55:28Z
```

# Hug multiline-strings preview style

---

_@MichaReiser_

## Summary

This PR implements the hugging for multiline strings in call expression. 

The implementation doesn't match black's behavior exactly yet:

1. Black removes the hugging for nested calls `call(nested("""multiline\nstring"))`. Ruff will not to ensure consistency with `hug_parentheses` 
2. Black applies the hugging even for `"multiline" % placeholder` strings. This can get pretty hard to read if the formatting has many arguments. The solution here is to use f-strings instead.
3. Black avoids parenthesizing lambdas with a multiline string body, Ruff parenthesizes it. 

I find 1 and 2 acceptable and wouldn't support them. 3. is a pre-existing deviation that we should tackle separately.

## Test Plan

* I added tests documenting the deviations
* I reviewed the snapshot changes
* This PR increases the poetry compatbility from 0.99905 to 0.99934
* I reviewed the ecosystem changes. I like what I see. I only noticed that the last line of the multiline string now tends to have one indent too much. There isn't anything we can do about it without knowing if it is safe to trim the last string or not (depends on the called function). This is something that requires manual intervention. 

```python
a = call(
	"""
	Multiline string
	"""
)
```

becomes

```python
a = call("""
	Multiline string
	""")

```


---

_Label `formatter` added by @MichaReiser on 2023-12-22 09:24_

---

_Label `preview` added by @MichaReiser on 2023-12-22 09:24_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-22 09:26_

---

_Review requested from @konstin by @MichaReiser on 2023-12-22 09:26_

---

_Comment by @github-actions[bot] on 2023-12-22 09:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+5 -6 lines in 2 files in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+2 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --no-preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/33a4829031d36f83595165e9f2beb8376c8a98e0/Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py#L107'>Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py~L107</a>
```diff
     )
 
     if is_error(response):
-        error = (
-            f'Failed retrieving tasks for incident ID {incident["id"]}.\n \
+        error = f'Failed retrieving tasks for incident ID {incident["id"]}.\n \
            Make sure that the API key configured in the Core REST API integration \
-is one with sufficient permissions to access that incident.\n'
-            + get_error(response)
-        )
+is one with sufficient permissions to access that incident.\n' + get_error(response)
         raise Exception(error)
 
     return response[0]["Contents"]["response"]
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/7cec7feb63af7c489474c18894ce3ca262f69103/reflex/components/markdown/markdown.py#L259'>reflex/components/markdown/markdown.py~L259</a>
```diff
         }
 
         # Separate out inline code and code blocks.
-        components["code"] = f"""{{({{node, inline, className, {_CHILDREN._var_name}, {_PROPS._var_name}}}) => {{
+        components[
+            "code"
+        ] = f"""{{({{node, inline, className, {_CHILDREN._var_name}, {_PROPS._var_name}}}) => {{
     const match = (className || '').match(/language-(?<lang>.*)/);
     const language = match ? match[1] : '';
     if (language) {{
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+2971 -5900 lines in 374 files in 31 projects; 12 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+326 -652 lines across 19 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/telemetry.py#L105'>rasa/telemetry.py~L105</a>
```diff
 
 def print_telemetry_reporting_info() -> None:
     """Print telemetry information to std out."""
-    message = textwrap.dedent(
-        f"""
+    message = textwrap.dedent(f"""
         Rasa Open Source reports anonymous usage telemetry to help improve the product
         for all its users.
 
         If you'd like to opt-out, you can use `rasa telemetry disable`.
-        To learn more, check out {DOCS_URL_TELEMETRY}."""
-    ).strip()
+        To learn more, check out {DOCS_URL_TELEMETRY}.""").strip()
 
     table = SingleTable([[message]])
     print(table.table)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L357'>tests/cli/test_utils.py~L357</a>
```diff
     file_type: Text, data_type: Text, tmp_path: Path
 ):
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(
-        f"""
+    file_name.write_text(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
           steps:
           - intent: goodbye
           - action: action_test
-        """
-    )
+        """)
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L389'>tests/cli/test_utils.py~L389</a>
```diff
     file_type: Text, data_type: Text, tmp_path: Path
 ):
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(
-        f"""
+    file_name.write_text(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L398'>tests/cli/test_utils.py~L398</a>
```diff
             - intent: request_restaurant
             - action: restaurant_form
             - active_loop: restaurant_form
-        """
-    )
+        """)
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L425'>tests/cli/test_utils.py~L425</a>
```diff
     )
     nlu_file = "data/test_nlu/test_nlu_validate_files_with_active_loop_null.yml"
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(
-        f"""
+    file_name.write_text(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L436'>tests/cli/test_utils.py~L436</a>
```diff
             - active_loop: restaurant_form
             - active_loop: null
             - action: action_search_restaurants
-        """
-    )
+        """)
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_unique_assistant_id.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L464'>tests/cli/test_utils.py~L464</a>
```diff
 
 def test_validate_files_form_slots_not_matching(tmp_path: Path):
     domain_file_name = tmp_path / "domain.yml"
-    domain_file_name.write_text(
-        f"""
+    domain_file_name.write_text(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         forms:
           name_form:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L481'>tests/cli/test_utils.py~L481</a>
```diff
                 type: text
                 mappings:
                 - type: from_text
-        """
-    )
+        """)
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L536'>tests/cli/test_utils.py~L536</a>
```diff
     tested_slot = "duration"
     form_name = "booking_form"
     # form required_slots does not include the tested_slot
-    domain.write_text(
-        f"""
+    domain.write_text(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - state_length_of_time
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L561'>tests/cli/test_utils.py~L561</a>
```diff
               {form_name}:
                 required_slots:
                 - location
-                """
-    )
+                """)
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml", str(domain), None
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L44'>tests/core/actions/test_forms.py~L44</a>
```diff
     form_name = "my form"
     action = FormAction(form_name, None)
     slot_name = "num_people"
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L59'>tests/core/actions/test_forms.py~L59</a>
```diff
     responses:
       utter_ask_num_people:
       - text: "How many people?"
-      """
-    )
+      """)
     domain = Domain.from_yaml(domain)
 
     events = await action.run(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L81'>tests/core/actions/test_forms.py~L81</a>
```diff
     domain_required_slot_name = "num_people"
     slot_set_by_remote_custom_extraction_method = "some_slot"
     slot_value_set_by_remote_custom_extraction_method = "anything"
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     slots:
       {domain_required_slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L102'>tests/core/actions/test_forms.py~L102</a>
```diff
       - text: "How many people?"
     actions:
       - validate_{form_name}
-      """
-    )
+      """)
     domain = Domain.from_yaml(domain)
 
     form_validation_events = [
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L141'>tests/core/actions/test_forms.py~L141</a>
```diff
     form_name = "my form"
     action = FormAction(form_name, None)
     slot_name = "num_people"
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L158'>tests/core/actions/test_forms.py~L158</a>
```diff
     responses:
       utter_ask_num_people:
       - text: "How many people?"
-      """
-    )
+      """)
     domain = Domain.from_yaml(domain)
 
     events = await action.run(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L869'>tests/core/actions/test_forms.py~L869</a>
```diff
 def test_temporary_tracker():
     extra_slot = "some_slot"
     sender_id = "test"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         slots:
           {extra_slot}:
             type: any
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     previous_events = [ActionExecuted(ACTION_LISTEN_NAME)]
     old_tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1543'>tests/core/actions/test_forms.py~L1543</a>
```diff
     form = FormAction(form_name, None)
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1573'>tests/core/actions/test_forms.py~L1573</a>
```diff
                required_slots:
                  - email
                  - name
-            """
-        )
+            """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1615'>tests/core/actions/test_forms.py~L1615</a>
```diff
     form = FormAction(form_name, None)
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1645'>tests/core/actions/test_forms.py~L1645</a>
```diff
                required_slots:
                  - email
                  - name
-            """
-        )
+            """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1685'>tests/core/actions/test_forms.py~L1685</a>
```diff
 
 
 async def test_action_extract_slots_custom_mapping_with_condition():
-    domain_yaml = textwrap.dedent(
-        f"""
+    domain_yaml = textwrap.dedent(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
 
         slots:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1705'>tests/core/actions/test_forms.py~L1705</a>
```diff
 
         actions:
         - validate_some_form
-        """
-    )
+        """)
     domain = Domain.from_yaml(domain_yaml)
     events = [ActiveLoop("some_form"), UserUttered("Hi")]
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1747'>tests/core/actions/test_forms.py~L1747</a>
```diff
 
 async def test_form_slots_empty_with_restart():
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1768'>tests/core/actions/test_forms.py~L1768</a>
```diff
              some_form:
                required_slots:
                  - name
-            """
-        )
+            """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1811'>tests/core/actions/test_forms.py~L1811</a>
```diff
 
     form_name = "test_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
     entities:
     - {entity_name}
     slots:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1827'>tests/core/actions/test_forms.py~L1827</a>
```diff
       {form_name}:
         {REQUIRED_SLOTS_KEY}:
             - {slot_name}
-    """
-    )
+    """)
 
     events = [
         ActionExecuted("action_listen"),
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1879'>tests/core/actions/test_forms.py~L1879</a>
```diff
     action_server = EndpointConfig(ACTION_SERVER_URL)
     action = FormAction(form_name, action_server)
     slot_name = "num_people"
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1896'>tests/core/actions/test_forms.py~L1896</a>
```diff
       - text: "How many people?"
     actions:
       - validate_{form_name}
-    """
-    )
+    """)
     domain = Domain.from_yaml(domain)
     form_validation_events = [
         {
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1949'>tests/core/actions/test_forms.py~L1949</a>
```diff
     required_slot = "send_sms"
     global_slot = "membership"
 
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     intents:
     - start_form
     entities:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1988'>tests/core/actions/test_forms.py~L1988</a>
```diff
       - text: "Would you like to receive an SMS?"
     actions:
       - validate_{form}
-    """
-    )
+    """)
     domain = Domain.from_yaml(domain)
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L2055'>tests/core/actions/test_forms.py~L2055</a>
```diff
     dynamic_slot = "customer_name"
     bot_utterance = "What is your name?"
 
-    domain = textwrap.dedent(
-        f"""
+    domain = textwrap.dedent(f"""
     intents:
     - start_form
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L2075'>tests/core/actions/test_forms.py~L2075</a>
```diff
       - text: "{bot_utterance}"
     actions:
       - validate_{form}
-    """
-    )
+    """)
     domain = Domain.from_yaml(domain)
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L157'>tests/core/policies/test_rule_policy.py~L157</a>
```diff
     # conversation start -> ensure that this isn't flagged as a contradiction.
 
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """
-    )
+        """)
 
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L210'>tests/core/policies/test_rule_policy.py~L210</a>
```diff
     utter_anti_greet_action = "utter_anti_greet"
     some_slot = "slot1"
     some_slot_initial_value = "slot1value"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L224'>tests/core/policies/test_rule_policy.py~L224</a>
```diff
             initial_value: {some_slot_initial_value}
             mappings:
             - type: from_text
-        """
-    )
+        """)
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L283'>tests/core/policies/test_rule_policy.py~L283</a>
```diff
     utter_anti_greet_action = "utter_anti_greet"
     some_slot = "slot1"
     some_slot_initial_value = "slot1value"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L297'>tests/core/policies/test_rule_policy.py~L297</a>
```diff
             initial_value: {some_slot_initial_value}
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L348'>tests/core/policies/test_rule_policy.py~L348</a>
```diff
 
 
 def test_restrict_multiple_user_inputs_in_rules(policy: RulePolicy):
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
 
     greet_events = [
         UserUttered(intent={"name": GREET_INTENT_NAME}),
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L380'>tests/core/policies/test_rule_policy.py~L380</a>
```diff
 def test_incomplete_rules_due_to_slots(policy: RulePolicy):
     some_action = "some_action"
     some_slot = "some_slot"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L392'>tests/core/policies/test_rule_policy.py~L392</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L450'>tests/core/policies/test_rule_policy.py~L450</a>
```diff
 def test_no_incomplete_rules_due_to_slots_after_listen(policy: RulePolicy):
     some_action = "some_action"
     some_slot = "some_slot"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L464'>tests/core/policies/test_rule_policy.py~L464</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L512'>tests/core/policies/test_rule_policy.py~L512</a>
```diff
     some_slot_value = "value1"
     some_other_slot = "some_other_slot"
     some_other_slot_value = "value2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L528'>tests/core/policies/test_rule_policy.py~L528</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     simple_rule = TrackerWithCachedStates.from_events(
         "simple rule with an action that sets 1 slot",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L567'>tests/core/policies/test_rule_policy.py~L567</a>
```diff
 
 def test_incomplete_rules_due_to_loops(policy: RulePolicy):
     some_form = "some_form"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         forms:
           {some_form}:
             required_slots: []
-        """
-    )
+        """)
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L632'>tests/core/policies/test_rule_policy.py~L632</a>
```diff
 
 def test_contradicting_rules(policy: RulePolicy):
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """
-    )
+        """)
 
     anti_greet_rule = TrackerWithCachedStates.from_events(
         "anti greet rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L672'>tests/core/policies/test_rule_policy.py~L672</a>
```diff
 
 def test_contradicting_rules_and_stories(policy: RulePolicy):
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """
-    )
+        """)
 
     anti_greet_story = TrackerWithCachedStates.from_events(
         "anti greet story",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L816'>tests/core/policies/test_rule_policy.py~L816</a>
```diff
 
 
 def test_faq_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
     # remove first ... action and utter_greet and last action_listen from greet rule
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L843'>tests/core/policies/test_rule_policy.py~L843</a>
```diff
 async def test_predict_form_action_if_in_form(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L859'>tests/core/policies/test_rule_policy.py~L859</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L888'>tests/core/policies/test_rule_policy.py~L888</a>
```diff
 async def test_predict_loop_action_if_in_loop_but_there_is_e2e_rule(policy: RulePolicy):
     loop_name = "some_loop"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L904'>tests/core/policies/test_rule_policy.py~L904</a>
```diff
         forms:
           {loop_name}:
             required_slots: []
-        """
-    )
+        """)
     e2e_rule = TrackerWithCachedStates.from_events(
         "bla",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L944'>tests/core/policies/test_rule_policy.py~L944</a>
```diff
 async def test_predict_form_action_if_multiple_turns(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L961'>tests/core/policies/test_rule_policy.py~L961</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L994'>tests/core/policies/test_rule_policy.py~L994</a>
```diff
 
 
 async def test_predict_slot_initial_value_not_required_in_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(
-        """
+    domain = Domain.from_yaml("""
 intents:
 - i1
 actions:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1009'>tests/core/policies/test_rule_policy.py~L1009</a>
```diff
     initial_value: v1
     mappings:
     - type: from_text
-"""
-    )
+""")
 
     rule = DialogueStateTracker.from_events(
         "slot rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1044'>tests/core/policies/test_rule_policy.py~L1044</a>
```diff
 
 
 async def test_predict_slot_with_initial_slot_matches_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(
-        """
+    domain = Domain.from_yaml("""
 intents:
 - i1
 actions:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1059'>tests/core/policies/test_rule_policy.py~L1059</a>
```diff
     initial_value: v1
     mappings:
     - type: from_text
-"""
-    )
+""")
 
     rule = DialogueStateTracker.from_events(
         "slot rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1093'>tests/core/policies/test_rule_policy.py~L1093</a>
```diff
 async def test_predict_action_listen_after_form(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1109'>tests/core/policies/test_rule_policy.py~L1109</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1140'>tests/core/policies/test_rule_policy.py~L1140</a>
```diff
 async def test_dont_predict_form_if_already_finished(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1156'>tests/core/policies/test_rule_policy.py~L1156</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1191'>tests/core/policies/test_rule_policy.py~L1191</a>
```diff
 async def test_form_unhappy_path(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1207'>tests/core/policies/test_rule_policy.py~L1207</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1236'>tests/core/policies/test_rule_policy.py~L1236</a>
```diff
 async def test_form_unhappy_path_from_general_rule(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1252'>tests/core/policies/test_rule_policy.py~L1252</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     # RulePolicy should memorize that unhappy_rule overrides GREET_RULE
     policy.train([GREET_RULE], domain)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1293'>tests/core/policies/test_rule_policy.py~L1293</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1310'>tests/core/policies/test_rule_policy.py~L1310</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     unhappy_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1369'>tests/core/policies/test_rule_policy.py~L1369</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1386'>tests/core/policies/test_rule_policy.py~L1386</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     unhappy_story = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1446'>tests/core/policies/test_rule_policy.py~L1446</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1463'>tests/core/policies/test_rule_policy.py~L1463</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     unhappy_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1537'>tests/core/policies/test_rule_policy.py~L1537</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1554'>tests/core/policies/test_rule_policy.py~L1554</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     unhappy_story = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1607'>tests/core/policies/test_rule_policy.py~L1607</a>
```diff
 async def test_form_unhappy_path_without_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1624'>tests/core/policies/test_rule_policy.py~L1624</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1653'>tests/core/policies/test_rule_policy.py~L1653</a>
```diff
 async def test_form_activation_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1670'>tests/core/policies/test_rule_policy.py~L1670</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     form_activation_rule = _form_activation_rule(domain, form_name, other_intent)
     policy.train([GREET_RULE, form_activation_rule], domain)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1694'>tests/core/policies/test_rule_policy.py~L1694</a>
```diff
 async def test_failing_form_activation_due_to_no_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1711'>tests/core/policies/test_rule_policy.py~L1711</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1735'>tests/core/policies/test_rule_policy.py~L1735</a>
```diff
 def test_form_submit_rule(policy: RulePolicy):
     form_name = "some_form"
     submit_action_name = "utter_submit"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1752'>tests/core/policies/test_rule_policy.py~L1752</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     form_submit_rule = _form_submit_rule(domain, submit_action_name, form_name)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1789'>tests/core/policies/test_rule_policy.py~L1789</a>
```diff
     submit_action_name = "utter_submit"
     entity = "some_entity"
     slot = "some_slot"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1812'>tests/core/policies/test_rule_policy.py~L1812</a>
```diff
             required_slots: []
         entities:
         - {entity}
-        """
-    )
+        """)
 
     form_activation_rule = _form_activation_rule(domain, form_name, GREET_INTENT_NAME)
     form_submit_rule = _form_submit_rule(domain, submit_action_name, form_name)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1895'>tests/core/policies/test_rule_policy.py~L1895</a>
```diff
 
 
 async def test_one_stage_fallback_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         - {DEFAULT_NLU_FALLBACK_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
 
     fallback_recover_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1978'>tests/core/policies/test_rule_policy.py~L1978</a>
```diff
 def test_default_actions(
     intent_name: Text, expected_action_name: Text, policy: RulePolicy
 ):
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
     policy.train([GREET_RULE], domain)
     new_conversation = DialogueStateTracker.from_events(
         "bla2",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2006'>tests/core/policies/test_rule_policy.py~L2006</a>
```diff
     "intent_name", [USER_INTENT_RESTART, USER_INTENT_BACK, USER_INTENT_SESSION_START]
 )
 def test_e2e_beats_default_actions(intent_name: Text, policy: RulePolicy):
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
 
     e2e_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2067'>tests/core/policies/test_rule_policy.py~L2067</a>
```diff
     expected_prediction: Text,
 ):
     other_intent = "other"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2076'>tests/core/policies/test_rule_policy.py~L2076</a>
```diff
         actions:
         - {UTTER_GREET_ACTION}
         - my_core_fallback
-        """
-    )
+        """)
     rule_policy = policy_with_config(config)
     rule_policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2100'>tests/core/policies/test_rule_policy.py~L2100</a>
```diff
     policy_with_config: Callable[..., RulePolicy],
 ):
     other_intent = "other"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         - {other_intent}
         actions:
         - {UTTER_GREET_ACTION}
-        """
-    )
+        """)
     policy = policy_with_config({"enable_fallback_prediction": False})
     policy.train([GREET_RULE], domain)
     new_conversation = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2127'>tests/core/policies/test_rule_policy.py~L2127</a>
```diff
 def test_hide_rule_turn(policy: RulePolicy):
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2136'>tests/core/policies/test_rule_policy.py~L2136</a>
```diff
         actions:
         - {UTTER_GREET_ACTION}
         - {action_chitchat}
-        """
-    )
+        """)
     chitchat_story = TrackerWithCachedStates.from_events(
         "chitchat story",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2207'>tests/core/policies/test_rule_policy.py~L2207</a>
```diff
     some_slot_value = "value1"
     slot_which_is_also_in_story = "slot_which_is_also_in_story"
     some_other_slot_value = "value2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {some_intent}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2225'>tests/core/policies/test_rule_policy.py~L2225</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """
-    )
+        """)
 
     simple_rule = TrackerWithCachedStates.from_events(
         "simple rule with an action that sets 1 slot",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2334'>tests/core/policies/test_rule_policy.py~L2334</a>
```diff
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
     followup_on_chitchat = "followup_on_chitchat"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {chitchat}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2347'>tests/core/policies/test_rule_policy.py~L2347</a>
```diff
             type: bool
             mappings:
             - type: from_text
-        """
-    )
+        """)
     simple_rule_no_last_action_listen = TrackerWithCachedStates.from_events(
         "simple rule without action listen in the end",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2422'>tests/core/policies/test_rule_policy.py~L2422</a>
```diff
     activate_another_form = "activate_another_form"
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2443'>tests/core/policies/test_rule_policy.py~L2443</a>
```diff
             required_slots: []
           {another_form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2530'>tests/core/policies/test_rule_policy.py~L2530</a>
```diff
 def test_do_not_hide_rule_turn_with_loops_in_stories(policy: RulePolicy):
     form_name = "some_form"
     activate_form = "activate_form"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {activate_form}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2543'>tests/core/policies/test_rule_policy.py~L2543</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
     form_activation_story = form_activation_rule.copy()
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2585'>tests/core/policies/test_rule_policy.py~L2585</a>
```diff
 def test_hide_rule_turn_with_loops_as_followup_action(policy: RulePolicy):
     form_name = "some_form"
     activate_form = "activate_form"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2601'>tests/core/policies/test_rule_policy.py~L2601</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """
-    )
+        """)
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
     form_activation_story = form_activation_rule.copy()
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2689'>tests/core/policies/test_rule_policy.py~L2689</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """
-    )
+        """)
     rule = TrackerWithCachedStates.from_events(
         "conditioned on action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2734'>tests/core/policies/test_rule_policy.py~L2734</a>
```diff
     utter_1 = "utter_1"
     utter_2 = "utter_2"
     utter_3 = "utter_3"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2743'>tests/core/policies/test_rule_policy.py~L2743</a>
```diff
         - {utter_1}
         - {utter_2}
         - {utter_3}
-        """
-    )
+        """)
     rule = TrackerWithCachedStates.from_events(
         "action_listen after predictable action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2780'>tests/core/policies/test_rule_policy.py~L2780</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """
-    )
+        """)
     rule = TrackerWithCachedStates.from_events(
         "last prediction is action_listen",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2821'>tests/core/policies/test_rule_policy.py~L2821</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """
-    )
+        """)
     rule = TrackerWithCachedStates.from_events(
         "conditioned on action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2864'>tests/core/policies/test_rule_policy.py~L2864</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """
-    )
+        """)
     rule = TrackerWithCachedStates.from_events(
         "rule without action_listen",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2906'>tests/core/policies/test_rule_policy.py~L2906</a>
```diff
     entity_1 = "entity_1"
     entity_2 = "entity_2"
     utter_1 = "utter_1"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2916'>tests/core/policies/test_rule_policy.py~L2916</a>
```diff
         - {entity_2}
         actions:
         - {utter_1}
-        """
-    )
+        """)
 
     rule = TrackerWithCachedStates.from_events(
         "rule without action_listen",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2970'>tests/core/policies/test_rule_policy.py~L2970</a>
```diff
     value_2 = "value_2"
     slot_1 = "slot_1"
     slot_2 = "slot_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - {intent_1}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2994'>tests/core/policies/test_rule_policy.py~L2994</a>
```diff
                  - {value_2}
                 mappings:
                 - type: from_text
-            """
-    )
+            """)
     rule = TrackerWithCachedStates.from_events(
         "rule without action_listen",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L3038'>tests/core/policies/test_rule_policy.py~L3038</a>
```diff
     value_2 = "value_2"
     slot_1 = "slot_1"
     slot_2 = "slot_2"
-    domain = Domain.from_yaml(
-        f"""
+    domain = Domain.from_yaml(f"""
                 version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
                 intents:
                 - {intent_1}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L3062'>tests/core/policies/test_rule_policy.py~L3062</a>
```diff
                      - {value_2}
                     mappings:
                     - type: from_text
-                """
-    )
+                """)
     rule_1 = TrackerWithCachedStates.from_events(
         "normal rule",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_ted_policy.py#L225'>tests/core/policies/test_ted_policy.py~L225</a>
```diff
         execution_context: ExecutionContext,
     ):
         stories = tmp_path / "stories.yml"
-        stories.write_text(
-            f"""
+        stories.write_text(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             stories:
             - story: test path
               steps:
               - action: utter_greet
-            """
-        )
+            """)
         policy = self.create_policy(
             featurizer=featurizer,
             model_storage=model_storage,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_unexpected_intent_policy.py#L135'>tests/core/policies/test_unexpected_intent_policy.py~L135</a>
```diff
         execution_context: ExecutionContext,
     ):
         stories = tmp_path / "stories.yml"
-        stories.write_text(
-            f"""
+        stories.write_text(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             stories:
             - story: test path
               steps:
               - action: utter_greet
-            """
-        )
+            """)
         policy = self.create_policy(
             featurizer=featurizer,
             model_storage=model_storage,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1051'>tests/core/test_actions.py~L1051</a>
```diff
     form_action_name = "my_business_logic"
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     slots:
       my_slot:
         type: text
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1064'>tests/core/test_actions.py~L1064</a>
```diff
       {form_action_name}:
         {REQUIRED_SLOTS_KEY}:
           {slot}
-    """
-        )
+    """)
     )
 
     actual = action.action_for_name_or_text(form_action_name, domain, None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1075'>tests/core/test_actions.py~L1075</a>
```diff
 def test_overridden_form_action():
     form_action_name = "my_business_logic"
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     actions:
     - my_action
     - {form_action_name}
     forms:
         {form_action_name}:
           {REQUIRED_SLOTS_KEY}: []
-    """
-        )
+    """)
     )
 
     actual = action.action_for_name_or_text(form_action_name, domain, None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1094'>tests/core/test_actions.py~L1094</a>
```diff
 def test_get_form_action_if_not_in_forms():
     form_action_name = "my_business_logic"
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            """
+        textwrap.dedent("""
     actions:
     - my_action
-    """
-        )
+    """)
     )
 
     with pytest.raises(ActionNotFoundException):
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1111'>tests/core/test_actions.py~L1111</a>
```diff
 )
 def test_get_end_to_end_utterance_action(end_to_end_utterance: Text):
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     actions:
     - my_action
     {KEY_E2E_ACTIONS}:
     - {end_to_end_utterance}
     - Bye Bye
-"""
-        )
+""")
     )
 
     actual = action.action_for_name_or_text(end_to_end_utterance, domain, None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1132'>tests/core/test_actions.py~L1132</a>
```diff
     end_to_end_utterance = "Hi"
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     actions:
     - my_action
     {KEY_E2E_ACTIONS}:
     - {end_to_end_utterance}
     - Bye Bye
-"""
-        )
+""")
     )
 
     e2e_action = action.action_for_name_or_text("Hi", domain, None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1226'>tests/core/test_actions.py~L1226</a>
```diff
     user: Event, slot_name: Text, slot_value: Any, new_user: Event, updated_value: Any
 ):
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - inform
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1266'>tests/core/test_actions.py~L1266</a>
```diff
                 influence_conversation: false
                 mappings:
                 - type: from_entity
-                  entity: name"""
-        )
+                  entity: name""")
     )
 
     action_extract_slots = ActionExtractSlots(action_endpoint=None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1309'>tests/core/test_actions.py~L1309</a>
```diff
 
 async def test_action_extract_slots_with_from_trigger_mappings():
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1334'>tests/core/test_actions.py~L1334</a>
```diff
             forms:
               registration_form:
                 required_slots:
-                  - email"""
-        )
+                  - email""")
     )
 
     action_extract_slots = ActionExtractSlots(action_endpoint=None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1428'>tests/core/test_actions.py~L1428</a>
```diff
     slot_name = "toppings"
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
 
     entities:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1447'>tests/core/test_actions.py~L1447</a>
```diff
       {form_name}:
         {REQUIRED_SLOTS_KEY}:
           - {slot_name}
-    """
-        )
+    """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1525'>tests/core/test_actions.py~L1525</a>
```diff
     form_name = "some_form"
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1548'>tests/core/test_actions.py~L1548</a>
```diff
              other_form:
                required_slots:
                  - name
-            """
-        )
+            """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1578'>tests/core/test_actions.py~L1578</a>
```diff
     form_name = "some_form"
 
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1608'>tests/core/test_actions.py~L1608</a>
```diff
                required_slots:
                  - email
                  - name
-            """
-        )
+            """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1819'>tests/core/test_actions.py~L1819</a>
```diff
     form_name = "some_form"
     slot_name = "toppings"
     domain = Domain.from_yaml(
-        textwrap.dedent(
-            f"""
+        textwrap.dedent(f"""
     version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
 
     entities:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1843'>tests/core/test_actions.py~L1843</a>
```diff
       {form_name}:
         {REQUIRED_SLOTS_KEY}:
           - {slot_name}
-    """
-        )
+    """)
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L2031'>tests/core/test_actions.py~L2031</a>
```diff
     validate_return_events: List[Dict[Tex...*[Comment body truncated]*

---

_Marked ready for review by @MichaReiser on 2023-12-22 09:52_

---

_Comment by @MichaReiser on 2023-12-22 11:10_

I noticed one more pattern where we diverge from black:

```python
print("""a very long string that exceeds the line width on the first line
	but goes on
""")
```

Black keeps the indentation for those strings:

```python
print(
	"""a very long string that exceeds the line width on the first line
	but goes on
	"""
)
```

Which makes sense, especially if the call already starts late on the line. We can either use Prettier's heuristic which allows the user to choose where to place the quotes or use `best_fitting![string, indent(&string)]` so that the string gets indented if the first line doesn't fit

---

_Comment by @charliermarsh on 2023-12-22 15:15_

Those aren't quite the same string though, right? Because in the second case, you have an indent before the `"""` within the string contents, i.e., extra whitespace? Or am I still waking up...

---

_Comment by @MichaReiser on 2023-12-24 01:32_

> Those aren't quite the same string though, right? Because in the second case, you have an indent before the `"""` within the string contents, i.e., extra whitespace? Or am I still waking up...

Yeah sorry, I messed up. The formatting should be

```python
print("""a very long string that exceeds the line width on the first line
	but goes on
""")
```

or, based on whether the opening line fits in the configured line length.

```python
print(
	"""a very long string that exceeds the line width on the first line
	but goes on
"""
)
```

the downside I see with adopting the layout based on the first line is that it may require manually changing the indent of the last line to make it perfectly align, at least if whitespace doesn't matter and users care.



---

_@konstin approved on 2023-12-26 12:56_

---

_Comment by @MichaReiser on 2023-12-28 10:14_

I pushed a new update where we only apply the hugging layout if there's no other content on the same line as the opening and closing quotes because:

* having content on the same line as the opening quotes could lead to the first line exceed the configured line width. using `best_fitting` could help but has the downside that the parent (e.g. parameters) won't expand even if they contain a multiline string
* having content on the same line as the closing quotes: I find that it can decrease readability when the last line has content when the call expression is e.g. nested in a dictionary or another call (see the following example)


```python
            request=ListLogEntriesRequest(
                resource_names=["projects/project_id"],
                filter=('''resource.type="global"
logName="projects/project_id/logs/airflow"
labels.task_id="task_for_testing_stackdriver_task_handler"
labels.dag_id="dag_for_testing_stackdriver_file_task_handler"
labels.execution_date="2016-01-01T00:00:00+00:00"
labels.try_number="3"'''),
                order_by="timestamp asc",
                page_size=1000,
                page_token=None,
            )
        )
```

The downside is that there are significantly more usages of `dedent` with content on the same line as the the opening and closing quotes.


Following the diff between the two versions

ℹ️ ecosystem check **detected format changes**. (+925 -462 lines in 125 files in 21 projects; 20 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+36 -18 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/telemetry.py#L105'>rasa/telemetry.py~L105</a>
```diff
 
 def print_telemetry_reporting_info() -> None:
     """Print telemetry information to std out."""
-    message = textwrap.dedent(f"""
+    message = textwrap.dedent(
+        f"""
         Rasa Open Source reports anonymous usage telemetry to help improve the product
         for all its users.
 
         If you'd like to opt-out, you can use `rasa telemetry disable`.
-        To learn more, check out {DOCS_URL_TELEMETRY}.""").strip()
+        To learn more, check out {DOCS_URL_TELEMETRY}."""
+    ).strip()
 
     table = SingleTable([[message]])
     print(table.table)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1216'>tests/core/test_actions.py~L1216</a>
```diff
     user: Event, slot_name: Text, slot_value: Any, new_user: Event, updated_value: Any
 ):
     domain = Domain.from_yaml(
-        textwrap.dedent(f"""
+        textwrap.dedent(
+            f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - inform
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1255'>tests/core/test_actions.py~L1255</a>
```diff
                 influence_conversation: false
                 mappings:
                 - type: from_entity
-                  entity: name""")
+                  entity: name"""
+        )
     )
 
     action_extract_slots = ActionExtractSlots(action_endpoint=None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1297'>tests/core/test_actions.py~L1297</a>
```diff
 
 async def test_action_extract_slots_with_from_trigger_mappings():
     domain = Domain.from_yaml(
-        textwrap.dedent(f"""
+        textwrap.dedent(
+            f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/test_actions.py#L1321'>tests/core/test_actions.py~L1321</a>
```diff
             forms:
               registration_form:
                 required_slots:
-                  - email""")
+                  - email"""
+        )
     )
 
     action_extract_slots = ActionExtractSlots(action_endpoint=None)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/shared/core/test_domain.py#L292'>tests/shared/core/test_domain.py~L292</a>
```diff
 
 
 def test_domain_to_dict():
-    test_yaml = textwrap.dedent(f"""
+    test_yaml = textwrap.dedent(
+        f"""
     actions:
     - action_save_world
     config:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/shared/core/test_domain.py#L318'>tests/shared/core/test_domain.py~L318</a>
```diff
         - high
         - low
         mappings:
-        - type: from_text""")
+        - type: from_text"""
+    )
 
     domain_as_dict = Domain.from_yaml(test_yaml).as_dict()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/shared/core/test_domain.py#L2230'>tests/shared/core/test_domain.py~L2230</a>
```diff
 
 
 def test_merge_yaml_domains_loads_actions_which_explicitly_need_domain():
-    test_yaml_1 = textwrap.dedent("""
+    test_yaml_1 = textwrap.dedent(
+        """
         actions:
           - action_hello
           - action_bye
-          - action_send_domain: {send_domain: True}""")
+          - action_send_domain: {send_domain: True}"""
+    )
 
-    test_yaml_2 = textwrap.dedent("""
+    test_yaml_2 = textwrap.dedent(
+        """
         actions:
           - action_find_restaurants:
-                send_domain: True""")
+                send_domain: True"""
+    )
 
     domain_1 = Domain.from_yaml(test_yaml_1)
     domain_2 = Domain.from_yaml(test_yaml_2)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/test_server.py#L2285'>tests/test_server.py~L2285</a>
```diff
 
     story_content = response.body.decode("utf-8")
 
-    expected_first_story = textwrap.dedent(f"""
+    expected_first_story = textwrap.dedent(
+        f"""
     - story: {sender_id}, story 1
       steps:
       - intent: greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/test_server.py#L2295'>tests/test_server.py~L2295</a>
```diff
       - intent: restart
         user: |-
           /restart
-      - action: action_restart""")
+      - action: action_restart"""
+    )
     assert expected_first_story in story_content
 
-    expected_second_story = textwrap.dedent(f"""
+    expected_second_story = textwrap.dedent(
+        f"""
     - story: {sender_id}, story 2
       steps:
       - intent: greet
         user: |-
-          hi again""")
+          hi again"""
+    )
     assert expected_second_story in story_content
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/test_server.py#L2321'>tests/test_server.py~L2321</a>
```diff
     assert response.status == 200
 
     story_content = response.body.decode("utf-8")
-    expected_story = textwrap.dedent(f"""
+    expected_story = textwrap.dedent(
+        f"""
     - story: {sender_id}
       steps:
       - intent: greet
         user: |-
-          hi again""")
+          hi again"""
+    )
     assert expected_story in story_content
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+8 -4 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/5ceb1bf6ab0daadb07c9a5fa270b7096e89f9b38/tests/nativeapp/test_manager.py#L76'>tests/nativeapp/test_manager.py~L76</a>
```diff
         mock.call("select current_role()", cursor_class=DictCursor),
         mock.call("use role new_role"),
         mock.call(f"create schema if not exists app_pkg.app_src"),
-        mock.call(f"""
+        mock.call(
+            f"""
                     create stage if not exists app_pkg.app_src.stage
-                    encryption = (TYPE = 'SNOWFLAKE_SSE')"""),
+                    encryption = (TYPE = 'SNOWFLAKE_SSE')"""
+        ),
         mock.call("use role old_role"),
     ]
     assert mock_execute.mock_calls == expected
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/5ceb1bf6ab0daadb07c9a5fa270b7096e89f9b38/tests/test_command_options.py#L11'>tests/test_command_options.py~L11</a>
```diff
         "invalid_format",
     ])
 
-    assert result.output == ("""Usage: default object stage list [OPTIONS] STAGE_NAME
+    assert result.output == (
+        """Usage: default object stage list [OPTIONS] STAGE_NAME
 Try 'default object stage list --help' for help.
 ╭─ Error ──────────────────────────────────────────────────────────────────────╮
 │ Invalid value for '--format': 'invalid_format' is not one of 'TABLE',        │
 │ 'JSON'.                                                                      │
 ╰──────────────────────────────────────────────────────────────────────────────╯
-""")
+"""
+    )
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+40 -20 lines across 10 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/airflow/cli/commands/internal_api_command.py#L81'>airflow/cli/commands/internal_api_command.py~L81</a>
```diff
         )
     else:
         log.info(
-            textwrap.dedent(f"""\
+            textwrap.dedent(
+                f"""\
                 Running the Gunicorn Server with:
                 Workers: {num_workers} {args.workerclass}
                 Host: {args.hostname}:{args.port}
                 Timeout: {worker_timeout}
                 Logfiles: {access_logfile} {error_logfile}
                 Access Logformat: {access_logformat}
-                =================================================================""")
+                ================================================================="""
+            )
         )
 
         pid_file, _, _, _ = setup_locations("internal-api", pid=args.pid)
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/airflow/cli/commands/task_command.py#L667'>airflow/cli/commands/task_command.py~L667</a>
```diff
         ti.render_templates()
     for attr in task.template_fields:
         print(
-            textwrap.dedent(f"""        # ----------------------------------------------------------
+            textwrap.dedent(
+                f"""        # ----------------------------------------------------------
         # property: {attr}
         # ----------------------------------------------------------
         {getattr(ti.task, attr)}
-        """)
+        """
+            )
         )
 
 
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/airflow/cli/commands/webserver_command.py#L364'>airflow/cli/commands/webserver_command.py~L364</a>
```diff
         )
     else:
         print(
-            textwrap.dedent(f"""\
+            textwrap.dedent(
+                f"""\
                 Running the Gunicorn Server with:
                 Workers: {num_workers} {args.workerclass}
                 Host: {args.hostname}:{args.port}
                 Timeout: {worker_timeout}
                 Logfiles: {access_logfile} {error_logfile}
                 Access Logformat: {access_logformat}
-                =================================================================""")
+                ================================================================="""
+            )
         )
 
         pid_file, _, _, _ = setup_locations("webserver", pid=args.pid)
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/assign_cherry_picked_prs_with_milestone.py#L89'>dev/assign_cherry_picked_prs_with_milestone.py~L89</a>
```diff
     "--github-token",
     type=str,
     required=True,
-    help=textwrap.dedent("""
+    help=textwrap.dedent(
+        """
         GitHub token used to authenticate.
         You can set omit it if you have GITHUB_TOKEN env variable set
         Can be generated with:
-        https://github.com/settings/tokens/new?description=Read%20Write%20isssues&scopes=repo"""),
+        https://github.com/settings/tokens/new?description=Read%20Write%20isssues&scopes=repo"""
+    ),
     envvar="GITHUB_TOKEN",
 )
 
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1533'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py~L1533</a>
```diff
 @click.option(
     "--github-token",
     envvar="GITHUB_TOKEN",
-    help=textwrap.dedent("""
+    help=textwrap.dedent(
+        """
       GitHub token used to authenticate.
       You can set omit it if you have GITHUB_TOKEN env variable set.
       Can be generated with:
-      https://github.com/settings/tokens/new?description=Read%20sssues&scopes=repo:status"""),
+      https://github.com/settings/tokens/new?description=Read%20sssues&scopes=repo:status"""
+    ),
 )
 @click.option(
     "--only-available-in-dist",
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/breeze/src/airflow_breeze/utils/path_utils.py#L161'>dev/breeze/src/airflow_breeze/utils/path_utils.py~L161</a>
```diff
         if "importlib_metadata" in e.msg:
             return False
         if "apache-airflow-breeze" in e.msg:
-            print("""Missing Package `apache-airflow-breeze`.
-                   Use `pipx install -e ./dev/breeze` to install the package.""")
+            print(
+                """Missing Package `apache-airflow-breeze`.
+                   Use `pipx install -e ./dev/breeze` to install the package."""
+            )
             return False
     sources_hash = get_installation_sources_config_metadata_hash()
     if sources_hash != package_hash:
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/prepare_bulk_issues.py#L108'>dev/prepare_bulk_issues.py~L108</a>
```diff
     "--github-token",
     type=str,
     required=True,
-    help=textwrap.dedent("""
+    help=textwrap.dedent(
+        """
         GitHub token used to authenticate.
         You can omit it if you have GITHUB_TOKEN env variable set
         Can be generated with:
-        https://github.com/settings/tokens/new?description=Write%20issues&scopes=repo:status,public_repo"""),
+        https://github.com/settings/tokens/new?description=Write%20issues&scopes=repo:status,public_repo"""
+    ),
     envvar="GITHUB_TOKEN",
 )
 
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/prepare_release_issue.py#L70'>dev/prepare_release_issue.py~L70</a>
```diff
     "--github-token",
     type=str,
     required=True,
-    help=textwrap.dedent("""
+    help=textwrap.dedent(
+        """
         GitHub token used to authenticate.
         You can set omit it if you have GITHUB_TOKEN env variable set
         Can be generated with:
-        https://github.com/settings/tokens/new?description=Read%20sssues&scopes=repo:status"""),
+        https://github.com/settings/tokens/new?description=Read%20sssues&scopes=repo:status"""
+    ),
     envvar="GITHUB_TOKEN",
 )
 
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/dev/stats/calculate_statistics_provider_testing_issues.py#L57'>dev/stats/calculate_statistics_provider_testing_issues.py~L57</a>
```diff
     "--github-token",
     type=str,
     required=True,
-    help=textwrap.dedent("""
+    help=textwrap.dedent(
+        """
         GitHub token used to authenticate.
         You can set omit it if you have GITHUB_TOKEN env variable set
         Can be generated with:
-        https://github.com/settings/tokens/new?description=Read%20Write%20isssues&scopes=repo"""),
+        https://github.com/settings/tokens/new?description=Read%20Write%20isssues&scopes=repo"""
+    ),
     envvar="GITHUB_TOKEN",
 )
 
```
<a href='https://github.com/apache/airflow/blob/73cd0d5f8e99f93d2ff90a7cca04e6cdf086c359/tests/providers/google/cloud/log/test_stackdriver_task_handler.py#L242'>tests/providers/google/cloud/log/test_stackdriver_task_handler.py~L242</a>
```diff
         mock_client.return_value.list_log_entries.assert_called_once_with(
             request=ListLogEntriesRequest(
                 resource_names=["projects/project_id"],
-                filter=('''resource.type="global"
+                filter=(
+                    '''resource.type="global"
 logName="projects/project_id/logs/airflow"
 labels.task_id="task_for_testing_stackdriver_task_handler"
 labels.dag_id="dag_for_testing_stackdriver_file_task_handler"
 labels.execution_date="2016-01-01T00:00:00+00:00"
-labels.try_number="3"'''),
+labels.try_number="3"'''
+                ),
                 order_by="timestamp asc",
                 page_size=1000,
                 page_token=None,
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+24 -12 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/components.py#L40'>examples/output/apis/components.py~L40</a>
```diff
 
 script, div = components(plots)
 
-template = Template("""<!DOCTYPE html>
+template = Template(
+    """<!DOCTYPE html>
 <html lang="en">
     <head>
         <meta charset="utf-8">
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/components.py#L62'>examples/output/apis/components.py~L62</a>
```diff
         </div>
     </body>
 </html>
-""")
+"""
+)
 
 resources = INLINE.render()
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/components_themed.py#L60'>examples/output/apis/components_themed.py~L60</a>
```diff
 
 script, div = components(p, theme=theme)
 
-template = Template("""<!DOCTYPE html>
+template = Template(
+    """<!DOCTYPE html>
 <html lang="en">
     <head>
         <meta charset="utf-8">
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/components_themed.py#L85'>examples/output/apis/components_themed.py~L85</a>
```diff
         </div>
     </body>
 </html>
-""")
+"""
+)
 
 resources = INLINE.render()
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/embed/test_util__embed.py#L775'>tests/unit/bokeh/embed/test_util__embed.py~L775</a>
```diff
         assert beu.is_tex_string("part0$$part1\\[part2\\(part3\\]") is False
         assert beu.is_tex_string("part0$$part1\\[part2\\(part3\\)") is False
         assert (
-            beu.is_tex_string("""$$
+            beu.is_tex_string(
+                """$$
           cos(x)
-        $$""")
+        $$"""
+            )
             is True
         )
         assert (
-            beu.is_tex_string("""$$
+            beu.is_tex_string(
+                """$$
           cos(x)$$
-        """)
+        """
+            )
             is False
         )
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/embed/test_util__embed.py#L807'>tests/unit/bokeh/embed/test_util__embed.py~L807</a>
```diff
         assert beu.contains_tex_string("part0$$part1\\[part2\\(part3\\]") is True
         assert beu.contains_tex_string("part0$$part1\\[part2\\(part3\\)") is True
         assert (
-            beu.contains_tex_string("""$$
+            beu.contains_tex_string(
+                """$$
           cos(x)
-        $$""")
+        $$"""
+            )
             is True
         )
         assert (
-            beu.contains_tex_string("""$$
+            beu.contains_tex_string(
+                """$$
           cos(x)$$
-        """)
+        """
+            )
             is True
         )
 
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+12 -6 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/commaai/openpilot/blob/b38c580c2e687401fc819520ea038653873e9996/selfdrive/car/tests/test_lateral_limits.py#L94'>selfdrive/car/tests/test_lateral_limits.py~L94</a>
```diff
         violation = _jerks["up_jerk"] > MAX_LAT_JERK_UP + MAX_LAT_JERK_UP_TOLERANCE or _jerks["down_jerk"] > MAX_LAT_JERK_DOWN
         violation_str = " - VIOLATION" if violation else ""
 
-        print(f"{car_model:{max_car_model_len}} - up jerk: {round(_jerks['up_jerk'], 2):5} \
-              m/s^3, down jerk: {round(_jerks['down_jerk'], 2):5} m/s^3{violation_str}")
+        print(
+            f"{car_model:{max_car_model_len}} - up jerk: {round(_jerks['up_jerk'], 2):5} \
+              m/s^3, down jerk: {round(_jerks['down_jerk'], 2):5} m/s^3{violation_str}"
+        )
 
     # exit with test result
     sys.exit(not result.result.wasSuccessful())
```
<a href='https://github.com/commaai/openpilot/blob/b38c580c2e687401fc819520ea038653873e9996/selfdrive/debug/test_fw_query_on_routes.py#L112'>selfdrive/debug/test_fw_query_on_routes.py~L112</a>
```diff
                     padding = max([len(fw.brand or UNKNOWN_BRAND) for fw in car_fw])
                     for version in sorted(car_fw, key=lambda fw: fw.brand):
                         subaddr = None if version.subAddress == 0 else hex(version.subAddress)
-                        print(f"  Brand: {version.brand or UNKNOWN_BRAND:{padding}}, bus: {version.bus} - \
-                      (Ecu.{version.ecu}, {hex(version.address)}, {subaddr}): [{version.fwVersion}],")
+                        print(
+                            f"  Brand: {version.brand or UNKNOWN_BRAND:{padding}}, bus: {version.bus} - \
+                      (Ecu.{version.ecu}, {hex(version.address)}, {subaddr}): [{version.fwVersion}],"
+                        )
 
                     print("Mismatches")
                     found = False
```
<a href='https://github.com/commaai/openpilot/blob/b38c580c2e687401fc819520ea038653873e9996/tools/tuning/measure_steering_accuracy.py#L113'>tools/tuning/measure_steering_accuracy.py~L113</a>
```diff
                     print(f"  {'-'*118}")
                     for k in sorted(self.speed_group_stats[group].keys()):
                         v = self.speed_group_stats[group][k]
-                        print(f'  {k:#2}° | actuator:{int(v["steer"] / v["cnt"] * 100):#3}% \
+                        print(
+                            f'  {k:#2}° | actuator:{int(v["steer"] / v["cnt"] * 100):#3}% \
                               | error: {round(v["err"] / v["cnt"], 2):2.2f}° | -:{int(v["-"] / v["cnt"] * 100):#3}% \
                               | =:{int(v["="] / v["cnt"] * 100):#3}% | +:{int(v["+"] / v["cnt"] * 100):#3}% | lim:{v["limited"]:#5} \
-                              | sat:{v["saturated"]:#5} | path dev: {round(v["dpp"] / v["cnt"], 2):2.2f}m | total: {v["cnt"]:#5}')
+                              | sat:{v["saturated"]:#5} | path dev: {round(v["dpp"] / v["cnt"], 2):2.2f}m | total: {v["cnt"]:#5}'
+                        )
                     print("")
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+253 -126 lines across 35 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py#L634'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub.py~L634</a>
```diff
         except Exception:
             demisto.error(f"Failed parsing filters: {string_filters}\n error: {Exception}")
     else:
-        demisto.info(f"Advanced filters does not contain all fields or fields are not in\
+        demisto.info(
+            f"Advanced filters does not contain all fields or fields are not in\
             the correct order: name,value,comparison: {string_filters}\
-            Will run with an empty filter.")
+            Will run with an empty filter."
+        )
     return filters
 
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/ApiModules/Scripts/MicrosoftApiModule/MicrosoftApiModule.py#L1562'>Packs/ApiModules/Scripts/MicrosoftApiModule/MicrosoftApiModule.py~L1562</a>
```diff
     if value := args.get(key, params.get(key)):
         return value
     else:
-        raise Exception(f"No {key} was provided. Please provide a {key} either in the \
-instance configuration or as a command argument.")
+        raise Exception(
+            f"No {key} was provided. Please provide a {key} either in the \
+instance configuration or as a command argument."
+        )
 
 
 def azure_tag_formatter(arg):
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/ApiModules/Scripts/SiemApiModule/SiemApiModule.py#L135'>Packs/ApiModules/Scripts/SiemApiModule/SiemApiModule.py~L135</a>
```diff
         for logs in self._iter_events():
             stored.extend(logs)
             if self.options.limit:
-                demisto.debug(f"{self.options.limit=} reached. \
+                demisto.debug(
+                    f"{self.options.limit=} reached. \
                     slicing from {len(logs)=}. \
-                    limit must be presented ONLY in commands and not in fetch-events.")
+                    limit must be presented ONLY in commands and not in fetch-events."
+                )
                 if len(stored) >= self.options.limit:
                     return stored[: self.options.limit]
         return stored
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Armis/Integrations/ArmisEventCollector/ArmisEventCollector.py#L385'>Packs/Armis/Integrations/ArmisEventCollector/ArmisEventCollector.py~L385</a>
```diff
         aql_query=activities_aql_query, order_by=EVENT_TYPES["Activities"].order_by
     )
     devices_response = client.fetch_by_ids_in_aql_query(aql_query=devices_aql_query, order_by=EVENT_TYPES["Devices"].order_by)
-    demisto.debug(f"debug-log: fetch by alert ids\
-fetched {len(activities_response)} Activities and {len(devices_response)} Devices")
+    demisto.debug(
+        f"debug-log: fetch by alert ids\
+fetched {len(activities_response)} Activities and {len(devices_response)} Devices"
+    )
     alert["activitiesData"] = activities_response if activities_response else {}
     alert["devicesData"] = devices_response if devices_response else {}
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/AzureNetworkSecurityGroups/Integrations/AzureNetworkSecurityGroups/AzureNetworkSecurityGroups.py#L138'>Packs/AzureNetworkSecurityGroups/Integrations/AzureNetworkSecurityGroups/AzureNetworkSecurityGroups.py~L138</a>
```diff
             )
         except Exception as e:
             if "404" in str(e):
-                raise ValueError(f'Rule {rule_name} under subscription ID "{subscription_id}" \
-and resource group "{resource_group_name}" was not found.')
+                raise ValueError(
+                    f'Rule {rule_name} under subscription ID "{subscription_id}" \
+and resource group "{resource_group_name}" was not found.'
+                )
             raise
 
     @logger
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L2585'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py~L2585</a>
```diff
     at_least_one_is_provided = any(related_args.values())
     at_least_one_is_not_provided = not all(related_args.values())
     if at_least_one_is_not_provided and at_least_one_is_provided:
-        raise ValueError(f"The arguments: {list(related_args.keys())} either all should all have value,\
-                         or none should have value.")
+        raise ValueError(
+            f"The arguments: {list(related_args.keys())} either all should all have value,\
+                         or none should have value."
+        )
 
 
 def extract_args_from_additional_fields_arg(additional_fields: str, field_name: str) -> Tuple[Any, List[str]]:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/BreachRx/Integrations/BreachRx/BreachRx.py#L13'>Packs/BreachRx/Integrations/BreachRx/BreachRx.py~L13</a>
```diff
 # Disable insecure warnings
 urllib3.disable_warnings()  # pylint: disable=no-member
 
-create_incident_mutation = gql("""
+create_incident_mutation = gql(
+    """
 mutation CreateIncident(
   $severity: String!,
   $name: String!,
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/BreachRx/Integrations/BreachRx/BreachRx.py#L39'>Packs/BreachRx/Integrations/BreachRx/BreachRx.py~L39</a>
```diff
     description
     identifier
   }
-}""")
+}"""
+)
 
-get_incident_severities = gql("""{
+get_incident_severities = gql(
+    """{
   incidentSeverities {
     name
     ordering
   }
-}""")
+}"""
+)
 
-get_incident_types = gql("""{
+get_incident_types = gql(
+    """{
   types {
     name
   }
-}""")
+}"""
+)
 
-get_actions_for_incident = gql("""
+get_actions_for_incident = gql(
+    """
 query GetActionsForIncident($incidentId: Int!) {
   actions(where: {
     incidentId: {
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/BreachRx/Integrations/BreachRx/BreachRx.py#L73'>Packs/BreachRx/Integrations/BreachRx/BreachRx.py~L73</a>
```diff
       email
     }
   }
-}""")
+}"""
+)
 
-get_incident_by_name = gql("""
+get_incident_by_name = gql(
+    """
 query GetIncidentByName($name: String, $identifier: String) {
   incidents(first: 1, where: {
     name: {
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/BreachRx/Integrations/BreachRx/BreachRx.py#L100'>Packs/BreachRx/Integrations/BreachRx/BreachRx.py~L100</a>
```diff
     description
     identifier
   }
-}""")
+}"""
+)
 
 
 class BreachRxClient:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L2033'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py~L2033</a>
```diff
 
     except Exception as e:
         demisto.error(traceback.format_exc())  # print the traceback
-        return_error(f"Failed to execute {demisto.command()} command.\nError:\n{str(e)} \
-            {traceback.format_exc()}")
+        return_error(
+            f"Failed to execute {demisto.command()} command.\nError:\n{str(e)} \
+            {traceback.format_exc()}"
+        )
 
 
 if __name__ in ("__main__", "__builtin__", "builtins"):
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Cryptosim/Integrations/Cryptosim/Cryptosim.py#L131'>Packs/Cryptosim/Integrations/Cryptosim/Cryptosim.py~L131</a>
```diff
         if client.connection_test().get("StatusCode") == 200:
             message = "ok"
         else:
-            raise Exception(f"""StatusCode:
+            raise Exception(
+                f"""StatusCode:
                             {client.correlations().get('StatusCode')},
                             Error: {client.correlations().get('ErrorMessage')}
-                            """)
+                            """
+            )
     except DemistoException as e:
         if "401" in str(e):
             message = "Authorization Error: make sure API User and Password is correctly set"
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/EDLMonitor/Integrations/EDLMonitor/EDLMonitor.py#L110'>Packs/EDLMonitor/Integrations/EDLMonitor/EDLMonitor.py~L110</a>
```diff
     try:
         demisto.debug(f"Start time {start_time}")
         demisto.debug(f"Cur time {datetime.now()}")
-        demisto.debug(f"Parameters: {start_time}, {EDL}, {edl_user}, {verify_certificate}, {email}, \
-        {email_server}, {email_user}, {timeout}, {mon_contents}")
+        demisto.debug(
+            f"Parameters: {start_time}, {EDL}, {edl_user}, {verify_certificate}, {email}, \
+        {email_server}, {email_user}, {timeout}, {mon_contents}"
+        )
 
         if edl_pwd is None:
             # No auth on EDL
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py#L297'>Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py~L297</a>
```diff
             if current_batch_data:
                 dedup_data = [item for item in current_batch_data if get_activity_log_id(item) not in fetched_ids]
                 if dedup_data:
-                    demisto.debug(f"Fetching {len(dedup_data)} non duplicates alerts from\
-                            {len(current_batch_data)} found in API for {event_type.name}")
+                    demisto.debug(
+                        f"Fetching {len(dedup_data)} non duplicates alerts from\
+                            {len(current_batch_data)} found in API for {event_type.name}"
+                    )
                     res.extend(dedup_data)
 
                     fetched_ids = fetched_ids.union({get_activity_log_id(item) for item in dedup_data})
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py#L358'>Packs/FireEyeETP/Integrations/FireEyeETPEventCollector/FireEyeETPEventCollector.py~L358</a>
```diff
                     )
                 )
                 if dedup_data:
-                    demisto.debug(f"Fetching {len(dedup_data)} alerts from {len(current_batch_data)} \
-                            found in API for {event_type.name}")
+                    demisto.debug(
+                        f"Fetching {len(dedup_data)} alerts from {len(current_batch_data)} \
+                            found in API for {event_type.name}"
+                    )
                     res.extend(dedup_data)
                     res_count += len(dedup_data)
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Gmail/Integrations/Gmail/Gmail.py#L2632'>Packs/Gmail/Integrations/Gmail/Gmail.py~L2632</a>
```diff
     max_results = max_fetch
     if max_fetch > 200:
         max_results = 200
-    demisto.debug(f"GMAIL: fetch parameters: user: {user_key} query={query} fetch time: {last_fetch} \
-    page_token: {page_token} max results: {max_results}")
+    demisto.debug(
+        f"GMAIL: fetch parameters: user: {user_key} query={query} fetch time: {last_fetch} \
+    page_token: {page_token} max results: {max_results}"
+    )
     result = service.users().messages().list(userId=user_key, maxResults=max_results, pageToken=page_token, q=query).execute()
 
     incidents = []
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/GoogleSheets/Integrations/GoogleSheets/GoogleSheets.py#L833'>Packs/GoogleSheets/Integrations/GoogleSheets/GoogleSheets.py~L833</a>
```diff
         service_account_credentials = service_account_credentials.get("password")
         service_account_credentials = json.loads(service_account_credentials)
     except Exception as e:
-        raise DemistoException(f"\nProblem with json format please validate the correctness\
-                               of your service account json. Json parser error {e}")
+        raise DemistoException(
+            f"\nProblem with json format please validate the correctness\
+                               of your service account json. Json parser error {e}"
+        )
 
     try:
         credentials = service_account.ServiceAccountCredentials.from_json_keyfile_dict(service_account_credentials, scopes=SCOPES)
     except KeyError as e:
-        raise DemistoException(f"There is a problem with the authentication of the Service account credentials.\
+        raise DemistoException(
+            f"There is a problem with the authentication of the Service account credentials.\
                                 Please check the correctness of your credentials. You are missing the following key\
-                                 in the service account {e}")
+                                 in the service account {e}"
+        )
     except ValueError as e:
-        raise DemistoException(f"(There is a problem with the authentication of the Service account credentials\
-                                The cred are of the wrong type. Google error is : {e})")
+        raise DemistoException(
+            f"(There is a problem with the authentication of the Service account credentials\
+                                The cred are of the wrong type. Google error is : {e})"
+        )
 
     # add delegation to help manage the UI - link to a google-account
     if params.get("user_id"):
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MailListener/Integrations/MailListenerV2/MailListenerV2.py#L354'>Packs/MailListener/Integrations/MailListenerV2/MailListenerV2.py~L354</a>
```diff
         try:
             email_message_object = Email(message_bytes, include_raw_body, save_file, mail_id)
         except Exception as e:
-            demisto.debug(f"Create Email object was un-successful for {mail_id=}, {message_data=}.\
-                Skipping to next available email. Error: {e}")
+            demisto.debug(
+                f"Create Email object was un-successful for {mail_id=}, {message_data=}.\
+                Skipping to next available email. Error: {e}"
+            )
             continue
 
         # Add mails if the current email UID is higher than the previous incident UID
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MicrosoftCloudAppSecurity/Integrations/MicrosoftDefenderEventCollector/MicrosoftDefenderEventCollector.py#L159'>Packs/MicrosoftCloudAppSecurity/Integrations/MicrosoftDefenderEventCollector/MicrosoftDefenderEventCollector.py~L159</a>
```diff
         for logs in self._iter_events():
             stored.extend(logs)
             if self.options.limit:
-                demisto.debug(f"{self.options.limit=} reached. \
+                demisto.debug(
+                    f"{self.options.limit=} reached. \
                     slicing from {len(logs)=}. \
-                    limit must be presented ONLY in commands and not in fetch-events.")
+                    limit must be presented ONLY in commands and not in fetch-events."
+                )
                 if len(stored) >= self.options.limit:
                     return stored[: self.options.limit]
         return stored
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection_test.py#L1341'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection_test.py~L1341</a>
```diff
     mocker.patch.object(demisto, "debug")
 
     def raise_mock(params=None, overwrite_rate_limit_retry=True):
-        raise DemistoException("""Verify that the server URL parameter is correct and that you have access to the server from your host.
+        raise DemistoException(
+            """Verify that the server URL parameter is correct and that you have access to the server from your host.
 Error Type: <requests.exceptions.ConnectionError>
 Error Number: [None]
 Message: None
-""")  # noqa: E501
+"""
+        )  # noqa: E501
 
     mocker.patch.object(client_mocker, "list_alerts_by_params", side_effect=raise_mock)
     with pytest.raises(Exception) as e:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L1075'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py~L1075</a>
```diff
             )
             raise exc
         except ErrorMimeContentConversionFailed as exc:
-            demisto.debug(f"Encountered an ErrorMimeContentConversionFailed error object while iterating: {exc}.\
-                Continuing to next item.")
+            demisto.debug(
+                f"Encountered an ErrorMimeContentConversionFailed error object while iterating: {exc}.\
+                Continuing to next item."
+            )
             continue
         except AttributeError as exc:
-            demisto.debug(f"Encountered an Attribute error object while iterating: {exc}.\
-                 Continuing to next item.")
+            demisto.debug(
+                f"Encountered an Attribute error object while iterating: {exc}.\
+                 Continuing to next item."
+            )
 
     demisto.debug(f"EWS V2 - Got total of {len(result)} from ews query. ")
     return result
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Mimecast/Integrations/MimecastEventCollector/MimecastEventCollector.py#L108'>Packs/Mimecast/Integrations/MimecastEventCollector/MimecastEventCollector.py~L108</a>
```diff
                 # More than SIEM_LOG_LIMIT were saved from prev run.
                 # take SIEM_LOG_LIMIT from events_from_prev_run and put in stored
                 # save the rest in events_from_prev_run for the next run.
-                demisto.info(f"{self.options.limit=} reached. \
-                    slicing from {len(self.events_from_prev_run)=}.")
+                demisto.info(
+                    f"{self.options.limit=} reached. \
+                    slicing from {len(self.events_from_prev_run)=}."
+                )
                 stored = self.events_from_prev_run[: self.options.limit]
                 self.events_from_prev_run = self.events_from_prev_run[self.options.limit :]
                 return stored
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Mimecast/Integrations/MimecastEventCollector/MimecastEventCollector.py#L124'>Packs/Mimecast/Integrations/MimecastEventCollector/MimecastEventCollector.py~L124</a>
```diff
         for logs in self._iter_events():
             stored.extend(logs)
             if self.options.limit and len(stored) >= self.options.limit:
-                demisto.info(f"{self.options.limit=} reached. \
-                    slicing from {len(logs)=}.")
+                demisto.info(
+                    f"{self.options.limit=} reached. \
+                    slicing from {len(logs)=}."
+                )
                 self.events_from_prev_run = stored[self.options.limit :]
                 demisto.info(f"storing {len(self.events_from_prev_run)} siem events for next run")
                 return stored[: self.options.limit]
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MinervaLabsAntiEvasionPlatform/Integrations/MinervaLabsAntiEvasionPlatform/MinervaLabsAntiEvasionPlatform.py#L284'>Packs/MinervaLabsAntiEvasionPlatform/Integrations/MinervaLabsAntiEvasionPlatform/MinervaLabsAntiEvasionPlatform.py~L284</a>
```diff
         demisto.results(f"Vaccination with id {vaccine_id} was not found")
         return
     if response.status_code != 200:
-        return_error(f"Error while deleting vaccination id: {vaccine_id}. More information: {response.status_code},\
-                     {response.reason}")
+        return_error(
+            f"Error while deleting vaccination id: {vaccine_id}. More information: {response.status_code},\
+                     {response.reason}"
+        )
     demisto.results(f"Vaccine '{vaccine_id}' was deleted")
 
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/MinervaLabsAntiEvasionPlatform/Integrations/MinervaLabsAntiEvasionPlatform/MinervaLabsAntiEvasionPlatform.py#L294'>Packs/MinervaLabsAntiEvasionPlatform/Integrations/MinervaLabsAntiEvasionPlatform/MinervaLabsAntiEvasionPlatform.py~L294</a>
```diff
     json_params = {"filters": [{"param": search_param, "condition": search_condition, "value": search_value}]}
     response = session.post(search_url, json=json_params)
     if response.status_code != 200:
-        return_error(f'Error while perfroming search for\
-                     {search_url.rsplit("/")[1]}. More information: {response.status_code}, {response.reason}')
+        return_error(
+            f'Error while perfroming search for\
+                     {search_url.rsplit("/")[1]}. More information: {response.status_code}, {response.reason}'
+        )
 
     results = {
         "Type": entryTypes["note"],
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/NutanixHypervisor/Integrations/NutanixHypervisor/NutanixHypervisor.py#L126'>Packs/NutanixHypervisor/Integrations/NutanixHypervisor/NutanixHypervisor.py~L126</a>
```diff
             return self._http_request(method=method, url_suffix=url_suffix, params=params, json_data=json_data)
         except DemistoException as e:
             if "Invalid filter criteria specified." in str(e):
-                raise DemistoException("""Filter criteria given is invalid or is not written in the correct format.
-                 Use the 'filter' argument description 'to build your filter correctly.""")
+                raise DemistoException(
+                    """Filter criteria given is invalid or is not written in the correct format.
+                 Use the 'filter' argument description 'to build your filter correctly."""
+                )
 
             if "Unrecognized field" in str(e):
                 raise DemistoException("Filter criteria given is invalid.")
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Okta/Integrations/OktaEventCollector/OktaEventCollector.py#L69'>Packs/Okta/Integrations/OktaEventCollector/OktaEventCollector.py~L69</a>
```diff
             res: requests.models.Response = exc.res
             status_code: int = res.status_code
             if status_code == HTTPStatus.TOO_MANY_REQUESTS.value:
-                demisto.debug(f'fetch-events Got 429. okta rate limit headers:\n \
+                demisto.debug(
+                    f'fetch-events Got 429. okta rate limit headers:\n \
                 x-rate-limit-remaining: {res.headers["x-rate-limit-remaining"]}\n \
-                    x-rate-limit-reset: {res.headers["x-rate-limit-reset"]}\n')
+                    x-rate-limit-reset: {res.headers["x-rate-limit-reset"]}\n'
+                )
                 return stored_events, int(res.headers["x-rate-limit-reset"])
             return stored_events, 0
         except Exception as exc:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Okta/Integrations/OktaEventCollector/OktaEventCollector.py#L116'>Packs/Okta/Integrations/OktaEventCollector/OktaEventCollector.py~L116</a>
```diff
 
         sleep_time = abs(epoch_time_to_continue_fetch - start_time_epoch)
         if sleep_time and sleep_time < FETCH_TIME_LIMIT:
-            demisto.debug(f"Will try fetch again in: {sleep_time},\
-                as a result of 429 Too Many Requests HTTP status.")
+            demisto.debug(
+                f"Will try fetch again in: {sleep_time},\
+                as a result of 429 Too Many Requests HTTP status."
+            )
             time.sleep(sleep_time)  # pylint: disable=E9003
         else:
             break
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L52'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L52</a>
```diff
     """
     monkeypatch.setattr(demisto, "params", lambda: {"url": "https://test.com"})
 
-    mock_alert_response = json.loads("""{"ver":"v4.0","api":"/alert/list","items":[{"date":"2020-01-15T05:06:50.540Z",
+    mock_alert_response = json.loads(
+        """{"ver":"v4.0","api":"/alert/list","items":[{"date":"2020-01-15T05:06:50.540Z",
 "name":"foo","description":"The baseline","zb_ticketid":"alert-Ob81iwWe"},{"date":"2020-01-15T05:06:50.540Z",
-"name":"bar","description":"x","zb_ticketid":"alert-Lqy4ikEz"}]}""")
+"name":"bar","description":"x","zb_ticketid":"alert-Lqy4ikEz"}]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/alert/list?customerid=foobar&offset=0&pagelength=10&stime=-1"
         "&type=policy_alert&resolved=no&sortfield=date&sortdirection=asc",
         json=mock_alert_response,
     )
 
-    mock_vuln_response = json.loads("""{"ver":"v4.0","api":"/vulnerability/list","items":[{"name":"HPD41936",
+    mock_vuln_response = json.loads(
+        """{"ver":"v4.0","api":"/vulnerability/list","items":[{"name":"HPD41936",
 "ip":"10.55.132.114","deviceid":"a0:d3:c1:d4:19:36","detected_date":"2020-05-31T23:59:59.000Z",
-"vulnerability_name":"SMB v1 Usage"}]}""")
+"vulnerability_name":"SMB v1 Usage"}]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/vulnerability/list?customerid=foobar&offset=0&pagelength=10"
         "&stime=1970-01-01T00:00:00.001000Z&type=vulnerability&status=Confirmed&groupby=device",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L93'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L93</a>
```diff
     """
     monkeypatch.setattr(demisto, "params", lambda: {"url": "https://test.com"})
 
-    mock_alert_response = json.loads("""{"items": [
+    mock_alert_response = json.loads(
+        """{"items": [
         {
             "name": "alert1",
             "date": "2019-11-07T23:11:30.509Z",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L105'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L105</a>
```diff
             "date": "2019-11-07T23:11:31.509Z",
             "zb_ticketid": "zb_ticketid2"
         }
-    ]}""")
+    ]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/alert/list?customerid=foobar&offset=0&pagelength=2&stime=-1"
         "&type=policy_alert&resolved=no&sortfield=date&sortdirection=asc",
         json=mock_alert_response,
     )
-    mock_alert_response = json.loads("""{"items": [
+    mock_alert_response = json.loads(
+        """{"items": [
         {
             "name": "alert3",
             "date": "2019-11-07T23:11:31.509Z",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L122'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L122</a>
```diff
             "date": "2019-11-07T23:11:31.509Z",
             "zb_ticketid": "zb_ticketid4"
         }
-    ]}""")
+    ]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/alert/list?customerid=foobar&offset=2&pagelength=2&stime=-1"
         "&type=policy_alert&resolved=no&sortfield=date&sortdirection=asc",
         json=mock_alert_response,
     )
-    mock_alert_response = json.loads("""{"items": [
+    mock_alert_response = json.loads(
+        """{"items": [
         {
             "name": "alert5",
             "date": "2019-11-07T23:11:31.509Z",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L139'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L139</a>
```diff
             "date": "2019-12-07T12:01:32.509Z",
             "zb_ticketid": "zb_ticketid6"
         }
-    ]}""")
+    ]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/alert/list?customerid=foobar&offset=4&pagelength=2&stime=-1"
         "&type=policy_alert&resolved=no&sortfield=date&sortdirection=asc",
         json=mock_alert_response,
     )
 
-    mock_vuln_response = json.loads("""{"items": [
+    mock_vuln_response = json.loads(
+        """{"items": [
         {
             "name": "vuln1",
             "detected_date": "2019-11-07T23:11:30.509Z",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L161'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L161</a>
```diff
             "vulnerability_name": "vname2",
             "deviceid": "deviceid2"
         }
-    ]}""")
+    ]}"""
+    )
     requests_mock.get(
         "https://test.com/pub/v4.0/vulnerability/list?customerid=foobar&offset=0&pagelength=2&stime=-1"
         "&type=vulnerability&status=Confirmed&groupby=device",
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py#L246'>Packs/PaloAltoNetworks_IoT/Integrations/PaloAltoNetworks_IoT/PaloAltoNetworks_IoT_test.py~L246</a>
```diff
     Then
     - Ensure the api URL is correct with the right parameters and payload
     """
-    mock_response = json.loads("""{"api":"/vulnerability/update","ver":"v4.0","updatedVulnerInstanceList":[{
-"newScore":10,"newLevel":"Low","newAnomalyMap":{"application":0,"protocol":0,"payload":0,"external":0,"internal":0}}]}""")
+    mock_response = json.loads(
+        """{"api":"/vulnerability/update","ver":"v4.0","updatedVulnerInstanceList":[{
+"newScore":10,"newLevel":"Low","newAnomalyMap":{"application":0,"protocol":0,"payload":0,"external":0,"internal":0}}]}"""
+    )
     adapter = requests_mock.put("https://test.com/pub/v4.0/vulnerability/update?customerid=foobar", json=mock_response)
 
     client = Client(base_url="https://test.com/pub/v4.0", tenant_id="foobar", verify=False)
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py#L2416'>Packs/QRadar/Integrations/QRadar_v3/QRadar_v3.py~L2416</a>
```diff
     status = args.get("status")
     closing_reason_id = args.get("closing_reason_id")
     if status == "CLOSED" and (not closing_reason_id and not closing_reason_name):
-        raise DemistoException("""Closing reason ID must be provided when closing an offense. Available closing reasons can be achieved
-             by 'qradar-closing-reasons' command.""")
+        raise DemistoException(
+            """Closing reason ID must be provided when closing an offense. Available closing reasons can be achieved
+             by 'qradar-closing-reasons' command."""
+        )
 
     if closing_reason_name:
         # if this call fails, raise an error and stop command execution
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py#L460'>Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py~L460</a>
```diff
 
         if arg in fields_to_clear:
             if input_arg:
-                raise DemistoException(f"Could not set a value for the argument '{arg}' and add it to the clear_fields. \
-                You can either set or clear the field value.")
+                raise DemistoException(
+                    f"Could not set a value for the argument '{arg}' and add it to the clear_fields. \
+                You can either set or clear the field value."
+                )
             ticket_fields[arg] = ""
         elif input_arg:
             if arg in ["impact", "urgency", "severity"]:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py#L2773'>Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py~L2773</a>
```diff
 
     # if custom close code parameter is set and ticket close code is returned from the SNOW incident
     if server_custom_close_code and ticket_close_code:
-        demisto.debug(f"trying to close XSOAR incident using custom resolution code: {server_custom_close_code}, with \
-            received close code: {ticket_close_code}")
+        demisto.debug(
+            f"trying to close XSOAR incident using custom resolution code: {server_custom_close_code}, with \
+            received close code: {ticket_close_code}"
+        )
         # parse custom close code parameter into a dictionary of custom close codes and their names (label)
         server_close_custom_code_dict = dict(item.strip().split("=") for item in server_custom_close_code.split(","))
         # check if close code is in the parsed dictionary
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py#L2783'>Packs/ServiceNow/Integrations/ServiceNowv2/ServiceNowv2.py~L2783</a>
```diff
             return close_code_label
     # if custom state parameter is set and ticket state is returned from incident is not empty
     if server_close_custom_state and ticket_state:
-        demisto.debug(f"trying to close XSOAR incident using custom states: {server_close_custom_state}, with \
-            received state code: {ticket_state}")
+        demisto.debug(
+            f"trying to close XSOAR incident using custom states: {server_close_custom_state}, with \
+            received state code: {ticket_state}"
+        )
         # parse custom state parameter into a dictionary of custom state codes and their names (label)
         server_close_custom_state_dict = dict(item.strip().split("=") for item in server_close_custom_state.split(","))
         # check if state code is in the parsed dictionary
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py#L1132'>Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py~L1132</a>
```diff
                     else:
                         demisto.debug(f"Enrichment {enrichment.type} for notable {notable.id} is still not done")
                 except Exception as e:
-                    demisto.error(f"Caught an exception while retrieving {enrichment.type}\
-                        enrichment results for notable {notable.id}: {str(e)}")
+                    demisto.error(
+                        f"Caught an exception while retrieving {enrichment.type}\
+                        enrichment results for notable {notable.id}: {str(e)}"
+                    )
                     enrichment.status = Enrichment.FAILED
 
         if notable.handled():
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py#L1144'>Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py~L1144</a>
```diff
 
     else:
         task_status = True
-        demisto.debug(f"Open enrichment {notable.id} has exceeded the enrichment timeout of {enrichment_timeout}.\
-            Submitting the notable without the enrichment.")
+        demisto.debug(
+            f"Open enrichment {notable.id} has exceeded the enrichment timeout of {enrichment_timeout}.\
+            Submitting the notable without the enrichment."
+        )
 
     return task_status
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py#L1188'>Packs/SplunkPy/Integrations/SplunkPy/SplunkPy.py~L1188</a>
```diff
         demisto.debug(f"Submitted {len(submitted_notables)}/{total} notables successfully.")
 
     if failed_notables:
-        demisto.debug(f"The following {len(failed_notables)} notables failed the enrichment process: \
+        demisto.debug(
+            f"The following {len(failed_notables)} notables failed the enrichment process: \
             {[notable.id for notable in failed_notables]}, \
-            creating incidents without enrichment.")
+            creating incidents without enrichment."
+        )
 
 
 def submit_notable(service: client.Service, notable: Notable, num_enrichment_events) -> bool:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/SumoLogic_Cloud_SIEM/Integrations/SumoLogicCloudSIEM/SumoLogicCloudSIEM.py#L810'>Packs/SumoLogic_Cloud_SIEM/Integrations/SumoLogicCloudSIEM/SumoLogicCloudSIEM.py~L810</a>
```diff
             resolution = insight_resolution
             if resolution == "No Action":
                 resolution = "Other"
-            demisto.info(f"Closing incident related to Sumo Logic Insight {insight_id} with resolution {insight_resolution}, \
-                which is mapped to XSOAR reason: {resolution}")
+            demisto.info(
+                f"Closing incident related to Sumo Logic Insight {insight_id} with resolution {insight_resolution}, \
+                which is mapped to XSOAR reason: {resolution}"
+            )
             entries = [
                 {
                     "Type": EntryType.NOTE,
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/SumoLogic_Cloud_SIEM/Integrations/SumoLogicCloudSIEM/SumoLogicCloudSIEM.py#L851'>Packs/SumoLogic_Cloud_SIEM/Integrations/SumoLogicCloudSIEM/SumoLogicCloudSIEM.py~L851</a>
```diff
         return insight_id
 
     if parsed_args.incident_changed and delta:
-        demisto.debug(f"Got the following delta keys {str(list(delta.keys()))} to update incident \
-            corresponding to Insight {insight_id}")
+        demisto.debug(
+            f"Got the following delta keys {str(list(delta.keys()))} to update incident \
+            corresponding to Insight {insight_id}"
+        )
         demisto.debug(f"Got the following delta {delta} to update incident corresponding to Insight {insight_id}")
         demisto.debug(f"Incident Id: {incident_id}")
         changed_data = {field: "" for field in OUTGOING_MIRRORED_FIELDS}
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Tenable_sc/Integrations/Tenable_sc/Tenable_sc.py#L133'>Packs/Tenable_sc/Integrations/Tenable_sc/Tenable_sc.py~L133</a>
```diff
                 error = res.json()
             except Exception:
                 # type: ignore
-                raise DemistoException(f"Error: Got status code {str(res.status_code)} with {url=} \
-                    with body {res.content} with headers {str(res.headers)}")  # type: ignore
-
-            raise DemistoException(f"Error: Got an error from TenableSC, code: {error['error_code']}, \
-                        details: {error['error_msg']}")  # type: ignore
+                raise DemistoException(
+                    f"Error: Got status code {str(res.status_code)} with {url=} \
+                    with body {res.content} with headers {str(res.headers)}"
+                )  # type: ignore
+
+            raise DemistoException(
+                f"Error: Got an error from TenableSC, code: {error['error_code']}, \
+                        details: {error['error_msg']}"
+            )  # type: ignore
         return res.json()
 
     def login(self):
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Tenable_sc/Integrations/Tenable_sc/Tenable_sc.py#L177'>Packs/Tenable_sc/Integrations/Tenable_sc/Tenable_sc.py~L177</a>
```diff
         res = self.session.request("post", url, headers=headers, data=json.dumps(login_body), verify=self.verify_ssl)
 
         if res.status_code < 200 or res.status_code >= 300:
-            raise DemistoException(f"Error: Got status code {str(res.status_code)} with {url=} \
-                        with body {res.content} with headers {str(res.headers)}")  # type: ignore
+            raise DemistoException(
+                f"Error: Got status code {str(res.status_code)} with {url=} \
+                        with body {res.content} with headers {str(res.headers)}"
+            )  # type: ignore
 
         self.cookie = res.cookies.get("TNS_SESSIONID", self.cookie)
         demisto.setIntegrationContext({"cookie": self.cookie})
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Tufin/Integrations/Tufin/Tufin.py#L19'>Packs/Tufin/Integrations/Tufin/Tufin.py~L19</a>
```diff
 API: https://<SecureChange IP Address>/securechangeworkflow/api/securechange/tickets/<tickt_ID>.json
 """
 
-FW_CHANGE_REQ = json.loads("""{ "ticket": { "subject": "", "priority": "", "workflow": { "name": "Firewall Change Request",
+FW_CHANGE_REQ = json.loads(
+    """{ "ticket": { "subject": "", "priority": "", "workflow": { "name": "Firewall Change Request",
                            "uses_topology": true }, "steps": { "step": [ { "name": "Submit Access Request", "tasks": { "task":
                            { "fields": { "field": { "@xsi.type": "multi_access_request", "name": "Required Access",
                            "access_request": { "use_topology": true, "targets": { "target": { "@type": "ANY" } },
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Tufin/Integrations/Tufin/Tufin.py#L27'>Packs/Tufin/Integrations/Tufin/Tufin.py~L27</a>
```diff
                            "netmask": "255.255.255.255", "cidr": 32 } ] }, "destinations": { "destination": [ { "@type": "IP",
                            "ip_address": "", "netmask": "255.255.255.255", "cidr": 32 } ] }, "services": { "service":
                             [ { "@type": "PROTOCOL", "protocol": "", "port": 0 } ] }, "action": "" } } } } } } ] },
-                            "comments": "" } }""")
-SERVER_DECOM_REQ = json.loads("""{ "ticket": { "subject": "", "priority": "", "workflow": { "name":
+                            "comments": "" } }"""
+)
+SERVER_DECOM_REQ = json.loads(
+    """{ "ticket": { "subject": "", "priority": "", "workflow": { "name":
                               "Server Decommission Request", "uses_topology": false }, "steps": { "step": [ {
                               "name": "Server Decommission Request", "tasks": { "task": { "fields": { "field": { "@xsi.type":
                               "multi_server_decommission_request", "name": "Request verification",
                               "server_decommission_request": { "servers": { "server": { "@type": "IP", "ip_address": "",
                               "netmask": "255.255.255.255", "cidr": 32 } }, "targets": { "target": { "@type": "ANY" } },
-                              "comment": "" } } } } } } ] }, "comments": "" }}""")
+                              "comment": "" } } } } } } ] }, "comments": "" }}"""
+)
 
 # remove proxy if not set to true in params
 if not demisto.params().get("proxy"):
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Tufin/Integrations/Tufin/Tufin.py#L415'>Packs/Tufin/Integrations/Tufin/Tufin.py~L415</a>
```diff
                 or port is None
                 or action is None
             ):
-                return_error("""Request Type, Subject, Priority, Source, Destination, Protocol,
-                             Port and Action parameters are mandatory for this request type""")
+                return_error(
+                    """Request Type, Subject, Priority, Source, Destination, Protocol,
+                             Port and Action parameters are mandatory for this request type"""
+                )
             if priority.capitalize() not in ["Critical", "High", "Normal", "Low"]:
                 return_error("Priority must be Critical, High, Normal or Low")
             if proto.upper() not in ["TCP", "UDP"]:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Vectra_AI/Integrations/VectraAIEventCollector/VectraAIEventCollector.py#L151'>Packs/Vectra_AI/Integrations/VectraAIEventCollector/VectraAIEventCollector.py~L151</a>
```diff
         return event
 
     except Exception as e:
-        demisto.debug(f"""Failed adding parsing rules to event '{str(event)}': {str(e)}.
-            Will be added in ingestion time""")
+        demisto.debug(
+            f"""Failed adding parsing rules to event '{str(event)}': {str(e)}.
+            Will be added in ingestion time"""
+        )
 
         return event
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/XSOARmirroring/Integrations/XSOARmirroring/XSOARmirroring.py#L640'>Packs/XSOARmirroring/Integrations/XSOARmirroring/XSOARmirroring.py~L640</a>
```diff
             del XSOARMirror_mirror_reset[remote_args.remote_incident_id]
             integration_context[MIRROR_RESET] = XSOARMirror_mirror_reset
             set_to_integration_context_with_retries(context=integration_context)
-            demisto.debug(f"Removed incident id: {remote_args.remote_incident_id} from XSOARMirror_mirror_reset\
-                context data list.")
+            demisto.debug(
+                f"Removed incident id: {remote_args.remote_incident_id} from XSOARMirror_mirror_reset\
+                context data list."
+            )
 
         formatted_entries = []
         # file_attachments = []
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Zoom/Integrations/Zoom/Zoom.py#L757'>Packs/Zoom/Integrations/Zoom/Zoom.py~L757</a>
```diff
         if MIRRORING_ENABLED and (
             not LONG_RUNNING or not SECRET_TOKEN or not client.bot_client_id or not client.bot_client_secret or not client.bot_jid
         ):
-            raise DemistoException("""Mirroring is enabled, however long running is disabled
+            raise DemistoException(
+                """Mirroring is enabled, however long running is disabled
 or the necessary bot authentication parameters are missing.
 For mirrors to work correctly, long running must be enabled and you must provide all
 the zoom-bot following parameters:
 secret token,
 Bot JID,
-bot client id and secret id""")
+bot client id and secret id"""
+            )
         client.zoom_list_users(page_size=1, url_suffix="users")
         if client.bot_access_token:
             json_data = {
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Zoom/Integrations/Zoom/Zoom.py#L2098'>Packs/Zoom/Integrations/Zoom/Zoom.py~L2098</a>
```diff
         except DemistoException as e:
             error_message = e.message
             if "The next page token is invalid or expired." in error_message and next_page_token:
-                raise DemistoException(f"Please ensure that the correct argument values are used when attempting to use \
+                raise DemistoException(
+                    f"Please ensure that the correct argument values are used when attempting to use \
 the next_page_toke.\n Note that when using next_page_token it is mandatory to specify date time and not relative time.\n \
-To find the appropriate values, refer to the ChatMessageNextToken located in the context. \n {error_message}")
+To find the appropriate values, refer to the ChatMessageNextToken located in the context. \n {error_message}"
+                )
 
     outputs = []
     for i in all_messages:
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Zoom/Integrations/Zoom/Zoom.py#L2440'>Packs/Zoom/Integrations/Zoom/Zoom.py~L2440</a>
```diff
     CACHED_INTEGRATION_CONTEXT = get_integration_context(SYNC_CONTEXT)
 
     if MIRRORING_ENABLED and (not LONG_RUNNING or not SECRET_TOKEN or not bot_client_id or not bot_client_secret or not bot_jid):
-        raise DemistoException("""Mirroring is enabled, however long running is disabled
+        raise DemistoException(
+            """Mirroring is enabled, however long running is disabled
 or the necessary bot authentication parameters are missing.
 For mirrors to work correctly, long running must be enabled and you must provide all
 the zoom-bot following parameters:
 secret token,
 Bot JID,
-bot client id and secret id""")
+bot client id and secret id"""
+        )
     if LONG_RUNNING:
         try:
             port = int(params.get("longRunningPort"))
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Packs/Zoom/Integrations/Zoom_IAM/Zoom_IAM.py#L206'>Packs/Zoom/Integrations/Zoom_IAM/Zoom_IAM.py~L206</a>
```diff
 
 def check_authentication_type_arguments(api_key: str, api_secret: str, account_id: str, client_id: str, client_secret: str):
     if any((api_key, api_secret)) and any((account_id, client_id, client_secret)):
-        raise DemistoException("""Too many fields were filled.
+        raise DemistoException(
+            """Too many fields were filled.
                                    You should fill the Account ID, Client ID, and Client Secret fields (OAuth),
-                                   OR the API Key and API Secret fields (JWT - Deprecated)""")
+                                   OR the API Key and API Secret fields (JWT - Deprecated)"""
+        )
 
 
 def main():  # pragma: no cover
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Tests/Marketplace/marketplace_services.py#L1547'>Tests/Marketplace/marketplace_services.py~L1547</a>
```diff
         logging.debug(f"Release notes after filtering by display -\n{filtered_release_notes}")
 
         if not filtered_release_notes:
-            logging.debug(f"Didn't find relevant release notes entries after filtering by display name.\n \
-                            Release notes: {release_notes}")
+            logging.debug(
+                f"Didn't find relevant release notes entries after filtering by display name.\n \
+                            Release notes: {release_notes}"
+            )
 
         return filtered_release_notes
 
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Tests/Marketplace/upload_packs.py#L792'>Tests/Marketplace/upload_packs.py~L792</a>
```diff
         fail_build (bool): indicates whether to fail the build upon failing pack to upload or not
 
     """
-    logging.info(f"""\n
+    logging.info(
+        f"""\n
 ------------------------------------------ Packs Upload Summary ------------------------------------------
 Total number of packs: {len(successful_packs + skipped_packs + failed_packs)}
-----------------------------------------------------------------------------------------------------------""")
+----------------------------------------------------------------------------------------------------------"""
+    )
 
     if successful_packs:
         successful_packs_table = _build_summary_table(successful_packs)
```
<a href='https://github.com/demisto/content/blob/f7043f77765fceb0136f529ab560f1a1314b2077/Tests/private_build/marketplace_services_private.py#L1813'>Tests/private_build/marketplace_services_private.py~L1813</a>
```diff
         logging.debug(f"Release notes after filtering by display -\n{filtered_release_notes}")
 
         if not filtered_release_notes:
-            logging.debug(f"Didn't find relevant release notes entries after filtering by display name.\n \
-                            Release notes: {release_notes}")
+            logging.debug(
+                f"Didn't find relevant release notes entries after filtering by display name.\n \
+                            Release notes: {release_notes}"
+            )
 
         return filtered_release_notes
 
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+44 -22 lines across 9 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/journalist_gui/journalist_gui/SecureDropUpdater.py#L187'>journalist_gui/journalist_gui/SecureDropUpdater.py~L187</a>
```diff
 
         # Connect buttons to their functions.
         self.pushButton.setText(strings.install_later_button)
-        self.pushButton.setStyleSheet("""background-color: lightgrey;
+        self.pushButton.setStyleSheet(
+            """background-color: lightgrey;
                                       min-height: 2em;
-                                      border-radius: 10px""")
+                                      border-radius: 10px"""
+        )
         self.pushButton.clicked.connect(self.close)
         self.pushButton_2.setText(strings.install_update_button)
-        self.pushButton_2.setStyleSheet("""background-color: #E6FFEB;
+        self.pushButton_2.setStyleSheet(
+            """background-color: #E6FFEB;
                                         min-height: 2em;
-                                        border-radius: 10px;""")
+                                        border-radius: 10px;"""
+        )
         self.pushButton_2.clicked.connect(self.update_securedrop)
         self.update_thread = UpdateThread()
         self.update_thread.signal.connect(self.update_status)
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/3d91d6948753_create_source_uuid_column.py#L32'>securedrop/alembic/versions/3d91d6948753_create_source_uuid_column.py~L32</a>
```diff
 
     for source in sources:
         conn.execute(
-            sa.text("""UPDATE sources_tmp SET uuid=:source_uuid WHERE
-                       id=:id""").bindparams(source_uuid=str(uuid.uuid4()), id=source.id)
+            sa.text(
+                """UPDATE sources_tmp SET uuid=:source_uuid WHERE
+                       id=:id"""
+            ).bindparams(source_uuid=str(uuid.uuid4()), id=source.id)
         )
 
     # Now create new table with unique constraint applied.
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/60f41bb14d98_add_session_nonce_to_journalist.py#L30'>securedrop/alembic/versions/60f41bb14d98_add_session_nonce_to_journalist.py~L30</a>
```diff
 
     for journalist in journalists:
         conn.execute(
-            sa.text("""UPDATE journalists_tmp SET session_nonce=0 WHERE
-                       id=:id""").bindparams(id=journalist.id)
+            sa.text(
+                """UPDATE journalists_tmp SET session_nonce=0 WHERE
+                       id=:id"""
+            ).bindparams(id=journalist.id)
         )
 
     # Now create new table with null constraint applied.
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/6db892e17271_add_reply_uuid.py#L32'>securedrop/alembic/versions/6db892e17271_add_reply_uuid.py~L32</a>
```diff
 
     for reply in replies:
         conn.execute(
-            sa.text("""UPDATE replies_tmp SET uuid=:reply_uuid WHERE
-                       id=:id""").bindparams(reply_uuid=str(uuid.uuid4()), id=reply.id)
+            sa.text(
+                """UPDATE replies_tmp SET uuid=:reply_uuid WHERE
+                       id=:id"""
+            ).bindparams(reply_uuid=str(uuid.uuid4()), id=reply.id)
         )
 
     # Now create new table with constraints applied to UUID column.
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/b58139cfdc8c_add_checksum_columns_revoke_table.py#L47'>securedrop/alembic/versions/b58139cfdc8c_add_checksum_columns_revoke_table.py~L47</a>
```diff
     # we need an app context for the rq worker extension to work properly
     with app.app_context():
         conn = op.get_bind()
-        query = sa.text("""SELECT submissions.id, sources.filesystem_id, submissions.filename
+        query = sa.text(
+            """SELECT submissions.id, sources.filesystem_id, submissions.filename
                            FROM submissions
                            INNER JOIN sources
                            ON submissions.source_id = sources.id
-                        """)
+                        """
+        )
         for sub_id, filesystem_id, filename in conn.execute(query):
             full_path = Storage.get_default().path(filesystem_id, filename)
             create_queue(config.RQ_WORKER_NAME).enqueue(
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/b58139cfdc8c_add_checksum_columns_revoke_table.py#L62'>securedrop/alembic/versions/b58139cfdc8c_add_checksum_columns_revoke_table.py~L62</a>
```diff
                 app.config["SQLALCHEMY_DATABASE_URI"],
             )
 
-        query = sa.text("""SELECT replies.id, sources.filesystem_id, replies.filename
+        query = sa.text(
+            """SELECT replies.id, sources.filesystem_id, replies.filename
                            FROM replies
                            INNER JOIN sources
                            ON replies.source_id = sources.id
-                        """)
+                        """
+        )
         for rep_id, filesystem_id, filename in conn.execute(query):
             full_path = Storage.get_default().path(filesystem_id, filename)
             create_queue(config.RQ_WORKER_NAME).enqueue(
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/c5a02eb52f2d_dropped_session_nonce_from_journalist_.py#L45'>securedrop/alembic/versions/c5a02eb52f2d_dropped_session_nonce_from_journalist_.py~L45</a>
```diff
 
     for journalist in journalists:
         conn.execute(
-            sa.text("""UPDATE journalists_tmp SET session_nonce=0 WHERE
-                       id=:id""").bindparams(id=journalist.id)
+            sa.text(
+                """UPDATE journalists_tmp SET session_nonce=0 WHERE
+                       id=:id"""
+            ).bindparams(id=journalist.id)
         )
 
     # Now create new table with null constraint applied.
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/e0a525cbab83_add_column_to_track_source_deletion_of_.py#L30'>securedrop/alembic/versions/e0a525cbab83_add_column_to_track_source_deletion_of_.py~L30</a>
```diff
 
     for reply in replies:
         conn.execute(
-            sa.text("""UPDATE replies_tmp SET deleted_by_source=0 WHERE
-                       id=:id""").bindparams(id=reply.id)
+            sa.text(
+                """UPDATE replies_tmp SET deleted_by_source=0 WHERE
+                       id=:id"""
+            ).bindparams(id=reply.id)
         )
 
     # Now create new table with not null constraint applied to
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/f2833ac34bb6_add_uuid_column_for_users_table.py#L32'>securedrop/alembic/versions/f2833ac34bb6_add_uuid_column_for_users_table.py~L32</a>
```diff
 
     for journalist in journalists:
         conn.execute(
-            sa.text("""UPDATE journalists_tmp SET uuid=:journalist_uuid WHERE
-                       id=:id""").bindparams(journalist_uuid=str(uuid.uuid4()), id=journalist.id)
+            sa.text(
+                """UPDATE journalists_tmp SET uuid=:journalist_uuid WHERE
+                       id=:id"""
+            ).bindparams(journalist_uuid=str(uuid.uuid4()), id=journalist.id)
         )
 
     # Now create new table with unique constraint applied.
```
<a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/alembic/versions/fccf57ceef02_create_submission_uuid_column.py#L32'>securedrop/alembic/versions/fccf57ceef02_create_submission_uuid_column.py~L32</a>
```diff
 
     for submission in submissions:
         conn.execute(
-            sa.text("""UPDATE submissions_tmp SET uuid=:submission_uuid WHERE
-                       id=:id""").bindparams(submission_uuid=str(uuid.uuid4()), id=submission.id)
+            sa.text(
+                """UPDATE submissions_tmp SET uuid=:submission_uuid WHERE
+                       id=:id"""
+            ).bindparams(submission_uuid=str(uuid.uuid4()), id=submission.id)
         )
 
     # Now create new table with unique constraint applied.
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+8 -4 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/904c0cdff378edbf9aeb20a5c8fd12057696baf7/ibis/backends/conftest.py#L293'>ibis/backends/conftest.py~L293</a>
```diff
                 if backend := set(util.promote_list(mark.args[0])).difference(
                     ALL_BACKENDS
                 ):
-                    unrecognized_backends.add(f"""Unrecognize backend(s) {backend} passed to {name} marker in
-{item.path}::{item.originalname}""")
+                    unrecognized_backends.add(
+                        f"""Unrecognize backend(s) {backend} passed to {name} marker in
+{item.path}::{item.originalname}"""
+                    )
 
         # add the backend marker to any tests are inside "ibis/backends"
         parts = item.path.parts
```
<a href='https://github.com/ibis-project/ibis/blob/904c0cdff378edbf9aeb20a5c8fd12057696baf7/ibis/backends/impala/tests/test_ddl.py#L307'>ibis/backends/impala/tests/test_ddl.py~L307</a>
```diff
 def temp_char_table(con):
     name = "testing_varchar_support"
     with closing(
-        con.raw_sql(f"""\
+        con.raw_sql(
+            f"""\
 CREATE TABLE IF NOT EXISTS {name} (
   group1 VARCHAR(10),
   group2 CHAR(10)
-)""")
+)"""
+        )
     ):
         pass
     assert name in con.list_tables(), name
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+16 -8 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/7b3a536a3d7c728b607f02faee5a938424a6d218/latch_cli/centromere/ctx.py#L120'>latch_cli/centromere/ctx.py~L120</a>
```diff
                             )
                 if not hasattr(self, "workflow_name"):
                     click.secho(
-                        dedent("""
+                        dedent(
+                            """
                                      Unable to locate workflow code. If you
                                      are a registering a Snakemake project,
                                      make sure to pass the Snakefile path with
-                                     the --snakefile flag."""),
+                                     the --snakefile flag."""
+                        ),
                         fg="red",
                     )
                     raise click.exceptions.Exit(1)
```
<a href='https://github.com/latchbio/latch/blob/7b3a536a3d7c728b607f02faee5a938424a6d218/latch_cli/services/cp/upload.py#L262'>latch_cli/services/cp/upload.py~L262</a>
```diff
     total_time = end - start
     if config.progress != Progress.none:
         click.clear()
-        click.echo(f"""{click.style("Upload Complete", fg="green")}
+        click.echo(
+            f"""{click.style("Upload Complete", fg="green")}
 
 {click.style("Time Elapsed: ", fg="blue")}{human_readable_time(total_time)}
-{click.style("Files Uploaded: ", fg="blue")}{num_files} ({with_si_suffix(total_bytes)})""")
+{click.style("Files Uploaded: ", fg="blue")}{num_files} ({with_si_suffix(total_bytes)})"""
+        )
 
 
 @dataclass(frozen=True)
```
<a href='https://github.com/latchbio/latch/blob/7b3a536a3d7c728b607f02faee5a938424a6d218/latch_cli/services/local_dev.py#L251'>latch_cli/services/local_dev.py~L251</a>
```diff
                     res = subprocess.run(ssh_command, stderr=subprocess.PIPE)
                     if "Too many authentication failures" in res.stderr.decode():
                         click.secho(
-                            dedent("""
+                            dedent(
+                                """
                             Too many authentication failures. Try resetting your ssh-agent with
 
                                 $ ssh-add -D
 
-                            and trying again."""),
+                            and trying again."""
+                            ),
                             fg="red",
                         )
                     stop_rsync.set()
```
<a href='https://github.com/latchbio/latch/blob/7b3a536a3d7c728b607f02faee5a938424a6d218/latch_cli/services/local_dev_old.py#L435'>latch_cli/services/local_dev_old.py~L435</a>
```diff
         except asyncssh.ProtocolError as e:
             if "Too many authentication failures" in str(e):
                 click.secho(
-                    dedent("""
+                    dedent(
+                        """
                     Too many authentication failures. Try resetting your ssh-agent with
 
                         $ ssh-add -D
 
-                    and trying again."""),
+                    and trying again."""
+                    ),
                     fg="red",
                 )
             else:
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/15929d1be0d50a2a0b27191746acaf0a9eaf2242/examples/multithreading_hello_milvus.py#L71'>examples/multithreading_hello_milvus.py~L71</a>
```diff
         print(
             f"Inserted {len(self.batchs)} batches of {num_entities} entities in {duration} seconds"
         )
-        print(f"Expected num_entities: {len(self.batchs)*num_entities}. \
-                Acutal num_entites: {self.get_thread_local_collection().num_entities}")
+        print(
+            f"Expected num_entities: {len(self.batchs)*num_entities}. \
+                Acutal num_entites: {self.get_thread_local_collection().num_entities}"
+        )
 
 
 if __name__ == "__main__":
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+24 -12 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/pyfunc/__init__.py#L1366'>mlflow/pyfunc/__init__.py~L1366</a>
```diff
             result_type = _parse_spark_datatype(result_type)
 
     if not _check_udf_return_type(result_type):
-        raise MlflowException.invalid_parameter_value(f"""Invalid 'spark_udf' result type: {result_type}.
+        raise MlflowException.invalid_parameter_value(
+            f"""Invalid 'spark_udf' result type: {result_type}.
 It must be one of the following types:
 Primitive types:
  - int
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/pyfunc/__init__.py#L1381'>mlflow/pyfunc/__init__.py~L1381</a>
```diff
  - struct<field: primitive | array<primitive> | array<array<primitive>>, ...>:
    A struct with primitive, array<primitive>, or array<array<primitive>>,
    e.g., struct<a:int, b:array<int>>.
-""")
+"""
+        )
     params = _validate_params(params, model_metadata)
 
     def _predict_row_batch(predict_fn, args):
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/utils/docstring_utils.py#L157'>mlflow/utils/docstring_utils.py~L157</a>
```diff
 
 # `{{ ... }}` represents a placeholder.
 LOG_MODEL_PARAM_DOCS = ParamDocs({
-    "conda_env": ("""Either a dictionary representation of a Conda environment or the path to a conda
+    "conda_env": (
+        """Either a dictionary representation of a Conda environment or the path to a conda
 environment yaml file. If provided, this describes the environment this model should be run in.
 At a minimum, it should specify the dependencies contained in :func:`get_default_conda_env()`.
 If ``None``, a conda environment with pip requirements inferred by
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/utils/docstring_utils.py#L178'>mlflow/utils/docstring_utils.py~L178</a>
```diff
                 ],
             },
         ],
-    }"""),
-    "pip_requirements": ("""Either an iterable of pip requirement strings
+    }"""
+    ),
+    "pip_requirements": (
+        """Either an iterable of pip requirement strings
 (e.g. ``["{{ package_name }}", "-r requirements.txt", "-c constraints.txt"]``) or the string path to
 a pip requirements file on the local filesystem (e.g. ``"requirements.txt"``). If provided, this
 describes the environment this model should be run in. If ``None``, a default list of requirements
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/utils/docstring_utils.py#L187'>mlflow/utils/docstring_utils.py~L187</a>
```diff
 If the requirement inference fails, it falls back to using :func:`get_default_pip_requirements`.
 Both requirements and constraints are automatically parsed and written to ``requirements.txt`` and
 ``constraints.txt`` files, respectively, and stored as part of the model. Requirements are also
-written to the ``pip`` section of the model's conda environment (``conda.yaml``) file."""),
-    "extra_pip_requirements": ("""Either an iterable of pip
+written to the ``pip`` section of the model's conda environment (``conda.yaml``) file."""
+    ),
+    "extra_pip_requirements": (
+        """Either an iterable of pip
 requirement strings
 (e.g. ``["pandas", "-r requirements.txt", "-c constraints.txt"]``) or the string path to
 a pip requirements file on the local filesystem (e.g. ``"requirements.txt"``). If provided, this
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/utils/docstring_utils.py#L206'>mlflow/utils/docstring_utils.py~L206</a>
```diff
     - ``extra_pip_requirements``
 
 :ref:`This example<pip-requirements-example>` demonstrates how to specify pip requirements using
-``pip_requirements`` and ``extra_pip_requirements``."""),
-    "signature": ("""an instance of the :py:class:`ModelSignature <mlflow.models.ModelSignature>`
+``pip_requirements`` and ``extra_pip_requirements``."""
+    ),
+    "signature": (
+        """an instance of the :py:class:`ModelSignature <mlflow.models.ModelSignature>`
 class that describes the model's inputs and outputs. If not specified but an
 ``input_example`` is supplied, a signature will be automatically inferred
 based on the supplied input example and model. To disable automatic signature
```
<a href='https://github.com/mlflow/mlflow/blob/412b90072cae6642e7a0adfd8828bb0c67a88c15/mlflow/utils/docstring_utils.py#L225'>mlflow/utils/docstring_utils.py~L225</a>
```diff
     train = df.drop_column("target_label")
     predictions = ...  # compute model predictions
     signature = infer_signature(train, predictions)
-"""),
-    "input_example": ("""one or several instances of valid model input. The input example is used
+"""
+    ),
+    "input_example": (
+        """one or several instances of valid model input. The input example is used
 as a hint of what data to feed the model. It will be converted to a Pandas
 DataFrame and then serialized to json using the Pandas split-oriented
 format, or a numpy array where the example will be serialized to json
 by converting it to a list. Bytes are base64-encoded. When the ``signature`` parameter is
 ``None``, the input example is used to infer a model signature.
-"""),
+"""
+    ),
 })
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+208 -104 lines across 17 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/_version.py#L140'>pandas/_version.py~L140</a>
```diff
         root = os.path.dirname(root)  # up a level
 
     if verbose:
-        print(f"Tried directories {str(rootdirs)} \
-            but none started with prefix {parentdir_prefix}")
+        print(
+            f"Tried directories {str(rootdirs)} \
+            but none started with prefix {parentdir_prefix}"
+        )
     raise NotThisMethod("rootdir doesn't start with parentdir_prefix")
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/frame.py#L7655'>pandas/core/frame.py~L7655</a>
```diff
     @doc(
         Series.swaplevel,
         klass=_shared_doc_kwargs["klass"],
-        extra_params=dedent("""axis : {0 or 'index', 1 or 'columns'}, default 0
+        extra_params=dedent(
+            """axis : {0 or 'index', 1 or 'columns'}, default 0
             The axis to swap levels on. 0 or 'index' for row-wise, 1 or
-            'columns' for column-wise."""),
-        examples=dedent("""\
+            'columns' for column-wise."""
+        ),
+        examples=dedent(
+            """\
         Examples
         --------
         >>> df = pd.DataFrame(
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/frame.py#L7708'>pandas/core/frame.py~L7708</a>
```diff
         History     Final exam  January         A
         Geography   Final exam  February        B
         History     Coursework  March           A
-        Geography   Coursework  April           C"""),
+        Geography   Coursework  April           C"""
+        ),
     )
     def swaplevel(self, i: Axis = -2, j: Axis = -1, axis: Axis = 0) -> DataFrame:
         result = self.copy(deep=None)
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/frame.py#L9832'>pandas/core/frame.py~L9832</a>
```diff
         extra_params="axis : {0 or 'index', 1 or 'columns'}, default 0\n    "
         "Take difference over rows (0) or columns (1).\n",
         other_klass="Series",
-        examples=dedent("""
+        examples=dedent(
+            """
         Difference with previous row
 
         >>> df = pd.DataFrame({'a': [1, 2, 3, 4, 5, 6],
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/frame.py#L9895'>pandas/core/frame.py~L9895</a>
```diff
         >>> df.diff()
                a
         0    NaN
-        1  255.0"""),
+        1  255.0"""
+        ),
     )
     def diff(self, periods: int = 1, axis: Axis = 0) -> DataFrame:
         if not lib.is_integer(periods):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L991'>pandas/core/groupby/groupby.py~L991</a>
```diff
 
     @Substitution(
         klass="GroupBy",
-        examples=dedent("""\
+        examples=dedent(
+            """\
         >>> df = pd.DataFrame({'A': 'a b a b'.split(), 'B': [1, 2, 3, 4]})
         >>> df
            A  B
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L1007'>pandas/core/groupby/groupby.py~L1007</a>
```diff
            B
         A
         a  2
-        b  2"""),
+        b  2"""
+        ),
     )
     @Appender(_pipe_template)
     def pipe(
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3086'>pandas/core/groupby/groupby.py~L3086</a>
```diff
         mc=0,
         e=None,
         ek=None,
-        example=dedent("""\
+        example=dedent(
+            """\
         For SeriesGroupBy:
 
         >>> lst = ['a', 'a', 'b', 'b']
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3117'>pandas/core/groupby/groupby.py~L3117</a>
```diff
              b   c
         a
         1   10   7
-        2   11  17"""),
+        2   11  17"""
+        ),
     )
     def sum(
         self,
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3155'>pandas/core/groupby/groupby.py~L3155</a>
```diff
         fname="prod",
         no=False,
         mc=0,
-        example=dedent("""\
+        example=dedent(
+            """\
         For SeriesGroupBy:
 
         >>> lst = ['a', 'a', 'b', 'b']
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3186'>pandas/core/groupby/groupby.py~L3186</a>
```diff
              b    c
         a
         1   16   10
-        2   30   72"""),
+        2   30   72"""
+        ),
     )
     def prod(self, numeric_only: bool = False, min_count: int = 0) -> NDFrameT:
         return self._agg_general(
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3201'>pandas/core/groupby/groupby.py~L3201</a>
```diff
         mc=-1,
         e=None,
         ek=None,
-        example=dedent("""\
+        example=dedent(
+            """\
         For SeriesGroupBy:
 
         >>> lst = ['a', 'a', 'b', 'b']
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3232'>pandas/core/groupby/groupby.py~L3232</a>
```diff
             b  c
         a
         1   2  2
-        2   5  8"""),
+        2   5  8"""
+        ),
     )
     def min(
         self,
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3267'>pandas/core/groupby/groupby.py~L3267</a>
```diff
         mc=-1,
         e=None,
         ek=None,
-        example=dedent("""\
+        example=dedent(
+            """\
         For SeriesGroupBy:
 
         >>> lst = ['a', 'a', 'b', 'b']
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/groupby/groupby.py#L3298'>pandas/core/groupby/groupby.py~L3298</a>
```diff
             b  c
         a
         1   8  5
-        2   6  9"""),
+        2   6  9"""
+        ),
     )
     def max(
         self,
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/indexes/interval.py#L241'>pandas/core/indexes/interval.py~L241</a>
```diff
         _interval_shared_docs["from_breaks"]
         % {
             "klass": "IntervalIndex",
-            "name": textwrap.dedent("""
+            "name": textwrap.dedent(
+                """
              name : str, optional
-                  Name of the resulting IntervalIndex."""),
+                  Name of the resulting IntervalIndex."""
+            ),
             "examples": textwrap.dedent("""\
         Examples
         --------
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/indexes/interval.py#L272'>pandas/core/indexes/interval.py~L272</a>
```diff
         _interval_shared_docs["from_arrays"]
         % {
             "klass": "IntervalIndex",
-            "name": textwrap.dedent("""
+            "name": textwrap.dedent(
+                """
              name : str, optional
-                  Name of the resulting IntervalIndex."""),
+                  Name of the resulting IntervalIndex."""
+            ),
             "examples": textwrap.dedent("""\
         Examples
         --------
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/indexes/interval.py#L304'>pandas/core/indexes/interval.py~L304</a>
```diff
         _interval_shared_docs["from_tuples"]
         % {
             "klass": "IntervalIndex",
-            "name": textwrap.dedent("""
+            "name": textwrap.dedent(
+                """
              name : str, optional
-                  Name of the resulting IntervalIndex."""),
+                  Name of the resulting IntervalIndex."""
+            ),
             "examples": textwrap.dedent("""\
         Examples
         --------
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L1866'>pandas/core/series.py~L1866</a>
```diff
     @doc(
         klass=_shared_doc_kwargs["klass"],
         storage_options=_shared_docs["storage_options"],
-        examples=dedent("""Examples
+        examples=dedent(
+            """Examples
             --------
             >>> s = pd.Series(["elk", "pig", "dog", "quetzal"], name="animal")
             >>> print(s.to_markdown())
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L1890'>pandas/core/series.py~L1890</a>
```diff
             |  2 | dog      |
             +----+----------+
             |  3 | quetzal  |
-            +----+----------+"""),
+            +----+----------+"""
+        ),
     )
     def to_markdown(
         self,
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L2989'>pandas/core/series.py~L2989</a>
```diff
         klass="Series",
         extra_params="",
         other_klass="DataFrame",
-        examples=dedent("""
+        examples=dedent(
+            """
         Difference with previous row
 
         >>> s = pd.Series([1, 1, 2, 3, 5, 8])
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L3030'>pandas/core/series.py~L3030</a>
```diff
         >>> s.diff()
         0      NaN
         1    255.0
-        dtype: float64"""),
+        dtype: float64"""
+        ),
     )
     def diff(self, periods: int = 1) -> Series:
         """
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L4285'>pandas/core/series.py~L4285</a>
```diff
 
     @doc(
         klass=_shared_doc_kwargs["klass"],
-        extra_params=dedent("""copy : bool, default True
+        extra_params=dedent(
+            """copy : bool, default True
             Whether to copy underlying data.
 
             .. note::
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L4298'>pandas/core/series.py~L4298</a>
```diff
                 future version of pandas.
 
                 You can already get the future behavior and improvements through
-                enabling copy on write ``pd.options.mode.copy_on_write = True``"""),
-        examples=dedent("""\
+                enabling copy on write ``pd.options.mode.copy_on_write = True``"""
+        ),
+        examples=dedent(
+            """\
         Examples
         --------
         >>> s = pd.Series(
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/series.py#L4349'>pandas/core/series.py~L4349</a>
```diff
         Geography   Final exam  February        B
         History     Coursework  March           A
         Geography   Coursework  April           C
-        dtype: object"""),
+        dtype: object"""
+        ),
     )
     def swaplevel(
         self, i: Level = -2, j: Level = -1, copy: bool | None = None
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/core/sorting.py#L584'>pandas/core/sorting.py~L584</a>
```diff
             #  error: Too many arguments for "ExtensionArray"
             result = type_of_values(result)  # type: ignore[call-arg]
     except TypeError:
-        raise TypeError(f"User-provided `key` function returned an invalid type {type(result)} \
-            which could not be converted to {type(values)}.")
+        raise TypeError(
+            f"User-provided `key` function returned an invalid type {type(result)} \
+            which could not be converted to {type(values)}."
+        )
 
     return result
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/formats/info.py#L33'>pandas/io/formats/info.py~L33</a>
```diff
     )
 
 
-frame_max_cols_sub = dedent("""\
+frame_max_cols_sub = dedent(
+    """\
     max_cols : int, optional
         When to switch from the verbose to the truncated output. If the
         DataFrame has more than `max_cols` columns, the truncated output
         is used. By default, the setting in
-        ``pandas.options.display.max_info_columns`` is used.""")
+        ``pandas.options.display.max_info_columns`` is used."""
+)
 
 
-show_counts_sub = dedent("""\
+show_counts_sub = dedent(
+    """\
     show_counts : bool, optional
         Whether to show the non-null counts. By default, this is shown
         only if the DataFrame is smaller than
         ``pandas.options.display.max_info_rows`` and
         ``pandas.options.display.max_info_columns``. A value of True always
-        shows the counts, and False never shows the counts.""")
+        shows the counts, and False never shows the counts."""
+)
 
 
-frame_examples_sub = dedent("""\
+frame_examples_sub = dedent(
+    """\
     >>> int_values = [1, 2, 3, 4, 5]
     >>> text_values = ['alpha', 'beta', 'gamma', 'delta', 'epsilon']
     >>> float_values = [0.0, 0.25, 0.5, 0.75, 1.0]
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/formats/info.py#L131'>pandas/io/formats/info.py~L131</a>
```diff
      1   column_2  1000000 non-null  object
      2   column_3  1000000 non-null  object
     dtypes: object(3)
-    memory usage: 165.9 MB""")
+    memory usage: 165.9 MB"""
+)
 
 
-frame_see_also_sub = dedent("""\
+frame_see_also_sub = dedent(
+    """\
     DataFrame.describe: Generate descriptive statistics of DataFrame
         columns.
-    DataFrame.memory_usage: Memory usage of DataFrame columns.""")
+    DataFrame.memory_usage: Memory usage of DataFrame columns."""
+)
 
 
 frame_sub_kwargs = {
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/formats/info.py#L151'>pandas/io/formats/info.py~L151</a>
```diff
 }
 
 
-series_examples_sub = dedent("""\
+series_examples_sub = dedent(
+    """\
     >>> int_values = [1, 2, 3, 4, 5]
     >>> text_values = ['alpha', 'beta', 'gamma', 'delta', 'epsilon']
     >>> s = pd.Series(text_values, index=int_values)
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/formats/info.py#L208'>pandas/io/formats/info.py~L208</a>
```diff
     --------------    -----
     1000000 non-null  object
     dtypes: object(1)
-    memory usage: 55.3 MB""")
+    memory usage: 55.3 MB"""
+)
 
 
-series_see_also_sub = dedent("""\
+series_see_also_sub = dedent(
+    """\
     Series.describe: Generate descriptive statistics of Series.
-    Series.memory_usage: Memory usage of Series.""")
+    Series.memory_usage: Memory usage of Series."""
+)
 
 
 series_sub_kwargs = {
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/formats/style_render.py#L1785'>pandas/io/formats/style_render.py~L1785</a>
```diff
         elif escape == "latex-math":
             return _escape_latex_math(x)
         else:
-            raise ValueError(f"`escape` only permitted in {{'html', 'latex', 'latex-math'}}, \
-got {escape}")
+            raise ValueError(
+                f"`escape` only permitted in {{'html', 'latex', 'latex-math'}}, \
+got {escape}"
+            )
     return x
 
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/io/stata.py#L2671'>pandas/io/stata.py~L2671</a>
```diff
                 inferred_dtype = infer_dtype(column, skipna=True)
                 if not ((inferred_dtype == "string") or len(column) == 0):
                     col = column.name
-                    raise ValueError(f"""\
+                    raise ValueError(
+                        f"""\
 Column `{col}` cannot be exported.\n\nOnly string-like object arrays
 containing all strings or a mix of strings and None can be exported.
 Object arrays containing only null values are prohibited. Other object
 types cannot be exported and must first be converted to one of the
-supported types.""")
+supported types."""
+                    )
                 encoded = self.data[col].str.encode(self._encoding)
                 # If larger than _max_string_length do nothing
                 if (
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/style/test_html.py#L241'>pandas/tests/io/formats/style/test_html.py~L241</a>
```diff
 def test_from_custom_template_table(tmpdir):
     p = tmpdir.mkdir("tpl").join("myhtml_table.tpl")
     p.write(
-        dedent("""\
+        dedent(
+            """\
             {% extends "html_table.tpl" %}
             {% block table %}
             <h1>{{custom_title}}</h1>
             {{ super() }}
-            {% endblock table %}""")
+            {% endblock table %}"""
+        )
     )
     result = Styler.from_custom_template(str(tmpdir.join("tpl")), "myhtml_table.tpl")
     assert issubclass(result, Styler)
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/style/test_html.py#L259'>pandas/tests/io/formats/style/test_html.py~L259</a>
```diff
 def test_from_custom_template_style(tmpdir):
     p = tmpdir.mkdir("tpl").join("myhtml_style.tpl")
     p.write(
-        dedent("""\
+        dedent(
+            """\
             {% extends "html_style.tpl" %}
             {% block style %}
             <link rel="stylesheet" href="mystyle.css">
             {{ super() }}
-            {% endblock style %}""")
+            {% endblock style %}"""
+        )
     )
     result = Styler.from_custom_template(
         str(tmpdir.join("tpl")), html_style="myhtml_style.tpl"
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L42'>pandas/tests/io/formats/test_to_string.py~L42</a>
```diff
             "b": Series([1, 2], dtype="Int64"),
         })
         result = df.to_string(formatters=["{:.2f}".format, "{:.2f}".format])
-        expected = dedent("""\
+        expected = dedent(
+            """\
                   a     b
             0  0.12  1.00
-            1  1.12  2.00""")
+            1  1.12  2.00"""
+        )
         assert result == expected
 
     def test_to_string_with_formatters(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L81'>pandas/tests/io/formats/test_to_string.py~L81</a>
```diff
             return x.strftime("%Y-%m")
 
         result = x.to_string(formatters={"months": format_func})
-        expected = dedent("""\
+        expected = dedent(
+            """\
             months
             0 2016-01
-            1 2016-02""")
+            1 2016-02"""
+        )
         assert result.strip() == expected
 
     def test_to_string_with_datetime64_hourformatter(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L96'>pandas/tests/io/formats/test_to_string.py~L96</a>
```diff
             return x.strftime("%H:%M")
 
         result = x.to_string(formatters={"hod": format_func})
-        expected = dedent("""\
+        expected = dedent(
+            """\
             hod
             0 10:10
-            1 12:12""")
+            1 12:12"""
+        )
         assert result.strip() == expected
 
     def test_to_string_with_formatters_unicode(self):
         df = DataFrame({"c/\u03c3": [1, 2, 3]})
         result = df.to_string(formatters={"c/\u03c3": str})
-        expected = dedent("""\
+        expected = dedent(
+            """\
               c/\u03c3
             0   1
             1   2
-            2   3""")
+            2   3"""
+        )
         assert result == expected
 
         def test_to_string_index_formatter(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L714'>pandas/tests/io/formats/test_to_string.py~L714</a>
```diff
         # GH#13828
         df = DataFrame([["A", 1.2225], ["A", None]], columns=["Group", "Data"])
         result = df.to_string(na_rep=na_rep, float_format="{:.2f}".format)
-        expected = dedent(f"""\
+        expected = dedent(
+            f"""\
                Group  Data
              0     A  1.22
-             1     A   {na_rep}""")
+             1     A   {na_rep}"""
+        )
         assert result == expected
 
     def test_to_string_string_dtype(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L734'>pandas/tests/io/formats/test_to_string.py~L734</a>
```diff
             "z": "int64[pyarrow]",
         })
         result = df.dtypes.to_string()
-        expected = dedent("""\
+        expected = dedent(
+            """\
             x    string[pyarrow]
             y     string[python]
-            z     int64[pyarrow]""")
+            z     int64[pyarrow]"""
+        )
         assert result == expected
 
     def test_to_string_pos_args_deprecation(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L1106'>pandas/tests/io/formats/test_to_string.py~L1106</a>
```diff
     def test_to_string_complex_number_trims_zeros(self):
         ser = Series([1.000000 + 1.000000j, 1.0 + 1.0j, 1.05 + 1.0j])
         result = ser.to_string()
-        expected = dedent("""\
+        expected = dedent(
+            """\
             0    1.00+1.00j
             1    1.00+1.00j
-            2    1.05+1.00j""")
+            2    1.05+1.00j"""
+        )
         assert result == expected
 
     def test_nullable_float_to_string(self, float_ea_dtype):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L1117'>pandas/tests/io/formats/test_to_string.py~L1117</a>
```diff
         dtype = float_ea_dtype
         ser = Series([0.0, 1.0, None], dtype=dtype)
         result = ser.to_string()
-        expected = dedent("""\
+        expected = dedent(
+            """\
             0     0.0
             1     1.0
-            2    <NA>""")
+            2    <NA>"""
+        )
         assert result == expected
 
     def test_nullable_int_to_string(self, any_int_ea_dtype):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/formats/test_to_string.py#L1128'>pandas/tests/io/formats/test_to_string.py~L1128</a>
```diff
         dtype = any_int_ea_dtype
         ser = Series([0, 1, None], dtype=dtype)
         result = ser.to_string()
-        expected = dedent("""\
+        expected = dedent(
+            """\
             0       0
             1       1
-            2    <NA>""")
+            2    <NA>"""
+        )
         assert result == expected
 
     def test_to_string_mixed(self):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/parser/test_parse_dates.py#L52'>pandas/tests/io/parser/test_parse_dates.py~L52</a>
```diff
         time = time.astype(int)  # convert float seconds to int type
         return pd.to_timedelta(time, unit="s")
 
-    testdata = StringIO("""time e n h
+    testdata = StringIO(
+        """time e n h
         41047.00 -98573.7297 871458.0640 389.0089
         41048.00 -98573.7299 871458.0640 389.0089
         41049.00 -98573.7300 871458.0642 389.0088
         41050.00 -98573.7299 871458.0643 389.0088
         41051.00 -98573.7302 871458.0640 389.0086
-        """)
+        """
+    )
     result = all_parsers.read_csv_check_warnings(
         FutureWarning,
         "Please use 'date_format' instead",
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/parser/test_parse_dates.py#L90'>pandas/tests/io/parser/test_parse_dates.py~L90</a>
```diff
         time = time.astype(int)  # convert float seconds to int type
         return pd.to_timedelta(time, unit="s")
 
-    testdata = StringIO("""time e
+    testdata = StringIO(
+        """time e
         41047.00 -93.77
         41048.00 -95.79
         41049.00 -98.73
         41050.00 -93.99
         41051.00 -97.72
-        """)
+        """
+    )
     result = all_parsers.read_csv_check_warnings(
         FutureWarning,
         "Please use 'date_format' instead",
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/pytables/test_append.py#L747'>pandas/tests/io/pytables/test_append.py~L747</a>
```diff
         )
         df["invalid"] = [["a"]] * len(df)
         assert df.dtypes["invalid"] == np.object_
-        msg = re.escape("""Cannot serialize the column [invalid]
-because its data contents are not [string] but [mixed] object dtype""")
+        msg = re.escape(
+            """Cannot serialize the column [invalid]
+because its data contents are not [string] but [mixed] object dtype"""
+        )
         with pytest.raises(TypeError, match=msg):
             store.append("df", df)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L119'>pandas/tests/io/test_html.py~L119</a>
```diff
         )
 
         with tm.assert_produces_warning(FutureWarning, match=msg):
-            flavor_read_html("""<table>
+            flavor_read_html(
+                """<table>
                 <thead>
                     <tr>
                         <th>A</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L138'>pandas/tests/io/test_html.py~L138</a>
```diff
                         <td>4</td>
                     </tr>
                 </tbody>
-            </table>""")
+            </table>"""
+            )
 
     @pytest.fixture
     def spam_data(self, datapath):
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L583'>pandas/tests/io/test_html.py~L583</a>
```diff
         # GH-20690
         # Read all tbody tags within a single table.
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
             <thead>
                 <tr>
                     <th>A</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L602'>pandas/tests/io/test_html.py~L602</a>
```diff
                     <td>4</td>
                 </tr>
             </tbody>
-        </table>""")
+        </table>"""
+            )
         )[0]
 
         expected = DataFrame(data=[[1, 2], [3, 4]], columns=["A", "B"])
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L615'>pandas/tests/io/test_html.py~L615</a>
```diff
         as described in issue #9178
         """
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
                 <thead>
                     <tr>
                         <th>Header</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L626'>pandas/tests/io/test_html.py~L626</a>
```diff
                         <td>first</td>
                     </tr>
                 </tbody>
-            </table>""")
+            </table>"""
+            )
         )[0]
 
         expected = DataFrame(data={"Header": "first"}, index=[0])
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L638'>pandas/tests/io/test_html.py~L638</a>
```diff
         Ensure parser adds <tr> within <thead> on malformed HTML.
         """
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
             <thead>
                 <tr>
                     <th>Country</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L653'>pandas/tests/io/test_html.py~L653</a>
```diff
                     <td>1944</td>
                 </tr>
             </tbody>
-        </table>""")
+        </table>"""
+            )
         )[0]
 
         expected = DataFrame(
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L786'>pandas/tests/io/test_html.py~L786</a>
```diff
 
     def test_different_number_of_cols(self, flavor_read_html):
         expected = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
                         <thead>
                             <tr style="text-align: right;">
                             <th></th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L815'>pandas/tests/io/test_html.py~L815</a>
```diff
                             <td> 0.222</td>
                             </tr>
                         </tbody>
-                    </table>"""),
+                    </table>"""
+            ),
             index_col=0,
         )[0]
 
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
                     <thead>
                         <tr style="text-align: right;">
                         <th></th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L846'>pandas/tests/io/test_html.py~L846</a>
```diff
                         <td> 0.222</td>
                         </tr>
                     </tbody>
-                 </table>"""),
+                 </table>"""
+            ),
             index_col=0,
         )[0]
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1075'>pandas/tests/io/test_html.py~L1075</a>
```diff
     def test_decimal_rows(self, flavor_read_html):
         # GH 12907
         result = flavor_read_html(
-            StringIO("""<html>
+            StringIO(
+                """<html>
             <body>
              <table>
                 <thead>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1090'>pandas/tests/io/test_html.py~L1090</a>
```diff
                 </tbody>
             </table>
             </body>
-        </html>"""),
+        </html>"""
+            ),
             decimal="#",
         )[0]
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1113'>pandas/tests/io/test_html.py~L1113</a>
```diff
     def test_converters(self, flavor_read_html):
         # GH 13461
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
                  <thead>
                    <tr>
                      <th>a</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1127'>pandas/tests/io/test_html.py~L1127</a>
```diff
                      <td> 0.244</td>
                    </tr>
                  </tbody>
-               </table>"""),
+               </table>"""
+            ),
             converters={"a": str},
         )[0]
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1138'>pandas/tests/io/test_html.py~L1138</a>
```diff
     def test_na_values(self, flavor_read_html):
         # GH 13461
         result = flavor_read_html(
-            StringIO("""<table>
+            StringIO(
+                """<table>
                  <thead>
                    <tr>
                      <th>a</th>
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1152'>pandas/tests/io/test_html.py~L1152</a>
```diff
                      <td> 0.244</td>
                    </tr>
                  </tbody>
-               </table>"""),
+               </table>"""
+            ),
             na_values=[0.244],
         )[0]
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/tests/io/test_html.py#L1384'>pandas/tests/io/test_html.py~L1384</a>
```diff
             def seekable(self):
                 return False
 
-        bad = UnseekableStringIO("""
-            <table><tr><td>spam<foobr />eggs</td></tr></table>""")
+        bad = UnseekableStringIO(
+            """
+            <table><tr><td>spam<foobr />eggs</td></tr></table>"""
+        )
 
         assert flavor_read_html(bad)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/pandas/util/_decorators.py#L83'>pandas/util/_decorators.py~L83</a>
```diff
         empty1, summary, empty2, doc_string = alternative.__doc__.split("\n", 3)
         if empty1 or empty2 and not summary:
             raise AssertionError(doc_error_msg)
-        wrapper.__doc__ = dedent(f"""
+        wrapper.__doc__ = dedent(
+            f"""
         {summary.strip()}
 
         .. deprecated:: {version}
             {msg}
 
-        {dedent(doc_string)}""")
+        {dedent(doc_string)}"""
+        )
     # error: Incompatible return value type (got "Callable[[VarArg(Any), KwArg(Any)],
     # Callable[...,Any]]", expected "Callable[[F], F]")
     return wrapper  # type: ignore[return-value]
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/scripts/validate_rst_title_capitalization.py#L265'>scripts/validate_rst_title_capitalization.py~L265</a>
```diff
     for filename in source_paths:
         for title, line_number in find_titles(filename):
             if title != correct_title_capitalization(title):
-                print(f"""{filename}:{line_number}:{err_msg} "{title}" to "{
-                    correct_title_capitalization(title)}" """)
+                print(
+                    f"""{filename}:{line_number}:{err_msg} "{title}" to "{
+                    correct_title_capitalization(title)}" """
+                )
                 number_of_errors += 1
 
     return number_of_errors
```
<a href='https://github.com/pandas-dev/pandas/blob/4ac340e89a2a1a2652530edc74968b821516de05/setup.py#L259'>setup.py~L259</a>
```diff
             for src in ext.sources:
                 if not os.path.exists(src):
                     print(f"{ext.name}: -> [{ext.sources}]")
-                    raise Exception(f"""Cython-generated file '{src}' not found.
+                    raise Exception(
+                        f"""Cython-generated file '{src}' not found.
                 Cython is required to compile pandas from a development branch.
                 Please install Cython or download a release package of pandas.
-                """)
+                """
+                    )
 
     def build_extensions(self) -> None:
         self.check_cython_extensions(self.extensions)
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+20 -10 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/blocks/core.py#L576'>src/prefect/blocks/core.py~L576</a>
```diff
         class_name = cls.__name__
         block_variable_name = f'{cls.get_block_type_slug().replace("-", "_")}_block'
 
-        return dedent(f"""\
+        return dedent(
+            f"""\
         ```python
         from {module_str} import {class_name}
 
         {block_variable_name} = {class_name}.load("BLOCK_NAME")
-        ```""")
+        ```"""
+        )
 
     @classmethod
     def _to_block_type(cls) -> BlockType:
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/cloud_run.py#L390'>src/prefect/infrastructure/provisioners/cloud_run.py~L390</a>
```diff
                     """),
                 Panel(
                     Syntax(
-                        dedent(f"""\
+                        dedent(
+                            f"""\
                         from prefect import flow
                         from prefect.deployments import DeploymentImage
 
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/cloud_run.py#L408'>src/prefect/infrastructure/provisioners/cloud_run.py~L408</a>
```diff
                                     name="my-image:latest",
                                     platform="linux/amd64",
                                 )
-                            )"""),
+                            )"""
+                        ),
                         "python",
                         background_color="default",
                     ),
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/container_instance.py#L1031'>src/prefect/infrastructure/provisioners/container_instance.py~L1031</a>
```diff
                     """),
             Panel(
                 Syntax(
-                    dedent(f"""\
+                    dedent(
+                        f"""\
                         from prefect import flow
                         from prefect.deployments import DeploymentImage
 
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/container_instance.py#L1049'>src/prefect/infrastructure/provisioners/container_instance.py~L1049</a>
```diff
                                     name="my-image:latest",
                                     platform="linux/amd64",
                                 )
-                            )"""),
+                            )"""
+                    ),
                     "python",
                     background_color="default",
                 ),
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/ecs.py#L937'>src/prefect/infrastructure/provisioners/ecs.py~L937</a>
```diff
                     """),
             Panel(
                 Syntax(
-                    dedent(f"""\
+                    dedent(
+                        f"""\
                                 from prefect import flow
                                 from prefect.deployments import DeploymentImage
 
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/src/prefect/infrastructure/provisioners/ecs.py#L955'>src/prefect/infrastructure/provisioners/ecs.py~L955</a>
```diff
                                             name="{self._repository_name}:latest",
                                             platform="linux/amd64",
                                         )
-                                    )"""),
+                                    )"""
+                    ),
                     "python",
                     background_color="default",
                 ),
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/tests/infrastructure/provisioners/test_ecs.py#L1364'>tests/infrastructure/provisioners/test_ecs.py~L1364</a>
```diff
 
             To build and push a Docker image to your newly created repository, use [blue]'prefect-flows'[/] as your image name:
             """)
-        assert container_repository_resource.next_steps[1].renderable.code == dedent("""\
+        assert container_repository_resource.next_steps[1].renderable.code == dedent(
+            """\
                 from prefect import flow
                 from prefect.deployments import DeploymentImage
 
```
<a href='https://github.com/prefecthq/prefect/blob/41e44ed5959caf884a439f6b6fc3085a69dd1a4e/tests/infrastructure/provisioners/test_ecs.py#L1382'>tests/infrastructure/provisioners/test_ecs.py~L1382</a>
```diff
                             name="prefect-flows:latest",
                             platform="linux/amd64",
                         )
-                    )""")
+                    )"""
+        )
 
     @pytest.mark.usefixtures("existing_ecr_repository")
     async def test_no_next_steps_when_no_provision(self, container_repository_resource):
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+20 -10 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/conftest.py#L849'>tests/conftest.py~L849</a>
```diff
     )
     # Output won't be nicely indented because dedent() acts after f-string
     # arg insertion.
-    index_html = dedent(f"""\
+    index_html = dedent(
+        f"""\
         <!DOCTYPE html>
         <html>
           <head>
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/conftest.py#L859'>tests/conftest.py~L859</a>
```diff
           <body>
           {pkg_links}
           </body>
-        </html>""")
+        </html>"""
+    )
     # (2) Generate the index.html in a new subdirectory of the temp directory.
     (html_dir / "index.html").write_text(index_html)
 
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/conftest.py#L890'>tests/conftest.py~L890</a>
```diff
         # write an index.html with the generated download links for each
         # copied file for this specific package name.
         download_links_str = "\n".join(download_links)
-        pkg_index_content = dedent(f"""\
+        pkg_index_content = dedent(
+            f"""\
             <!DOCTYPE html>
             <html>
               <head>
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/conftest.py#L901'>tests/conftest.py~L901</a>
```diff
                 <h1>Links for {pkg}</h1>
                 {download_links_str}
               </body>
-            </html>""")
+            </html>"""
+        )
         with open(pkg_subdir / "index.html", "w") as f:
             f.write(pkg_index_content)
 
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/functional/test_install.py#L1036'>tests/functional/test_install.py~L1036</a>
```diff
     result.did_create(dist_info_folder)
 
 
-mock100_setup_py = textwrap.dedent("""\
+mock100_setup_py = textwrap.dedent(
+    """\
                         from setuptools import setup
                         setup(name='mock',
-                              version='100.1')""")
+                              version='100.1')"""
+)
 
 
 def test_install_folder_using_dot_slash(script: PipTestEnvironment) -> None:
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/functional/test_install_reqs.py#L319'>tests/functional/test_install_reqs.py~L319</a>
```diff
     script.scratch_path.joinpath("bin").mkdir()
     with open(user_cfg, "w") as cfg:
         cfg.write(
-            textwrap.dedent(f"""
+            textwrap.dedent(
+                f"""
             [install]
-            prefix={script.scratch_path}""")
+            prefix={script.scratch_path}"""
+            )
         )
 
     result = script.pip(
```
<a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/tests/unit/test_collector.py#L983'>tests/unit/test_collector.py~L983</a>
```diff
         # Check that index URLs are marked as *un*cacheable.
         assert not pages[0].link.cache_link_parsing
 
-        expected_message = dedent("""\
+        expected_message = dedent(
+            """\
         1 location(s) to search for versions of twine:
-        * https://pypi.org/simple/twine/""")
+        * https://pypi.org/simple/twine/"""
+        )
         assert caplog.record_tuples == [
             ("pip._internal.index.collector", logging.DEBUG, expected_message),
         ]
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+36 -18 lines across 6 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/pkg_resources/tests/test_resources.py#L339'>pkg_resources/tests/test_resources.py~L339</a>
```diff
         reason="https://github.com/python/cpython/issues/103632",
     )
     def testDistroDependsOptions(self):
-        d = self.distRequires("""
+        d = self.distRequires(
+            """
             Twisted>=1.5
             [docgen]
             ZConfig>=2.0
             docutils>=0.3
             [fastcgi]
-            fcgiapp>=0.1""")
+            fcgiapp>=0.1"""
+        )
         self.checkRequires(d, "Twisted>=1.5")
         self.checkRequires(
             d, "Twisted>=1.5 ZConfig>=2.0 docutils>=0.3".split(), ["docgen"]
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/_distutils/msvc9compiler.py#L157'>setuptools/_distutils/msvc9compiler.py~L157</a>
```diff
             else:
                 raise KeyError("sdkinstallrootv2.0")
         except KeyError:
-            raise DistutilsPlatformError("""Python was built with Visual Studio 2008;
+            raise DistutilsPlatformError(
+                """Python was built with Visual Studio 2008;
 extensions must be built with a compiler than can generate compatible binaries.
 Visual Studio 2008 was not found on this system. If you have Cygwin installed,
-you can try compiling with MingW32, by passing "-c mingw32" to setup.py.""")
+you can try compiling with MingW32, by passing "-c mingw32" to setup.py."""
+            )
 
         if version >= 9.0:
             self.set_macro("FrameworkVersion", self.vsbase, "clr version")
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/_distutils/msvccompiler.py#L145'>setuptools/_distutils/msvccompiler.py~L145</a>
```diff
             else:
                 self.set_macro("FrameworkSDKDir", net, "sdkinstallroot")
         except KeyError:
-            raise DistutilsPlatformError("""Python was built with Visual Studio 2003;
+            raise DistutilsPlatformError(
+                """Python was built with Visual Studio 2003;
 extensions must be built with a compiler than can generate compatible binaries.
 Visual Studio 2003 was not found on this system. If you have Cygwin installed,
-you can try compiling with MingW32, by passing "-c mingw32" to setup.py.""")
+you can try compiling with MingW32, by passing "-c mingw32" to setup.py."""
+            )
 
         p = r"Software\Microsoft\NET Framework Setup\Product"
         for base in HKEYS:
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/_distutils/tests/test_dist.py#L429'>setuptools/_distutils/tests/test_dist.py~L429</a>
```diff
         assert "Metadata-Version: 1.1" in meta
 
     def test_long_description(self):
-        long_desc = textwrap.dedent("""\
+        long_desc = textwrap.dedent(
+            """\
         example::
               We start here
             and continue here
-          and end here.""")
+          and end here."""
+        )
         attrs = {"name": "package", "version": "1.0", "long_description": long_desc}
 
         dist = Distribution(attrs)
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/tests/test_build_meta.py#L545'>setuptools/tests/test_build_meta.py~L545</a>
```diff
 
     def test_build_sdist_pyproject_toml_exists(self, tmpdir_cwd):
         files = {
-            "setup.py": DALS("""
+            "setup.py": DALS(
+                """
                 __import__('setuptools').setup(
                     name='foo',
                     version='0.0.0',
                     py_modules=['hello']
-                )"""),
+                )"""
+            ),
             "hello.py": "",
             "pyproject.toml": DALS("""
                 [build-system]
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/tests/test_build_meta.py#L577'>setuptools/tests/test_build_meta.py~L577</a>
```diff
     def test_build_sdist_setup_py_manifest_excluded(self, tmpdir_cwd):
         # Ensure that MANIFEST.in can exclude setup.py
         files = {
-            "setup.py": DALS("""
+            "setup.py": DALS(
+                """
         __import__('setuptools').setup(
             name='foo',
             version='0.0.0',
             py_modules=['hello']
-        )"""),
+        )"""
+            ),
             "hello.py": "",
             "MANIFEST.in": DALS("""
         exclude setup.py
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/tests/test_build_meta.py#L598'>setuptools/tests/test_build_meta.py~L598</a>
```diff
 
     def test_build_sdist_builds_targz_even_if_zip_indicated(self, tmpdir_cwd):
         files = {
-            "setup.py": DALS("""
+            "setup.py": DALS(
+                """
                 __import__('setuptools').setup(
                     name='foo',
                     version='0.0.0',
                     py_modules=['hello']
-                )"""),
+                )"""
+            ),
             "hello.py": "",
             "setup.cfg": DALS("""
                 [sdist]
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/tests/test_build_meta.py#L617'>setuptools/tests/test_build_meta.py~L617</a>
```diff
         build_backend.build_sdist("temp")
 
     _relative_path_import_files = {
-        "setup.py": DALS("""
+        "setup.py": DALS(
+            """
             __import__('setuptools').setup(
                 name='foo',
                 version=__import__('hello').__version__,
                 py_modules=['hello']
-            )"""),
+            )"""
+        ),
         "hello.py": '__version__ = "0.0.0"',
         "setup.cfg": DALS("""
             [sdist]
```
<a href='https://github.com/pypa/setuptools/blob/e92440ad6210b21f3ede64d7eb69dead94b37d3a/setuptools/tests/test_egg_info.py#L1082'>setuptools/tests/test_egg_info.py~L1082</a>
```diff
         # `Project-URL` is described at https://packaging.python.org
         #     /specifications/core-metadata/#project-url-multiple-use
 
-        self._setup_script_with_requires("""project_urls={
+        self._setup_script_with_requires(
+            """project_urls={
                 'Link One': 'https://example.com/one/',
                 'Link Two': 'https://example.com/two/',
-                },""")
+                },"""
+        )
         environ = os.environ.copy().update(
             HOME=env.paths["home"],
         )
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/mypy/blob/1dd8e7fe654991b01bd80ef7f1f675d9e3910c3a/mypy/build.py#L1198'>mypy/build.py~L1198</a>
```diff
     cachedir_tag = os.path.join(target_dir, "CACHEDIR.TAG")
     try:
         with open(cachedir_tag, "x") as f:
-            f.write("""Signature: 8a477f597d28d172789f06886806bc55
+            f.write(
+                """Signature: 8a477f597d28d172789f06886806bc55
 # This file is a cache directory tag automatically created by mypy.
 # For information about cache directory tags see https://bford.info/cachedir/
-""")
+"""
+            )
     except FileExistsError:
         pass
 
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+8 -4 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/42bcea76312341f542e89da1434f16c84e617ac4/scripts/pyi_generator.py#L124'>scripts/pyi_generator.py~L124</a>
```diff
             names=[ast.alias(name=imp) for imp in sorted(typing_imports)],
         ),
         *ast.parse(  # type: ignore
-            textwrap.dedent("""
+            textwrap.dedent(
+                """
                 from reflex.vars import Var, BaseVar, ComputedVar
                 from reflex.event import EventChain, EventHandler, EventSpec
-                from reflex.style import Style""")
+                from reflex.style import Style"""
+            )
         ).body,
         # *[
         #     ast.ImportFrom(module=name, names=[ast.alias(name=val) for val in values])
```
<a href='https://github.com/reflex-dev/reflex/blob/42bcea76312341f542e89da1434f16c84e617ac4/tests/utils/test_utils.py#L291'>tests/utils/test_utils.py~L291</a>
```diff
 
     if gitignore_exists:
         gitignore_file.touch()
-        gitignore_file.write_text("""*.db
+        gitignore_file.write_text(
+            """*.db
         __pycache__/
-        """)
+        """
+        )
 
     prerequisites.initialize_gitignore()
 
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+136 -68 lines across 11 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v26_v27.py#L35'>rotkehlchen/db/upgrades/v26_v27.py~L35</a>
```diff
     progress_handler.new_step()
 
     cursor.execute("DROP TABLE IF EXISTS amm_swaps;")
-    cursor.execute("""
+    cursor.execute(
+        """
 CREATE TABLE IF NOT EXISTS amm_swaps (
     tx_hash VARCHAR[42] NOT NULL,
     log_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v26_v27.py#L53'>rotkehlchen/db/upgrades/v26_v27.py~L53</a>
```diff
     FOREIGN KEY(token0_identifier) REFERENCES assets(identifier) ON UPDATE CASCADE,
     FOREIGN KEY(token1_identifier) REFERENCES assets(identifier) ON UPDATE CASCADE,
     PRIMARY KEY (tx_hash, log_index)
-);""")
+);"""
+    )
     cursor.execute('DELETE FROM used_query_ranges WHERE name LIKE "balancer_trades%";')
     cursor.execute('DELETE FROM used_query_ranges WHERE name LIKE "uniswap_trades%";')
     progress_handler.new_step()
 
     cursor.execute("DROP TABLE IF EXISTS uniswap_events;")
-    cursor.execute("""
+    cursor.execute(
+        """
 CREATE TABLE IF NOT EXISTS uniswap_events (
     tx_hash VARCHAR[42] NOT NULL,
     log_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v26_v27.py#L76'>rotkehlchen/db/upgrades/v26_v27.py~L76</a>
```diff
     FOREIGN KEY(token0_identifier) REFERENCES assets(identifier) ON UPDATE CASCADE,
     FOREIGN KEY(token1_identifier) REFERENCES assets(identifier) ON UPDATE CASCADE,
     PRIMARY KEY (tx_hash, log_index)
-);""")
+);"""
+    )
     cursor.execute('DELETE FROM used_query_ranges WHERE name LIKE "uniswap_events%";')
     progress_handler.new_step()
 
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v27_v28.py#L24'>rotkehlchen/db/upgrades/v27_v28.py~L24</a>
```diff
         cursor.execute("DELETE FROM aave_events;")
         cursor.execute('DELETE FROM used_query_ranges WHERE name LIKE "aave_events%";')
         # Create the gitcoin tables that are added in this DB version
-        cursor.execute("""
+        cursor.execute(
+            """
         CREATE TABLE IF NOT EXISTS gitcoin_tx_type (
         type    CHAR(1)       PRIMARY KEY NOT NULL,
         seq     INTEGER UNIQUE
-        )""")
+        )"""
+        )
         # Ethereum Transaction
         cursor.execute('INSERT OR IGNORE INTO gitcoin_tx_type(type, seq) VALUES ("A", 1)')
         # ZKSync Transaction
         cursor.execute('INSERT OR IGNORE INTO gitcoin_tx_type(type, seq) VALUES ("B", 2)')
 
-        cursor.execute("""CREATE TABLE IF NOT EXISTS ledger_actions_gitcoin_data (
+        cursor.execute(
+            """CREATE TABLE IF NOT EXISTS ledger_actions_gitcoin_data (
         parent_id INTEGER NOT NULL,
         tx_id TEXT NOT NULL UNIQUE,
         grant_id INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v27_v28.py#L42'>rotkehlchen/db/upgrades/v27_v28.py~L42</a>
```diff
         tx_type NOT NULL DEFAULT('A') REFERENCES gitcoin_tx_type(type),
         FOREIGN KEY(parent_id) REFERENCES ledger_actions(identifier) ON DELETE CASCADE ON UPDATE CASCADE,
         PRIMARY KEY(parent_id, tx_id, grant_id)
-        )""")  # noqa: E501
+        )"""
+        )  # noqa: E501
 
-        cursor.execute("""CREATE TABLE IF NOT EXISTS gitcoin_grant_metadata (
+        cursor.execute(
+            """CREATE TABLE IF NOT EXISTS gitcoin_grant_metadata (
         grant_id INTEGER NOT NULL PRIMARY KEY,
         grant_name TEXT NOT NULL,
         created_on INTEGER NOT NULL
-        )""")
+        )"""
+        )
         progress_handler.new_step()
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v28_v29.py#L12'>rotkehlchen/db/upgrades/v28_v29.py~L12</a>
```diff
     Should be called at the end of the upgrade as it depends on the changes
     done to the transactions table
     """
-    cursor.execute("""
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS ethtx_receipts (
     tx_hash BLOB NOT NULL PRIMARY KEY,
     contract_address TEXT, /* can be null */
     status INTEGER NOT NULL CHECK (status IN (0, 1)),
     type INTEGER NOT NULL,
     FOREIGN KEY(tx_hash) REFERENCES ethereum_transactions(tx_hash) ON DELETE CASCADE ON UPDATE CASCADE
-    );""")  # noqa: E501
-    cursor.execute("""
+    );"""
+    )  # noqa: E501
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS ethtx_receipt_logs (
     tx_hash BLOB NOT NULL,
     log_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v28_v29.py#L29'>rotkehlchen/db/upgrades/v28_v29.py~L29</a>
```diff
     removed INTEGER NOT NULL CHECK (removed IN (0, 1)),
     FOREIGN KEY(tx_hash) REFERENCES ethtx_receipts(tx_hash) ON DELETE CASCADE ON UPDATE CASCADE,
     PRIMARY KEY(tx_hash, log_index)
-    );""")
-    cursor.execute("""
+    );"""
+    )
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS ethtx_receipt_log_topics (
     tx_hash BLOB NOT NULL,
     log_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v28_v29.py#L38'>rotkehlchen/db/upgrades/v28_v29.py~L38</a>
```diff
     topic_index INTEGER NOT NULL,
     FOREIGN KEY(tx_hash, log_index) REFERENCES ethtx_receipt_logs(tx_hash, log_index) ON DELETE CASCADE ON UPDATE CASCADE,
     PRIMARY KEY(tx_hash, log_index, topic_index)
-    );""")  # noqa: E501
-    cursor.execute("""CREATE TABLE IF NOT EXISTS nfts (
+    );"""
+    )  # noqa: E501
+    cursor.execute(
+        """CREATE TABLE IF NOT EXISTS nfts (
     identifier TEXT NOT NULL PRIMARY KEY,
     name TEXT,
     last_price TEXT,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v28_v29.py#L50'>rotkehlchen/db/upgrades/v28_v29.py~L50</a>
```diff
     FOREIGN KEY(blockchain, owner_address) REFERENCES blockchain_accounts(blockchain, account) ON DELETE CASCADE,
     FOREIGN KEY (identifier) REFERENCES assets(identifier) ON UPDATE CASCADE,
     FOREIGN KEY (last_price_asset) REFERENCES assets(identifier) ON UPDATE CASCADE
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
 
 
 def _upgrade_existing_tables(
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v30_v31.py#L31'>rotkehlchen/db/upgrades/v30_v31.py~L31</a>
```diff
     # Add all new tables
     cursor.execute("DROP TABLE IF EXISTS eth2_deposits;")
     cursor.execute("DROP TABLE IF EXISTS eth2_daily_staking_details;")
-    cursor.execute("""
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS eth2_validators (
     validator_index INTEGER NOT NULL PRIMARY KEY,
     public_key TEXT NOT NULL UNIQUE,
     ownership_proportion TEXT NOT NULL
-    );""")
-    cursor.execute("""
+    );"""
+    )
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS eth2_deposits (
     tx_hash BLOB NOT NULL,
     tx_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v30_v31.py#L49'>rotkehlchen/db/upgrades/v30_v31.py~L49</a>
```diff
     usd_value TEXT NOT NULL,
     FOREIGN KEY(pubkey) REFERENCES eth2_validators(public_key) ON UPDATE CASCADE ON DELETE CASCADE,
     PRIMARY KEY(tx_hash, pubkey, amount) /* multiple deposits can exist for same pubkey */
-    );""")
-    cursor.execute("""
+    );"""
+    )
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS eth2_daily_staking_details (
     validator_index INTEGER NOT NULL,
     timestamp integer NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v30_v31.py#L69'>rotkehlchen/db/upgrades/v30_v31.py~L69</a>
```diff
     deposits_number INTEGER,
     amount_deposited TEXT,
     FOREIGN KEY(validator_index) REFERENCES eth2_validators(validator_index) ON UPDATE CASCADE ON DELETE CASCADE,
-    PRIMARY KEY (validator_index, timestamp));""")  # noqa: E501
+    PRIMARY KEY (validator_index, timestamp));"""
+    )  # noqa: E501
     progress_handler.new_step()
-    cursor.execute("""
+    cursor.execute(
+        """
 CREATE VIEW IF NOT EXISTS combined_trades_view AS
     WITH amounts_query AS (
         SELECT
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v30_v31.py#L168'>rotkehlchen/db/upgrades/v30_v31.py~L168</a>
```diff
 FROM SWAPS
 UNION ALL /* using union all as there can be no duplicates so no need to handle them */
 SELECT * from trades
-;""")  # noqa: E501
-    cursor.execute("""CREATE TABLE IF NOT EXISTS history_events (
+;"""
+    )  # noqa: E501
+    cursor.execute(
+        """CREATE TABLE IF NOT EXISTS history_events (
     identifier TEXT NOT NULL PRIMARY KEY,
     event_identifier TEXT NOT NULL,
     sequence_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v30_v31.py#L182'>rotkehlchen/db/upgrades/v30_v31.py~L182</a>
```diff
     notes TEXT,
     type TEXT NOT NULL,
     subtype TEXT
-    );""")
+    );"""
+    )
     progress_handler.new_step()
 
 
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v31_v32.py#L11'>rotkehlchen/db/upgrades/v31_v32.py~L11</a>
```diff
 
 
 def _upgrade_history_events(cursor: "DBCursor") -> None:
-    cursor.execute("""
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS history_events_copy (
         identifier INTEGER NOT NULL PRIMARY KEY,
         event_identifier TEXT NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v31_v32.py#L28'>rotkehlchen/db/upgrades/v31_v32.py~L28</a>
```diff
         counterparty TEXT,
         extra_data TEXT,
         UNIQUE(event_identifier, sequence_index)
-    );""")
+    );"""
+    )
     cursor.execute("UPDATE history_events SET timestamp = timestamp / 10;")
     cursor.execute(
         'UPDATE history_events SET subtype = "deposit asset" WHERE subtype = "staking deposit asset";'
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v31_v32.py#L69'>rotkehlchen/db/upgrades/v31_v32.py~L69</a>
```diff
 
 def _add_new_tables(cursor: "DBCursor") -> None:
     cursor.execute('INSERT OR IGNORE INTO location(location, seq) VALUES ("d", 36)')
-    cursor.execute("""
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS ethereum_internal_transactions (
     parent_tx_hash BLOB NOT NULL,
     trace_id INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v31_v32.py#L80'>rotkehlchen/db/upgrades/v31_v32.py~L80</a>
```diff
     value TEXT NOT NULL,
     FOREIGN KEY(parent_tx_hash) REFERENCES ethereum_transactions(tx_hash) ON DELETE CASCADE ON UPDATE CASCADE,
     PRIMARY KEY(parent_tx_hash, trace_id)
-);""")  # noqa: E501
-    cursor.execute("""
+);"""
+    )  # noqa: E501
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS ethtx_address_mappings (
     address TEXT NOT NULL,
     tx_hash BLOB NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v31_v32.py#L89'>rotkehlchen/db/upgrades/v31_v32.py~L89</a>
```diff
     FOREIGN KEY(blockchain, address) REFERENCES blockchain_accounts(blockchain, account) ON DELETE CASCADE,
     FOREIGN KEY(tx_hash) references ethereum_transactions(tx_hash) ON UPDATE CASCADE ON DELETE CASCADE,
     PRIMARY KEY (address, tx_hash, blockchain)
-);""")  # noqa: E501
-    cursor.execute("""
+);"""
+    )  # noqa: E501
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evm_tx_mappings (
     tx_hash BLOB NOT NULL,
     blockchain TEXT NOT NULL,
     value TEXT NOT NULL,
     FOREIGN KEY(tx_hash) references ethereum_transactions(tx_hash) ON UPDATE CASCADE ON DELETE CASCADE,
     PRIMARY KEY (tx_hash, value)
-);""")  # noqa: E501
-    cursor.execute("""
+);"""
+    )  # noqa: E501
+    cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS history_events_mappings (
     parent_identifier INTEGER NOT NULL,
     value TEXT NOT NULL,
     FOREIGN KEY(parent_identifier) references history_events(identifier) ON UPDATE CASCADE ON DELETE CASCADE,
     PRIMARY KEY (parent_identifier, value)
-);""")  # noqa: E501
+);"""
+    )  # noqa: E501
     cursor.execute("""
     CREATE TABLE IF NOT EXISTS ens_mappings (
     address TEXT NOT NULL PRIMARY KEY,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v34_v35.py#L134'>rotkehlchen/db/upgrades/v34_v35.py~L134</a>
```diff
     log.debug("Enter _change_xpub_mappings_primary_key")
     with conn.read_ctx() as read_cursor:
         xpub_mappings = read_cursor.execute("SELECT * from xpub_mappings").fetchall()
-    write_cursor.execute("""CREATE TABLE xpub_mappings_copy (
+    write_cursor.execute(
+        """CREATE TABLE xpub_mappings_copy (
         address TEXT NOT NULL,
         xpub TEXT NOT NULL,
         derivation_path TEXT NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v34_v35.py#L150'>rotkehlchen/db/upgrades/v34_v35.py~L150</a>
```diff
         ) ON DELETE CASCADE
         PRIMARY KEY (address, xpub, derivation_path, blockchain)
     );
-    """)
+    """
+    )
     write_cursor.executemany(
         "INSERT INTO xpub_mappings_copy VALUES (?, ?, ?, ?, ?, ?)", xpub_mappings
     )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v34_v35.py#L242'>rotkehlchen/db/upgrades/v34_v35.py~L242</a>
```diff
         ))
 
     cursor.execute("DROP TABLE history_events;")
-    cursor.execute("""CREATE TABLE IF NOT EXISTS history_events (
+    cursor.execute(
+        """CREATE TABLE IF NOT EXISTS history_events (
     identifier INTEGER NOT NULL PRIMARY KEY,
     event_identifier BLOB NOT NULL,
     sequence_index INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v34_v35.py#L259'>rotkehlchen/db/upgrades/v34_v35.py~L259</a>
```diff
     extra_data TEXT,
     FOREIGN KEY(asset) REFERENCES assets(identifier) ON UPDATE CASCADE,
     UNIQUE(event_identifier, sequence_index)
-    );""")
+    );"""
+    )
 
     insertion_query = (
         "INSERT INTO history_events( "
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v36_v37.py#L101'>rotkehlchen/db/upgrades/v36_v37.py~L101</a>
```diff
     log.debug("Enter _update_history_events_schema")
 
     _reset_decoded_events(write_cursor)
-    write_cursor.execute("""CREATE TABLE IF NOT EXISTS history_events_copy (
+    write_cursor.execute(
+        """CREATE TABLE IF NOT EXISTS history_events_copy (
     identifier INTEGER NOT NULL PRIMARY KEY,
     entry_type INTEGER NOT NULL,
     event_identifier TEXT NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v36_v37.py#L117'>rotkehlchen/db/upgrades/v36_v37.py~L117</a>
```diff
     subtype TEXT NOT NULL,
     FOREIGN KEY(asset) REFERENCES assets(identifier) ON UPDATE CASCADE,
     UNIQUE(event_identifier, sequence_index)
-    );""")
+    );"""
+    )
     new_entries = []
     extra_evm_info_entries = []
     with conn.read_ctx() as read_cursor:
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L48'>rotkehlchen/db/upgrades/v38_v39.py~L48</a>
```diff
 
 def _create_new_tables(write_cursor: "DBCursor") -> None:
     log.debug("Enter _create_new_tables")
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS optimism_transactions (
         tx_id INTEGER NOT NULL PRIMARY KEY,
         l1_fee TEXT,
     FOREIGN KEY(tx_id) REFERENCES evm_transactions(identifier) ON DELETE CASCADE ON UPDATE CASCADE
-    );""")
+    );"""
+    )
     log.debug("Exit _create_new_tables")
 
 
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L114'>rotkehlchen/db/upgrades/v38_v39.py~L114</a>
```diff
     write_cursor.execute("DROP TABLE IF EXISTS evm_tx_mappings")
     write_cursor.execute("DROP TABLE IF EXISTS evmtx_address_mappings")
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evm_transactions (
         identifier INTEGER NOT NULL PRIMARY KEY,
         tx_hash BLOB NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L130'>rotkehlchen/db/upgrades/v38_v39.py~L130</a>
```diff
         input_data BLOB NOT NULL,
         nonce INTEGER NOT NULL,
         UNIQUE(tx_hash, chain_id)
-    );""")
+    );"""
+    )
     hashchain_to_id = {}
     tx_data = []
     for identifier, data in enumerate(txs):
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L143'>rotkehlchen/db/upgrades/v38_v39.py~L143</a>
```diff
         tx_data,
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evm_tx_mappings (
         tx_id INTEGER NOT NULL,
         value INTEGER NOT NULL,
         FOREIGN KEY(tx_id) references evm_transactions(identifier) ON UPDATE CASCADE ON DELETE CASCADE,
         PRIMARY KEY (tx_id, value)
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     write_cursor.executemany(
         "INSERT INTO evm_tx_mappings(tx_id, value) VALUES(?, ?)",
         [(hashchain_to_id[x[0] + x[1].to_bytes(4, byteorder="big")], *x[2:]) for x in tx_mappings],
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evmtx_address_mappings (
         tx_id INTEGER NOT NULL,
         address TEXT NOT NULL,
         FOREIGN KEY(tx_id) references evm_transactions(identifier) ON UPDATE CASCADE ON DELETE CASCADE,
         PRIMARY KEY (tx_id, address)
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     write_cursor.executemany(
         "INSERT INTO evmtx_address_mappings(tx_id, address) VALUES(?, ?)",
         [
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L170'>rotkehlchen/db/upgrades/v38_v39.py~L170</a>
```diff
         ],  # noqa: E501
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evm_internal_transactions (
         parent_tx INTEGER NOT NULL,
         trace_id INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L179'>rotkehlchen/db/upgrades/v38_v39.py~L179</a>
```diff
         value TEXT NOT NULL,
         FOREIGN KEY(parent_tx) REFERENCES evm_transactions(identifier) ON DELETE CASCADE ON UPDATE CASCADE,
         PRIMARY KEY(parent_tx, trace_id, from_address, to_address, value)
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     write_cursor.executemany(
         "INSERT INTO evm_internal_transactions(parent_tx, trace_id, from_address, to_address, "
         "value) VALUES(?, ?, ?, ?, ?)",
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L189'>rotkehlchen/db/upgrades/v38_v39.py~L189</a>
```diff
         ],  # noqa: E501
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evmtx_receipts (
         tx_id INTEGER NOT NULL PRIMARY KEY,
         contract_address TEXT, /* can be null */
         status INTEGER NOT NULL CHECK (status IN (0, 1)),
         type INTEGER NOT NULL,
         FOREIGN KEY(tx_id) REFERENCES evm_transactions(identifier) ON DELETE CASCADE ON UPDATE CASCADE
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     write_cursor.executemany(
         "INSERT INTO evmtx_receipts(tx_id, contract_address, status, type) " "VALUES(?, ?, ?, ?)",
         [(hashchain_to_id[x[0] + x[1].to_bytes(4, byteorder="big")], *x[2:]) for x in receipts],
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evmtx_receipt_logs (
         identifier INTEGER NOT NULL PRIMARY KEY,  /* adding identifier here instead of composite key in order to not duplicate in topics which are A LOT */
         tx_id INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L212'>rotkehlchen/db/upgrades/v38_v39.py~L212</a>
```diff
         removed INTEGER NOT NULL CHECK (removed IN (0, 1)),
         FOREIGN KEY(tx_id) REFERENCES evmtx_receipts(tx_id) ON DELETE CASCADE ON UPDATE CASCADE,
         UNIQUE(tx_id, log_index)
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     hashchainlog_to_id = {}
     log_data = []
     for identifier, data in enumerate(logs):
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v38_v39.py#L225'>rotkehlchen/db/upgrades/v38_v39.py~L225</a>
```diff
         log_data,
     )
 
-    write_cursor.execute("""
+    write_cursor.execute(
+        """
     CREATE TABLE IF NOT EXISTS evmtx_receipt_log_topics (
         log INTEGER NOT NULL,
         topic BLOB NOT NULL,
         topic_index INTEGER NOT NULL,
         FOREIGN KEY(log) REFERENCES evmtx_receipt_logs(identifier) ON DELETE CASCADE ON UPDATE CASCADE,
         PRIMARY KEY(log, topic_index)
-    );""")  # noqa: E501
+    );"""
+    )  # noqa: E501
     write_cursor.executemany(
         "INSERT INTO evmtx_receipt_log_topics(log, topic, topic_index) " "VALUES(?, ?, ?)",
         [
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/db/upgrades/v39_v40.py#L372'>rotkehlchen/db/upgrades/v39_v40.py~L372</a>
```diff
     Add new tables for this upgrade
     """
     log.debug("Entered _add_new_tables")
-    write_cursor.execute("""CREATE TABLE IF NOT EXISTS skipped_external_events (
+    write_cursor.execute(
+        """CREATE TABLE IF NOT EXISTS skipped_external_events (
     identifier INTEGER NOT NULL PRIMARY KEY,
     data TEXT NOT NULL,
     location CHAR(1) NOT NULL DEFAULT('A') REFERENCES location(location),
     extra_data TEXT,
     UNIQUE(data, location)
-    );""")
+    );"""
+    )
     write_cursor.execute("""
     CREATE TABLE IF NOT EXISTS accounting_rules(
         identifier INTEGER NOT NULL PRIMARY KEY,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/globaldb/upgrades/v2_v3.py#L399'>rotkehlchen/globaldb/upgrades/v2_v3.py~L399</a>
```diff
         cursor.switch_foreign_keys("ON")
 
         log.debug("Create new tables")
-        cursor.execute("""
+        cursor.execute(
+            """
         CREATE TABLE IF NOT EXISTS token_kinds (
           token_kind    CHAR(1)       PRIMARY KEY NOT NULL,
           seq     INTEGER UNIQUE
-        );""")
+        );"""
+        )
         # ERC20
         cursor.execute('INSERT OR IGNORE INTO token_kinds(token_kind, seq) VALUES ("A", 1)')
         # ERC721
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/globaldb/upgrades/v3_v4.py#L108'>rotkehlchen/globaldb/upgrades/v3_v4.py~L108</a>
```diff
 def _create_new_tables(cursor: "DBCursor") -> None:
     log.debug("Enter _create_new_tables")
 
-    cursor.execute("""
+    cursor.execute(
+        """
         CREATE TABLE IF NOT EXISTS contract_abi (
             id INTEGER NOT NULL PRIMARY KEY,
             value TEXT NOT NULL,
             name TEXT
-        );""")
-    cursor.execute("""
+        );"""
+    )
+    cursor.execute(
+        """
         CREATE TABLE IF NOT EXISTS contract_data (
             address VARCHAR[42] NOT NULL,
             chain_id INTEGER NOT NULL,
```
<a href='https://github.com/rotki/rotki/blob/311e772caeb0bcb564db82a553ecc2c9c196094e/rotkehlchen/globaldb/upgrades/v3_v4.py#L123'>rotkehlchen/globaldb/upgrades/v3_v4.py~L123</a>
```diff
             deployed_block INTEGER,
             FOREIGN KEY(abi) REFERENCES contract_abi(id) ON UPDATE CASCADE ON DELETE SET NULL,
             PRIMARY KEY(address, chain_id)
-        );""")
+        );"""
+    )
 
     log.debug("Exit _create_new_tables")
 
```

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/scikit-build/scikit-build/blob/59492c19ba35922fa612cf447414f38a2aa0a93e/skbuild/platform_specifics/abstract.py#L170'>skbuild/platform_specifics/abstract.py~L170</a>
```diff
         if working_generator is None:
             line = "*" * 80
             installation_help = self.generator_installation_help
-            msg = textwrap.dedent(f"""\
+            msg = textwrap.dedent(
+                f"""\
                 {line}
                 scikit-build could not get a working generator for your system. Aborting build.
 
                 {installation_help}
 
-                {line}""")
+                {line}"""
+            )
             raise SKBuildGeneratorNotFoundError(msg)
 
         if cleanup:
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/ext/apidoc.py#L344'>sphinx/ext/apidoc.py~L344</a>
```diff
     parser = argparse.ArgumentParser(
         usage="%(prog)s [OPTIONS] -o <OUTPUT_PATH> <MODULE_PATH> " "[EXCLUDE_PATTERN, ...]",
         epilog=__("For more information, visit <https://www.sphinx-doc.org/>."),
-        description=__("""
+        description=__(
+            """
 Look recursively in <MODULE_PATH> for Python modules and packages and create
 one reST file with automodule directives per package in the <OUTPUT_PATH>.
 
 The <EXCLUDE_PATTERN>s can be file and/or directory patterns that will be
 excluded from generation.
 
-Note: By default this script will not overwrite already created files."""),
+Note: By default this script will not overwrite already created files."""
+        ),
     )
 
     parser.add_argument(
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+16 -8 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/zulip/zulip/blob/3fde4f977d0f91dc85eb98ae71fb9536d3184fa6/zerver/lib/export.py#L525'>zerver/lib/export.py~L525</a>
```diff
 
         if self.id_source is not None:
             if self.virtual_parent is None:
-                raise AssertionError("""
+                raise AssertionError(
+                    """
                     You must specify a virtual_parent if you are
-                    using id_source.""")
+                    using id_source."""
+                )
             if self.id_source[0] != self.virtual_parent.table:
-                raise AssertionError(f"""
+                raise AssertionError(
+                    f"""
                     Configuration error.  To populate {self.table}, you
                     want data from {self.id_source[0]}, but that differs from
                     the table name of your virtual parent ({self.virtual_parent.table}),
                     which suggests you many not have set up
                     the ordering correctly.  You may simply
                     need to assign a virtual_parent, or there
-                    may be deeper issues going on.""")
+                    may be deeper issues going on."""
+                )
 
 
 def export_from_config(
```
<a href='https://github.com/zulip/zulip/blob/3fde4f977d0f91dc85eb98ae71fb9536d3184fa6/zerver/lib/test_classes.py#L1282'>zerver/lib/test_classes.py~L1282</a>
```diff
             for index, query in enumerate(queries):
                 print(f"#{index + 1}\nsql: {query.sql}\ntime: {query.time}\n")
             print(f"expected count: {count}\nactual count: {actual_count}")
-            raise AssertionError(f"""
+            raise AssertionError(
+                f"""
     {count} queries expected, got {actual_count}.
     This is a performance-critical code path, where we check
     the number of database queries used in order to avoid accidental regressions.
     If an unnecessary query was removed or the new query is necessary, you should
     update this test, and explain what queries we added/removed in the pull request
-    and why any new queries can't be avoided.""")
+    and why any new queries can't be avoided."""
+            )
 
     def assert_json_error_contains(
         self, result: "TestHttpResponse", msg_substring: str, status_code: int = 400
```
<a href='https://github.com/zulip/zulip/blob/3fde4f977d0f91dc85eb98ae71fb9536d3184fa6/zerver/lib/test_classes.py#L1377'>zerver/lib/test_classes.py~L1377</a>
```diff
                 can_remove_subscribers_group=administrators_user_group,
             )
         except IntegrityError:  # nocoverage -- this is for bugs in the tests
-            raise Exception(f"""
+            raise Exception(
+                f"""
                 {stream_name} already exists
 
                 Please call make_stream with a stream name
-                that is not already in use.""")
+                that is not already in use."""
+            )
 
         recipient = Recipient.objects.create(type_id=stream.id, type=Recipient.STREAM)
         stream.recipient = recipient
```

</p>
</details>


---

_Converted to draft by @MichaReiser on 2024-01-08 17:03_

---

_Comment by @MichaReiser on 2024-01-08 17:03_

I plan to make changes to this implementation.

---

_Comment by @MichaReiser on 2024-01-09 10:54_

The stable style formatting changes now match Black's stable formatting. We can consider it a bugfix.

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:1502 on 2024-01-09 10:55_

This is a bug fix in the printer. So far, it always returned `Fits::Yes` if a text contained a newline character regardless if the text before the newline exceeded the line width. This was incorrect. 

---

_@MichaReiser reviewed on 2024-01-09 10:55_

---

_Review requested from @konstin by @MichaReiser on 2024-01-09 11:04_

---

_Marked ready for review by @MichaReiser on 2024-01-09 11:04_

---

_Comment by @MichaReiser on 2024-01-10 06:49_

@charliermarsh or @konstin I changed the implementation quiet significantly. It would be great to get a second review. 

---

_@konstin approved on 2024-01-10 10:27_

---

_Merged by @MichaReiser on 2024-01-10 11:47_

---

_Closed by @MichaReiser on 2024-01-10 11:47_

---

_Branch deleted on 2024-01-10 11:47_

---

_Comment by @T-256 on 2024-01-10 12:24_

cc @zanieb 
Are diffs now show correct before/after? I think they are reversed.
I checked source links of diffs, and it seems what it shows as added lines, already exist at source.


> ### Formatter (stable)
> ℹ️ ecosystem check **detected format changes**. (+5 -6 lines in 2 files in 2 projects; 41 projects unchanged)
> 
> [demisto/content](https://github.com/demisto/content) (+2 -5 lines across 1 file)
> ruff format --no-preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py
> [Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py~L107](https://github.com/demisto/content/blob/33a4829031d36f83595165e9f2beb8376c8a98e0/Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py#L107)
> 
> ```diff
>      )
>  
>      if is_error(response):
> -        error = (
> -            f'Failed retrieving tasks for incident ID {incident["id"]}.\n \
> +        error = f'Failed retrieving tasks for incident ID {incident["id"]}.\n \
>             Make sure that the API key configured in the Core REST API integration \
> -is one with sufficient permissions to access that incident.\n'
> -            + get_error(response)
> -        )
> +is one with sufficient permissions to access that incident.\n' + get_error(response)
>          raise Exception(error)
>  
>      return response[0]["Contents"]["response"]
> ```
> 
> [reflex-dev/reflex](https://github.com/reflex-dev/reflex) (+3 -1 lines across 1 file)
> [reflex/components/markdown/markdown.py~L259](https://github.com/reflex-dev/reflex/blob/7cec7feb63af7c489474c18894ce3ca262f69103/reflex/components/markdown/markdown.py#L259)
> 
> ```diff
>          }
>  
>          # Separate out inline code and code blocks.
> -        components["code"] = f"""{{({{node, inline, className, {_CHILDREN._var_name}, {_PROPS._var_name}}}) => {{
> +        components[
> +            "code"
> +        ] = f"""{{({{node, inline, className, {_CHILDREN._var_name}, {_PROPS._var_name}}}) => {{
>      const match = (className || '').match(/language-(?<lang>.*)/);
>      const language = match ? match[1] : '';
>      if (language) {{
> ```



---

_Comment by @MichaReiser on 2024-01-10 12:37_

@T-256 The diff shows that the old version (main) used to parenthesize `f'Failed...` and the new version (this PR) removes the parentheses around it. 

The diff isn't showing the difference to the commit version in Git because some projects in our ecosystem check may not use Ruff formatter. 

---
