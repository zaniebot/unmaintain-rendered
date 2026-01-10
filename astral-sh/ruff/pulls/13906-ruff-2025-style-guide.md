```yaml
number: 13906
title: Ruff 2025 style guide
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - formatter
assignees: []
merged: true
base: ruff-0.9
head: micha/ruff-2025
created_at: 2024-10-24T11:23:04Z
updated_at: 2025-01-03T13:16:13Z
url: https://github.com/astral-sh/ruff/pull/13906
synced_at: 2026-01-10T20:42:26Z
```

# Ruff 2025 style guide

---

_Pull request opened by @MichaReiser on 2024-10-24 11:23_

## Summary

- Stabilize `comprehension_leading_expression_comments_same_line`
- Stabilize `empty_parameters_no_unnecessary_parentheses_around_return_value`
- Stabilize `docstring_code_block_in_docstring_indent`
- Stabilize `match_case_parenthese`
- Stabilize `with_single_item_pre_39`
- Stabilize `f_string_implicit_concatenated_string_literal_quotes`
- Stabilize `implicit_concatenated_string_formatting` (includes new assert formatting)
- [x] Stabilize https://github.com/astral-sh/ruff/pull/14005
- [x] Stabilize https://github.com/astral-sh/ruff/pull/13928 and remove the "conflicts with ISC001" warning

Closes #13371 


## Tasks

* [ ] Update deviation black document. It should mention f-string formatting, implicit concatenated string formatting, and assert formatting.
* [x] Decide if we have to revisit https://github.com/astral-sh/ruff/issues/14118
* [x] ~~https://github.com/astral-sh/ruff/issues/14143~~ not a new deviation: Postpone to next year

## Test plan

**Main (`8a860b8`)**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.76424 |              1959 |              1781 |
| django         |           0.99942 |              2791 |                76 |
| home-assistant |           1.00000 |             12993 |                 0 |
| poetry         |           0.99978 |               444 |                 2 |
| transformers   |           0.99907 |              3151 |                24 |
| twine          |           1.00000 |                32 |                 0 |
| typeshed       |           0.99139 |              4766 |               133 |
| warehouse      |           0.99962 |               767 |                33 |
| zulip          |           1.00000 |              1699 |                 0 |


**2025**


| project        | similarity index  | total files       | changed files     |                                     
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.76395 |              1959 |              1786 |
| django         |           0.99921 |              2791 |               108 |
| home-assistant |           0.99980 |             12993 |               146 |
| poetry         |           0.99935 |               444 |                 9 |
| transformers   |           0.99858 |              3151 |               221 |
| twine          |           0.99871 |                32 |                 3 |
| typeshed       |           0.99135 |              4766 |               138 |
| warehouse      |           0.99916 |               767 |                51 |
| zulip          |           0.99969 |              1699 |                29 |


* [ ] Review ecosystem changes
* [x] Review snapshot changes 
* [x] Test on larger projects

---

_Label `breaking` added by @MichaReiser on 2024-10-24 11:23_

---

_Label `formatter` added by @MichaReiser on 2024-10-24 11:23_

---

_Comment by @github-actions[bot] on 2024-10-24 13:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+1707 -1827 lines in 538 files in 27 projects; 28 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/disnake/opus.py#L371'>disnake/opus.py~L371</a>
```diff
     def set_bandwidth(self, req: BAND_CTL) -> None:
         if req not in band_ctl:
             raise KeyError(
-                f'{req!r} is not a valid bandwidth setting. Try one of: {",".join(band_ctl)}'
+                f"{req!r} is not a valid bandwidth setting. Try one of: {','.join(band_ctl)}"
             )
 
         k = band_ctl[req]
```
<a href='https://github.com/DisnakeDev/disnake/blob/a95ddafdb5c8edd58f3907ad6c839c5bc07687b0/disnake/opus.py#L380'>disnake/opus.py~L380</a>
```diff
     def set_signal_type(self, req: SIGNAL_CTL) -> None:
         if req not in signal_ctl:
             raise KeyError(
-                f'{req!r} is not a valid bandwidth setting. Try one of: {",".join(signal_ctl)}'
+                f"{req!r} is not a valid bandwidth setting. Try one of: {','.join(signal_ctl)}"
             )
 
         k = signal_ctl[req]
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+56 -70 lines across 29 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/utils.py#L278'>rasa/cli/utils.py~L278</a>
```diff
     # Check if a valid setting for `max_history` was given
     if isinstance(max_history, int) and max_history < 1:
         raise argparse.ArgumentTypeError(
-            f"The value of `--max-history {max_history}` " f"is not a positive integer."
+            f"The value of `--max-history {max_history}` is not a positive integer."
         )
 
     return validator.verify_story_structure(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/x.py#L165'>rasa/cli/x.py~L165</a>
```diff
         attempts -= 1
 
     rasa.shared.utils.cli.print_error_and_exit(
-        "Could not fetch runtime config from server at '{}'. " "Exiting.".format(
+        "Could not fetch runtime config from server at '{}'. Exiting.".format(
             config_endpoint
         )
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L322'>rasa/core/actions/action.py~L322</a>
```diff
         if message is None:
             if not self.silent_fail:
                 logger.error(
-                    "Couldn't create message for response '{}'." "".format(
+                    "Couldn't create message for response '{}'.".format(
                         self.utter_action
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L470'>rasa/core/actions/action.py~L470</a>
```diff
         else:
             if not self.silent_fail:
                 logger.error(
-                    "Couldn't create message for response action '{}'." "".format(
+                    "Couldn't create message for response action '{}'.".format(
                         self.action_name
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/console.py#L194'>rasa/core/channels/console.py~L194</a>
```diff
     exit_text = INTENT_MESSAGE_PREFIX + "stop"
 
     rasa.shared.utils.cli.print_success(
-        "Bot loaded. Type a message and press enter " "(use '{}' to exit): ".format(
+        "Bot loaded. Type a message and press enter (use '{}' to exit): ".format(
             exit_text
         )
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/telegram.py#L97'>rasa/core/channels/telegram.py~L97</a>
```diff
                     reply_markup.add(KeyboardButton(button["title"]))
         else:
             logger.error(
-                "Trying to send text with buttons for unknown " "button type {}".format(
+                "Trying to send text with buttons for unknown button type {}".format(
                     button_type
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/nlg/callback.py#L81'>rasa/core/nlg/callback.py~L81</a>
```diff
         body = nlg_request_format(utter_action, tracker, output_channel, **kwargs)
 
         logger.debug(
-            "Requesting NLG for {} from {}." "The request body is {}." "".format(
+            "Requesting NLG for {} from {}.The request body is {}.".format(
                 utter_action, self.nlg_endpoint.url, json.dumps(body)
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/policy.py#L250'>rasa/core/policies/policy.py~L250</a>
```diff
         max_training_samples = kwargs.get("max_training_samples")
         if max_training_samples is not None:
             logger.debug(
-                "Limit training data to {} training samples." "".format(
+                "Limit training data to {} training samples.".format(
                     max_training_samples
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/ted_policy.py#L837'>rasa/core/policies/ted_policy.py~L837</a>
```diff
             # take the last prediction in the sequence
             similarities = outputs["similarities"][:, -1, :]
         else:
-            raise TypeError(
-                "model output for `similarities` " "should be a numpy array"
-            )
+            raise TypeError("model output for `similarities` should be a numpy array")
         if isinstance(outputs["scores"], np.ndarray):
             confidences = outputs["scores"][:, -1, :]
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/unexpected_intent_policy.py#L612'>rasa/core/policies/unexpected_intent_policy.py~L612</a>
```diff
         if isinstance(output["similarities"], np.ndarray):
             sequence_similarities = output["similarities"][:, -1, :]
         else:
-            raise TypeError(
-                "model output for `similarities` " "should be a numpy array"
-            )
+            raise TypeError("model output for `similarities` should be a numpy array")
 
         # Check for unlikely intent
         last_user_uttered_event = tracker.get_last_event_for(UserUttered)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L772'>rasa/core/test.py~L772</a>
```diff
         ):
             story_dump = YAMLStoryWriter().dumps(partial_tracker.as_story().story_steps)
             error_msg = (
-                f"Model predicted a wrong action. Failed Story: " f"\n\n{story_dump}"
+                f"Model predicted a wrong action. Failed Story: \n\n{story_dump}"
             )
             raise WrongPredictionException(error_msg)
     elif prev_action_unlikely_intent:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/train.py#L34'>rasa/core/train.py~L34</a>
```diff
             for policy_config in policy_configs:
                 config_name = os.path.splitext(os.path.basename(policy_config))[0]
                 logging.info(
-                    "Starting to train {} round {}/{}" " with {}% exclusion" "".format(
+                    "Starting to train {} round {}/{} with {}% exclusion".format(
                         config_name, current_run, len(exclusion_percentages), percentage
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/train.py#L43'>rasa/core/train.py~L43</a>
```diff
                     domain,
                     policy_config,
                     stories=story_file,
-                    output=str(Path(output_path, f"run_{r +1}")),
+                    output=str(Path(output_path, f"run_{r + 1}")),
                     fixed_model_name=config_name + PERCENTAGE_KEY + str(percentage),
                     additional_arguments={
                         **additional_arguments,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L346'>rasa/core/training/interactive.py~L346</a>
```diff
     choices = []
     for p in sorted_intents:
         name_with_confidence = (
-            f'{p.get("confidence"):03.2f} {p.get(INTENT_NAME_KEY):40}'
+            f"{p.get('confidence'):03.2f} {p.get(INTENT_NAME_KEY):40}"
         )
         choice = {
             INTENT_NAME_KEY: name_with_confidence,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L674'>rasa/core/training/interactive.py~L674</a>
```diff
     await _print_history(conversation_id, endpoint)
 
     choices = [
-        {"name": f'{a["score"]:03.2f} {a["action"]:40}', "value": a["action"]}
+        {"name": f"{a['score']:03.2f} {a['action']:40}", "value": a["action"]}
         for a in predictions
     ]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L723'>rasa/core/training/interactive.py~L723</a>
```diff
     # export training data and quit
     questions = questionary.form(
         export_stories=questionary.text(
-            message="Export stories to (if file exists, this "
-            "will append the stories)",
+            message="Export stories to (if file exists, this will append the stories)",
             default=PATHS["stories"],
             validate=io_utils.file_type_validator(
                 rasa.shared.data.YAML_FILE_EXTENSIONS,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L738'>rasa/core/training/interactive.py~L738</a>
```diff
             default=PATHS["nlu"],
             validate=io_utils.file_type_validator(
                 list(rasa.shared.data.TRAINING_DATA_EXTENSIONS),
-                "Please provide a valid export path for the NLU data, "
-                "e.g. 'nlu.yml'.",
+                "Please provide a valid export path for the NLU data, e.g. 'nlu.yml'.",
             ),
         ),
         export_domain=questionary.text(
-            message="Export domain file to (if file exists, this "
-            "will be overwritten)",
+            message="Export domain file to (if file exists, this will be overwritten)",
             default=PATHS["domain"],
             validate=io_utils.file_type_validator(
                 rasa.shared.data.YAML_FILE_EXTENSIONS,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/utils.py#L41'>rasa/core/utils.py~L41</a>
```diff
     """
     if use_syslog:
         formatter = logging.Formatter(
-            "%(asctime)s [%(levelname)-5.5s] [%(process)d]" " %(message)s"
+            "%(asctime)s [%(levelname)-5.5s] [%(process)d] %(message)s"
         )
         socktype = SOCK_STREAM if syslog_protocol == TCP_PROTOCOL else SOCK_DGRAM
         syslog_handler = logging.handlers.SysLogHandler(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/utils.py#L73'>rasa/core/utils.py~L73</a>
```diff
     """
     if hot_idx >= length:
         raise ValueError(
-            "Can't create one hot. Index '{}' is out " "of range (length '{}')".format(
+            "Can't create one hot. Index '{}' is out of range (length '{}')".format(
                 hot_idx, length
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L166'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L166</a>
```diff
                 )
             if self.stop_words is not None:
                 logger.warning(
-                    "Analyzer is set to character, "
-                    "provided stop words will be ignored."
+                    "Analyzer is set to character, provided stop words will be ignored."
                 )
             if self.max_ngram == 1:
                 logger.warning(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/server.py#L289'>rasa/server.py~L289</a>
```diff
         raise ErrorResponse(
             HTTPStatus.BAD_REQUEST,
             "BadRequest",
-            "Invalid parameter value for 'include_events'. "
-            "Should be one of {}".format(enum_values),
+            "Invalid parameter value for 'include_events'. Should be one of {}".format(
+                enum_values
+            ),
             {"parameter": "include_events", "in": "query"},
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L198'>rasa/shared/core/domain.py~L198</a>
```diff
             domain = cls.from_directory(path)
         else:
             raise InvalidDomain(
-                "Failed to load domain specification from '{}'. "
-                "File not found!".format(os.path.abspath(path))
+                "Failed to load domain specification from '{}'. File not found!".format(
+                    os.path.abspath(path)
+                )
             )
 
         return domain
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L1964'>rasa/shared/core/events.py~L1964</a>
```diff
 
     def __str__(self) -> Text:
         """Returns text representation of event."""
-        return (
-            "ActionExecutionRejected("
-            "action: {}, policy: {}, confidence: {})"
-            "".format(self.action_name, self.policy, self.confidence)
+        return "ActionExecutionRejected(action: {}, policy: {}, confidence: {})".format(
+            self.action_name, self.policy, self.confidence
         )
 
     def __hash__(self) -> int:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/generator.py#L401'>rasa/shared/core/generator.py~L401</a>
```diff
 
             if num_active_trackers:
                 logger.debug(
-                    "Starting {} ... (with {} trackers)" "".format(
+                    "Starting {} ... (with {} trackers)".format(
                         phase_name, num_active_trackers
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/generator.py#L517'>rasa/shared/core/generator.py~L517</a>
```diff
                     phase = 0
                 else:
                     logger.debug(
-                        "Found {} unused checkpoints " "in current phase." "".format(
+                        "Found {} unused checkpoints in current phase.".format(
                             len(unused_checkpoints)
                         )
                     )
                     logger.debug(
-                        "Found {} active trackers " "for these checkpoints." "".format(
+                        "Found {} active trackers for these checkpoints.".format(
                             num_active_trackers
                         )
                     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/generator.py#L553'>rasa/shared/core/generator.py~L553</a>
```diff
                 augmented_trackers, self.config.max_number_of_augmented_trackers
             )
             logger.debug(
-                "Subsampled to {} augmented training trackers." "".format(
+                "Subsampled to {} augmented training trackers.".format(
                     len(augmented_trackers)
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/trackers.py#L634'>rasa/shared/core/trackers.py~L634</a>
```diff
         """
         if not isinstance(dialogue, Dialogue):
             raise ValueError(
-                f"story {dialogue} is not of type Dialogue. "
-                f"Have you deserialized it?"
+                f"story {dialogue} is not of type Dialogue. Have you deserialized it?"
             )
 
         self._reset()
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/story_reader.py#L83'>rasa/shared/core/training_data/story_reader/story_reader.py~L83</a>
```diff
         )
         if parsed_events is None:
             raise StoryParseError(
-                "Unknown event '{}'. It is Neither an event " "nor an action).".format(
+                "Unknown event '{}'. It is Neither an event nor an action).".format(
                     event_name
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L334'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L334</a>
```diff
 
         if not self.domain:
             logger.debug(
-                "Skipped validating if intent is in domain as domain " "is `None`."
+                "Skipped validating if intent is in domain as domain is `None`."
             )
             return
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/formats/dialogflow.py#L34'>rasa/shared/nlu/training_data/formats/dialogflow.py~L34</a>
```diff
 
         if fformat not in {DIALOGFLOW_INTENT, DIALOGFLOW_ENTITIES}:
             raise ValueError(
-                "fformat must be either {}, or {}" "".format(
+                "fformat must be either {}, or {}".format(
                     DIALOGFLOW_INTENT, DIALOGFLOW_ENTITIES
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/common.py#L308'>rasa/utils/common.py~L308</a>
```diff
         access_logger.addHandler(file_handler)
     if use_syslog:
         formatter = logging.Formatter(
-            "%(asctime)s [%(levelname)-5.5s] [%(process)d]" " %(message)s"
+            "%(asctime)s [%(levelname)-5.5s] [%(process)d] %(message)s"
         )
         socktype = SOCK_STREAM if syslog_protocol == TCP_PROTOCOL else SOCK_DGRAM
         syslog_handler = logging.handlers.SysLogHandler(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/endpoints.py#L33'>rasa/utils/endpoints.py~L33</a>
```diff
         return EndpointConfig.from_dict(content[endpoint_type])
     except FileNotFoundError:
         logger.error(
-            "Failed to read endpoint configuration " "from {}. No such file.".format(
+            "Failed to read endpoint configuration from {}. No such file.".format(
                 os.path.abspath(filename)
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_tracker_stores.py#L311'>tests/core/test_tracker_stores.py~L311</a>
```diff
     assert isinstance(tracker_store, InMemoryTrackerStore)
 
 
-async def _tracker_store_and_tracker_with_slot_set() -> (
-    Tuple[InMemoryTrackerStore, DialogueStateTracker]
-):
+async def _tracker_store_and_tracker_with_slot_set() -> Tuple[
+    InMemoryTrackerStore, DialogueStateTracker
+]:
     # returns an InMemoryTrackerStore containing a tracker with a slot set
 
     slot_key = "cuisine"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/recipes/test_default_recipe.py#L98'>tests/engine/recipes/test_default_recipe.py~L98</a>
```diff
         (
             "data/test_config/config_pretrained_embeddings_mitie.yml",
             "data/graph_schemas/config_pretrained_embeddings_mitie_train_schema.yml",
-            "data/graph_schemas/"
-            "config_pretrained_embeddings_mitie_predict_schema.yml",
+            "data/graph_schemas/config_pretrained_embeddings_mitie_predict_schema.yml",
             TrainingType.BOTH,
             False,
         ),
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L780'>tests/graph_components/validators/test_default_recipe_validator.py~L780</a>
```diff
     if should_warn:
         with pytest.warns(
             UserWarning,
-            match=(f"'{RulePolicy.__name__}' is not " "included in the model's "),
+            match=(f"'{RulePolicy.__name__}' is not included in the model's "),
         ) as records:
             validator.validate(importer)
     else:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L883'>tests/graph_components/validators/test_default_recipe_validator.py~L883</a>
```diff
     num_duplicates: bool,
     priority: int,
 ):
-    assert (
-        len(policy_types) >= priority + num_duplicates
-    ), f"This tests needs at least {priority+num_duplicates} many types."
+    assert len(policy_types) >= priority + num_duplicates, (
+        f"This tests needs at least {priority + num_duplicates} many types."
+    )
 
     # start with a schema where node i has priority i
     nodes = {
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L895'>tests/graph_components/validators/test_default_recipe_validator.py~L895</a>
```diff
 
     # give nodes p+1, .., p+num_duplicates-1 priority "priority"
     for idx in range(num_duplicates):
-        nodes[f"{priority+idx+1}"].config["priority"] = priority
+        nodes[f"{priority + idx + 1}"].config["priority"] = priority
 
     validator = DefaultV1RecipeValidator(graph_schema=GraphSchema(nodes))
     monkeypatch.setattr(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L992'>tests/graph_components/validators/test_default_recipe_validator.py~L992</a>
```diff
     with pytest.warns(
         UserWarning,
         match=(
-            "Found rule-based training data but no policy "
-            "supporting rule-based data."
+            "Found rule-based training data but no policy supporting rule-based data."
         ),
     ):
         validator.validate(importer)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_regex_featurizer.py#L44'>tests/nlu/featurizers/test_regex_featurizer.py~L44</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features,"
-    "labeled_tokens",
+    "sentence, expected_sequence_features, expected_sentence_features,labeled_tokens",
     [
         (
             "hey how are you today",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_regex_featurizer.py#L219'>tests/nlu/featurizers/test_regex_featurizer.py~L219</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features, "
-    "labeled_tokens",
+    "sentence, expected_sequence_features, expected_sentence_features, labeled_tokens",
     [
         (
             "lemonade and mapo tofu",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_regex_featurizer.py#L383'>tests/nlu/featurizers/test_regex_featurizer.py~L383</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features,"
-    "case_sensitive",
+    "sentence, expected_sequence_features, expected_sentence_features,case_sensitive",
     [
         ("Hey How are you today", [0.0, 0.0, 0.0], [0.0, 0.0, 0.0], True),
         ("Hey How are you today", [0.0, 1.0, 0.0], [0.0, 1.0, 0.0], False),
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_spacy_featurizer.py#L133'>tests/nlu/featurizers/test_spacy_featurizer.py~L133</a>
```diff
         vecs = ftr._features_for_doc(doc)
         vecs_capitalized = ftr._features_for_doc(doc_capitalized)
 
-        assert np.allclose(
-            vecs, vecs_capitalized, atol=1e-5
-        ), "Vectors are unequal for texts '{}' and '{}'".format(
-            e.get(TEXT), e.get(TEXT).capitalize()
+        assert np.allclose(vecs, vecs_capitalized, atol=1e-5), (
+            "Vectors are unequal for texts '{}' and '{}'".format(
+                e.get(TEXT), e.get(TEXT).capitalize()
+            )
         )
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+12 -10 lines across 5 files)</summary>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/1c2ef97810b3494119d3cae9d3d1106d227e15b8/src/snowflake/cli/_plugins/nativeapp/artifacts.py#L250'>src/snowflake/cli/_plugins/nativeapp/artifacts.py~L250</a>
```diff
     def __init__(self, *, project_root: Path, deploy_root: Path):
         # If a relative path ends up here, it's a bug in the app and can lead to other
         # subtle bugs as paths would be resolved relative to the current working directory.
-        assert (
-            project_root.is_absolute()
-        ), f"Project root {project_root} must be an absolute path."
-        assert (
-            deploy_root.is_absolute()
-        ), f"Deploy root {deploy_root} must be an absolute path."
+        assert project_root.is_absolute(), (
+            f"Project root {project_root} must be an absolute path."
+        )
+        assert deploy_root.is_absolute(), (
+            f"Deploy root {deploy_root} must be an absolute path."
+        )
 
         self._project_root: Path = resolve_without_follow(project_root)
         self._deploy_root: Path = resolve_without_follow(deploy_root)
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/1c2ef97810b3494119d3cae9d3d1106d227e15b8/src/snowflake/cli/_plugins/nativeapp/codegen/snowpark/python_processor.py#L433'>src/snowflake/cli/_plugins/nativeapp/codegen/snowpark/python_processor.py~L433</a>
```diff
         create_query += f"\nEXTERNAL_ACCESS_INTEGRATIONS=({', '.join(ensure_all_string_literals(extension_fn.external_access_integrations))})"
 
     if extension_fn.secrets:
-        create_query += f"""\nSECRETS=({', '.join([f"{ensure_string_literal(k)}={v}" for k, v in extension_fn.secrets.items()])})"""
+        create_query += f"""\nSECRETS=({", ".join([f"{ensure_string_literal(k)}={v}" for k, v in extension_fn.secrets.items()])})"""
 
     create_query += f"\nHANDLER={ensure_string_literal(extension_fn.handler)}"
 
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/1c2ef97810b3494119d3cae9d3d1106d227e15b8/src/snowflake/cli/_plugins/stage/manager.py#L106'>src/snowflake/cli/_plugins/stage/manager.py~L106</a>
```diff
 
     def get_standard_stage_path(self) -> str:
         path = self.get_full_stage_path(self.path)
-        return f"@{path}{'/'if self.is_directory and not path.endswith('/') else ''}"
+        return f"@{path}{'/' if self.is_directory and not path.endswith('/') else ''}"
 
     def get_standard_stage_directory_path(self) -> str:
         path = self.get_standard_stage_path()
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/1c2ef97810b3494119d3cae9d3d1106d227e15b8/tests/test_connection.py#L1312'>tests/test_connection.py~L1312</a>
```diff
             ["connection", "generate-jwt", "-c", "jwt"],
         )
         assert (
-            f'{attribute.capitalize().replace("_", " ")} is not set in the connection context'
+            f"{attribute.capitalize().replace('_', ' ')} is not set in the connection context"
             in result.output
         )
 
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/1c2ef97810b3494119d3cae9d3d1106d227e15b8/tests_e2e/test_installation.py#L68'>tests_e2e/test_installation.py~L68</a>
```diff
     assert "Initialized the new project in" in output
     for file in files_to_check:
         expected_generated_file = project_path / file
-        assert expected_generated_file.exists(), f"[{expected_generated_file}] does not exist. It should be generated from templates directory."
+        assert expected_generated_file.exists(), (
+            f"[{expected_generated_file}] does not exist. It should be generated from templates directory."
+        )
 
 
 @pytest.mark.e2e
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/cli.py#L870'>aiven/client/cli.py~L870</a>
```diff
                         types = spec["type"]
                         if isinstance(types, str) and types == "null":
                             print(
-                                "  {title}{description}\n" "     => --remove-option {name}".format(
+                                "  {title}{description}\n     => --remove-option {name}".format(
                                     name=name,
                                     title=spec["title"],
                                     description=description,
```
<a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/cli.py#L881'>aiven/client/cli.py~L881</a>
```diff
                                 types = [types]
                             type_str = " or ".join(t for t in types if t != "null")
                             print(
-                                "  {title}{description}\n" "     => -c {name}=<{type}>  {default}".format(
+                                "  {title}{description}\n     => -c {name}=<{type}>  {default}".format(
                                     name=name,
                                     type=type_str,
                                     default=default_desc,
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+22 -24 lines across 19 files)</summary>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/commands/init/interactive_init_flow.py#L544'>samcli/commands/init/interactive_init_flow.py~L544</a>
```diff
     click.echo(f"\n{question}")
 
     for index, option in enumerate(options_list):
-        click.echo(f"\t{index+1} - {option}")
+        click.echo(f"\t{index + 1} - {option}")
         click_choices.append(str(index + 1))
     choice = click.prompt(msg, type=click.Choice(click_choices), show_choices=False)
     return options_list[int(choice) - 1]
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/commands/init/interactive_init_flow.py#L557'>samcli/commands/init/interactive_init_flow.py~L557</a>
```diff
         click.echo("\nSelect your starter template")
         click_template_choices = []
         for index, template in enumerate(templates):
-            click.echo(f"\t{index+1} - {template['displayName']}")
+            click.echo(f"\t{index + 1} - {template['displayName']}")
             click_template_choices.append(str(index + 1))
         template_choice = click.prompt("Template", type=click.Choice(click_template_choices), show_choices=False)
         chosen_template = templates[int(template_choice) - 1]
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/lib/deploy/deployer.py#L513'>samcli/lib/deploy/deployer.py~L513</a>
```diff
             The maximum duration in minutes to wait for the deployment to complete.
         """
         sys.stdout.write(
-            "\n{} - Waiting for stack create/update " "to complete\n".format(
-                datetime.now().strftime("%Y-%m-%d %H:%M:%S")
-            )
+            "\n{} - Waiting for stack create/update to complete\n".format(datetime.now().strftime("%Y-%m-%d %H:%M:%S"))
         )
         sys.stdout.flush()
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py#L319'>samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py~L319</a>
```diff
         verify_intrinsic_type_list(
             value_list,
             IntrinsicResolver.FN_JOIN,
-            message="The list of values in {} after the " "delimiter must be a list".format(IntrinsicResolver.FN_JOIN),
+            message="The list of values in {} after the delimiter must be a list".format(IntrinsicResolver.FN_JOIN),
         )
 
         sanitized_value_list = [
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py#L487'>samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py~L487</a>
```diff
         verify_intrinsic_type_dict(
             top_level_value,
             IntrinsicResolver.FN_FIND_IN_MAP,
-            message="The TopLevelKey is missing in the Mappings dictionary in Fn::FindInMap " "for {}".format(
+            message="The TopLevelKey is missing in the Mappings dictionary in Fn::FindInMap for {}".format(
                 top_level_key
             ),
         )
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py#L496'>samcli/lib/intrinsic_resolver/intrinsic_property_resolver.py~L496</a>
```diff
         verify_non_null(
             second_level_value,
             IntrinsicResolver.FN_FIND_IN_MAP,
-            message="The SecondLevelKey is missing in the Mappings dictionary in Fn::FindInMap  " "for {}".format(
+            message="The SecondLevelKey is missing in the Mappings dictionary in Fn::FindInMap  for {}".format(
                 second_level_key
             ),
         )
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L11'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py~L11</a>
```diff
 def get_package_hierarchy(schema_name):
     path = "schema"
     if schema_name.startswith("aws.partner-"):
-        path = path + "." "aws.partner"
+        path = path + ".aws.partner"
         tail = schema_name[len("aws.partner-") :]
         path = path + "." + sanitize_name(tail)
         return path.lower()
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/local/invoke/test_integrations_cli.py#L183'>tests/integration/local/invoke/test_integrations_cli.py~L183</a>
```diff
         self.assertEqual(
             process_stdout.decode("utf-8"),
             "",
-            msg="The return statement in the LambdaFunction " "should never return leading to an empty string",
+            msg="The return statement in the LambdaFunction should never return leading to an empty string",
         )
 
     @pytest.mark.flaky(reruns=3)
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L9'>tests/integration/testdata/remote_invoke/lambda-fns/main.py~L9</a>
```diff
     print("value2 = " + event["key2"])
     print("value3 = " + event["key3"])
 
-    return {"message": f'{event["key1"]} {event["key3"]}'}
+    return {"message": f"{event['key1']} {event['key3']}"}
 
 
 def custom_env_var_echo_handler(event, context):
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/code/after/function/app.py#L11'>tests/integration/testdata/sync/code/after/function/app.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+2}",
+                "message": f"{layer_method() + 2}",
                 "message_from_layer": f"{layer_method()}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/code/before/function/app.py#L11'>tests/integration/testdata/sync/code/before/function/app.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+1}",
+                "message": f"{layer_method() + 1}",
                 "message_from_layer": f"{layer_method()}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/after/Python/function/app.py#L11'>tests/integration/testdata/sync/infra/after/Python/function/app.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+2}",
+                "message": f"{layer_method() + 2}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/before/Python/function/app.py#L11'>tests/integration/testdata/sync/infra/before/Python/function/app.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+1}",
+                "message": f"{layer_method() + 1}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/cdk/after/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py#L11'>tests/integration/testdata/sync/infra/cdk/after/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+2}",
+                "message": f"{layer_method() + 2}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/cdk/after/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py#L11'>tests/integration/testdata/sync/infra/cdk/after/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+2}",
+                "message": f"{layer_method() + 2}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/cdk/before/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py#L11'>tests/integration/testdata/sync/infra/cdk/before/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+1}",
+                "message": f"{layer_method() + 1}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/infra/cdk/before/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py#L11'>tests/integration/testdata/sync/infra/cdk/before/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py~L11</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+1}",
+                "message": f"{layer_method() + 1}",
                 "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
             }
         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/nested/after/child_stack/child_functions/child_function.py#L7'>tests/integration/testdata/sync/nested/after/child_stack/child_functions/child_function.py~L7</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}"}),
+        "body": json.dumps({"message": f"{layer_method() + 6}"}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/nested/after/root_function/root_function.py#L17'>tests/integration/testdata/sync/nested/after/root_function/root_function.py~L17</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}", "location": ip.text.replace("\n", "")}),
+        "body": json.dumps({"message": f"{layer_method() + 6}", "location": ip.text.replace("\n", "")}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/nested/before/child_stack/child_functions/child_function.py#L7'>tests/integration/testdata/sync/nested/before/child_stack/child_functions/child_function.py~L7</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+5}"}),
+        "body": json.dumps({"message": f"{layer_method() + 5}"}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/nested/before/root_function/root_function.py#L17'>tests/integration/testdata/sync/nested/before/root_function/root_function.py~L17</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}", "location": ip.text.replace("\n", "")}),
+        "body": json.dumps({"message": f"{layer_method() + 6}", "location": ip.text.replace("\n", "")}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/59abce37aec1450683d5310771d7799a27336ca5/tests/integration/testdata/sync/nested_intrinsics/before/child_stack/child_function/function/function.py#L20'>tests/integration/testdata/sync/nested_intrinsics/before/child_stack/child_function/function/function.py~L20</a>
```diff
         "statusCode": 200,
         "body": json.dumps(
             {
-                "message": f"{layer_method()+1}",
+                "message": f"{layer_method() + 1}",
                 "location": ip.text.replace("\n", ""),
                 # "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist() # checking external library call will succeed
             }
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+62 -60 lines across 33 files)</summary>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/Conversation_To_File.py#L251'>crazy_functions/Conversation_To_File.py~L251</a>
```diff
             [
                 "`" + hide_cwd(f) + f" ({gen_file_preview(f)})" + "`"
                 for f in glob.glob(
-                    f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+                    f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
                     recursive=True,
                 )
             ]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/Conversation_To_File.py#L294'>crazy_functions/Conversation_To_File.py~L294</a>
```diff
         [
             "`" + hide_cwd(f) + "`"
             for f in glob.glob(
-                f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+                f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
                 recursive=True,
             )
         ]
     )
     for f in glob.glob(
-        f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+        f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
         recursive=True,
     ):
         os.remove(f)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/Markdown_Translate.py#L193'>crazy_functions/Markdown_Translate.py~L193</a>
```diff
                 txt = txt.replace("/blob/", "/")
 
         r = requests.get(txt, proxies=proxies)
-        download_local = f'{get_log_folder(plugin_name="批量Markdown翻译")}/raw-readme-{gen_time_str()}.md'
-        project_folder = f'{get_log_folder(plugin_name="批量Markdown翻译")}'
+        download_local = f"{get_log_folder(plugin_name='批量Markdown翻译')}/raw-readme-{gen_time_str()}.md"
+        project_folder = f"{get_log_folder(plugin_name='批量Markdown翻译')}"
         with open(download_local, "wb+") as f:
             f.write(r.content)
         file_manifest = [download_local]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/Rag_Interface.py#L168'>crazy_functions/Rag_Interface.py~L168</a>
```diff
             HALF = REMEMBER_PREVIEW // 2
             i_say_to_remember = (
                 txt[:HALF]
-                + f" ...\n...(省略{len(txt_origin)-REMEMBER_PREVIEW}字)...\n... "
+                + f" ...\n...(省略{len(txt_origin) - REMEMBER_PREVIEW}字)...\n... "
                 + txt[-HALF:]
             )
             if (flags["original_input_len"] - flags["clipped_input_len"]) > HALF:
                 txt_clip = (
                     txt_clip
-                    + f" ...\n...(省略{len(txt_origin)-len(txt_clip)-HALF}字)...\n... "
+                    + f" ...\n...(省略{len(txt_origin) - len(txt_clip) - HALF}字)...\n... "
                     + txt[-HALF:]
                 )
         else:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/Social_Helper.py#L182'>crazy_functions/Social_Helper.py~L182</a>
```diff
 
         try:
             Explaination = "\n".join(
-                [f'{k}: {v["explain_to_llm"]}' for k, v in self.tools_to_select.items()]
+                [f"{k}: {v['explain_to_llm']}" for k, v in self.tools_to_select.items()]
             )
 
             class UserSociaIntention(BaseModel):
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/SourceCode_Analyse.py#L28'>crazy_functions/SourceCode_Analyse.py~L28</a>
```diff
     sys_prompt_array = []
     report_part_1 = []
 
-    assert (
-        len(file_manifest) <= 512
-    ), "源文件太多（超过512个）, 请缩减输入文件的数量。或者，您也可以选择删除此行警告，并修改代码拆分file_manifest列表，从而实现分批次处理。"
+    assert len(file_manifest) <= 512, (
+        "源文件太多（超过512个）, 请缩减输入文件的数量。或者，您也可以选择删除此行警告，并修改代码拆分file_manifest列表，从而实现分批次处理。"
+    )
     ############################## <第一步，逐个文件分析，多线程> ##################################
     for index, fp in enumerate(file_manifest):
         # 读取文件
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/SourceCode_Analyse.py#L43'>crazy_functions/SourceCode_Analyse.py~L43</a>
```diff
         )
         i_say_show_user = (
             prefix
-            + f"[{index+1}/{len(file_manifest)}] 请对下面的程序文件做一个概述: {fp}"
+            + f"[{index + 1}/{len(file_manifest)}] 请对下面的程序文件做一个概述: {fp}"
         )
         # 装载请求内容
         inputs_array.append(i_say)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/SourceCode_Analyse.py#L92'>crazy_functions/SourceCode_Analyse.py~L92</a>
```diff
         for index, content in enumerate(this_iteration_gpt_response_collection):
             if index % 2 == 0:
                 this_iteration_gpt_response_collection[index] = (
-                    f"{file_rel_path[index//2]}"  # 只保留文件名节省token
+                    f"{file_rel_path[index // 2]}"  # 只保留文件名节省token
                 )
         this_iteration_files = [
             os.path.relpath(fp, project_folder)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/SourceCode_Comment.py#L37'>crazy_functions/SourceCode_Comment.py~L37</a>
```diff
     history_array = []
     sys_prompt_array = []
 
-    assert (
-        len(file_manifest) <= 512
-    ), "源文件太多（超过512个）, 请缩减输入文件的数量。或者，您也可以选择删除此行警告，并修改代码拆分file_manifest列表，从而实现分批次处理。"
+    assert len(file_manifest) <= 512, (
+        "源文件太多（超过512个）, 请缩减输入文件的数量。或者，您也可以选择删除此行警告，并修改代码拆分file_manifest列表，从而实现分批次处理。"
+    )
 
     # 建立文件树
     file_tree_struct = FileNode("root", build_manifest=True)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/SourceCode_Comment.py#L59'>crazy_functions/SourceCode_Comment.py~L59</a>
```diff
         )
         i_say_show_user = (
             prefix
-            + f"[{index+1}/{len(file_manifest)}] 请用一句话对下面的程序文件做一个整体概述: {fp}"
+            + f"[{index + 1}/{len(file_manifest)}] 请用一句话对下面的程序文件做一个整体概述: {fp}"
         )
         # 装载请求内容
         MAX_TOKEN_SINGLE_FILE = 2560
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/VideoResource_GPT.py#L65'>crazy_functions/VideoResource_GPT.py~L65</a>
```diff
     tic_time = 8
     for i in range(tic_time):
         yield from update_ui_lastest_msg(
-            lastmsg=f"即将下载音频。等待{tic_time-i}秒后自动继续, 点击“停止”键取消此操作。",
+            lastmsg=f"即将下载音频。等待{tic_time - i}秒后自动继续, 点击“停止”键取消此操作。",
             chatbot=chatbot,
             history=[],
             delay=1,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/VideoResource_GPT.py#L94'>crazy_functions/VideoResource_GPT.py~L94</a>
```diff
     tic_time = 16
     for i in range(tic_time):
         yield from update_ui_lastest_msg(
-            lastmsg=f"即将下载视频。等待{tic_time-i}秒后自动继续, 点击“停止”键取消此操作。",
+            lastmsg=f"即将下载视频。等待{tic_time - i}秒后自动继续, 点击“停止”键取消此操作。",
             chatbot=chatbot,
             history=[],
             delay=1,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/VideoResource_GPT.py#L216'>crazy_functions/VideoResource_GPT.py~L216</a>
```diff
 
     # 展示候选资源
     candadate_display = "\n".join(
-        [f"{i+1}. {it['title']}" for i, it in enumerate(candadate_dictionary)]
+        [f"{i + 1}. {it['title']}" for i, it in enumerate(candadate_dictionary)]
     )
     chatbot.append((None, f"候选:\n\n{candadate_display}"))
     yield from update_ui(chatbot=chatbot, history=history)  # 刷新界面
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/crazy_utils.py#L166'>crazy_functions/crazy_utils.py~L166</a>
```diff
                 if retry_op > 0:
                     retry_op -= 1
                     mutable[0] += (
-                        f"[Local Message] 重试中，请稍等 {retry_times_at_unknown_error-retry_op}/{retry_times_at_unknown_error}：\n\n"
+                        f"[Local Message] 重试中，请稍等 {retry_times_at_unknown_error - retry_op}/{retry_times_at_unknown_error}：\n\n"
                     )
                     if ("Rate limit reached" in tb_str) or (
                         "Too Many Requests" in tb_str
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/crazy_utils.py#L366'>crazy_functions/crazy_utils.py~L366</a>
```diff
                         fail_info = ""
                     # 也许等待十几秒后，情况会好转
                     for i in range(wait):
-                        mutable[index][2] = f"{fail_info}等待重试 {wait-i}"
+                        mutable[index][2] = f"{fail_info}等待重试 {wait - i}"
                         time.sleep(1)
                     # 开始重试
                     if detect_timeout():
                         raise RuntimeError("检测到程序终止。")
                     mutable[index][2] = (
-                        f"重试中 {retry_times_at_unknown_error-retry_op}/{retry_times_at_unknown_error}"
+                        f"重试中 {retry_times_at_unknown_error - retry_op}/{retry_times_at_unknown_error}"
                     )
                     continue  # 返回重试
                 else:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/diagram_fns/file_tree.py#L85'>crazy_functions/diagram_fns/file_tree.py~L85</a>
```diff
             )
             p2 = """ --> """
             p3 = (
-                f"""{code+str(j)}[\"🗎{child.name}\"]"""
+                f"""{code + str(j)}[\"🗎{child.name}\"]"""
                 if child.is_leaf
-                else f"""{code+str(j)}[[\"📁{child.name}\"]]"""
+                else f"""{code + str(j)}[[\"📁{child.name}\"]]"""
             )
             edge_code = p1 + p2 + p3
             if edge_code in self.parenting_ship:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/gen_fns/gen_fns_shared.py#L22'>crazy_functions/gen_fns/gen_fns_shared.py~L22</a>
```diff
 
 def try_make_module(code, chatbot):
     module_file = "gpt_fn_" + gen_time_str().replace("-", "_")
-    fn_path = f'{get_log_folder(plugin_name="gen_plugin_verify")}/{module_file}.py'
+    fn_path = f"{get_log_folder(plugin_name='gen_plugin_verify')}/{module_file}.py"
     with open(fn_path, "w", encoding="utf8") as f:
         f.write(code)
     promote_file_to_downloadzone(fn_path, chatbot=chatbot)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/gen_fns/gen_fns_shared.py#L70'>crazy_functions/gen_fns/gen_fns_shared.py~L70</a>
```diff
     return_dict["traceback"] = ""
     try:
         module_file = "gpt_fn_" + gen_time_str().replace("-", "_")
-        fn_path = f'{get_log_folder(plugin_name="gen_plugin_run")}/{module_file}.py'
+        fn_path = f"{get_log_folder(plugin_name='gen_plugin_run')}/{module_file}.py"
         with open(fn_path, "w", encoding="utf8") as f:
             f.write(code)
         class_name = get_class_name(code)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/latex_fns/latex_toolbox.py#L473'>crazy_functions/latex_fns/latex_toolbox.py~L473</a>
```diff
             main_file = insert_abstract(main_file)
         match_opt1 = pattern_opt1.search(main_file)
         match_opt2 = pattern_opt2.search(main_file)
-        assert (match_opt1 is not None) or (
-            match_opt2 is not None
-        ), "Cannot find paper abstract section!"
+        assert (match_opt1 is not None) or (match_opt2 is not None), (
+            "Cannot find paper abstract section!"
+        )
     return main_file
 
 
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/pdf_fns/breakdown_txt.py#L82'>crazy_functions/pdf_fns/breakdown_txt.py~L82</a>
```diff
                 remain_txt_to_cut, remain_txt_to_cut_storage
             )
             process = fin_len / total_len
-            logger.info(f"正在文本切分 {int(process*100)}%")
+            logger.info(f"正在文本切分 {int(process * 100)}%")
             if len(remain_txt_to_cut.strip()) == 0:
                 break
     return res
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/pdf_fns/parse_pdf.py#L181'>crazy_functions/pdf_fns/parse_pdf.py~L181</a>
```diff
         for i, fragment in enumerate(section_frags):
             heading = section["heading"]
             if len(section_frags) > 1:
-                heading += f" Part-{i+1}"
+                heading += f" Part-{i + 1}"
             inputs_array.append(f"你需要翻译{heading}章节，内容如下: \n\n{fragment}")
             inputs_show_user_array.append(f"# {heading}\n\n{fragment}")
 
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/pdf_fns/parse_pdf_legacy.py#L96'>crazy_functions/pdf_fns/parse_pdf_legacy.py~L96</a>
```diff
         for i, k in enumerate(gpt_response_collection_md):
             if i % 2 == 0:
                 gpt_response_collection_md[i] = (
-                    f"\n\n---\n\n ## 原文[{i//2}/{len(gpt_response_collection_md)//2}]： \n\n {paper_fragments[i//2].replace('#', '')}  \n\n---\n\n ## 翻译[{i//2}/{len(gpt_response_collection_md)//2}]：\n "
+                    f"\n\n---\n\n ## 原文[{i // 2}/{len(gpt_response_collection_md) // 2}]： \n\n {paper_fragments[i // 2].replace('#', '')}  \n\n---\n\n ## 翻译[{i // 2}/{len(gpt_response_collection_md) // 2}]：\n "
                 )
             else:
                 gpt_response_collection_md[i] = gpt_response_collection_md[i]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/rag_fns/llama_index_worker.py#L143'>crazy_functions/rag_fns/llama_index_worker.py~L143</a>
```diff
         buf = "\n".join(
             (
                 [
-                    f"(No.{i+1} | score {n.score:.3f}): {n.text}"
+                    f"(No.{i + 1} | score {n.score:.3f}): {n.text}"
                     for i, n in enumerate(nodes)
                 ]
             )
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/crazy_functions/vt_fns/vt_call_plugin.py#L187'>crazy_functions/vt_fns/vt_call_plugin.py~L187</a>
```diff
     # ⭐ ⭐ ⭐ 执行插件
     fn = plugin["Function"]
     fn_name = fn.__name__
-    msg = f'{llm_kwargs["llm_model"]}为您选择了插件: `{fn_name}`\n\n插件说明：{plugin["Info"]}\n\n插件参数：{plugin_sel.plugin_arg}\n\n假如偏离了您的要求，按停止键终止。'
+    msg = f"{llm_kwargs['llm_model']}为您选择了插件: `{fn_name}`\n\n插件说明：{plugin['Info']}\n\n插件参数：{plugin_sel.plugin_arg}\n\n假如偏离了您的要求，按停止键终止。"
     yield from update_ui_lastest_msg(
         lastmsg=msg, chatbot=chatbot, history=history, delay=2
     )
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\345\207\275\346\225\260\345\212\250\346\200\201\347\224\237\346\210\220.py"#L269'>"a/crazy_functions/\345\207\275\346\225\260\345\212\250\346\200\201\347\224\237\346\210\220.py"~L269</a>
```diff
         if not traceback:
             traceback = trimmed_format_exc()
         yield from update_ui_lastest_msg(
-            f"第 {j+1}/{MAX_TRY} 次代码生成尝试, 失败了~ 别担心, 我们5秒后再试一次... \n\n此次我们的错误追踪是\n```\n{traceback}\n```\n",
+            f"第 {j + 1}/{MAX_TRY} 次代码生成尝试, 失败了~ 别担心, 我们5秒后再试一次... \n\n此次我们的错误追踪是\n```\n{traceback}\n```\n",
             chatbot,
             history,
             5,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\346\200\273\347\273\223word\346\226\207\346\241\243.py"#L57'>"a/crazy_functions/\346\200\273\347\273\223word\346\226\207\346\241\243.py"~L57</a>
```diff
         this_paper_history = []
         for i, paper_frag in enumerate(paper_fragments):
             i_say = f"请对下面的文章片段用中文做概述，文件名是{os.path.relpath(fp, project_folder)}，文章内容是 ```{paper_frag}```"
-            i_say_show_user = f"请对下面的文章片段做概述: {os.path.abspath(fp)}的第{i+1}/{len(paper_fragments)}个片段。"
+            i_say_show_user = f"请对下面的文章片段做概述: {os.path.abspath(fp)}的第{i + 1}/{len(paper_fragments)}个片段。"
             gpt_say = yield from request_gpt_model_in_new_thread_with_ui_alive(
                 inputs=i_say,
                 inputs_show_user=i_say_show_user,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\346\211\271\351\207\217\346\200\273\347\273\223PDF\346\226\207\346\241\243.py"#L76'>"a/crazy_functions/\346\211\271\351\207\217\346\200\273\347\273\223PDF\346\226\207\346\241\243.py"~L76</a>
```diff
         for i in range(n_fragment):
             NUM_OF_WORD = MAX_WORD_TOTAL // n_fragment
             i_say = f"Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} Chinese characters: {paper_fragments[i]}"
-            i_say_show_user = f"[{i+1}/{n_fragment}] Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} Chinese characters: {paper_fragments[i][:200]}"
+            i_say_show_user = f"[{i + 1}/{n_fragment}] Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} Chinese characters: {paper_fragments[i][:200]}"
             gpt_say = yield from request_gpt_model_in_new_thread_with_ui_alive(
                 i_say,
                 i_say_show_user,  # i_say=真正给chatgpt的提问， i_say_show_user=给用户看的提问
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\346\211\271\351\207\217\346\200\273\347\273\223PDF\346\226\207\346\241\243pdfminer.py"#L96'>"a/crazy_functions/\346\211\271\351\207\217\346\200\273\347\273\223PDF\346\226\207\346\241\243pdfminer.py"~L96</a>
```diff
         )
         i_say_show_user = (
             prefix
-            + f"[{index+1}/{len(file_manifest)}] 请对下面的文章片段做一个概述: {os.path.abspath(fp)}"
+            + f"[{index + 1}/{len(file_manifest)}] 请对下面的文章片段做一个概述: {os.path.abspath(fp)}"
         )
         chatbot.append((i_say_show_user, "[Local Message] waiting gpt response."))
         yield from update_ui(chatbot=chatbot, history=history)  # 刷新界面
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\347\220\206\350\247\243PDF\346\226\207\346\241\243\345\206\205\345\256\271.py"#L64'>"a/crazy_functions/\347\220\206\350\247\243PDF\346\226\207\346\241\243\345\206\205\345\256\271.py"~L64</a>
```diff
     for i in range(n_fragment):
         NUM_OF_WORD = MAX_WORD_TOTAL // n_fragment
         i_say = f"Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} words: {paper_fragments[i]}"
-        i_say_show_user = f"[{i+1}/{n_fragment}] Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} words: {paper_fragments[i][:200]} ...."
+        i_say_show_user = f"[{i + 1}/{n_fragment}] Read this section, recapitulate the content of this section with less than {NUM_OF_WORD} words: {paper_fragments[i][:200]} ...."
         gpt_say = yield from request_gpt_model_in_new_thread_with_ui_alive(
             i_say,
             i_say_show_user,  # i_say=真正给chatgpt的提问， i_say_show_user=给用户看的提问
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\347\224\237\346\210\220\345\207\275\346\225\260\346\263\250\351\207\212.py"#L22'>"a/crazy_functions/\347\224\237\346\210\220\345\207\275\346\225\260\346\263\250\351\207\212.py"~L22</a>
```diff
             file_content = f.read()
 
         i_say = f"请对下面的程序文件做一个概述，并对文件中的所有函数生成注释，使用markdown表格输出结果，文件名是{os.path.relpath(fp, project_folder)}，文件内容是 ```{file_content}```"
-        i_say_show_user = f"[{index+1}/{len(file_manifest)}] 请对下面的程序文件做一个概述，并对文件中的所有函数生成注释: {os.path.abspath(fp)}"
+        i_say_show_user = f"[{index + 1}/{len(file_manifest)}] 请对下面的程序文件做一个概述，并对文件中的所有函数生成注释: {os.path.abspath(fp)}"
         chatbot.append((i_say_show_user, "[Local Message] waiting gpt response."))
         yield from update_ui(chatbot=chatbot, history=history)  # 刷新界面
 
```
<a href='https://github.com/binary-husky/gpt_academic/blob/060af0d2e6db22e36d6b53b59c8eae56f91d3928/"a/crazy_functions/\347\224\237\346\210\220\345\244\232\347\247\215Mermaid\345\233\276\350\241\250.py"#L205'>"a/crazy_functions/\347\224\237\346\210\220\345\244\232\347\247\215Mermaid\345\233\276\350\241\250.py"~L205</a>
```diff
     for i in ra...*[Comment body truncated]*

---

_Label `do-not-merge` added by @MichaReiser on 2024-10-24 13:33_

---

_Marked ready for review by @MichaReiser on 2024-12-23 12:34_

---

_Label `do-not-merge` removed by @MichaReiser on 2025-01-03 13:02_

---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-03 13:02_

---

_Merged by @MichaReiser on 2025-01-03 13:16_

---

_Closed by @MichaReiser on 2025-01-03 13:16_

---

_Branch deleted on 2025-01-03 13:16_

---
