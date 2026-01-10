```yaml
number: 9637
title: Preserve indent around multiline strings
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: multiline-string-preserve-indent
created_at: 2024-01-24T17:22:16Z
updated_at: 2024-01-26T07:18:32Z
url: https://github.com/astral-sh/ruff/pull/9637
synced_at: 2026-01-10T22:57:09Z
```

# Preserve indent around multiline strings

---

_Pull request opened by @MichaReiser on 2024-01-24 17:22_

## Summary

This PR changes our `multiline_string` preview style implementation (added in https://github.com/astral-sh/ruff/pull/9243) to only hug strings in call arguments when there's no newline between the `(` and the string's quotes.

This is to address the feedback raised in Black's repository ([issue](https://github.com/psf/black/issues/4159))

The stable formatting always indents strings:

```python
dedent(
    """
    value
    """
)
```

The preview style as it is implemented today (not this PR) tries to hug multiline strings, and only falls back to indenting the string if the first line of the string exceeds the configured line width:

```python
dedent("""
    value
""")
```

The benefit of the new style is that it reduces vertical spacing (The two strings in the examples aren't equivalent because of the whitespace before the closing quotes but the whitespace before the closing quotes with `indent` is only used to align the quotes. 

The main concern with the new preview style is that it messes up the multiline string formatting for existing Black users because it can dealign the quotes and fixing the quote alignment isn't possible without knowing if the argument is whitespace sensitive or not. 

For example, it turns...

```python
dedent(
    """
    value
    """
)
```

into...

```python
dedent("""
    value
    """)
```

which looks worse. 

However, the preview style improves formatting for new black users or new code written when you have:

```python
def test():
    a = dedent("""
      Some code
      that needs dedenting
    """)
```

because it no longer changes it to

```python
def test():
    a = dedent(
        """
      Some code
      that needs dedenting
    """
    )
```


This PR refiens the `multiline_string` preview style with a heuristic of when to hug the multiline string and when not. 

The idea is to honor the author's decision by checking if the opening parentheses and the string both start on the same line. If so, hug the multline string, otherwise don't. 

There are two advantages to this:

* Existing Blacked code won't change because the formatter detects the newline between the opening parentheses and the quotes and continues to indent the multiline string
* Existing Blacked or Ruffed code using preview style won't change because the formatter detects that there's no newline between the opening parentheses and the string start and, because of it, won't indent the multiline string. 

This heuristic is the same as Prettier uses for single-argument, multiline string call expressions. 

## Downsides

The downside of this heuristic over always hugging single argument multiline strings is that existing calls to `dedent` need manual updating to match the desired formatting. I think that's fine because the new formatting also requires to manually updating the spacing before the closing `"""` to match indentation:

```python
dedent("""
	Multiline
	""")
```

Doesn't look good. You want 

```python
dedent("""
	Multiline
""")
```

which ruff can't do without changing the string's semantics.

## Test Plan

**Running Ruff before introducing the `multline-string` preview style and then this branch**:

There are no upgrade changes, compared to the original `multiline-string` preview style PR that changed+2971 -5900 lines in 374 files in 31 projects. 

ℹ️ ecosystem check **detected format changes**. (+7 -8 lines in 3 files in 3 projects; 1 project error; 39 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+2 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/3d621f512f4575d87e68e317570dcd90411b9607/Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py#L107'>Packs/IntegrationsAndIncidentsHealthCheck/Scripts/GetFailedTasks/GetFailedTasks.py~L107</a>
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
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/0d8efb9b5570425615db00cd65e6070317488f6f/reflex/components/markdown/markdown.py#L253'>reflex/components/markdown/markdown.py~L253</a>
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
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/5b72d2f90a87d5f82df519cbe385edd7a61474fd/rotkehlchen/rotkehlchen.py#L1196'>rotkehlchen/rotkehlchen.py~L1196</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with (
-            contextlib.suppress(UnknownAsset)
+        with contextlib.suppress(
+            UnknownAsset
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

**Running ruff preview (main) and then this branch**:

Results in no changes

ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Label `formatter` added by @MichaReiser on 2024-01-24 17:25_

---

_Label `preview` added by @MichaReiser on 2024-01-24 17:25_

---

_Comment by @github-actions[bot] on 2024-01-24 17:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+10156 -5096 lines in 500 files in 30 projects; 13 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/scripts/codemods/typed_flags.py#L104'>scripts/codemods/typed_flags.py~L104</a>
```diff
             else:
                 # one doesn't currently exist, great.
                 # so we have two options here... we could make a typechecking block from scratch or... we could cheat
-                code = textwrap.dedent("""
+                code = textwrap.dedent(
+                    """
                     if TYPE_CHECKING:
                         @_generated
                         def __init__(self):
                             ...
-                    """)
+                    """
+                )
                 if_block = cast(
                     "cst.If", cst.parse_statement(code, config=self.module.config_for_parsing)
                 )
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+892 -446 lines across 32 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/cli/telemetry.py#L57'>rasa/cli/telemetry.py~L57</a>
```diff
         )
 
     print(
-        textwrap.dedent("""
+        textwrap.dedent(
+            """
             Rasa uses telemetry to report anonymous usage information. This information
-            is essential to help improve Rasa Open Source for all users.""")
+            is essential to help improve Rasa Open Source for all users."""
+        )
     )
 
     if not is_enabled:
```
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
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L357'>tests/cli/test_utils.py~L357</a>
```diff
     file_type: Text, data_type: Text, tmp_path: Path
 ):
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(f"""
+    file_name.write_text(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
           steps:
           - intent: goodbye
           - action: action_test
-        """)
+        """
+    )
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L387'>tests/cli/test_utils.py~L387</a>
```diff
     file_type: Text, data_type: Text, tmp_path: Path
 ):
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(f"""
+    file_name.write_text(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L395'>tests/cli/test_utils.py~L395</a>
```diff
             - intent: request_restaurant
             - action: restaurant_form
             - active_loop: restaurant_form
-        """)
+        """
+    )
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L421'>tests/cli/test_utils.py~L421</a>
```diff
     )
     nlu_file = "data/test_nlu/test_nlu_validate_files_with_active_loop_null.yml"
     file_name = tmp_path / f"{file_type}.yml"
-    file_name.write_text(f"""
+    file_name.write_text(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         {file_type}:
         - {data_type}: test path
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L431'>tests/cli/test_utils.py~L431</a>
```diff
             - active_loop: restaurant_form
             - active_loop: null
             - action: action_search_restaurants
-        """)
+        """
+    )
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_unique_assistant_id.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L458'>tests/cli/test_utils.py~L458</a>
```diff
 
 def test_validate_files_form_slots_not_matching(tmp_path: Path):
     domain_file_name = tmp_path / "domain.yml"
-    domain_file_name.write_text(f"""
+    domain_file_name.write_text(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         forms:
           name_form:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L474'>tests/cli/test_utils.py~L474</a>
```diff
                 type: text
                 mappings:
                 - type: from_text
-        """)
+        """
+    )
 
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L528'>tests/cli/test_utils.py~L528</a>
```diff
     tested_slot = "duration"
     form_name = "booking_form"
     # form required_slots does not include the tested_slot
-    domain.write_text(f"""
+    domain.write_text(
+        f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intents:
             - state_length_of_time
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_utils.py#L552'>tests/cli/test_utils.py~L552</a>
```diff
               {form_name}:
                 required_slots:
                 - location
-                """)
+                """
+    )
     importer = TrainingDataImporter.load_from_config(
         "data/test_config/config_defaults.yml", str(domain), None
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/conftest.py#L172'>tests/conftest.py~L172</a>
```diff
 def simple_config_path(tmp_path_factory: TempPathFactory) -> Text:
     project_path = tmp_path_factory.mktemp(uuid.uuid4().hex)
 
-    config = textwrap.dedent(f"""
+    config = textwrap.dedent(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         assistant_id: placeholder_default
         pipeline:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/conftest.py#L183'>tests/conftest.py~L183</a>
```diff
         - name: AugmentedMemoizationPolicy
           max_history: 3
         - name: RulePolicy
-        """)
+        """
+    )
     config_path = project_path / "config.yml"
     rasa.shared.utils.io.write_text_file(config, config_path)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L44'>tests/core/actions/test_forms.py~L44</a>
```diff
     form_name = "my form"
     action = FormAction(form_name, None)
     slot_name = "num_people"
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L58'>tests/core/actions/test_forms.py~L58</a>
```diff
     responses:
       utter_ask_num_people:
       - text: "How many people?"
-      """)
+      """
+    )
     domain = Domain.from_yaml(domain)
 
     events = await action.run(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L79'>tests/core/actions/test_forms.py~L79</a>
```diff
     domain_required_slot_name = "num_people"
     slot_set_by_remote_custom_extraction_method = "some_slot"
     slot_value_set_by_remote_custom_extraction_method = "anything"
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     slots:
       {domain_required_slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L99'>tests/core/actions/test_forms.py~L99</a>
```diff
       - text: "How many people?"
     actions:
       - validate_{form_name}
-      """)
+      """
+    )
     domain = Domain.from_yaml(domain)
 
     form_validation_events = [
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L137'>tests/core/actions/test_forms.py~L137</a>
```diff
     form_name = "my form"
     action = FormAction(form_name, None)
     slot_name = "num_people"
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L153'>tests/core/actions/test_forms.py~L153</a>
```diff
     responses:
       utter_ask_num_people:
       - text: "How many people?"
-      """)
+      """
+    )
     domain = Domain.from_yaml(domain)
 
     events = await action.run(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L863'>tests/core/actions/test_forms.py~L863</a>
```diff
 def test_temporary_tracker():
     extra_slot = "some_slot"
     sender_id = "test"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         slots:
           {extra_slot}:
             type: any
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     previous_events = [ActionExecuted(ACTION_LISTEN_NAME)]
     old_tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1535'>tests/core/actions/test_forms.py~L1535</a>
```diff
     form = FormAction(form_name, None)
 
     domain = Domain.from_yaml(
-        textwrap.dedent(f"""
+        textwrap.dedent(
+            f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1564'>tests/core/actions/test_forms.py~L1564</a>
```diff
                required_slots:
                  - email
                  - name
-            """)
+            """
+        )
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1605'>tests/core/actions/test_forms.py~L1605</a>
```diff
     form = FormAction(form_name, None)
 
     domain = Domain.from_yaml(
-        textwrap.dedent(f"""
+        textwrap.dedent(
+            f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1634'>tests/core/actions/test_forms.py~L1634</a>
```diff
                required_slots:
                  - email
                  - name
-            """)
+            """
+        )
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1673'>tests/core/actions/test_forms.py~L1673</a>
```diff
 
 
 async def test_action_extract_slots_custom_mapping_with_condition():
-    domain_yaml = textwrap.dedent(f"""
+    domain_yaml = textwrap.dedent(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
 
         slots:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1692'>tests/core/actions/test_forms.py~L1692</a>
```diff
 
         actions:
         - validate_some_form
-        """)
+        """
+    )
     domain = Domain.from_yaml(domain_yaml)
     events = [ActiveLoop("some_form"), UserUttered("Hi")]
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1733'>tests/core/actions/test_forms.py~L1733</a>
```diff
 
 async def test_form_slots_empty_with_restart():
     domain = Domain.from_yaml(
-        textwrap.dedent(f"""
+        textwrap.dedent(
+            f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             intent:
             - greet
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1753'>tests/core/actions/test_forms.py~L1753</a>
```diff
              some_form:
                required_slots:
                  - name
-            """)
+            """
+        )
     )
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1795'>tests/core/actions/test_forms.py~L1795</a>
```diff
 
     form_name = "test_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
     entities:
     - {entity_name}
     slots:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1810'>tests/core/actions/test_forms.py~L1810</a>
```diff
       {form_name}:
         {REQUIRED_SLOTS_KEY}:
             - {slot_name}
-    """)
+    """
+    )
 
     events = [
         ActionExecuted("action_listen"),
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1861'>tests/core/actions/test_forms.py~L1861</a>
```diff
     action_server = EndpointConfig(ACTION_SERVER_URL)
     action = FormAction(form_name, action_server)
     slot_name = "num_people"
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     slots:
       {slot_name}:
         type: float
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1877'>tests/core/actions/test_forms.py~L1877</a>
```diff
       - text: "How many people?"
     actions:
       - validate_{form_name}
-    """)
+    """
+    )
     domain = Domain.from_yaml(domain)
     form_validation_events = [
         {
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1929'>tests/core/actions/test_forms.py~L1929</a>
```diff
     required_slot = "send_sms"
     global_slot = "membership"
 
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     intents:
     - start_form
     entities:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L1967'>tests/core/actions/test_forms.py~L1967</a>
```diff
       - text: "Would you like to receive an SMS?"
     actions:
       - validate_{form}
-    """)
+    """
+    )
     domain = Domain.from_yaml(domain)
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L2033'>tests/core/actions/test_forms.py~L2033</a>
```diff
     dynamic_slot = "customer_name"
     bot_utterance = "What is your name?"
 
-    domain = textwrap.dedent(f"""
+    domain = textwrap.dedent(
+        f"""
     intents:
     - start_form
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_forms.py#L2052'>tests/core/actions/test_forms.py~L2052</a>
```diff
       - text: "{bot_utterance}"
     actions:
       - validate_{form}
-    """)
+    """
+    )
     domain = Domain.from_yaml(domain)
 
     tracker = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/actions/test_two_stage_fallback.py#L157'>tests/core/actions/test_two_stage_fallback.py~L157</a>
```diff
         ],
     )
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
             utter_ask_rephrase:
             - text: {rephrase_text}
-        """)
+        """
+    )
     action = TwoStageFallbackAction()
 
     events = await action.run(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L89'>tests/core/nlg/test_generator.py~L89</a>
```diff
     )
     output_channel = "default"
 
-    domain_yaml = textwrap.dedent("""
+    domain_yaml = textwrap.dedent(
+        """
         version: "3.1"
 
         intents:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L124'>tests/core/nlg/test_generator.py~L124</a>
```diff
               - type: slot
                 name: logged_in
                 value: true
-        """)
+        """
+    )
 
     domain = Domain.from_yaml(domain_yaml)
     response_variation_filter = ResponseVariationFilter(domain.responses)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L161'>tests/core/nlg/test_generator.py~L161</a>
```diff
     )
     output_channel = "default"
 
-    domain_yaml = textwrap.dedent("""
+    domain_yaml = textwrap.dedent(
+        """
         version: "3.1"
 
         intents:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L192'>tests/core/nlg/test_generator.py~L192</a>
```diff
               - type: slot
                 name: membership
                 value: gold
-        """)
+        """
+    )
 
     domain = Domain.from_yaml(domain_yaml)
     response_variation_filter = ResponseVariationFilter(domain.responses)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L220'>tests/core/nlg/test_generator.py~L220</a>
```diff
         sender_id="crv_channel", evts=[UserUttered("Hello")], slots=[membership_slot]
     )
 
-    domain_yaml = textwrap.dedent("""
+    domain_yaml = textwrap.dedent(
+        """
         version: "3.1"
 
         intents:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L257'>tests/core/nlg/test_generator.py~L257</a>
```diff
                 name: membership
                 value: gold
               channel: web
-        """)
+        """
+    )
 
     domain = Domain.from_yaml(domain_yaml)
     response_variation_filter = ResponseVariationFilter(domain.responses)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L312'>tests/core/nlg/test_generator.py~L312</a>
```diff
         sender_id="edge_cases", evts=[UserUttered("Hello")], slots=[membership_slot]
     )
 
-    domain_yaml = textwrap.dedent("""
+    domain_yaml = textwrap.dedent(
+        """
         version: "3.1"
 
         intents:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L363'>tests/core/nlg/test_generator.py~L363</a>
```diff
               - type: slot
                 name: membership
                 value: gold
-        """)
+        """
+    )
 
     domain = Domain.from_yaml(domain_yaml)
     response_variation_filter = ResponseVariationFilter(domain.responses)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L391'>tests/core/nlg/test_generator.py~L391</a>
```diff
         sender_id="duplicate_ids", evts=[UserUttered("Hello")], slots=[membership_slot]
     )
 
-    domain_yaml = textwrap.dedent("""
+    domain_yaml = textwrap.dedent(
+        """
         version: "3.1"
 
         intents:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_generator.py#L418'>tests/core/nlg/test_generator.py~L418</a>
```diff
 
             - text: ""
               id: "ID_1"
-        """)
+        """
+    )
 
     domain = Domain.from_yaml(domain_yaml)
     response_variation_filter = ResponseVariationFilter(domain.responses)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L368'>tests/core/nlg/test_response.py~L368</a>
```diff
 
 
 async def test_nlg_non_matching_channel():
-    domain = Domain.from_yaml("""
+    domain = Domain.from_yaml(
+        """
     version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
     responses:
         utter_hi:
         - text: "Hello"
         - text: "Hello Slack"
           channel: "slack"
-    """)
+    """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     tracker = DialogueStateTracker(sender_id="test", slots=[])
     r = await t.generate("utter_hi", tracker, "signal")
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L383'>tests/core/nlg/test_response.py~L383</a>
```diff
 
 
 async def test_nlg_conditional_response_variations_with_none_slot():
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
             utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L392'>tests/core/nlg/test_response.py~L392</a>
```diff
               - type: slot
                 name: account
                 value: "A"
-        """)
+        """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     slot = AnySlot(
         name="account", mappings=[{}], initial_value=None, influence_conversation=False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L403'>tests/core/nlg/test_response.py~L403</a>
```diff
 
 
 async def test_nlg_conditional_response_variations_with_slot_not_a_constraint():
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
             version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
             responses:
                 utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L412'>tests/core/nlg/test_response.py~L412</a>
```diff
                   - type: slot
                     name: account
                     value: "A"
-            """)
+            """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     slot = TextSlot(
         name="account", mappings=[{}], initial_value="B", influence_conversation=False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L423'>tests/core/nlg/test_response.py~L423</a>
```diff
 
 
 async def test_nlg_conditional_response_variations_with_null_slot():
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
                 version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
                 responses:
                     utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L432'>tests/core/nlg/test_response.py~L432</a>
```diff
                       - type: slot
                         name: account
                         value: null
-                """)
+                """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     slot = AnySlot(
         name="account", mappings=[{}], initial_value=None, influence_conversation=False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L447'>tests/core/nlg/test_response.py~L447</a>
```diff
 
 
 async def test_nlg_conditional_response_variations_channel_no_condition_met():
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
            utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L458'>tests/core/nlg/test_response.py~L458</a>
```diff
                   value: A
                channel: os
              - text: "default"
-        """)
+        """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     tracker = DialogueStateTracker(sender_id="test", slots=[])
     r = await t.generate("utter_action", tracker, "os")
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L466'>tests/core/nlg/test_response.py~L466</a>
```diff
 
 
 async def test_nlg_conditional_response_variation_condition_met_channel_mismatch():
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
            utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L478'>tests/core/nlg/test_response.py~L478</a>
```diff
                channel: os
              - text: "app default"
                channel: app
-        """)
+        """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     slot = TextSlot(
         "test", mappings=[{}], initial_value="A", influence_conversation=False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L530'>tests/core/nlg/test_response.py~L530</a>
```diff
     ],
 )
 async def test_nlg_conditional_edgecases(slots, channel, expected_response):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
            utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L561'>tests/core/nlg/test_response.py~L561</a>
```diff
                   value: B
 
              - text: "default"
-        """)
+        """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     tracker = DialogueStateTracker(sender_id="test", slots=slots)
     r = await t.generate("utter_action", tracker, channel)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L571'>tests/core/nlg/test_response.py~L571</a>
```diff
 async def test_nlg_conditional_response_variations_condition_logging(
     caplog: LogCaptureFixture,
 ):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         responses:
            utter_action:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/nlg/test_response.py#L584'>tests/core/nlg/test_response.py~L584</a>
```diff
                   name: test_B
                   value: B
              - text: "default"
-        """)
+        """
+    )
     t = TemplatedNaturalLanguageGenerator(domain.responses)
     slot_A = TextSlot(
         name="test_A", mappings=[{}], initial_value="A", influence_conversation=False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L157'>tests/core/policies/test_rule_policy.py~L157</a>
```diff
     # conversation start -> ensure that this isn't flagged as a contradiction.
 
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """)
+        """
+    )
 
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L208'>tests/core/policies/test_rule_policy.py~L208</a>
```diff
     utter_anti_greet_action = "utter_anti_greet"
     some_slot = "slot1"
     some_slot_initial_value = "slot1value"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L221'>tests/core/policies/test_rule_policy.py~L221</a>
```diff
             initial_value: {some_slot_initial_value}
             mappings:
             - type: from_text
-        """)
+        """
+    )
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L279'>tests/core/policies/test_rule_policy.py~L279</a>
```diff
     utter_anti_greet_action = "utter_anti_greet"
     some_slot = "slot1"
     some_slot_initial_value = "slot1value"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L292'>tests/core/policies/test_rule_policy.py~L292</a>
```diff
             initial_value: {some_slot_initial_value}
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     greet_rule_at_conversation_start = TrackerWithCachedStates.from_events(
         "greet rule at conversation start",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L342'>tests/core/policies/test_rule_policy.py~L342</a>
```diff
 
 
 def test_restrict_multiple_user_inputs_in_rules(policy: RulePolicy):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
 
     greet_events = [
         UserUttered(intent={"name": GREET_INTENT_NAME}),
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L372'>tests/core/policies/test_rule_policy.py~L372</a>
```diff
 def test_incomplete_rules_due_to_slots(policy: RulePolicy):
     some_action = "some_action"
     some_slot = "some_slot"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L383'>tests/core/policies/test_rule_policy.py~L383</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L440'>tests/core/policies/test_rule_policy.py~L440</a>
```diff
 def test_no_incomplete_rules_due_to_slots_after_listen(policy: RulePolicy):
     some_action = "some_action"
     some_slot = "some_slot"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L453'>tests/core/policies/test_rule_policy.py~L453</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L500'>tests/core/policies/test_rule_policy.py~L500</a>
```diff
     some_slot_value = "value1"
     some_other_slot = "some_other_slot"
     some_other_slot_value = "value2"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L515'>tests/core/policies/test_rule_policy.py~L515</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     simple_rule = TrackerWithCachedStates.from_events(
         "simple rule with an action that sets 1 slot",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L553'>tests/core/policies/test_rule_policy.py~L553</a>
```diff
 
 def test_incomplete_rules_due_to_loops(policy: RulePolicy):
     some_form = "some_form"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         forms:
           {some_form}:
             required_slots: []
-        """)
+        """
+    )
 
     complete_rule = TrackerWithCachedStates.from_events(
         "complete_rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L616'>tests/core/policies/test_rule_policy.py~L616</a>
```diff
 
 def test_contradicting_rules(policy: RulePolicy):
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """)
+        """
+    )
 
     anti_greet_rule = TrackerWithCachedStates.from_events(
         "anti greet rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L654'>tests/core/policies/test_rule_policy.py~L654</a>
```diff
 
 def test_contradicting_rules_and_stories(policy: RulePolicy):
     utter_anti_greet_action = "utter_anti_greet"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
         - {utter_anti_greet_action}
-        """)
+        """
+    )
 
     anti_greet_story = TrackerWithCachedStates.from_events(
         "anti greet story",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L796'>tests/core/policies/test_rule_policy.py~L796</a>
```diff
 
 
 def test_faq_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
     # remove first ... action and utter_greet and last action_listen from greet rule
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L821'>tests/core/policies/test_rule_policy.py~L821</a>
```diff
 async def test_predict_form_action_if_in_form(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L836'>tests/core/policies/test_rule_policy.py~L836</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L864'>tests/core/policies/test_rule_policy.py~L864</a>
```diff
 async def test_predict_loop_action_if_in_loop_but_there_is_e2e_rule(policy: RulePolicy):
     loop_name = "some_loop"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L879'>tests/core/policies/test_rule_policy.py~L879</a>
```diff
         forms:
           {loop_name}:
             required_slots: []
-        """)
+        """
+    )
     e2e_rule = TrackerWithCachedStates.from_events(
         "bla",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L918'>tests/core/policies/test_rule_policy.py~L918</a>
```diff
 async def test_predict_form_action_if_multiple_turns(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L934'>tests/core/policies/test_rule_policy.py~L934</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L966'>tests/core/policies/test_rule_policy.py~L966</a>
```diff
 
 
 async def test_predict_slot_initial_value_not_required_in_rule(policy: RulePolicy):
-    domain = Domain.from_yaml("""
+    domain = Domain.from_yaml(
+        """
 intents:
 - i1
 actions:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L980'>tests/core/policies/test_rule_policy.py~L980</a>
```diff
     initial_value: v1
     mappings:
     - type: from_text
-""")
+"""
+    )
 
     rule = DialogueStateTracker.from_events(
         "slot rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1014'>tests/core/policies/test_rule_policy.py~L1014</a>
```diff
 
 
 async def test_predict_slot_with_initial_slot_matches_rule(policy: RulePolicy):
-    domain = Domain.from_yaml("""
+    domain = Domain.from_yaml(
+        """
 intents:
 - i1
 actions:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1028'>tests/core/policies/test_rule_policy.py~L1028</a>
```diff
     initial_value: v1
     mappings:
     - type: from_text
-""")
+"""
+    )
 
     rule = DialogueStateTracker.from_events(
         "slot rule",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1061'>tests/core/policies/test_rule_policy.py~L1061</a>
```diff
 async def test_predict_action_listen_after_form(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1076'>tests/core/policies/test_rule_policy.py~L1076</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1106'>tests/core/policies/test_rule_policy.py~L1106</a>
```diff
 async def test_dont_predict_form_if_already_finished(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1121'>tests/core/policies/test_rule_policy.py~L1121</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1155'>tests/core/policies/test_rule_policy.py~L1155</a>
```diff
 async def test_form_unhappy_path(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1170'>tests/core/policies/test_rule_policy.py~L1170</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1198'>tests/core/policies/test_rule_policy.py~L1198</a>
```diff
 async def test_form_unhappy_path_from_general_rule(policy: RulePolicy):
     form_name = "some_form"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1213'>tests/core/policies/test_rule_policy.py~L1213</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     # RulePolicy should memorize that unhappy_rule overrides GREET_RULE
     policy.train([GREET_RULE], domain)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1253'>tests/core/policies/test_rule_policy.py~L1253</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1269'>tests/core/policies/test_rule_policy.py~L1269</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     unhappy_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1327'>tests/core/policies/test_rule_policy.py~L1327</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1343'>tests/core/policies/test_rule_policy.py~L1343</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     unhappy_story = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1402'>tests/core/policies/test_rule_policy.py~L1402</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1418'>tests/core/policies/test_rule_policy.py~L1418</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     unhappy_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1491'>tests/core/policies/test_rule_policy.py~L1491</a>
```diff
     form_name = "some_form"
     handle_rejection_action_name = "utter_handle_rejection"
 
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1507'>tests/core/policies/test_rule_policy.py~L1507</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     unhappy_story = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1559'>tests/core/policies/test_rule_policy.py~L1559</a>
```diff
 async def test_form_unhappy_path_without_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1575'>tests/core/policies/test_rule_policy.py~L1575</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1603'>tests/core/policies/test_rule_policy.py~L1603</a>
```diff
 async def test_form_activation_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1619'>tests/core/policies/test_rule_policy.py~L1619</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     form_activation_rule = _form_activation_rule(domain, form_name, other_intent)
     policy.train([GREET_RULE, form_activation_rule], domain)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1642'>tests/core/policies/test_rule_policy.py~L1642</a>
```diff
 async def test_failing_form_activation_due_to_no_rule(policy: RulePolicy):
     form_name = "some_form"
     other_intent = "bye"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1658'>tests/core/policies/test_rule_policy.py~L1658</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1681'>tests/core/policies/test_rule_policy.py~L1681</a>
```diff
 def test_form_submit_rule(policy: RulePolicy):
     form_name = "some_form"
     submit_action_name = "utter_submit"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1697'>tests/core/policies/test_rule_policy.py~L1697</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     form_submit_rule = _form_submit_rule(domain, submit_action_name, form_name)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1733'>tests/core/policies/test_rule_policy.py~L1733</a>
```diff
     submit_action_name = "utter_submit"
     entity = "some_entity"
     slot = "some_slot"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1755'>tests/core/policies/test_rule_policy.py~L1755</a>
```diff
             required_slots: []
         entities:
         - {entity}
-        """)
+        """
+    )
 
     form_activation_rule = _form_activation_rule(domain, form_name, GREET_INTENT_NAME)
     form_submit_rule = _form_submit_rule(domain, submit_action_name, form_name)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1837'>tests/core/policies/test_rule_policy.py~L1837</a>
```diff
 
 
 async def test_one_stage_fallback_rule(policy: RulePolicy):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         - {DEFAULT_NLU_FALLBACK_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
 
     fallback_recover_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1918'>tests/core/policies/test_rule_policy.py~L1918</a>
```diff
 def test_default_actions(
     intent_name: Text, expected_action_name: Text, policy: RulePolicy
 ):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
     policy.train([GREET_RULE], domain)
     new_conversation = DialogueStateTracker.from_events(
         "bla2",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L1944'>tests/core/policies/test_rule_policy.py~L1944</a>
```diff
     "intent_name", [USER_INTENT_RESTART, USER_INTENT_BACK, USER_INTENT_SESSION_START]
 )
 def test_e2e_beats_default_actions(intent_name: Text, policy: RulePolicy):
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
 
     e2e_rule = TrackerWithCachedStates.from_events(
         "bla",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2003'>tests/core/policies/test_rule_policy.py~L2003</a>
```diff
     expected_prediction: Text,
 ):
     other_intent = "other"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2011'>tests/core/policies/test_rule_policy.py~L2011</a>
```diff
         actions:
         - {UTTER_GREET_ACTION}
         - my_core_fallback
-        """)
+        """
+    )
     rule_policy = policy_with_config(config)
     rule_policy.train([GREET_RULE], domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2034'>tests/core/policies/test_rule_policy.py~L2034</a>
```diff
     policy_with_config: Callable[..., RulePolicy],
 ):
     other_intent = "other"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
         - {other_intent}
         actions:
         - {UTTER_GREET_ACTION}
-        """)
+        """
+    )
     policy = policy_with_config({"enable_fallback_prediction": False})
     policy.train([GREET_RULE], domain)
     new_conversation = DialogueStateTracker.from_events(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2059'>tests/core/policies/test_rule_policy.py~L2059</a>
```diff
 def test_hide_rule_turn(policy: RulePolicy):
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2067'>tests/core/policies/test_rule_policy.py~L2067</a>
```diff
         actions:
         - {UTTER_GREET_ACTION}
         - {action_chitchat}
-        """)
+        """
+    )
     chitchat_story = TrackerWithCachedStates.from_events(
         "chitchat story",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2137'>tests/core/policies/test_rule_policy.py~L2137</a>
```diff
     some_slot_value = "value1"
     slot_which_is_also_in_story = "slot_which_is_also_in_story"
     some_other_slot_value = "value2"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {some_intent}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2154'>tests/core/policies/test_rule_policy.py~L2154</a>
```diff
             type: text
             mappings:
             - type: from_text
-        """)
+        """
+    )
 
     simple_rule = TrackerWithCachedStates.from_events(
         "simple rule with an action that sets 1 slot",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2262'>tests/core/policies/test_rule_policy.py~L2262</a>
```diff
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
     followup_on_chitchat = "followup_on_chitchat"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {chitchat}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2274'>tests/core/policies/test_rule_policy.py~L2274</a>
```diff
             type: bool
             mappings:
             - type: from_text
-        """)
+        """
+    )
     simple_rule_no_last_action_listen = TrackerWithCachedStates.from_events(
         "simple rule without action listen in the end",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2348'>tests/core/policies/test_rule_policy.py~L2348</a>
```diff
     activate_another_form = "activate_another_form"
     chitchat = "chitchat"
     action_chitchat = "action_chitchat"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2368'>tests/core/policies/test_rule_policy.py~L2368</a>
```diff
             required_slots: []
           {another_form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2454'>tests/core/policies/test_rule_policy.py~L2454</a>
```diff
 def test_do_not_hide_rule_turn_with_loops_in_stories(policy: RulePolicy):
     form_name = "some_form"
     activate_form = "activate_form"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {activate_form}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2466'>tests/core/policies/test_rule_policy.py~L2466</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
     form_activation_story = form_activation_rule.copy()
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2507'>tests/core/policies/test_rule_policy.py~L2507</a>
```diff
 def test_hide_rule_turn_with_loops_as_followup_action(policy: RulePolicy):
     form_name = "some_form"
     activate_form = "activate_form"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {GREET_INTENT_NAME}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2522'>tests/core/policies/test_rule_policy.py~L2522</a>
```diff
         forms:
           {form_name}:
             required_slots: []
-        """)
+        """
+    )
 
     form_activation_rule = _form_activation_rule(domain, form_name, activate_form)
     form_activation_story = form_activation_rule.copy()
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2609'>tests/core/policies/test_rule_policy.py~L2609</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """)
+        """
+    )
     rule = TrackerWithCachedStates.from_events(
         "conditioned on action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2652'>tests/core/policies/test_rule_policy.py~L2652</a>
```diff
     utter_1 = "utter_1"
     utter_2 = "utter_2"
     utter_3 = "utter_3"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2660'>tests/core/policies/test_rule_policy.py~L2660</a>
```diff
         - {utter_1}
         - {utter_2}
         - {utter_3}
-        """)
+        """
+    )
     rule = TrackerWithCachedStates.from_events(
         "action_listen after predictable action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2696'>tests/core/policies/test_rule_policy.py~L2696</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """)
+        """
+    )
     rule = TrackerWithCachedStates.from_events(
         "last prediction is action_listen",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2735'>tests/core/policies/test_rule_policy.py~L2735</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(f"""
+    domain = Domain.from_yaml(
+        f"""
         version: "{LATEST_TRAINING_DATA_FORMAT_VERSION}"
         intents:
         - {intent_1}
         actions:
         - {utter_1}
         - {utter_2}
-        """)
+        """
+    )
     rule = TrackerWithCachedStates.from_events(
         "conditioned on action",
         domain=domain,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/policies/test_rule_policy.py#L2776'>tests/core/policies/test_rule_policy.py~L2776</a>
```diff
     intent_1 = "intent_1"
     utter_1 = "utter_1"
     utter_2 = "utter_2"
-    domain = Domain.from_yaml(f""...*[Comment body truncated]*

---

_Review requested from @zanieb by @MichaReiser on 2024-01-25 08:28_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-25 08:39_

---

_Marked ready for review by @MichaReiser on 2024-01-25 08:39_

---

_@charliermarsh approved on 2024-01-25 17:40_

Looks great.

---

_@charliermarsh reviewed on 2024-01-25 17:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/parentheses.rs`:199 on 2024-01-25 17:41_

So this got much simpler because we no longer check if indenting would allow us to avoid line-length violations?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:199 on 2024-01-26 07:18_

Yes. Breaking only if the first line doesn't fit required a more complicated IR. Now it's simpler, because we always or never indent.

---

_@MichaReiser reviewed on 2024-01-26 07:18_

---

_Merged by @MichaReiser on 2024-01-26 07:18_

---

_Closed by @MichaReiser on 2024-01-26 07:18_

---

_Branch deleted on 2024-01-26 07:18_

---
