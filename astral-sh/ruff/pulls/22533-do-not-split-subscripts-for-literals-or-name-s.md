```yaml
number: 22533
title: "Do not split subscripts for literals or `Name`s"
type: pull_request
state: open
author: dylwil3
labels:
  - formatter
  - preview
assignees: []
draft: true
base: main
head: subscript-split
created_at: 2026-01-12T15:03:18Z
updated_at: 2026-01-12T15:16:14Z
url: https://github.com/astral-sh/ruff/pull/22533
synced_at: 2026-01-12T15:57:52Z
```

# Do not split subscripts for literals or `Name`s

---

_@dylwil3_

WIP experiment


---

_Label `formatter` added by @dylwil3 on 2026-01-12 15:03_

---

_Label `preview` added by @dylwil3 on 2026-01-12 15:03_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 15:16_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+1226 -1500 lines in 201 files in 29 projects; 26 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+6 -6 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/commands/flag_converter.py#L288'>disnake/ext/commands/flag_converter.py~L288</a>
```diff
                 flags.update(base.__dict__["__commands_flags__"])
                 aliases.update(base.__dict__["__commands_flag_aliases__"])
                 if case_insensitive is MISSING:
-                    attrs["__commands_flag_case_insensitive__"] = base.__dict__[
-                        "__commands_flag_case_insensitive__"
-                    ]
+                    attrs["__commands_flag_case_insensitive__"] = (
+                        base.__dict__["__commands_flag_case_insensitive__"]
+                    )
                 if delimiter is MISSING:
-                    attrs["__commands_flag_delimiter__"] = base.__dict__[
-                        "__commands_flag_delimiter__"
-                    ]
+                    attrs["__commands_flag_delimiter__"] = (
+                        base.__dict__["__commands_flag_delimiter__"]
+                    )
                 if prefix is MISSING:
                     attrs["__commands_flag_prefix__"] = base.__dict__["__commands_flag_prefix__"]
 
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+150 -172 lines across 14 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/actions/action.py#L422'>rasa/core/actions/action.py~L422</a>
```diff
         if RESPONSE_SELECTOR_PROPERTY_NAME not in latest_message.parse_data:
             return None
 
-        response_selector_properties = latest_message.parse_data[
-            RESPONSE_SELECTOR_PROPERTY_NAME  # type: ignore[literal-required]
-        ]
+        response_selector_properties = (
+            latest_message.parse_data[RESPONSE_SELECTOR_PROPERTY_NAME]  # type: ignore[literal-required]
+        )
 
         if (
             self.intent_name_from_action(self.action_name)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/actions/action.py#L437'>rasa/core/actions/action.py~L437</a>
```diff
             return None
 
         selected = response_selector_properties[query_key]
-        full_retrieval_utter_action = selected[RESPONSE_SELECTOR_PREDICTION_KEY][
-            RESPONSE_SELECTOR_UTTER_ACTION_KEY
-        ]
+        full_retrieval_utter_action = selected[RESPONSE_SELECTOR_PREDICTION_KEY][RESPONSE_SELECTOR_UTTER_ACTION_KEY]
         return full_retrieval_utter_action
 
     async def run(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/actions/action.py#L456'>rasa/core/actions/action.py~L456</a>
```diff
         if latest_message is None:
             return []
 
-        response_selector_properties = latest_message.parse_data[
-            RESPONSE_SELECTOR_PROPERTY_NAME  # type: ignore[literal-required]
-        ]
+        response_selector_properties = (
+            latest_message.parse_data[RESPONSE_SELECTOR_PROPERTY_NAME]  # type: ignore[literal-required]
+        )
 
         if (
             self.intent_name_from_action(self.action_name)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/actions/action.py#L482'>rasa/core/actions/action.py~L482</a>
```diff
         # Override utter action of ActionBotResponse
         # with the complete utter action retrieved from
         # the output of response selector.
-        self.utter_action = selected[RESPONSE_SELECTOR_PREDICTION_KEY][
-            RESPONSE_SELECTOR_UTTER_ACTION_KEY
-        ]
+        self.utter_action = selected[RESPONSE_SELECTOR_PREDICTION_KEY][RESPONSE_SELECTOR_UTTER_ACTION_KEY]
 
         return await super().run(output_channel, nlg, tracker, domain)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/rule_policy.py#L1233'>rasa/core/policies/rule_policy.py~L1233</a>
```diff
         result = super()._default_predictions(domain)
 
         if self._enable_fallback_prediction:
-            result[domain.index_for_action(self._fallback_action_name)] = self.config[
-                "core_fallback_threshold"
-            ]
+            result[domain.index_for_action(self._fallback_action_name)] = (
+                self.config["core_fallback_threshold"]
+            )
         return result
 
     def persist(self) -> None:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1431'>rasa/core/policies/ted_policy.py~L1431</a>
```diff
             # exactly the same positional encoding
             dialogue_in = tf.reverse_sequence(dialogue_in, dialogue_lengths, seq_axis=1)
 
-        dialogue_transformed, attention_weights = self._tf_layers[
-            f"transformer.{DIALOGUE}"
-        ](dialogue_in, 1 - mask, self._training)
+        dialogue_transformed, attention_weights = (
+            self._tf_layers[f"transformer.{DIALOGUE}"](
+                dialogue_in, 1 - mask, self._training
+            )
+        )
         dialogue_transformed = tf.nn.gelu(dialogue_transformed)
 
         if self.max_history_featurizer_is_used:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1515'>rasa/core/policies/ted_policy.py~L1515</a>
```diff
             # state-level attributes don't use an encoding layer, hence their size is
             # just the output size of the corresponding sparse+dense feature combining
             # layer
-            units = self._tf_layers[
-                f"sparse_dense_concat_layer.{attribute}"
-            ].output_units
+            units = (
+                self._tf_layers[f"sparse_dense_concat_layer.{attribute}"].output_units
+            )
 
         attribute_features = tf.zeros(
             (batch_dim, dialogue_dim, units), dtype=tf.float32
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1628'>rasa/core/policies/ted_policy.py~L1628</a>
```diff
                 sequence_feature_lengths, sequence_feature_lengths
             )
 
-            attribute_features, _, _, _, _, _ = self._tf_layers[
-                f"sequence_layer.{attribute}"
-            ](
-                (
-                    tf_batch_data[attribute][SEQUENCE],
-                    tf_batch_data[attribute][SENTENCE],
-                    sequence_feature_lengths,
-                ),
-                training=self._training,
+            attribute_features, _, _, _, _, _ = (
+                self._tf_layers[f"sequence_layer.{attribute}"](
+                    (
+                        tf_batch_data[attribute][SEQUENCE],
+                        tf_batch_data[attribute][SENTENCE],
+                        sequence_feature_lengths,
+                    ),
+                    training=self._training,
+                )
             )
 
             combined_sentence_sequence_feature_lengths = sequence_feature_lengths + 1
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1672'>rasa/core/policies/ted_policy.py~L1672</a>
```diff
         else:
             # resulting attribute features will have shape
             # combined batch dimension and dialogue length x 1 x units
-            attribute_features = self._tf_layers[
-                f"sparse_dense_concat_layer.{attribute}"
-            ]((tf_batch_data[attribute][SENTENCE],), training=self._training)
+            attribute_features = (
+                self._tf_layers[f"sparse_dense_concat_layer.{attribute}"](
+                    (tf_batch_data[attribute][SENTENCE],), training=self._training
+                )
+            )
 
         if attribute in SENTENCE_FEATURES_TO_ENCODE + LABEL_FEATURES_TO_ENCODE:
             attribute_features = self._tf_layers[f"encoding_layer.{attribute}"](
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L2062'>rasa/core/policies/ted_policy.py~L2062</a>
```diff
         ) = self._embed_dialogue(dialogue_in, tf_batch_data)
         dialogue_mask = tf.squeeze(dialogue_mask, axis=-1)
 
-        sim_all, scores = self._tf_layers[
-            f"loss.{LABEL}"
-        ].get_similarities_and_confidences_from_embeddings(
-            dialogue_embed[:, :, tf.newaxis, :],
-            self.all_labels_embed[tf.newaxis, tf.newaxis, :, :],
-            dialogue_mask,
+        sim_all, scores = (
+            self._tf_layers[f"loss.{LABEL}"].get_similarities_and_confidences_from_embeddings(
+                dialogue_embed[:, :, tf.newaxis, :],
+                self.all_labels_embed[tf.newaxis, tf.newaxis, :, :],
+                dialogue_mask,
+            )
         )
 
         predictions = {
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L564'>rasa/core/test.py~L564</a>
```diff
     else:
         response_selector_info = (
             {
-                RESPONSE_SELECTOR_PROPERTY_NAME: predicted[
-                    RESPONSE_SELECTOR_PROPERTY_NAME
-                ]
+                RESPONSE_SELECTOR_PROPERTY_NAME: predicted[RESPONSE_SELECTOR_PROPERTY_NAME]
             }
             if RESPONSE_SELECTOR_PROPERTY_NAME in predicted
             else None
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L356'>rasa/nlu/classifiers/diet_classifier.py~L356</a>
```diff
             # check that all hidden layer sizes are the same
             identical_hidden_layer_sizes = all(
                 current_hidden_layer_sizes == first_hidden_layer_sizes
-                for current_hidden_layer_sizes in self.component_config[
-                    HIDDEN_LAYERS_SIZES
-                ].values()
+                for current_hidden_layer_sizes in self.component_config[HIDDEN_LAYERS_SIZES].values()
             )
             if not identical_hidden_layer_sizes:
                 raise ValueError(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1774'>rasa/nlu/classifiers/diet_classifier.py~L1774</a>
```diff
             tf_batch_data, TEXT
         )
 
-        text_transformed, _, _, _, _, attention_weights = self._tf_layers[
-            f"sequence_layer.{self.text_name}"
-        ](
-            (
-                tf_batch_data[TEXT][SEQUENCE],
-                tf_batch_data[TEXT][SENTENCE],
-                sequence_feature_lengths,
-            ),
-            training=self._training,
+        text_transformed, _, _, _, _, attention_weights = (
+            self._tf_layers[f"sequence_layer.{self.text_name}"](
+                (
+                    tf_batch_data[TEXT][SEQUENCE],
+                    tf_batch_data[TEXT][SENTENCE],
+                    sequence_feature_lengths,
+                ),
+                training=self._training,
+            )
         )
         predictions = {
             DIAGNOSTIC_DATA: {
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1860'>rasa/nlu/classifiers/diet_classifier.py~L1860</a>
```diff
         )
         sentence_vector_embed = self._tf_layers[f"embed.{TEXT}"](sentence_vector)
 
-        _, scores = self._tf_layers[
-            f"loss.{LABEL}"
-        ].get_similarities_and_confidences_from_embeddings(
-            sentence_vector_embed[:, tf.newaxis, :],
-            self.all_labels_embed[tf.newaxis, :, :],
+        _, scores = (
+            self._tf_layers[f"loss.{LABEL}"].get_similarities_and_confidences_from_embeddings(
+                sentence_vector_embed[:, tf.newaxis, :],
+                self.all_labels_embed[tf.newaxis, :, :],
+            )
         )
 
         return {"i_scores": scores}
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/extractors/extractor.py#L402'>rasa/nlu/extractors/extractor.py~L402</a>
```diff
         }
 
         if confidences is not None:
-            entity[ENTITY_ATTRIBUTE_CONFIDENCE_TYPE] = confidences[
-                ENTITY_ATTRIBUTE_TYPE
-            ][idx]
+            entity[ENTITY_ATTRIBUTE_CONFIDENCE_TYPE] = (
+                confidences[ENTITY_ATTRIBUTE_TYPE][idx]
+            )
 
         if ENTITY_ATTRIBUTE_ROLE in tag_names and role_tag != NO_ENTITY_TAG:
             entity[ENTITY_ATTRIBUTE_ROLE] = role_tag
             if confidences is not None:
-                entity[ENTITY_ATTRIBUTE_CONFIDENCE_ROLE] = confidences[
-                    ENTITY_ATTRIBUTE_ROLE
-                ][idx]
+                entity[ENTITY_ATTRIBUTE_CONFIDENCE_ROLE] = (
+                    confidences[ENTITY_ATTRIBUTE_ROLE][idx]
+                )
         if ENTITY_ATTRIBUTE_GROUP in tag_names and group_tag != NO_ENTITY_TAG:
             entity[ENTITY_ATTRIBUTE_GROUP] = group_tag
             if confidences is not None:
-                entity[ENTITY_ATTRIBUTE_CONFIDENCE_GROUP] = confidences[
-                    ENTITY_ATTRIBUTE_GROUP
-                ][idx]
+                entity[ENTITY_ATTRIBUTE_CONFIDENCE_GROUP] = (
+                    confidences[ENTITY_ATTRIBUTE_GROUP][idx]
+                )
 
         return entity
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L324'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L324</a>
```diff
 
     def _sentence_encoding_of_text(self, batch: List[Text]) -> np.ndarray:
 
-        return self.sentence_encoding_signature(tf.convert_to_tensor(batch))[
-            "default"
-        ].numpy()
+        return self.sentence_encoding_signature(
+            tf.convert_to_tensor(batch)
+        )["default"].numpy()
 
     def _sequence_encoding_of_text(self, batch: List[Text]) -> np.ndarray:
 
-        return self.sequence_encoding_signature(tf.convert_to_tensor(batch))[
-            "sequence_encoding"
-        ].numpy()
+        return self.sequence_encoding_signature(
+            tf.convert_to_tensor(batch)
+        )["sequence_encoding"].numpy()
 
     def process_training_data(self, training_data: TrainingData) -> TrainingData:
         """Featurize all message attributes in the training data with the ConveRT model.
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L410'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L410</a>
```diff
 
     def _tokenize(self, sentence: Text) -> Any:
 
-        return self.tokenize_signature(tf.convert_to_tensor([sentence]))[
-            "default"
-        ].numpy()
+        return self.tokenize_signature(
+            tf.convert_to_tensor([sentence])
+        )["default"].numpy()
 
     def tokenize(self, message: Message, attribute: Text) -> List[Token]:
         """Tokenize the text using the ConveRT model.
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L814'>rasa/nlu/selectors/response_selector.py~L814</a>
```diff
         )
 
         # Combine all feature types into one and embed using a transformer.
-        label_transformed, _, _, _, _, _ = self._tf_layers[
-            f"sequence_layer.{self.label_name}"
-        ](
-            (
-                self.tf_label_data[LABEL][SEQUENCE],
-                self.tf_label_data[LABEL][SENTENCE],
-                sequence_feature_lengths,
-            ),
-            training=self._training,
+        label_transformed, _, _, _, _, _ = (
+            self._tf_layers[f"sequence_layer.{self.label_name}"](
+                (
+                    self.tf_label_data[LABEL][SEQUENCE],
+                    self.tf_label_data[LABEL][SENTENCE],
+                    sequence_feature_lengths,
+                ),
+                training=self._training,
+            )
         )
 
         # Last token is taken from the last position with real features, determined
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L879'>rasa/nlu/selectors/response_selector.py~L879</a>
```diff
         sequence_feature_lengths_label = self._get_sequence_feature_lengths(
             tf_batch_data, LABEL
         )
-        label_transformed, _, _, _, _, _ = self._tf_layers[
-            f"sequence_layer.{self.label_name}"
-        ](
-            (
-                tf_batch_data[LABEL][SEQUENCE],
-                tf_batch_data[LABEL][SENTENCE],
-                sequence_feature_lengths_label,
-            ),
-            training=self._training,
+        label_transformed, _, _, _, _, _ = (
+            self._tf_layers[f"sequence_layer.{self.label_name}"](
+                (
+                    tf_batch_data[LABEL][SEQUENCE],
+                    tf_batch_data[LABEL][SENTENCE],
+                    sequence_feature_lengths_label,
+                ),
+                training=self._training,
+            )
         )
 
         losses = []
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L953'>rasa/nlu/selectors/response_selector.py~L953</a>
```diff
         sequence_feature_lengths = self._get_sequence_feature_lengths(
             tf_batch_data, TEXT
         )
-        text_transformed, _, _, _, _, attention_weights = self._tf_layers[
-            f"sequence_layer.{self.text_name}"
-        ](
-            (
-                tf_batch_data[TEXT][SEQUENCE],
-                tf_batch_data[TEXT][SENTENCE],
-                sequence_feature_lengths,
-            ),
-            training=self._training,
+        text_transformed, _, _, _, _, attention_weights = (
+            self._tf_layers[f"sequence_layer.{self.text_name}"](
+                (
+                    tf_batch_data[TEXT][SEQUENCE],
+                    tf_batch_data[TEXT][SENTENCE],
+                    sequence_feature_lengths,
+                ),
+                training=self._training,
+            )
         )
 
         predictions = {
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L978'>rasa/nlu/selectors/response_selector.py~L978</a>
```diff
         sentence_vector = self._last_token(text_transformed, sequence_feature_lengths)
         sentence_vector_embed = self._tf_layers[f"embed.{TEXT}"](sentence_vector)
 
-        _, scores = self._tf_layers[
-            f"loss.{LABEL}"
-        ].get_similarities_and_confidences_from_embeddings(
-            sentence_vector_embed[:, tf.newaxis, :],
-            self.all_labels_embed[tf.newaxis, :, :],
+        _, scores = (
+            self._tf_layers[f"loss.{LABEL}"].get_similarities_and_confidences_from_embeddings(
+                sentence_vector_embed[:, tf.newaxis, :],
+                self.all_labels_embed[tf.newaxis, :, :],
+            )
         )
         predictions["i_scores"] = scores
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/domain.py#L616'>rasa/shared/core/domain.py~L616</a>
```diff
                     entity_properties.entities.append(_entity)
                     if sub_labels:
                         if ENTITY_ROLES_KEY in sub_labels:
-                            entity_properties.roles[_entity] = sub_labels[
-                                ENTITY_ROLES_KEY
-                            ]
+                            entity_properties.roles[_entity] = (
+                                sub_labels[ENTITY_ROLES_KEY]
+                            )
                         if ENTITY_GROUPS_KEY in sub_labels:
-                            entity_properties.groups[_entity] = sub_labels[
-                                ENTITY_GROUPS_KEY
-                            ]
+                            entity_properties.groups[_entity] = (
+                                sub_labels[ENTITY_GROUPS_KEY]
+                            )
                         if (
                             ENTITY_FEATURIZATION_KEY in sub_labels
                             and sub_labels[ENTITY_FEATURIZATION_KEY] is False
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/utils/tensorflow/rasa_layers.py#L82'>rasa/utils/tensorflow/rasa_layers.py~L82</a>
```diff
                     attribute in new_sparse_feature_sizes
                     and feature_type in new_sparse_feature_sizes[attribute]
                 ):
-                    new_feature_sizes = new_sparse_feature_sizes[attribute][
-                        feature_type
-                    ]
-                    old_feature_sizes = old_sparse_feature_sizes[attribute][
-                        feature_type
-                    ]
+                    new_feature_sizes = (
+                        new_sparse_feature_sizes[attribute][feature_type]
+                    )
+                    old_feature_sizes = (
+                        old_sparse_feature_sizes[attribute][feature_type]
+                    )
                     if sum(new_feature_sizes) > sum(old_feature_sizes):
                         self._tf_layers[name] = self._replace_dense_for_sparse_layer(
                             layer_to_replace=layer,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/utils/tensorflow/rasa_layers.py#L474'>rasa/utils/tensorflow/rasa_layers.py~L474</a>
```diff
             # featurizers.
             if sequence_units != sentence_units:
                 for feature_type in [SEQUENCE, SENTENCE]:
-                    self._tf_layers[
-                        f"unify_dims_before_seq_sent_concat.{feature_type}"
-                    ] = layers.Ffnn(
+                    self._tf_layers[f"unify_dims_before_seq_sent_concat.{feature_type}"] = layers.Ffnn(
                         layer_name_suffix=f"unify_dims.{attribute}_{feature_type}",
                         layer_sizes=[config[CONCAT_DIMENSION][attribute]],
                         dropout_rate=config[DROP_RATE],
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/utils/tensorflow/rasa_layers.py#L513'>rasa/utils/tensorflow/rasa_layers.py~L513</a>
```diff
         # If needed, pass both feature types through a dense layer to bring them to the
         # same shape.
         if f"unify_dims_before_seq_sent_concat.{SEQUENCE}" in self._tf_layers:
-            sequence_tensor = self._tf_layers[
-                f"unify_dims_before_seq_sent_concat.{SEQUENCE}"
-            ](sequence_tensor)
+            sequence_tensor = (
+                self._tf_layers[f"unify_dims_before_seq_sent_concat.{SEQUENCE}"](
+                    sequence_tensor
+                )
+            )
         if f"unify_dims_before_seq_sent_concat.{SENTENCE}" in self._tf_layers:
-            sentence_tensor = self._tf_layers[
-                f"unify_dims_before_seq_sent_concat.{SENTENCE}"
-            ](sentence_tensor)
+            sentence_tensor = (
+                self._tf_layers[f"unify_dims_before_seq_sent_concat.{SENTENCE}"](
+                    sentence_tensor
+                )
+            )
 
         # mask_combined_sequence_sentence has for each input example a sequence of 1s of
         # the length seq_length+1, where seq_length is the number of real tokens. The
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/evaluation/test_marker_stats.py#L284'>tests/core/evaluation/test_marker_stats.py~L284</a>
```diff
         rows = [row for row in reader]
 
     actual_information = {
-        (row["sender_id"], row["session_idx"], row["marker"], row["statistic"]): row[
-            "value"
-        ]
+        (
+            row["sender_id"],
+            row["session_idx"],
+            row["marker"],
+            row["statistic"],
+        ): row["value"]
         for row in rows
     }
     num_digits = 3
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/policies/test_unexpected_intent_policy.py#L122'>tests/core/policies/test_unexpected_intent_policy.py~L122</a>
```diff
             SENTENCE,
         ]
         assert list(assembled_label_data_signature[LABEL].keys()) == [IDS]
-        assert assembled_label_data_signature[f"{LABEL}_{INTENT}"][SENTENCE][
-            0
-        ].units == len(default_domain.intents)
+        assert (
+            assembled_label_data_signature[f"{LABEL}_{INTENT}"][SENTENCE][0].units
+            == len(default_domain.intents)
+        )
 
     def test_training_with_no_intent(
         self,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_diet_classifier.py#L788'>tests/nlu/classifiers/test_diet_classifier.py~L788</a>
```diff
     old_sparse_feature_sizes = processed_message.get_sparse_feature_sizes(
         attribute=TEXT
     )
-    initial_diet_layers = classifier.model._tf_layers["sequence_layer.text"]._tf_layers[
-        "feature_combining"
-    ]
-    initial_diet_sequence_layer = initial_diet_layers._tf_layers[
-        "sparse_dense.sequence"
-    ]._tf_layers["sparse_to_dense"]
-    initial_diet_sentence_layer = initial_diet_layers._tf_layers[
-        "sparse_dense.sentence"
-    ]._tf_layers["sparse_to_dense"]
+    initial_diet_layers = classifier.model._tf_layers["sequence_layer.text"]._tf_layers["feature_combining"]
+    initial_diet_sequence_layer = initial_diet_layers._tf_layers["sparse_dense.sequence"]._tf_layers["sparse_to_dense"]
+    initial_diet_sentence_layer = initial_diet_layers._tf_layers["sparse_dense.sentence"]._tf_layers["sparse_to_dense"]
 
     initial_diet_sequence_size = initial_diet_sequence_layer.get_kernel().shape[0]
     initial_diet_sentence_size = initial_diet_sentence_layer.get_kernel().shape[0]
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_diet_classifier.py#L819'>tests/nlu/classifiers/test_diet_classifier.py~L819</a>
```diff
         attribute=TEXT
     )
 
-    final_diet_layers = finetune_classifier.model._tf_layers[
-        "sequence_layer.text"
-    ]._tf_layers["feature_combining"]
-    final_diet_sequence_layer = final_diet_layers._tf_layers[
-        "sparse_dense.sequence"
-    ]._tf_layers["sparse_to_dense"]
-    final_diet_sentence_layer = final_diet_layers._tf_layers[
-        "sparse_dense.sentence"
-    ]._tf_layers["sparse_to_dense"]
+    final_diet_layers = finetune_classifier.model._tf_layers["sequence_layer.text"]._tf_layers["feature_combining"]
+    final_diet_sequence_layer = final_diet_layers._tf_layers["sparse_dense.sequence"]._tf_layers["sparse_to_dense"]
+    final_diet_sentence_layer = final_diet_layers._tf_layers["sparse_dense.sentence"]._tf_layers["sparse_to_dense"]
 
     final_diet_sequence_size = final_diet_sequence_layer.get_kernel().shape[0]
     final_diet_sentence_size = final_diet_sentence_layer.get_kernel().shape[0]
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/selectors/test_selectors.py#L700'>tests/nlu/selectors/test_selectors.py~L700</a>
```diff
         attribute=TEXT
     )
 
-    initial_rs_layers = response_selector.model._tf_layers[
-        "sequence_layer.text"
-    ]._tf_layers["feature_combining"]
-    initial_rs_sequence_layer = initial_rs_layers._tf_layers[
-        "sparse_dense.sequence"
-    ]._tf_layers["sparse_to_dense"]
-    initial_rs_sentence_layer = initial_rs_layers._tf_layers[
-        "sparse_dense.sentence"
-    ]._tf_layers["sparse_to_dense"]
+    initial_rs_layers = response_selector.model._tf_layers["sequence_layer.text"]._tf_layers["feature_combining"]
+    initial_rs_sequence_layer = initial_rs_layers._tf_layers["sparse_dense.sequence"]._tf_layers["sparse_to_dense"]
+    initial_rs_sentence_layer = initial_rs_layers._tf_layers["sparse_dense.sentence"]._tf_layers["sparse_to_dense"]
 
     initial_rs_sequence_size = initial_rs_sequence_layer.get_kernel().shape[0]
     initial_rs_sentence_size = initial_rs_sentence_layer.get_kernel().shape[0]
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/selectors/test_selectors.py#L737'>tests/nlu/selectors/test_selectors.py~L737</a>
```diff
         attribute=TEXT
     )
 
-    final_rs_layers = response_selector.model._tf_layers[
-        "sequence_layer.text"
-    ]._tf_layers["feature_combining"]
-    final_rs_sequence_layer = final_rs_layers._tf_layers[
-        "sparse_dense.sequence"
-    ]._tf_layers["sparse_to_dense"]
-    final_rs_sentence_layer = final_rs_layers._tf_layers[
-        "sparse_dense.sentence"
-    ]._tf_layers["sparse_to_dense"]
+    final_rs_layers = response_selector.model._tf_layers["sequence_layer.text"]._tf_layers["feature_combining"]
+    final_rs_sequence_layer = final_rs_layers._tf_layers["sparse_dense.sequence"]._tf_layers["sparse_to_dense"]
+    final_rs_sentence_layer = final_rs_layers._tf_layers["sparse_dense.sentence"]._tf_layers["sparse_to_dense"]
 
     final_rs_sequence_size = final_rs_sequence_layer.get_kernel().shape[0]
     final_rs_sentence_size = final_rs_sentence_layer.get_kernel().shape[0]
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+12 -12 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/d175ae1dc28451de72bdfd9ff33212a9f1920932/src/snowflake/cli/api/commands/overrideable_parameter.py#L107'>src/snowflake/cli/api/commands/overrideable_parameter.py~L107</a>
```diff
                         value and ctx.params.get(name, False)
                     ):  # if the current parameter is set to True and a previous parameter is also Truthy
                         curr_opt = param.opts[0]
-                        other_opt = [x for x in ctx.command.params if x.name == name][
-                            0
-                        ].opts[0]
+                        other_opt = [
+                            x for x in ctx.command.params if x.name == name
+                        ][0].opts[0]
                         raise IncompatibleParametersError([curr_opt, other_opt])
 
             # pass args to existing callback based on its signature (this is how Typer infers callback args)
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/d175ae1dc28451de72bdfd9ff33212a9f1920932/test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py#L18'>test_external_plugins/snowpark_hello_single_command/src/snowflakecli/test_plugins/snowpark_hello/manager.py~L18</a>
```diff
 
 class SnowparkHelloManager(SqlExecutionMixin):
     _plugin_config_manager = PluginConfigProvider()
-    _greeting = _plugin_config_manager.get_config("snowpark-hello").internal_config[
-        "greeting"
-    ]
+    _greeting = _plugin_config_manager.get_config(
+        "snowpark-hello"
+    ).internal_config["greeting"]
 
     def say_hello(self, name: str) -> SnowflakeCursor:
         return self.execute_query(
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/d175ae1dc28451de72bdfd9ff33212a9f1920932/tests/app/test_telemetry.py#L60'>tests/app/test_telemetry.py~L60</a>
```diff
         .to_dict()
     )
 
-    del usage_command_event["message"][
-        "command_ci_environment"
-    ]  # to avoid side effect from CI
+    del (
+        usage_command_event["message"]["command_ci_environment"]
+    )  # to avoid side effect from CI
     assert usage_command_event == {
         "message": {
             "driver_type": "PythonConnector",
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/d175ae1dc28451de72bdfd9ff33212a9f1920932/tests/nativeapp/test_manager.py#L1729'>tests/nativeapp/test_manager.py~L1729</a>
```diff
 
     def get_events():
         dm = _get_dm()
-        pkg_model: ApplicationPackageEntityModel = dm.project_definition.entities[
-            "app_pkg"
-        ]
+        pkg_model: ApplicationPackageEntityModel = (
+            dm.project_definition.entities["app_pkg"]
+        )
         app_model: ApplicationEntityModel = dm.project_definition.entities["myapp"]
         app = ApplicationEntity(app_model, workspace_context)
         return app.get_events(
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+16 -18 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/entityset/entityset.py#L917'>featuretools/entityset/entityset.py~L917</a>
```diff
         )
 
         new_dataframe = new_dataframe.dropna(subset=[index])
-        new_dataframe2 = new_dataframe.drop_duplicates(index, keep="first")[
-            selected_columns
-        ]
+        new_dataframe2 = new_dataframe.drop_duplicates(
+            index, keep="first"
+        )[selected_columns]
 
         if make_time_index:
             new_dataframe2 = new_dataframe2.rename(
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/entityset/entityset.py#L934'>featuretools/entityset/entityset.py~L934</a>
```diff
             secondary_columns = [index, secondary_time_index] + list(
                 make_secondary_time_index.values(),
             )[0]
-            secondary_df = new_dataframe.drop_duplicates(index, keep="last")[
-                secondary_columns
-            ]
+            secondary_df = new_dataframe.drop_duplicates(
+                index, keep="last"
+            )[secondary_columns]
             if new_dataframe_secondary_time_index:
                 secondary_df = secondary_df.rename(
                     columns={secondary_time_index: new_dataframe_secondary_time_index},
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/entityset/entityset.py#L984'>featuretools/entityset/entityset.py~L984</a>
```diff
             secondary_time_index=make_secondary_time_index,
         )
 
-        self.dataframe_dict[base_dataframe_name] = self.dataframe_dict[
-            base_dataframe_name
-        ].ww.drop(additional_columns)
+        self.dataframe_dict[base_dataframe_name] = (
+            self.dataframe_dict[base_dataframe_name].ww.drop(additional_columns)
+        )
 
         self.dataframe_dict[base_dataframe_name].ww.add_semantic_tags(
             {base_dataframe_index: "foreign_key"},
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/entityset/entityset.py#L1293'>featuretools/entityset/entityset.py~L1293</a>
```diff
 
         if dataframe_name is not None and values is not None:
             for column, vals in values.items():
-                self[dataframe_name].ww.columns[column].metadata[
-                    "interesting_values"
-                ] = vals
+                self[dataframe_name].ww.columns[column].metadata["interesting_values"] = vals
             return
 
         if dataframe_name:
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/primitives/options_utils.py#L96'>featuretools/primitives/options_utils.py~L96</a>
```diff
                 # don't globally ignore a column if it's included for a primitive
                 if "include_columns" in option:
                     for dataframe, include_cols in option["include_columns"].items():
-                        global_ignore_columns[dataframe] = global_ignore_columns[
-                            dataframe
-                        ].difference(include_cols)
+                        global_ignore_columns[dataframe] = (
+                            global_ignore_columns[dataframe].difference(include_cols)
+                        )
                 option["ignore_dataframes"] = option["ignore_dataframes"].union(
                     ignore_dataframes.difference(included_dataframes),
                 )
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/primitives/options_utils.py#L106'>featuretools/primitives/options_utils.py~L106</a>
```diff
                 # if already ignoring columns for this dataframe, add globals
                 for option in options:
                     if dataframe in option["ignore_columns"]:
-                        option["ignore_columns"][dataframe] = option["ignore_columns"][
-                            dataframe
-                        ].union(ignore_cols)
+                        option["ignore_columns"][dataframe] = (
+                            option["ignore_columns"][dataframe].union(ignore_cols)
+                        )
                     # if no ignore_columns and dataframe is explicitly included, don't ignore the column
                     elif dataframe in included_dataframes:
                         continue
```

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PlasmaPy/PlasmaPy/blob/5625ea9cd01bf599932a5e4bea01169f99f48fbc/src/plasmapy/formulary/braginskii.py#L2024'>src/plasmapy/formulary/braginskii.py~L2024</a>
```diff
     eta_0 = eta_0_e[Z_idx]
 
     def f_eta_2(Z_idx, r):
-        numerator = (6 / 5 * Z + 3 / 5 * np.sqrt(2)) * r + hprime_0[Z_idx] * eta_0_e[
-            Z_idx
-        ]
+        numerator = (
+            6 / 5 * Z + 3 / 5 * np.sqrt(2)
+        ) * r + hprime_0[Z_idx] * eta_0_e[Z_idx]
         denominator = (
             r**3
             + hprime_4[Z_idx] * r ** (7 / 3)
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+36 -39 lines across 8 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L4506'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py~L4506</a>
```diff
         )
         response_data = response.json()
         if expected_status_code == 200:
-            expected_json["task_instances"][0]["dag_version"]["created_at"] = response_data["task_instances"][
-                0
-            ]["dag_version"]["created_at"]
-            expected_json["task_instances"][0]["dag_version"]["id"] = response_data["task_instances"][0][
-                "dag_version"
-            ]["id"]
+            expected_json["task_instances"][0]["dag_version"]["created_at"] = (
+                response_data["task_instances"][0]["dag_version"]["created_at"]
+            )
+            expected_json["task_instances"][0]["dag_version"]["id"] = (
+                response_data["task_instances"][0]["dag_version"]["id"]
+            )
             expected_json["task_instances"][0]["id"] = response_data["task_instances"][0]["id"]
         assert response.status_code == expected_status_code
         assert response_data == expected_json
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L5265'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py~L5265</a>
```diff
         )
         response_data = response.json()
         if expected_status_code == 200:
-            expected_json["task_instances"][0]["dag_version"]["created_at"] = response_data["task_instances"][
-                0
-            ]["dag_version"]["created_at"]
-            expected_json["task_instances"][0]["dag_version"]["id"] = response_data["task_instances"][0][
-                "dag_version"
-            ]["id"]
+            expected_json["task_instances"][0]["dag_version"]["created_at"] = (
+                response_data["task_instances"][0]["dag_version"]["created_at"]
+            )
+            expected_json["task_instances"][0]["dag_version"]["id"] = (
+                response_data["task_instances"][0]["dag_version"]["id"]
+            )
             expected_json["task_instances"][0]["id"] = response_data["task_instances"][0]["id"]
         assert response.status_code == expected_status_code
         assert response_data == expected_json
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2584'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2584</a>
```diff
             "stuck in queued tries exceeded",
         ]
 
-        mock_executors[
-            0
-        ].send_callback.assert_called_once()  # this should only be called for the task that has a callback
+        mock_executors[0].send_callback.assert_called_once()  # this should only be called for the task that has a callback
         states = [x.state for x in dr.get_task_instances(session=session)]
         assert states == ["failed", "failed"]
         mock_executors[0].fail.assert_called()
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2686'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2686</a>
```diff
             "stuck in queued tries exceeded",
         ]
 
-        mock_executors[
-            0
-        ].send_callback.assert_called_once()  # this should only be called for the task that has a callback
+        mock_executors[0].send_callback.assert_called_once()  # this should only be called for the task that has a callback
         states = [x.state for x in dr.get_task_instances(session=session)]
         assert states == ["failed", "failed"]
         mock_executors[0].fail.assert_called()
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/airflow-ctl/src/airflowctl/ctl/cli_config.py#L603'>airflow-ctl/src/airflowctl/ctl/cli_config.py~L603</a>
```diff
             for parameter in api_operation["parameters"]:
                 for parameter_key, parameter_type in parameter.items():
                     if self._is_primitive_type(type_name=parameter_type):
-                        method_params[self._sanitize_method_param_key(parameter_key)] = args_dict[
-                            parameter_key
-                        ]
+                        method_params[self._sanitize_method_param_key(parameter_key)] = (
+                            args_dict[parameter_key]
+                        )
                     else:
                         datamodel = getattr(generated_datamodels, parameter_type)
                         for expanded_parameter in self.datamodels_extended_map[parameter_type]:
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py#L1205'>providers/amazon/src/airflow/providers/amazon/aws/hooks/sagemaker.py~L1205</a>
```diff
         except ClientError as e:
             # ValidationException can also happen if the package group name contains invalid char,
             # so we have to look at the error message too
-            if e.response["Error"]["Code"] == "ValidationException" and e.response["Error"][
-                "Message"
-            ].startswith("Model Package Group already exists"):
+            if (
+                e.response["Error"]["Code"] == "ValidationException"
+                and e.response["Error"]["Message"].startswith("Model Package Group already exists")
+            ):
                 # log msg only so it doesn't look like an error
                 self.log.info("%s", e.response["Error"]["Message"])
                 return False
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L216'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py~L216</a>
```diff
     :param collection_name: The name of the Collection to create.
     """
     log.info("\nCreating collection: %s.", collection_name)
-    return aoss_client.conn.create_collection(name=collection_name, type="VECTORSEARCH")[
-        "createCollectionDetail"
-    ]["id"]
+    return aoss_client.conn.create_collection(
+        name=collection_name, type="VECTORSEARCH"
+    )["createCollectionDetail"]["id"]
 
 
 @task
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/amazon/tests/unit/amazon/aws/executors/aws_lambda/test_lambda_executor.py#L909'>providers/amazon/tests/unit/amazon/aws/executors/aws_lambda/test_lambda_executor.py~L909</a>
```diff
         ]
         orphaned_tasks[0].external_executor_id = ser_airflow_key_1
         orphaned_tasks[1].external_executor_id = ser_airflow_key_2
-        orphaned_tasks[
-            2
-        ].external_executor_id = None  # One orphaned task has no external_executor_id, not adopted
+        orphaned_tasks[2].external_executor_id = (
+            None  # One orphaned task has no external_executor_id, not adopted
+        )
 
         for task in orphaned_tasks:
             task.try_number = 1
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/amazon/tests/unit/amazon/aws/hooks/test_emr.py#L291'>providers/amazon/tests/unit/amazon/aws/hooks/test_emr.py~L291</a>
```diff
 
         # Fetch a cluster from the second page using the boto API
         client = boto3.client("emr", region_name="us-east-1")
-        response_marker = client.list_clusters(ClusterStates=["RUNNING", "WAITING", "BOOTSTRAPPING"])[
-            "Marker"
-        ]
+        response_marker = client.list_clusters(
+            ClusterStates=["RUNNING", "WAITING", "BOOTSTRAPPING"]
+        )["Marker"]
         second_page_cluster = client.list_clusters(
             ClusterStates=["RUNNING", "WAITING", "BOOTSTRAPPING"], Marker=response_marker
         )["Clusters"][0]
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/apache/hive/tests/unit/apache/hive/hooks/test_hive.py#L140'>providers/apache/hive/tests/unit/apache/hive/hooks/test_hive.py~L140</a>
```diff
             if AIRFLOW_V_3_0_PLUS
             else AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_EXECUTION_DATE"]["env_var_format"]
         )
-        dag_run_id_ctx_var_name = AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_DAG_RUN_ID"][
-            "env_var_format"
-        ]
+        dag_run_id_ctx_var_name = (
+            AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_DAG_RUN_ID"]["env_var_format"]
+        )
         date_key = "logical_date" if AIRFLOW_V_3_0_PLUS else "execution_date"
         mock_output = [
             "Connecting to jdbc:hive2://localhost:10000/default",
```
<a href='https://github.com/apache/airflow/blob/cdecb7efbb3092e6c15ae5476436aa12add90fb8/providers/apache/hive/tests/unit/apache/hive/hooks/test_hive.py#L847'>providers/apache/hive/tests/unit/apache/hive/hooks/test_hive.py~L847</a>
```diff
             if AIRFLOW_V_3_0_PLUS
             else AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_EXECUTION_DATE"]["env_var_format"]
         )
-        dag_run_id_ctx_var_name = AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_DAG_RUN_ID"][
-            "env_var_format"
-        ]
+        dag_run_id_ctx_var_name = (
+            AIRFLOW_VAR_NAME_FORMAT_MAPPING["AIRFLOW_CONTEXT_DAG_RUN_ID"]["env_var_format"]
+        )
 
         with mock.patch.dict(
             "os.environ",
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+54 -54 lines across 13 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/169d27c9e974d045c36767ef30bfbc0af30648ac/superset/commands/dashboard/importers/v0.py#L199'>superset/commands/dashboard/importers/v0.py~L199</a>
```diff
             "expanded_slices" in i_params_dict
             and old_slc_id_str in i_params_dict["expanded_slices"]
         ):
-            new_expanded_slices[new_slc_id_str] = i_params_dict["expanded_slices"][
-                old_slc_id_str
-            ]
+            new_expanded_slices[new_slc_id_str] = (
+                i_params_dict["expanded_slices"][old_slc_id_str]
+            )
 
     # since PR #9109, filter_immune_slices and filter_immune_slice_fields
     # are converted to filter_scopes
```
<a href='https://github.com/apache/superset/blob/169d27c9e974d045c36767ef30bfbc0af30648ac/superset/commands/importers/v1/utils.py#L166'>superset/commands/importers/v1/utils.py~L166</a>
```diff
 
                 # populate ssh_tunnel_private_keys from the request or from existing DBs
                 if file_name in ssh_tunnel_private_keys:


... (truncated 5498 lines) ...



---
