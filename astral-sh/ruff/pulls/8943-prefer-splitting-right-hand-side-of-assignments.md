```yaml
number: 8943
title: "`prefer_splitting_right_hand_side_of_assignments` preview style"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: prefer_splitting_right_hand_side_of_assignments
created_at: 2023-12-01T07:18:54Z
updated_at: 2023-12-13T03:43:25Z
url: https://github.com/astral-sh/ruff/pull/8943
synced_at: 2026-01-10T23:31:11Z
```

# `prefer_splitting_right_hand_side_of_assignments` preview style

---

_Pull request opened by @MichaReiser on 2023-12-01 07:18_

## Summary

This PR implements Black's `prefer_splitting_right_hand_side_of_assignments` preview style. 

The gist of the new style is to prefer breaking the value before breaking the target or type annotation of an assignment:

```python
aaaa["long_index"] = some_type
```

Before

```python
aaaa[
	"long_index"
] = some_type
```

New

```python
aaaa["long_index"] = (
	some_type
)
```

Closes https://github.com/astral-sh/ruff/issues/6975

### Details

It turned out that there are some more rules involved than just splitting the value before the targets.I For example:

* The first target never gets parenthesized, even if it is the only target
* If the target right before the value (or type annotations, or type parameters) split, avoid parenthesizing the value because it leads to unnecessary parentheses
* For call expression: Prefer breaking after the call expressions opening parentheses and only parenthesize the entire call expression if that's insufficient. 

I added extensive documentation to `FormatStatementLastExpression` 

https://github.com/astral-sh/ruff/blob/7a3c504ec8bbff62e839c078b3ef65b2b5539bcb/crates/ruff_python_formatter/src/statement/stmt_assign.rs#L140-L270

### Differences to Black

Black doesn't seem to implement this behavior for type alias statements. We do this to ensure all assignment-like statements are formatted the same. 

## Performance

The new right-to-left layout is more expensive than our existing layout because it requires using `BestFitting` (allocates, needs to try multiple different variants). The good news is that this layout is only necessary when the assignment has:

* The target or type annotation can split (e.g. subscription)
* multiple targets
* type parameters

This is rare in comparison to most assignments that are of the form `a = b` or `a: b = c`. 

I checked Codspeed and our micro benchmarks only regress by 1-2%

## Test Plan

I added new tests, reviewed the Black related preview style tests (that we now match except for comment handling). 

The poetry compatibility improves from 0.96208 to 0.96224.


---

_Comment by @MichaReiser on 2023-12-01 07:19_

Current dependencies on/for this PR:
* main
  * **PR #8920** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8943** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #9102** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8941** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8940** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment).

---

_Label `formatter` added by @MichaReiser on 2023-12-01 07:24_

---

_Label `preview` added by @MichaReiser on 2023-12-01 07:24_

---

_Comment by @github-actions[bot] on 2023-12-01 07:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+753 -797 lines in 101 files in 41 projects)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+10 -10 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/3feb3b2e778b5c41526d829f22a9001755fd776e/housewatch/clickhouse/backups.py#L36'>housewatch/clickhouse/backups.py~L36</a>
```diff
     for shard, node in nodes:
         params["shard"] = shard
         if base_backup:
-            query_settings[
-                "base_backup"
-            ] = f"S3('{base_backup}/{shard}', '{aws_key}', '{aws_secret}')"
+            query_settings["base_backup"] = (
+                f"S3('{base_backup}/{shard}', '{aws_key}', '{aws_secret}')"
+            )
         final_query = query % (params or {}) if substitute_params else query
         client = Client(
             host=node["host_address"],
```
<a href='https://github.com/PostHog/HouseWatch/blob/3feb3b2e778b5c41526d829f22a9001755fd776e/housewatch/clickhouse/backups.py#L123'>housewatch/clickhouse/backups.py~L123</a>
```diff
     TO S3('https://%(bucket)s.s3.amazonaws.com/%(path)s', '%(aws_key)s', '%(aws_secret)s')
     ASYNC"""
     if base_backup:
-        query_settings[
-            "base_backup"
-        ] = f"S3('{base_backup}', '{aws_key}', '{aws_secret}')"
+        query_settings["base_backup"] = (
+            f"S3('{base_backup}', '{aws_key}', '{aws_secret}')"
+        )
     return run_query(
         QUERY,
         {
```
<a href='https://github.com/PostHog/HouseWatch/blob/3feb3b2e778b5c41526d829f22a9001755fd776e/housewatch/clickhouse/backups.py#L178'>housewatch/clickhouse/backups.py~L178</a>
```diff
                 TO S3('https://%(bucket)s.s3.amazonaws.com/%(path)s', '%(aws_key)s', '%(aws_secret)s')
                 ASYNC"""
     if base_backup:
-        query_settings[
-            "base_backup"
-        ] = f"S3('{base_backup}', '{aws_key}', '{aws_secret}')"
+        query_settings["base_backup"] = (
+            f"S3('{base_backup}', '{aws_key}', '{aws_secret}')"
+        )
     return run_query(
         QUERY,
         {
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+107 -107 lines across 14 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/cli/utils.py#L132'>rasa/cli/utils.py~L132</a>
```diff
 
         # add random value for assistant id, overwrite config file
         time_format = "%Y%m%d-%H%M%S"
-        config_data[
-            ASSISTANT_ID_KEY
-        ] = f"{time.strftime(time_format)}-{randomname.get_name()}"
+        config_data[ASSISTANT_ID_KEY] = (
+            f"{time.strftime(time_format)}-{randomname.get_name()}"
+        )
 
         rasa.shared.utils.io.write_yaml(
             data=config_data, target=config_file, should_preserve_key_order=True
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L774'>rasa/core/policies/rule_policy.py~L774</a>
```diff
         trackers_as_actions = rule_trackers_as_actions + story_trackers_as_actions
 
         # negative rules are not anti-rules, they are auxiliary to actual rules
-        self.lookup[
-            RULES_FOR_LOOP_UNHAPPY_PATH
-        ] = self._create_loop_unhappy_lookup_from_states(
-            trackers_as_states, trackers_as_actions
+        self.lookup[RULES_FOR_LOOP_UNHAPPY_PATH] = (
+            self._create_loop_unhappy_lookup_from_states(
+                trackers_as_states, trackers_as_actions
+            )
         )
 
     def train(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1264'>rasa/core/policies/ted_policy.py~L1264</a>
```diff
             )
             self._prepare_encoding_layers(name)
 
-        self._tf_layers[
-            f"transformer.{DIALOGUE}"
-        ] = rasa_layers.prepare_transformer_layer(
-            attribute_name=DIALOGUE,
-            config=self.config,
-            num_layers=self.config[NUM_TRANSFORMER_LAYERS][DIALOGUE],
-            units=self.config[TRANSFORMER_SIZE][DIALOGUE],
-            drop_rate=self.config[DROP_RATE_DIALOGUE],
-            # use bidirectional transformer, because
-            # we will invert dialogue sequence so that the last turn is located
-            # at the first position and would always have
-            # exactly the same positional encoding
-            unidirectional=not self.max_history_featurizer_is_used,
+        self._tf_layers[f"transformer.{DIALOGUE}"] = (
+            rasa_layers.prepare_transformer_layer(
+                attribute_name=DIALOGUE,
+                config=self.config,
+                num_layers=self.config[NUM_TRANSFORMER_LAYERS][DIALOGUE],
+                units=self.config[TRANSFORMER_SIZE][DIALOGUE],
+                drop_rate=self.config[DROP_RATE_DIALOGUE],
+                # use bidirectional transformer, because
+                # we will invert dialogue sequence so that the last turn is located
+                # at the first position and would always have
+                # exactly the same positional encoding
+                unidirectional=not self.max_history_featurizer_is_used,
+            )
         )
 
         self._prepare_label_classification_layers(DIALOGUE)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1307'>rasa/core/policies/ted_policy.py~L1307</a>
```diff
         # Attributes with sequence-level features also have sentence-level features,
         # all these need to be combined and further processed.
         if attribute_name in SEQUENCE_FEATURES_TO_ENCODE:
-            self._tf_layers[
-                f"sequence_layer.{attribute_name}"
-            ] = rasa_layers.RasaSequenceLayer(
-                attribute_name, attribute_signature, config_to_use
+            self._tf_layers[f"sequence_layer.{attribute_name}"] = (
+                rasa_layers.RasaSequenceLayer(
+                    attribute_name, attribute_signature, config_to_use
+                )
             )
         # Attributes without sequence-level features require some actual feature
         # processing only if they have sentence-level features. Attributes with no
         # sequence- and sentence-level features (dialogue, entity_tags, label) are
         # skipped here.
         elif SENTENCE in attribute_signature:
-            self._tf_layers[
-                f"sparse_dense_concat_layer.{attribute_name}"
-            ] = rasa_layers.ConcatenateSparseDenseFeatures(
-                attribute=attribute_name,
-                feature_type=SENTENCE,
-                feature_type_signature=attribute_signature[SENTENCE],
-                config=config_to_use,
+            self._tf_layers[f"sparse_dense_concat_layer.{attribute_name}"] = (
+                rasa_layers.ConcatenateSparseDenseFeatures(
+                    attribute=attribute_name,
+                    feature_type=SENTENCE,
+                    feature_type_signature=attribute_signature[SENTENCE],
+                    config=config_to_use,
+                )
             )
 
     def _prepare_encoding_layers(self, name: Text) -> None:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/graph.py#L107'>rasa/engine/graph.py~L107</a>
```diff
         nodes = {}
         for node_name, serialized_node in serialized_graph_schema["nodes"].items():
             try:
-                serialized_node[
-                    "uses"
-                ] = rasa.shared.utils.common.class_from_module_path(
-                    serialized_node["uses"]
+                serialized_node["uses"] = (
+                    rasa.shared.utils.common.class_from_module_path(
+                        serialized_node["uses"]
+                    )
                 )
 
                 resource = serialized_node["resource"]
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L150'>rasa/engine/recipes/default_recipe.py~L150</a>
```diff
             else:
                 unique_types = set(component_types)
 
-            cls._registered_components[
-                registered_class.__name__
-            ] = cls.RegisteredComponent(
-                registered_class, unique_types, is_trainable, model_from
+            cls._registered_components[registered_class.__name__] = (
+                cls.RegisteredComponent(
+                    registered_class, unique_types, is_trainable, model_from
+                )
             )
             return registered_class
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/graph_components/validators/default_recipe_validator.py#L294'>rasa/graph_components/validators/default_recipe_validator.py~L294</a>
```diff
         Both of these look for the same entities based on the same training data
         leading to ambiguity in the results.
         """
-        extractors_in_configuration: Set[
-            Type[GraphComponent]
-        ] = self._component_types.intersection(TRAINABLE_EXTRACTORS)
+        extractors_in_configuration: Set[Type[GraphComponent]] = (
+            self._component_types.intersection(TRAINABLE_EXTRACTORS)
+        )
         if len(extractors_in_configuration) > 1:
             rasa.shared.utils.io.raise_warning(
                 f"You have defined multiple entity extractors that do the same job "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/classifiers/diet_classifier.py#L1446'>rasa/nlu/classifiers/diet_classifier.py~L1446</a>
```diff
         # everything using a transformer and optionally also do masked language
         # modeling.
         self.text_name = TEXT
-        self._tf_layers[
-            f"sequence_layer.{self.text_name}"
-        ] = rasa_layers.RasaSequenceLayer(
-            self.text_name, self.data_signature[self.text_name], self.config
+        self._tf_layers[f"sequence_layer.{self.text_name}"] = (
+            rasa_layers.RasaSequenceLayer(
+                self.text_name, self.data_signature[self.text_name], self.config
+            )
         )
         if self.config[MASKED_LM]:
             self._prepare_mask_lm_loss(self.text_name)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/classifiers/diet_classifier.py#L1468'>rasa/nlu/classifiers/diet_classifier.py~L1468</a>
```diff
                 DENSE_INPUT_DROPOUT: False,
             })
 
-            self._tf_layers[
-                f"feature_combining_layer.{self.label_name}"
-            ] = rasa_layers.RasaFeatureCombiningLayer(
-                self.label_name, self.label_signature[self.label_name], label_config
+            self._tf_layers[f"feature_combining_layer.{self.label_name}"] = (
+                rasa_layers.RasaFeatureCombiningLayer(
+                    self.label_name, self.label_signature[self.label_name], label_config
+                )
             )
 
             self._prepare_ffnn_layer(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L336'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L336</a>
```diff
 
                 token = tokens[absolute_position]
                 for feature_name in self._feature_config[window_position]:
-                    token_features[
-                        (window_position, feature_name)
-                    ] = self._extract_raw_features_from_token(
-                        token=token,
-                        feature_name=feature_name,
-                        token_position=absolute_position,
-                        num_tokens=len(tokens),
+                    token_features[(window_position, feature_name)] = (
+                        self._extract_raw_features_from_token(
+                            token=token,
+                            feature_name=feature_name,
+                            token_position=absolute_position,
+                            num_tokens=len(tokens),
+                        )
                     )
 
             sentence_features.append(token_features)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/selectors/response_selector.py#L430'>rasa/nlu/selectors/response_selector.py~L430</a>
```diff
         self, message: Message, prediction_dict: Dict[Text, Any], selector_key: Text
     ) -> None:
         message_selector_properties = message.get(RESPONSE_SELECTOR_PROPERTY_NAME, {})
-        message_selector_properties[
-            RESPONSE_SELECTOR_RETRIEVAL_INTENTS
-        ] = self.all_retrieval_intents
+        message_selector_properties[RESPONSE_SELECTOR_RETRIEVAL_INTENTS] = (
+            self.all_retrieval_intents
+        )
         message_selector_properties[selector_key] = prediction_dict
         message.set(
             RESPONSE_SELECTOR_PROPERTY_NAME,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/selectors/response_selector.py#L793'>rasa/nlu/selectors/response_selector.py~L793</a>
```diff
             (self.text_name, self.config),
             (self.label_name, label_config),
         ]:
-            self._tf_layers[
-                f"sequence_layer.{attribute}"
-            ] = rasa_layers.RasaSequenceLayer(
-                attribute, self.data_signature[attribute], config
+            self._tf_layers[f"sequence_layer.{attribute}"] = (
+                rasa_layers.RasaSequenceLayer(
+                    attribute, self.data_signature[attribute], config
+                )
             )
 
         if self.config[MASKED_LM]:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/core/domain.py#L1496'>rasa/shared/core/domain.py~L1496</a>
```diff
                 if not response_text or "\n" not in response_text:
                     continue
                 # Has new lines, use `LiteralScalarString`
-                final_responses[utter_action][i][
-                    KEY_RESPONSES_TEXT
-                ] = LiteralScalarString(response_text)
+                final_responses[utter_action][i][KEY_RESPONSES_TEXT] = (
+                    LiteralScalarString(response_text)
+                )
 
         return final_responses
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/nlu/training_data/formats/rasa_yaml.py#L529'>rasa/shared/nlu/training_data/formats/rasa_yaml.py~L529</a>
```diff
             )
 
             if examples_have_metadata or example_texts_have_escape_chars:
-                intent[
-                    key_examples
-                ] = RasaYAMLWriter._render_training_examples_as_objects(converted)
+                intent[key_examples] = (
+                    RasaYAMLWriter._render_training_examples_as_objects(converted)
+                )
             else:
                 intent[key_examples] = RasaYAMLWriter._render_training_examples_as_text(
                     converted
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/model_data.py#L735'>rasa/utils/tensorflow/model_data.py~L735</a>
```diff
         # if a label was skipped in current batch
         skipped = [False] * num_label_ids
 
-        new_data: DefaultDict[
-            Text, DefaultDict[Text, List[List[FeatureArray]]]
-        ] = defaultdict(lambda: defaultdict(list))
+        new_data: DefaultDict[Text, DefaultDict[Text, List[List[FeatureArray]]]] = (
+            defaultdict(lambda: defaultdict(list))
+        )
 
         while min(num_data_cycles) == 0:
             if shuffle:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/model_data.py#L888'>rasa/utils/tensorflow/model_data.py~L888</a>
```diff
         Returns:
             The test and train RasaModelData
         """
-        data_train: DefaultDict[
-            Text, DefaultDict[Text, List[FeatureArray]]
-        ] = defaultdict(lambda: defaultdict(list))
+        data_train: DefaultDict[Text, DefaultDict[Text, List[FeatureArray]]] = (
+            defaultdict(lambda: defaultdict(list))
+        )
         data_val: DefaultDict[Text, DefaultDict[Text, List[Any]]] = defaultdict(
             lambda: defaultdict(list)
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/models.py#L324'>rasa/utils/tensorflow/models.py~L324</a>
```diff
                 # We only need input, since output is always None and not
                 # consumed by our TF graphs.
                 batch_in = next(data_iterator)[0]
-                batch_out: Dict[
-                    Text, Union[np.ndarray, Dict[Text, Any]]
-                ] = self._rasa_predict(batch_in)
+                batch_out: Dict[Text, Union[np.ndarray, Dict[Text, Any]]] = (
+                    self._rasa_predict(batch_in)
+                )
                 if output_keys_expected:
                     batch_out = {
                         key: output
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/rasa_layers.py#L442'>rasa/utils/tensorflow/rasa_layers.py~L442</a>
```diff
         for feature_type, present in self._feature_types_present.items():
             if not present:
                 continue
-            self._tf_layers[
-                f"sparse_dense.{feature_type}"
-            ] = ConcatenateSparseDenseFeatures(
-                attribute=attribute,
-                feature_type=feature_type,
-                feature_type_signature=attribute_signature[feature_type],
-                config=config,
+            self._tf_layers[f"sparse_dense.{feature_type}"] = (
+                ConcatenateSparseDenseFeatures(
+                    attribute=attribute,
+                    feature_type=feature_type,
+                    feature_type_signature=attribute_signature[feature_type],
+                    config=config,
+                )
             )
 
     def _prepare_sequence_sentence_concat(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/rasa_layers.py#L851'>rasa/utils/tensorflow/rasa_layers.py~L851</a>
```diff
                 not signature.is_sparse for signature in attribute_signature[SEQUENCE]
             ])
             if not expect_dense_seq_features:
-                self._tf_layers[
-                    self.SPARSE_TO_DENSE_FOR_TOKEN_IDS
-                ] = layers.DenseForSparse(
-                    units=2,
-                    use_bias=False,
-                    trainable=False,
-                    name=f"{self.SPARSE_TO_DENSE_FOR_TOKEN_IDS}.{attribute}",
+                self._tf_layers[self.SPARSE_TO_DENSE_FOR_TOKEN_IDS] = (
+                    layers.DenseForSparse(
+                        units=2,
+                        use_bias=False,
+                        trainable=False,
+                        name=f"{self.SPARSE_TO_DENSE_FOR_TOKEN_IDS}.{attribute}",
+                    )
                 )
 
     def _calculate_output_units(
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+4 -4 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/058819dc4da790b9f4becad507df8cac68f70ffe/src/snowcli/app/commands_registration/command_plugins_loader.py#L78'>src/snowcli/app/commands_registration/command_plugins_loader.py~L78</a>
```diff
             )
             return None
         self._loaded_plugins[plugin_name] = loaded_plugin
-        self._loaded_command_paths[
-            loaded_plugin.command_spec.full_command_path
-        ] = loaded_plugin
+        self._loaded_command_paths[loaded_plugin.command_spec.full_command_path] = (
+            loaded_plugin
+        )
         return loaded_plugin
 
     def _load_plugin_spec(
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+27 -27 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/jobs/test_backfill_job.py#L875'>tests/jobs/test_backfill_job.py~L875</a>
```diff
         dag_maker.create_dagrun(state=None)
 
         executor = MockExecutor(parallelism=16)
-        executor.mock_task_results[
-            TaskInstanceKey(dag.dag_id, task1.task_id, DEFAULT_DATE, try_number=1)
-        ] = State.UP_FOR_RETRY
-        executor.mock_task_results[
-            TaskInstanceKey(dag.dag_id, task1.task_id, DEFAULT_DATE, try_number=2)
-        ] = State.UP_FOR_RETRY
+        executor.mock_task_results[TaskInstanceKey(dag.dag_id, task1.task_id, DEFAULT_DATE, try_number=1)] = (
+            State.UP_FOR_RETRY
+        )
+        executor.mock_task_results[TaskInstanceKey(dag.dag_id, task1.task_id, DEFAULT_DATE, try_number=2)] = (
+            State.UP_FOR_RETRY
+        )
         job = Job(executor=executor)
         job_runner = BackfillJobRunner(
             job=job,
```
<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/jobs/test_backfill_job.py#L903'>tests/jobs/test_backfill_job.py~L903</a>
```diff
         dr = dag_maker.create_dagrun(state=None)
 
         executor = MockExecutor(parallelism=16)
-        executor.mock_task_results[
-            TaskInstanceKey(dag.dag_id, task1.task_id, dr.run_id, try_number=1)
-        ] = State.UP_FOR_RETRY
+        executor.mock_task_results[TaskInstanceKey(dag.dag_id, task1.task_id, dr.run_id, try_number=1)] = (
+            State.UP_FOR_RETRY
+        )
         executor.mock_task_fail(dag.dag_id, task1.task_id, dr.run_id, try_number=2)
         job = Job(executor=executor)
         job_runner = BackfillJobRunner(
```
<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L856'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py~L856</a>
```diff
 
         os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.REGION_NAME}".upper()] = "us-west-1"
         os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CLUSTER}".upper()] = "some-cluster"
-        os.environ[
-            f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CONTAINER_NAME}".upper()
-        ] = "container-name"
-        os.environ[
-            f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.TASK_DEFINITION}".upper()
-        ] = "some-task-def"
+        os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CONTAINER_NAME}".upper()] = (
+            "container-name"
+        )
+        os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.TASK_DEFINITION}".upper()] = (
+            "some-task-def"
+        )
         os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.LAUNCH_TYPE}".upper()] = "FARGATE"
         os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.PLATFORM_VERSION}".upper()] = "LATEST"
         os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.ASSIGN_PUBLIC_IP}".upper()] = "False"
```
<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L872'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py~L872</a>
```diff
         assert raised.match("At least one subnet is required to run a task.")
 
     def test_config_defaults_are_applied(self, assign_subnets):
-        os.environ[
-            f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CONTAINER_NAME}".upper()
-        ] = "container-name"
+        os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CONTAINER_NAME}".upper()] = (
+            "container-name"
+        )
         from airflow.providers.amazon.aws.executors.ecs import ecs_executor_config
 
         task_kwargs = _recursive_flatten_dict(ecs_executor_config.build_task_kwargs())
```
<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L1078'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py~L1078</a>
```diff
 
         executor.ecs = ecs_mock
 
-        os.environ[
-            f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CHECK_HEALTH_ON_STARTUP}".upper()
-        ] = "False"
+        os.environ[f"AIRFLOW__{CONFIG_GROUP_NAME}__{AllEcsConfigKeys.CHECK_HEALTH_ON_STARTUP}".upper()] = (
+            "False"
+        )
 
         executor.start()
 
```
<a href='https://github.com/apache/airflow/blob/357355ac09b4741d621a5408d859b697a07b3ceb/tests/system/providers/papermill/conftest.py#L49'>tests/system/providers/papermill/conftest.py~L49</a>
```diff
 
 @pytest.fixture(scope="session", autouse=True)
 def airflow_conn(remote_kernel):
-    os.environ[
-        "AIRFLOW_CONN_JUPYTER_KERNEL_DEFAULT"
-    ] = '{"host": "localhost", "extra": {"shell_port": 60316} }'
+    os.environ["AIRFLOW_CONN_JUPYTER_KERNEL_DEFAULT"] = (
+        '{"host": "localhost", "extra": {"shell_port": 60316} }'
+    )
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+24 -24 lines across 6 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/samcli/commands/_utils/options.py#L748'>samcli/commands/_utils/options.py~L748</a>
```diff
     def hook_name_processer_wrapper(f):
         configuration_setup_params = ()
         configuration_setup_attrs = {}
-        configuration_setup_attrs[
-            "help"
-        ] = "This is a hidden click option whose callback function to run the provided hook package."
+        configuration_setup_attrs["help"] = (
+            "This is a hidden click option whose callback function to run the provided hook package."
+        )
         configuration_setup_attrs["is_eager"] = True
         configuration_setup_attrs["expose_value"] = False
         configuration_setup_attrs["hidden"] = True
```
<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L158'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py~L158</a>
```diff
     cfn_resource_update_call_back_function: Callable[[Dict, List[ReferenceType]], None]
     linking_exceptions: ResourcePairExceptions
     # function to extract the terraform destination value from the linking field value
-    tf_destination_value_extractor_from_link_field_value_function: Callable[
-        [str], str
-    ] = _default_tf_destination_value_id_extractor
+    tf_destination_value_extractor_from_link_field_value_function: Callable[[str], str] = (
+        _default_tf_destination_value_id_extractor
+    )
 
 
 class ResourceLinker:
```
<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/samcli/lib/list/endpoints/endpoints_producer.py#L469'>samcli/lib/list/endpoints/endpoints_producer.py~L469</a>
```diff
             resource.get(RESOURCE_TYPE, "") == AWS_APIGATEWAY_DOMAIN_NAME
             or resource.get(RESOURCE_TYPE, "") == AWS_APIGATEWAY_V2_DOMAIN_NAME
         ):
-            response_domain_dict[
-                resource.get(LOGICAL_RESOURCE_ID, "")
-            ] = f'https://{resource.get(PHYSICAL_RESOURCE_ID, "")}'
+            response_domain_dict[resource.get(LOGICAL_RESOURCE_ID, "")] = (
+                f'https://{resource.get(PHYSICAL_RESOURCE_ID, "")}'
+            )
     return response_domain_dict
 
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/tests/integration/buildcmd/test_build_terraform_applications.py#L80'>tests/integration/buildcmd/test_build_terraform_applications.py~L80</a>
```diff
             command_list_parameters["use_container"] = True
             command_list_parameters["build_image"] = self.docker_tag
             if self.override:
-                command_list_parameters[
-                    "container_env_var"
-                ] = "TF_VAR_HELLO_FUNCTION_SRC_CODE=./artifacts/HelloWorldFunction2"
+                command_list_parameters["container_env_var"] = (
+                    "TF_VAR_HELLO_FUNCTION_SRC_CODE=./artifacts/HelloWorldFunction2"
+                )
 
         environment_variables = os.environ.copy()
         if self.override:
```
<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/tests/unit/commands/_utils/test_template.py#L286'>tests/unit/commands/_utils/test_template.py~L286</a>
```diff
                 self.expected_result,
             )
 
-            expected_template_dict["Resources"]["MyResourceWithRelativePath"]["Metadata"][
-                "aws:asset:path"
-            ] = self.expected_result
+            expected_template_dict["Resources"]["MyResourceWithRelativePath"]["Metadata"]["aws:asset:path"] = (
+                self.expected_result
+            )
 
             result = _update_relative_paths(template_dict, self.src, self.dest)
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/aa132f863a6580ed2e279fabfc44f7fc6af7f2f1/tests/unit/commands/deploy/test_auth_utils.py#L56'>tests/unit/commands/deploy/test_auth_utils.py~L56</a>
```diff
         ]
         # setup authorizer and auth explicitly on the event properties.
         event_properties["Auth"] = {"ApiKeyRequired": True, "Authorizer": None}
-        self.template_dict["Resources"]["HelloWorldFunction"]["Properties"]["Events"]["HelloWorld"][
-            "Properties"
-        ] = event_properties
+        self.template_dict["Resources"]["HelloWorldFunction"]["Properties"]["Events"]["HelloWorld"]["Properties"] = (
+            event_properties
+        )
         _auth_per_resource = auth_per_resource([Stack("", "", "", {}, self.template_dict)])
         self.assertEqual(_auth_per_resource, [("HelloWorldFunction", True)])
 
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+7 -7 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/commaai/openpilot/blob/d2583d64f0ce997bdc7413f091f794ac3d704d4b/tools/replay/lib/ui_helpers.py#L246'>tools/replay/lib/ui_helpers.py~L246</a>
```diff
 def get_blank_lid_overlay(UP):
     lid_overlay = np.zeros((UP.lidar_x, UP.lidar_y), "uint8")
     # Draw the car.
-    lid_overlay[
-        int(round(UP.lidar_car_x - UP.car_hwidth)) : int(round(UP.lidar_car_x + UP.car_hwidth)), int(round(UP.lidar_car_y - UP.car_front))
-    ] = UP.car_color
-    lid_overlay[
-        int(round(UP.lidar_car_x - UP.car_hwidth)) : int(round(UP.lidar_car_x + UP.car_hwidth)), int(round(UP.lidar_car_y + UP.car_back))
-    ] = UP.car_color
+    lid_overlay[int(round(UP.lidar_car_x - UP.car_hwidth)) : int(round(UP.lidar_car_x + UP.car_hwidth)), int(round(UP.lidar_car_y - UP.car_front))] = (
+        UP.car_color
+    )
+    lid_overlay[int(round(UP.lidar_car_x - UP.car_hwidth)) : int(round(UP.lidar_car_x + UP.car_hwidth)), int(round(UP.lidar_car_y + UP.car_back))] = (
+        UP.car_color
+    )
     lid_overlay[int(round(UP.lidar_car_x - UP.car_hwidth)), int(round(UP.lidar_car_y - UP.car_front)) : int(round(UP.lidar_car_y + UP.car_back))] = UP.car_color
     lid_overlay[int(round(UP.lidar_car_x + UP.car_hwidth)), int(round(UP.lidar_car_y - UP.car_front)) : int(round(UP.lidar_car_y + UP.car_back))] = UP.car_color
     return lid_overlay
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+65 -79 lines across 8 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L1098'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py~L1098</a>
```diff
         durations = []
     else:
         load_external_resources()
-        (
-            X,
-            exceptions_log,
-            short_text_indices,
-            exception_indices,
-            timeout_indices,
-            durations,
-        ) = extract_features_from_all_incidents(incidents_df, label_fields)
+        X, exceptions_log, short_text_indices, exception_indices, timeout_indices, durations = (
+            extract_features_from_all_incidents(incidents_df, label_fields)
+        )
 
     return {
         "X": X,
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/ExportIndicators/Integrations/ExportIndicators/ExportIndicators.py#L806'>Packs/ExportIndicators/Integrations/ExportIndicators/ExportIndicators.py~L806</a>
```diff
             ],
         )
         resp.cache_control.max_age = max_age
-        resp.cache_control[
-            "stale-if-error"
-        ] = "600"  # number of seconds we are willing to serve stale content when there is an error
+        resp.cache_control["stale-if-error"] = (
+            "600"  # number of seconds we are willing to serve stale content when there is an error
+        )
         return resp
 
     except Exception:
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory.py#L2291'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory.py~L2291</a>
```diff
         return [], curatedrule_detection_to_process, curatedrule_detection_to_pull, pending_curatedrule_id, simple_backoff_rules
 
     # get curated rule detections using API call.
-    (
-        curatedrule_detection_to_process,
-        curatedrule_detection_to_pull,
-        pending_curatedrule_id,
-        simple_backoff_rules,
-    ) = get_max_fetch_curatedrule_detections(
-        client_obj,
-        start_time,
-        end_time,
-        max_fetch,
-        curatedrule_detection_to_process,
-        curatedrule_detection_to_pull,
-        pending_curatedrule_id,
-        alert_state,
-        simple_backoff_rules,
-        fetch_detection_by_list_basis,
+    (curatedrule_detection_to_process, curatedrule_detection_to_pull, pending_curatedrule_id, simple_backoff_rules) = (
+        get_max_fetch_curatedrule_detections(
+            client_obj,
+            start_time,
+            end_time,
+            max_fetch,
+            curatedrule_detection_to_process,
+            curatedrule_detection_to_pull,
+            pending_curatedrule_id,
+            alert_state,
+            simple_backoff_rules,
+            fetch_detection_by_list_basis,
+        )
     )
 
     if len(curatedrule_detection_to_process) > max_fetch:
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3063'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py~L3063</a>
```diff
     detection_to_pull = {"rule_id": "rule_1", "next_page_token": "next_page_token"}
     simple_backoff_rules = {}
     for _ in range(93):
-        (
-            detection_incidents,
-            detection_to_pull,
-            pending_rule_or_version_id,
-            simple_backoff_rules,
-        ) = get_max_fetch_curatedrule_detections(
-            client,
-            "st_dummy",
-            "et_dummy",
-            5,
-            [],
-            detection_to_pull,
-            pending_rule_or_version_id,
-            "",
-            simple_backoff_rules,
-            "CREATED_TIME",
+        detection_incidents, detection_to_pull, pending_rule_or_version_id, simple_backoff_rules = (
+            get_max_fetch_curatedrule_detections(
+                client,
+                "st_dummy",
+                "et_dummy",
+                5,
+                [],
+                detection_to_pull,
+                pending_rule_or_version_id,
+                "",
+                simple_backoff_rules,
+                "CREATED_TIME",
+            )
         )
 
     assert client.http_client.request.call_count == 93
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3115'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py~L3115</a>
```diff
 
     simple_backoff_rules = {}
     for _ in range(5):
-        (
-            detection_incidents,
-            detection_to_pull,
-            pending_rule_or_version_id,
-            simple_backoff_rules,
-        ) = get_max_fetch_curatedrule_detections(
-            client,
-            "st_dummy",
-            "et_dummy",
-            15,
-            [],
-            detection_to_pull,
-            pending_rule_or_version_id,
-            "",
-            simple_backoff_rules,
-            "CREATED_TIME",
+        detection_incidents, detection_to_pull, pending_rule_or_version_id, simple_backoff_rules = (
+            get_max_fetch_curatedrule_detections(
+                client,
+                "st_dummy",
+                "et_dummy",
+                15,
+                [],
+                detection_to_pull,
+                pending_rule_or_version_id,
+                "",
+                simple_backoff_rules,
+                "CREATED_TIME",
+            )
         )
 
 
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/HealthCheck/Scripts/HealthCheckSystemDiagnostics/HealthCheckSystemDiagnostics.py#L78'>Packs/HealthCheck/Scripts/HealthCheckSystemDiagnostics/HealthCheckSystemDiagnostics.py~L78</a>
```diff
         elif dataSource == "bigTasks":
             taskId = re.match(r"(?P<incidentid>\d+)##(?P<taskid>[\d+])##(?P<pbiteration>-\d+|\d+)", entry["taskId"])
             if taskId is not None:
-                newEntry[
-                    "details"
-                ] = f"Playbook:{entry['playbookName']},\n TaskName:{entry['taskName']},\n TaskID:{taskId['taskid']}"
+                newEntry["details"] = (
+                    f"Playbook:{entry['playbookName']},\n TaskName:{entry['taskName']},\n TaskID:{taskId['taskid']}"
+                )
                 newEntry["size"] = FormatSize(entry["taskSize"])
                 newEntry["incidentid"] = entry["investigationId"]
                 newFormat.append(newEntry)
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/PAN-OS/Integrations/Panorama/Panorama.py#L10744'>Packs/PAN-OS/Integrations/Panorama/Panorama.py~L10744</a>
```diff
         """
         result = []
         if style == "device group":
-            commit_groups: Union[
-                List[DeviceGroupInformation], List[TemplateStackInformation]
-            ] = PanoramaCommand.get_device_groups(topology, resolve_host_id(device))
+            commit_groups: Union[List[DeviceGroupInformation], List[TemplateStackInformation]] = (
+                PanoramaCommand.get_device_groups(topology, resolve_host_id(device))
+            )
             commit_group_names = set([x.name for x in commit_groups])
         elif style == "template stack":
             commit_groups = PanoramaCommand.get_template_stacks(topology, resolve_host_id(device))
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L586'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py~L586</a>
```diff
                     elif isinstance(sub_value, dict):
                         capitalized_output[capitalize(field)][capitalize(sub_field)] = {}
                         for sub_sub_field, sub_sub_value in sub_value.items():
-                            capitalized_output[capitalize(field)][capitalize(sub_field)][
-                                capitalize(sub_sub_field)
-                            ] = sub_sub_value  # Support up to dict[x: dict[y: dict]]
+                            capitalized_output[capitalize(field)][capitalize(sub_field)][capitalize(sub_sub_field)] = (
+                                sub_sub_value  # Support up to dict[x: dict[y: dict]]
+                            )
         capitalized_outputs.append(capitalized_output)
 
     return capitalized_outputs
```
<a href='https://github.com/demisto/content/blob/7f7b4e934814b8fcc8e17e8ff13201d5fdd9846a/Tests/Marketplace/upload_packs.py#L1158'>Tests/Marketplace/upload_packs.py~L1158</a>
```diff
         f'{GCPConfig.versions_metadata_contents["version_map"][override_corepacks_server_version]["file_version"]} to'
         f'{override_corepacks_file_version}'
     )
-    GCPConfig.versions_metadata_contents["version_map"][override_corepacks_server_version][
-        "file_version"
-    ] = override_corepacks_file_version
+    GCPConfig.versions_metadata_contents["version_map"][override_corepacks_server_version]["file_version"] = (
+        override_corepacks_file_version
+    )
 
 
 def upload_server_versions_metadata(artifacts_dir: str):
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/tests/test_sync_module.py#L32'>tests/test_sync_module.py~L32</a>
```diff
         self.blink: Blink = Blink(motion_interval=0, session=mock.AsyncMock())
         self.blink.last_refresh = 0
         self.blink.urls = BlinkURLHandler("test")
-        self.blink.sync["test"]: (BlinkSyncModule) = BlinkSyncModule(
+        self.blink.sync["test"]: BlinkSyncModule = BlinkSyncModule(
             self.blink, "test", "1234", []
         )
         self.blink.sync["test"].network_info = {"network": {"armed": True}}
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+4 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/719418af509ec2ff8000d12e54f7fc2a0f3f7878/latch_cli/centromere/ctx.py#L283'>latch_cli/centromere/ctx.py~L283</a>
```diff
                 self.public_key = generate_temporary_ssh_credentials(self.ssh_key_path)
 
                 if use_new_centromere:
-                    (
-                        self.internal_ip,
-                        self.username,
-                    ) = self.provision_register_deployment()
+                    self.internal_ip, self.username = (
+                        self.provision_register_deployment()
+                    )
                 else:
                     self.internal_ip, self.username = self.get_old_centromere_info()
 
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+39 -39 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/langchain/api_request_parallel_processor.py#L172'>mlflow/langchain/api_request_parallel_processor.py~L172</a>
```diff
             status_tracker.complete_task(success=True)
             self.results.append((self.index, response))
         except Exception as e:
-            self.errors[
-                self.index
-            ] = f"error: {e!r} {traceback.format_exc()}\n request payload: {self.request_json}"
+            self.errors[self.index] = (
+                f"error: {e!r} {traceback.format_exc()}\n request payload: {self.request_json}"
+            )
             status_tracker.increment_num_api_errors()
             status_tracker.complete_task(success=False)
 
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/pyspark/ml/__init__.py#L950'>mlflow/pyspark/ml/__init__.py~L950</a>
```diff
             )
             artifact_dict[param_search_estimator_name] = {}
 
-            artifact_dict[param_search_estimator_name][
-                "tuning_parameter_map_list"
-            ] = _get_tuning_param_maps(
-                param_search_estimator, autologging_metadata.uid_to_indexed_name_map
+            artifact_dict[param_search_estimator_name]["tuning_parameter_map_list"] = (
+                _get_tuning_param_maps(
+                    param_search_estimator, autologging_metadata.uid_to_indexed_name_map
+                )
             )
 
-            artifact_dict[param_search_estimator_name][
-                "tuned_estimator_parameter_map"
-            ] = _get_instance_param_map_recursively(
-                param_search_estimator.getEstimator(),
-                1,
-                autologging_metadata.uid_to_indexed_name_map,
+            artifact_dict[param_search_estimator_name]["tuned_estimator_parameter_map"] = (
+                _get_instance_param_map_recursively(
+                    param_search_estimator.getEstimator(),
+                    1,
+                    autologging_metadata.uid_to_indexed_name_map,
+                )
             )
 
         if artifact_dict:
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/server/auth/__init__.py#L569'>mlflow/server/auth/__init__.py~L569</a>
```diff
         len(response_message.registered_models) < request_message.max_results
         and response_message.next_page_token != ""
     ):
-        refetched: PagedList[
-            RegisteredModel
-        ] = _get_model_registry_store().search_registered_models(
-            filter_string=request_message.filter,
-            max_results=request_message.max_results,
-            order_by=request_message.order_by,
-            page_token=response_message.next_page_token,
+        refetched: PagedList[RegisteredModel] = (
+            _get_model_registry_store().search_registered_models(
+                filter_string=request_message.filter,
+                max_results=request_message.max_results,
+                order_by=request_message.order_by,
+                page_token=response_message.next_page_token,
+            )
         )
         refetched = refetched[
             : request_message.max_results - len(response_message.registered_models)
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/store/tracking/sqlalchemy_store.py#L145'>mlflow/store/tracking/sqlalchemy_store.py~L145</a>
```diff
                 # inefficiency from multiple threads waiting for the lock to check for engine
                 # existence if it has already been created.
                 if db_uri not in SqlAlchemyStore._db_uri_sql_alchemy_engine_map:
-                    SqlAlchemyStore._db_uri_sql_alchemy_engine_map[
-                        db_uri
-                    ] = mlflow.store.db.utils.create_sqlalchemy_engine_with_retry(db_uri)
+                    SqlAlchemyStore._db_uri_sql_alchemy_engine_map[db_uri] = (
+                        mlflow.store.db.utils.create_sqlalchemy_engine_with_retry(db_uri)
+                    )
         self.engine = SqlAlchemyStore._db_uri_sql_alchemy_engine_map[db_uri]
         # On a completely fresh MLflow installation against an empty database (verify database
         # emptiness by checking that 'experiments' etc aren't in the list of table names), run all
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/store/tracking/sqlalchemy_store.py#L1412'>mlflow/store/tracking/sqlalchemy_store.py~L1412</a>
```diff
             )
             dataset_uuids = {}
             for existing_dataset in existing_datasets:
-                dataset_uuids[
-                    (existing_dataset.name, existing_dataset.digest)
-                ] = existing_dataset.dataset_uuid
+                dataset_uuids[(existing_dataset.name, existing_dataset.digest)] = (
+                    existing_dataset.dataset_uuid
+                )
 
             # collect all objects to write to DB in a single list
             objs_to_write = []
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/store/tracking/sqlalchemy_store.py#L1423'>mlflow/store/tracking/sqlalchemy_store.py~L1423</a>
```diff
             for dataset_input in dataset_inputs:
                 if (dataset_input.dataset.name, dataset_input.dataset.digest) not in dataset_uuids:
                     new_dataset_uuid = uuid.uuid4().hex
-                    dataset_uuids[
-                        (dataset_input.dataset.name, dataset_input.dataset.digest)
-                    ] = new_dataset_uuid
+                    dataset_uuids[(dataset_input.dataset.name, dataset_input.dataset.digest)] = (
+                        new_dataset_uuid
+                    )
                     objs_to_write.append(
                         SqlDataset(
                             dataset_uuid=new_dataset_uuid,
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/store/tracking/sqlalchemy_store.py#L1451'>mlflow/store/tracking/sqlalchemy_store.py~L1451</a>
```diff
             )
             input_uuids = {}
             for existing_input in existing_inputs:
-                input_uuids[
-                    (existing_input.source_id, existing_input.destination_id)
-                ] = existing_input.input_uuid
+                input_uuids[(existing_input.source_id, existing_input.destination_id)] = (
+                    existing_input.input_uuid
+                )
 
             # add input edges to objs_to_write
             for dataset_input in dataset_inputs:
```
<a href='https://github.com/mlflow/mlflow/blob/f0b4a3e965fd570440ba3e36fa796c1cce86cc8c/mlflow/store/tracking/sqlalchemy_store.py#L1462'>mlflow/store/tracking/sqlalchemy_store.py~L1462</a>
```diff
                 ]
                 if (dataset_uuid, run_id) not in input_uuids:
                     new_input_uuid = uuid.uuid4().hex
-                    input_uuids[
-                        (dataset_input.dataset.name, dataset_input.dataset.digest)
-                    ] = new_input_uuid
+                    input_uuids[(dataset_input.dataset.name, dataset_input.dataset.digest)] = (
+                        new_input_uuid
+                    )
                     objs_to_write.append(
                         SqlInput(
                             input_uuid=new_input_uuid,
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+31 -31 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/core/ops/docstrings.py#L420'>pandas/core/ops/docstrings.py~L420</a>
```diff
     if reverse_op is not None:
         _op_descriptions[reverse_op] = _op_descriptions[key].copy()
         _op_descriptions[reverse_op]["reverse"] = key
-        _op_descriptions[key][
-            "see_also_desc"
-        ] = f"Reverse of the {_op_descriptions[key]['desc']} operator, {_py_num_ref}"
-        _op_descriptions[reverse_op][
-            "see_also_desc"
-        ] = f"Element-wise {_op_descriptions[key]['desc']}, {_py_num_ref}"
+        _op_descriptions[key]["see_also_desc"] = (
+            f"Reverse of the {_op_descriptions[key]['desc']} operator, {_py_num_ref}"
+        )
+        _op_descriptions[reverse_op]["see_also_desc"] = (
+            f"Element-wise {_op_descriptions[key]['desc']}, {_py_num_ref}"
+        )
 
 _flex_doc_SERIES = """
 Return {desc} of series and other, element-wise (binary operator `{op_name}`).
```
<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/core/reshape/melt.py#L122'>pandas/core/reshape/melt.py~L122</a>
```diff
     if frame.shape[1] > 0 and not any(
         not isinstance(dt, np.dtype) and dt._supports_2d for dt in frame.dtypes
     ):
-        mdata[value_name] = concat([
-            frame.iloc[:, i] for i in range(frame.shape[1])
-        ]).values
+        mdata[value_name] = (
+            concat([frame.iloc[:, i] for i in range(frame.shape[1])]).values
+        )
     else:
         mdata[value_name] = frame._values.ravel("F")
     for i, col in enumerate(var_name):
```
<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/io/formats/style_render.py#L314'>pandas/io/formats/style_render.py~L314</a>
```diff
             max_cols,
         )
 
-        self.cellstyle_map_columns: DefaultDict[
-            tuple[CSSPair, ...], list[str]
-        ] = defaultdict(list)
+        self.cellstyle_map_columns: DefaultDict[tuple[CSSPair, ...], list[str]] = (
+            defaultdict(list)
+        )
         head = self._translate_header(sparse_cols, max_cols)
         d.update({"head": head})
 
```
<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/io/formats/style_render.py#L329'>pandas/io/formats/style_render.py~L329</a>
```diff
         self.cellstyle_map: DefaultDict[tuple[CSSPair, ...], list[str]] = defaultdict(
             list
         )
-        self.cellstyle_map_index: DefaultDict[
-            tuple[CSSPair, ...], list[str]
-        ] = defaultdict(list)
+        self.cellstyle_map_index: DefaultDict[tuple[CSSPair, ...], list[str]] = (
+            defaultdict(list)
+        )
         body: list = self._translate_body(idx_lengths, max_rows, max_cols)
         d.update({"body": body})
 
```
<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/io/formats/style_render.py#L776'>pandas/io/formats/style_render.py~L776</a>
```diff
             )
 
             if self.cell_ids:
-                header_element[
-                    "id"
-                ] = f"{self.css['level']}{c}_{self.css['row']}{r}"  # id is given
+                header_element["id"] = (
+                    f"{self.css['level']}{c}_{self.css['row']}{r}"  # id is given
+                )
             if (
                 header_element_visible
                 and (r, c) in self.ctx_index
```
<a href='https://github.com/pandas-dev/pandas/blob/14cf864c6e97269f6adc0e1146747aa9fd7234f1/pandas/tests/io/excel/test_writers.py#L1226'>pandas/tests/io/excel/test_writers.py~L1226</a>
```diff
         }
 
         if PY310:
-            msgs[
-                "openpyxl"
-            ] = "Workbook.__init__() got an unexpected keyword argument 'foo'"
-            msgs[
-                "xlsxwriter"
-            ] = "Workbook.__init__() got an unexpected keyword argument 'foo'"
+            msgs["openpyxl"] = (
+                "Workbook.__init__() got an unexpected keyword argument 'foo'"
+            )
+            msgs["xlsxwriter"] = (
+                "Workbook.__init__() got an unexpected keyword argument 'foo'"
+            )
 
         # Handle change in error message for openpyxl (write and append mode)
         if engine == "openpyxl" and not os.path.exists(path):
-            msgs[
-                "openpyxl"
-            ] = r"load_workbook() got an unexpected keyword argument 'foo'"
+            msgs["openpyxl"] = (
+                r"load_workbook() got an unexpected keyword argument 'foo'"
+            )
 
         with pytest.raises(TypeError, match=re.escape(msgs[engine])):
             df.to_excel(
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+138 -138 lines across 21 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/_internal/pydantic/annotations/pendulum.py#L13'>src/prefect/_internal/pydantic/annotations/pendulum.py~L13</a>
```diff
 
 
 class _PendulumDateTimeAnnotation:
-    _pendulum_type: t.Type[
-        t.Union[pendulum.DateTime, pendulum.Date, pendulum.Time]
-    ] = pendulum.DateTime
+    _pendulum_type: t.Type[t.Union[pendulum.DateTime, pendulum.Date, pendulum.Time]] = (
+        pendulum.DateTime
+    )
 
     _pendulum_types_to_schemas = {
         pendulum.DateTime: core_schema.datetime_schema(),
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/_vendor/fastapi/routing.py#L408'>src/prefect/_vendor/fastapi/routing.py~L408</a>
```diff
             methods = ["GET"]
         self.methods: Set[str] = {method.upper() for method in methods}
         if isinstance(generate_unique_id_function, DefaultPlaceholder):
-            current_generate_unique_id: Callable[
-                ["APIRoute"], str
-            ] = generate_unique_id_function.value
+            current_generate_unique_id: Callable[["APIRoute"], str] = (
+                generate_unique_id_function.value
+            )
         else:
             current_generate_unique_id = generate_unique_id_function
         self.unique_id = self.operation_id or current_generate_unique_id(self)
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/_vendor/fastapi/routing.py#L433'>src/prefect/_vendor/fastapi/routing.py~L433</a>
```diff
             # would pass the validation and be returned as is.
             # By being a new field, no inheritance will be passed as is. A new model
             # will be always created.
-            self.secure_cloned_response_field: Optional[
-                ModelField
-            ] = create_cloned_field(self.response_field)
+            self.secure_cloned_response_field: Optional[ModelField] = (
+                create_cloned_field(self.response_field)
+            )
         else:
             self.response_field = None  # type: ignore
             self.secure_cloned_response_field = None
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/_vendor/fastapi/utils.py#L39'>src/prefect/_vendor/fastapi/utils.py~L39</a>
```diff
     from .routing import APIRoute
 
 # Cache for `create_cloned_field`
-_CLONED_TYPES_CACHE: MutableMapping[
-    Type[BaseModel], Type[BaseModel]
-] = WeakKeyDictionary()
+_CLONED_TYPES_CACHE: MutableMapping[Type[BaseModel], Type[BaseModel]] = (
+    WeakKeyDictionary()
+)
 
 
 def is_body_allowed_for_status_code(status_code: Union[int, str, None]) -> bool:
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/blocks/core.py#L257'>src/prefect/blocks/core.py~L257</a>
```diff
                                     type_._to_block_schema_reference_dict(),
                                 ]
                             else:
-                                refs[
-                                    field.name
-                                ] = type_._to_block_schema_reference_dict()
+                                refs[field.name] = (
+                                    type_._to_block_schema_reference_dict()
+                                )
 
     def __init__(self, *args, **kwargs):
         super().__init__(*args, **kwargs)
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/blocks/notifications.py#L24'>src/prefect/blocks/notifications.py~L24</a>
```diff
     An abstract class for sending notifications using Apprise.
     """
 
-    notify_type: Literal[
-        "prefect_default", "info", "success", "warning", "failure"
-    ] = Field(
-        default=PREFECT_NOTIFY_TYPE_DEFAULT,
-        description=(
-            "The type of notification being performed; the prefect_default "
-            "is a plain notification that does not attach an image."
-        ),
+    notify_type: Literal["prefect_default", "info", "success", "warning", "failure"] = (
+        Field(
+            default=PREFECT_NOTIFY_TYPE_DEFAULT,
+            description=(
+                "The type of notification being performed; the prefect_default "
+                "is a plain notification that does not attach an image."
+            ),
+        )
     )
 
     def __init__(self, *args, **kwargs):
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/cli/_prompts.py#L482'>src/prefect/cli/_prompts.py~L482</a>
```diff
                 import prefect_docker
 
             credentials_block = prefect_docker.DockerRegistryCredentials
-            push_step[
-                "credentials"
-            ] = "{{ prefect_docker.docker-registry-credentials.docker_registry_creds_name }}"
+            push_step["credentials"] = (
+                "{{ prefect_docker.docker-registry-credentials.docker_registry_creds_name }}"
+            )
         else:
             credentials_block = DockerRegistry
-            push_step[
-                "credentials"
-            ] = "{{ prefect.docker-registry.docker_registry_creds_name }}"
+            push_step["credentials"] = (
+                "{{ prefect.docker-registry.docker_registry_creds_name }}"
+            )
         docker_registry_creds_name = f"deployment-{slugify(deployment_config['name'])}-{slugify(deployment_config['work_pool']['name'])}-registry-creds"
         create_new_block = False
         try:
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/context.py#L137'>src/prefect/context.py~L137</a>
```diff
     )
 
     # Failures will be a tuple of (exception, instance, args, kwargs)
-    _instance_init_failures: Dict[
-        Type[T], List[Tuple[Exception, T, Tuple, Dict]]
-    ] = PrivateAttr(default_factory=lambda: defaultdict(list))
+    _instance_init_failures: Dict[Type[T], List[Tuple[Exception, T, Tuple, Dict]]] = (
+        PrivateAttr(default_factory=lambda: defaultdict(list))
+    )
 
     block_code_execution: bool = False
     capture_failures: bool = False
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/deployments/deployments.py#L440'>src/prefect/deployments/deployments.py~L440</a>
```diff
             )
         )
         if all_fields["storage"]:
-            all_fields["storage"][
-                "_block_type_slug"
-            ] = self.storage.get_block_type_slug()
+            all_fields["storage"]["_block_type_slug"] = (
+                self.storage.get_block_type_slug()
+            )
         if all_fields["infrastructure"]:
-            all_fields["infrastructure"][
-                "_block_type_slug"
-            ] = self.infrastructure.get_block_type_slug()
+            all_fields["infrastructure"]["_block_type_slug"] = (
+                self.infrastructure.get_block_type_slug()
+            )
         return all_fields
 
     # top level metadata
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/filesystems.py#L694'>src/prefect/filesystems.py~L694</a>
```diff
     def filesystem(self) -> RemoteFileSystem:
         settings = {}
         if self.azure_storage_connection_string:
-            settings[
-                "connection_string"
-            ] = self.azure_storage_connection_string.get_secret_value()
+            settings["connection_string"] = (
+                self.azure_storage_connection_string.get_secret_value()
+            )
         if self.azure_storage_account_name:
-            settings[
-                "account_name"
-            ] = self.azure_storage_account_name.get_secret_value()
+            settings["account_name"] = (
+                self.azure_storage_account_name.get_secret_value()
+            )
         if self.azure_storage_account_key:
             settings["account_key"] = self.azure_storage_account_key.get_secret_value()
         if self.azure_storage_tenant_id:
```
<a href='https://github.com/prefecthq/prefect/blob/8f8098a543627e53f8a5228a6f972c24ea710322/src/prefect/filesystems.py#L708'>src/prefect/filesystems.py~L708</a>
```diff
         if self.azure_storage_client_id:
             settings["client_id"] = self.azure_storage_client_id.get_secret_value()
         if self.azure_storage_client_secret:
-            settings[
-                "client_secret"
-            ] = self.azure_storage_client_secret.get_secret_value()
+            settings["client_secret"] = (
+                self.azure_storage_client_secret.get_secret_value()
+            )
         settings["anon"] = self.azure_storage_anon
         self._remote_file_system = RemoteFileSystem(
             ba...*[Comment body truncated]*

---

_Renamed from "WIP: `prefer_splitting_right_hand_side_of_assignments` preview style" to "`prefer_splitting_right_hand_side_of_assignments` preview style" by @MichaReiser on 2023-12-11 08:52_

---

_Comment by @MichaReiser on 2023-12-11 09:40_

I like the changes that I see in the ecosystem check

---

_Marked ready for review by @MichaReiser on 2023-12-11 09:40_

---

_Review requested from @konstin by @MichaReiser on 2023-12-11 09:40_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-11 09:40_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/preview.rs`:21 on 2023-12-11 11:52_

I missed that previously, but do we also want to do an enum here, and then have `is_enabled(PreviewStyles::PreferSplittingRhsOfAssignments)`?

---

_@konstin approved on 2023-12-11 12:23_

Looks much better!

---

_@MichaReiser reviewed on 2023-12-12 02:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:21 on 2023-12-12 02:44_

I don't have an opinion. We could use `f.context().is_enabled(PreviewStyle::LongName)`, which may be easier to document and add. For me, any approach that allows identifying the call sites easily works for me.

---

_Merged by @MichaReiser on 2023-12-13 03:43_

---

_Closed by @MichaReiser on 2023-12-13 03:43_

---

_Branch deleted on 2023-12-13 03:43_

---
