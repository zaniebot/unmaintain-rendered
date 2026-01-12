```yaml
number: 8921
title: Insert trailing comma when function breaks with single argument
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/comma
created_at: 2023-11-30T04:29:36Z
updated_at: 2023-12-01T02:49:29Z
url: https://github.com/astral-sh/ruff/pull/8921
synced_at: 2026-01-12T15:55:27Z
```

# Insert trailing comma when function breaks with single argument

---

_@charliermarsh_

## Summary

Given:

```python
def _example_function_xxxxxxx(
    variable: Optional[List[str]]
) -> List[example.ExampleConfig]:
    pass
```

We should be inserting a trailing comma after the argument (as long as it's a single-argument function). This was an inconsistency with Black, but also led to some internal inconsistencies, whereby we added the comma if the argument contained a trailing end-of-line comment, but not otherwise.

Closes https://github.com/astral-sh/ruff/issues/8912.

## Test Plan

`cargo test`

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
| home-assistant |           0.99955 |             10596 |               213 |
| poetry         |           0.99917 |               317 |                13 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99957 |              1459 |                36 |

---

_@MichaReiser approved on 2023-11-30 04:31_

---

_Comment by @github-actions[bot] on 2023-11-30 04:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+270 -270 lines in 86 files in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/594c12ad5230522a12008445217e71726cc60611/disnake/audit_logs.py#L215'>disnake/audit_logs.py~L215</a>
```diff
 
 
 def _list_transformer(
-    func: Callable[[AuditLogEntry, Any], T]
+    func: Callable[[AuditLogEntry, Any], T],
 ) -> Callable[[AuditLogEntry, Any], List[T]]:
     def _transform(entry: AuditLogEntry, data: Any) -> List[T]:
         if not data:
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+47 -47 lines across 19 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/brokers/pika.py#L94'>rasa/core/brokers/pika.py~L94</a>
```diff
 
     @staticmethod
     def _get_queues_from_args(
-        queues_arg: Union[List[Text], Tuple[Text, ...], Text, None]
+        queues_arg: Union[List[Text], Tuple[Text, ...], Text, None],
     ) -> Union[List[Text], Tuple[Text, ...]]:
         """Get queues for this event broker.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/console.py#L113'>rasa/core/channels/console.py~L113</a>
```diff
 
 
 async def _get_user_input(
-    previous_response: Optional[Dict[str, Any]]
+    previous_response: Optional[Dict[str, Any]],
 ) -> Optional[Text]:
     button_response = None
     if previous_response is not None:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/evaluation/marker_stats.py#L12'>rasa/core/evaluation/marker_stats.py~L12</a>
```diff
 
 
 def compute_statistics(
-    values: List[Union[float, int]]
+    values: List[Union[float, int]],
 ) -> Dict[Text, Union[int, float, np.floating]]:
     """Computes some statistics over the given numbers."""
     return {
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/migrate.py#L83'>rasa/core/migrate.py~L83</a>
```diff
 
 
 def _migrate_form_slots(
-    domain: Dict[Text, Any]
+    domain: Dict[Text, Any],
 ) -> Tuple[Dict[Any, Dict[str, Any]], Optional[Any]]:
     updated_slots = domain.get(KEY_SLOTS, {})
     forms = domain.get(KEY_FORMS, {})
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L468'>rasa/core/policies/ted_policy.py~L468</a>
```diff
 
     @staticmethod
     def _should_extract_entities(
-        entity_tags: List[List[Dict[Text, List[Features]]]]
+        entity_tags: List[List[Dict[Text, List[Features]]]],
     ) -> bool:
         for turns_tags in entity_tags:
             for turn_tags in turns_tags:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1360'>rasa/core/policies/ted_policy.py~L1360</a>
```diff
 
     @staticmethod
     def _compute_dialogue_indices(
-        tf_batch_data: Dict[Text, Dict[Text, List[tf.Tensor]]]
+        tf_batch_data: Dict[Text, Dict[Text, List[tf.Tensor]]],
     ) -> None:
         dialogue_lengths = tf.cast(tf_batch_data[DIALOGUE][LENGTH][0], dtype=tf.int32)
         # wrap in a list, because that's the structure of tf_batch_data
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1399'>rasa/core/policies/ted_policy.py~L1399</a>
```diff
 
     @staticmethod
     def _collect_label_attribute_encodings(
-        all_labels_encoded: Dict[Text, tf.Tensor]
+        all_labels_encoded: Dict[Text, tf.Tensor],
     ) -> tf.Tensor:
         # Initialize with at least one attribute first
         # so that the subsequent TF ops are simplified.
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/unexpected_intent_policy.py#L807'>rasa/core/policies/unexpected_intent_policy.py~L807</a>
```diff
 
     @staticmethod
     def _compute_label_quantiles(
-        label_id_scores: Dict[int, Dict[Text, List[float]]]
+        label_id_scores: Dict[int, Dict[Text, List[float]]],
     ) -> Dict[int, List[float]]:
         """Computes multiple quantiles for each label id.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L123'>rasa/core/processor.py~L123</a>
```diff
 
     @staticmethod
     def _load_model(
-        model_path: Union[Text, Path]
+        model_path: Union[Text, Path],
     ) -> Tuple[Text, ModelMetadata, GraphRunner]:
         """Unpacks a model from a given path using the graph model loader."""
         try:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/utils.py#L215'>rasa/core/utils.py~L215</a>
```diff
 
 
 def read_endpoints_from_path(
-    endpoints_path: Optional[Union[Path, Text]] = None
+    endpoints_path: Optional[Union[Path, Text]] = None,
 ) -> AvailableEndpoints:
     """Get `AvailableEndpoints` object from specified path.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/utils.py#L282'>rasa/core/utils.py~L282</a>
```diff
 
 
 def _lock_store_is_multi_worker_compatible(
-    lock_store: Union[EndpointConfig, LockStore, None]
+    lock_store: Union[EndpointConfig, LockStore, None],
 ) -> bool:
     if isinstance(lock_store, InMemoryLockStore):
         return False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L338'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L338</a>
```diff
 
     @staticmethod
     def _convert_attribute_tokens_to_texts(
-        attribute_tokens: Dict[Text, List[List[Text]]]
+        attribute_tokens: Dict[Text, List[List[Text]]],
     ) -> Dict[Text, List[Text]]:
         attribute_texts = {}
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L679'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L679</a>
```diff
 
     @staticmethod
     def _is_any_model_trained(
-        attribute_vocabularies: Dict[Text, Optional[Dict[Text, int]]]
+        attribute_vocabularies: Dict[Text, Optional[Dict[Text, int]]],
     ) -> bool:
         """Check if any model got trained."""
         return any(value is not None for value in attribute_vocabularies.values())
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L353'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L353</a>
```diff
 
     @staticmethod
     def _build_feature_to_index_map(
-        feature_vocabulary: Dict[Tuple[int, Text], Set[Text]]
+        feature_vocabulary: Dict[Tuple[int, Text], Set[Text]],
     ) -> Dict[Tuple[int, Text], Dict[Text, int]]:
         """Creates a nested dictionary for mapping raw features to indices.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/nlu/utils/spacy_utils.py#L195'>rasa/nlu/utils/spacy_utils.py~L195</a>
```diff
 
     @staticmethod
     def _filter_training_samples_by_content(
-        indexed_training_samples: List[Tuple[int, Text]]
+        indexed_training_samples: List[Tuple[int, Text]],
     ) -> Tuple[List[Tuple[int, Text]], List[Tuple[int, Text]]]:
         """Separates empty training samples from content bearing ones."""
         docs_to_pipe = list(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L465'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L465</a>
```diff
 
     @staticmethod
     def _parse_raw_entities(
-        raw_entities: Union[List[Dict[Text, Text]], List[Text]]
+        raw_entities: Union[List[Dict[Text, Text]], List[Text]],
     ) -> List[Dict[Text, Optional[Text]]]:
         final_entities = []
         for entity in raw_entities:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/importers/importer.py#L443'>rasa/shared/importers/importer.py~L443</a>
```diff
 
     @staticmethod
     def _get_nlu_data_with_responses(
-        responses: Dict[Text, List[Dict[Text, Any]]]
+        responses: Dict[Text, List[Dict[Text, Any]]],
     ) -> TrainingData:
         """Construct training data object with only the responses supplied.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/models.py#L239'>rasa/utils/tensorflow/models.py~L239</a>
```diff
 
     @staticmethod
     def _dynamic_signature(
-        batch_in: Union[Tuple[tf.Tensor, ...], Tuple[np.ndarray, ...]]
+        batch_in: Union[Tuple[tf.Tensor, ...], Tuple[np.ndarray, ...]],
     ) -> List[List[tf.TensorSpec]]:
         element_spec = []
         for tensor in batch_in:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/tensorflow/models.py#L377'>rasa/utils/tensorflow/models.py~L377</a>
```diff
         """Recursively replaces empty list or np array with None in a dictionary."""
 
         def _recurse(
-            x: Union[Dict[Text, Any], List[Any], np.ndarray]
+            x: Union[Dict[Text, Any], List[Any], np.ndarray],
         ) -> Optional[Union[Dict[Text, Any], List[Any], np.ndarray]]:
             if isinstance(x, dict):
                 return {k: _recurse(v) for k, v in x.items()}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_rasa_data.py#L208'>tests/cli/test_rasa_data.py~L208</a>
```diff
 
 
 def test_data_validate_failed_to_load_domain(
-    run_in_simple_project_with_no_domain: Callable[..., RunResult]
+    run_in_simple_project_with_no_domain: Callable[..., RunResult],
 ):
     result = run_in_simple_project_with_no_domain(
         "data",
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_rasa_train.py#L243'>tests/cli/test_rasa_train.py~L243</a>
```diff
 
 
 def test_train_dry_run_force(
-    run_in_simple_project_with_model: Callable[..., RunResult]
+    run_in_simple_project_with_model: Callable[..., RunResult],
 ):
     temp_dir = os.getcwd()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_rasa_train.py#L497'>tests/cli/test_rasa_train.py~L497</a>
```diff
 
 
 def test_train_nlu_finetune_with_model(
-    run_in_simple_project_with_model: Callable[..., RunResult]
+    run_in_simple_project_with_model: Callable[..., RunResult],
 ):
     temp_dir = os.getcwd()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/graph_components/validators/test_finetuning_validator.py#L70'>tests/graph_components/validators/test_finetuning_validator.py~L70</a>
```diff
 
 @pytest.fixture
 def get_validation_method(
-    get_finetuning_validator: Callable[[bool, bool], FinetuningValidator]
+    get_finetuning_validator: Callable[[bool, bool], FinetuningValidator],
 ) -> Callable[[bool, bool, bool, bool, GraphSchema], ValidationMethodType]:
     def inner(
         finetuning: bool,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/nlu/classifiers/test_diet_classifier.py#L471'>tests/nlu/classifiers/test_diet_classifier.py~L471</a>
```diff
 
 
 async def test_margin_loss_is_not_normalized(
-    create_train_load_and_process_diet: Callable[..., Message]
+    create_train_load_and_process_diet: Callable[..., Message],
 ):
     _, parsed_message = create_train_load_and_process_diet(
         {
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/nlu/classifiers/test_diet_classifier.py#L498'>tests/nlu/classifiers/test_diet_classifier.py~L498</a>
```diff
 
 @pytest.mark.timeout(120, func_only=True)
 async def test_set_random_seed(
-    create_train_load_and_process_diet: Callable[..., Message]
+    create_train_load_and_process_diet: Callable[..., Message],
 ):
     """test if train result is the same for two runs of tf embedding"""
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/nlu/classifiers/test_diet_classifier.py#L618'>tests/nlu/classifiers/test_diet_classifier.py~L618</a>
```diff
 
 
 async def test_train_fails_with_zero_eval_num_epochs(
-    create_diet: Callable[..., DIETClassifier]
+    create_diet: Callable[..., DIETClassifier],
 ):
     with pytest.raises(InvalidConfigException):
         with pytest.warns(UserWarning) as warning:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/nlu/extractors/test_regex_entity_extractor.py#L264'>tests/nlu/extractors/test_regex_entity_extractor.py~L264</a>
```diff
 
 
 def test_train_process_and_load_with_empty_model(
-    create_or_load_extractor: Callable[..., RegexEntityExtractor]
+    create_or_load_extractor: Callable[..., RegexEntityExtractor],
 ):
     extractor = create_or_load_extractor({})
     with pytest.warns(UserWarning):
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/nlu/extractors/test_regex_entity_extractor.py#L276'>tests/nlu/extractors/test_regex_entity_extractor.py~L276</a>
```diff
 
 
 def test_process_does_not_overwrite_any_entities(
-    create_or_load_extractor: Callable[..., RegexEntityExtractor]
+    create_or_load_extractor: Callable[..., RegexEntityExtractor],
 ):
     pre_existing_entity = {
         ENTITY_ATTRIBUTE_TYPE: "person",
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/0ceba7306971f3d8247b933f14c854c9ba599762/performance_history_analysis.py#L40'>performance_history_analysis.py~L40</a>
```diff
 
 
 def _print_summary_performance_descending(
-    commits_with_results: List[Tuple[Commit, float]]
+    commits_with_results: List[Tuple[Commit, float]],
 ) -> None:
     commits_with_diffs: List[Tuple[Commit, Decimal]] = []
     last_result: Optional[Decimal] = None
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+11 -11 lines across 5 files)</summary>
<p>
<pre>ruff format --no-preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L765'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py~L765</a>
```diff
 
 
 def prepare_outputs_for_categories(
-    records: List[Dict[str, Any]]
+    records: List[Dict[str, Any]],
 ) -> Tuple[List[Dict[str, Optional[Any]]], List[Dict[str, Optional[Any]]]]:
     """
     Prepares human readables and context output for 'bmc-remedy-category-details-get' command.
```
<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/ExpanseV2/Scripts/ExpanseAggregateAttributionCI/ExpanseAggregateAttributionCI.py#L14'>Packs/ExpanseV2/Scripts/ExpanseAggregateAttributionCI/ExpanseAggregateAttributionCI.py~L14</a>
```diff
 
 
 def deconstruct_entry(
-    ServiceNowCMDBContext: Dict[str, Any]
+    ServiceNowCMDBContext: Dict[str, Any],
 ) -> Tuple[str, Optional[str], Optional[str], Optional[str], Optional[str], Optional[str]]:
     """
     deconstruct_entry
```
<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory.py#L1426'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory.py~L1426</a>
```diff
 
 
 def validate_and_parse_curatedrule_detection_start_end_time(
-    args: Dict[str, Any]
+    args: Dict[str, Any],
 ) -> Tuple[Optional[datetime], Optional[datetime]]:
     """
     Validate and return detection_start_time and detection_end_time as per Chronicle Backstory or \
```
<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/PAN-OS/Integrations/Panorama/Panorama.py#L10144'>Packs/PAN-OS/Integrations/Panorama/Panorama.py~L10144</a>
```diff
 
     @staticmethod
     def get_all_security_rules_in_container(
-        container: Union[Panorama, Firewall, DeviceGroup, Template, Vsys]
+        container: Union[Panorama, Firewall, DeviceGroup, Template, Vsys],
     ) -> List[SecurityRule]:
         """
         Gets all the security rule objects from the given VSYS or device group and return them.
```
<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/PAN-OS/Integrations/Panorama/Panorama.py#L13768'>Packs/PAN-OS/Integrations/Panorama/Panorama.py~L13768</a>
```diff
 
 
 def parse_incident_entries(
-    incident_entries: List[Dict[str, Any]]
+    incident_entries: List[Dict[str, Any]],
 ) -> Tuple[Dict[str, str] | None, datetime | None, List[Dict[str, Any]]]:
     """parses raw incident entries of a specific log type query into basic context incidents.
 
```
<a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Tests/scripts/test_playbooks_report.py#L17'>Tests/scripts/test_playbooks_report.py~L17</a>
```diff
 
 
 def calculate_test_playbooks_results(
-    test_playbooks_result_files_list: dict[str, Path]
+    test_playbooks_result_files_list: dict[str, Path],
 ) -> tuple[dict[str, dict[str, Any]], set[str]]:
     playbooks_results: dict[str, dict[str, Any]] = {}
     server_versions = set()
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/latchbio/latch/blob/965a74f9c4aaefc1f8cb33b7113e429535f5fb8a/latch_cli/snakemake/workflow.py#L912'>latch_cli/snakemake/workflow.py~L912</a>
```diff
 
 
 def annotated_str_to_json(
-    x: Union[str, snakemake.io._IOFile, snakemake.io.AnnotatedString]
+    x: Union[str, snakemake.io._IOFile, snakemake.io.AnnotatedString],
 ) -> MaybeAnnotatedStrJson:
     if not isinstance(x, (snakemake.io.AnnotatedString, snakemake.io._IOFile)):
         return x
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -4 lines across 2 files)</summary>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/0e8174fbb89972690ea4e825d727d2ddf7653c73/pandas/core/apply.py#L1778'>pandas/core/apply.py~L1778</a>
```diff
 
 
 def _make_unique_kwarg_list(
-    seq: Sequence[tuple[Any, Any]]
+    seq: Sequence[tuple[Any, Any]],
 ) -> Sequence[tuple[Any, Any]]:
     """
     Uniquify aggfunc name of the pairs in the order list
```
<a href='https://github.com/pandas-dev/pandas/blob/0e8174fbb89972690ea4e825d727d2ddf7653c73/pandas/plotting/_matplotlib/core.py#L465'>pandas/plotting/_matplotlib/core.py~L465</a>
```diff
     @final
     @staticmethod
     def _iter_data(
-        data: DataFrame | dict[Hashable, Series | DataFrame]
+        data: DataFrame | dict[Hashable, Series | DataFrame],
     ) -> Iterator[tuple[Hashable, np.ndarray]]:
         for col, values in data.items():
             # This was originally written to use values.values before EAs
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+5 -5 lines across 2 files)</summary>
<p>

<a href='https://github.com/prefecthq/prefect/blob/3c8f953316435f7dfd44a7c21a7ae1ea531c66e6/src/prefect/_internal/pydantic/annotations/pendulum.py#L35'>src/prefect/_internal/pydantic/annotations/pendulum.py~L35</a>
```diff
             return pendulum.parse(value)
 
         def to_str(
-            value: t.Union[pendulum.DateTime, pendulum.Date, pendulum.Time]
+            value: t.Union[pendulum.DateTime, pendulum.Date, pendulum.Time],
         ) -> str:
             return value.isoformat()
 
```
<a href='https://github.com/prefecthq/prefect/blob/3c8f953316435f7dfd44a7c21a7ae1ea531c66e6/src/prefect/server/database/query_components.py#L1256'>src/prefect/server/database/query_components.py~L1256</a>
```diff
             # help smooth over those differences.
 
             def edges(
-                value: Union[str, Sequence[UUID], Sequence[str], None]
+                value: Union[str, Sequence[UUID], Sequence[str], None],
             ) -> List[UUID]:
                 if not value:
                     return []
```
<a href='https://github.com/prefecthq/prefect/blob/3c8f953316435f7dfd44a7c21a7ae1ea531c66e6/src/prefect/server/database/query_components.py#L1265'>src/prefect/server/database/query_components.py~L1265</a>
```diff
                 return [Edge(id=id) for id in value]
 
             def time(
-                value: Union[str, datetime.datetime, None]
+                value: Union[str, datetime.datetime, None],
             ) -> Optional[pendulum.DateTime]:
                 if not value:
                     return None
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+3 -3 lines across 1 file)</summary>
<p>

<a href='https://github.com/python/mypy/blob/69b31445280d7c495fa0268b24fc558bcbe74505/mypy/types.py#L3087'>mypy/types.py~L3087</a>
```diff
 
 @overload
 def get_proper_types(
-    types: list[Type | None] | tuple[Type | None, ...]
+    types: list[Type | None] | tuple[Type | None, ...],
 ) -> list[ProperType | None]:
     ...
 
 
 def get_proper_types(
-    types: list[Type] | list[Type | None] | tuple[Type | None, ...]
+    types: list[Type] | list[Type | None] | tuple[Type | None, ...],
 ) -> list[ProperType] | list[ProperType | None]:
     if isinstance(types, list):
         typelist = types
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+165 -165 lines across 43 files)</summary>
<p>

<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/compiler/plugin_pb2.pyi#L89'>stubs/protobuf/google/protobuf/compiler/plugin_pb2.pyi~L89</a>
```diff
     """The generator parameter passed on the command-line."""
     @property
     def proto_file(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[google.protobuf.descriptor_pb2.FileDescriptorProto]:
         """FileDescriptorProtos for all files in files_to_generate and everything
         they import.  The files will appear in topological order, so each file
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/compiler/plugin_pb2.pyi#L283'>stubs/protobuf/google/protobuf/compiler/plugin_pb2.pyi~L283</a>
```diff
     """
     @property
     def file(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CodeGeneratorResponse.File]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L242'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L242</a>
```diff
     def enum_type(self) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___EnumDescriptorProto]: ...
     @property
     def extension_range(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___DescriptorProto.ExtensionRange]: ...
     @property
     def oneof_decl(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___OneofDescriptorProto]: ...
     @property
     def options(self) -> global___MessageOptions: ...
     @property
     def reserved_range(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___DescriptorProto.ReservedRange]: ...
     @property
     def reserved_name(self) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[builtins.str]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L309'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L309</a>
```diff
     UNINTERPRETED_OPTION_FIELD_NUMBER: builtins.int
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L638'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L638</a>
```diff
     def options(self) -> global___EnumOptions: ...
     @property
     def reserved_range(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___EnumDescriptorProto.EnumReservedRange]:
         """Range of reserved numeric values. Reserved numeric values may not be used
         by enum values in the same enum declaration. Reserved ranges may not
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L986'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L986</a>
```diff
     """
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here.
         See the documentation for the "Options" section above.
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1176'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1176</a>
```diff
     """
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1350'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1350</a>
```diff
     """For Google-internal migration only. Do not use."""
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1415'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1415</a>
```diff
     UNINTERPRETED_OPTION_FIELD_NUMBER: builtins.int
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1446'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1446</a>
```diff
     """
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1482'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1482</a>
```diff
     """
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1517'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1517</a>
```diff
     """
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1580'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1580</a>
```diff
     idempotency_level: global___MethodOptions.IdempotencyLevel.ValueType
     @property
     def uninterpreted_option(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption]:
         """The parser stores options it doesn't recognize here. See above."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1656'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1656</a>
```diff
     AGGREGATE_VALUE_FIELD_NUMBER: builtins.int
     @property
     def name(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___UninterpretedOption.NamePart]: ...
     identifier_value: builtins.str
     """The value of the uninterpreted option, in whatever type the tokenizer
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/protobuf/google/protobuf/descriptor_pb2.pyi#L1964'>stubs/protobuf/google/protobuf/descriptor_pb2.pyi~L1964</a>
```diff
     ANNOTATION_FIELD_NUMBER: builtins.int
     @property
     def annotation(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GeneratedCodeInfo.Annotation]:
         """An Annotation connects some span of text in generated code to an element
         of its generating .proto file.
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi#L31'>stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi~L31</a>
```diff
     def pathing(self) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___RequestQueryPathing]: ...
     @property
     def abilities(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___RequestQueryAvailableAbilities]: ...
     @property
     def placements(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___RequestQueryBuildingPlacement]: ...
     ignore_resource_requirements: builtins.bool
     """Ignores requirements like food, minerals and so on."""
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi#L77'>stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi~L77</a>
```diff
     def pathing(self) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ResponseQueryPathing]: ...
     @property
     def abilities(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ResponseQueryAvailableAbilities]: ...
     @property
     def placements(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ResponseQueryBuildingPlacement]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi#L179'>stubs/s2clientprotocol/s2clientprotocol/query_pb2.pyi~L179</a>
```diff
     UNIT_TYPE_ID_FIELD_NUMBER: builtins.int
     @property
     def abilities(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.common_pb2.AvailableAbility]: ...
     unit_tag: builtins.int
     unit_type_id: builtins.int
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/raw_pb2.pyi#L124'>stubs/s2clientprotocol/s2clientprotocol/raw_pb2.pyi~L124</a>
```diff
         """The playable cells."""
     @property
     def start_locations(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.common_pb2.Point2D]:
         """Possible start locations for players."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi#L1620'>stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi~L1620</a>
```diff
     RESULT_FIELD_NUMBER: builtins.int
     @property
     def result(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[s2clientprotocol.error_pb2.ActionResult.ValueType]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi#L1768'>stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi~L1768</a>
```diff
     EFFECTS_FIELD_NUMBER: builtins.int
     @property
     def abilities(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.data_pb2.AbilityData]: ...
     @property
     def units(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.data_pb2.UnitTypeData]: ...
     @property
     def upgrades(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.data_pb2.UpgradeData]: ...
     @property
     def buffs(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.data_pb2.BuffData]: ...
     @property
     def effects(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.data_pb2.EffectData]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi#L2210'>stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi~L2210</a>
```diff
     DEBUG_FIELD_NUMBER: builtins.int
     @property
     def debug(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.debug_pb2.DebugCommand]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi#L2630'>stubs/s2clientprotocol/s2clientprotocol/sc2api_pb2.pyi~L2630</a>
```diff
     def alerts(self) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[global___Alert.ValueType]: ...
     @property
     def abilities(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.common_pb2.AvailableAbility]:
         """Abilities available in the selection. Enabled if in this list, disabled otherwise."""
     @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/s2clientprotocol/s2clientprotocol/spatial_pb2.pyi#L661'>stubs/s2clientprotocol/s2clientprotocol/spatial_pb2.pyi~L661</a>
```diff
     SELECTION_ADD_FIELD_NUMBER: builtins.int
     @property
     def selection_screen_coord(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[s2clientprotocol.common_pb2.RectangleI]:
         """Eventually this should not be an array, but a single field (multiple would be cheating)."""
     selection_add: builtins.bool
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L351'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L351</a>
```diff
     batch_group_count: builtins.int
     @property
     def slice_dimensions(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___HloInstructionProto.SliceDimensions]: ...
     exponent_bits: builtins.int
     """The bit sizes for a reduce-precision operation."""
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L425'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L425</a>
```diff
     """Backend configuration for the instruction. Has backend-specific meaning."""
     @property
     def replica_groups(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.compiler.xla.xla_data_pb2.ReplicaGroup]:
         """Cross replica op fields."""
     all_reduce_id: builtins.int
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L450'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L450</a>
```diff
         """Precision configuration for the instruction. Has backend-specific meaning."""
     @property
     def source_target_pairs(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.compiler.xla.xla_data_pb2.SourceTarget]:
         """Collective permute field."""
     @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L466'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L466</a>
```diff
     """
     @property
     def operand_shapes_with_layout(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.compiler.xla.xla_data_pb2.ShapeProto]: ...
     @property
     def triangular_solve_options(self) -> tensorflow.compiler.xla.xla_data_pb2.TriangularSolveOptions:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L483'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L483</a>
```diff
     """
     @property
     def output_operand_aliasing(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         tensorflow.compiler.xla.xla_data_pb2.OutputOperandAliasing
     ]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L912'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L912</a>
```diff
     SEQUENCES_FIELD_NUMBER: builtins.int
     @property
     def sequences(
-        self
+        self,
     ) -> google.protobuf.internal.containers.MessageMap[builtins.int, global___HloScheduleProto.InstructionSequence]:
         """Map from computation id to sequence."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L986'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L986</a>
```diff
     ENTRIES_FIELD_NUMBER: builtins.int
     @property
     def entries(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         global___HloInputOutputAliasProto.AliasEntryProto
     ]: ...
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1072'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1072</a>
```diff
     ENTRIES_FIELD_NUMBER: builtins.int
     @property
     def entries(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___DynamicParameterBindingProto.Binding]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1215'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1215</a>
```diff
     def dynamic_parameter_binding(self) -> global___DynamicParameterBindingProto: ...
     @property
     def cross_program_prefetches(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CrossProgramPrefetch]: ...
     is_dynamic: builtins.bool
     """True if the module contains dynamic computation."""
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1223'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1223</a>
```diff
     def spmd_output_sharding(self) -> tensorflow.compiler.xla.xla_data_pb2.OpSharding: ...
     @property
     def spmd_parameters_shardings(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.compiler.xla.xla_data_pb2.OpSharding]: ...
     use_auto_spmd_partitioning: builtins.bool
     """Uses AutoSharding pass or not."""
     @property
     def profile_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___HloModuleProto.ProfileInfo]:
         """Profile information for the HLO module."""
     @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1431'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1431</a>
```diff
     color: builtins.int
     @property
     def assigned(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___BufferAllocationProto.Assigned]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1573'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1573</a>
```diff
     BUFFER_ALLOCATION_INDEX_FIELD_NUMBER: builtins.int
     @property
     def events(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___HeapSimulatorTrace.Event]: ...
     whole_module_simulation: builtins.bool
     buffer_allocation_index: builtins.int
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1657'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1657</a>
```diff
     HEAP_SIMULATOR_TRACES_FIELD_NUMBER: builtins.int
     @property
     def logical_buffers(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___LogicalBufferProto]: ...
     @property
     def buffer_aliases(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___BufferAssignmentProto.BufferAlias]: ...
     @property
     def buffer_allocations(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___BufferAllocationProto]: ...
     @property
     def heap_simulator_traces(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___HeapSimulatorTrace]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L1740'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L1740</a>
```diff
         """The hlo graph."""
     @property
     def arguments(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.compiler.xla.xla_data_pb2.LiteralProto]:
         """The arguments passed to the graph."""
     @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi#L2007'>stubs/tensorflow/tensorflow/compiler/xla/service/hlo_pb2.pyi~L2007</a>
```diff
     RESULT_XLA_SHAPE_FIELD_NUMBER: builtins.int
     @property
     def buffers(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         global___EntryFunctionAttributes.BufferParameterAttributes
     ]: ...
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L440'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L440</a>
```diff
     DIMENSIONS_FIELD_NUMBER: builtins.int
     @property
     def dimensions(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___PaddingConfig.PaddingConfigDimension]:
         """The padding configuration for all dimensions."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L506'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L506</a>
```diff
     DYNAMIC_SHAPE_METADATA_PREFIX_BYTES_FIELD_NUMBER: builtins.int
     @property
     def dim_level_types(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[global___DimLevelType.ValueType]:
         """The dimension level type list for this array, specifying the way in which
         each array dimension is represented in memory. If this list is empty, the
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L757'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L757</a>
```diff
         COMPILATION_EVENT_FIELD_NUMBER: builtins.int
         @property
         def profile_type(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[global___ProfileType.ValueType]:
             """The type of optimization profiles that this operation contains."""
         relative_speedup: builtins.float
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L1116'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L1116</a>
```diff
     computation_count: builtins.int
     @property
     def computation_devices(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         global___DeviceAssignmentProto.ComputationDevice
     ]: ...
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L1835'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L1835</a>
```diff
         """
     @property
     def last_tile_dims(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[global___OpSharding.Type.ValueType]:
         """This field is used to represented the sharding type of each subgroup.
         For example, sharding={devices=[2,2,2,2]0,1,2,...,15 last_tile_dims={
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi#L1955'>stubs/tensorflow/tensorflow/compiler/xla/xla_data_pb2.pyi~L1955</a>
```diff
     OPERAND_PRECISION_FIELD_NUMBER: builtins.int
     @property
     def operand_precision(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[global___PrecisionConfig.Precision.ValueType]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/attr_value_pb2.pyi#L57'>stubs/tensorflow/tensorflow/core/framework/attr_value_pb2.pyi~L57</a>
```diff
             """ "list(bool)" """
         @property
         def type(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[
             tensorflow.core.framework.types_pb2.DataType.ValueType
         ]:
             """ "list(type)" """
         @property
         def shape(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
             tensorflow.core.framework.tensor_shape_pb2.TensorShapeProto
         ]:
             """ "list(shape)" """
         @property
         def tensor(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
             tensorflow.core.framework.tensor_pb2.TensorProto
         ]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/cost_graph_pb2.pyi#L112'>stubs/tensorflow/tensorflow/core/framework/cost_graph_pb2.pyi~L112</a>
```diff
         """The id of the node. Node ids are only unique inside a partition."""
         @property
         def input_info(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CostGraphDef.Node.InputInfo]: ...
         @property
         def output_info(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CostGraphDef.Node.OutputInfo]: ...
         temporary_memory_size: builtins.int
         """Temporary memory used by this node."""
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/cost_graph_pb2.pyi#L228'>stubs/tensorflow/tensorflow/core/framework/cost_graph_pb2.pyi~L228</a>
```diff
     def node(self) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CostGraphDef.Node]: ...
     @property
     def cost(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CostGraphDef.AggregatedCost]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/dataset_pb2.pyi#L73'>stubs/tensorflow/tensorflow/core/framework/dataset_pb2.pyi~L73</a>
```diff
     """Compressed tensor bytes for all components of the element."""
     @property
     def component_metadata(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___CompressedComponentMetadata]:
         """Metadata for the components of the element."""
     version: builtins.int
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/dataset_pb2.pyi#L108'>stubs/tensorflow/tensorflow/core/framework/dataset_pb2.pyi~L108</a>
```diff
     COMPONENTS_FIELD_NUMBER: builtins.int
     @property
     def components(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         tensorflow.core.framework.tensor_pb2.TensorProto
     ]: ...
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi#L35'>stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi~L35</a>
```diff
     def gradient(self) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GradientDef]: ...
     @property
     def registered_gradients(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___RegisteredGradient]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi#L112'>stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi~L112</a>
```diff
         ATTR_FIELD_NUMBER: builtins.int
         @property
         def attr(
-            self
+            self,
         ) -> google.protobuf.internal.containers.MessageMap[builtins.str, tensorflow.core.framework.attr_value_pb2.AttrValue]: ...
         def __init__(
             self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi#L201'>stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi~L201</a>
```diff
         """
     @property
     def attr(
-        self
+        self,
     ) -> google.protobuf.internal.containers.MessageMap[builtins.str, tensorflow.core.framework.attr_value_pb2.AttrValue]:
         """Attributes specific to this function definition."""
     @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi#L220'>stubs/tensorflow/tensorflow/core/framework/function_pb2.pyi~L220</a>
```diff
         """
     @property
     def node_def(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.core.framework.node_def_pb2.NodeDef]:
         """The body of the function.  Unlike the NodeDefs in a GraphDef, attrs
         may have values of type `placeholder` and the `input` field uses
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/graph_pb2.pyi#L32'>stubs/tensorflow/tensorflow/core/framework/graph_pb2.pyi~L32</a>
```diff
     LIBRARY_FIELD_NUMBER: builtins.int
     @property
     def node(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.core.framework.node_def_pb2.NodeDef]: ...
     @property
     def versions(self) -> tensorflow.core.framework.versions_pb2.VersionDef:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/graph_transfer_info_pb2.pyi#L131'>stubs/tensorflow/tensorflow/core/framework/graph_transfer_info_pb2.pyi~L131</a>
```diff
     node_id: builtins.int
     @property
     def node_input(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferNodeInput]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/graph_transfer_info_pb2.pyi#L245'>stubs/tensorflow/tensorflow/core/framework/graph_transfer_info_pb2.pyi~L245</a>
```diff
     DESTINATION_FIELD_NUMBER: builtins.int
     @property
     def node_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferNodeInfo]: ...
     @property
     def const_node_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferConstNodeInfo]: ...
     @property
     def node_input_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferNodeInputInfo]: ...
     @property
     def node_output_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferNodeOutputInfo]: ...
     @property
     def graph_input_node_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferGraphInputNodeInfo]:
         """Input Node parameters of transferred graph"""
     @property
     def graph_output_node_info(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GraphTransferGraphOutputNodeInfo]: ...
     destination: global___GraphTransferInfo.Destination.ValueType
     """Destination of graph transfer"""
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/kernel_def_pb2.pyi#L58'>stubs/tensorflow/tensorflow/core/framework/kernel_def_pb2.pyi~L58</a>
```diff
     """Type of device this kernel runs on."""
     @property
     def constraint(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___KernelDef.AttrConstraint]: ...
     @property
     def host_memory_arg(self) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[builtins.str]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/model_pb2.pyi#L178'>stubs/tensorflow/tensorflow/core/framework/model_pb2.pyi~L178</a>
```diff
         """
         @property
         def parameters(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ModelProto.Node.Parameter]:
             """Parameters of this node."""
         input_processing_time_sum: builtins.float
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/node_def_pb2.pyi#L128'>stubs/tensorflow/tensorflow/core/framework/node_def_pb2.pyi~L128</a>
```diff
     """
     @property
     def attr(
-        self
+        self,
     ) -> google.protobuf.internal.containers.MessageMap[builtins.str, tensorflow.core.framework.attr_value_pb2.AttrValue]:
         """Operation-specific graph-construction-time configuration.
         Note that this should include all attrs defined in the
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/op_def_pb2.pyi#L71'>stubs/tensorflow/tensorflow/core/framework/op_def_pb2.pyi~L71</a>
```diff
         """
         @property
         def handle_data(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
             tensorflow.core.framework.resource_handle_pb2.ResourceHandleProto.DtypeAndShape
         ]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/optimized_function_graph_pb2.pyi#L64'>stubs/tensorflow/tensorflow/core/framework/optimized_function_graph_pb2.pyi~L64</a>
```diff
         """
     @property
     def ret_types(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[tensorflow.core.framework.types_pb2.DataType.ValueType]:
         """Return node types of the function. This is an output of graph
         preprocessing.
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/resource_handle_pb2.pyi#L70'>stubs/tensorflow/tensorflow/core/framework/resource_handle_pb2.pyi~L70</a>
```diff
     """
     @property
     def dtypes_and_shapes(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ResourceHandleProto.DtypeAndShape]:
         """Data types and shapes for the underlying resource."""
     def __init__(
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi#L61'>stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi~L61</a>
```diff
     """The bytes that are not deallocated."""
     @property
     def allocation_records(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___AllocationRecord]:
         """The allocation and deallocation timeline."""
     allocator_bytes_in_use: builtins.int
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi#L142'>stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi~L142</a>
```diff
     device_persistent_memory_size: builtins.int
     @property
     def device_persistent_tensor_alloc_ids(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedScalarFieldContainer[builtins.int]: ...
     def __init__(
         self,
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi#L216'>stubs/tensorflow/tensorflow/core/framework/step_stats_pb2.pyi~L216</a>
```diff
     thread_id: builtins.int
     @property
     def referenced_tensor(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         tensorflow.core.framework.allocation_description_pb2.AllocationDescription
     ]: ...
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/framework/tensor_pb2.pyi#L106'>stubs/tensorflow/tensorflow/core/framework/tensor_pb2.pyi~L106</a>
```diff
         """
     @property
     def resource_handle_val(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[
         tensorflow.core.framework.resource_handle_pb2.ResourceHandleProto
     ]:
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi#L106'>stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi~L106</a>
```diff
         GPU_HOST_MEM_DISALLOW_GROWTH_FIELD_NUMBER: builtins.int
         @property
         def virtual_devices(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___GPUOptions.Experimental.VirtualDevices]:
             """The multi virtual device settings. If empty (not set), it will create
             single virtual device on each visible GPU, according to the settings
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi#L1080'>stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi~L1080</a>
```diff
     """
     @property
     def session_inter_op_thread_pool(
-        self
+        self,
     ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[global___ThreadPoolOptionProto]:
         """This option is experimental - it may be replaced with a different mechanism
         in the future.
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi#L1412'>stubs/tensorflow/tensorflow/core/protobuf/config_pb2.pyi~L1412</a>
```diff
         POST_OPTIMIZATION_GRAPH_FIELD_NUMBER: builtins.int
         @property
         def partition_graphs(
-            self
+            self,
         ) -> google.protobuf.internal.containers.RepeatedCompositeFieldContainer[tensorflow.core.framework.graph_pb2.GraphDef]:
             """TODO(nareshmodi): Include some sort of function/cache-key identifier?"""
         @property
```
<a href='https://github.com/python/typeshed/blob/78052791df327b716406f3f78962c4247cfe381f/stubs/...*[Comment body truncated]*

---

_Comment by @charliermarsh on 2023-11-30 05:01_

This leads to decreased similarity in most cases (but the test fixtures show improvement). I'm pretty lost on what Black's logic is for inserting the trailing comma vs. not. For example, Black adds a trailing comma to the second example here, but not the first...

```python
def _start_listening(
    ws_event_callback: Callable[str, int]
) -> List[example.ExampleConfig]:
    pass

def _start_listening(
    ws_event_callback: Callable[List[str]]
) -> List[example.ExampleConfig]:
    pass
```

---

_Comment by @zanieb on 2023-11-30 05:20_

+1 for consistency

---

_Comment by @charliermarsh on 2023-11-30 05:47_

I'm now leaning towards proceeding with this change. My best guess is that Black omits the trailing comma if the _annotation_ contains a comma, which seems incorrect to me.

---

_@konstin approved on 2023-11-30 08:29_

+1 for the consistency, since it changes a bunch of files and decreases black compatibility, do we want to guard it behind e.g. preview?

---

_Comment by @charliermarsh on 2023-11-30 16:00_

Another idea is that we could only add the trailing comma if the argument itself breaks...

So:

```python
# Add a trailing comma here...
def _start_listening(
    ws_event_callback: List[
        CallableButImagineItIsVeryLong[str, int]
    ],
) -> List[example.ExampleConfig]:
    pass

# But not here...
def _start_listening(
    ws_event_callback: Callable[List[str]]
) -> List[example.ExampleConfig]:
    pass
```

This would still be a deviation from Black in some cases, but it would avoid adding the trailing comma in the event that more arguments are added (since those arguments would stay on the same line).

Eh, on second thought, I don't see why that would be better.

(Filed an issue on: https://github.com/psf/black/issues/4080)

---

_Comment by @zanieb on 2023-11-30 16:11_

I think this is a bug and we don't need to gate it with preview; perhaps if the formatter was not in beta, I would feel differently.



---

_Merged by @charliermarsh on 2023-12-01 02:49_

---

_Closed by @charliermarsh on 2023-12-01 02:49_

---

_Branch deleted on 2023-12-01 02:49_

---
