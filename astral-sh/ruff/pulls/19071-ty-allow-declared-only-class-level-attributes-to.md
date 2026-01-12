```yaml
number: 19071
title: "[ty] Allow declared-only class-level attributes to be accessed on the class"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/declared-only-class-level-attributes
created_at: 2025-07-01T14:19:50Z
updated_at: 2025-07-02T16:03:58Z
url: https://github.com/astral-sh/ruff/pull/19071
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Allow declared-only class-level attributes to be accessed on the class

---

_@sharkdp_

## Summary

Allow declared-only class-level attributes to be accessed on the class:
```py
class C:
    attr: int

C.attr  # this is now allowed
``` 

closes https://github.com/astral-sh/ty/issues/384
closes https://github.com/astral-sh/ty/issues/553

## Ecosystem analysis

![image](https://github.com/user-attachments/assets/7aa6c130-36a5-4466-8888-808ee1865aff)

* We see many removed `unresolved-attribute` false-positives for code that makes use of sqlalchemy, as expected (see changes for `prefect`)
* We see many removed `call-non-callable` false-positives for uses of `pytest.skip` and similar, as expected
* Most new diagnostics seem to be related to cases like the following, where we previously inferred `int` for `Derived().x`, but now we infer `int | None`. I think this should be a conflicting-declarations/bad-override error anyway? The new behavior may even be preferred here?
  ```py
  class Base:
      x: int | None
  
  
  class Derived(Base):
      def __init__(self):
          self.x: int = 1
  ```

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-07-01 14:19_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-01 14:19_

---

_Comment by @github-actions[bot] on 2025-07-01 14:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

beartype (https://github.com/beartype/beartype)
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `Unknown | None | BeartypeForwardScope` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `Unknown | None | BeartypeForwardScope` is possibly unbound
- Found 555 diagnostics
+ Found 557 diagnostics
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

comtypes (https://github.com/enthought/comtypes)
- error[unresolved-attribute] comtypes/_vtbl.py:363:14: Type `type[IUnknown]` has no attribute `_disp_methods_`
- Found 484 diagnostics
+ Found 483 diagnostics

trio (https://github.com/python-trio/trio)
- error[call-non-callable] src/trio/_core/_tests/test_asyncgen.py:241:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[invalid-argument-type] src/trio/_core/_tests/test_guest_mode.py:67:17: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `_WithException[Unknown, <class 'Failed'>]`
- error[invalid-argument-type] src/trio/_core/_tests/test_guest_mode.py:77:17: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `_WithException[Unknown, <class 'Failed'>]`
- error[call-non-callable] src/trio/_core/_tests/test_guest_mode.py:192:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] src/trio/_tests/pytest_plugin.py:52:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_exports.py:166:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_exports.py:179:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_exports.py:205:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_exports.py:280:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_file_io.py:109:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_file_io.py:115:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_subprocess.py:596:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_threads.py:270:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[invalid-attribute-access] src/trio/_tests/test_threads.py:483:9: Cannot assign to instance attribute `ran` from the class object `<class 'state'>`
- error[invalid-attribute-access] src/trio/_tests/test_threads.py:484:9: Cannot assign to instance attribute `high_water` from the class object `<class 'state'>`
- error[invalid-attribute-access] src/trio/_tests/test_threads.py:485:9: Cannot assign to instance attribute `running` from the class object `<class 'state'>`
- error[invalid-attribute-access] src/trio/_tests/test_threads.py:486:9: Cannot assign to instance attribute `parked` from the class object `<class 'state'>`
- error[unresolved-attribute] src/trio/_tests/test_threads.py:494:17: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:495:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[invalid-attribute-access] src/trio/_tests/test_threads.py:496:17: Cannot assign to instance attribute `high_water` from the class object `<class 'state'>`
- error[unresolved-attribute] src/trio/_tests/test_threads.py:496:40: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:496:58: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:499:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:502:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:503:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:539:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:545:16: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:554:16: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:555:16: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[call-non-callable] src/trio/_tests/test_timeouts.py:119:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 841 diagnostics
+ Found 811 diagnostics

rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-argument-type] discord/asset.py:174:39: Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | None | Any | under_cached_property[Unknown]`
- error[no-matching-overload] discord/asset.py:419:19: No overload of function `splitext` matches arguments
- error[no-matching-overload] discord/asset.py:504:19: No overload of function `splitext` matches arguments
- error[unresolved-attribute] discord/gateway.py:378:69: Attribute `COMPRESSION_TYPE` can only be accessed on instances, not on the class object `type[_DecompressionContext]` itself.
- error[non-subscriptable] discord/utils.py:893:20: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- warning[possibly-unbound-attribute] discord/utils.py:894:24: Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 553 diagnostics
+ Found 547 diagnostics
-     memo fields = ~207MB
+     memo fields = ~189MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[unresolved-attribute] strawberry/ext/mypy_plugin.py:340:26: Attribute `VERSION` can only be accessed on instances, not on the class object `<class 'MypyVersion'>` itself.
- error[invalid-attribute-access] strawberry/ext/mypy_plugin.py:619:9: Cannot assign to instance attribute `VERSION` from the class object `<class 'MypyVersion'>`
- error[invalid-attribute-access] strawberry/ext/mypy_plugin.py:621:9: Cannot assign to instance attribute `VERSION` from the class object `<class 'MypyVersion'>`
- Found 363 diagnostics
+ Found 360 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[non-subscriptable] gridfs/asynchronous/grid_file.py:1282:32: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] gridfs/asynchronous/grid_file.py:1287:24: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] gridfs/synchronous/grid_file.py:1270:32: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] gridfs/synchronous/grid_file.py:1275:24: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] pymongo/asynchronous/mongo_client.py:2577:16: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] pymongo/synchronous/mongo_client.py:2567:16: Cannot subscript object of type `None` with no `__getitem__` method
- Found 455 diagnostics
+ Found 449 diagnostics

pylox (https://github.com/sco1/pylox)
-     memo fields = ~49MB
+     memo fields = ~54MB

cloud-init (https://github.com/canonical/cloud-init)
- error[call-non-callable] tests/integration_tests/bugs/test_lp1835584.py:54:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/integration_tests/bugs/test_lp1835584.py:79:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/bugs/test_lp1835584.py:85:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:73:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:312:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:316:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:321:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:491:9: Object of type `_WithException[Unknown, <class 'Exit'>]` is not callable
- error[call-non-callable] tests/integration_tests/conftest.py:505:9: Object of type `_WithException[Unknown, <class 'Exit'>]` is not callable
- error[call-non-callable] tests/integration_tests/datasources/test_ec2_ipv6.py:16:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_ntp_servers.py:50:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_set_password.py:172:13: Object of type `_WithException[Unknown, <class 'XFailed'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_ubuntu_drivers.py:52:9: Object of type `_WithException[Unknown, <class 'XFailed'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_user_events.py:37:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_user_events.py:51:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_user_events.py:74:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/modules/test_user_events.py:87:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/net/test_dhcp.py:86:17: Object of type `_WithException[Unknown, <class 'XFailed'>]` is not callable
- error[call-non-callable] tests/integration_tests/test_upgrade.py:64:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/test_upgrade.py:187:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/integration_tests/test_upgrade.py:189:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/test_upgrade.py:209:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/util.py:562:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/util.py:569:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/integration_tests/util.py:571:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/unittests/analyze/test_dump.py:103:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/unittests/config/test_cc_set_passwords.py:303:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/unittests/config/test_cc_ubuntu_pro.py:628:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/unittests/reporting/test_webhook_handler.py:60:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/unittests/reporting/test_webhook_handler.py:118:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/unittests/test_conftest.py:26:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tools/test_tools.py:16:5: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tools/test_tools.py:32:5: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tools/test_tools.py:35:5: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tools/test_tools.py:42:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 713 diagnostics
+ Found 678 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     memo fields = ~171MB
+     memo fields = ~189MB

poetry (https://github.com/python-poetry/poetry)
- error[call-non-callable] tests/installation/test_chooser.py:60:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/masonry/builders/test_editable_builder.py:291:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/repositories/test_legacy_repository.py:163:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/repositories/test_pypi_repository.py:248:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/utils/env/test_env.py:324:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/utils/env/test_env.py:338:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/utils/test_cache.py:360:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/utils/test_isolated_build.py:130:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/utils/test_password_manager.py:383:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 948 diagnostics
+ Found 939 diagnostics

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

apprise (https://github.com/caronc/apprise)
- error[call-non-callable] test/test_plugin_dbus.py:48:5: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_plugin_gnome.py:345:15: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_plugin_macosx.py:46:5: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_plugin_mqtt.py:51:15: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_plugin_ses.py:45:11: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_plugin_syslog.py:42:5: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- Found 4305 diagnostics
+ Found 4299 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[unresolved-attribute] mkdocs/config/base.py:277:16: Attribute `_schema` can only be accessed on instances, not on the class object `type[Config]` itself.
- error[unresolved-attribute] mkdocs/config/defaults.py:218:12: Attribute `_schema` can only be accessed on instances, not on the class object `<class 'MkDocsConfig'>` itself.
- Found 191 diagnostics
+ Found 189 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- error[invalid-argument-type] dragonchain/transaction_processor/level_1_actions.py:181:54: Argument to function `set_current_block_level_sync` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_1_actions.py:181:54: Argument to function `set_current_block_level_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_1_actions.py:182:59: Argument to function `schedule_block_for_broadcast_sync` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_1_actions.py:182:59: Argument to function `schedule_block_for_broadcast_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_2_actions.py:107:42: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_2_actions.py:107:42: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_3_actions.py:144:45: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_3_actions.py:144:45: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_3_actions.py:149:63: Argument to function `get_registration` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_3_actions.py:149:63: Argument to function `get_registration` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_4_actions.py:143:42: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_4_actions.py:143:42: Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_5_actions.py:140:27: Argument to function `set_last_block_number` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_5_actions.py:140:27: Argument to function `set_last_block_number` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/transaction_processor/level_5_actions.py:196:65: Argument to function `put_document` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/transaction_processor/level_5_actions.py:196:65: Argument to function `put_document` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:82:120: Argument to function `verification_storage_location` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:82:120: Argument to function `verification_storage_location` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:89:79: Argument to function `add_receipt` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:89:79: Argument to function `add_receipt` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:89:86: Argument to function `add_receipt` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:89:86: Argument to function `add_receipt` is incorrect: Expected `str`, found `str | Unknown | None`
- error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:93:124: Argument to function `set_receieved_verification_for_block_from_chain_sync` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/webserver/lib/dragonnet.py:93:124: Argument to function `set_receieved_verification_for_block_from_chain_sync` is incorrect: Expected `str`, found `str | Unknown | None`

boostedblob (https://github.com/hauntsaninja/boostedblob)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

jinja (https://github.com/pallets/jinja)
- error[invalid-attribute-access] src/jinja2/environment.py:1666:1: Cannot assign to instance attribute `template_class` from the class object `<class 'Environment'>`
- error[invalid-attribute-access] src/jinja2/nativetypes.py:130:1: Cannot assign to instance attribute `template_class` from the class object `<class 'NativeEnvironment'>`
- Found 199 diagnostics
+ Found 197 diagnostics
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

colour (https://github.com/colour-science/colour)
- error[call-non-callable] colour/recovery/tests/test_jakob2019.py:168:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] colour/recovery/tests/test_jakob2019.py:332:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] colour/recovery/tests/test_mallett2019.py:98:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 491 diagnostics
+ Found 488 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[unresolved-attribute] schema_salad/jsonld_context.py:95:27: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/jsonld_context.py:95:37: Attribute `Class` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/jsonld_context.py:161:34: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/jsonld_context.py:161:44: Attribute `Property` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/jsonld_context.py:162:34: Attribute `domain` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/jsonld_context.py:171:35: Attribute `subClassOf` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:274:49: Attribute `range` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:327:50: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:327:60: Attribute `Property` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:329:50: Attribute `subPropertyOf` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:332:50: Attribute `range` can only be accessed on instances, not on the class object `<class 'RDFS'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:334:50: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] schema_salad/ref_resolver.py:334:60: Attribute `ObjectProperty` can only be accessed on instances, not on the class object `<class 'OWL'>` itself.
- Found 157 diagnostics
+ Found 144 diagnostics
-     memo metadata = ~13MB
+     memo metadata = ~11MB

freqtrade (https://github.com/freqtrade/freqtrade)
- error[invalid-attribute-access] freqtrade/optimize/hyperopt/hyperopt_interface.py:41:9: Cannot assign to instance attribute `timeframe` from the class object `<class 'IHyperOpt'>`
- error[invalid-attribute-access] freqtrade/optimize/hyperopt/hyperopt_interface.py:217:9: Cannot assign to instance attribute `timeframe` from the class object `<class 'IHyperOpt'>`
- error[invalid-attribute-access] freqtrade/plot/plotting.py:639:5: Cannot assign to instance attribute `dp` from the class object `<class 'IStrategy'>`
- Found 400 diagnostics
+ Found 397 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[unsupported-operator] src/urllib3/response.py:798:39: Unary operator `-` is unsupported for type `bytes | int`
- error[call-non-callable] test/__init__.py:185:17: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:120:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:257:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:273:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:335:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:342:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:349:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/conftest.py:359:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/contrib/emscripten/test_emscripten.py:14:5: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/contrib/emscripten/test_emscripten.py:980:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/contrib/emscripten/test_emscripten.py:1166:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/contrib/test_pyopenssl.py:26:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/contrib/test_pyopenssl_dependencies.py:19:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/test_connectionpool.py:414:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] test/tz_stub.py:26:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:79:20: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:83:20: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:88:20: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:99:20: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:751:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:803:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:815:21: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:820:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:822:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:851:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:853:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:868:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:886:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:919:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:998:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:1128:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:1229:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:1250:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_https.py:1255:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_proxy_poolmanager.py:637:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_proxy_poolmanager.py:885:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_socketlevel.py:1347:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_socketlevel.py:1399:17: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_socketlevel.py:2200:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] test/with_dummyserver/test_socketlevel.py:2613:17: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- Found 479 diagnostics
+ Found 438 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[non-subscriptable] src/_pytest/junitxml.py:125:21: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] src/_pytest/junitxml.py:127:12: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] src/_pytest/junitxml.py:128:33: Cannot subscript object of type `None` with no `__getitem__` method
+ error[type-assertion-failure] testing/typing_checks.py:60:5: Argument does not have asserted type `Literal["setup", "call", "teardown"]`
+ error[type-assertion-failure] testing/typing_checks.py:61:5: Argument does not have asserted type `tuple[str, int | None, str]`
- Found 537 diagnostics
+ Found 542 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] scrapy/contracts/__init__.py:97:28: Attribute `name` can only be accessed on instances, not on the class object `type[Contract]` itself.
- error[unresolved-attribute] scrapy/spiderloader.py:85:25: Attribute `name` can only be accessed on instances, not on the class object `type[Spider]` itself.
- error[unresolved-attribute] scrapy/spiderloader.py:86:27: Attribute `name` can only be accessed on instances, not on the class object `type[Spider]` itself.
- error[unresolved-attribute] scrapy/utils/url.py:55:15: Attribute `name` can only be accessed on instances, not on the class object `type[Spider]` itself.
- error[invalid-attribute-access] tests/test_feedexport.py:861:13: Cannot assign to instance attribute `start_urls` from the class object `type[Spider]`
- error[invalid-attribute-access] tests/test_feedexport.py:1869:13: Cannot assign to instance attribute `start_urls` from the class object `type[Spider]`
- error[invalid-attribute-access] tests/test_feedexport.py:2369:9: Cannot assign to instance attribute `start_urls` from the class object `type[Spider]`
- error[invalid-attribute-access] tests/test_feedexport.py:2765:9: Cannot assign to instance attribute `start_urls` from the class object `<class 'TestSpider'>`
- error[unresolved-attribute] tests/test_spiderloader/__init__.py:115:16: Attribute `name` can only be accessed on instances, not on the class object `type[Spider]` itself.
- error[unresolved-attribute] tests/test_spiderloader/__init__.py:131:16: Attribute `name` can only be accessed on instances, not on the class object `type[Spider]` itself.
- Found 1158 diagnostics
+ Found 1148 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[call-non-callable] tests/test_io.py:216:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_io.py:290:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_io.py:1590:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_series.py:2562:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_series.py:2682:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_series.py:2691:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_series.py:2744:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_series.py:2753:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- Found 2779 diagnostics
+ Found 2771 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-return-type] examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: expected `HTTPFlow`, found `@Todo(Type::Intersection.call()) | (@Todo(Type::Intersection.call()) & HTTPFlow) | None`
+ error[invalid-return-type] examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: expected `HTTPFlow`, found `(@Todo(Type::Intersection.call()) & HTTPFlow) | None`
- error[invalid-attribute-access] examples/contrib/upstream_pac.py:47:17: Cannot assign to instance attribute `pac_file` from the class object `<class 'UpstreamPac'>`
- error[invalid-attribute-access] examples/contrib/upstream_pac.py:50:17: Cannot assign to instance attribute `pac_file` from the class object `<class 'UpstreamPac'>`
- error[unresolved-attribute] examples/contrib/upstream_pac.py:59:20: Attribute `pac_file` can only be accessed on instances, not on the class object `<class 'UpstreamPac'>` itself.
- error[unresolved-attribute] examples/contrib/upstream_pac.py:66:12: Attribute `pac_file` can only be accessed on instances, not on the class object `<class 'UpstreamPac'>` itself.
- error[unresolved-attribute] examples/contrib/upstream_pac.py:67:21: Attribute `pac_file` can only be accessed on instances, not on the class object `<class 'UpstreamPac'>` itself.
- Found 1818 diagnostics
+ Found 1813 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[possibly-unbound-attribute] static_frame/core/container_util.py:1686:31: Attribute `_IMMUTABLE_CONSTRUCTOR` on type `type[IndexBase] | type[IMTOAdapter] | type[Unknown]` is possibly unbound
- warning[possibly-unbound-attribute] static_frame/core/container_util.py:1688:31: Attribute `_MUTABLE_CONSTRUCTOR` on type `type[IndexBase] | type[IMTOAdapter] | type[Unknown]` is possibly unbound
- warning[unused-ignore-comment] static_frame/core/db_util.py:459:67: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/db_util.py:469:72: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/db_util.py:513:66: Unused blanket `type: ignore` directive
- error[invalid-attribute-access] static_frame/core/index.py:1669:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'Index'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:361:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexYear'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:450:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexYearMonth'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:530:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexDate'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:547:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexHour'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:564:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexMinute'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:581:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexSecond'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:598:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexMillisecond'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:615:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexMicrosecond'>`
- error[invalid-attribute-access] static_frame/core/index_datetime.py:632:1: Cannot assign to instance attribute `_MUTABLE_CONSTRUCTOR` from the class object `<class 'IndexNanosecond'>`
- error[unresolved-attribute] static_frame/core/index_hierarchy.py:371:43: Type `<class 'IndexNanosecond'> | <class 'Index'>` has no attribute `_MUTABLE_CONSTRUCTOR`
+ error[invalid-argument-type] static_frame/core/reduce.py:310:54: Argument to function `ufunc_dtype_to_dtype` is incorrect: Expected `dtype[Any]`, found `Unknown | ndarray[@Todo(Support for `typing.TypeAlias`), dtype[object_]]`
- error[unresolved-attribute] static_frame/test/unit/test_index.py:623:26: Attribute `_MUTABLE_CONSTRUCTOR` can only be accessed on instances, not on the class object `<class 'Index'>` itself.
+ error[unsupported-operator] static_frame/test/unit/test_index.py:488:27: Operator `+` is unsupported between objects of type `Index[Any]` and `Literal[2]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:489:27: Operator `+` is unsupported between objects of type `Literal[2]` and `Index[Any]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:490:27: Operator `*` is unsupported between objects of type `Index[Any]` and `Literal[2]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:491:27: Operator `*` is unsupported between objects of type `Literal[2]` and `Index[Any]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:492:27: Operator `-` is unsupported between objects of type `Index[Any]` and `Literal[2]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:493:27: Operator `-` is unsupported between objects of type `Literal[2]` and `Index[Any]`
+ error[unresolved-attribute] static_frame/test/unit/test_index.py:500:26: Type `bool` has no attribute `tolist`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:505:26: Operator `@` is unsupported between objects of type `Index[Any]` and `Index[Any]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:510:14: Operator `*` is unsupported between objects of type `IndexGO[Any]` and `Literal[2]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:522:27: Operator `+` is unsupported between objects of type `Index[Any]` and `Literal["_"]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:523:27: Operator `+` is unsupported between objects of type `Literal["_"]` and `Index[Any]`
+ error[unsupported-operator] static_frame/test/unit/test_index.py:524:27: Operator `*` is unsupported between objects of type `Index[Any]` and `Literal[3]`
- error[unresolved-attribute] static_frame/test/unit/test_index_datetime.py:53:30: Type `<class 'IndexYear'> | <class 'IndexYearMonth'> | <class 'IndexDate'> | <class 'IndexMinute'> | <class 'IndexSecond'> | <class 'IndexMillisecond'> | <class 'IndexNanosecond'>` has no attribute `_MUTABLE_CONSTRUCTOR`
+ error[unsupported-operator] static_frame/test/unit/test_index_hierarchy.py:2381:51: Operator `*` is unsupported between objects of type `Index[Any]` and `Literal[2]`
+ error[unsupported-operator] static_frame/test/unit/test_index_hierarchy.py:2381:61: Operator `*` is unsupported between objects of type `Index[Any]` and `Literal[2]`
- Found 1806 diagnostics
+ Found 1803 diagnostics
-     memo fields = ~304MB
+     memo fields = ~334MB

psycopg (https://github.com/psycopg/psycopg)
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:144:42: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:152:12: Attribute `oid` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:153:45: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:154:38: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:155:21: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:157:42: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:159:34: Attribute `format` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:159:49: Attribute `oid` on type `type[Dumper] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] psycopg/psycopg/_adapters_map.py:180:42: Attribute `format` on type `type[Loader] | Unknown` is possibly unbound
- error[call-non-callable] tests/_test_cursor.py:41:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/crdb/test_cursor.py:76:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/crdb/test_cursor_async.py:77:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_db.py:74:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:94:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:190:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_db.py:213:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:236:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:320:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_db.py:326:17: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:331:17: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:340:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_db.py:365:9: Object of type `_WithException[Unknown, <class 'Exit'>]` is not callable
- error[call-non-callable] tests/fix_dns.py:45:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_faker.py:387:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_gc.py:75:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_pq.py:47:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_pq.py:62:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_pq.py:86:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_proxy.py:92:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/fix_psycopg.py:47:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/fix_psycopg.py:51:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/pool/test_pool.py:976:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pool/test_pool_async.py:979:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pool/test_pool_common.py:683:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pool/test_pool_common_async.py:694:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pool/test_pool_null.py:487:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pool/test_pool_null_async.py:489:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pq/test_pgconn.py:45:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/pq/test_pgconn.py:660:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/pq/test_pgconn.py:664:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/test_concurrency.py:370:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_concurrency_async.py:283:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_connection_info.py:23:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_cursor_common.py:184:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_cursor_common_async.py:183:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_dns.py:23:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_generators.py:19:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_generators.py:178:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_gevent.py:62:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_gevent.py:77:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_sql.py:409:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_tpc.py:16:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_tpc_async.py:13:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_transaction.py:93:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_transaction_async.py:92:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/test_typing.py:282:9: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- error[call-non-callable] tests/test_waiting.py:132:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/types/test_composite.py:155:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/types/test_json.py:67:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/types/test_range.py:263:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/types/test_shapely.py:83:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] tests/utils.py:89:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 620 diagnostics
+ Found 558 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[call-non-callable] tests/test_examples.py:1565:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[unresolved-attribute] tests/test_provenance.py:421:30: Attribute `conformsTo` can only be accessed on instances, not on the class object `<class 'DCTERMS'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:489:17: Attribute `conformsTo` can only be accessed on instances, not on the class object `<class 'DCTERMS'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:538:27: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:548:30: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:560:9: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:565:31: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:568:29: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:581:34: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:583:29: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:598:37: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:605:41: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:643:38: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:647:24: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:648:24: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:649:24: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:654:32: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:661:32: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:662:32: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:672:29: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:675:28: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:676:28: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- error[unresolved-attribute] tests/test_provenance.py:678:38: Attribute `type` can only be accessed on instances, not on the class object `<class 'RDF'>` itself.
- Found 161 diagnostics
+ Found 138 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
-     memo metadata = ~17MB
+     memo metadata = ~15MB

aiohttp (https://github.com/aio-libs/aiohttp)
- error[invalid-argument-type] aiohttp/client_reqrep.py:961:43: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
- error[invalid-argument-type] aiohttp/client_reqrep.py:961:59: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
- warning[possibly-unbound-attribute] aiohttp/connector.py:1393:12: Attribute `endswith` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- warning[possibly-unbound-attribute] aiohttp/connector.py:1394:20: Attribute `rstrip` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- warning[possibly-unbound-attribute] aiohttp/connector.py:1415:17: Attribute `rstrip` on type `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~None) | under_cached_property[Unknown] | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
- error[invalid-argument-type] aiohttp/cookiejar.py:237:47: Argument to function `is_ip_address` is incorrect: Expected `str | None`, found `Unknown | under_cached_property[Unknown]`
- warning[possibly-unbound-attribute] aiohttp/cookiejar.py:276:24: Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- error[non-subscriptable] aiohttp/cookiejar.py:280:34: Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- warning[possibly-unbound-attribute] aiohttp/cookiejar.py:280:43: Attribute `rfind` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- error[invalid-argument-type] aiohttp/cookiejar.py:350:26: Argument to function `is_ip_address` is incorrect: Expected `str | None`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
- warning[possibly-unbound-attribute] aiohttp/cookiejar.py:361:38: Attribute `split` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- error[invalid-argument-type] aiohttp/cookiejar.py:365:24: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | under_cached_property[Unknown]`
- error[invalid-argument-type] aiohttp/helpers.py:199:43: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
- error[invalid-argument-type] aiohttp/helpers.py:199:59: Argument to function `__new__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (under_cached_property[Unknown] & ~AlwaysFalsy) | Literal[""]`
- error[invalid-argument-type] aiohttp/helpers.py:310:46: Argument to function `proxy_bypass` is incorrect: Expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown]`
- error[call-non-callable] aiohttp/helpers.py:315:22: Method `__getitem__` of type `bound method dict[str, ProxyInfo].__getitem__(key: str, /) -> ProxyInfo` is not callable on object of type `dict[str, ProxyInfo]`
- warning[possibly-unbound-attribute] aiohttp/web_urldispatcher.py:814:55: Attribute `split` on type `(Unknown & ~None) | under_cached_property[Unknown]` is possibly unbound
- error[invalid-return-type] aiohttp/web_urldispatcher.py:817:20: Return type does not match returned value: expected `str`, found `(Unknown & ~None) | under_cached_property[Unknown]`
- error[invalid-return-type] aiohttp/web_urldispatcher.py:1260:12: Return type does not match returned value: expected `str`, found `Unknown | under_cached_property[Unknown]`
- Found 197 diagnostics
+ Found 178 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

streamlit (https://github.com/streamlit/streamlit)
- error[call-non-callable] lib/tests/conftest.py:105:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] lib/tests/conftest.py:111:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] lib/tests/streamlit/navigation/page_test.py:55:13: Object of type `_WithException[Unknown, <class 'Failed'>]` is not callable
- Found 3307 diagnostics
+ Found 3304 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[unresolved-attribute] src/prefect/cli/root.py:136:20: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/logging/loggers.py:245:42: Attribute `type` can only be accessed on instances, not on the class object `type[BaseWorker]` itself.
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:230:21: Attribute `start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:230:45: Attribute `expected_start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:231:44: Attribute `start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:232:46: Attribute `expected_start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:235:21: Attribute `start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:237:24: Attribute `expected_start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/flow_runs.py:239:46: Attribute `expected_start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- error[unresolved-attribute] src/prefect/server/api/run_history.py:74:17: Type `type[FlowRun] | type[TaskRun]` has no attribute `expected_start_time`
- error[unresolved-attribute] src/prefect/server/api/run_history.py:78:17: Type `type[FlowRun] | type[TaskRun]` has no attribute `state_name`
- error[unresolved-attribute] src/prefect/server/api/server.py:712:9: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/flow_runs.py:64:9: Attribute `start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/flow_runs.py:65:9: Attribute `expected_start_time` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/flow_runs.py:69:9: Attribute `state_timestamp` on type `type[FlowRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/task_runs.py:92:39: Attribute `start_time` on type `type[TaskRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/task_runs.py:102:37: Attribute `end_time` on type `type[TaskRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/api/ui/task_runs.py:130:33: Attribute `start_time` on type `type[TaskRun] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:69:24: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:79:9: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:116:25: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:119:35: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:147:25: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:150:35: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:169:8: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/database/_migrations/env.py:176:8: Attribute `name` on type `Unknown | type[Dialect]` is possibly unbound
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:95:12: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:97:14: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:102:34: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:108:12: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:110:14: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:115:26: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:121:12: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:123:14: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/dependencies.py:128:26: Attribute `name` can only be accessed on instances, not on the class object `type[Dialect]` itself.
- error[unresolved-attribute] src/prefect/server/database/orm_models.py:1531:37: Attribute `name` can only be access...*[Comment body truncated]*

---

_@sharkdp reviewed on 2025-07-01 18:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1242 on 2025-07-01 18:36_

This is a pre-existing problem on main that just surfaces here in a somewhat confusing way:
```py
from typing import ClassVar, Any

def _(flag: bool):
    class Base:
        x: ClassVar[Any]

    class Derived(Base):
        if flag:
            x: ClassVar[int]

    reveal_type(Derived.x)  # ty (main and this branch): int
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:542 on 2025-07-01 18:36_

@AlexWaygood I would appreciate if you could take a look if this looks correct

---

_@sharkdp reviewed on 2025-07-01 18:36_

---

_@sharkdp reviewed on 2025-07-01 18:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:83 on 2025-07-01 18:39_

I'm not sure if it's valuable to keep this test and write a TODO. We do not emit `invalid-attribute-access` here anymore because of the missing union handling (see my other comment). But also, the main behavior is tested above, and the definition of `a: int` on `Derived` should be a inconsistent-overwrite anyway.

---

_Marked ready for review by @sharkdp on 2025-07-01 18:39_

---

_Review requested from @carljm by @sharkdp on 2025-07-01 18:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-01 18:39_

---

_Review requested from @dcreager by @sharkdp on 2025-07-01 18:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-01 18:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-01 18:39_

---

_@AlexWaygood reviewed on 2025-07-01 18:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:542 on 2025-07-01 18:54_

hmm... no, I don't think it is correct :/

Mutable attribute members on protocols are invariant, so a type should only be assignable to `HasX` if it has an `x` attribute where the attribute type is assignable to `int` _and_ `int` is assignable to the attribute type. Here, `SubclassOfAny` (the type of `FooSubclassOfAny.x`) is assignable to `int`, but `int` is not assignable to `SubclassOfAny`. That means that the requirement of the `x` protocol member is not satisfied, so `FooSubclassOfAny` should not be considered assignable to `HasX`

---

_@sharkdp reviewed on 2025-07-02 08:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:542 on 2025-07-02 08:40_

What's happening here is the following: previously, we treated `x: SubclassOfAny` as a pure instance attribute. Accessing `x` on `FooSubclassOfAny` did not invoke the descriptor protocol because `x` was not accessible on the class.

With the change here, we now treat that declared-only `x` as being accessible on the class. Consequently, we also invoke the descriptor protocol. Accessing `__get__` on `SubclassOfAny` yields `Any`. Calling `__get__` also yields `Any`. So accessing `x` on instances of `FooSubclassOfAny` yields `Any`. And `Any` is assignable to `int` and vice versa.

This is an existing behavior on `main` as well. When you add a right-hand side (`x: SubclassOfAny = SubclassOfAny()`), that attribute becomes accessible on the class. And the assignability test yields true: https://play.ty.dev/9920e738-07ab-4baa-979e-cc3db413475b


So in that sense, I think the behavior here *is* correct?

---

_@AlexWaygood reviewed on 2025-07-02 08:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:542 on 2025-07-02 08:44_

Wow, I see! Okay, that does make sense. Could you possibly distill that explanation into a comment above this assertion? 😃

---

_@AlexWaygood approved on 2025-07-02 15:59_

it still makes me a little sad that we have to do this -- but, needs must!

---

_Merged by @sharkdp on 2025-07-02 16:03_

---

_Closed by @sharkdp on 2025-07-02 16:03_

---

_Branch deleted on 2025-07-02 16:03_

---
