```yaml
number: 14148
title: Prefer breaking return-type over parameters
type: pull_request
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
draft: true
base: main
head: micha/parametes-return-ty
created_at: 2024-11-07T07:42:37Z
updated_at: 2024-11-18T06:53:41Z
url: https://github.com/astral-sh/ruff/pull/14148
synced_at: 2026-01-10T20:50:57Z
```

# Prefer breaking return-type over parameters

---

_Pull request opened by @MichaReiser on 2024-11-07 07:42_

## Summary

Related to https://github.com/astral-sh/ruff/pull/13381 and fixes https://github.com/astral-sh/ruff/issues/14143

This needs some work because it introduces new black incompatibilites. 

Ruff

```py
def parameters_subscript_return_type(a) -> list[str]:
    pass


# Unlike with no-parameters, the return type gets never parenthesized.
def parameters_overlong_subscript_return_type_with_single_element(
    a
) -> list[xxxxxxxxxxxxxxxxxxxxx]:
    pass


def parameters_subscript_return_type_multiple_elements(a) -> list[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
]:
    pass


def parameters_subscript_return_type_multiple_overlong_elements(a) -> list[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
]:
    pass


def parameters_subscriptreturn_type_with_overlong_value_(
    a
) -> liiiiiiiiiiiiiiiiiiiiist[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
]:
    pass
```

Black 
```py
def parameters_subscript_return_type(a) -> list[str]:
    pass


# Unlike with no-parameters, the return type gets never parenthesized.
def parameters_overlong_subscript_return_type_with_single_element(
    a,
) -> list[xxxxxxxxxxxxxxxxxxxxx]:
    pass


def parameters_subscript_return_type_multiple_elements(
    a,
) -> list[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx, xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
]:
    pass


def parameters_subscript_return_type_multiple_overlong_elements(
    a,
) -> list[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
]:
    pass


def parameters_subscriptreturn_type_with_overlong_value_(
    a,
) -> liiiiiiiiiiiiiiiiiiiiist[
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx, xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
]:
    pass

```

I overall like Ruff's formatting better but it is a rather significant deviation. But I suspect that any non-deviating fix is probably more invovled.

## Test Plan

TODO


---

_Comment by @github-actions[bot] on 2024-11-07 07:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+3388 -3492 lines in 540 files in 38 projects; 16 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+234 -234 lines across 25 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L84'>disnake/audit_logs.py~L84</a>
```diff
     return int(data)
 
 
-def _transform_channel(
-    entry: AuditLogEntry, data: Optional[Snowflake]
-) -> Optional[Union[abc.GuildChannel, Object]]:
+def _transform_channel(entry: AuditLogEntry, data: Optional[Snowflake]) -> Optional[
+    Union[abc.GuildChannel, Object]
+]:
     if data is None:
         return None
     channel = entry.guild.get_channel(int(data))
     return channel or Object(id=data)
 
 
-def _transform_role(
-    entry: AuditLogEntry, data: Optional[Snowflake]
-) -> Optional[Union[Role, Object]]:
+def _transform_role(entry: AuditLogEntry, data: Optional[Snowflake]) -> Optional[
+    Union[Role, Object]
+]:
     if data is None:
         return None
     role = entry.guild.get_role(int(data))
     return role or Object(id=data)
 
 
-def _transform_member_id(
-    entry: AuditLogEntry, data: Optional[Snowflake]
-) -> Union[Member, User, Object, None]:
+def _transform_member_id(entry: AuditLogEntry, data: Optional[Snowflake]) -> Union[
+    Member, User, Object, None
+]:
     if data is None:
         return None
     return entry._get_member(int(data))
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L116'>disnake/audit_logs.py~L116</a>
```diff
     return entry._state._get_guild(int(data))
 
 
-def _transform_overwrites(
-    entry: AuditLogEntry, data: List[PermissionOverwritePayload]
-) -> List[Tuple[Object, PermissionOverwrite]]:
+def _transform_overwrites(entry: AuditLogEntry, data: List[PermissionOverwritePayload]) -> List[
+    Tuple[Object, PermissionOverwrite]
+]:
     overwrites = []
     for elem in data:
         allow = Permissions(int(elem["allow"]))
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L170'>disnake/audit_logs.py~L170</a>
```diff
     return ForumTag._from_data(data=data, state=entry._state)
 
 
-def _transform_tag_id(
-    entry: AuditLogEntry, data: Optional[str]
-) -> Optional[Union[ForumTag, Object]]:
+def _transform_tag_id(entry: AuditLogEntry, data: Optional[str]) -> Optional[
+    Union[ForumTag, Object]
+]:
     if data is None:
         return None
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L215'>disnake/audit_logs.py~L215</a>
```diff
     return _transform
 
 
-def _list_transformer(
-    func: Callable[[AuditLogEntry, Any], T],
-) -> Callable[[AuditLogEntry, Any], List[T]]:
+def _list_transformer(func: Callable[[AuditLogEntry, Any], T]) -> Callable[
+    [AuditLogEntry, Any], List[T]
+]:
     def _transform(entry: AuditLogEntry, data: Any) -> List[T]:
         if not data:
             return []
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L226'>disnake/audit_logs.py~L226</a>
```diff
     return _transform
 
 
-def _transform_type(
-    entry: AuditLogEntry, data: Any
-) -> Union[enums.ChannelType, enums.StickerType, enums.WebhookType, str, int]:
+def _transform_type(entry: AuditLogEntry, data: Any) -> Union[
+    enums.ChannelType, enums.StickerType, enums.WebhookType, str, int
+]:
     action_name = entry.action.name
     if action_name.startswith("sticker_"):
         return enums.try_enum(enums.StickerType, data)
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L245'>disnake/audit_logs.py~L245</a>
```diff
     return utils.parse_time(data)
 
 
-def _transform_privacy_level(
-    entry: AuditLogEntry, data: Optional[int]
-) -> Optional[Union[enums.StagePrivacyLevel, enums.GuildScheduledEventPrivacyLevel]]:
+def _transform_privacy_level(entry: AuditLogEntry, data: Optional[int]) -> Optional[
+    Union[enums.StagePrivacyLevel, enums.GuildScheduledEventPrivacyLevel]
+]:
     if data is None:
         return None
     if entry.action.target_type == "guild_scheduled_event":
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L255'>disnake/audit_logs.py~L255</a>
```diff
     return enums.try_enum(enums.StagePrivacyLevel, data)
 
 
-def _transform_guild_scheduled_event_image(
-    entry: AuditLogEntry, data: Optional[str]
-) -> Optional[Asset]:
+def _transform_guild_scheduled_event_image(entry: AuditLogEntry, data: Optional[str]) -> Optional[
+    Asset
+]:
     if data is None:
         return None
     return Asset._from_guild_scheduled_event_image(entry._state, entry._target_id, data)  # type: ignore
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L701'>disnake/audit_logs.py~L701</a>
```diff
             return None
         return self.guild.get_member(user_id) or self._users.get(user_id) or Object(id=user_id)
 
-    def _get_channel_or_thread(
-        self, channel_id: Optional[int]
-    ) -> Union[abc.GuildChannel, Thread, Object, None]:
+    def _get_channel_or_thread(self, channel_id: Optional[int]) -> Union[
+        abc.GuildChannel, Thread, Object, None
+    ]:
         if not channel_id:
             return None
         return self.guild.get_channel_or_thread(channel_id) or Object(channel_id)
 
-    def _get_integration_by_application_id(
-        self, application_id: int
-    ) -> Optional[PartialIntegration]:
+    def _get_integration_by_application_id(self, application_id: int) -> Optional[
+        PartialIntegration
+    ]:
         if not application_id:
             return None
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/audit_logs.py#L840'>disnake/audit_logs.py~L840</a>
```diff
             self.guild.get_thread(target_id) or self._threads.get(target_id) or Object(id=target_id)
         )
 
-    def _convert_target_guild_scheduled_event(
-        self, target_id: int
-    ) -> Union[GuildScheduledEvent, Object]:
+    def _convert_target_guild_scheduled_event(self, target_id: int) -> Union[
+        GuildScheduledEvent, Object
+    ]:
         return (
             self.guild.get_scheduled_event(target_id)
             or self._guild_scheduled_events.get(target_id)
             or Object(id=target_id)
         )
 
-    def _convert_target_application_command_or_integration(
-        self, target_id: int
-    ) -> Union[APIApplicationCommand, PartialIntegration, Object]:
+    def _convert_target_application_command_or_integration(self, target_id: int) -> Union[
+        APIApplicationCommand, PartialIntegration, Object
+    ]:
         # try application command
         if target := (
             self._state._get_guild_application_command(self.guild.id, target_id)
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/channel.py#L5013'>disnake/channel.py~L5013</a>
```diff
     return cls, value
 
 
-def _channel_type_factory(
-    cls: Union[Type[disnake.abc.GuildChannel], Type[Thread]],
-) -> List[ChannelType]:
+def _channel_type_factory(cls: Union[Type[disnake.abc.GuildChannel], Type[Thread]]) -> List[
+    ChannelType
+]:
     return {
         disnake.abc.GuildChannel: list(ChannelType.__members__.values()),
         VocalGuildChannel: [ChannelType.voice, ChannelType.stage_voice],
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/client.py#L664'>disnake/client.py~L664</a>
```diff
         return utils.get(self.cached_messages, id=id)
 
     @overload
-    async def get_or_fetch_user(
-        self, user_id: int, *, strict: Literal[False] = ...
-    ) -> Optional[User]: ...
+    async def get_or_fetch_user(self, user_id: int, *, strict: Literal[False] = ...) -> Optional[
+        User
+    ]: ...
 
     @overload
     async def get_or_fetch_user(self, user_id: int, *, strict: Literal[True]) -> User: ...
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/client.py#L2988'>disnake/client.py~L2988</a>
```diff
 
     # Application command permissions
 
-    async def bulk_fetch_command_permissions(
-        self, guild_id: int
-    ) -> List[GuildApplicationCommandPermissions]:
+    async def bulk_fetch_command_permissions(self, guild_id: int) -> List[
+        GuildApplicationCommandPermissions
+    ]:
         """|coro|
 
         Retrieves a list of :class:`.GuildApplicationCommandPermissions` configured for the guild with the given ID.
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/cooldowns.py#L237'>disnake/ext/commands/cooldowns.py~L237</a>
```diff
 
         return bucket
 
-    def update_rate_limit(
-        self, message: Message, current: Optional[float] = None
-    ) -> Optional[float]:
+    def update_rate_limit(self, message: Message, current: Optional[float] = None) -> Optional[
+        float
+    ]:
         bucket = self.get_bucket(message, current)
         return bucket.update_rate_limit(current)
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/core.py#L1477'>disnake/ext/commands/core.py~L1477</a>
```diff
 
     class CommandDecorator(Protocol):
         @overload
-        def __call__(
-            self, func: Callable[Concatenate[ContextT, P], Coro[T]]
-        ) -> Command[None, P, T]: ...
+        def __call__(self, func: Callable[Concatenate[ContextT, P], Coro[T]]) -> Command[
+            None, P, T
+        ]: ...
 
         @overload
-        def __call__(
-            self, func: Callable[Concatenate[CogT, ContextT, P], Coro[T]]
-        ) -> Command[CogT, P, T]: ...
+        def __call__(self, func: Callable[Concatenate[CogT, ContextT, P], Coro[T]]) -> Command[
+            CogT, P, T
+        ]: ...
 
     class GroupDecorator(Protocol):
         @overload
-        def __call__(
-            self, func: Callable[Concatenate[ContextT, P], Coro[T]]
-        ) -> Group[None, P, T]: ...
+        def __call__(self, func: Callable[Concatenate[ContextT, P], Coro[T]]) -> Group[
+            None, P, T
+        ]: ...
 
         @overload
-        def __call__(
-            self, func: Callable[Concatenate[CogT, ContextT, P], Coro[T]]
-        ) -> Group[CogT, P, T]: ...
+        def __call__(self, func: Callable[Concatenate[CogT, ContextT, P], Coro[T]]) -> Group[
+            CogT, P, T
+        ]: ...
 
 
 # Small explanation regarding these overloads:
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/flag_converter.py#L137'>disnake/ext/commands/flag_converter.py~L137</a>
```diff
             raise ValueError(f"flag name {name!r} cannot have any of {forbidden!r} within them")
 
 
-def get_flags(
-    namespace: Dict[str, Any], globals: Dict[str, Any], locals: Dict[str, Any]
-) -> Dict[str, Flag]:
+def get_flags(namespace: Dict[str, Any], globals: Dict[str, Any], locals: Dict[str, Any]) -> Dict[
+    str, Flag
+]:
     annotations = namespace.get("__annotations__", {})
     case_insensitive = namespace["__commands_flag_case_insensitive__"]
     flags: Dict[str, Flag] = {}
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/flag_converter.py#L340'>disnake/ext/commands/flag_converter.py~L340</a>
```diff
         return type.__new__(cls, name, bases, attrs)
 
 
-async def tuple_convert_all(
-    ctx: Context, argument: str, flag: Flag, converter: Any
-) -> Tuple[Any, ...]:
+async def tuple_convert_all(ctx: Context, argument: str, flag: Flag, converter: Any) -> Tuple[
+    Any, ...
+]:
     view = StringView(argument)
     results = []
     param: inspect.Parameter = ctx.current_parameter  # type: ignore
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/flag_converter.py#L367'>disnake/ext/commands/flag_converter.py~L367</a>
```diff
     return tuple(results)
 
 
-async def tuple_convert_flag(
-    ctx: Context, argument: str, flag: Flag, converters: Any
-) -> Tuple[Any, ...]:
+async def tuple_convert_flag(ctx: Context, argument: str, flag: Flag, converters: Any) -> Tuple[
+    Any, ...
+]:
     view = StringView(argument)
     results = []
     param: inspect.Parameter = ctx.current_parameter  # type: ignore
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/interaction_bot_base.py#L407'>disnake/ext/commands/interaction_bot_base.py~L407</a>
```diff
             return None
         return command
 
-    def get_slash_command(
-        self, name: str
-    ) -> Optional[Union[InvokableSlashCommand, SubCommandGroup, SubCommand]]:
+    def get_slash_command(self, name: str) -> Optional[
+        Union[InvokableSlashCommand, SubCommandGroup, SubCommand]
+    ]:
         """Works like ``Bot.get_command``, but for slash commands.
 
         If the name contains spaces, then it will assume that you are looking for a :class:`SubCommand` or
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/interaction_bot_base.py#L731'>disnake/ext/commands/interaction_bot_base.py~L731</a>
```diff
 
     # command synchronisation
 
-    def _ordered_unsynced_commands(
-        self, test_guilds: Optional[Sequence[int]] = None
-    ) -> Tuple[List[ApplicationCommand], Dict[int, List[ApplicationCommand]]]:
+    def _ordered_unsynced_commands(self, test_guilds: Optional[Sequence[int]] = None) -> Tuple[
+        List[ApplicationCommand], Dict[int, List[ApplicationCommand]]
+    ]:
         global_cmds = []
         guilds = {}
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/interaction_bot_base.py#L1174'>disnake/ext/commands/interaction_bot_base.py~L1174</a>
```diff
             user_commands = True
             message_commands = True
 
-        def decorator(
-            func: Callable[[ApplicationCommandInteraction], Any],
-        ) -> Callable[[ApplicationCommandInteraction], Any]:
+        def decorator(func: Callable[[ApplicationCommandInteraction], Any]) -> Callable[
+            [ApplicationCommandInteraction], Any
+        ]:
             # T was used instead of Check to ensure the type matches on return
             self.add_app_command_check(
                 func,
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/commands/params.py#L1318'>disnake/ext/commands/params.py~L1318</a>
```diff
     return decorator
 
 
-def option_enum(
-    choices: Union[Dict[str, TChoice], List[TChoice]], **kwargs: TChoice
-) -> Type[TChoice]:
+def option_enum(choices: Union[Dict[str, TChoice], List[TChoice]], **kwargs: TChoice) -> Type[
+    TChoice
+]:
     """A utility function to create an enum type.
     Returns a new :class:`~enum.Enum` based on the provided parameters.
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ext/tasks/__init__.py#L724'>disnake/ext/tasks/__init__.py~L724</a>
```diff
 
 
 @overload
-def loop(
-    cls: Type[Object[L_co, Concatenate[LF, P]]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[LF], L_co]: ...
+def loop(cls: Type[Object[L_co, Concatenate[LF, P]]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [LF], L_co
+]: ...
 
 
 def loop(
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/guild.py#L436'>disnake/guild.py~L436</a>
```diff
         inner = " ".join(f"{k!s}={v!r}" for k, v in attrs)
         return f"<Guild {inner}>"
 
-    def _update_voice_state(
-        self, data: GuildVoiceState, channel_id: Optional[int]
-    ) -> Tuple[Optional[Member], VoiceState, VoiceState]:
+    def _update_voice_state(self, data: GuildVoiceState, channel_id: Optional[int]) -> Tuple[
+        Optional[Member], VoiceState, VoiceState
+    ]:
         user_id = int(data["user_id"])
         channel: Optional[VocalGuildChannel] = self.get_channel(channel_id)  # type: ignore
         try:
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/guild.py#L2402'>disnake/guild.py~L2402</a>
```diff
 
         return threads
 
-    async def fetch_scheduled_events(
-        self, *, with_user_count: bool = False
-    ) -> List[GuildScheduledEvent]:
+    async def fetch_scheduled_events(self, *, with_user_count: bool = False) -> List[
+        GuildScheduledEvent
+    ]:
         """|coro|
 
         Retrieves a list of all :class:`GuildScheduledEvent` instances that the guild has.
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/guild.py#L3612'>disnake/guild.py~L3612</a>
```diff
     @overload
     async def get_or_fetch_member(self, member_id: int, *, strict: Literal[True]) -> Member: ...
 
-    async def get_or_fetch_member(
-        self, member_id: int, *, strict: bool = False
-    ) -> Optional[Member]:
+    async def get_or_fetch_member(self, member_id: int, *, strict: bool = False) -> Optional[
+        Member
+    ]:
         """|coro|
 
         Tries to get the member from the cache. If it fails,
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L151'>disnake/http.py~L151</a>
```diff
     return multipart
 
 
-def to_multipart_with_attachments(
-    payload: Dict[str, Any], files: Sequence[File]
-) -> List[Dict[str, Any]]:
+def to_multipart_with_attachments(payload: Dict[str, Any], files: Sequence[File]) -> List[
+    Dict[str, Any]
+]:
     """Updates the payload's attachments and converts it to a multipart payload
 
     Shorthand for ``set_attachments`` + ``to_multipart``
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L489'>disnake/http.py~L489</a>
```diff
 
     # Group functionality
 
-    def start_group(
-        self, user_id: Snowflake, recipients: List[int]
-    ) -> Response[channel.GroupDMChannel]:
+    def start_group(self, user_id: Snowflake, recipients: List[int]) -> Response[
+        channel.GroupDMChannel
+    ]:
         payload = {
             "recipients": recipients,
         }
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L593'>disnake/http.py~L593</a>
```diff
             params=params,
         )
 
-    def expire_poll(
-        self, channel_id: Snowflake, message_id: Snowflake
-    ) -> Response[message.Message]:
+    def expire_poll(self, channel_id: Snowflake, message_id: Snowflake) -> Response[
+        message.Message
+    ]:
         return self.request(
             Route(
                 "POST",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L725'>disnake/http.py~L725</a>
```diff
             return self.request(r, form=multipart, files=files)
         return self.request(r, json=fields)
 
-    def add_reaction(
-        self, channel_id: Snowflake, message_id: Snowflake, emoji: str
-    ) -> Response[None]:
+    def add_reaction(self, channel_id: Snowflake, message_id: Snowflake, emoji: str) -> Response[
+        None
+    ]:
         r = Route(
             "PUT",
             "/channels/{channel_id}/messages/{message_id}/reactions/{emoji}/@me",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L807'>disnake/http.py~L807</a>
```diff
         )
         return self.request(r)
 
-    def get_message(
-        self, channel_id: Snowflake, message_id: Snowflake
-    ) -> Response[message.Message]:
+    def get_message(self, channel_id: Snowflake, message_id: Snowflake) -> Response[
+        message.Message
+    ]:
         r = Route(
             "GET",
             "/channels/{channel_id}/messages/{message_id}",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L845'>disnake/http.py~L845</a>
```diff
             Route("GET", "/channels/{channel_id}/messages", channel_id=channel_id), params=params
         )
 
-    def publish_message(
-        self, channel_id: Snowflake, message_id: Snowflake
-    ) -> Response[message.Message]:
+    def publish_message(self, channel_id: Snowflake, message_id: Snowflake) -> Response[
+        message.Message
+    ]:
         return self.request(
             Route(
                 "POST",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L884'>disnake/http.py~L884</a>
```diff
 
     # Member management
 
-    def search_guild_members(
-        self, guild_id: Snowflake, query: str, limit: int = 1
-    ) -> Response[List[member.MemberWithUser]]:
+    def search_guild_members(self, guild_id: Snowflake, query: str, limit: int = 1) -> Response[
+        List[member.MemberWithUser]
+    ]:
         r = Route("GET", "/guilds/{guild_id}/members/search", guild_id=guild_id)
 
         return self.request(r, params={"query": query, "limit": limit})
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1240'>disnake/http.py~L1240</a>
```diff
         route = Route("GET", "/guilds/{guild_id}/threads/active", guild_id=guild_id)
         return self.request(route)
 
-    def get_thread_member(
-        self, channel_id: Snowflake, user_id: Snowflake
-    ) -> Response[threads.ThreadMember]:
+    def get_thread_member(self, channel_id: Snowflake, user_id: Snowflake) -> Response[
+        threads.ThreadMember
+    ]:
         route = Route(
             "GET",
             "/channels/{channel_id}/thread-members/{user_id}",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1448'>disnake/http.py~L1448</a>
```diff
     def guild_templates(self, guild_id: Snowflake) -> Response[List[template.Template]]:
         return self.request(Route("GET", "/guilds/{guild_id}/templates", guild_id=guild_id))
 
-    def create_template(
-        self, guild_id: Snowflake, payload: template.CreateTemplate
-    ) -> Response[template.Template]:
+    def create_template(self, guild_id: Snowflake, payload: template.CreateTemplate) -> Response[
+        template.Template
+    ]:
         return self.request(
             Route("POST", "/guilds/{guild_id}/templates", guild_id=guild_id), json=payload
         )
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1460'>disnake/http.py~L1460</a>
```diff
             Route("PUT", "/guilds/{guild_id}/templates/{code}", guild_id=guild_id, code=code)
         )
 
-    def edit_template(
-        self, guild_id: Snowflake, code: str, payload: Dict[str, Any]
-    ) -> Response[template.Template]:
+    def edit_template(self, guild_id: Snowflake, code: str, payload: Dict[str, Any]) -> Response[
+        template.Template
+    ]:
         valid_keys = (
             "name",
             "description",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1478'>disnake/http.py~L1478</a>
```diff
             Route("DELETE", "/guilds/{guild_id}/templates/{code}", guild_id=guild_id, code=code)
         )
 
-    def create_from_template(
-        self, code: str, name: str, icon: Optional[str]
-    ) -> Response[guild.Guild]:
+    def create_from_template(self, code: str, name: str, icon: Optional[str]) -> Response[
+        guild.Guild
+    ]:
         payload = {
             "name": name,
         }
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1542'>disnake/http.py~L1542</a>
```diff
     def get_all_guild_channels(self, guild_id: Snowflake) -> Response[List[guild.GuildChannel]]:
         return self.request(Route("GET", "/guilds/{guild_id}/channels", guild_id=guild_id))
 
-    def get_members(
-        self, guild_id: Snowflake, limit: int, after: Optional[Snowflake]
-    ) -> Response[List[member.MemberWithUser]]:
+    def get_members(self, guild_id: Snowflake, limit: int, after: Optional[Snowflake]) -> Response[
+        List[member.MemberWithUser]
+    ]:
         params: Dict[str, Any] = {
             "limit": limit,
         }
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1554'>disnake/http.py~L1554</a>
```diff
         r = Route("GET", "/guilds/{guild_id}/members", guild_id=guild_id)
         return self.request(r, params=params)
 
-    def get_member(
-        self, guild_id: Snowflake, member_id: Snowflake
-    ) -> Response[member.MemberWithUser]:
+    def get_member(self, guild_id: Snowflake, member_id: Snowflake) -> Response[
+        member.MemberWithUser
+    ]:
         return self.request(
             Route(
                 "GET",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L1616'>disnake/http.py~L1616</a>
```diff
     def get_all_guild_stickers(self, guild_id: Snowflake) -> Response[List[sticker.GuildSticker]]:
         return self.request(Route("GET", "/guilds/{guild_id}/stickers", guild_id=guild_id))
 
-    def get_guild_sticker(
-        self, guild_id: Snowflake, sticker_id: Snowflake
-    ) -> Response[sticker.GuildSticker]:
+    def get_guild_sticker(self, guild_id: Snowflake, sticker_id: Snowflake) -> Response[
+        sticker.GuildSticker
+    ]:
         return self.request(
             Route(
                 "GET",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2058'>disnake/http.py~L2058</a>
```diff
     def get_stage_instance(self, channel_id: Snowflake) -> Response[channel.StageInstance]:
         return self.request(Route("GET", "/stage-instances/{channel_id}", channel_id=channel_id))
 
-    def create_stage_instance(
-        self, *, reason: Optional[str] = None, **payload: Any
-    ) -> Response[channel.StageInstance]:
+    def create_stage_instance(self, *, reason: Optional[str] = None, **payload: Any) -> Response[
+        channel.StageInstance
+    ]:
         valid_keys = (
             "channel_id",
             "topic",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2172'>disnake/http.py~L2172</a>
```diff
 
         return self.request(route, json=fields, reason=reason)
 
-    def delete_guild_scheduled_event(
-        self, guild_id: Snowflake, event_id: Snowflake
-    ) -> Response[None]:
+    def delete_guild_scheduled_event(self, guild_id: Snowflake, event_id: Snowflake) -> Response[
+        None
+    ]:
         route = Route(
             method="DELETE",
             path="/guilds/{guild_id}/scheduled-events/{event_id}",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2216'>disnake/http.py~L2216</a>
```diff
 
     # Welcome screens
 
-    def get_guild_welcome_screen(
-        self, guild_id: Snowflake
-    ) -> Response[welcome_screen.WelcomeScreen]:
+    def get_guild_welcome_screen(self, guild_id: Snowflake) -> Response[
+        welcome_screen.WelcomeScreen
+    ]:
         r = Route("GET", "/guilds/{guild_id}/welcome-screen", guild_id=guild_id)
         return self.request(r)
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2246'>disnake/http.py~L2246</a>
```diff
             Route("GET", "/guilds/{guild_id}/auto-moderation/rules", guild_id=guild_id)
         )
 
-    def get_auto_moderation_rule(
-        self, guild_id: Snowflake, rule_id: Snowflake
-    ) -> Response[automod.AutoModRule]:
+    def get_auto_moderation_rule(self, guild_id: Snowflake, rule_id: Snowflake) -> Response[
+        automod.AutoModRule
+    ]:
         return self.request(
             Route(
                 "GET",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2441'>disnake/http.py~L2441</a>
```diff
             params=params,
         )
 
-    def get_global_command(
-        self, application_id: Snowflake, command_id: Snowflake
-    ) -> Response[interactions.ApplicationCommand]:
+    def get_global_command(self, application_id: Snowflake, command_id: Snowflake) -> Response[
+        interactions.ApplicationCommand
+    ]:
         r = Route(
             "GET",
             "/applications/{application_id}/commands/{command_id}",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2472'>disnake/http.py~L2472</a>
```diff
         )
         return self.request(r, json=payload)
 
-    def delete_global_command(
-        self, application_id: Snowflake, command_id: Snowflake
-    ) -> Response[None]:
+    def delete_global_command(self, application_id: Snowflake, command_id: Snowflake) -> Response[
+        None
+    ]:
         r = Route(
             "DELETE",
             "/applications/{application_id}/commands/{command_id}",
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/http.py#L2821'>disnake/http.py~L2821</a>
```diff
 
         return self._format_gateway_url(data["url"], encoding=encoding, zlib=zlib)
 
-    async def get_bot_gateway(
-        self, *, encoding: str = "json", zlib: bool = True
-    ) -> Tuple[int, str, gateway.SessionStartLimit]:
+    async def get_bot_gateway(self, *, encoding: str = "json", zlib: bool = True) -> Tuple[
+        int, str, gateway.SessionStartLimit
+    ]:
         try:
             data: gateway.GatewayBot = await self.request(Route("GET", "/gateway/bot"))
         except HTTPException as exc:
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/i18n.py#L127'>disnake/i18n.py~L127</a>
```diff
 
     @overload
     @classmethod
-    def _cast(
-        cls, string: LocalizedOptional, required: Literal[False]
-    ) -> Localized[Optional[str]]: ...
+    def _cast(cls, string: LocalizedOptional, required: Literal[False]) -> Localized[
+        Optional[str]
+    ]: ...
 
     @overload
     @classmethod
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/interactions/base.py#L1965'>disnake/interactions/base.py~L1965</a>
```diff
         )
 
     @overload
-    def get_with_type(
-        self, key: Snowflake, data_type: Union[OptionType, ComponentType]
-    ) -> Union[Member, User, Role, AnyChannel, Message, Attachment, None]: ...
+    def get_with_type(self, key: Snowflake, data_type: Union[OptionType, ComponentType]) -> Union[
+        Member, User, Role, AnyChannel, Message, Attachment, None
+    ]: ...
 
     @overload
     def get_with_type(
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/interactions/base.py#L2002'>disnake/interactions/base.py~L2002</a>
```diff
 
         return default
 
-    def get_by_id(
-        self, key: Optional[int]
-    ) -> Optional[Union[Member, User, Role, AnyChannel, Message, Attachment]]:
+    def get_by_id(self, key: Optional[int]) -> Optional[
+        Union[Member, User, Role, AnyChannel, Message, Attachment]
+    ]:
         if key is None:
             return None
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/member.py#L440'>disnake/member.py~L440</a>
```diff
         self._flags = data.get("flags", 0)
         self._avatar_decoration_data = data.get("avatar_decoration_data")
 
-    def _presence_update(
-        self, data: PresenceData, user: UserPayload
-    ) -> Optional[Tuple[User, User]]:
+    def _presence_update(self, data: PresenceData, user: UserPayload) -> Optional[
+        Tuple[User, User]
+    ]:
         self.activities = tuple(create_activity(a, state=self._state) for a in data["activities"])
         self._client_status = {
             sys.intern(key): sys.intern(value)
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/partial_emoji.py#L253'>disnake/partial_emoji.py~L253</a>
```diff
     # utility method for unusual emoji model in forums
     # (e.g. default reaction, tag emoji)
     @staticmethod
-    def _emoji_to_name_id(
-        emoji: Optional[Union[str, Emoji, PartialEmoji]],
-    ) -> Tuple[Optional[str], Optional[int]]:
+    def _emoji_to_name_id(emoji: Optional[Union[str, Emoji, PartialEmoji]]) -> Tuple[
+        Optional[str], Optional[int]
+    ]:
         if emoji is None:
             return None, None
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/player.py#L577'>disnake/player.py~L577</a>
```diff
             return codec, bitrate  # noqa: B012
 
     @staticmethod
-    def _probe_codec_native(
-        source, executable: str = "ffmpeg"
-    ) -> Tuple[Optional[str], Optional[int]]:
+    def _probe_codec_native(source, executable: str = "ffmpeg") -> Tuple[
+        Optional[str], Optional[int]
+    ]:
         exe = executable[:2] + "probe" if executable in ("ffmpeg", "avconv") else executable
         args = [
             exe,
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/player.py#L606'>disnake/player.py~L606</a>
```diff
         return codec, bitrate
 
     @staticmethod
-    def _probe_codec_fallback(
-        source, executable: str = "ffmpeg"
-    ) -> Tuple[Optional[str], Optional[int]]:
+    def _probe_codec_fallback(source, executable: str = "ffmpeg") -> Tuple[
+        Optional[str], Optional[int]
+    ]:
         args = [executable, "-hide_banner", "-i", source]
         proc = subprocess.Popen(
             args, creationflags=CREATE_NO_WINDOW, stdout=subprocess.PIPE, stderr=subprocess.STDOUT
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/state.py#L443'>disnake/state.py~L443</a>
```diff
 
         del guild
 
-    def _get_global_application_command(
-        self, application_command_id: int
-    ) -> Optional[APIApplicationCommand]:
+    def _get_global_application_command(self, application_command_id: int) -> Optional[
+        APIApplicationCommand
+    ]:
         return self._global_application_commands.get(application_command_id)
 
     def _add_global_application_command(
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/state.py#L1950'>disnake/state.py~L1950</a>
```diff
         entitlement = Entitlement(data=data, state=self)
         self.dispatch("entitlement_delete", entitlement)
 
-    def _get_reaction_user(
-        self, channel: MessageableChannel, user_id: int
-    ) -> Optional[Union[User, Member]]:
+    def _get_reaction_user(self, channel: MessageableChannel, user_id: int) -> Optional[
+        Union[User, Member]
+    ]:
         if isinstance(channel, (TextChannel, VoiceChannel, Thread, StageChannel)):
             return channel.guild.get_member(user_id)
         return self.get_user(user_id)
 
     # methods to handle all sorts of different emoji formats
 
-    def _get_emoji_from_data(
-        self, data: PartialEmojiPayload
-    ) -> Optional[Union[str, Emoji, PartialEmoji]]:
+    def _get_emoji_from_data(self, data: PartialEmojiPayload) -> Optional[
+        Union[str, Emoji, PartialEmoji]
+    ]:
         """Convert partial emoji data to proper emoji.
         Returns unicode emojis as strings.
 
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/state.py#L2227'>disnake/state.py~L2227</a>
```diff
 
     # Application command permissions
 
-    async def bulk_fetch_command_permissions(
-        self, guild_id: int
-    ) -> List[GuildApplicationCommandPermissions]:
+    async def bulk_fetch_command_permissions(self, guild_id: int) -> List[
+        GuildApplicationCommandPermissions
+    ]:
         array = await self.http.get_guild_application_command_permissions(
             self.application_id,
             guild_id,  # type: ignore
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/sticker.py#L507'>disnake/sticker.py~L507</a>
```diff
         await self._state.http.delete_guild_sticker(self.guild_id, self.id, reason=reason)
 
 
-def _sticker_factory(
-    sticker_type: Literal[1, 2],
-) -> Tuple[Type[Union[StandardSticker, GuildSticker, Sticker]], StickerType]:
+def _sticker_factory(sticker_type: Literal[1, 2]) -> Tuple[
+    Type[Union[StandardSticker, GuildSticker, Sticker]], StickerType
+]:
     value = try_enum(StickerType, sticker_type)
     if value == StickerType.standard:
         return StandardSticker, value
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/button.py#L265'>disnake/ui/button.py~L265</a>
```diff
 
 
 @overload
-def button(
-    cls: Type[Object[B_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[B_co]], DecoratedItem[B_co]]: ...
+def button(cls: Type[Object[B_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[B_co]], DecoratedItem[B_co]
+]: ...
 
 
-def button(
-    cls: Type[Object[B_co, ...]] = Button[Any], **kwargs: Any
-) -> Callable[[ItemCallbackType[B_co]], DecoratedItem[B_co]]:
+def button(cls: Type[Object[B_co, ...]] = Button[Any], **kwargs: Any) -> Callable[
+    [ItemCallbackType[B_co]], DecoratedItem[B_co]
+]:
     """A decorator that attaches a button to a component.
 
     The function being decorated should have three parameters, ``self`` representing
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/select/channel.py#L158'>disnake/ui/select/channel.py~L158</a>
```diff
 
 
 @overload
-def channel_select(
-    cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]: ...
+def channel_select(cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]: ...
 
 
-def channel_select(
-    cls: Type[Object[S_co, ...]] = ChannelSelect[Any], **kwargs: Any
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]:
+def channel_select(cls: Type[Object[S_co, ...]] = ChannelSelect[Any], **kwargs: Any) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]:
     """A decorator that attaches a channel select menu to a component.
 
     The function being decorated should have three parameters, ``self`` representing
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/select/mentionable.py#L136'>disnake/ui/select/mentionable.py~L136</a>
```diff
 
 
 @overload
-def mentionable_select(
-    cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]: ...
+def mentionable_select(cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]: ...
 
 
 def mentionable_select(
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/select/role.py#L132'>disnake/ui/select/role.py~L132</a>
```diff
 
 
 @overload
-def role_select(
-    cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]: ...
+def role_select(cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]: ...
 
 
-def role_select(
-    cls: Type[Object[S_co, ...]] = RoleSelect[Any], **kwargs: Any
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]:
+def role_select(cls: Type[Object[S_co, ...]] = RoleSelect[Any], **kwargs: Any) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]:
     """A decorator that attaches a role select menu to a component.
 
     The function being decorated should have three parameters, ``self`` representing
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/select/string.py#L258'>disnake/ui/select/string.py~L258</a>
```diff
 
 
 @overload
-def string_select(
-    cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]: ...
+def string_select(cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]: ...
 
 
-def string_select(
-    cls: Type[Object[S_co, ...]] = StringSelect[Any], **kwargs: Any
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]:
+def string_select(cls: Type[Object[S_co, ...]] = StringSelect[Any], **kwargs: Any) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]:
     """A decorator that attaches a string select menu to a component.
 
     The function being decorated should have three parameters, ``self`` representing
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/ui/select/user.py#L133'>disnake/ui/select/user.py~L133</a>
```diff
 
 
 @overload
-def user_select(
-    cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]: ...
+def user_select(cls: Type[Object[S_co, P]], *_: P.args, **kwargs: P.kwargs) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]: ...
 
 
-def user_select(
-    cls: Type[Object[S_co, ...]] = UserSelect[Any], **kwargs: Any
-) -> Callable[[ItemCallbackType[S_co]], DecoratedItem[S_co]]:
+def user_select(cls: Type[Object[S_co, ...]] = UserSelect[Any], **kwargs: Any) -> Callable[
+    [ItemCallbackType[S_co]], DecoratedItem[S_co]
+]:
     """A decorator that attaches a user select menu to a component.
 
     The function being decorated should have three parameters, ``self`` representing
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/utils.py#L694'>disnake/utils.py~L694</a>
```diff
 
 
 @overload
-def resolve_invite(
-    invite: Union[Invite, str], *, with_params: Literal[True]
-) -> Tuple[str, Dict[str, str]]: ...
+def resolve_invite(invite: Union[Invite, str], *, with_params: Literal[True]) -> Tuple[
+    str, Dict[str, str]
+]: ...
 
 
-def resolve_invite(
-    invite: Union[Invite, str], *, with_params: bool = False
-) -> Union[str, Tuple[str, Dict[str, str]]]:
+def resolve_invite(invite: Union[Invite, str], *, with_params: bool = False) -> Union[
+    str, Tuple[str, Dict[str, str]]
+]:
     """Resolves an invite from a :class:`~disnake.Invite`, URL or code.
 
     Parameters
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+348 -354 lines across 57 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/examples/reminderbot/actions/actions.py#L68'>examples/reminderbot/actions/actions.py~L68</a>
```diff
     def name(self) -> Text:
         return "action_tell_id"
 
-    async def run(
-        self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
-    ) -> List[Dict[Text, Any]]:
+    async def run(self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]) -> List[
+        Dict[Text, Any]
+    ]:
         conversation_id = tracker.sender_id
 
         dispatcher.utter_message(f"The ID of this conversation is '{conversation_id}'.")
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/examples/reminderbot/actions/actions.py#L110'>examples/reminderbot/actions/actions.py~L110</a>
```diff
     def name(self) -> Text:
         return "action_forget_reminders"
 
-    async def run(
-        self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]
-    ) -> List[Dict[Text, Any]]:
+    async def run(self, dispatcher, tracker: Tracker, domain: Dict[Text, Any]) -> List[
+        Dict[Text, Any]
+    ]:
         dispatcher.utter_message("Okay, I'll cancel all your reminders.")
 
         # Cancel all reminders
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/cli/utils.py#L90'>rasa/cli/utils.py~L90</a>
```diff
     return current
 
 
-def missing_config_keys(
-    path: Union["Path", Text], mandatory_keys: List[Text]
-) -> List[Text]:
+def missing_config_keys(path: Union["Path", Text], mandatory_keys: List[Text]) -> List[
+    Text
+]:
     """Checks whether the config file at `path` contains the `mandatory_keys`.
 
     Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/actions/action.py#L399'>rasa/core/actions/action.py~L399</a>
```diff
         """Resolve the name of the intent from the action name."""
         return action_name.split(UTTER_PREFIX)[1]
 
-    def get_full_retrieval_name(
-        self, tracker: "DialogueStateTracker"
-    ) -> Optional[Text]:
+    def get_full_retrieval_name(self, tracker: "DialogueStateTracker") -> Optional[
+        Text
+    ]:
         """Returns full retrieval name for the action.
 
         Extracts retrieval intent from response selector and
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/actions/forms.py#L100'>rasa/core/actions/forms.py~L100</a>
```diff
             "group": group,
         }
 
-    def get_mappings_for_slot(
-        self, slot_to_fill: Text, domain: Domain
-    ) -> List[Dict[Text, Any]]:
+    def get_mappings_for_slot(self, slot_to_fill: Text, domain: Domain) -> List[
+        Dict[Text, Any]
+    ]:
         """Get mappings for requested slot.
 
         If None, map requested slot to an entity with the same name
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/agent.py#L401'>rasa/core/agent.py~L401</a>
```diff
 
         return await self.processor.parse_message(message)  # type: ignore[union-attr]
 
-    async def handle_message(
-        self, message: UserMessage
-    ) -> Optional[List[Dict[Text, Any]]]:
+    async def handle_message(self, message: UserMessage) -> Optional[
+        List[Dict[Text, Any]]
+    ]:
         """Handle a single message."""
         if not self.is_ready():
             logger.info("Ignoring message as there is no agent to handle it.")
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/agent.py#L415'>rasa/core/agent.py~L415</a>
```diff
             )
 
     @agent_must_be_ready
-    async def predict_next_for_sender_id(
-        self, sender_id: Text
-    ) -> Optional[Dict[Text, Any]]:
+    async def predict_next_for_sender_id(self, sender_id: Text) -> Optional[
+        Dict[Text, Any]
+    ]:
         """Predict the next action for a sender id."""
         return await self.processor.predict_next_for_sender_id(  # type: ignore[union-attr] # noqa:E501
             sender_id
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L110'>rasa/core/channels/console.py~L110</a>
```diff
 async def _get_user_input(previous_response: Dict[str, Any]) -> Optional[Text]: ...
 
 
-async def _get_user_input(
-    previous_response: Optional[Dict[str, Any]],
-) -> Optional[Text]:
+async def _get_user_input(previous_response: Optional[Dict[str, Any]]) -> Optional[
+    Text
+]:
     button_response = None
     if previous_response is not None:
         button_response = _print_bot_output(previous_response, is_latest_message=True)
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/evaluation/marker_stats.py#L11'>rasa/core/evaluation/marker_stats.py~L11</a>
```diff
 from rasa.core.evaluation.marker_base import EventMetaData
 
 
-def compute_statistics(
-    values: List[Union[float, int]],
-) -> Dict[Text, Union[int, float, np.floating]]:
+def compute_statistics(values: List[Union[float, int]]) -> Dict[
+    Text, Union[int, float, np.floating]
+]:
     """Computes some statistics over the given numbers."""
     return {
         "count": len(values) if values else 0,
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/featurizers/single_state_featurizer.py#L43'>rasa/core/featurizers/single_state_featurizer.py~L43</a>
```diff
         self.action_texts: List[Text] = []
         self.entity_tag_specs: List[EntityTagSpec] = []
 
-    def _create_entity_tag_specs(
-        self, bilou_tagging: bool = False
-    ) -> List[EntityTagSpec]:
+    def _create_entity_tag_specs(self, bilou_tagging: bool = False) -> List[
+        EntityTagSpec
+    ]:
         """Returns the tag to index mapping for entities.
 
         Returns:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/featurizers/tracker_featurizers.py#L671'>rasa/core/featurizers/tracker_featurizers.py~L671</a>
```diff
         self.remove_duplicates = remove_duplicates
 
     @staticmethod
-    def slice_state_history(
-        states: List[State], slice_length: Optional[int]
-    ) -> List[State]:
+    def slice_state_history(states: List[State], slice_length: Optional[int]) -> List[
+        State
+    ]:
         """Slices states from the trackers history.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/migrate.py#L80'>rasa/core/migrate.py~L80</a>
```diff
     return updated_mappings
 
 
-def _migrate_form_slots(
-    domain: Dict[Text, Any],
-) -> Tuple[Dict[Any, Dict[str, Any]], Optional[Any]]:
+def _migrate_form_slots(domain: Dict[Text, Any]) -> Tuple[
+    Dict[Any, Dict[str, Any]], Optional[Any]
+]:
     updated_slots = domain.get(KEY_SLOTS, {})
     forms = domain.get(KEY_FORMS, {})
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/migrate.py#L134'>rasa/core/migrate.py~L134</a>
```diff
     return properties
 
 
-def _migrate_custom_slots(
-    slot_name: Text, properties: Dict[Text, Any]
-) -> Dict[Text, Any]:
+def _migrate_custom_slots(slot_name: Text, properties: Dict[Text, Any]) -> Dict[
+    Text, Any
+]:
     if not properties.get("mappings"):
         properties.update({"mappings": [{"type": "custom"}]})
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/policy.py#L613'>rasa/core/policies/policy.py~L613</a>
```diff
         return max(self.probabilities, default=0.0)
 
 
-def confidence_scores_for(
-    action_name: Text, value: float, domain: Domain
-) -> List[float]:
+def confidence_scores_for(action_name: Text, value: float, domain: Domain) -> List[
+    float
+]:
     """Returns confidence scores if a single action is predicted.
 
     Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/rule_policy.py#L908'>rasa/core/policies/rule_policy.py~L908</a>
```diff
             reversed_rule_states[turn_index], conversation_state
         )
 
-    def _get_possible_keys(
-        self, lookup: Dict[Text, Text], states: List[State]
-    ) -> Set[Text]:
+    def _get_possible_keys(self, lookup: Dict[Text, Text], states: List[State]) -> Set[
+        Text
+    ]:
         possible_keys = set(lookup.keys())
         for i, state in enumerate(reversed(states)):
             # find rule keys that correspond to current state
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/rule_policy.py#L1110'>rasa/core/policies/rule_policy.py~L1110</a>
```diff
         prediction, _ = self._predict(tracker, domain)
         return prediction
 
-    def _predict(
-        self, tracker: DialogueStateTracker, domain: Domain
-    ) -> Tuple[PolicyPrediction, Optional[Text]]:
+    def _predict(self, tracker: DialogueStateTracker, domain: Domain) -> Tuple[
+        PolicyPrediction, Optional[Text]
+    ]:
         (
             rules_action_name_from_text,
             prediction_source_from_text,
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/unexpected_intent_policy.py#L987'>rasa/core/policies/unexpected_intent_policy.py~L987</a>
```diff
 
         return labels_embed
 
-    def run_bulk_inference(
-        self, model_data: RasaModelData
-    ) -> Dict[Text, Union[np.ndarray, Dict[Text, Any]]]:
+    def run_bulk_inference(self, model_data: RasaModelData) -> Dict[
+        Text, Union[np.ndarray, Dict[Text, Any]]
+    ]:
         """Computes model's predictions for input data.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/processor.py#L122'>rasa/core/processor.py~L122</a>
```diff
         self.http_interpreter = http_interpreter
 
     @staticmethod
-    def _load_model(
-        model_path: Union[Text, Path],
-    ) -> Tuple[Text, ModelMetadata, GraphRunner]:
+    def _load_model(model_path: Union[Text, Path]) -> Tuple[
+        Text, ModelMetadata, GraphRunner
+    ]:
         """Unpacks a model from a given path using the graph model loader."""
         try:
             if os.path.isfile(model_path):
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/processor.py#L150'>rasa/core/processor.py~L150</a>
```diff
             except tarfile.ReadError:
                 raise ModelNotFound(f"Model {model_path} can not be loaded.")
 
-    async def handle_message(
-        self, message: UserMessage
-    ) -> Optional[List[Dict[Text, Any]]]:
+    async def handle_message(self, message: UserMessage) -> Optional[
+        List[Dict[Text, Any]]
+    ]:
         """Handle a single message with this processor."""
         # preprocess message if necessary
         tracker = await self.log_message(message, should_save_tracker=False)
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/processor.py#L230'>rasa/core/processor.py~L230</a>
```diff
             body.update(event.as_dict())
             anonymization_pipeline.run(body)
 
-    async def predict_next_for_sender_id(
-        self, sender_id: Text
-    ) -> Optional[Dict[Text, Any]]:
+    async def predict_next_for_sender_id(self, sender_id: Text) -> Optional[
+        Dict[Text, Any]
+    ]:
         """Predict the next action for the given sender_id.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/processor.py#L693'>rasa/core/processor.py~L693</a>
```diff
                     docs=DOCS_URL_DOMAINS,
                 )
 
-    def _get_action(
-        self, action_name: Text
-    ) -> Optional[rasa.core.actions.action.Action]:
+    def _get_action(self, action_name: Text) -> Optional[
+        rasa.core.actions.action.Action
+    ]:
         return rasa.core.actions.action.action_for_name_or_text(
             action_name, self.domain, self.action_endpoint
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/test.py#L452'>rasa/core/test.py~L452</a>
```diff
     )
 
 
-def _clean_entity_results(
-    text: Text, entity_results: List[Dict[Text, Any]]
-) -> List["EntityPrediction"]:
+def _clean_entity_results(text: Text, entity_results: List[Dict[Text, Any]]) -> List[
+    "EntityPrediction"
+]:
     """Extract only the token variables from an entity dict."""
     cleaned_entities = []
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L274'>rasa/core/tracker_store.py~L274</a>
```diff
         """
         raise NotImplementedError()
 
-    async def retrieve_full_tracker(
-        self, conversation_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, conversation_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Retrieve method for fetching all tracker events across conversation sessions\
         that may be overridden by specific tracker.
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L396'>rasa/core/tracker_store.py~L396</a>
```diff
         """Returns sender_ids of the Tracker Store in memory."""
         return self.store.keys()
 
-    async def retrieve_full_tracker(
-        self, sender_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, sender_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Returns tracker matching sender_id.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L406'>rasa/core/tracker_store.py~L406</a>
```diff
         """
         return await self._retrieve(sender_id, fetch_all_sessions=True)
 
-    async def _retrieve(
-        self, sender_id: Text, fetch_all_sessions: bool
-    ) -> Optional[DialogueStateTracker]:
+    async def _retrieve(self, sender_id: Text, fetch_all_sessions: bool) -> Optional[
+        DialogueStateTracker
+    ]:
         """Returns tracker matching sender_id.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L531'>rasa/core/tracker_store.py~L531</a>
```diff
         """
         return await self._retrieve(sender_id, fetch_all_sessions=False)
 
-    async def retrieve_full_tracker(
-        self, sender_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, sender_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Retrieves tracker for all conversation sessions.
 
         The Redis key is formed by appending a prefix to sender_id.
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L546'>rasa/core/tracker_store.py~L546</a>
```diff
         """
         return await self._retrieve(sender_id, fetch_all_sessions=True)
 
-    async def _retrieve(
-        self, sender_id: Text, fetch_all_sessions: bool
-    ) -> Optional[DialogueStateTracker]:
+    async def _retrieve(self, sender_id: Text, fetch_all_sessions: bool) -> Optional[
+        DialogueStateTracker
+    ]:
         """Returns tracker matching sender_id.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L697'>rasa/core/tracker_store.py~L697</a>
```diff
         """
         return await self._retrieve(sender_id, fetch_all_sessions=False)
 
-    async def retrieve_full_tracker(
-        self, sender_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, sender_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Retrieves tracker for all conversation sessions.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L707'>rasa/core/tracker_store.py~L707</a>
```diff
         """
         return await self._retrieve(sender_id, fetch_all_sessions=True)
 
-    async def _retrieve(
-        self, sender_id: Text, fetch_all_sessions: bool
-    ) -> Optional[DialogueStateTracker]:
+    async def _retrieve(self, sender_id: Text, fetch_all_sessions: bool) -> Optional[
+        DialogueStateTracker
+    ]:
         """Returns tracker matching sender_id.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L909'>rasa/core/tracker_store.py~L909</a>
```diff
 
         return DialogueStateTracker.from_dict(sender_id, events, self.domain.slots)
 
-    async def retrieve_full_tracker(
-        self, conversation_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, conversation_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Fetching all tracker events across conversation sessions."""
         events = await self._retrieve(
             conversation_id, fetch_events_from_all_sessions=True
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L1248'>rasa/core/tracker_store.py~L1248</a>
```diff
         """Retrieves tracker for the latest conversation session."""
         return await self._retrieve(sender_id, fetch_events_from_all_sessions=False)
 
-    async def retrieve_full_tracker(
-        self, conversation_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, conversation_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Fetching all tracker events across conversation sessions."""
         return await self._retrieve(
             conversation_id, fetch_events_from_all_sessions=True
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L1449'>rasa/core/tracker_store.py~L1449</a>
```diff
             self.on_tracker_store_error(e)
             await self.fallback_tracker_store.save(tracker)
 
-    async def retrieve_full_tracker(
-        self, sender_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, sender_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Calls `retrieve_full_tracker` method of primary tracker store.
 
         Args:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/tracker_store.py#L1642'>rasa/core/tracker_store.py~L1642</a>
```diff
         result = self._tracker_store.save(tracker)
         return await result if isawaitable(result) else result
 
-    async def retrieve_full_tracker(
-        self, conversation_id: Text
-    ) -> Optional[DialogueStateTracker]:
+    async def retrieve_full_tracker(self, conversation_id: Text) -> Optional[
+        DialogueStateTracker
+    ]:
         """Wrapper to call `retrieve_full_tracker` method of primary tracker store."""
         result = self._tracker_store.retrieve_full_tracker(conversation_id)
         return (
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L758'>rasa/core/training/interactive.py~L758</a>
```diff
     return answers["export_stories"], answers["export_nlu"], answers["export_domain"]
 
 
-def _split_conversation_at_restarts(
-    events: List[Dict[Text, Any]],
-) -> List[List[Dict[Text, Any]]]:
+def _split_conversation_at_restarts(events: List[Dict[Text, Any]]) -> List[
+    List[Dict[Text, Any]]
+]:
     """Split a conversation at restart events.
 
     Returns an array of event lists, without the restart events.
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/utils.py#L131'>rasa/core/utils.py~L131</a>
```diff
     return output
 
 
-def extract_args(
-    kwargs: Dict[Text, Any], keys_to_extract: Set[Text]
-) -> Tuple[Dict[Text, Any], Dict[Text, Any]]:
+def extract_args(kwargs: Dict[Text, Any], keys_to_extract: Set[Text]) -> Tuple[
+    Dict[Text, Any], Dict[Text, Any]
+]:
     """Go through the kwargs and filter out the specified keys.
 
     Return both, the filtered kwargs as well as the remaining kwargs.
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/engine/caching.py#L421'>rasa/engine/caching.py~L421</a>
```diff
             output_fingerprint_key,
         )
 
-    def _get_cached_result(
-        self, output_fingerprint_key: Text
-    ) -> Tuple[Optional[Path], Optional[Text]]:
+    def _get_cached_result(self, output_fingerprint_key: Text) -> Tuple[
+        Optional[Path], Optional[Text]
+    ]:
         with self._sessionmaker.begin() as session:
             query = sa.select(
                 self.CacheEntry.result_location, self.CacheEntry.result_type
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/engine/recipes/default_recipe.py#L988'>rasa/engine/recipes/default_recipe.py~L988</a>
```diff
         return {k for k in all_keys if k not in config.keys()}
 
     @staticmethod
-    def complete_config(
-        config: D...*[Comment body truncated]*

---

_Label `formatter` added by @AlexWaygood on 2024-11-08 10:42_

---

_Closed by @MichaReiser on 2024-11-18 06:53_

---
