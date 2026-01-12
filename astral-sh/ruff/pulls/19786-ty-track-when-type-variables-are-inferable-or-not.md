```yaml
number: 19786
title: "[ty] Track when type variables are inferable or not"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/inferrable
created_at: 2025-08-06T15:47:32Z
updated_at: 2025-08-16T22:25:05Z
url: https://github.com/astral-sh/ruff/pull/19786
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Track when type variables are inferable or not

---

_@dcreager_

`Type::TypeVar` now distinguishes whether the typevar in question is inferable or not.

A typevar is _not inferable_ inside the body of the generic class or function that binds it:

```py
def f[T](t: T) -> T:
    return t
```

The infered type of `t` in the function body is `TypeVar(T, NotInferable)`. This represents how e.g. assignability checks need to be valid for all possible specializations of the typevar. Most of the existing assignability/etc logic only applies to non-inferable typevars.

Outside of the function body, the typevar is _inferable_:

```py
f(4)
```

Here, the parameter type of `f` is `TypeVar(T, Inferable)`. This represents how e.g. assignability doesn't need to hold for _all_ specializations; instead, we need to find the constraints under which this specific assignability check holds.

This is in support of starting to perform specialization inference _as part of_ performing the assignability check at the call site.

In the [[POPL2015][]] paper, this concept is called _monomorphic_ / _polymorphic_, but I thought _non-inferable_ / _inferable_ would be clearer for us.

Depends on #19784 

[POPL2015]: https://doi.org/10.1145/2676726.2676991

---

_Label `ty` added by @dcreager on 2025-08-06 15:47_

---

_Comment by @github-actions[bot] on 2025-08-06 15:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-16 22:08:04.048640851 +0000
+++ new-output.txt	2025-08-16 22:08:04.117640859 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1201d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1b09b)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-06 15:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
+ tests/test_array.py:57:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_array.py:68:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_array.py:79:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_array.py:90:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_array.py:98:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 226 diagnostics
+ Found 231 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_kits/webhacks.py:81:26: error[invalid-argument-type] Argument to function `wraps` is incorrect: Argument type `(...) -> _RWrapped@wraps` does not satisfy upper bound of type variable `_ServerFn`
- Found 60 diagnostics
+ Found 59 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_path.py:235:34: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def readlink(self) -> Self@readlink`
+ src/trio/_path.py:236:32: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def rename(self, target: str | PurePath) -> Self@rename`
+ src/trio/_path.py:237:33: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def replace(self, target: str | PurePath) -> Self@replace`
+ src/trio/_path.py:238:33: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def resolve(self, strict: bool = Literal[False]) -> Self@resolve`
+ src/trio/_path.py:245:34: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def absolute(self) -> Self@absolute`
+ src/trio/_path.py:246:36: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def expanduser(self) -> Self@expanduser`
+ src/trio/_tests/type_tests/raisesgroup.py:98:5: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ src/trio/_tests/type_tests/raisesgroup.py:103:5: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/trio/_tests/type_tests/raisesgroup.py:92:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/raisesgroup.py:95:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/raisesgroup.py:99:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/raisesgroup.py:102:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/raisesgroup.py:106:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/raisesgroup.py:107:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 654 diagnostics
+ Found 656 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ bson/regex.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `Regex[_T@Regex]`, found `Regex[str]`
- Found 514 diagnostics
+ Found 515 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/sourceline.py:17:33: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Argument type `AnyStr@_add_lc_filename` does not satisfy constraints of type variable `AnyStr`
+ schema_salad/sourceline.py:20:33: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Argument type `AnyStr@_add_lc_filename` does not satisfy constraints of type variable `AnyStr`
- Found 144 diagnostics
+ Found 146 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/web_request.py:599:16: error[invalid-return-type] Return type does not match returned value: expected `slice[int, int, int]`, found `slice[Any, _StartT_co@slice, _StartT_co@slice | _StopT_co@slice]`
+ aiohttp/web_runner.py:411:16: error[invalid-return-type] Return type does not match returned value: expected `Server[Request]`, found `Server[BaseRequest]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `ContentviewMessage`, found `Response | None`
+ test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:794:44: error[invalid-argument-type] Argument to bound method `difference` is incorrect: Argument type `Iterable[Unknown]` does not satisfy upper bound of type variable `I`
- Found 1779 diagnostics
+ Found 1778 diagnostics

zulip (https://github.com/zulip/zulip)
- analytics/views/stats.py:444:9: error[invalid-assignment] Object of type `Unknown | _ST@ForeignKey` is not assignable to `Realm | None`
- analytics/views/stats.py:476:25: warning[possibly-unbound-attribute] Attribute `date_created` on type `Realm | None` is possibly unbound
- analytics/views/stats.py:489:17: warning[possibly-unbound-attribute] Attribute `string_id` on type `Realm | None` is possibly unbound
- analytics/views/stats.py:517:21: warning[possibly-unbound-attribute] Attribute `id` on type `Realm | None` is possibly unbound
- confirmation/models.py:205:44: error[invalid-argument-type] Argument to function `confirmation_url` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/registration.py:14:52: warning[possibly-unbound-attribute] Attribute `exempt_from_license_number_check` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:2168:34: error[invalid-argument-type] Argument to function `get_price_per_license_and_discount` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe.py:2241:12: warning[possibly-unbound-attribute] Attribute `exempt_from_license_number_check` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:2372:70: error[invalid-argument-type] Argument to function `get_price_per_license_and_discount` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe.py:2418:71: error[invalid-argument-type] Argument to function `get_price_per_license_and_discount` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe.py:3073:17: warning[possibly-unbound-attribute] Attribute `exempt_from_license_number_check` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3109:17: warning[possibly-unbound-attribute] Attribute `exempt_from_license_number_check` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3157:59: error[invalid-argument-type] Argument to function `get_price_per_license_and_discount` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe.py:3207:21: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3224:20: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3230:33: warning[possibly-unbound-attribute] Attribute `licenses` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- corporate/lib/stripe.py:3231:39: warning[possibly-unbound-attribute] Attribute `id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- corporate/lib/stripe.py:3248:30: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3308:34: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3354:21: warning[possibly-unbound-attribute] Attribute `flat_discounted_months` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3355:21: warning[possibly-unbound-attribute] Attribute `save` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3359:34: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3387:16: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3407:17: warning[possibly-unbound-attribute] Attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:3739:20: warning[possibly-unbound-attribute] Attribute `exempt_from_license_number_check` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:4170:16: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:4172:13: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5555:8: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5556:69: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5557:10: warning[possibly-unbound-attribute] Attribute `remote_realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5558:24: warning[possibly-unbound-attribute] Attribute `remote_realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5561:10: warning[possibly-unbound-attribute] Attribute `remote_server` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5562:25: warning[possibly-unbound-attribute] Attribute `remote_server` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe.py:5634:9: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:86:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `(Unknown & ~None) | (_ST@OneToOneField & ~None)`
- corporate/lib/stripe_event_handler.py:88:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteZulipServer`, found `(Unknown & ~None) | (_ST@OneToOneField & ~None)`
- corporate/lib/stripe_event_handler.py:92:73: error[invalid-argument-type] Argument to function `get_active_user_profile_by_id_in_realm` is incorrect: Expected `Realm`, found `(Unknown & ~None) | (_ST@OneToOneField & ~None)`
- corporate/lib/stripe_event_handler.py:95:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `(Unknown & ~None) | (_ST@OneToOneField & ~None)`
- corporate/lib/stripe_event_handler.py:109:9: error[invalid-argument-type] Argument to function `get_billing_session_for_stripe_webhook` is incorrect: Expected `Customer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:129:8: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:131:13: error[invalid-argument-type] Argument to function `get_configured_fixed_price_plan_offer` is incorrect: Expected `Customer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:131:23: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:139:66: error[invalid-argument-type] Argument to function `get_billing_session_for_stripe_webhook` is incorrect: Expected `Customer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:140:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:140:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:140:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:141:16: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:143:23: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:165:66: error[invalid-argument-type] Argument to function `get_billing_session_for_stripe_webhook` is incorrect: Expected `Customer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:166:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:166:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:166:83: error[invalid-argument-type] Argument to bound method `get_complimentary_access_plan` is incorrect: Expected `Customer | None`, found `Unknown | _ST@ForeignKey`
- corporate/lib/stripe_event_handler.py:170:44: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:171:20: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:173:27: warning[possibly-unbound-attribute] Attribute `required_plan_tier` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/lib/stripe_event_handler.py:188:16: warning[possibly-unbound-attribute] Attribute `is_free_trial` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- corporate/lib/stripe_event_handler.py:194:20: warning[possibly-unbound-attribute] Attribute `status` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- corporate/lib/stripe_event_handler.py:196:49: error[invalid-argument-type] Argument to function `get_current_plan_by_customer` is incorrect: Expected `Customer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/support.py:439:58: error[invalid-argument-type] Argument to function `get_push_status_for_remote_request` is incorrect: Expected `RemoteZulipServer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/support.py:468:52: error[invalid-argument-type] Argument to function `has_stale_audit_log` is incorrect: Expected `RemoteZulipServer`, found `Unknown | _ST@ForeignKey`
- corporate/lib/support.py:490:37: error[invalid-argument-type] Argument to function `get_realm_user_data` is incorrect: Expected `Realm`, found `Unknown | Realm | _ST@ForeignKey`
- corporate/lib/support.py:501:65: error[invalid-argument-type] Argument to function `get_formatted_realm_upload_space_used` is incorrect: Expected `Realm`, found `Unknown | Realm | _ST@ForeignKey`
- corporate/tests/test_remote_billing.py:86:46: warning[possibly-unbound-attribute] Attribute `host` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/tests/test_remote_billing.py:145:42: warning[possibly-unbound-attribute] Attribute `host` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/tests/test_remote_billing.py:168:35: warning[possibly-unbound-attribute] Attribute `uuid` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/tests/test_remote_billing.py:175:78: warning[possibly-unbound-attribute] Attribute `uuid` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/tests/test_stripe.py:6207:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `stripe_customer_id` on type `Unknown | _ST@ForeignKey`
- corporate/tests/test_stripe.py:6208:9: warning[possibly-unbound-attribute] Attribute `save` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/views/audit_logs.py:20:12: warning[possibly-unbound-attribute] Attribute `host` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- corporate/views/plan_activity.py:15:16: warning[possibly-unbound-attribute] Attribute `name` on type `(Unknown & ~AlwaysFalsy) | (_ST@OneToOneField & ~AlwaysFalsy)` is possibly unbound
- corporate/views/plan_activity.py:17:16: warning[possibly-unbound-attribute] Attribute `name` on type `(Unknown & ~AlwaysFalsy) | (_ST@OneToOneField & ~AlwaysFalsy)` is possibly unbound
- corporate/views/plan_activity.py:19:12: warning[possibly-unbound-attribute] Attribute `hostname` on type `(Unknown & ~None) | (_ST@OneToOneField & ~None)` is possibly unbound
- corporate/views/realm_activity.py:106:16: warning[possibly-unbound-attribute] Attribute `delivery_email` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/views/sponsorship.py:27:8: warning[possibly-unbound-attribute] Attribute `demo_organization_scheduled_deletion_date` on type `Unknown | Realm | _ST@ForeignKey` is possibly unbound
- corporate/views/upgrade.py:64:13: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/views/upgrade.py:65:13: warning[possibly-unbound-attribute] Attribute `string_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- corporate/views/upgrade.py:206:8: warning[possibly-unbound-attribute] Attribute `demo_organization_scheduled_deletion_date` on type `Unknown | Realm | _ST@ForeignKey` is possibly unbound
- corporate/views/user_activity.py:55:13: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/alert_words.py:12:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:31:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:41:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:54:9: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:69:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:88:59: error[invalid-argument-type] Argument to function `send_bot_owner_update_events` is incorrect: Expected `UserProfile | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:126:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:168:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/bots.py:205:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/channel_folders.py:73:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/channel_folders.py:73:56: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/channel_folders.py:105:22: error[invalid-argument-type] Argument to function `render_channel_folder_description` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:87:13: warning[possibly-unbound-attribute] Attribute `topics_policy` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/create_user.py:92:42: error[invalid-argument-type] Argument to function `internal_send_stream_message` is incorrect: Expected `Stream`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/create_user.py:106:35: error[invalid-argument-type] Argument to function `realm_user_count` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:112:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:116:64: error[invalid-argument-type] Argument to function `send_message_to_signup_notification_stream` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:122:17: error[invalid-argument-type] Argument to function `generate_licenses_low_warning_message_if_required` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:127:61: error[invalid-argument-type] Argument to function `send_group_direct_message_to_admins` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:143:9: error[invalid-assignment] Object of type `Unknown | _ST@ForeignKey` is not assignable to `UserProfile | None`
- zerver/actions/create_user.py:159:62: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:171:9: error[invalid-argument-type] Argument to function `bulk_add_subscriptions` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:216:22: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:225:87: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:298:21: warning[possibly-unbound-attribute] Attribute `is_zephyr_mirror_realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:305:13: warning[possibly-unbound-attribute] Attribute `is_active` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/create_user.py:309:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/create_user.py:311:59: warning[possibly-unbound-attribute] Attribute `realm_id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/create_user.py:312:17: error[invalid-argument-type] Argument to function `internal_send_private_message` is incorrect: Expected `UserProfile`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/create_user.py:339:32: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:339:52: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `UserProfile | None`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/create_user.py:345:8: warning[possibly-unbound-attribute] Attribute `send_welcome_emails` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:356:35: warning[possibly-unbound-attribute] Attribute `welcome_message_custom_text` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:412:13: warning[possibly-unbound-attribute] Attribute `get_active_users` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:415:35: warning[possibly-unbound-attribute] Attribute `get_active_users` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:459:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:464:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:470:51: error[invalid-argument-type] Argument to function `get_data_for_inaccessible_user` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:473:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:482:47: error[invalid-argument-type] Argument to function `stream_name` is incorrect: Expected `Stream | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:483:55: error[invalid-argument-type] Argument to function `stream_name` is incorrect: Expected `Stream | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:510:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:574:64: error[invalid-argument-type] Argument to function `realm_user_count_by_role` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:577:36: error[invalid-argument-type] Argument to function `maybe_enqueue_audit_log_upload` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:592:66: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:704:68: error[invalid-argument-type] Argument to function `realm_user_count_by_role` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:707:40: error[invalid-argument-type] Argument to function `maybe_enqueue_audit_log_upload` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:709:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:732:64: error[invalid-argument-type] Argument to function `realm_user_count_by_role` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:735:36: error[invalid-argument-type] Argument to function `maybe_enqueue_audit_log_upload` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:741:17: warning[possibly-unbound-attribute] Attribute `is_active` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/create_user.py:759:66: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:765:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:774:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:776:18: warning[possibly-unbound-attribute] Attribute `get_human_admin_users` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/create_user.py:788:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:794:69: error[invalid-argument-type] Argument to function `send_bot_owner_update_events` is incorrect: Expected `UserProfile | None`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/create_user.py:801:9: error[invalid-argument-type] Argument to function `bulk_get_subscriber_peer_info` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:813:9: error[invalid-argument-type] Argument to function `send_peer_subscriber_events` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/create_user.py:838:34: error[invalid-argument-type] Argument to function `send_update_events_for_anonymous_group_settings` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/custom_profile_fields.py:169:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/custom_profile_fields.py:197:52: error[invalid-argument-type] Argument to function `render_stream_description` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/default_streams.py:75:32: error[invalid-argument-type] Argument to function `notify_default_streams` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/default_streams.py:83:28: error[invalid-argument-type] Argument to function `notify_default_streams` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/invites.py:340:36: warning[possibly-unbound-attribute] Attribute `id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/invites.py:407:28: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/invites.py:427:28: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `Realm`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/invites.py:427:35: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `UserProfile | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/invites.py:432:13: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/invites.py:438:35: error[invalid-argument-type] Argument to function `notify_invites_changed` is incorrect: Expected `UserProfile | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/invites.py:469:35: warning[possibly-unbound-attribute] Attribute `full_name` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/invites.py:470:31: warning[possibly-unbound-attribute] Attribute `delivery_email` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/invites.py:492:30: error[invalid-argument-type] Argument to function `common_context` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/invites.py:495:23: warning[possibly-unbound-attribute] Attribute `full_name` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/invites.py:496:24: warning[possibly-unbound-attribute] Attribute `delivery_email` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_delete.py:120:28: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:169:12: warning[possibly-unbound-attribute] Attribute `allow_message_editing` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:176:8: warning[possibly-unbound-attribute] Attribute `message_content_edit_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:177:28: warning[possibly-unbound-attribute] Attribute `message_content_edit_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:273:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:313:24: error[invalid-argument-type] Argument to function `do_delete_messages` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:343:45: error[invalid-argument-type] Argument to function `stream_message_url` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:346:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:362:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:483:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:554:18: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:555:9: error[invalid-argument-type] Argument to function `get_recipient_info` is incorrect: Expected `Recipient`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:736:34: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:849:13: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:915:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1052:17: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1213:13: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1223:13: error[invalid-argument-type] Argument to function `bulk_access_stream_messages_query` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1237:16: warning[possibly-unbound-attribute] Attribute `is_bot` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1238:57: error[invalid-argument-type] Argument to function `apply_automatic_unmute_follow_topics_policy` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1240:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1310:17: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1357:35: warning[possibly-unbound-attribute] Attribute `move_messages_within_stream_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1361:13: warning[possibly-unbound-attribute] Attribute `move_messages_within_stream_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1365:34: warning[possibly-unbound-attribute] Attribute `move_messages_between_streams_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1370:13: warning[possibly-unbound-attribute] Attribute `move_messages_between_streams_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1391:40: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1391:67: error[invalid-argument-type] Argument to function `get_stream_by_id_in_realm` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1489:22: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1490:61: error[invalid-argument-type] Argument to function `get_stream_by_id_in_realm` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1498:46: error[invalid-argument-type] Argument to function `get_stream_topics_policy` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1600:17: warning[possibly-unbound-attribute] Attribute `move_messages_within_stream_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1604:21: warning[possibly-unbound-attribute] Attribute `move_messages_within_stream_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1620:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `UserProfile | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1631:13: error[invalid-argument-type] Argument to function `render_incoming_message` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1637:56: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1638:52: error[invalid-argument-type] Argument to function `stream_wildcard_mention_allowed` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1638:76: error[invalid-argument-type] Argument to function `stream_wildcard_mention_allowed` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1643:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1643:58: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1646:17: error[invalid-argument-type] Argument to function `topic_wildcard_mention_allowed` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1646:58: error[invalid-argument-type] Argument to function `topic_wildcard_mention_allowed` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_edit.py:1665:17: warning[possibly-unbound-attribute] Attribute `move_messages_between_streams_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_edit.py:1669:21: warning[possibly-unbound-attribute] Attribute `move_messages_between_streams_limit_seconds` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_flags.py:99:32: error[invalid-argument-type] Argument to function `send_event_rollback_unsafe` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_flags.py:142:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_flags.py:185:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_flags.py:391:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:603:42: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:608:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `UserProfile | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:612:21: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:621:18: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:622:9: error[invalid-argument-type] Argument to function `get_recipient_info` is incorrect: Expected `Recipient`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:636:9: error[invalid-argument-type] Argument to function `render_incoming_message` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:697:9: error[invalid-argument-type] Argument is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:753:8: warning[possibly-unbound-attribute] Attribute `type` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:1273:8: warning[possibly-unbound-attribute] Attribute `type` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:1470:8: warning[possibly-unbound-attribute] Attribute `is_zephyr_mirror_realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:1470:47: warning[possibly-unbound-attribute] Attribute `deactivated` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/message_send.py:1492:51: warning[possibly-unbound-attribute] Attribute `realm_id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/message_send.py:1493:9: error[invalid-argument-type] Argument to function `internal_send_private_message` is incorrect: Expected `UserProfile`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/message_send.py:1515:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/message_send.py:1744:9: error[invalid-assignment] Object of type `Unknown | _ST@ForeignKey` is not assignable to `Realm | None`
- zerver/actions/message_send.py:1755:77: error[invalid-argument-type] Argument to function `validate_stream_name_with_pm_notification` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1757:73: error[invalid-argument-type] Argument to function `validate_stream_id_with_pm_notification` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1791:50: error[invalid-argument-type] Argument to function `get_stream_topics_policy` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1808:44: error[invalid-argument-type] Argument to function `check_sender_can_access_recipients` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1811:13: error[invalid-argument-type] Argument to function `get_recipients_for_user_creation_events` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1826:39: error[invalid-argument-type] Argument to function `check_can_send_direct_message` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1893:65: error[invalid-argument-type] Argument to function `stream_wildcard_mention_allowed` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1901:81: error[invalid-argument-type] Argument to function `topic_wildcard_mention_allowed` is incorrect: Expected `Realm`, found `Realm | None`
- zerver/actions/message_send.py:1993:9: error[invalid-argument-type] Argument to function `_internal_prep_message` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_send.py:2046:9: error[invalid-argument-type] Argument to function `_internal_prep_message` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_summary.py:91:53: error[invalid-argument-type] Argument to function `clean_narrow_for_message_fetch` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_summary.py:95:9: error[invalid-argument-type] Argument to function `fetch_messages` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_summary.py:126:9: error[invalid-argument-type] Argument to function `messages_for_ids` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/message_summary.py:213:50: error[invalid-argument-type] Argument to function `markdown_convert` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/muted_users.py:24:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/muted_users.py:41:65: error[invalid-argument-type] Argument to function `get_user_mutes` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/muted_users.py:42:26: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/muted_users.py:42:54: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/muted_users.py:45:15: warning[possibly-unbound-attribute] Attribute `realm` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/muted_users.py:50:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/navigation_views.py:38:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/navigation_views.py:96:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/navigation_views.py:121:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/onboarding_steps.py:12:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/presence.py:77:32: error[invalid-argument-type] Argument to function `send_event_rollback_unsafe` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/presence.py:269:13: warning[possibly-unbound-attribute] Attribute `presence_disabled` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/reactions.py:47:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/reactions.py:80:21: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/reactions.py:161:29: error[invalid-argument-type] Argument to function `check_emoji_request` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:61:48: error[invalid-argument-type] Argument to function `get_realm_domains` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:73:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:92:48: error[invalid-argument-type] Argument to function `get_realm_domains` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:97:65: warning[possibly-unbound-attribute] Attribute `emails_restricted_to_domains` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/realm_domains.py:102:31: error[invalid-argument-type] Argument to function `do_set_realm_property` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:104:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_domains.py:104:56: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/realm_emoji.py:65:24: error[invalid-argument-type] Argument to function `notify_realm_emoji` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_export.py:26:25: error[invalid-argument-type] Argument to function `notify_realm_export` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/realm_settings.py:540:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@OneToOneField`
- zerver/actions/realm_settings.py:540:56: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@OneToOneField` is possibly unbound
- zerver/actions/reminders.py:31:9: error[invalid-argument-type] Argument to function `check_message` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/reminders.py:52:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/saved_snippets.py:40:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/saved_snippets.py:69:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/saved_snippets.py:91:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:90:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:99:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:165:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:193:9: error[invalid-argument-type] Argument to function `get_recipient_ids` is incorrect: Expected `Recipient | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:294:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:310:34: error[invalid-argument-type] Argument to function `access_message` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:312:22: error[invalid-argument-type] Argument to function `get_reminder_formatted_content` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:320:51: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/scheduled_messages.py:321:9: error[invalid-argument-type] Argument to function `internal_send_private_message` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:339:8: warning[possibly-unbound-attribute] Attribute `deactivated` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/scheduled_messages.py:342:12: warning[possibly-unbound-attribute] Attribute `is_active` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/scheduled_messages.py:355:42: error[invalid-argument-type] Argument to function `for_stream` is incorrect: Expected `Stream`, found `(Unknown & ~None) | (_ST@ForeignKey & ~None)`
- zerver/actions/scheduled_messages.py:362:60: error[invalid-argument-type] Argument to function `for_user_ids` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:371:9: error[invalid-argument-type] Argument to function `check_message` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:372:9: error[invalid-argument-type] Argument to function `check_message` is incorrect: Expected `Client`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:375:9: error[invalid-argument-type] Argument to function `check_message` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/scheduled_messages.py:385:37: error[invalid-argument-type] Argument to function `notify_remove_scheduled_message` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:166:53: error[invalid-argument-type] Argument to function `do_remove_streams_from_default_stream_group` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:176:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:178:32: error[invalid-argument-type] Argument to function `send_stream_deletion_event` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:190:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:267:18: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:289:55: error[invalid-argument-type] Argument to function `get_streams_traffic` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:301:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:305:9: error[invalid-argument-type] Argument to function `send_stream_creation_event` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:314:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:340:8: warning[possibly-unbound-attribute] Attribute `id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/streams.py:340:35: warning[possibly-unbound-attribute] Attribute `id` on type `(Unknown & ~None) | (_ST@ForeignKey & ~None)` is possibly unbound
- zerver/actions/streams.py:1217:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1226:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1239:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1356:33: warning[possibly-unbound-attribute] Attribute `get_admin_users_and_bots` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1368:59: error[invalid-argument-type] Argument to function `get_streams_traffic` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1371:13: error[invalid-argument-type] Argument to function `send_stream_creation_event` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1382:33: warning[possibly-unbound-attribute] Attribute `get_admin_users_and_bots` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1395:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1412:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1471:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1536:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1538:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1558:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1592:26: error[invalid-argument-type] Argument to function `render_stream_description` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1617:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1636:21: warning[possibly-unbound-attribute] Attribute `message_retention_days` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1638:21: warning[possibly-unbound-attribute] Attribute `message_retention_days` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1640:28: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1706:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1748:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1752:59: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1777:32: warning[possibly-unbound-attribute] Attribute `default_language` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/streams.py:1855:13: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1861:63: error[invalid-argument-type] Argument to function `get_streams_traffic` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1864:17: error[invalid-argument-type] Argument to function `send_stream_creation_event` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1880:17: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1885:40: error[invalid-argument-type] Argument to function `send_stream_deletion_event` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1887:30: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1902:34: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1916:34: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/streams.py:1962:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:63:9: error[invalid-argument-type] Argument to function `set_visibility_policy_possible` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:63:17: error[invalid-argument-type] Argument to function `set_visibility_policy_possible` is incorrect: Expected `Message`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:65:9: warning[possibly-unbound-attribute] Attribute `automatically_follow_topics_policy` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/submessage.py:66:9: warning[possibly-unbound-attribute] Attribute `automatically_unmute_topics_in_muted_streams_policy` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/submessage.py:68:21: warning[possibly-unbound-attribute] Attribute `recipient` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/submessage.py:69:45: error[invalid-argument-type] Argument to function `access_stream_by_id` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:72:73: error[invalid-argument-type] Argument to function `visibility_policy_for_participation` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:75:17: error[invalid-argument-type] Argument to function `should_change_visibility_policy` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:77:28: warning[possibly-unbound-attribute] Attribute `topic_name` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/submessage.py:80:21: error[invalid-argument-type] Argument to function `do_set_user_topic_visibility_policy` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/submessage.py:82:32: warning[possibly-unbound-attribute] Attribute `topic_name` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/typing.py:59:83: error[invalid-argument-type] Argument to function `get_user_by_id_in_realm_including_cross_realm` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/typing.py:65:9: error[invalid-argument-type] Argument to function `do_send_typing_notification` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/typing.py:102:32: error[invalid-argument-type] Argument to function `send_event_rollback_unsafe` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/typing.py:141:32: error[invalid-argument-type] Argument to function `send_event_rollback_unsafe` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/typing.py:152:79: error[invalid-argument-type] Argument to function `get_user_by_id_in_realm_including_cross_realm` is incorrect: Expected `Realm | None`, found `Unknown | _ST@ForeignKey`
- zerver/actions/typing.py:173:32: error[invalid-argument-type] Argument to function `send_event_rollback_unsafe` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/uploads.py:33:30: warning[possibly-unbound-attribute] Attribute `currently_used_upload_space_bytes` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/uploads.py:35:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/uploads.py:47:44: warning[possibly-unbound-attribute] Attribute `type_id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/uploads.py:51:44: error[invalid-argument-type] Argument to function `validate_attachment_request` is incorrect: Expected `UserProfile | AnonymousUser`, found `Unknown | _ST@ForeignKey`
- zerver/actions/uploads.py:63:17: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | _ST@ForeignKey` is possibly unbound
- zerver/actions/uploads.py:76:38: error[invalid-argument-type] Argument to function `notify_attachment_update` is incorrect: Expected `UserProfile`, found `Unknown | _ST@ForeignKey`
- zerver/actions/user_groups.py:215:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unknown | _ST@ForeignKey`
- zerver/actions/user_groups.py:252:26: error[invalid-argument-type] Argument to function `send_event_on_commit` is incorrect: Expected `Realm`, found `Unkno...*[Comment body truncated]*

---

_Renamed from "[ty] Track when type variables are inferrable or not" to "[ty] Track when type variables are inferable or not" by @dcreager on 2025-08-06 16:49_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-06 16:50_

---

_Comment by @codspeed-hq[bot] on 2025-08-06 17:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finferrable?runnerMode=WallTime)

### Merging #19786 will **not alter performance**

<sub>Comparing <code>dcreager/inferrable</code> (3c9d084) with <code>main</code> (9ac39ce)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-11 19:41_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-11 19:41_

---

_Comment by @github-actions[bot] on 2025-08-11 19:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 8 | 2 | 6 |
| `unused-ignore-comment` | 5 | 6 | 0 |
| `invalid-return-type` | 2 | 0 | 0 |
| **Total** | **15** | **8** | **6** |

**[Full report with detailed diff](https://dcreager-inferrable.ecosystem-663.pages.dev/diff)**


---

_Comment by @dcreager on 2025-08-11 21:59_

Looking at some of the ecosystem results:

- `aiohttp` is a new true positive, because `Server` uses an invariant typevar
- `schema_salad` is a false positive we were [already seeing](https://play.ty.dev/6ae21754-d3a0-4596-a285-9e827cd59c3c) for non-recursive calls, which now shows up for recursive calls too because we're tracking the inferable/non-inferable typevars separately

---

_Comment by @dcreager on 2025-08-12 20:49_

- `trio` false positives are because we are not recursing into `Callable` to infer specializations of typevars in a function call. We would then try to check the argument type against the parameter type that still contains an unspecialized typevar. Before, we would perform that assignability check as if we were in the function body, checking the argument against the typevar bound. Now, we see that there's an _inferrable_ typevar that was not specialized, and (for now) treat that as "not assignable".

---

_Comment by @dcreager on 2025-08-13 16:21_

- `mongo-python-driver` false positive is because there's a `todo` type that doesn't let us simplify an intersection that we get from a narrowing constraint, and that intersection therefore isn't equivalent to the invariant typevar in the `Regex` we're supposed to return

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-13 16:43_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-13 16:43_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-13 18:10_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-13 18:10_

---

_Marked ready for review by @dcreager on 2025-08-13 19:30_

---

_Review requested from @carljm by @dcreager on 2025-08-13 19:30_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-13 19:30_

---

_Review requested from @sharkdp by @dcreager on 2025-08-13 19:30_

---

_Review requested from @MichaReiser by @dcreager on 2025-08-13 19:30_

---

_Comment by @dcreager on 2025-08-13 19:31_

Addressed an earlier performance regression, looked at the ecosystem report. This is ready for review!

---

_Comment by @AlexWaygood on 2025-08-13 20:00_

> Addressed an earlier performance regression, looked at the ecosystem report. This is ready for review!

Hmm I still see a 9% regression in https://github.com/astral-sh/ruff/pull/19786#issuecomment-3160920788 (and at https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finferrable?runnerMode=WallTime) — is that expected?

---

_Comment by @dcreager on 2025-08-13 20:16_

> Hmm I still see a 9% regression in [#19786 (comment)](https://github.com/astral-sh/ruff/pull/19786#issuecomment-3160920788) (and at https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finferrable?runnerMode=WallTime) — is that expected?

It is not! I was basing my claim on the CI job having a green :heavy_check_mark:. I wonder why that job passed if there's still an unacknowledged regression...

---

_Comment by @MichaReiser on 2025-08-14 06:35_

> > Hmm I still see a 9% regression in [#19786 (comment)](https://github.com/astral-sh/ruff/pull/19786#issuecomment-3160920788) (and at [codspeed.io/astral-sh/ruff/branches/dcreager%2Finferrable?runnerMode=WallTime](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finferrable?runnerMode=WallTime)) — is that expected?
> 
> It is not! I was basing my claim on the CI job having a green ✔️. I wonder why that job passed if there's still an unacknowledged regression...

I reported this to the codspee folks

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:104 on 2025-08-14 17:51_

What are the reasons we need to do this eagerly, rather than wait and infer the type of `Self` as a typevar at call time, based on the self-type stored in the `BoundMethod` type? Seems like it should have the same result.

I wonder if this is a significant part of the perf regression? Because even on methods that don't use `Self` (the vast majority of methods), now we have to eagerly traverse the signature looking for `Self` to replace, rather than waiting until call time and handling it if we run across it in the signature.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:342 on 2025-08-14 17:58_

Oh, I guess this (and similar for parameters below) is another candidate for the perf regression, since we now have to always walk all of these types looking for typevars, even for functions that don't use typevars.

It feels like in principle we should be able to do this via contextual type inference rather than via a type substitution -- but we don't have contextual type inference yet.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1132 on 2025-08-14 17:59_

Is this right? Are typevars in the default type of a parameter actually inferable? I don't think they are...

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:615 on 2025-08-14 18:33_

I think this distinction is pretty subtle, so these doc comments will be important. And I don't think this quite captures it. When I think "outside the generic class or function that binds it", I think of

```py
T = TypeVar("T")

T # <-- here

def f(x: T) -> T: ...
```

But that's not `Type::TypeVar`, that's `Type::KnownInstance`.

I feel like what we really want to say here is something more like "only in the externally-visible signature of a generic function that binds it, or in the signature of the constructor of a generic class that binds it."

I don't think inferrable typevars are even a thing for generic classes at all, except for in a constructor signature? (How is that case handled in this PR? I don't remember seeing it.)



---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:619 on 2025-08-14 18:37_

Would "inside the body of" make this clearer? Because to me, "in the signature" still feels like "inside", if not otherwise clarified.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-08-14 18:54_

It's not totally clear to me how this PR will interact with the case discussed in https://github.com/astral-sh/ty/issues/95, where we end up needing to consider equivalence and subtyping of inferrable typevars, but not in a context where we are actually specializing a call, we are just doing subtyping/equivalence/assignability of generic function signatures.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-14 18:56_

What are example cases where we would ever access a member on an inferrable typevar? It seems like inferrable typevars only exist in signatures, not in arbitrary code?

Separately, I'm not sure what this constraint could practically look like, other than the constraint "must be a subtype of [synthesized protocol with given instance member]"?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6886 on 2025-08-14 19:00_

I think similar comments apply to this text as to the doc comments above the type variants.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6904 on 2025-08-14 19:03_

This enum seems unused? Is it left over from an earlier approach, but no longer needed now that you are using distinct `Type` variants?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8338 on 2025-08-14 19:04_

Not clear how "instance of the bound type" differs from "the bound instance type". For a class method, shouldn't it be "subclass-of the bound type"?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8452 on 2025-08-14 19:05_

The classmethod handling here seems like a (desirable) behavior change; is there a test demonstrating that behavior improvement?

---

_@carljm reviewed on 2025-08-14 19:10_

This is a thought-provoking one! Lots of questions here; I'm not sure I've fully wrapped my head around this yet.

It felt a bit "wrong" to me initially to use separate types for this, because the use of "inferrable" type var types seems quite limited and contextual. That said, threading that context through all the places we need it does seem quite challenging, and using a separate Type variant does solve that pretty neatly. So I think I'm convinced that makes sense.


---

_@dcreager reviewed on 2025-08-14 19:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-08-14 19:19_

The thought is that with https://github.com/astral-sh/ruff/pull/19838, this would infer the constraints under which the assignability holds, and so for https://github.com/astral-sh/ty/issues/95 that would produce (constraints that solve to) a `T = U` specialization.

---

_@carljm reviewed on 2025-08-14 19:29_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-08-14 19:29_

And if `T` and `U` had incompatible bounds/constraints, there would then be no solution to the constraints, so we'd deny the assignability?

And for example in a case like checking whether `[T](T) -> T` is a supertype or subtype of `(int) -> int`, we'd end up with the constraints that `int <: T` and `T <: int` (because of seeing those in both a covariant and contravariant position), which also wouldn't solve, so we'd (correctly) deny that as well.

Yeah, OK, it does seem like that should all work out correctly!

---

_@dcreager reviewed on 2025-08-14 20:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-08-14 20:19_

> And for example in a case like checking whether `[T](T) -> T` is a supertype or subtype of `(int) -> int`, we'd end up with the constraints that `int <: T` and `T <: int` (because of seeing those in both a covariant and contravariant position), which also wouldn't solve, so we'd (correctly) deny that as well.

A clarification: it would solve to `{T = int}`, indicating that the two types are equivalent when `T = int`, and neither super- nor subtypes of each other otherwise. That would actually let you check both whether there are _any_ specializations where assignability holds, and whether it holds for _all_ specializations.

---

_@carljm reviewed on 2025-08-14 21:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-08-14 21:10_

Makes sense. I think that is relevant to the original question about issue 95, then? Because I think when we are doing subtyping between two generic functions, we need it to hold for _all_ specializations. (It's when we are actually inferring a call to a generic function that we just need it to hold for _any_ specialization, and then that specialization is the one we should use for the return type.) So that means if we get the constraint `{T = U}`, we need that constraint to hold for all specializations, which is only true if the bounds and constraints of `T` and `U` are equivalent.

---

_Comment by @dcreager on 2025-08-14 21:49_

With local testing, this latest patch addresses the performance regression; :crossed_fingers: that Codspeed agrees!

If so, the issue was with the new `BindSelf` type mapping, which is used e.g. with bound methods — when we bind the `self` parameter to a particular type, we also go through and substitute all occurrences of `typing.Self` with that same type. We do that inside of `into_callable_type`, which can be called as part of assignability checking (when comparing with a `Callable` type, for instance). We weren't caching the result, and so if a particular bound method participates in many assignability checks, we could end up repeating a lot of work — and this PR adds to the amount of stuff that we do in that repeated work. Caching `into_callable_type` seems to alleviate that.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:8452 on 2025-08-14 22:09_

The `reveal_type` output here shows it

https://github.com/astral-sh/ruff/pull/19786/files#diff-60e548f13a26365603be56addf56cee3daf1c35ced7f4517a7a36a0eb6301190R246

since `NamedTuple._make` is a classmethod. But I can add a more explicit test for this.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:104 on 2025-08-14 22:17_

> What are the reasons we need to do this eagerly, rather than wait and infer the type of `Self` as a typevar at call time, based on the self-type stored in the `BoundMethod` type? Seems like it should have the same result.

I think that would work as far as inferring those call sites correctly. One benefit of this approach is how bound methods are rendered. e.g.

```py
class C:
    def method(self) -> Self:
        return self

class D(C):
    pass

reveal_type(C.method)    # revealed: def method(self) -> Self@method
reveal_type(C().method)  # revealed: bound method C.method() -> C
reveal_type(D().method)  # revealed: bound method D.method() -> D
```

You could argue that's purely cosmetic, but binding the self parameter is when the `Self` type is fixed, even before the method is called.

> I wonder if this is a significant part of the perf regression?

This was a good guess!  See above, I address the perf regression with some cachine.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:342 on 2025-08-14 22:33_

> It feels like in principle we should be able to do this via contextual type inference rather than via a type substitution -- but we don't have contextual type inference yet.

We discussed this part in our 1:1. We could possibly have a type context that includes "inferrable position or not" in addition to things like "type form or value form". But we don't have that machinery at the moment.

> Oh, I guess this (and similar for parameters below) is another candidate for the perf regression, since we now have to always walk all of these types looking for typevars, even for functions that don't use typevars.

It could be, though I think it's less worrisome than `BindSelf`, since this was already inside of a salsa-cached method, which amortizes the cost.

---

_@dcreager reviewed on 2025-08-14 22:33_

---

_Comment by @MichaReiser on 2025-08-15 06:44_

Nice find on the perf regression. I spent a few minutes looking at perf profiles yesterday and couldn't spot anything. Congrats on narrowing it down!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:1132 on 2025-08-15 17:32_

I think you're right. I was thinking they'd need to be inferable in case the default was applied — but that happens _after_ inference takes place, if no suitable assignment was found for the typevar.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:615 on 2025-08-15 17:53_

> I feel like what we really want to say here is something more like "only in the externally-visible signature of a generic function that binds it, or in the signature of the constructor of a generic class that binds it."

I went with "in a context where we can infer a specialization for it"

> I don't think inferrable typevars are even a thing for generic classes at all, except for in a constructor signature? (How is that case handled in this PR? I don't remember seeing it.)

This is correct. I made the comment mention that it's the _constructor_ of a generic class that has an inferable typevar, not the class itself. This is handled in `GenericContext::identity_specialization` (which isn't changed in this PR), which is called during the call constructor machinery, and which maps each of the class's typevars to the _inferable_ version of itself. (Added some documentation to clarify that)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:619 on 2025-08-15 17:55_

Ditto above, went with "in a context where we cannot infer a specialization"

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-15 17:57_

> What are example cases where we would ever access a member on an inferrable typevar? It seems like inferrable typevars only exist in signatures, not in arbitrary code?

I think that's correct; the "consider" is pretty load-bearing in my comment :sweat_smile: 

Do you feel confident enough in that for me to turn this into a panic? (Or at least a `debug_assert`)

> Separately, I'm not sure what this constraint could practically look like, other than the constraint "must be a subtype of [synthesized protocol with given instance member]"?

That's the only reasonable constraint I can think of.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6886 on 2025-08-15 17:58_

moot; this is dead code per below

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6904 on 2025-08-15 17:58_

Yes that's right, this is from an earlier attempt to have this be a field of a single `Type::TypeVar` variant

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:8338 on 2025-08-15 18:00_

I might be wording things poorly here. For a classmethod

```py
class C:
    @classmethod
    def method(cls) -> Self: ...
```

The "bound instance type" (aka the `self_instance` field of the `BoundMethod`) will be the type of the `cls` parameter, aka `type[C]`. But the return value (and what `Self` should be bound to) is `C` — that is, an instance of the "bound instance type".

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:8452 on 2025-08-15 18:04_

Tests added

---

_@dcreager reviewed on 2025-08-15 18:14_

---

_@carljm reviewed on 2025-08-15 19:29_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1132 on 2025-08-15 19:29_

Sounds like you might be thinking of a typevar default? I think you're right that those also aren't inferable. But this is a function parameter default, and those aren't inferable because they aren't really part of the external signature at all (only presence/absence of a default is). We only need the actual type of the default for the parameter when inferring the type of that parameter inside the body of the function. And the default is a value expression, not a type expression, which I think also makes it clear any typevars in it can't be inferable.

---

_@carljm reviewed on 2025-08-15 19:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-15 19:38_

If we can't come up with a scenario, then I'd favor a `debug_assert`, so that if we're wrong, we are more likely to find out and can consider the correct handling of that case explicitly. (Or it reveals a bug to us where an inferrable TypeVar escapes where it shouldn't.)

Same for the other similar case with callability below.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:256 on 2025-08-15 19:41_

Aha, this comment is very helpful! I realize now that this function subtly changed without its source code changing, because it now make a non-inferrable typevar inferrable!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:623 on 2025-08-15 19:43_

This wording is great! Seems very clear to me.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8338 on 2025-08-15 19:46_

Ah right, that makes sense. Will propose a (very) minor tweak that would have clarified this for me.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8434 on 2025-08-15 19:47_

```suggestion
    /// This is normally the bound-instance type (the type of `self` or `cls`), but if the bound method is
    /// a `@classmethod`, then it should be an instance of that bound-instance type.
```

---

_@carljm approved on 2025-08-15 19:47_

Looks great, thank you!!

---

_@dcreager reviewed on 2025-08-16 12:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-16 12:16_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-16 12:26_

> if we're wrong, we are more likely to find out and can consider the correct handling of that case explicitly. (Or it reveals a bug to us where an inferrable TypeVar escapes where it shouldn't.)

Well that didn't take long! The new assert is firing in the ecosystem report. Investigating...

---

_@dcreager reviewed on 2025-08-16 12:26_

---

_@dcreager reviewed on 2025-08-16 13:26_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-16 13:26_

The minimal repro for this is:

```py
class Base[T, U=T]:
    def __init__(self) -> None: ...

b = Base()
reveal_type(b)  # revealed: Base[Unknown, T@Base]
```

(and in particular, that's an inferable `T@Base` in the specialization)

Removing the `__init__` method, or removing the default type for the typevar, both change the revealed type to `Base[Unknown, Unknown]`.


---

_@dcreager reviewed on 2025-08-16 21:47_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2834 on 2025-08-16 21:47_

The issue was that when we create a `Specialization` during specialization inference, we weren't invoking `specialize_partial`, which is where we have the logic to repeatedly apply the specialization in case a typevar default refers to another typevar. We _were_ calling `specialize_partial` when applying an explicit specialization (e.g. `Base[int, int]`) or when applying the _default_ specialization (e.g. `Base`).

Added a test case for this along with the fix.

---

_Merged by @dcreager on 2025-08-16 22:25_

---

_Closed by @dcreager on 2025-08-16 22:25_

---

_Branch deleted on 2025-08-16 22:25_

---
