```yaml
number: 22742
title: "Stabilize `allow_newline_after_block_open`"
type: pull_request
state: open
author: dylwil3
labels:
  - breaking
  - formatter
assignees: []
base: 2026-style
head: stabilize-allow_newline_after_block_open
created_at: 2026-01-19T20:43:37Z
updated_at: 2026-01-19T20:54:32Z
url: https://github.com/astral-sh/ruff/pull/22742
synced_at: 2026-01-19T21:33:07Z
```

# Stabilize `allow_newline_after_block_open`

---

_@dylwil3_

_No description provided._

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-19 20:43_

---

_Label `breaking` added by @dylwil3 on 2026-01-19 20:43_

---

_Label `formatter` added by @dylwil3 on 2026-01-19 20:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:54_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+430 -0 lines in 292 files in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+2 -0 lines across 2 files)</summary>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/api/async_migration.py#L52'>housewatch/api/async_migration.py~L52</a>
```diff
 
     @action(methods=["POST"], detail=True)
     def trigger(self, request, **kwargs):
+
         migration = self.get_object()
 
         migration.status = MigrationStatus.Starting
```
<a href='https://github.com/PostHog/HouseWatch/blob/9b126c0d588c1dc33045dfea3dd76f8abf888d29/housewatch/async_migrations/runner.py#L27'>housewatch/async_migrations/runner.py~L27</a>
```diff
 def start_async_migration(
     migration: AsyncMigration, ignore_posthog_version=False
 ) -> bool:
+
     if migration.status not in [MigrationStatus.Starting, MigrationStatus.NotStarted]:
         logger.error(f"Initial check failed for async migration {migration.name}")
         return False
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+152 -0 lines across 87 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/data/test_classes/custom_slots.py#L28'>data/test_classes/custom_slots.py~L28</a>
```diff
         value_reset_delay: Optional[int] = None,
         influence_conversation: bool = True,
     ) -> None:
+
         super().__init__(
             name=name,
             initial_value=initial_value,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/actions/actions.py#L28'>examples/reminderbot/actions/actions.py~L28</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         dispatcher.utter_message("I will remind you in 5 seconds.")
 
         date = datetime.datetime.now() + datetime.timedelta(seconds=5)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/actions/actions.py#L56'>examples/reminderbot/actions/actions.py~L56</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         name = next(tracker.get_slot("PERSON"), "someone")
         dispatcher.utter_message(f"Remember to call {name}!")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/actions/actions.py#L71'>examples/reminderbot/actions/actions.py~L71</a>
```diff
     async def run(
         self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
     ) -> List[Dict[Text, Any]]:
+
         conversation_id = tracker.sender_id
 
         dispatcher.utter_message(f"The ID of this conversation is '{conversation_id}'.")
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/actions/actions.py#L98'>examples/reminderbot/actions/actions.py~L98</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         plant = next(tracker.get_latest_entity_values("plant"), "someone")
         dispatcher.utter_message(f"Your {plant} needs some water!")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/actions/actions.py#L113'>examples/reminderbot/actions/actions.py~L113</a>
```diff
     async def run(
         self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
     ) -> List[Dict[Text, Any]]:
+
         dispatcher.utter_message("Okay, I'll cancel all your reminders.")
 
         # Cancel all reminders
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/examples/reminderbot/callback_server.py#L4'>examples/reminderbot/callback_server.py~L4</a>
```diff
 
 
 def create_app() -> Sanic:
+
     bot_app = Sanic("callback_server", configure_logging=False)
 
     @bot_app.post("/bot")
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/export.py#L177'>rasa/cli/export.py~L177</a>
```diff
 
 
 async def _export_trackers(args: argparse.Namespace) -> None:
+
     _assert_max_timestamp_is_greater_than_min_timestamp(args)
 
     endpoints = rasa.core.utils.read_endpoints_from_path(args.endpoints)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/run.py#L58'>rasa/cli/run.py~L58</a>
```diff
 
 
 def _validate_model_path(model_path: Text, parameter: Text, default: Text) -> Text:
+
     if model_path is not None and not os.path.exists(model_path):
         reason_str = f"'{model_path}' not found."
         if model_path is None:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/actions/action.py#L658'>rasa/core/actions/action.py~L658</a>
```diff
 
 class RemoteAction(Action):
     def __init__(self, name: Text, action_endpoint: Optional[EndpointConfig]) -> None:
+
         self._name = name
         self.action_endpoint = action_endpoint
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/agent.py#L503'>rasa/core/agent.py~L503</a>
```diff
         return await self.handle_message(msg)
 
     def _set_fingerprint(self, fingerprint: Optional[Text] = None) -> None:
+
         if fingerprint:
             self.fingerprint = fingerprint
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/botframework.py#L48'>rasa/core/channels/botframework.py~L48</a>
```diff
         bot: Text,
         service_url: Text,
     ) -> None:
+
         service_url = (
             f"{service_url}/" if not service_url.endswith("/") else service_url
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/callback.py#L22'>rasa/core/channels/callback.py~L22</a>
```diff
         return "callback"
 
     def __init__(self, endpoint: EndpointConfig) -> None:
+
         self.callback_endpoint = endpoint
         super().__init__()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/facebook.py#L33'>rasa/core/channels/facebook.py~L33</a>
```diff
         page_access_token: Text,
         on_new_message: Callable[[UserMessage], Awaitable[Any]],
     ) -> None:
+
         self.on_new_message = on_new_message
         self.client = MessengerClient(page_access_token)
         self.last_message: Dict[Text, Any] = {}
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/facebook.py#L171'>rasa/core/channels/facebook.py~L171</a>
```diff
         return "facebook"
 
     def __init__(self, messenger_client: MessengerClient) -> None:
+
         self.messenger_client = messenger_client
         super().__init__()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/facebook.py#L349'>rasa/core/channels/facebook.py~L349</a>
```diff
     def blueprint(
         self, on_new_message: Callable[[UserMessage], Awaitable[Any]]
     ) -> Blueprint:
+
         fb_webhook = Blueprint("fb_webhook", __name__)
 
         # noinspection PyUnusedLocal
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L40'>rasa/core/channels/hangouts.py~L40</a>
```diff
 
     @staticmethod
     def _text_card(message: Dict[Text, Any]) -> Dict:
+
         card = {
             "cards": [
                 {
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L192'>rasa/core/channels/hangouts.py~L192</a>
```diff
 
     @classmethod
     def from_credentials(cls, credentials: Optional[Dict[Text, Any]]) -> InputChannel:
+
         if credentials:
             return cls(credentials.get("project_id"))
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L204'>rasa/core/channels/hangouts.py~L204</a>
```diff
         hangouts_room_added_intent_name: Optional[Text] = "/room_added",
         hangouts_removed_intent_name: Optional[Text] = "/bot_removed",
     ) -> None:
+
         self.project_id = project_id
         self.hangouts_user_added_intent_name = hangouts_user_added_intent_name
         self.hangouts_room_added_intent_name = hangouts_room_added_intent_name
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L226'>rasa/core/channels/hangouts.py~L226</a>
```diff
 
     @staticmethod
     def _extract_sender(req: Request) -> Text:
+
         if req.json["type"] == "MESSAGE":
             return req.json["message"]["sender"]["displayName"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L233'>rasa/core/channels/hangouts.py~L233</a>
```diff
 
     # noinspection PyMethodMayBeStatic
     def _extract_message(self, req: Request) -> Text:
+
         if req.json["type"] == "MESSAGE":
             message = req.json["message"]["text"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/hangouts.py#L292'>rasa/core/channels/hangouts.py~L292</a>
```diff
 
         @custom_webhook.route("/webhook", methods=["POST"])
         async def receive(request: Request) -> HTTPResponse:
+
             if self.project_id:
                 token = request.headers.get("Authorization", "").replace("Bearer ", "")
                 self._check_token(token)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/rocketchat.py#L114'>rasa/core/channels/rocketchat.py~L114</a>
```diff
         )
 
     def __init__(self, user: Text, password: Text, server_url: Text) -> None:
+
         self.user = user
         self.password = password
         self.server_url = server_url
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/webexteams.py#L78'>rasa/core/channels/webexteams.py~L78</a>
```diff
         sender_id: Optional[Text],
         metadata: Optional[Dict],
     ) -> Any:
+
         try:
             out_channel = self.get_output_channel()
             user_msg = UserMessage(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/featurizers/single_state_featurizer.py#L186'>rasa/core/featurizers/single_state_featurizer.py~L186</a>
```diff
         precomputations: Optional[MessageContainerForCoreFeaturization],
         sparse: bool = False,
     ) -> Dict[Text, List[Features]]:
+
         # Remove entities from possible attributes
         attributes = set(
             attribute for attribute in sub_state.keys() if attribute != ENTITIES
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/nlg/callback.py#L62'>rasa/core/nlg/callback.py~L62</a>
```diff
     """
 
     def __init__(self, endpoint_config: EndpointConfig) -> None:
+
         self.nlg_endpoint = endpoint_config
 
     async def generate(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/rule_policy.py#L951'>rasa/core/policies/rule_policy.py~L951</a>
```diff
     def _find_action_from_loop_happy_path(
         tracker: DialogueStateTracker,
     ) -> Tuple[Optional[Text], Optional[Text]]:
+
         active_loop_name = tracker.active_loop_name
         if active_loop_name is None:
             return None, None
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L1926'>rasa/core/policies/ted_policy.py~L1926</a>
```diff
         text_output: tf.Tensor,
         text_sequence_lengths: tf.Tensor,
     ) -> tf.Tensor:
+
         text_transformed, text_mask, text_sequence_lengths = self._reshape_for_entities(
             tf_batch_data,
             dialogue_transformer_output,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ted_policy.py#L2128'>rasa/core/policies/ted_policy.py~L2128</a>
```diff
         text_output: tf.Tensor,
         text_sequence_lengths: tf.Tensor,
     ) -> Tuple[tf.Tensor, tf.Tensor]:
+
         text_transformed, _, text_sequence_lengths = self._reshape_for_entities(
             tf_batch_data,
             dialogue_transformer_output,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/processor.py#L785'>rasa/core/processor.py~L785</a>
```diff
     async def _handle_message_with_tracker(
         self, message: UserMessage, tracker: DialogueStateTracker
     ) -> None:
+
         if message.parse_data:
             parse_data = message.parse_data
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L702'>rasa/core/test.py~L702</a>
```diff
     event: ActionExecuted,
     fail_on_prediction_errors: bool,
 ) -> Tuple[EvaluationStore, PolicyPrediction, Optional[EntityEvaluationResult]]:
+
     action_executed_eval_store = EvaluationStore()
 
     expected_action_name = event.action_name
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L820'>rasa/core/test.py~L820</a>
```diff
     List[Dict[Text, Any]],
     List[EntityEvaluationResult],
 ]:
+
     processor = agent.processor
     if agent.processor is not None:
         processor = agent.processor
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/recipes/default_recipe.py#L668'>rasa/engine/recipes/default_recipe.py~L668</a>
```diff
         preprocessors: List[Text],
         train_nodes: Dict[Text, SchemaNode],
     ) -> Dict[Text, SchemaNode]:
+
         predict_config = copy.deepcopy(config)
         predict_nodes = {}
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/storage/local_model_storage.py#L221'>rasa/engine/storage/local_model_storage.py~L221</a>
```diff
 
     @staticmethod
     def _persist_metadata(metadata: ModelMetadata, temporary_directory: Path) -> None:
+
         rasa.shared.utils.io.dump_obj_as_json_to_file(
             temporary_directory / MODEL_ARCHIVE_METADATA_FILE, metadata.as_dict()
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/validation.py#L485'>rasa/engine/validation.py~L485</a>
```diff
     parent_return_type: TypeAnnotation,
     required_type: TypeAnnotation,
 ) -> None:
+
     if not typing_utils.issubtype(parent_return_type, required_type):
         parent_node_text = ""
         if parent_node:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L506'>rasa/nlu/classifiers/diet_classifier.py~L506</a>
```diff
     def _extract_features(
         self, message: Message, attribute: Text
     ) -> Dict[Text, Union[scipy.sparse.spmatrix, np.ndarray]]:
+
         (
             sparse_sequence_features,
             sparse_sentence_features,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L775'>rasa/nlu/classifiers/diet_classifier.py~L775</a>
```diff
         sparse_feature_sizes: Dict[Text, Dict[Text, List[int]]],
         label_attribute: Optional[Text] = None,
     ) -> Dict[Text, Dict[Text, List[int]]]:
+
         if label_attribute in sparse_feature_sizes:
             del sparse_feature_sizes[label_attribute]
         return sparse_feature_sizes
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1260'>rasa/nlu/classifiers/diet_classifier.py~L1260</a>
```diff
         config: Dict[Text, Any],
         finetune_mode: bool,
     ) -> "RasaModel":
+
         predict_data_example = RasaModelData(
             label_key=model_data_example.label_key,
             data={
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1502'>rasa/nlu/classifiers/diet_classifier.py~L1502</a>
```diff
         sequence_feature_lengths: tf.Tensor,
         name: Text,
     ) -> tf.Tensor:
+
         x, _ = self._tf_layers[f"feature_combining_layer.{name}"](
             (sequence_features, sentence_features, sequence_feature_lengths),
             training=self._training,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1683'>rasa/nlu/classifiers/diet_classifier.py~L1683</a>
```diff
         return loss
 
     def _update_label_metrics(self, loss: tf.Tensor, acc: tf.Tensor) -> None:
+
         self.intent_loss.update_state(loss)
         self.intent_acc.update_state(acc)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/diet_classifier.py#L1841'>rasa/nlu/classifiers/diet_classifier.py~L1841</a>
```diff
         combined_sequence_sentence_feature_lengths: tf.Tensor,
         text_transformed: tf.Tensor,
     ) -> Dict[Text, tf.Tensor]:
+
         if self.all_labels_embed is None:
             raise ValueError(
                 "The model was not prepared for prediction. "
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L323'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L323</a>
```diff
         return texts
 
     def _sentence_encoding_of_text(self, batch: List[Text]) -> np.ndarray:
+
         return self.sentence_encoding_signature(tf.convert_to_tensor(batch))[
             "default"
         ].numpy()
 
     def _sequence_encoding_of_text(self, batch: List[Text]) -> np.ndarray:
+
         return self.sequence_encoding_signature(tf.convert_to_tensor(batch))[
             "sequence_encoding"
         ].numpy()
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L407'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L407</a>
```diff
             )
 
     def _tokenize(self, sentence: Text) -> Any:
+
         return self.tokenize_signature(tf.convert_to_tensor([sentence]))[
             "default"
         ].numpy()
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L98'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L98</a>
```diff
         return ["sklearn"]
 
     def _load_count_vect_params(self) -> None:
+
         # Use shared vocabulary between text and all other attributes of Message
         self.use_shared_vocab = self._config["use_shared_vocab"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/persistor.py#L95'>rasa/nlu/persistor.py~L95</a>
```diff
 
     @staticmethod
     def _tar_name(model_name: Text, include_extension: bool = True) -> Text:
+
         ext = ".tar.gz" if include_extension else ""
         return f"{model_name}{ext}"
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L625'>rasa/nlu/selectors/response_selector.py~L625</a>
```diff
         config: Dict[Text, Any],
         finetune_mode: bool = False,
     ) -> "RasaModel":
+
         predict_data_example = RasaModelData(
             label_key=model_data_example.label_key,
             data={
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/selectors/response_selector.py#L721'>rasa/nlu/selectors/response_selector.py~L721</a>
```diff
             logger.debug(f"  {metric} ({name})")
 
     def _update_label_metrics(self, loss: tf.Tensor, acc: tf.Tensor) -> None:
+
         self.response_loss.update_state(loss)
         self.response_acc.update_state(acc)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/test.py#L1568'>rasa/nlu/test.py~L1568</a>
```diff
 
 
 def _contains_entity_labels(entity_results: List[EntityEvaluationResult]) -> bool:
+
     for result in entity_results:
         if result.entity_targets or result.entity_predictions:
             return True
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/server.py#L234'>rasa/server.py~L234</a>
```diff
         async def decorated(
             request: Request, *args: Any, **kwargs: Any
         ) -> response.HTTPResponse:
+
             provided = request.args.get("token", None)
 
             # noinspection PyProtectedMember
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L317'>rasa/shared/core/events.py~L317</a>
```diff
     def from_parameters(
         parameters: Dict[Text, Any], default: Optional[Type["Event"]] = None
     ) -> Optional["Event"]:
+
         event_name = parameters.get("event")
         if event_name is None:
             return None
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1020'>rasa/shared/core/events.py~L1020</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["SlotSet"]]:
+
         slots = []
         for slot_key, slot_val in parameters.items():
             slots.append(SlotSet(slot_key, slot_val))
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1221'>rasa/shared/core/events.py~L1221</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["ReminderScheduled"]]:
+
         trigger_date_time = parser.parse(parameters.get("date_time"))
 
         return [
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/events.py#L1472'>rasa/shared/core/events.py~L1472</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["FollowupAction"]]:
+
         return [
             FollowupAction(
                 parameters.get("name"),
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_reader/story_reader.py#L111'>rasa/shared/core/training_data/story_reader/story_reader.py~L111</a>
```diff
     def _add_checkpoint(
         self, name: Text, conditions: Optional[Dict[Text, Any]]
     ) -> None:
+
         # Ensure story part already has a name
         if not self.current_step_builder:
             raise StoryParseError(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L539'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L539</a>
```diff
         return default_value
 
     def _parse_action(self, step: Dict[Text, Any]) -> None:
+
         action_name = step.get(KEY_ACTION, "")
         if not action_name:
             rasa.shared.utils.io.raise_warning(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L560'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L560</a>
```diff
         self._add_event(ActiveLoop.type_name, {LOOP_NAME: active_loop_name})
 
     def _parse_checkpoint(self, step: Dict[Text, Any]) -> None:
+
         checkpoint_name = step.get(KEY_CHECKPOINT, "")
         slots = step.get(KEY_CHECKPOINT_SLOTS, [])
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/importers/importer.py#L477'>rasa/shared/importers/importer.py~L477</a>
```diff
         return original.merge(e2e_domain)
 
     def _get_domain_with_e2e_actions(self) -> Domain:
+
         stories = self.get_stories()
 
         additional_e2e_action_names = set()
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/importers/rasa.py#L26'>rasa/shared/importers/rasa.py~L26</a>
```diff
         domain_path: Optional[Text] = None,
         training_data_paths: Optional[Union[List[Text], Text]] = None,
     ):
+
         self._domain_path = domain_path
 
         self._nlu_files = rasa.shared.data.get_data_files(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/nlu/training_data/formats/rasa_yaml.py#L105'>rasa/shared/nlu/training_data/formats/rasa_yaml.py~L105</a>
```diff
         )
 
     def _parse_nlu(self, nlu_data: Optional[List[Dict[Text, Any]]]) -> None:
+
         if not nlu_data:
             return
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/nlu/training_data/training_data.py#L47'>rasa/shared/nlu/training_data/training_data.py~L47</a>
```diff
         lookup_tables: Optional[List[Dict[Text, Any]]] = None,
         responses: Optional[Dict[Text, List[Dict[Text, Any]]]] = None,
     ) -> None:
+
         if training_examples:
             self.training_examples = self.sanitize_examples(training_examples)
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/utils/tensorflow/models.py#L889'>rasa/utils/tensorflow/models.py~L889</a>
```diff
         tag_name: Text,
         entity_tags: Optional[tf.Tensor] = None,
     ) -> Tuple[tf.Tensor, tf.Tensor, tf.Tensor]:
+
         tag_ids = tf.cast(tag_ids[:, :, 0], tf.int32)
 
         if entity_tags is not None:
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/cli/test_rasa_data.py#L50'>tests/cli/test_rasa_data.py~L50</a>
```diff
 
 
 def test_data_convert_nlu_json(run_in_simple_project: Callable[..., RunResult]):
+
     result = run_in_simple_project(
         "data",
         "convert",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/cli/test_rasa_data.py#L69'>tests/cli/test_rasa_data.py~L69</a>
```diff
 def test_data_convert_nlu_yml(
     run: Callable[..., RunResult], tmp_path: Path, request: FixtureRequest
 ):
+
     target_file = tmp_path / "out.yml"
 
     # The request rootdir is required as the `testdir` fixture in `run` changes the
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/cli/test_rasa_train.py#L119'>tests/cli/test_rasa_train.py~L119</a>
```diff
 def test_train_no_domain_exists(
     run_in_simple_project: Callable[..., RunResult], tmp_path: Path
 ) -> None:
+
     os.remove("domain.yml")
     run_in_simple_project(
         "train",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/cli/test_rasa_x.py#L146'>tests/cli/test_rasa_x.py~L146</a>
```diff
 
 
 def test_rasa_x_raises_warning_and_exits_without_production_flag():
+
     args = argparse.Namespace(loglevel=None, log_file=None, production=None)
     with pytest.raises(SystemExit):
         with pytest.warns(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_hangouts.py#L9'>tests/core/channels/test_hangouts.py~L9</a>
```diff
 
 
 def test_hangouts_channel():
+
     from rasa.core.channels.hangouts import HangoutsInput
     import rasa.core
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_hangouts.py#L161'>tests/core/channels/test_hangouts.py~L161</a>
```diff
 
 @pytest.mark.asyncio
 async def test_hangouts_output_channel_functions():
+
     from rasa.core.channels.hangouts import HangoutsOutput
 
     output_channel = HangoutsOutput()
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L15'>tests/core/channels/test_twilio_voice.py~L15</a>
```diff
 
 
 async def test_twilio_voice_twiml_response_text():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L43'>tests/core/channels/test_twilio_voice.py~L43</a>
```diff
 
 
 async def test_twilio_voice_twiml_response_buttons():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L156'>tests/core/channels/test_twilio_voice.py~L156</a>
```diff
 
 
 async def test_twilio_voice_remove_image():
+
     with pytest.warns(UserWarning):
         output_channel = TwilioVoiceCollectingOutputChannel()
         await output_channel.send_response(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L165'>tests/core/channels/test_twilio_voice.py~L165</a>
```diff
 
 
 async def test_twilio_voice_keep_image_text():
+
     output_channel = TwilioVoiceCollectingOutputChannel()
     await output_channel.send_response(
         recipient_id="Chuck Norris",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L175'>tests/core/channels/test_twilio_voice.py~L175</a>
```diff
 
 
 async def test_twilio_emoji_warning():
+
     with pytest.warns(UserWarning):
         output_channel = TwilioVoiceCollectingOutputChannel()
         await output_channel.send_response(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/channels/test_twilio_voice.py#L183'>tests/core/channels/test_twilio_voice.py~L183</a>
```diff
 
 
 async def test_twilio_voice_multiple_responses():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/evaluation/test_marker.py#L651'>tests/core/evaluation/test_marker.py~L651</a>
```diff
     ],
 )
 def test_marker_from_path_adds_special_or_marker(tmp_path: Path, configs: Any):
+
     yaml_file = tmp_path / "config.yml"
     rasa.shared.utils.io.write_yaml(data=configs, target=yaml_file)
     loaded = Marker.from_path(tmp_path)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/evaluation/test_marker_stats.py#L258'>tests/core/evaluation/test_marker_stats.py~L258</a>
```diff
 
 @pytest.mark.parametrize("seed", [2345, 5654, 2345234])
 def test_per_session_statistics_to_csv(tmp_path: Path, seed: int):
+
     rng = np.random.default_rng(seed=seed)
     (
         per_session_results,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/featurizers/test_precomputation.py#L497'>tests/core/featurizers/test_precomputation.py~L497</a>
```diff
     collector: CoreFeaturizationCollector,
     messages_with_unique_lookup_key: List[Message],
 ):
+
     messages = messages_with_unique_lookup_key
 
     # pass as training data
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/featurizers/test_single_state_featurizers.py#L418'>tests/core/featurizers/test_single_state_featurizers.py~L418</a>
```diff
 
 
 def test_encode_entities__with_entity_roles_and_groups():
+
     # create fake message that has been tokenized and entities have been extracted
     text = "I am flying from London to Paris"
     tokens = [
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/featurizers/test_single_state_featurizers.py#L475'>tests/core/featurizers/test_single_state_featurizers.py~L475</a>
```diff
 
 
 def test_encode_entities__with_bilou_entity_roles_and_groups():
+
     # Instantiate domain and configure the single state featurizer for this domain.
     # Note that there are 2 entity tags here.
     entity_tags = ["city", f"city{ENTITY_LABEL_SEPARATOR}to"]
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/policies/test_ted_policy.py#L101'>tests/core/policies/test_ted_policy.py~L101</a>
```diff
 class TestTEDPolicy(PolicyTestCollection):
     @staticmethod
     def _policy_class_to_test() -> Type[TEDPolicy]:
+
         return TEDPolicy
 
     def test_train_model_checkpointing(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/policies/test_unexpected_intent_policy.py#L56'>tests/core/policies/test_unexpected_intent_policy.py~L56</a>
```diff
 class TestUnexpecTEDIntentPolicy(TestTEDPolicy):
     @staticmethod
     def _policy_class_to_test() -> Type[UnexpecTEDIntentPolicy]:
+
         return UnexpecTEDIntentPolicy
 
     @pytest.fixture(scope="class")
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/policies/test_unexpected_intent_policy.py#L98'>tests/core/policies/test_unexpected_intent_policy.py~L98</a>
```diff
     def test_label_data_assembly(
         self, trained_policy: UnexpecTEDIntentPolicy, default_domain: Domain
     ):
+
         # Construct input data
         state_featurizer = trained_policy.featurizer.state_featurizer
         encoded_all_labels = state_featurizer.encode_all_labels(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/policies/test_unexpected_intent_policy.py#L846'>tests/core/policies/test_unexpected_intent_policy.py~L846</a>
```diff
             all_similarities: np.array,
             label_index: int,
         ):
+
             expected_score = all_similarities[0][label_index]
             expected_threshold = (
                 all_thresholds[label_index] if label_index in all_thresholds else None
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_actions.py#L787'>tests/core/test_actions.py~L787</a>
```diff
 
 
 async def test_response_channel_specific(default_nlg, default_tracker, domain: Domain):
+
     output_channel = SlackBot("DummyToken", "General")
 
     events = await ActionBotResponse("utter_channel").run(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_broker.py#L99'>tests/core/test_broker.py~L99</a>
```diff
 
 
 async def test_pika_raise_connection_exception(monkeypatch: MonkeyPatch):
+
     monkeypatch.setattr(
         PikaEventBroker, "connect", AsyncMock(side_effect=ChannelNotFoundEntity())
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_broker.py#L326'>tests/core/test_broker.py~L326</a>
```diff
 
 
 async def test_create_pika_invalid_port():
+
     cfg = EndpointConfig(
         username="username", password="password", type="pika", port="PORT"
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_nlg.py#L12'>tests/core/test_nlg.py~L12</a>
```diff
 
 
 def nlg_app(base_url="/"):
+
     app = Sanic("test_nlg")
 
     @app.route(base_url, methods=["POST"])
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_policies.py#L424'>tests/core/test_policies.py~L424</a>
```diff
         default_domain: Domain,
         stories_path: Text,
     ):
+
         execution_context = dataclasses.replace(execution_context, is_finetuning=True)
         loaded_policy = MemoizationPolicy.load(
             trained_policy.config, model_storage, resource, execution_context
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_test.py#L198'>tests/core/test_test.py~L198</a>
```diff
     monkeypatch: MonkeyPatch,
     moodbot_domain_path: Path,
 ) -> Callable[[Path, bool], Coroutine]:
+
     # We need `RulePolicy` to predict the correct actions
     # in a particular conversation context as seen during training.
     # Since it can get affected by `action_unlikely_intent` being triggered in
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/core/test_tracker_stores.py#L179'>tests/core/test_tracker_stores.py~L179</a>
```diff
 
 
 def test_redis_tracker_store_invalid_key_prefix(domain: Domain):
+
     test_invalid_key_prefix = "$$ &!"
 
     tracker_store = RedisTrackerStore(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/recipes/test_default_recipe.py#L593'>tests/engine/recipes/test_default_recipe.py~L593</a>
```diff
 
 
 def test_dump_config_missing_file(tmp_path: Path, capsys: CaptureFixture):
+
     config_path = tmp_path / "non_existent_config.yml"
 
     config = rasa.shared.utils.io.read_config_file(str(SOME_CONFIG))
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/test_caching.py#L50'>tests/engine/test_caching.py~L50</a>
```diff
         model_storage: ModelStorage,
         output_fingerprint: Text,
     ) -> "TestCacheableOutput":
+
         value = rasa.shared.utils.io.read_json_file(directory / "cached.json")
 
         return cls(value, cache_dir=directory)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/test_validation.py#L93'>tests/engine/test_validation.py~L93</a>
```diff
     language: Optional[Text] = None,
     is_train_graph: bool = True,
 ) -> GraphModelConfiguration:
+
     parent_node = {}
     if parent:
         parent_node = {
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_components.py#L17'>tests/engine/training/test_components.py~L17</a>
```diff
 
 
 def test_cached_component_returns_value_from_cache(default_model_storage: ModelStorage):
+
     cached_output = CacheableText("Cache me!!")
 
     node = GraphNode(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_components.py#L105'>tests/engine/training/test_components.py~L105</a>
```diff
 def test_fingerprint_component_hit(
     default_model_storage: ModelStorage, temp_cache: TrainingCache
 ):
+
     cached_output = CacheableText("Cache me!!")
     output_fingerprint = uuid.uuid4().hex
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_components.py#L159'>tests/engine/training/test_components.py~L159</a>
```diff
 def test_fingerprint_component_miss(
     default_model_storage: ModelStorage, temp_cache: TrainingCache
 ):
+
     component_config = {"x": 1}
 
     node = GraphNode(
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_graph_trainer.py#L112'>tests/engine/training/test_graph_trainer.py~L112</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_graph_trainer.py#L212'>tests/engine/training/test_graph_trainer.py~L212</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_graph_trainer.py#L267'>tests/engine/training/test_graph_trainer.py~L267</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/engine/training/test_graph_trainer.py#L382'>tests/engine/training/test_graph_trainer.py~L382</a>
```diff
     train_with_schema: Callable,
     caplog: LogCaptureFixture,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/examples/test_example_bots_training_data.py#L65'>tests/examples/test_example_bots_training_data.py~L65</a>
```diff
     raise_slot_warning: bool,
     msg: Optional[Text],
 ):
+
     importer = TrainingDataImporter.load_from_config(
         config_file, domain_file, [data_folder]
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/graph_components/validators/test_default_recipe_validator.py#L705'>tests/graph_components/validators/test_default_recipe_validator.py~L705</a>
```diff
 def test_core_warn_if_data_but_no_policy(
     monkeypatch: MonkeyPatch, policy_type: Optional[Type[Policy]]
 ):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_e2ebot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/graph_components/validators/test_default_recipe_validator.py#L838'>tests/graph_components/validators/test_default_recipe_validator.py~L838</a>
```diff
 def test_core_raise_if_a_rule_policy_is_incompatible_with_domain(
     monkeypatch: MonkeyPatch,
 ):
+
     domain = Domain.empty()
 
     num_instances = 2
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/graph_components/validators/test_default_recipe_validator.py#L883'>tests/graph_components/validators/test_default_recipe_validator.py~L883</a>
```diff
     num_duplicates: bool,
     priority: int,
 ):
+
     assert len(policy_types) >= priority + num_duplicates, (
         f"This tests needs at least {priority + num_duplicates} many types."
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/graph_components/validators/test_default_recipe_validator.py#L947'>tests/graph_components/validators/test_default_recipe_validator.py~L947</a>
```diff
 
 @pytest.mark.parametrize("policy_type_consuming_rule_data", [RulePolicy])
 def test_core_warn_if_rule_data_missing(policy_type_consuming_rule_data: Type[Policy]):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_e2ebot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/graph_components/validators/test_default_recipe_validator.py#L976'>tests/graph_components/validators/test_default_recipe_validator.py~L976</a>
```diff
 def test_core_warn_if_rule_data_unused(
     policy_type_not_consuming_rule_data: Type[Policy],
 ):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_moodbot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_diet_classifier.py#L132'>tests/nlu/classifiers/test_diet_classifier.py~L132</a>
```diff
         message_text: Text = "Rasa is great!",
         expect_intent: bool = True,
     ) -> Message:
+
         if not pipeline:
             pipeline = [
                 {"component": WhitespaceTokenizer},
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_regex_message_handler.py#L38'>tests/nlu/classifiers/test_regex_message_handler.py~L38</a>
```diff
 def test_process_does_not_do_anything(
     regex_message_handler: RegexMessageHandler, text: Text
 ):
+
     message = Message(
         data={TEXT: text, INTENT: "bla"},
         features=[
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_regex_message_handler.py#L77'>tests/nlu/classifiers/test_regex_message_handler.py~L77</a>
```diff
 def test_regex_message_handler_adds_extractor_name(
     regex_message_handler: RegexMessageHandler, text: Text
 ):
+
     message = Message(
         data={TEXT: text, INTENT: "bla"},
         features=[
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/conftest.py#L39'>tests/nlu/conftest.py~L39</a>
```diff
     def inner(
         pipeline: List[Dict[Text, Any]], training_data: Union[Text, TrainingData]
     ) -> Tuple[TrainingData, List[GraphComponent]]:
+
         if isinstance(training_data, str):
             importer = RasaFileImporter(training_data_paths=[training_data])
             training_data: TrainingData = importer.get_nlu_data()
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/conftest.py#L75'>tests/nlu/conftest.py~L75</a>
```diff
 @pytest.fixture()
 def process_message(default_model_storage: ModelStorage) -> Callable[..., Message]:
     def inner(loaded_pipeline: List[GraphComponent], message: Message) -> Message:
+
         for component in loaded_pipeline:
             component.process([message])
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/extractors/test_crf_entity_extractor.py#L132'>tests/nlu/extractors/test_crf_entity_extractor.py~L132</a>
```diff
     spacy_nlp_component: SpacyNLP,
     spacy_model: SpacyModel,
 ):
+
     crf_extractor = crf_entity_extractor(config_params)
 
     importer = RasaFileImporter(training_data_paths=["data/examples/rasa"])
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/extractors/test_mitie_entity_extractor.py#L73'>tests/nlu/extractors/test_mitie_entity_extractor.py~L73</a>
```diff
     mitie_model: MitieModel,
     with_trainable_examples: bool,
 ):
+
     # some texts where last token is a city
     texts_ending_with_city = ["Bert lives in Berlin", "Ernie asks where is Bielefeld"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/extractors/test_regex_entity_extractor.py#L278'>tests/nlu/extractors/test_regex_entity_extractor.py~L278</a>
```diff
 def test_process_does_not_overwrite_any_entities(
     create_or_load_extractor: Callable[..., RegexEntityExtractor],
 ):
+
     pre_existing_entity = {
         ENTITY_ATTRIBUTE_TYPE: "person",
         ENTITY_ATTRIBUTE_VALUE: "Max",
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_lexical_syntactic_featurizer.py#L235'>tests/nlu/featurizers/test_lexical_syntactic_featurizer.py~L235</a>
```diff
     resource_lexical_syntactic_featurizer: Resource,
     feature_config: List[Text],
 ) -> Callable[..., LexicalSyntacticFeaturizer]:
+
     config = {"alias": "lsf", "features": feature_config}
     featurizer = create_lexical_syntactic_featurizer(config)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_lexical_syntactic_featurizer.py#L296'>tests/nlu/featurizers/test_lexical_syntactic_featurizer.py~L296</a>
```diff
     feature_config: Dict[Text, Any],
     expected_features: np.ndarray,
 ):
+
     featurizer = create_lexical_syntactic_featurizer(
         {"alias": "lsf", "features": feature_config}
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_lm_featurizer.py#L810'>tests/nlu/featurizers/test_lm_featurizer.py~L810</a>
```diff
         [Dict[Text, Any]], LanguageModelFeaturizer
     ],
 ):
+
     config = {"model_name": "bert", "model_weights": "bert-base-chinese"}
 
     lm_featurizer = create_language_model_featurizer(config)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_mitie_featurizer.py#L57'>tests/nlu/featurizers/test_mitie_featurizer.py~L57</a>
```diff
     mitie_model: MitieModel,
     mitie_tokenizer: MitieTokenizer,
 ):
+
     featurizer = create({"alias": "mitie_featurizer"})
 
     sentence = "Hey how are you today"
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_mitie_featurizer.py#L83'>tests/nlu/featurizers/test_mitie_featurizer.py~L83</a>
```diff
     mitie_model: MitieModel,
     mitie_tokenizer: MitieTokenizer,
 ):
+
     featurizer = create({"alias": "mitie_featurizer"})
 
     sentence = "Hey how are you today"
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_regex_featurizer.py#L302'>tests/nlu/featurizers/test_regex_featurizer.py~L302</a>
```diff
     create_featurizer: Callable[..., RegexFeaturizer],
     spacy_tokenizer: SpacyTokenizer,
 ):
+
     patterns = [
         {"pattern": "[0-9]+", "name": "number", "usage": "intent"},
         {"pattern": "\\bhey*", "name": "hello", "usage": "intent"},
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/featurizers/test_regex_featurizer.py#L398'>tests/nlu/featurizers/test_regex_featurizer.py~L398</a>
```diff
     create_featurizer: Callable[..., R

... (truncated 3188 lines) ...



---
