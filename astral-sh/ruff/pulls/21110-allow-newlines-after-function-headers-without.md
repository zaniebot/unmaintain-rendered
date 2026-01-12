```yaml
number: 21110
title: Allow newlines after function headers without docstrings
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: brent/newline-after-function-header
created_at: 2025-10-28T16:51:12Z
updated_at: 2025-11-26T17:13:10Z
url: https://github.com/astral-sh/ruff/pull/21110
synced_at: 2026-01-12T15:57:16Z
```

# Allow newlines after function headers without docstrings

---

_@ntBre_

Summary
--

This is a first step toward fixing #9745. After reviewing our open issues and several Black issues and PRs, I personally found the function case the most compelling, especially with very long argument lists:

```py
def func(
	self,
	arg1: int,
	arg2: bool,
	arg3: bool,
	arg4: float,
	arg5: bool,
) -> tuple[...]:
	if arg2 and arg3:
		raise ValueError
```

or many annotations:

```py
def function(
    self, data: torch.Tensor | tuple[torch.Tensor, ...], other_argument: int
) -> torch.Tensor | tuple[torch.Tensor, ...]:
    do_something(data)
    return something
```

I think docstrings help the situation substantially both because syntax highlighting will usually give a very clear separation between the annotations and the docstring and because we already allow a blank line _after_ the docstring:

```py
def function(
    self, data: torch.Tensor | tuple[torch.Tensor, ...], other_argument: int
) -> torch.Tensor | tuple[torch.Tensor, ...]:
    """
	A function doing something.

	And a longer description of the things it does.
	"""

    do_something(data)
    return something
```

There are still other comments on #9745, such as [this one] with 9 upvotes, where users specifically request blank lines in all block types, or at least including conditionals and loops. I'm sympathetic to that case as well, even if personally I don't find an [example] like this:

```py
if blah:

    # Do some stuff that is logically related
    data = get_data()

    # Do some different stuff that is logically related
    results = calculate_results()

    return results
```

to be much more readable than:

```py
if blah:
    # Do some stuff that is logically related
    data = get_data()

    # Do some different stuff that is logically related
    results = calculate_results()

    return results
```

I'm probably just used to the latter from the formatters I've used, but I do prefer it. I also think that functions are the least susceptible to the accidental introduction of a newline after refactoring described in Micha's [comment] on #8893.

I actually considered further restricting this change to functions with multiline headers. I don't think very short functions like:

```py
def foo():

    return 1
```

benefit nearly as much from the allowed newline, but I just went with any function without a docstring for now. I guess a marginal case like:

```py
def foo(a_long_parameter: ALongType, b_long_parameter: BLongType) -> CLongType:

    return 1
```

might be a good argument for not restricting it.

I caused a couple of syntax errors before adding special handling for the ellipsis-only case, so I suspect that there are some other interesting edge cases that may need to be handled better.

Test Plan
--

Existing tests, plus a few simple new ones. As noted above, I suspect that we may need a few more for edge cases I haven't considered.

[this one]: https://github.com/astral-sh/ruff/issues/9745#issuecomment-2876771400
[example]: https://github.com/psf/black/issues/902#issuecomment-1562154809
[comment]: https://github.com/astral-sh/ruff/issues/8893#issuecomment-1867259744


---

_Label `formatter` added by @ntBre on 2025-10-28 16:51_

---

_Label `preview` added by @ntBre on 2025-10-28 16:51_

---

_Comment by @github-actions[bot] on 2025-10-28 17:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+419 -0 lines in 282 files in 18 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+2 -0 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/e3acfcdacd7916900fa003ed1d308b67dea291b0/housewatch/api/async_migration.py#L52'>housewatch/api/async_migration.py~L52</a>
```diff
 
     @action(methods=["POST"], detail=True)
     def trigger(self, request, **kwargs):
+
         migration = self.get_object()
 
         migration.status = MigrationStatus.Starting
```
<a href='https://github.com/PostHog/HouseWatch/blob/e3acfcdacd7916900fa003ed1d308b67dea291b0/housewatch/async_migrations/runner.py#L27'>housewatch/async_migrations/runner.py~L27</a>
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
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/data/test_classes/custom_slots.py#L28'>data/test_classes/custom_slots.py~L28</a>
```diff
         value_reset_delay: Optional[int] = None,
         influence_conversation: bool = True,
     ) -> None:
+
         super().__init__(
             name=name,
             initial_value=initial_value,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/actions/actions.py#L28'>examples/reminderbot/actions/actions.py~L28</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         dispatcher.utter_message("I will remind you in 5 seconds.")
 
         date = datetime.datetime.now() + datetime.timedelta(seconds=5)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/actions/actions.py#L56'>examples/reminderbot/actions/actions.py~L56</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         name = next(tracker.get_slot("PERSON"), "someone")
         dispatcher.utter_message(f"Remember to call {name}!")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/actions/actions.py#L71'>examples/reminderbot/actions/actions.py~L71</a>
```diff
     async def run(
         self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
     ) -> List[Dict[Text, Any]]:
+
         conversation_id = tracker.sender_id
 
         dispatcher.utter_message(f"The ID of this conversation is '{conversation_id}'.")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/actions/actions.py#L98'>examples/reminderbot/actions/actions.py~L98</a>
```diff
         tracker: Tracker,
         domain: Dict[Text, Any],
     ) -> List[Dict[Text, Any]]:
+
         plant = next(tracker.get_latest_entity_values("plant"), "someone")
         dispatcher.utter_message(f"Your {plant} needs some water!")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/actions/actions.py#L113'>examples/reminderbot/actions/actions.py~L113</a>
```diff
     async def run(
         self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
     ) -> List[Dict[Text, Any]]:
+
         dispatcher.utter_message("Okay, I'll cancel all your reminders.")
 
         # Cancel all reminders
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/reminderbot/callback_server.py#L4'>examples/reminderbot/callback_server.py~L4</a>
```diff
 
 
 def create_app() -> Sanic:
+
     bot_app = Sanic("callback_server", configure_logging=False)
 
     @bot_app.post("/bot")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/export.py#L177'>rasa/cli/export.py~L177</a>
```diff
 
 
 async def _export_trackers(args: argparse.Namespace) -> None:
+
     _assert_max_timestamp_is_greater_than_min_timestamp(args)
 
     endpoints = rasa.core.utils.read_endpoints_from_path(args.endpoints)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/cli/run.py#L58'>rasa/cli/run.py~L58</a>
```diff
 
 
 def _validate_model_path(model_path: Text, parameter: Text, default: Text) -> Text:
+
     if model_path is not None and not os.path.exists(model_path):
         reason_str = f"'{model_path}' not found."
         if model_path is None:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/actions/action.py#L658'>rasa/core/actions/action.py~L658</a>
```diff
 
 class RemoteAction(Action):
     def __init__(self, name: Text, action_endpoint: Optional[EndpointConfig]) -> None:
+
         self._name = name
         self.action_endpoint = action_endpoint
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/agent.py#L503'>rasa/core/agent.py~L503</a>
```diff
         return await self.handle_message(msg)
 
     def _set_fingerprint(self, fingerprint: Optional[Text] = None) -> None:
+
         if fingerprint:
             self.fingerprint = fingerprint
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/botframework.py#L48'>rasa/core/channels/botframework.py~L48</a>
```diff
         bot: Text,
         service_url: Text,
     ) -> None:
+
         service_url = (
             f"{service_url}/" if not service_url.endswith("/") else service_url
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/callback.py#L22'>rasa/core/channels/callback.py~L22</a>
```diff
         return "callback"
 
     def __init__(self, endpoint: EndpointConfig) -> None:
+
         self.callback_endpoint = endpoint
         super().__init__()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/facebook.py#L33'>rasa/core/channels/facebook.py~L33</a>
```diff
         page_access_token: Text,
         on_new_message: Callable[[UserMessage], Awaitable[Any]],
     ) -> None:
+
         self.on_new_message = on_new_message
         self.client = MessengerClient(page_access_token)
         self.last_message: Dict[Text, Any] = {}
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/facebook.py#L171'>rasa/core/channels/facebook.py~L171</a>
```diff
         return "facebook"
 
     def __init__(self, messenger_client: MessengerClient) -> None:
+
         self.messenger_client = messenger_client
         super().__init__()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/facebook.py#L349'>rasa/core/channels/facebook.py~L349</a>
```diff
     def blueprint(
         self, on_new_message: Callable[[UserMessage], Awaitable[Any]]
     ) -> Blueprint:
+
         fb_webhook = Blueprint("fb_webhook", __name__)
 
         # noinspection PyUnusedLocal
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L40'>rasa/core/channels/hangouts.py~L40</a>
```diff
 
     @staticmethod
     def _text_card(message: Dict[Text, Any]) -> Dict:
+
         card = {
             "cards": [
                 {
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L190'>rasa/core/channels/hangouts.py~L190</a>
```diff
 
     @classmethod
     def from_credentials(cls, credentials: Optional[Dict[Text, Any]]) -> InputChannel:
+
         if credentials:
             return cls(credentials.get("project_id"))
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L202'>rasa/core/channels/hangouts.py~L202</a>
```diff
         hangouts_room_added_intent_name: Optional[Text] = "/room_added",
         hangouts_removed_intent_name: Optional[Text] = "/bot_removed",
     ) -> None:
+
         self.project_id = project_id
         self.hangouts_user_added_intent_name = hangouts_user_added_intent_name
         self.hangouts_room_added_intent_name = hangouts_room_added_intent_name
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L224'>rasa/core/channels/hangouts.py~L224</a>
```diff
 
     @staticmethod
     def _extract_sender(req: Request) -> Text:
+
         if req.json["type"] == "MESSAGE":
             return req.json["message"]["sender"]["displayName"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L231'>rasa/core/channels/hangouts.py~L231</a>
```diff
 
     # noinspection PyMethodMayBeStatic
     def _extract_message(self, req: Request) -> Text:
+
         if req.json["type"] == "MESSAGE":
             message = req.json["message"]["text"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/hangouts.py#L290'>rasa/core/channels/hangouts.py~L290</a>
```diff
 
         @custom_webhook.route("/webhook", methods=["POST"])
         async def receive(request: Request) -> HTTPResponse:
+
             if self.project_id:
                 token = request.headers.get("Authorization", "").replace("Bearer ", "")
                 self._check_token(token)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/rocketchat.py#L114'>rasa/core/channels/rocketchat.py~L114</a>
```diff
         )
 
     def __init__(self, user: Text, password: Text, server_url: Text) -> None:
+
         self.user = user
         self.password = password
         self.server_url = server_url
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/webexteams.py#L78'>rasa/core/channels/webexteams.py~L78</a>
```diff
         sender_id: Optional[Text],
         metadata: Optional[Dict],
     ) -> Any:
+
         try:
             out_channel = self.get_output_channel()
             user_msg = UserMessage(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/featurizers/single_state_featurizer.py#L186'>rasa/core/featurizers/single_state_featurizer.py~L186</a>
```diff
         precomputations: Optional[MessageContainerForCoreFeaturization],
         sparse: bool = False,
     ) -> Dict[Text, List[Features]]:
+
         # Remove entities from possible attributes
         attributes = set(
             attribute for attribute in sub_state.keys() if attribute != ENTITIES
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/nlg/callback.py#L62'>rasa/core/nlg/callback.py~L62</a>
```diff
     """
 
     def __init__(self, endpoint_config: EndpointConfig) -> None:
+
         self.nlg_endpoint = endpoint_config
 
     async def generate(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/rule_policy.py#L951'>rasa/core/policies/rule_policy.py~L951</a>
```diff
     def _find_action_from_loop_happy_path(
         tracker: DialogueStateTracker,
     ) -> Tuple[Optional[Text], Optional[Text]]:
+
         active_loop_name = tracker.active_loop_name
         if active_loop_name is None:
             return None, None
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/ted_policy.py#L1925'>rasa/core/policies/ted_policy.py~L1925</a>
```diff
         text_output: tf.Tensor,
         text_sequence_lengths: tf.Tensor,
     ) -> tf.Tensor:
+
         text_transformed, text_mask, text_sequence_lengths = self._reshape_for_entities(
             tf_batch_data,
             dialogue_transformer_output,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/ted_policy.py#L2127'>rasa/core/policies/ted_policy.py~L2127</a>
```diff
         text_output: tf.Tensor,
         text_sequence_lengths: tf.Tensor,
     ) -> Tuple[tf.Tensor, tf.Tensor]:
+
         text_transformed, _, text_sequence_lengths = self._reshape_for_entities(
             tf_batch_data,
             dialogue_transformer_output,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/processor.py#L785'>rasa/core/processor.py~L785</a>
```diff
     async def _handle_message_with_tracker(
         self, message: UserMessage, tracker: DialogueStateTracker
     ) -> None:
+
         if message.parse_data:
             parse_data = message.parse_data
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L702'>rasa/core/test.py~L702</a>
```diff
     event: ActionExecuted,
     fail_on_prediction_errors: bool,
 ) -> Tuple[EvaluationStore, PolicyPrediction, Optional[EntityEvaluationResult]]:
+
     action_executed_eval_store = EvaluationStore()
 
     expected_action_name = event.action_name
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L820'>rasa/core/test.py~L820</a>
```diff
     List[Dict[Text, Any]],
     List[EntityEvaluationResult],
 ]:
+
     processor = agent.processor
     if agent.processor is not None:
         processor = agent.processor
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/engine/recipes/default_recipe.py#L666'>rasa/engine/recipes/default_recipe.py~L666</a>
```diff
         preprocessors: List[Text],
         train_nodes: Dict[Text, SchemaNode],
     ) -> Dict[Text, SchemaNode]:
+
         predict_config = copy.deepcopy(config)
         predict_nodes = {}
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/engine/storage/local_model_storage.py#L221'>rasa/engine/storage/local_model_storage.py~L221</a>
```diff
 
     @staticmethod
     def _persist_metadata(metadata: ModelMetadata, temporary_directory: Path) -> None:
+
         rasa.shared.utils.io.dump_obj_as_json_to_file(
             temporary_directory / MODEL_ARCHIVE_METADATA_FILE, metadata.as_dict()
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/engine/validation.py#L485'>rasa/engine/validation.py~L485</a>
```diff
     parent_return_type: TypeAnnotation,
     required_type: TypeAnnotation,
 ) -> None:
+
     if not typing_utils.issubtype(parent_return_type, required_type):
         parent_node_text = ""
         if parent_node:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L506'>rasa/nlu/classifiers/diet_classifier.py~L506</a>
```diff
     def _extract_features(
         self, message: Message, attribute: Text
     ) -> Dict[Text, Union[scipy.sparse.spmatrix, np.ndarray]]:
+
         (
             sparse_sequence_features,
             sparse_sentence_features,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L775'>rasa/nlu/classifiers/diet_classifier.py~L775</a>
```diff
         sparse_feature_sizes: Dict[Text, Dict[Text, List[int]]],
         label_attribute: Optional[Text] = None,
     ) -> Dict[Text, Dict[Text, List[int]]]:
+
         if label_attribute in sparse_feature_sizes:
             del sparse_feature_sizes[label_attribute]
         return sparse_feature_sizes
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L1260'>rasa/nlu/classifiers/diet_classifier.py~L1260</a>
```diff
         config: Dict[Text, Any],
         finetune_mode: bool,
     ) -> "RasaModel":
+
         predict_data_example = RasaModelData(
             label_key=model_data_example.label_key,
             data={
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L1503'>rasa/nlu/classifiers/diet_classifier.py~L1503</a>
```diff
         sequence_feature_lengths: tf.Tensor,
         name: Text,
     ) -> tf.Tensor:
+
         x, _ = self._tf_layers[f"feature_combining_layer.{name}"](
             (sequence_features, sentence_features, sequence_feature_lengths),
             training=self._training,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L1684'>rasa/nlu/classifiers/diet_classifier.py~L1684</a>
```diff
         return loss
 
     def _update_label_metrics(self, loss: tf.Tensor, acc: tf.Tensor) -> None:
+
         self.intent_loss.update_state(loss)
         self.intent_acc.update_state(acc)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L1842'>rasa/nlu/classifiers/diet_classifier.py~L1842</a>
```diff
         combined_sequence_sentence_feature_lengths: tf.Tensor,
         text_transformed: tf.Tensor,
     ) -> Dict[Text, tf.Tensor]:
+
         if self.all_labels_embed is None:
             raise ValueError(
                 "The model was not prepared for prediction. "
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L323'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L323</a>
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
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py#L407'>rasa/nlu/featurizers/dense_featurizer/convert_featurizer.py~L407</a>
```diff
             )
 
     def _tokenize(self, sentence: Text) -> Any:
+
         return self.tokenize_signature(tf.convert_to_tensor([sentence]))[
             "default"
         ].numpy()
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L98'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L98</a>
```diff
         return ["sklearn"]
 
     def _load_count_vect_params(self) -> None:
+
         # Use shared vocabulary between text and all other attributes of Message
         self.use_shared_vocab = self._config["use_shared_vocab"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/persistor.py#L95'>rasa/nlu/persistor.py~L95</a>
```diff
 
     @staticmethod
     def _tar_name(model_name: Text, include_extension: bool = True) -> Text:
+
         ext = ".tar.gz" if include_extension else ""
         return f"{model_name}{ext}"
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/selectors/response_selector.py#L625'>rasa/nlu/selectors/response_selector.py~L625</a>
```diff
         config: Dict[Text, Any],
         finetune_mode: bool = False,
     ) -> "RasaModel":
+
         predict_data_example = RasaModelData(
             label_key=model_data_example.label_key,
             data={
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/selectors/response_selector.py#L721'>rasa/nlu/selectors/response_selector.py~L721</a>
```diff
             logger.debug(f"  {metric} ({name})")
 
     def _update_label_metrics(self, loss: tf.Tensor, acc: tf.Tensor) -> None:
+
         self.response_loss.update_state(loss)
         self.response_acc.update_state(acc)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/test.py#L1569'>rasa/nlu/test.py~L1569</a>
```diff
 
 
 def _contains_entity_labels(entity_results: List[EntityEvaluationResult]) -> bool:
+
     for result in entity_results:
         if result.entity_targets or result.entity_predictions:
             return True
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/server.py#L234'>rasa/server.py~L234</a>
```diff
         async def decorated(
             request: Request, *args: Any, **kwargs: Any
         ) -> response.HTTPResponse:
+
             provided = request.args.get("token", None)
 
             # noinspection PyProtectedMember
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L318'>rasa/shared/core/events.py~L318</a>
```diff
     def from_parameters(
         parameters: Dict[Text, Any], default: Optional[Type["Event"]] = None
     ) -> Optional["Event"]:
+
         event_name = parameters.get("event")
         if event_name is None:
             return None
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L1019'>rasa/shared/core/events.py~L1019</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["SlotSet"]]:
+
         slots = []
         for slot_key, slot_val in parameters.items():
             slots.append(SlotSet(slot_key, slot_val))
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L1218'>rasa/shared/core/events.py~L1218</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["ReminderScheduled"]]:
+
         trigger_date_time = parser.parse(parameters.get("date_time"))
 
         return [
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L1471'>rasa/shared/core/events.py~L1471</a>
```diff
     def _from_story_string(
         cls, parameters: Dict[Text, Any]
     ) -> Optional[List["FollowupAction"]]:
+
         return [
             FollowupAction(
                 parameters.get("name"),
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/story_reader.py#L111'>rasa/shared/core/training_data/story_reader/story_reader.py~L111</a>
```diff
     def _add_checkpoint(
         self, name: Text, conditions: Optional[Dict[Text, Any]]
     ) -> None:
+
         # Ensure story part already has a name
         if not self.current_step_builder:
             raise StoryParseError(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L539'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L539</a>
```diff
         return default_value
 
     def _parse_action(self, step: Dict[Text, Any]) -> None:
+
         action_name = step.get(KEY_ACTION, "")
         if not action_name:
             rasa.shared.utils.io.raise_warning(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L560'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L560</a>
```diff
         self._add_event(ActiveLoop.type_name, {LOOP_NAME: active_loop_name})
 
     def _parse_checkpoint(self, step: Dict[Text, Any]) -> None:
+
         checkpoint_name = step.get(KEY_CHECKPOINT, "")
         slots = step.get(KEY_CHECKPOINT_SLOTS, [])
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/importers/importer.py#L475'>rasa/shared/importers/importer.py~L475</a>
```diff
         return original.merge(e2e_domain)
 
     def _get_domain_with_e2e_actions(self) -> Domain:
+
         stories = self.get_stories()
 
         additional_e2e_action_names = set()
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/importers/rasa.py#L26'>rasa/shared/importers/rasa.py~L26</a>
```diff
         domain_path: Optional[Text] = None,
         training_data_paths: Optional[Union[List[Text], Text]] = None,
     ):
+
         self._domain_path = domain_path
 
         self._nlu_files = rasa.shared.data.get_data_files(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/formats/rasa_yaml.py#L105'>rasa/shared/nlu/training_data/formats/rasa_yaml.py~L105</a>
```diff
         )
 
     def _parse_nlu(self, nlu_data: Optional[List[Dict[Text, Any]]]) -> None:
+
         if not nlu_data:
             return
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/training_data.py#L47'>rasa/shared/nlu/training_data/training_data.py~L47</a>
```diff
         lookup_tables: Optional[List[Dict[Text, Any]]] = None,
         responses: Optional[Dict[Text, List[Dict[Text, Any]]]] = None,
     ) -> None:
+
         if training_examples:
             self.training_examples = self.sanitize_examples(training_examples)
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/models.py#L889'>rasa/utils/tensorflow/models.py~L889</a>
```diff
         tag_name: Text,
         entity_tags: Optional[tf.Tensor] = None,
     ) -> Tuple[tf.Tensor, tf.Tensor, tf.Tensor]:
+
         tag_ids = tf.cast(tag_ids[:, :, 0], tf.int32)
 
         if entity_tags is not None:
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/cli/test_rasa_data.py#L50'>tests/cli/test_rasa_data.py~L50</a>
```diff
 
 
 def test_data_convert_nlu_json(run_in_simple_project: Callable[..., RunResult]):
+
     result = run_in_simple_project(
         "data",
         "convert",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/cli/test_rasa_data.py#L69'>tests/cli/test_rasa_data.py~L69</a>
```diff
 def test_data_convert_nlu_yml(
     run: Callable[..., RunResult], tmp_path: Path, request: FixtureRequest
 ):
+
     target_file = tmp_path / "out.yml"
 
     # The request rootdir is required as the `testdir` fixture in `run` changes the
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/cli/test_rasa_train.py#L119'>tests/cli/test_rasa_train.py~L119</a>
```diff
 def test_train_no_domain_exists(
     run_in_simple_project: Callable[..., RunResult], tmp_path: Path
 ) -> None:
+
     os.remove("domain.yml")
     run_in_simple_project(
         "train",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/cli/test_rasa_x.py#L146'>tests/cli/test_rasa_x.py~L146</a>
```diff
 
 
 def test_rasa_x_raises_warning_and_exits_without_production_flag():
+
     args = argparse.Namespace(loglevel=None, log_file=None, production=None)
     with pytest.raises(SystemExit):
         with pytest.warns(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L9'>tests/core/channels/test_hangouts.py~L9</a>
```diff
 
 
 def test_hangouts_channel():
+
     from rasa.core.channels.hangouts import HangoutsInput
     import rasa.core
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_hangouts.py#L161'>tests/core/channels/test_hangouts.py~L161</a>
```diff
 
 @pytest.mark.asyncio
 async def test_hangouts_output_channel_functions():
+
     from rasa.core.channels.hangouts import HangoutsOutput
 
     output_channel = HangoutsOutput()
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L15'>tests/core/channels/test_twilio_voice.py~L15</a>
```diff
 
 
 async def test_twilio_voice_twiml_response_text():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L43'>tests/core/channels/test_twilio_voice.py~L43</a>
```diff
 
 
 async def test_twilio_voice_twiml_response_buttons():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L156'>tests/core/channels/test_twilio_voice.py~L156</a>
```diff
 
 
 async def test_twilio_voice_remove_image():
+
     with pytest.warns(UserWarning):
         output_channel = TwilioVoiceCollectingOutputChannel()
         await output_channel.send_response(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L165'>tests/core/channels/test_twilio_voice.py~L165</a>
```diff
 
 
 async def test_twilio_voice_keep_image_text():
+
     output_channel = TwilioVoiceCollectingOutputChannel()
     await output_channel.send_response(
         recipient_id="Chuck Norris",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L175'>tests/core/channels/test_twilio_voice.py~L175</a>
```diff
 
 
 async def test_twilio_emoji_warning():
+
     with pytest.warns(UserWarning):
         output_channel = TwilioVoiceCollectingOutputChannel()
         await output_channel.send_response(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/channels/test_twilio_voice.py#L183'>tests/core/channels/test_twilio_voice.py~L183</a>
```diff
 
 
 async def test_twilio_voice_multiple_responses():
+
     inputs = {
         "initial_prompt": "hello",
         "reprompt_fallback_phrase": "i didn't get that",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/evaluation/test_marker.py#L651'>tests/core/evaluation/test_marker.py~L651</a>
```diff
     ],
 )
 def test_marker_from_path_adds_special_or_marker(tmp_path: Path, configs: Any):
+
     yaml_file = tmp_path / "config.yml"
     rasa.shared.utils.io.write_yaml(data=configs, target=yaml_file)
     loaded = Marker.from_path(tmp_path)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/evaluation/test_marker_stats.py#L258'>tests/core/evaluation/test_marker_stats.py~L258</a>
```diff
 
 @pytest.mark.parametrize("seed", [2345, 5654, 2345234])
 def test_per_session_statistics_to_csv(tmp_path: Path, seed: int):
+
     rng = np.random.default_rng(seed=seed)
     (
         per_session_results,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/featurizers/test_precomputation.py#L497'>tests/core/featurizers/test_precomputation.py~L497</a>
```diff
     collector: CoreFeaturizationCollector,
     messages_with_unique_lookup_key: List[Message],
 ):
+
     messages = messages_with_unique_lookup_key
 
     # pass as training data
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/featurizers/test_single_state_featurizers.py#L414'>tests/core/featurizers/test_single_state_featurizers.py~L414</a>
```diff
 
 
 def test_encode_entities__with_entity_roles_and_groups():
+
     # create fake message that has been tokenized and entities have been extracted
     text = "I am flying from London to Paris"
     tokens = [
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/featurizers/test_single_state_featurizers.py#L471'>tests/core/featurizers/test_single_state_featurizers.py~L471</a>
```diff
 
 
 def test_encode_entities__with_bilou_entity_roles_and_groups():
+
     # Instantiate domain and configure the single state featurizer for this domain.
     # Note that there are 2 entity tags here.
     entity_tags = ["city", f"city{ENTITY_LABEL_SEPARATOR}to"]
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_ted_policy.py#L101'>tests/core/policies/test_ted_policy.py~L101</a>
```diff
 class TestTEDPolicy(PolicyTestCollection):
     @staticmethod
     def _policy_class_to_test() -> Type[TEDPolicy]:
+
         return TEDPolicy
 
     def test_train_model_checkpointing(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L56'>tests/core/policies/test_unexpected_intent_policy.py~L56</a>
```diff
 class TestUnexpecTEDIntentPolicy(TestTEDPolicy):
     @staticmethod
     def _policy_class_to_test() -> Type[UnexpecTEDIntentPolicy]:
+
         return UnexpecTEDIntentPolicy
 
     @pytest.fixture(scope="class")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L98'>tests/core/policies/test_unexpected_intent_policy.py~L98</a>
```diff
     def test_label_data_assembly(
         self, trained_policy: UnexpecTEDIntentPolicy, default_domain: Domain
     ):
+
         # Construct input data
         state_featurizer = trained_policy.featurizer.state_featurizer
         encoded_all_labels = state_featurizer.encode_all_labels(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L846'>tests/core/policies/test_unexpected_intent_policy.py~L846</a>
```diff
             all_similarities: np.array,
             label_index: int,
         ):
+
             expected_score = all_similarities[0][label_index]
             expected_threshold = (
                 all_thresholds[label_index] if label_index in all_thresholds else None
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_actions.py#L787'>tests/core/test_actions.py~L787</a>
```diff
 
 
 async def test_response_channel_specific(default_nlg, default_tracker, domain: Domain):
+
     output_channel = SlackBot("DummyToken", "General")
 
     events = await ActionBotResponse("utter_channel").run(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_broker.py#L99'>tests/core/test_broker.py~L99</a>
```diff
 
 
 async def test_pika_raise_connection_exception(monkeypatch: MonkeyPatch):
+
     monkeypatch.setattr(
         PikaEventBroker, "connect", AsyncMock(side_effect=ChannelNotFoundEntity())
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_broker.py#L324'>tests/core/test_broker.py~L324</a>
```diff
 
 
 async def test_create_pika_invalid_port():
+
     cfg = EndpointConfig(
         username="username", password="password", type="pika", port="PORT"
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_nlg.py#L12'>tests/core/test_nlg.py~L12</a>
```diff
 
 
 def nlg_app(base_url="/"):
+
     app = Sanic("test_nlg")
 
     @app.route(base_url, methods=["POST"])
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_policies.py#L424'>tests/core/test_policies.py~L424</a>
```diff
         default_domain: Domain,
         stories_path: Text,
     ):
+
         execution_context = dataclasses.replace(execution_context, is_finetuning=True)
         loaded_policy = MemoizationPolicy.load(
             trained_policy.config, model_storage, resource, execution_context
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_test.py#L198'>tests/core/test_test.py~L198</a>
```diff
     monkeypatch: MonkeyPatch,
     moodbot_domain_path: Path,
 ) -> Callable[[Path, bool], Coroutine]:
+
     # We need `RulePolicy` to predict the correct actions
     # in a particular conversation context as seen during training.
     # Since it can get affected by `action_unlikely_intent` being triggered in
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_tracker_stores.py#L177'>tests/core/test_tracker_stores.py~L177</a>
```diff
 
 
 def test_redis_tracker_store_invalid_key_prefix(domain: Domain):
+
     test_invalid_key_prefix = "$$ &!"
 
     tracker_store = RedisTrackerStore(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/recipes/test_default_recipe.py#L593'>tests/engine/recipes/test_default_recipe.py~L593</a>
```diff
 
 
 def test_dump_config_missing_file(tmp_path: Path, capsys: CaptureFixture):
+
     config_path = tmp_path / "non_existent_config.yml"
 
     config = rasa.shared.utils.io.read_config_file(str(SOME_CONFIG))
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_caching.py#L50'>tests/engine/test_caching.py~L50</a>
```diff
         model_storage: ModelStorage,
         output_fingerprint: Text,
     ) -> "TestCacheableOutput":
+
         value = rasa.shared.utils.io.read_json_file(directory / "cached.json")
 
         return cls(value, cache_dir=directory)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/test_validation.py#L91'>tests/engine/test_validation.py~L91</a>
```diff
     language: Optional[Text] = None,
     is_train_graph: bool = True,
 ) -> GraphModelConfiguration:
+
     parent_node = {}
     if parent:
         parent_node = {
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_components.py#L17'>tests/engine/training/test_components.py~L17</a>
```diff
 
 
 def test_cached_component_returns_value_from_cache(default_model_storage: ModelStorage):
+
     cached_output = CacheableText("Cache me!!")
 
     node = GraphNode(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_components.py#L105'>tests/engine/training/test_components.py~L105</a>
```diff
 def test_fingerprint_component_hit(
     default_model_storage: ModelStorage, temp_cache: TrainingCache
 ):
+
     cached_output = CacheableText("Cache me!!")
     output_fingerprint = uuid.uuid4().hex
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_components.py#L159'>tests/engine/training/test_components.py~L159</a>
```diff
 def test_fingerprint_component_miss(
     default_model_storage: ModelStorage, temp_cache: TrainingCache
 ):
+
     component_config = {"x": 1}
 
     node = GraphNode(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_graph_trainer.py#L108'>tests/engine/training/test_graph_trainer.py~L108</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_graph_trainer.py#L206'>tests/engine/training/test_graph_trainer.py~L206</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_graph_trainer.py#L259'>tests/engine/training/test_graph_trainer.py~L259</a>
```diff
     train_with_schema: Callable,
     spy_on_all_components: Callable,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/engine/training/test_graph_trainer.py#L372'>tests/engine/training/test_graph_trainer.py~L372</a>
```diff
     train_with_schema: Callable,
     caplog: LogCaptureFixture,
 ):
+
     input_file = tmp_path / "input_file.txt"
     input_file.write_text("3")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/examples/test_example_bots_training_data.py#L65'>tests/examples/test_example_bots_training_data.py~L65</a>
```diff
     raise_slot_warning: bool,
     msg: Optional[Text],
 ):
+
     importer = TrainingDataImporter.load_from_config(
         config_file, domain_file, [data_folder]
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L685'>tests/graph_components/validators/test_default_recipe_validator.py~L685</a>
```diff
 def test_core_warn_if_data_but_no_policy(
     monkeypatch: MonkeyPatch, policy_type: Optional[Type[Policy]]
 ):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_e2ebot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L814'>tests/graph_components/validators/test_default_recipe_validator.py~L814</a>
```diff
 def test_core_raise_if_a_rule_policy_is_incompatible_with_domain(
     monkeypatch: MonkeyPatch,
 ):
+
     domain = Domain.empty()
 
     num_instances = 2
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L859'>tests/graph_components/validators/test_default_recipe_validator.py~L859</a>
```diff
     num_duplicates: bool,
     priority: int,
 ):
+
     assert len(policy_types) >= priority + num_duplicates, (
         f"This tests needs at least {priority + num_duplicates} many types."
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L923'>tests/graph_components/validators/test_default_recipe_validator.py~L923</a>
```diff
 
 @pytest.mark.parametrize("policy_type_consuming_rule_data", [RulePolicy])
 def test_core_warn_if_rule_data_missing(policy_type_consuming_rule_data: Type[Policy]):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_e2ebot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/graph_components/validators/test_default_recipe_validator.py#L952'>tests/graph_components/validators/test_default_recipe_validator.py~L952</a>
```diff
 def test_core_warn_if_rule_data_unused(
     policy_type_not_consuming_rule_data: Type[Policy],
 ):
+
     importer = TrainingDataImporter.load_from_dict(
         domain_path="data/test_moodbot/domain.yml",
         training_data_paths=[
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L132'>tests/nlu/classifiers/test_diet_classifier.py~L132</a>
```diff
         message_text: Text = "Rasa is great!",
         expect_intent: bool = True,
     ) -> Message:
+
         if not pipeline:
             pipeline = [
                 {"component": WhitespaceTokenizer},
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_regex_message_handler.py#L38'>tests/nlu/classifiers/test_regex_message_handler.py~L38</a>
```diff
 def test_process_does_not_do_anything(
     regex_message_handler: RegexMessageHandler, text: Text
 ):
+
     message = Message(
         data={TEXT: text, INTENT: "bla"},
         features=[
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_regex_message_handler.py#L77'>tests/nlu/classifiers/test_regex_message_handler.py~L77</a>
```diff
 def test_regex_message_handler_adds_extractor_name(
     regex_message_handler: RegexMessageHandler, text: Text
 ):
+
     message = Message(
         data={TEXT: text, INTENT: "bla"},
         features=[
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/conftest.py#L39'>tests/nlu/conftest.py~L39</a>
```diff
     def inner(
         pipeline: List[Dict[Text, Any]], training_data: Union[Text, TrainingData]
     ) -> Tuple[TrainingData, List[GraphComponent]]:
+
         if isinstance(training_data, str):
             importer = RasaFileImporter(training_data_paths=[training_data])
             training_data: TrainingData = importer.get_nlu_data()
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/conftest.py#L75'>tests/nlu/conftest.py~L75</a>
```diff
 @pytest.fixture()
 def process_message(default_model_storage: ModelStorage) -> Callable[..., Message]:
     def inner(loaded_pipeline: List[GraphComponent], message: Message) -> Message:
+
         for component in loaded_pipeline:
             component.process([message])
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/extractors/test_crf_entity_extractor.py#L128'>tests/nlu/extractors/test_crf_entity_extractor.py~L128</a>
```diff
     spacy_nlp_component: SpacyNLP,
     spacy_model: SpacyModel,
 ):
+
     crf_extractor = crf_entity_extractor(config_params)
 
     importer = RasaFileImporter(training_data_paths=["data/examples/rasa"])
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/extractors/test_mitie_entity_extractor.py#L73'>tests/nlu/extractors/test_mitie_entity_extractor.py~L73</a>
```diff
     mitie_model: MitieModel,
     with_trainable_examples: bool,
 ):
+
     # some texts where last token is a city
     texts_ending_with_city = ["Bert lives in Berlin", "Ernie asks where is Bielefeld"]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/extractors/test_regex_entity_extractor.py#L278'>tests/nlu/extractors/test_regex_entity_extractor.py~L278</a>
```diff
 def test_process_does_not_overwrite_any_entities(
     create_or_load_extractor: Callable[..., RegexEntityExtractor],
 ):
+
     pre_existing_entity = {
         ENTITY_ATTRIBUTE_TYPE: "person",
         ENTITY_ATTRIBUTE_VALUE: "Max",
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_lexical_syntactic_featurizer.py#L239'>tests/nlu/featurizers/test_lexical_syntactic_featurizer.py~L239</a>
```diff
     resource_lexical_syntactic_featurizer: Resource,
     feature_config: List[Text],
 ) -> Callable[..., LexicalSyntacticFeaturizer]:
+
     config = {"alias": "lsf", "features": feature_config}
     featurizer = create_lexical_syntactic_featurizer(config)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_lexical_syntactic_featurizer.py#L300'>tests/nlu/featurizers/test_lexical_syntactic_featurizer.py~L300</a>
```diff
     feature_config: Dict[Text, Any],
     expected_features: np.ndarray,
 ):
+
     featurizer = create_lexical_syntactic_featurizer({
         "alias": "lsf",
         "features": feature_config,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_lm_featurizer.py#L810'>tests/nlu/featurizers/test_lm_featurizer.py~L810</a>
```diff
         [Dict[Text, Any]], LanguageModelFeaturizer
     ],
 ):
+
     config = {"model_name": "bert", "model_weights": "bert-base-chinese"}
 
     lm_featurizer = create_language_model_featurizer(config)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_mitie_featurizer.py#L57'>tests/nlu/featurizers/test_mitie_featurizer.py~L57</a>
```diff
     mitie_model: MitieModel,
     mitie_tokenizer: MitieTokenizer,
 ):
+
     featurizer = create({"alias": "mitie_featurizer"})
 
     sentence = "Hey how are you today"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_mitie_featurizer.py#L87'>tests/nlu/featurizers/test_mitie_featurizer.py~L87</a>
```diff
     mitie_model: MitieModel,
     mitie_tokenizer: MitieTokenizer,
 ):
+
     featurizer = create({"alias": "mitie_featurizer"})
 
     sentence = "Hey how are you today"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_regex_featurizer.py#L302'>tests/nlu/featurizers/test_regex_featurizer.py~L302</a>
```diff
     create_featurizer: Callable[..., RegexFeaturizer],
     spacy_tokenizer: SpacyTokenizer,
 ):
+
     patterns = [
         {"pattern": "[0-9]+", "name": "number", "usage": "intent"},
         {"pattern": "\\bhey*", "name": "hello", "usage": "intent"},
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_regex_featurizer.py#L398'>tests/nlu/featurizers/test_regex_featurizer.py~L398</a>
```diff
     create_featurizer: Callable[..., RegexFeaturizer],
     spacy_tokenizer: SpacyTokenizer,
 ):
+
     patterns = [
         {"pattern": "[0-9]+", "name": "number", "usage": "intent"},
         {"pattern": "\\bhey*", "name": "hello", "usage": "intent"},
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_spacy_featurizer.py#L39'>tests/nlu/featurizers/test_spacy_featurizer.py~L39</a>
```diff
 
 @pytest.mark.parametrize("sentence", ["hey how are you today"])
 def test_spacy_featurizer(sentence, spacy_nlp):
+
     ftr = create_spacy_featurizer({})
 
     doc = spacy_nlp(sentence)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/featurizers/test_spacy_featurizer.py#L157'>tests/nlu/featurizers/test_spacy_featurizer.py~L157</a>
```diff
 
 
 def test_spacy_featurizer_train(spacy_nlp):
+
     featurizer = create_spacy_featurizer({})
 
     sentence = "Hey how are you today"
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L125'>tests/nlu/selectors/test_selectors.py~L125</a>
```diff
         config_params: Dict[Text, Any],
         should_finetune: bool,
     ):
+
         training_data, loaded_pipeline = train_and_preprocess(
             pipeline, "data/examples/rasa/demo-rasa.yml"
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L225'>tests/nlu/selectors/test_selectors.py~L225</a>
```diff
     response_selector_training_data: TrainingData,
     create_response_selector: Callable[[Dict[Text, Any]], ResponseSelector],
 ):
+
     training_data_extra_intent = TrainingData([
         Message.build(text="Is it possible to detect the version?", intent="faq/q1"),
         Message.build(text="How can I get a new virtual env", intent="faq/q2"),
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L276'>tests/nlu/selectors/test_selectors.py~L276</a>
```diff
     response_selector_training_data: TrainingData,
     create_response_selector: Callable[[Dict[Text, Any]], ResponseSelector],
 ):
+
     response_selector = create_response_selector({"use_text_as_label": train_on_text})
     response_selector.preprocess_train_data(response_selector_training_data)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/selectors/test_selectors.py#L342'>tests/nlu/selectors/test_selectors.py~L342</a>
```diff
     default_execution_context: ExecutionContext,
     train_persist_load_with_different_settings,
 ):
+
     pipeline = [
         {"component": WhitespaceTokenizer},
         {"component": CountVectorsFeaturizer},
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/test_evaluation.py#L918'>tests/nlu/test_evaluation.py~L918</a>
```diff
 
 
 def test_entity_evaluation_report(tmp_path: Path):
+
     path = tmp_path / "evaluation"
     path.mkdir()
     report_folder = str(path / "reports")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/tokenizers/test_whitespace_tokenizer.py#L80'>tests/nlu/tokenizers/test_whitespace_tokenizer.py~L80</a>
```diff
     ],
 )
 def test_whitespace(text, expected_tokens, expected_indices):
+
     tk = create_whitespace_tokenizer()
 
     tokens = tk.tokenize(Message.build(text=text), attribute=TEXT)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/utils/test_bilou_utils.py#L88'>tests/nlu/utils/test_bilou_utils.py~L88</a>
```diff
 
 
 def test_apply_bilou_schema(whitespace_tokenizer: WhitespaceTokenizer):
+
     message_1 = Message.build(
         text="Germany is part of the European Union", intent="inform"
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/utils/test_bilou_utils.py#L228'>tests/nlu/utils/test_bilou_utils.py~L228</a>
```diff
     debug_message: Optional[Text],
     caplog: LogCaptureFixture,
 ):
+
     with caplog.at_level(logging.DEBUG):
         actual_tags, actual_confidences = bilou_utils.ensure_consistent_bilou_tagging(
             tags, confidences
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/core/training_data/story_reader/test_yaml_story_reader.py#L300'>tests/shared/core/training_data/story_reader/test_yaml_story_reader.py~L300</a>
```diff
 
 
 def test_read_rules_with_stories(domain: Domain):
+
     yaml_file = "data/test_yaml_stories/stories_and_rules.yml"
 
     steps = loading.load_data_from_files([yaml_file], domain)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/formats/test_rasa_yaml.py#L132'>tests/shared/nlu/training_data/formats/test_rasa_yaml.py~L132</a>
```diff
 
 
 def test_wrong_format_raises():
+
     wrong_yaml_nlu_content = """
     !!
     """
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/formats/test_rasa_yaml.py#L145'>tests/shared/nlu/training_data/formats/test_rasa_yaml.py~L145</a>
```diff
     "example", [WRONG_YAML_NLU_CONTENT_1, WRONG_YAML_NLU_CONTENT_2]
 )
 def test_wrong_schema_raises(example: Text):
+
     parser = RasaYAMLReader()
     with pytest.raises(YamlException):
         parser.reads(example)
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/formats/test_rasa_yaml.py#L336'>tests/shared/nlu/training_data/formats/test_rasa_yaml.py~L336</a>
```diff
 
 
 def test_lookup_is_parsed():
+
     parser = RasaYAMLReader()
     training_data = parser.reads(LOOKUP_EXAMPLE)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/formats/test_rasa_yaml.py#L344'>tests/shared/nlu/training_data/formats/test_rasa_yaml.py~L344</a>
```diff
 
 
 def test_regex_is_parsed():
+
     parser = RasaYAMLReader()
     training_data = parser.reads(REGEX_EXAMPLE)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_entities_parser.py#L131'>tests/shared/nlu/training_data/test_entities_parser.py~L131</a>
```diff
 def test_markdown_entity_regex(
     example: Text, expected_entities: List[Dict[Text, Any]], expected_text: Text
 ):
+
     result = entities_parser.find_entities_in_training_example(example)
     assert result == expected_entities
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_features.py#L250'>tests/shared/nlu/training_data/test_features.py~L250</a>
```diff
     ),
 )
 def test_combine(is_sparse: bool, type: Text, number: int, use_expected_origin: bool):
+
     features_list, modifications = _generate_feature_list_and_modifications(
         is_sparse=is_sparse, type=type, number=number
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_features.py#L291'>tests/shared/nlu/training_data/test_features.py~L291</a>
```diff
     ),
 )
 def test_filter(is_sparse: bool, type: Text, number: int):
+
     features_list, modifications = _generate_feature_list_and_modifications(
         is_sparse=is_sparse, type=type, number=number
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_features.py#L344'>tests/shared/nlu/training_data/test_features.py~L344</a>
```diff
     num_features_per_attribute: Dict[Text, int],
     specified_attributes: Optional[List[Text]],
 ):
+
     features_list = []
     for attribute, number in num_features_per_attribute.items():
         for idx in range(number):
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_features.py#L382'>tests/shared/nlu/training_data/test_features.py~L382</a>
```diff
 def test_reduce(
     shuffle_mode: Text, num_features_per_combination: Tuple[int, int, int, int]
 ):
+
     # all combinations - in the expected order
     # (i.e. all sparse before all dense and sequence before sentence)
     all_combinations = [
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_message.py#L60'>tests/shared/nlu/training_data/test_message.py~L60</a>
```diff
     expected_seq_features: Optional[List[Features]],
     expected_sen_features: Optional[List[Features]],
 ):
+
     message = Message(data={TEXT: "This is a test sentence."}, features=features)
 
     actual_seq_features, actual_sen_features = message.get_dense_features(
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_synonyms_parser.py#L2'>tests/shared/nlu/training_data/test_synonyms_parser.py~L2</a>
```diff
 
 
 def test_add_synonym():
+
     synonym_name = "savings"
     synonym_examples = ["pink pig", "savings account"]
     expected_result = {"pink pig": synonym_name, "savings account": synonym_name}
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_synonyms_parser.py#L15'>tests/shared/nlu/training_data/test_synonyms_parser.py~L15</a>
```diff
 
 
 def test_add_synonyms_from_entities():
+
     training_example = "I want to fly from Berlin to LA"
 
     entities = [
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/utils/schemas/test_events.py#L72'>tests/shared/utils/schemas/test_events.py~L72</a>
```diff
 
 @pytest.mark.parametrize("event_class", rasa.shared.utils.common.all_subclasses(Event))
 def test_remote_action_validate_all_event_subclasses(event_class: Type[Event]):
+
     if event_class.type_name == "slot":
         response = {
             "events": [{"event": "slot", "name": "test", "value": "example"}],
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/utils/test_io.py#L591'>tests/shared/utils/test_io.py~L591</a>
```diff
 
 @pytest.mark.parametrize("length", [4, 8, 16, 32])
 def test_random_string(length):
+
     s = rasa.shared.utils.io.random_string(length)
     s2 = rasa.shared.utils.io.random_string(length)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/utils/test_validation.py#L295'>tests/shared/utils/test_validation.py~L295</a>
```diff
 
 
 async def test_future_training_data_format_version_not_compatible():
+
     next_minor = str(Version(LATEST_TRAINING_DATA_FORMAT_VERSION).next_minor())
 
     incompatible_version = {KEY_TRAINING_DATA_FORMAT_VERSION: next_minor}
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/utils/test_validation.py#L306'>tests/shared/utils/test_validation.py~L306</a>
```diff
 
 
 async def test_compatible_training_data_format_version():
+
     prev_major = str(Version("1.0"))
 
     compatible_version_1 = {KEY_TRAINING_DATA_FORMAT_VERSION: prev_major}
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/utils/test_validation.py#L319'>tests/shared/utils/test_validation.py~L319</a>
```diff
 
 
 async def test_invalid_training_data_format_version_warns():
+
     invalid_version_1 = {KEY_TRAINING_DATA_FORMAT_VERSION: 2.0}
     invalid_version_2 = {KEY_TRAINING_DATA_FORMAT_VERSION: "Rasa"}
 
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/test_server.py#L693'>tests/test_server.py~L693</a>
```diff
 
 
 async def test_add_message(rasa_app: SanicASGITestClient):
+
     conversation_id = "test_add_message_test_id"
 
     _, response = await rasa_app.get(f"/conversations/{conversation_id}/tracker")
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/utils/tensorflow/test_models.py#L35'>tests/utils/tensorflow/test_models.py~L35</a>
```diff
     new_batch_outputs: Dict[Text, Union[np.ndarray, Dict[Text, np.ndarray]]],
     expected_output: Dict[Text, Union[np.ndarray, Dict[Text, np.ndarray]]],
 ):
+
     predicted_output = RasaModel._merge_batch_outputs(
         existing_outputs, new_batch_outputs
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/utils/tensorflow/test_models.py#L72'>tests/utils/tensorflow/test_models.py~L72</a>
```diff
     def _batch_predict(
         batch_in: Tuple[np.ndarray],
     ) -> Dict[Text, Union[np.ndarray, Dict[Text, np.ndarray]]]:
+
         dummy_output = batch_in[0]
         output = {
             "dummy_output": dummy_output,
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+59 -0 lines across 34 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/4ffbe639c2412fbfa2877978877e38d16dd8f284/src/snowflake/cli/_plugins/logs/manager.py#L87'>src/snowflake/cli/_plugins/logs/manager.py~L87</a>
```diff
         log_level: Optional[str] = "INFO",
         partial_match: bool = False,
     ) -> SnowflakeCursor:
+
         table = event_table if event_table else "SNOWFLAKE.TELEMETRY.EVENTS"
 
         # Escape single quotes in object_name to prevent SQL injection
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/4ffbe639c2412fbfa2877978877e38d16dd8f284/src/snowflake/cli/_plugins/snowpark/snowpark_entity.py#L41'>src/snowflake/cli/_plugins/snowpark/snowpark_entity.py~L41</a>
```diff
 
 class SnowparkEntity(EntityBase[Generic[T]]):
     def __init__(self, *args, **kwargs):
+
         if not FeatureFlag.ENABLE_NATIVE_APP_CHILDREN.is_enabled():
             raise NotImplementedError("Snowpark entity is not implemented yet")
         super().__init__(*args, **kwargs)
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/4ffbe639c2412fbfa2877978877e38d16dd8f284/src/snowflake/cli/_plugins/snowpark/zipper.py#L67'>src/snowflake/cli/_plugins/snowpark/zipper.py~L67</a>
```diff
     dest_zip: Path,
     mode: Literal["r", "w", "x", "a"] = "w",
 ) -> None:
+
     if not dest_zip.parent.exists():
         SecurePath(dest_zip).parent.mkdir(parents=True)
 
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/4ffbe639c2412fbfa2877978877e38d16dd8f284/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L42'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py~L42</a>
```diff
         comment: Optional[str],
         if_not_exists: bool,
     ) -> SnowflakeCursor:
+
         create_statement = "CREATE COMPUTE POOL"
         if if_not_exists:
             create_statement = f"{create_statement} IF NOT EXISTS"
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/4ffbe639c2412fbfa2877978877e38d16dd8f284/src/snowflake/cli/_plugins/spcs/compute_pool/manager.py#L79'>src/snowflake/cli/_plugins/spcs/compute_pool/manager.py~L79</a>
```diff
         comment: Optional[str],
  ...*[Comment body truncated]*

---

_Comment by @ntBre on 2025-10-28 17:10_

The ecosystem results look good to me. There were only a couple of somewhat surprising cases:

- https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/conftest.py#L79
  ```py
	 @pytest.fixture()
	def process_message(default_model_storage: ModelStorage) -> Callable[..., Message]:
	    def inner(loaded_pipeline: List[GraphComponent], message: Message) -> Message:
	
	        for component in loaded_pipeline:
	            component.process([message])
	
	        return message
	
	    return inner
  ```
- https://github.com/snowflakedb/snowflake-cli/blob/b0306e131063d26be11f14cb1f9dbd9ae67b32c8/src/snowflake/cli/_plugins/snowpark/snowpark_entity.py#L44
  ```py
	class SnowparkEntity(EntityBase[Generic[T]]):
	    def __init__(self, *args, **kwargs):
	
	        if not FeatureFlag.ENABLE_NATIVE_APP_CHILDREN.is_enabled():
	            raise NotImplementedError("Snowpark entity is not implemented yet")
	        super().__init__(*args, **kwargs)
	```

These both feel a bit awkward to me because they preserve a blank line inside the inner functions but not outside. But I guess we're only preserving the input whitespace, so this might still be what the authors intended.

---

_Marked ready for review by @ntBre on 2025-10-28 19:48_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-28 19:48_

---

_@amyreese reviewed on 2025-10-29 01:33_

---

_Review comment by @amyreese on `crates/ruff_python_formatter/tests/snapshots/format@newlines.py.snap`:416 on 2025-10-29 01:33_

Is this expected? (edit: I'm guessing yes, missed the snapshot change further down with the single empty line)

---

_@amyreese approved on 2025-10-29 01:36_

---

_@ntBre reviewed on 2025-10-29 01:36_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@newlines.py.snap`:416 on 2025-10-29 01:36_

You scared me! 😄 

I'm still getting used to the input appearing in the formatter snapshots.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/newlines.py`:410 on 2025-10-29 15:07_

Can we add more tests to this, specifically tests including comments or cases where the first element is a function on its own?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:181 on 2025-10-29 15:16_

This elipsis check should exactly match the check we do in the function formatting (also test for trailing comments)

https://github.com/astral-sh/ruff/blob/c4506f0e94882fa1118eb2a13339f9be7bf0bcaa/crates/ruff_python_formatter/src/statement/clause.rs#L451-L453

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 15:22_

I think this is correct but maybe worth adding a test that we collapse any empty line before a comment:

```py
def test():

    # comment

    a

```

gets formatted to 

```py
def test():
    # comment

    a
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@range_formatting__indent.py.snap`:175 on 2025-10-29 15:27_

Can we add another test like this. As I don't think the original I wrote captured what I wanted to test

```py
def test6 ():
    <RANGE_START>print("Format" )
    print(3 +  4)<RANGE_END>
    print("Format to fix indentation" )
```



---

_@MichaReiser reviewed on 2025-10-29 15:32_

I like the idea of restricting it to functions with docstrings but I'm also still concerned that it reduces consistency and will lead to formatting discussions on diff, the very problem Ruff format tries to solve. 

For example, I dislike this change and I'd be inclined to comment in the PR review that the empty line here is unnecessary:

```
 def nlg_app(base_url="/"):
+
     app = Sanic("test_nlg")
 
     @app.route(base_url, methods=["POST"])
```


Having said that, I think this strikes a good balance.


I went through the original issue and I noticed that we don't preserve the empty line for

```py
if long_boolean_variable_name and another_long_conditional:

    # A pretty long comment documenting what the block does
    print("Do stuff")
```

Which I'd find surprising and seems worth checking if we can support it too

---

_@ntBre reviewed on 2025-10-29 17:02_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 17:02_

Is that different from the new test here?

https://github.com/astral-sh/ruff/blob/abb54b32b0bbf5becfdf58fb9aae5e10b951141a/crates/ruff_python_formatter/resources/test/fixtures/ruff/newlines.py#L358-L362

It looks like the formatting got slightly mangled in your comment.

---

_@ntBre reviewed on 2025-10-29 17:18_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:181 on 2025-10-29 17:18_

I added two new tests trying to exercise this, and they both seemed to work as I expected without any additional check:

```py
def preserved5():

    ...
    # trailing comment prevents collapsing the stub


def removed4():

    ...  # trailing same-line comment does not prevent collapsing the stub
```

and in fact when I updated this condition to:

```rust
            let will_collapse_stub_function =
                contains_only_an_ellipsis(statements, f.context().comments())
                    && !first_comments.has_trailing();
```

I caused another syntax error for the `removed4` case. `contains_only_an_ellipsis` already has one kind of check for trailing comments:

https://github.com/astral-sh/ruff/blob/d38a5292d2a9c8509da6868046da0caa49ed5e46/crates/ruff_python_formatter/src/statement/suite.rs#L730-L739

but I'm very likely to be missing something. Is [`first_comments`](https://github.com/astral-sh/ruff/blob/d38a5292d2a9c8509da6868046da0caa49ed5e46/crates/ruff_python_formatter/src/statement/suite.rs#L151) not what I should be checking?

---

_Comment by @ntBre on 2025-10-29 18:27_

> I went through the original issue and I noticed that we don't preserve the empty line for

Should we try preserving the empty line for any `if`, any `if` with a "long"/compound condition, and/or where the body starts with a comment? Just checking which part of the example to focus on.

One reason I was wary of the conditionals is that they don't seem much different from loops. For [example](https://play.ruff.rs/de50a314-446f-41c3-894e-48e793ddcd0e?secondary=Format):

```py
for i in some_generator_function(
    with_many_arguments=[1, 2, 3], another=(4, 5, 6), one_more=None
):

    # a comment
    print("body of the loop")
```

Should we handle those too?

Functions seemed like a good place to draw the line to me, but I can see an argument for more consistency too.

---

_Comment by @MichaReiser on 2025-10-29 18:31_

> Should we try preserving the empty line for any if, any if with a "long"/compound condition, and/or where the body starts with a comment? Just checking which part of the example to focus on.

No sorry. I think I tested it with a function header but then copied the original example from the issue, but maybe I'm just jetlagged and tested it with a function header?

So what I think we should preserve is:


```py
def foo():

    # A pretty long comment documenting what the block does
    print("Do stuff")
```

---

_Comment by @ntBre on 2025-10-29 18:39_

Ohh okay, that makes sense! So preserve lines before comments instead of removing them. I think that does make sense.

At least one person on one of the Black issues even wanted to preserve empty lines before docstrings. I don't feel too strongly about either comments or docstrings, I was just trying to narrow the scope even further.

---

_Comment by @MichaReiser on 2025-10-29 18:42_

> At least one person on one of the Black issues even wanted to preserve empty lines before docstrings. I don't feel too strongly about either comments or docstrings, I was just trying to narrow the scope even further.

The docstrings make sense to me (not allowing an empty line) and is sort of named exactly in the rule. The comments just were a surprise to me (but I don't feel strongly about it)

---

_Comment by @ntBre on 2025-10-29 19:10_

Oops, last commit was too simplistic. We're now adding blank lines before comments rather than just preserving them. That looks like the majority of new ecosystem hits.

---

_@ntBre reviewed on 2025-10-29 19:26_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 19:26_

Ah I see, this is what's coming up in the ecosystem check now. I accidentally skirted around this by not preserving cases with any leading comments.

---

_@ntBre reviewed on 2025-10-29 19:53_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 19:53_

`lines_before` may not be what I want here, if we want to preserve blank lines before comments. All of these are returning 2 `lines_before`:

```py
def untouched1():

    # comment
    # more lines in the comment
    # and one more

    return 0


def untouched2():

    # comment

    return 0


def untouched3():
    # comment

    return 0
```

---

_@MichaReiser reviewed on 2025-10-29 19:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 19:55_

That, or you'd have to pass the position of the first leading comment as the offset

---

_@MichaReiser reviewed on 2025-10-29 19:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 19:57_

Note, we do have a `lines_after_ignore_trivia` (or similar) that uses the tokenizer over doing a string search

---

_@ntBre reviewed on 2025-10-29 20:19_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:184 on 2025-10-29 20:19_

Ah I didn't consider that I could pass a comment offset there. I assumed it had to be a statement node. Thanks!

I saw `lines_after_ignore_trivia`, but not a `before` variant. Happy to try that approach if it would be better, but using the comment offset seems to be working well.

---

_Comment by @ntBre on 2025-10-29 20:51_

The ecosystem check looks good to me again. I didn't click through to every change, but I downloaded the full output from the Actions log and checked more than the subset in the comment.

~~This might be expected, but most of the links don't point to the exact line of the diff, which makes it a bit more of a manual process.~~ I checked the script and this is expected.

---

_@MichaReiser reviewed on 2025-10-29 20:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:181 on 2025-10-29 20:52_

Maybe it's not relevant. You have to trace through `FormatStmtFunctionDef`. 

But yes, `first_comments` is the wrong thing to test. `trailing_comments` in `FormatClauseBody` are the `trailing_definition_comments` in `FormatStmtFunctionDef`. So I think it's more about something like 

```py
def test(): # comment


	...
```

---

_@ntBre reviewed on 2025-10-30 14:37_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:181 on 2025-10-30 14:37_

Ah okay, I see now. Thanks! Yes, your example comment prevents collapsing the stub in `FormatClauseBody`, but we remove the newline because the logic is different.

Is there a way to pass this `should_collapse_stub` information into the suite formatting (maybe another `with_options` implementation?)? Or alternatively a way to retrieve the parent function definition so that we could recompute it?

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/suite.rs`:181 on 2025-10-31 14:47_

As we discussed on Discord, I added a variant of `contains_only_an_ellipsis` that returns the ellipsis statement, which we can then format directly in `FormatClauseBody` instead of calling into the suite formatting at all. Now the blank line in this example is correctly preserved.

---

_@ntBre reviewed on 2025-10-31 14:47_

---

_@MichaReiser approved on 2025-10-31 18:26_

---

_Merged by @ntBre on 2025-10-31 18:53_

---

_Closed by @ntBre on 2025-10-31 18:53_

---

_Branch deleted on 2025-10-31 18:53_

---

_Comment by @astralcai on 2025-11-26 16:26_

Thank you for making this change! I see that this feature is only available in preview. How do I enable this behaviour without enabling all other rules in preview?

---

_Comment by @MichaReiser on 2025-11-26 17:13_

You can set `format.preview` which enables all preview formatting changes. There's no feature-specific flag. You'll have to wait for our next minor where we plan on stabilizing this feature.

---
