```yaml
number: 9989
title: Indent nested binary expressions
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: preview-binary-formatting
created_at: 2024-02-14T17:54:46Z
updated_at: 2024-04-08T07:03:51Z
url: https://github.com/astral-sh/ruff/pull/9989
synced_at: 2026-01-12T15:55:30Z
```

# Indent nested binary expressions

---

_@MichaReiser_

## Summary

This isn't it yet... and in no way what we recommend going forward. This PR is just me playing around with some different formatting and to see what works and what doesn't.

The formatting of very long binary expression improves but there are now many cases
where the indent feels too deep and breaking before the `+` operator feels wrong. 
We probably need to become clever of when and when not to insert the line break when indenting as well

This is a good ecosystem stress test :laughing: This also raises the question if something as fundamental as binary expression formatting can still be changed...

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-02-14 18:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+6766 -6660 lines in 1009 files in 29 projects; 14 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+35 -35 lines across 8 files)</summary>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/audit_logs.py#L601'>disnake/audit_logs.py~L601</a>
```diff
                 self.extra = type("_AuditLogProxy", (), elems)()
             elif (
                 self.action is enums.AuditLogAction.member_move
-                or self.action is enums.AuditLogAction.message_delete
+                    or self.action is enums.AuditLogAction.message_delete
             ):
                 elems = {
                     "count": int(extra["count"]),
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/audit_logs.py#L844'>disnake/audit_logs.py~L844</a>
```diff
     ) -> Union[GuildScheduledEvent, Object]:
         return (
             self.guild.get_scheduled_event(target_id)
-            or self._guild_scheduled_events.get(target_id)
-            or Object(id=target_id)
+                or self._guild_scheduled_events.get(target_id)
+                or Object(id=target_id)
         )
 
     def _convert_target_application_command_or_integration(
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/audit_logs.py#L854'>disnake/audit_logs.py~L854</a>
```diff
         # try application command
         if target := (
             self._state._get_guild_application_command(self.guild.id, target_id)
-            or self._state._get_global_application_command(target_id)
-            or self._application_commands.get(target_id)
+                or self._state._get_global_application_command(target_id)
+                or self._application_commands.get(target_id)
         ):
             return target
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/channel.py#L1999'>disnake/channel.py~L1999</a>
```diff
             member
             for member in self.members
             if member.voice
-            and not member.voice.suppress
-            and member.voice.requested_to_speak_at is None
+                and not member.voice.suppress
+                and member.voice.requested_to_speak_at is None
         ]
 
     @property
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/core.py#L552'>disnake/ext/commands/core.py~L552</a>
```diff
             except ArgumentParsingError:
                 if (
                     self._is_typing_optional(param.annotation)
-                    and not param.kind == param.VAR_POSITIONAL
+                        and not param.kind == param.VAR_POSITIONAL
                 ):
                     view.index = previous
                     return None
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/core.py#L2490'>disnake/ext/commands/core.py~L2490</a>
```diff
                     disnake.StageChannel,
                 ),
             )
-            and ch.is_nsfw()
+                and ch.is_nsfw()
         ):
             return True
         raise NSFWChannelRequired(ch)  # type: ignore
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/interaction_bot_base.py#L160'>disnake/ext/commands/interaction_bot_base.py~L160</a>
```diff
 
         if command_sync_flags is not None and (
             sync_commands is not MISSING
-            or sync_commands_debug is not MISSING
-            or sync_commands_on_cog_unload is not MISSING
+                or sync_commands_debug is not MISSING
+                or sync_commands_on_cog_unload is not MISSING
         ):
             raise TypeError(
                 "cannot set 'command_sync_flags' and any of 'sync_commands', 'sync_commands_debug', 'sync_commands_on_cog_unload' at the same time."
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/interaction_bot_base.py#L891'>disnake/ext/commands/interaction_bot_base.py~L891</a>
```diff
 
         if (
             not self._command_sync_flags._sync_enabled
-            or self._sync_queued.locked()
-            or not self.is_ready()
-            or self._is_closed
-            or self.loop.is_closed()
+                or self._sync_queued.locked()
+                or not self.is_ready()
+                or self._is_closed
+                or self.loop.is_closed()
         ):
             return
         # We don't do this task on login or in parallel with a similar task
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/interaction_bot_base.py#L1314'>disnake/ext/commands/interaction_bot_base.py~L1314</a>
```diff
             # if we're not currently syncing,
             not self._sync_queued.locked()
             # and we're instructed to sync guild commands
-            and self._command_sync_flags.sync_guild_commands
-            # and the current command was registered to a guild
-            and interaction.data.get("guild_id")
-            # and we don't know the command
-            and not self.get_guild_command(interaction.guild_id, interaction.data.id)  # type: ignore
+                and self._command_sync_flags.sync_guild_commands
+                # and the current command was registered to a guild
+                and interaction.data.get("guild_id")
+                # and we don't know the command
+                and not self.get_guild_command(interaction.guild_id, interaction.data.id)  # type: ignore
         ):
             # don't do anything if we aren't allowed to disable them
             if self._command_sync_flags.allow_command_deletion:
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/params.py#L643'>disnake/ext/commands/params.py~L643</a>
```diff
         # If we received a `User` but didn't expect one, raise.
         if (
             isinstance(argument, disnake.User)
-            and issubclass_(self.type, disnake.Member)
-            and not issubclass_(self.type, disnake.User)
+                and issubclass_(self.type, disnake.Member)
+                and not issubclass_(self.type, disnake.User)
         ):
             raise errors.MemberNotFound(str(argument.id))
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/params.py#L703'>disnake/ext/commands/params.py~L703</a>
```diff
         if not converter_mode:
             self.converter = (
                 self.converter
-                or getattr(annotation, "__discord_converter__", None)
-                or self._registered_converters.get(annotation)
+                    or getattr(annotation, "__discord_converter__", None)
+                    or self._registered_converters.get(annotation)
             )
             if self.converter:
                 self.parse_converter_annotation(self.converter, annotation)
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/ext/commands/params.py#L735'>disnake/ext/commands/params.py~L735</a>
```diff
             self.type = annotation
         elif (
             isinstance(annotation, (EnumMeta, disnake.enums.EnumMeta))
-            or get_origin(annotation) is Literal
+                or get_origin(annotation) is Literal
         ):
             self._parse_enum(annotation)
         elif get_origin(annotation) in (Union, UnionType):
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/guild.py#L2278'>disnake/guild.py~L2278</a>
```diff
 
         if (
             community is not MISSING
-            or invites_disabled is not MISSING
-            or raid_alerts_disabled is not MISSING
+                or invites_disabled is not MISSING
+                or raid_alerts_disabled is not MISSING
         ):
             # If we don't have complete feature information for the guild,
             # it is possible to disable or enable other features that we didn't intend to touch.
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/invite.py#L440'>disnake/invite.py~L440</a>
```diff
         # if it was stored on the Guild object, we would be throwing away this data from the api request
         if (
             self.guild is not None
-            and (guild_data := data.get("guild"))
-            and "welcome_screen" in guild_data
+                and (guild_data := data.get("guild"))
+                and "welcome_screen" in guild_data
         ):
             self.guild_welcome_screen: Optional[WelcomeScreen] = WelcomeScreen(
                 state=self._state,
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/state.py#L585'>disnake/state.py~L585</a>
```diff
         # If presences are enabled then we get back the old guild.large behaviour
         return (
             self._chunk_guilds
-            and not guild.chunked
-            and not (self._intents.presences and not guild.large)
+                and not guild.chunked
+                and not (self._intents.presences and not guild.large)
         )
 
     def _get_guild_channel(
```
<a href='https://github.com/DisnakeDev/disnake/blob/47022234738cc34a70ac3b53bc8df54b1593bc76/disnake/state.py#L767'>disnake/state.py~L767</a>
```diff
             # This mirrors the current client and API behavior.
             if channel.__class__ is Thread and not (
                 message.type is MessageType.thread_starter_message
-                or (
-                    type(channel.parent) in (ForumChannel, MediaChannel)  # type: ignore
-                    and channel.id == message.id
-                )
+                    or (
+                        type(channel.parent) in (ForumChannel, MediaChannel)  # type: ignore
+                            and channel.id == message.id
+                    )
             ):
                 channel.total_message_sent += 1  # type: ignore
                 channel.message_count += 1  # type: ignore
```

</p>
</details>
<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+3 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/clickhouse/table.py#L5'>housewatch/clickhouse/table.py~L5</a>
```diff
     QUERY = """SELECT engine FROM system.tables WHERE database = '%(database)s' AND name = '%(table)s'"""
     return (
         "replicated"
-        in run_query(QUERY, {"database": database, "table": table})[0]["engine"].lower()
+            in run_query(QUERY, {"database": database, "table": table})[0][
+                "engine"
+            ].lower()
     )
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+475 -470 lines across 68 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/cli/export.py#L134'>rasa/cli/export.py~L134</a>
```diff
 
     if (
         min_timestamp is not None
-        and max_timestamp is not None
-        and max_timestamp < min_timestamp
+            and max_timestamp is not None
+            and max_timestamp < min_timestamp
     ):
         rasa.shared.utils.cli.print_error_and_exit(
             f"Maximum timestamp '{max_timestamp}' is smaller than minimum "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L184'>rasa/core/actions/action.py~L184</a>
```diff
 
     if (
         action_name_or_text in defaults
-        and action_name_or_text not in domain.user_actions_and_forms
+            and action_name_or_text not in domain.user_actions_and_forms
     ):
         return defaults[action_name_or_text]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L428'>rasa/core/actions/action.py~L428</a>
```diff
 
         if (
             self.intent_name_from_action(self.action_name)
-            in response_selector_properties
+                in response_selector_properties
         ):
             query_key = self.intent_name_from_action(self.action_name)
         elif RESPONSE_SELECTOR_DEFAULT_INTENT in response_selector_properties:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L462'>rasa/core/actions/action.py~L462</a>
```diff
 
         if (
             self.intent_name_from_action(self.action_name)
-            in response_selector_properties
+                in response_selector_properties
         ):
             query_key = self.intent_name_from_action(self.action_name)
         elif RESPONSE_SELECTOR_DEFAULT_INTENT in response_selector_properties:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L680'>rasa/core/actions/action.py~L680</a>
```diff
 
         if (
             not self._is_selective_domain_enabled()
-            or domain.does_custom_action_explicitly_need_domain(self.name())
+                or domain.does_custom_action_explicitly_need_domain(self.name())
         ):
             result["domain"] = domain.as_dict()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L1022'>rasa/core/actions/action.py~L1022</a>
```diff
         )
         if (
             intent_to_affirm == DEFAULT_NLU_FALLBACK_INTENT_NAME
-            and len(intent_ranking) > 1
+                and len(intent_ranking) > 1
         ):
             intent_to_affirm = intent_ranking[1][INTENT_NAME_KEY]  # type: ignore[literal-required] # noqa: E501
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L1101'>rasa/core/actions/action.py~L1101</a>
```diff
 
         if (
             tracker.is_active_loop_rejected
-            and tracker.get_slot(REQUESTED_SLOT) == slot_name
+                and tracker.get_slot(REQUESTED_SLOT) == slot_name
         ):
             return False
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L1364'>rasa/core/actions/action.py~L1364</a>
```diff
 
     should_fill_entity_slot = (
         mapping_type == SlotMappingType.FROM_ENTITY
-        and SlotMapping.entity_is_desired(mapping, tracker)
+            and SlotMapping.entity_is_desired(mapping, tracker)
     )
 
     should_fill_intent_slot = mapping_type == SlotMappingType.FROM_INTENT
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/action.py#L1382'>rasa/core/actions/action.py~L1382</a>
```diff
         trigger_mapping_condition_met = False
     elif (
         active_loops_in_mapping_conditions
-        and tracker.active_loop_name is not None
-        and (tracker.active_loop_name not in active_loops_in_mapping_conditions)
+            and tracker.active_loop_name is not None
+            and (tracker.active_loop_name not in active_loops_in_mapping_conditions)
     ):
         trigger_mapping_condition_met = False
 
     should_fill_trigger_slot = (
         mapping_type == SlotMappingType.FROM_TRIGGER_INTENT
-        and trigger_mapping_condition_met
+            and trigger_mapping_condition_met
     )
 
     value: List[Any] = []
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/forms.py#L114'>rasa/core/actions/forms.py~L114</a>
```diff
         for requested_slot_mapping in requested_slot_mappings:
             if (
                 not isinstance(requested_slot_mapping, dict)
-                or requested_slot_mapping.get("type") is None
+                    or requested_slot_mapping.get("type") is None
             ):
                 raise TypeError("Provided incompatible slot mapping")
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/forms.py#L290'>rasa/core/actions/forms.py~L290</a>
```diff
             current_tracker.sender_id,
             current_tracker.events_after_latest_restart()
             # Insert SlotSet event to make sure REQUESTED_SLOT belongs to active form.
-            + [SlotSet(REQUESTED_SLOT, self.get_slot_to_fill(current_tracker))]
-            # Insert form execution event so that it's clearly distinguishable which
-            # events were newly added.
-            + [ActionExecuted(self.name())]
-            + additional_events,
+                + [SlotSet(REQUESTED_SLOT, self.get_slot_to_fill(current_tracker))]
+                # Insert form execution event so that it's clearly distinguishable which
+                # events were newly added.
+                + [ActionExecuted(self.name())]
+                + additional_events,
             slots=domain.slots,
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/forms.py#L413'>rasa/core/actions/forms.py~L413</a>
```diff
 
         if (
             slot_to_fill
-            and not extracted_slot_values
-            and not some_slots_were_validated
-            and not self._user_rejected_manually(validation_events)
+                and not extracted_slot_values
+                and not some_slots_were_validated
+                and not self._user_rejected_manually(validation_events)
         ):
             # reject to execute the form action
             # if some slot was requested but nothing was extracted
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/actions/forms.py#L540'>rasa/core/actions/forms.py~L540</a>
```diff
         # no active_loop means that it is called during activation
         needs_validation = not tracker.active_loop or (
             tracker.latest_action_name == ACTION_LISTEN_NAME
-            and not tracker.is_active_loop_interrupted
+                and not tracker.is_active_loop_interrupted
         )
 
         if needs_validation:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L45'>rasa/core/channels/facebook.py~L45</a>
```diff
         """Check if the users message is a recorded voice message."""
         return (
             "message" in message
-            and "attachments" in message["message"]
-            and message["message"]["attachments"][0]["type"] == "audio"
+                and "attachments" in message["message"]
+                and message["message"]["attachments"][0]["type"] == "audio"
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L54'>rasa/core/channels/facebook.py~L54</a>
```diff
         """Check if the users message is an image."""
         return (
             "message" in message
-            and "attachments" in message["message"]
-            and message["message"]["attachments"][0]["type"] == "image"
+                and "attachments" in message["message"]
+                and message["message"]["attachments"][0]["type"] == "image"
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L63'>rasa/core/channels/facebook.py~L63</a>
```diff
         """Check if the users message is a video."""
         return (
             "message" in message
-            and "attachments" in message["message"]
-            and message["message"]["attachments"][0]["type"] == "video"
+                and "attachments" in message["message"]
+                and message["message"]["attachments"][0]["type"] == "video"
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L72'>rasa/core/channels/facebook.py~L72</a>
```diff
         """Check if the users message is a file."""
         return (
             "message" in message
-            and "attachments" in message["message"]
-            and message["message"]["attachments"][0]["type"] == "file"
+                and "attachments" in message["message"]
+                and message["message"]["attachments"][0]["type"] == "file"
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L81'>rasa/core/channels/facebook.py~L81</a>
```diff
         """Check if the message is a message from the user."""
         return (
             "message" in message
-            and "text" in message["message"]
-            and not message["message"].get("is_echo")
+                and "text" in message["message"]
+                and not message["message"].get("is_echo")
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/facebook.py#L90'>rasa/core/channels/facebook.py~L90</a>
```diff
         """Check if the message is a quick reply message."""
         return (
             message.get("message") is not None
-            and message["message"].get("quick_reply") is not None
-            and message["message"]["quick_reply"].get("payload")
+                and message["message"].get("quick_reply") is not None
+                and message["message"]["quick_reply"].get("payload")
         )
 
     async def handle(self, payload: Dict, metadata: Optional[Dict[Text, Any]]) -> None:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/hangouts.py#L247'>rasa/core/channels/hangouts.py~L247</a>
```diff
 
         elif (
             req.json["type"] == "REMOVED_FROM_SPACE"
-            and self.hangouts_user_added_intent_name
+                and self.hangouts_user_added_intent_name
         ):
             message = self.hangouts_user_added_intent_name
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/evaluation/marker_base.py#L342'>rasa/core/evaluation/marker_base.py~L342</a>
```diff
             idx
             for idx, event in enumerate(events)
             if isinstance(event, ActionExecuted)
-            and event.action_name
-            == rasa.shared.core.constants.ACTION_SESSION_START_NAME
+                and event.action_name
+                    == rasa.shared.core.constants.ACTION_SESSION_START_NAME
         ]
         if len(session_start_indices) == 0:
             return [(events, 0)]
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/single_state_featurizer.py#L302'>rasa/core/featurizers/single_state_featurizer.py~L302</a>
```diff
         #  distinguish between entities and roles/groups right now.
         if (
             not entity_data
-            or not self.entity_tag_specs
-            or self.entity_tag_specs[0].num_tags < 2
+                or not self.entity_tag_specs
+                or self.entity_tag_specs[0].num_tags < 2
         ):
             # we cannot build a classifier with fewer than 2 classes
             return {}
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/tracker_featurizers.py#L504'>rasa/core/featurizers/tracker_featurizers.py~L504</a>
```diff
             for event in events
             if (
                 not isinstance(event, ActionExecuted)
-                or event.action_name != ACTION_UNLIKELY_INTENT_NAME
+                    or event.action_name != ACTION_UNLIKELY_INTENT_NAME
             )
         ]
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/tracker_featurizers.py#L1007'>rasa/core/featurizers/tracker_featurizers.py~L1007</a>
```diff
                 # Only add unique example states unless `remove_duplicates` is `False`.
                 if (
                     not self.remove_duplicates
-                    or state_hash not in state_hash_to_label_set
+                        or state_hash not in state_hash_to_label_set
                 ):
                     example_states.append(states)
                     example_entities.append(entities)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/lock_store.py#L306'>rasa/core/lock_store.py~L306</a>
```diff
     """Given an endpoint configuration, create a proper `LockStore` object."""
     if (
         endpoint_config is None
-        or endpoint_config.type is None
-        or endpoint_config.type == "in_memory"
+            or endpoint_config.type is None
+            or endpoint_config.type == "in_memory"
     ):
         # this is the default type if no lock store type is set
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/migrate.py#L350'>rasa/core/migrate.py~L350</a>
```diff
 
         if (
             file_dict.get(KEY_TRAINING_DATA_FORMAT_VERSION)
-            == LATEST_TRAINING_DATA_FORMAT_VERSION
+                == LATEST_TRAINING_DATA_FORMAT_VERSION
         ):
             migrated_files.append(file)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/policy.py#L168'>rasa/core/policies/policy.py~L168</a>
```diff
         featurizer = featurizer_func(**featurizer_config)
         if (
             isinstance(featurizer, MaxHistoryTrackerFeaturizer)
-            and POLICY_MAX_HISTORY in policy_config
-            and POLICY_MAX_HISTORY not in featurizer_config
+                and POLICY_MAX_HISTORY in policy_config
+                and POLICY_MAX_HISTORY not in featurizer_config
         ):
             featurizer.max_history = policy_config[POLICY_MAX_HISTORY]
         return featurizer
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/policy.py#L244'>rasa/core/policies/policy.py~L244</a>
```diff
             precomputations=precomputations,
             bilou_tagging=bilou_tagging,
             ignore_action_unlikely_intent=self.supported_data()
-            == SupportedData.ML_DATA,
+                == SupportedData.ML_DATA,
         )
 
         max_training_samples = kwargs.get("max_training_samples")
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/policy.py#L287'>rasa/core/policies/policy.py~L287</a>
```diff
             ignore_rule_only_turns=self.supported_data() == SupportedData.ML_DATA,
             rule_only_data=rule_only_data,
             ignore_action_unlikely_intent=self.supported_data()
-            == SupportedData.ML_DATA,
+                == SupportedData.ML_DATA,
         )[0]
 
     def _featurize_for_prediction(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/policy.py#L327'>rasa/core/policies/policy.py~L327</a>
```diff
             ignore_rule_only_turns=self.supported_data() == SupportedData.ML_DATA,
             rule_only_data=rule_only_data,
             ignore_action_unlikely_intent=self.supported_data()
-            == SupportedData.ML_DATA,
+                == SupportedData.ML_DATA,
         )
 
     @abc.abstractmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/policy.py#L582'>rasa/core/policies/policy.py~L582</a>
```diff
 
         return (
             self.probabilities == other.probabilities
-            and self.policy_name == other.policy_name
-            and self.policy_priority == other.policy_priority
-            and self.events == other.events
-            and self.optional_events == other.optional_events
-            and self.is_end_to_end_prediction == other.is_end_to_end_prediction
-            and self.is_no_user_prediction == other.is_no_user_prediction
-            and self.hide_rule_turn == other.hide_rule_turn
-            and self.action_metadata == other.action_metadata
+                and self.policy_name == other.policy_name
+                and self.policy_priority == other.policy_priority
+                and self.events == other.events
+                and self.optional_events == other.optional_events
+                and self.is_end_to_end_prediction == other.is_end_to_end_prediction
+                and self.is_no_user_prediction == other.is_no_user_prediction
+                and self.hide_rule_turn == other.hide_rule_turn
+                and self.action_metadata == other.action_metadata
             # We do not compare `diagnostic_data`, because it has no effect on the
             # action prediction.
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L180'>rasa/core/policies/rule_policy.py~L180</a>
```diff
         fallback_action_name = config.get("core_fallback_action_name", None)
         if (
             fallback_action_name
-            and fallback_action_name not in domain.action_names_or_texts
+                and fallback_action_name not in domain.action_names_or_texts
         ):
             raise InvalidDomain(
                 f"The fallback action '{fallback_action_name}' which was "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L382'>rasa/core/policies/rule_policy.py~L382</a>
```diff
                 fingerprint = rule_fingerprints.get(previous_action_name)
                 if (
                     not previous_action_name
-                    or not fingerprint
-                    or action_name == RULE_SNIPPET_ACTION_NAME
-                    or previous_action_name == RULE_SNIPPET_ACTION_NAME
+                        or not fingerprint
+                        or action_name == RULE_SNIPPET_ACTION_NAME
+                        or previous_action_name == RULE_SNIPPET_ACTION_NAME
                 ):
                     # do not check fingerprints for rule snippet action
                     # and don't raise if fingerprints are not satisfied
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L462'>rasa/core/policies/rule_policy.py~L462</a>
```diff
         predicted_action_name = None
         if (
             probabilities != self._default_predictions(domain)
-            or tracker.is_rule_tracker
+                or tracker.is_rule_tracker
         ):
             predicted_action_name = domain.action_names_or_texts[
                 np.argmax(probabilities)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L481'>rasa/core/policies/rule_policy.py~L481</a>
```diff
         # but inside loop unhappy path there might be another action
         if (
             tracker.active_loop_name
-            and predicted_action_name != gold_action_name
-            and predicted_action_name == tracker.active_loop_name
+                and predicted_action_name != gold_action_name
+                and predicted_action_name == tracker.active_loop_name
         ):
             rasa.core.test.emulate_loop_rejection(tracker)
             predicted_action_name, prediction_source = self._predict_next_action(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L504'>rasa/core/policies/rule_policy.py~L504</a>
```diff
 
         if prediction_source is not None and (
             prediction_source.startswith(DEFAULT_RULES)
-            or prediction_source.startswith(LOOP_RULES)
+                or prediction_source.startswith(LOOP_RULES)
         ):
             # the real gold action contradict the one in the rules in this case
             gold_action_name = predicted_action_name
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L548'>rasa/core/policies/rule_policy.py~L548</a>
```diff
             # only apply to contradicting story, not rule
             tracker.is_rule_tracker
             # only apply for prediction after unpredictable action
-            or prediction_source.count(PREVIOUS_ACTION) > 1
-            # only apply for prediction of action_listen
-            or predicted_action_name != ACTION_LISTEN_NAME
+                or prediction_source.count(PREVIOUS_ACTION) > 1
+                # only apply for prediction of action_listen
+                or predicted_action_name != ACTION_LISTEN_NAME
         ):
             return False
         for source in self.lookup[RULES]:
             # remove rule only if another action is predicted after action_listen
             if (
                 source.startswith(prediction_source[:-2])
-                and not prediction_source == source
+                    and not prediction_source == source
             ):
                 return True
         return False
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L574'>rasa/core/policies/rule_policy.py~L574</a>
```diff
         # by better typing in this class, but requires some refactoring
         if (
             not predicted_action_name
-            or not prediction_source
-            or predicted_action_name == gold_action_name
+                or not prediction_source
+                or predicted_action_name == gold_action_name
         ):
             return []
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L661'>rasa/core/policies/rule_policy.py~L661</a>
```diff
                     # we need to know which rules were used in ML trackers
                     if (
                         not tracker.is_rule_tracker
-                        and predicted_action_name == gold_action_name
+                            and predicted_action_name == gold_action_name
                     ):
                         rules_used_in_stories.add(prediction_source)
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L732'>rasa/core/policies/rule_policy.py~L732</a>
```diff
         logger.debug("Found no contradicting rules.")
         all_rules = (
             set(self._rules_sources.keys())
-            | self._default_sources()
-            | self._handling_loop_sources(domain)
+                | self._default_sources()
+                | self._handling_loop_sources(domain)
         )
         # set is not json serializable, so convert to list
         return list(all_rules - rules_used_in_stories)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L841'>rasa/core/policies/rule_policy.py~L841</a>
```diff
                     # value should be set, therefore
                     # check whether it is the same as in the state
                     value_from_rules
-                    and value_from_rules != SHOULD_NOT_BE_SET
-                    and value_from_conversation != value_from_rules
+                        and value_from_rules != SHOULD_NOT_BE_SET
+                        and value_from_conversation != value_from_rules
                 ) or (
                     # value shouldn't be set, therefore
                     # it should be None or non existent in the state
                     value_from_rules == SHOULD_NOT_BE_SET
-                    and value_from_conversation
-                    # during training `SHOULD_NOT_BE_SET` is provided. Hence, we also
-                    # have to check for the value of the slot state
-                    and value_from_conversation != SHOULD_NOT_BE_SET
+                        and value_from_conversation
+                        # during training `SHOULD_NOT_BE_SET` is provided. Hence, we also
+                        # have to check for the value of the slot state
+                        and value_from_conversation != SHOULD_NOT_BE_SET
                 ):
                     return False
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L927'>rasa/core/policies/rule_policy.py~L927</a>
```diff
     ) -> Tuple[Optional[Text], Optional[Text]]:
         if (
             not tracker.latest_action_name == ACTION_LISTEN_NAME
-            or not tracker.latest_message
+                or not tracker.latest_message
         ):
             return None, None
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L958'>rasa/core/policies/rule_policy.py~L958</a>
```diff
         active_loop_rejected = tracker.is_active_loop_rejected
         should_predict_loop = (
             not active_loop_rejected
-            and tracker.latest_action
-            and tracker.latest_action.get(ACTION_NAME) != active_loop_name
+                and tracker.latest_action
+                and tracker.latest_action.get(ACTION_NAME) != active_loop_name
         )
         should_predict_listen = (
             not active_loop_rejected and tracker.latest_action_name == active_loop_name
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L1005'>rasa/core/policies/rule_policy.py~L1005</a>
```diff
         """
         if (
             use_text_for_last_user_input
-            and not tracker.latest_action_name == ACTION_LISTEN_NAME
+                and not tracker.latest_action_name == ACTION_LISTEN_NAME
         ):
             # make text prediction only directly after user utterance
             # because we've otherwise already decided whether to use
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L1058'>rasa/core/policies/rule_policy.py~L1058</a>
```diff
             # Hence, we have to take care of that.
             predicted_listen_from_general_rule = (
                 predicted_action_name == ACTION_LISTEN_NAME
-                and not get_active_loop_name(self._rule_key_to_state(best_rule_key)[-1])
+                    and not get_active_loop_name(
+                        self._rule_key_to_state(best_rule_key)[-1]
+                    )
             )
             if predicted_listen_from_general_rule:
                 if DO_NOT_PREDICT_LOOP_ACTION not in unhappy_path_conditions:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/rule_policy.py#L1199'>rasa/core/policies/rule_policy.py~L1199</a>
```diff
                     # returning_from_unhappy_path is a negative condition,
                     # so `or` should be applied
                     returning_from_unhappy_path_from_text
-                    or returning_from_unhappy_path_from_intent
+                        or returning_from_unhappy_path_from_intent
                 ),
                 is_end_to_end_prediction=False,
             ),
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L760'>rasa/core/policies/ted_policy.py~L760</a>
```diff
         # the second - text, but only after user utterance and if not only e2e
         if (
             tracker.latest_action_name == ACTION_LISTEN_NAME
-            and TEXT in self.fake_features
-            and not self.only_e2e
+                and TEXT in self.fake_features
+                and not self.only_e2e
         ):
             tracker_state_features += self._featurize_for_prediction(
                 tracker,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L796'>rasa/core/policies/ted_policy.py~L796</a>
```diff
             logger.debug(f"User text lead to '{e2e_action_name}'.")
             if (
                 np.max(confidences[1]) > self.config[E2E_CONFIDENCE_THRESHOLD]
-                # TODO maybe compare confidences is better
-                and np.max(similarities[1]) > np.max(similarities[0])
+                    # TODO maybe compare confidences is better
+                    and np.max(similarities[1]) > np.max(similarities[0])
             ):
                 logger.debug(f"TED predicted '{e2e_action_name}' based on user text.")
                 return confidences[1], True
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L854'>rasa/core/policies/ted_policy.py~L854</a>
```diff
         if 0 < ranking_length < len(confidence):
             renormalize = (
                 self.config[RENORMALIZE_CONFIDENCES]
-                and self.config[MODEL_CONFIDENCE] == SOFTMAX
+                    and self.config[MODEL_CONFIDENCE] == SOFTMAX
             )
             _, confidence = train_utils.rank_and_mask(
                 confidence, ranking_length=ranking_length, renormalize=renormalize
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1339'>rasa/core/policies/ted_policy.py~L1339</a>
```diff
         # check that there are SENTENCE features for the attribute name in data
         if (
             name in SENTENCE_FEATURES_TO_ENCODE
-            and FEATURE_TYPE_SENTENCE not in self.data_signature[name]
+                and FEATURE_TYPE_SENTENCE not in self.data_signature[name]
         ):
             return
         #  same for label_data
         if (
             name in LABEL_FEATURES_TO_ENCODE
-            and FEATURE_TYPE_SENTENCE not in self.label_signature[name]
+                and FEATURE_TYPE_SENTENCE not in self.label_signature[name]
         ):
             return
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1797'>rasa/core/policies/ted_policy.py~L1797</a>
```diff
 
         if (
             batch_encoded.get(ACTION_TEXT) is not None
-            and batch_encoded.get(ACTION_NAME) is not None
+                and batch_encoded.get(ACTION_NAME) is not None
         ):
             batch_action = batch_encoded.pop(ACTION_TEXT) + batch_encoded.pop(
                 ACTION_NAME
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L1809'>rasa/core/policies/ted_policy.py~L1809</a>
```diff
         # same for user input
         if (
             batch_encoded.get(INTENT) is not None
-            and batch_encoded.get(TEXT) is not None
+                and batch_encoded.get(TEXT) is not None
         ):
             batch_user = batch_encoded.pop(INTENT) + batch_encoded.pop(TEXT)
         elif batch_encoded.get(TEXT) is not None:
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L2009'>rasa/core/policies/ted_policy.py~L2009</a>
```diff
 
         if (
             self.config[ENTITY_RECOGNITION]
-            and text_output is not None
-            and text_sequence_lengths is not None
+                and text_output is not None
+                and text_sequence_lengths is not None
         ):
             losses.append(
                 self._batch_loss_entities(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/ted_policy.py#L2080'>rasa/core/policies/ted_policy.py~L2080</a>
```diff
 
         if (
             self.config[ENTITY_RECOGNITION]
-            and text_output is not None
-            and text_sequence_lengths is not None
+                and text_output is not None
+                and text_sequence_lengths is not None
         ):
             pred_ids, confidences = self._batch_predict_entities(
                 tf_batch_data,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/policies/unexpected_intent_policy.py#L756'>rasa/core/policies/unexpected_intent_policy.py~L756</a>
```diff
         # the query intent is not the top likely intent
         if (
             query_intent_similarity < self.label_thresholds[query_intent_id]
-            and query_intent_id != highest_likely_intent_id
+                and query_intent_id != highest_likely_intent_id
         ):
             logger.debug(
                 f"Intent `{query_intent}-{query_intent_id}` unlikely to occur here."
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L585'>rasa/core/processor.py~L585</a>
```diff
 
             if (
                 reminder_event.kill_on_user_message
-                and self._has_message_after_reminder(tracker, reminder_event)
-                or not self._is_reminder_still_valid(tracker, reminder_event)
+                    and self._has_message_after_reminder(tracker, reminder_event)
+                    or not self._is_reminder_still_valid(tracker, reminder_event)
             ):
                 logger.debug(
                     f"Canceled reminder because it is outdated ({reminder_event})."
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L816'>rasa/core/processor.py~L816</a>
```diff
     def _should_handle_message(tracker: DialogueStateTracker) -> bool:
         return not tracker.is_paused() or (
             tracker.latest_message is not None
-            and tracker.latest_message.intent.get(INTENT_NAME_KEY)
-            == USER_INTENT_RESTART
+                and tracker.latest_message.intent.get(INTENT_NAME_KEY)
+                    == USER_INTENT_RESTART
         )
 
     def is_action_limit_reached(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L844'>rasa/core/processor.py~L844</a>
```diff
 
         return (
             num_predicted_actions >= self.max_number_of_predictions
-            and should_predict_another_action
+                and should_predict_another_action
         )
 
     async def _run_prediction_loop(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L1081'>rasa/core/processor.py~L1081</a>
```diff
         time_delta_in_seconds = time.time() - user_uttered_event.timestamp
         has_expired = (
             time_delta_in_seconds / 60
-            > self.domain.session_config.session_expiration_time
+                > self.domain.session_config.session_expiration_time
         )
         if has_expired:
             logger.debug(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L236'>rasa/core/test.py~L236</a>
```diff
         """Checks if intent, entity or action predictions don't match expected ones."""
         return (
             self.intent_predictions != self.intent_targets
-            or self._check_entity_prediction_target_mismatch()
-            or self.action_predictions != self.action_targets
+                or self._check_entity_prediction_target_mismatch()
+                or self.action_predictions != self.action_targets
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L285'>rasa/core/test.py~L285</a>
```diff
         texts = sorted(
             set(
                 [str(e.get("text", "")) for e in self.entity_targets]
-                + [str(e.get("text", "")) for e in self.entity_predictions]
+                    + [str(e.get("text", "")) for e in self.entity_predictions]
             )
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L340'>rasa/core/test.py~L340</a>
```diff
 
         predictions = (
             self.action_predictions
-            + self.intent_predictions
-            + aligned_entity_predictions
+                + self.intent_predictions
+                + aligned_entity_predictions
         )
         return targets, predictions
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L648'>rasa/core/test.py~L648</a>
```diff
     """
     if (
         isinstance(predicted_action, ActionRetrieveResponse)
-        and expected_action_name != predicted_action.name()
+            and expected_action_name != predicted_action.name()
     ):
         full_retrieval_name = predicted_action.get_full_retrieval_name(partial_tracker)
         predicted_action_name = (
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L674'>rasa/core/test.py~L674</a>
```diff
     )
     if (
         prediction.policy_name
-        and predicted_action != expected_action
-        and _form_might_have_been_rejected(
-            processor.domain, partial_tracker, predicted_action
-        )
+            and predicted_action != expected_action
+            and _form_might_have_been_rejected(
+                processor.domain, partial_tracker, predicted_action
+            )
     ):
         # Wrong action was predicted,
         # but it might be Ok if form action is rejected.
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L767'>rasa/core/test.py~L767</a>
```diff
         )
         if (
             fail_on_prediction_errors
-            and predicted_action != ACTION_UNLIKELY_INTENT_NAME
-            and predicted_action != expected_action
+                and predicted_action != ACTION_UNLIKELY_INTENT_NAME
+                and predicted_action != expected_action
         ):
             story_dump = YAMLStoryWriter().dumps(partial_tracker.as_story().story_steps)
             error_msg = (
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L805'>rasa/core/test.py~L805</a>
```diff
 ) -> bool:
     return (
         tracker.active_loop_name == predicted_action_name
-        and predicted_action_name in domain.form_names
+            and predicted_action_name in domain.form_names
     )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L898'>rasa/core/test.py~L898</a>
```diff
         a["action"]
         for a in action_list
         if a["policy"]
-        and not rasa.core.policies.ensemble.is_not_in_training_data(a["policy"])
+            and not rasa.core.policies.ensemble.is_not_in_training_data(a["policy"])
     ]
 
     return len(in_training_data) / len(action_list) if action_list else 0
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L924'>rasa/core/test.py~L924</a>
```diff
         for event in tracker.applied_events():
             if (
                 isinstance(event, WronglyPredictedAction)
-                and event.action_name_prediction == ACTION_UNLIKELY_INTENT_NAME
+                    and event.action_name_prediction == ACTION_UNLIKELY_INTENT_NAME
             ):
                 max_severity = max(
                     max_severity,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L990'>rasa/core/test.py~L990</a>
```diff
 
             if any(
                 isinstance(event, WronglyPredictedAction)
-                and event.action_name_prediction == ACTION_UNLIKELY_INTENT_NAME
+                    and event.action_name_prediction == ACTION_UNLIKELY_INTENT_NAME
                 for event in predicted_tracker.events
             ):
                 stories_with_warnings.append(predicted_tracker)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/test.py#L1027'>rasa/core/test.py~L1027</a>
```diff
     for event in step.events:
         if (
             isinstance(event, WronglyPredictedAction)
-            and event.action_name
-            == event.action_name_prediction
-            == ACTION_UNLIKELY_INTENT_NAME
+                and event.action_name
+                    == event.action_name_prediction
+                    == ACTION_UNLIKELY_INTENT_NAME
         ):
             continue
         events.append(event)
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L555'>rasa/core/training/interactive.py~L555</a>
```diff
         if isinstance(event, ActionExecuted):
             if (
                 event.action_name == ACTION_UNLIKELY_INTENT_NAME
-                and event.confidence == 0
+                    and event.confidence == 0
             ):
                 continue
             bot_column.append(colored(str(event), "autocyan"))
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L842'>rasa/core/training/interactive.py~L842</a>
```diff
                 interactive_story_counter += 1
                 f.write(
                     "\n"
-                    + tracker.export_stories(
-                        writer=writer,
-                        should_append_stories=should_append_stories,
-                        e2e=SAVE_IN_E2E,
-                    )
+                        + tracker.export_stories(
+                            writer=writer,
+                            should_append_stories=should_append_stories,
+                            e2e=SAVE_IN_E2E,
+                        )
                 )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L928'>rasa/core/training/interactive.py~L928</a>
```diff
             e["name"]
             for e in actions
             if e["name"] not in rasa.shared.core.constants.DEFAULT_ACTION_NAMES
-            and e["name"] not in old_domain.form_names
+                and e["name"] not in old_domain.form_names
         }
     )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L1055'>rasa/core/training/interactive.py~L1055</a>
```diff
     """Check if the form got rejected with the most recent action name."""
     return (
         tracker.get(ACTIVE_LOOP, {}).get(LOOP_NAME)
-        and action_name != tracker[ACTIVE_LOOP][LOOP_NAME]
-        and action_name != ACTION_LISTEN_NAME
+            and action_name != tracker[ACTIVE_LOOP][LOOP_NAME]
+            and action_name != ACTION_LISTEN_NAME
     )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L1064'>rasa/core/training/interactive.py~L1064</a>
```diff
     """Check whether the form is called again after it was rejected."""
     return (
         tracker.get(ACTIVE_LOOP, {}).get(LOOP_REJECTED)
-        and tracker.get("latest_action_name") == ACTION_LISTEN_NAME
-        and action_name == tracker.get(ACTIVE_LOOP, {}).get(LOOP_NAME)
+            and tracker.get("latest_action_name") == ACTION_LISTEN_NAME
+            and action_name == tracker.get(ACTIVE_LOOP, {}).get(LOOP_NAME)
     )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L1326'>rasa/core/training/interactive.py~L1326</a>
```diff
 def _is_same_entity_annotation(entity: Dict[Text, Any], other: Dict[Text, Any]) -> bool:
     return (
         entity["value"] == other["value"]
-        and entity["entity"] == other["entity"]
-        and entity.get("group") == other.get("group")
-        and entity.get("role") == other.get("group")
+            and entity["entity"] == other["entity"]
+            and entity.get("group") == other.get("group")
+            and entity.get("role") == other.get("group")
     )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/training/interactive.py#L1731'>rasa/core/training/interactive.py~L1731</a>
```diff
         if all(
             [
                 terminaltables.width_and_alignment.visible_width(line)
-                <= monospace_wrapping_width
+                    <= monospace_wrapping_width
                 for line in lines
             ]
         ):
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/utils.py#L293'>rasa/core/utils.py~L293</a>
```diff
     # `lock_store` is `None` or `EndpointConfig`
     return (
         lock_store is not None
-        and isinstance(lock_store, EndpointConfig)
-        and lock_store.type != "in_memory"
+            and isinstance(lock_store, EndpointConfig)
+            and lock_store.type != "in_memory"
     )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/caching.py#L234'>rasa/engine/caching.py~L234</a>
```diff
             entry
             for entry in all_entries
             if version.parse(MINIMUM_COMPATIBLE_VERSION)
-            > version.parse(entry.rasa_version)
+                > version.parse(entry.rasa_version)
         ]
 
     def _delete_incompatible_entries_from_cache(
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/caching.py#L337'>rasa/engine/caching.py~L337</a>
```diff
                     self._cache_location,
                     filenames_to_exclude=[self._cache_database_name],
                 )
-                + output_size
-                > self._max_cache_size
+                    + output_size
+                    > self._max_cache_size
             ):
                 self._drop_least_recently_used_item()
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L207'>rasa/engine/recipes/default_recipe.py~L207</a>
```diff
 
         self._use_end_to_end = (
             self._use_nlu
-            and self._use_core
-            and training_type == TrainingType.END_TO_END
+                and self._use_core
+                and training_type == TrainingType.END_TO_END
         )
 
         self._is_finetuning = is_finetuning
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L312'>rasa/engine/recipes/default_recipe.py~L312</a>
```diff
 
             if (
                 self.ComponentType.POLICY_WITHOUT_END_TO_END_SUPPORT in component.types
-                or self.ComponentType.POLICY_WITH_END_TO_END_SUPPORT in component.types
+                    or self.ComponentType.POLICY_WITH_END_TO_END_SUPPORT
+                        in component.types
             ):
                 raise InvalidConfigException(
                     f"Found policy '{component_name}' in NLU pipeline. Policies should "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L464'>rasa/engine/recipes/default_recipe.py~L464</a>
```diff
 
         if (
             self._is_finetuning
-            and "finetuning_epoch_fraction" in cli_parameters
-            and EPOCHS in component.get_default_config()
+                and "finetuning_epoch_fraction" in cli_parameters
+                and EPOCHS in component.get_default_config()
         ):
             old_number_epochs = component_config.get(
                 EPOCHS, component.get_default_config()[EPOCHS]
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L866'>rasa/engine/recipes/default_recipe.py~L866</a>
```diff
             )
             if (
                 self.ComponentType.POLICY_WITH_END_TO_END_SUPPORT in component.types
-                and node_with_e2e_features
+                    and node_with_e2e_features
             ):
                 needs["precomputations"] = node_with_e2e_features
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/recipes/default_recipe.py#L1105'>rasa/engine/recipes/default_recipe.py~L1105</a>
```diff
 
         return (
             bool(content)
-            and missing_keys
-            == DefaultV1Recipe._get_missing_config_keys(content, training_type)
-            and auto_configured_keys
-            == DefaultV1Recipe._get_unspecified_autoconfigurable_keys(
-                content, training_type
-            )
+                and missing_keys
+                    == DefaultV1Recipe._get_missing_config_keys(content, training_type)
+                and auto_configured_keys
+                    == DefaultV1Recipe._get_unspecified_autoconfigurable_keys(
+                        content, training_type
+                    )
         )
 
     @staticmethod
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/validation.py#L211'>rasa/engine/validation.py~L211</a>
```diff
 
     if (
         language
-        and supported_languages is not None
-        and language not in supported_languages
+            and supported_languages is not None
+            and language not in supported_languages
     ):
         raise GraphSchemaValidationException(
             f"The component '{node.uses.__name__}' does not support the currently "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/validation.py#L221'>rasa/engine/validation.py~L221</a>
```diff
 
     if (
         language
-        and not_supported_languages is not None
-        and language in not_supported_languages
+            and not_supported_languages is not None
+            and language in not_supported_languages
     ):
         raise GraphSchemaValidationException(
             f"The component '{node.uses.__name__}' does not support the currently "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/validation.py#L263'>rasa/engine/validation.py~L263</a>
```diff
         type_info[param_name] = ParameterInfo(
             type_annotation=type_annotation,
             is_variable_length_keyword_arg=inspect_info.kind
-            == inspect.Parameter.VAR_KEYWORD,
+                == inspect.Parameter.VAR_KEYWORD,
             has_default=inspect_info.default != inspect.Parameter.empty,
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/engine/validation.py#L334'>rasa/engine/validation.py~L334</a>
```diff
         param_name
         for param_name, param in fn_params.items()
         if not param.has_default
-        and not param.is_variable_length_keyword_arg
-        and param_name not in keywords
+            and not param.is_variable_length_keyword_arg
+            and param_name not in keywords
     }
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/graph_components/validators/default_recipe_validator.py#L133'>rasa/graph_components/validators/default_recipe_validator.py~L133</a>
```diff
         """
         if (
             training_data.response_examples
-            and ResponseSelector not in self._component_types
+                and ResponseSelector not in self._component_types
         ):
             rasa.shared.utils.io.raise_warning(
                 f"You have defined training data with examples for training a response "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/graph_components/validators/default_recipe_validator.py#L242'>rasa/graph_components/validators/default_recipe_validator.py~L242</a>
```diff
 
         if (
             training_data.entity_synonyms
-            and EntitySynonymMapper not in self._component_types
+                and EntitySynonymMapper not in self._component_types
         ):
             rasa.shared.utils.io.raise_warning(
                 f"You have defined synonyms in your training data, but "
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/graph_components/validators/default_recipe_validator.py#L357'>rasa/graph_components/validators/default_recipe_validator.py~L357</a>
```diff
             if issubclass(node.uses, Featurizer)
             # Featurizers are split in `train` and `process_training_data` -
             # we only need to look at the node...*[Comment body truncated]*

---

_Closed by @MichaReiser on 2024-04-08 07:03_

---
