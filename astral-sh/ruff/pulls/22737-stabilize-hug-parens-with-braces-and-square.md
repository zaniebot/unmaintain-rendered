```yaml
number: 22737
title: "Stabilize `hug_parens_with_braces_and_square_brackets` preview style"
type: pull_request
state: open
author: dylwil3
labels:
  - breaking
  - formatter
assignees: []
base: 2026-style
head: stabilize-hug-parens
created_at: 2026-01-19T20:23:50Z
updated_at: 2026-01-19T20:44:36Z
url: https://github.com/astral-sh/ruff/pull/22737
synced_at: 2026-01-19T21:33:07Z
```

# Stabilize `hug_parens_with_braces_and_square_brackets` preview style

---

_@dylwil3_

Don't get excited! Opening PRs for all preview styles to decide.


---

_Added to milestone `v0.15` by @dylwil3 on 2026-01-19 20:23_

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-19 20:23_

---

_Label `breaking` added by @dylwil3 on 2026-01-19 20:23_

---

_Label `formatter` added by @dylwil3 on 2026-01-19 20:23_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:32_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+17288 -20358 lines in 909 files in 26 projects; 29 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+25 -37 lines across 1 file)</summary>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L71'>housewatch/api/analyze.py~L71</a>
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
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L94'>housewatch/api/analyze.py~L94</a>
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
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L111'>housewatch/api/analyze.py~L111</a>
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
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L124'>housewatch/api/analyze.py~L124</a>
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
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L141'>housewatch/api/analyze.py~L141</a>
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
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/analyze.py#L295'>housewatch/api/analyze.py~L295</a>
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
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1248 -1413 lines across 60 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/brokers/pika.py#L238'>rasa/core/brokers/pika.py~L238</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L70'>rasa/core/channels/hangouts.py~L70</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/evaluation/marker_base.py#L665'>rasa/core/evaluation/marker_base.py~L665</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/evaluation/marker_base.py#L686'>rasa/core/evaluation/marker_base.py~L686</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/evaluation/marker_stats.py#L279'>rasa/core/evaluation/marker_stats.py~L279</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/featurizers/tracker_featurizers.py#L143'>rasa/core/featurizers/tracker_featurizers.py~L143</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/migrate.py#L70'>rasa/core/migrate.py~L70</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/migrate.py#L178'>rasa/core/migrate.py~L178</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/migrate.py#L234'>rasa/core/migrate.py~L234</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L525'>rasa/core/policies/ted_policy.py~L525</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L555'>rasa/core/policies/ted_policy.py~L555</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1298'>rasa/core/policies/ted_policy.py~L1298</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/processor.py#L651'>rasa/core/processor.py~L651</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L859'>rasa/core/test.py~L859</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/tracker_store.py#L606'>rasa/core/tracker_store.py~L606</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/interactive.py#L920'>rasa/core/training/interactive.py~L920</a>
```diff
     responses = NEW_RESPONSES
 
     # TODO for now there is no way to distinguish between action and form
-    collected_actions = list(
-        {
-            e["name"]
-            for e in actions
-            if e["name"] not in rasa.shared.core.constants.DEFAULT_ACTION_NAMES
-            and e["name"] not in old_domain.form_names
-        }
-    )
-
-    new_domain = Domain.from_dict(
-        {
-            KEY_INTENTS: list(_intents_from_messages(messages)),
-            KEY_ENTITIES: _entities_from_messages(messages),
-            KEY_RESPONSES: responses,
-            KEY_ACTIONS: collected_actions,
-        }
-    )
+    collected_actions = list({
+        e["name"]
+        for e in actions
+        if e["name"] not in rasa.shared.core.constants.DEFAULT_ACTION_NAMES
+        and e["name"] not in old_domain.form_names
+    })
+
+    new_domain = Domain.from_dict({
+        KEY_INTENTS: list(_intents_from_messages(messages)),
+        KEY_ENTITIES: _entities_from_messages(messages),
+        KEY_RESPONSES: responses,
+        KEY_ACTIONS: collected_actions,
+    })
 
     old_domain.merge(new_domain).persist(domain_path)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/interactive.py#L1725'>rasa/core/training/interactive.py~L1725</a>
```diff
     for potential_width in range(monospace_wrapping_width, -1, -1):
         lines = textwrap.wrap(text, potential_width)
         # test whether all lines' visible width fits the available width
-        if all(
-            [
-                terminaltables.width_and_alignment.visible_width(line)
-                <= monospace_wrapping_width
-                for line in lines
-            ]
-        ):
+        if all([
+            terminaltables.width_and_alignment.visible_width(line)
+            <= monospace_wrapping_width
+            for line in lines
+        ]):
             true_wrapping_width = potential_width
             break
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/graph.py#L139'>rasa/engine/graph.py~L139</a>
```diff
             targets if targets else self.target_names
         )
 
-        return GraphSchema(
-            {
-                node_name: node
-                for node_name, node in self.nodes.items()
-                if node_name in dependencies
-            }
-        )
+        return GraphSchema({
+            node_name: node
+            for node_name, node in self.nodes.items()
+            if node_name in dependencies
+        })
 
     def _all_dependencies_schema(self, targets: List[Text]) -> List[Text]:
         required = []
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/recipes/default_recipe.py#L339'>rasa/engine/recipes/default_recipe.py~L339</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/recipes/default_recipe.py#L731'>rasa/engine/recipes/default_recipe.py~L731</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/recipes/default_recipe.py#L745'>rasa/engine/recipes/default_recipe.py~L745</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/validation.py#L510'>rasa/engine/validation.py~L510</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/graph_components/validators/default_recipe_validator.py#L156'>rasa/graph_components/validators/default_recipe_validator.py~L156</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/graph_components/validators/default_recipe_validator.py#L172'>rasa/graph_components/validators/default_recipe_validator.py~L172</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/graph_components/validators/default_recipe_validator.py#L186'>rasa/graph_components/validators/default_recipe_validator.py~L186</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/graph_components/validators/default_recipe_validator.py#L362'>rasa/graph_components/validators/default_recipe_validator.py~L362</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1463'>rasa/nlu/classifiers/diet_classifier.py~L1463</a>
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
 
             self._tf_layers[f"feature_combining_layer.{self.label_name}"] = (
                 rasa_layers.RasaFeatureCombiningLayer(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/dense_featurizer/lm_featurizer.py#L543'>rasa/nlu/featurizers/dense_featurizer/lm_featurizer.py~L543</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L436'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L436</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L459'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L459</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L199'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L199</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/test.py#L1116'>rasa/nlu/test.py~L1116</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/server.py#L692'>rasa/server.py~L692</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/domain.py#L419'>rasa/shared/core/domain.py~L419</a>
```diff
         # add the config, session_config and training data version defaults
         # if not included in the original domain dict
         if "config" not in data and not store_entities_as_slots:
-            data.update(
-                {"config": {"store_entities_as_slots": store_entities_as_slots}}
-            )
+            data.update({
+                "config": {"store_entities_as_slots": store_entities_as_slots}
+            })
 
         if SESSION_CONFIG_KEY not in data:
-            data.update(
-                {
-                    SESSION_CONFIG_KEY: {
-                        SESSION_EXPIRATION_TIME_KEY: (
-                            session_config.session_expiration_time
-                        ),
-                        CARRY_OVER_SLOTS_KEY: session_config.carry_over_slots,
-                    }
+            data.update({
+                SESSION_CONFIG_KEY: {
+                    SESSION_EXPIRATION_TIME_KEY: (
+                        session_config.session_expiration_time
+                    ),
+                    CARRY_OVER_SLOTS_KEY: session_config.carry_over_slots,
                 }
-            )
+            })
 
         if KEY_TRAINING_DATA_FORMAT_VERSION not in data:
-            data.update(
-                {
-                    KEY_TRAINING_DATA_FORMAT_VERSION: DoubleQuotedScalarString(
-                        LATEST_TRAINING_DATA_FORMAT_VERSION
-                    )
-                }
-            )
+            data.update({
+                KEY_TRAINING_DATA_FORMAT_VERSION: DoubleQuotedScalarString(
+                    LATEST_TRAINING_DATA_FORMAT_VERSION
+                )
+            })
 
         return data
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L151'>rasa/shared/core/events.py~L151</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L583'>rasa/shared/core/events.py~L583</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1165'>rasa/shared/core/events.py~L1165</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1327'>rasa/shared/core/events.py~L1327</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1639'>rasa/shared/core/events.py~L1639</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1992'>rasa/shared/core/events.py~L1992</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/generator.py#L598'>rasa/shared/core/generator.py~L598</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L751'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L751</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L180'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L180</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L225'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L225</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L327'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L327</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_writer/yaml_story_writer.py#L346'>rasa/shared/core/training_data/story_writer/yaml_story_writer.py~L346</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/importers/importer.py#L413'>rasa/shared/importers/importer.py~L413</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/importers/importer.py#L481'>rasa/shared/importers/importer.py~L481</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/nlu/training_data/formats/readerwriter.py#L179'>rasa/shared/nlu/training_data/formats/readerwriter.py~L179</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/nlu/training_data/formats/readerwriter.py#L212'>rasa/shared/nlu/training_data/formats/readerwriter.py~L212</a

... (truncated 56065 lines) ...



---
