```yaml
number: 20629
title: "[ty] Ensure first-party search paths always appear in a sensible order"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/first-party-search-path-ordering
created_at: 2025-09-29T13:09:31Z
updated_at: 2025-09-29T20:19:16Z
url: https://github.com/astral-sh/ruff/pull/20629
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Ensure first-party search paths always appear in a sensible order

---

_Pull request opened by @AlexWaygood on 2025-09-29 13:09_

This PR ensures that we always put `./src` before `.` in our list of first-party search paths. This better emulates the fact that at runtime, the module name of a file `src/foo.py` would almost certainly be `foo` rather than `src.foo`.

I wondered if fixing this might fix https://github.com/astral-sh/ruff/pull/20603#issuecomment-3345317444. It seems like that's not the case, but it also seems like it leads to better diagnostics because we report much more intuitive module names to the user in our error messages -- so, it's probably a good change anyway.

---

_Label `ty` added by @AlexWaygood on 2025-09-29 13:09_

---

_Comment by @github-actions[bot] on 2025-09-29 13:11_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-29 13:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:110:40: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:110:40: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:159:39: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:159:39: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:271:33: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:271:33: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:320:37: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:320:37: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:385:32: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:385:32: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:477:42: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:477:42: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:647:41: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:647:41: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:800:40: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:800:40: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:870:27: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:870:27: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:945:36: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:945:36: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:1018:36: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:1018:36: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:1132:33: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:1132:33: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`
- tests/test_validators.py:1352:32: error[unresolved-attribute] Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ tests/test_validators.py:1352:32: error[unresolved-attribute] Type `<module 'attr.validators'>` has no attribute `__all__`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/live.py:304:23: error[unresolved-import] Module `src.pip._vendor.rich.live` has no member `Live`
+ src/pip/_vendor/rich/live.py:304:23: error[unresolved-import] Module `pip._vendor.rich.live` has no member `Live`
- src/pip/_vendor/urllib3/connectionpool.py:468:21: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
+ src/pip/_vendor/urllib3/connectionpool.py:468:21: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
- src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:51:29: error[unresolved-import] Module `src.pip._vendor.urllib3.packages.six` has no member `raise_from`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:51:29: error[unresolved-import] Module `pip._vendor.urllib3.packages.six` has no member `raise_from`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:124:24: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:124:24: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:136:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:136:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:138:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:138:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:140:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:140:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:147:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:147:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:149:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:149:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:151:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:151:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:92:24: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:92:24: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:193:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:193:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:195:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:195:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:197:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:197:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:205:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:205:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:207:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:207:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:209:5: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:209:5: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/contrib/securetransport.py:857:23: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.util'>` has no attribute `ssl_`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:857:23: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.util'>` has no attribute `ssl_`
- src/pip/_vendor/urllib3/util/connection.py:68:16: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
+ src/pip/_vendor/urllib3/util/connection.py:68:16: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
- src/pip/_vendor/urllib3/util/url.py:310:13: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
+ src/pip/_vendor/urllib3/util/url.py:310:13: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
- src/pip/_vendor/urllib3/util/url.py:317:13: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
+ src/pip/_vendor/urllib3/util/url.py:317:13: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
- src/pip/_vendor/urllib3/util/url.py:397:16: error[unresolved-attribute] Type `<module 'src.pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`
+ src/pip/_vendor/urllib3/util/url.py:397:16: error[unresolved-attribute] Type `<module 'pip._vendor.urllib3.packages.six'>` has no attribute `raise_from`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/contrib/pyopenssl.py:133:24: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `ssl_`
+ src/urllib3/contrib/pyopenssl.py:133:24: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `ssl_`
- src/urllib3/contrib/pyopenssl.py:147:5: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `ssl_`
+ src/urllib3/contrib/pyopenssl.py:147:5: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `ssl_`
- src/urllib3/contrib/pyopenssl.py:154:5: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `ssl_`
+ src/urllib3/contrib/pyopenssl.py:154:5: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `ssl_`
- src/urllib3/contrib/pyopenssl.py:156:5: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `ssl_`
+ src/urllib3/contrib/pyopenssl.py:156:5: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `ssl_`
- src/urllib3/contrib/pyopenssl.py:497:22: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `util`
+ src/urllib3/contrib/pyopenssl.py:497:22: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `util`
- src/urllib3/contrib/pyopenssl.py:511:36: error[unresolved-attribute] Type `<module 'src.urllib3.util'>` has no attribute `ssl_`
+ src/urllib3/contrib/pyopenssl.py:511:36: error[unresolved-attribute] Type `<module 'urllib3.util'>` has no attribute `ssl_`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/__init__.py:55:1: error[unresolved-attribute] Type `<module 'psycopg.psycopg.types'>` has no attribute `array`
+ psycopg/psycopg/__init__.py:55:1: error[unresolved-attribute] Type `<module 'psycopg.types'>` has no attribute `array`
- psycopg/psycopg/_connection_info.py:50:22: error[unresolved-attribute] Type `<module 'psycopg.psycopg.pq'>` has no attribute `misc`
+ psycopg/psycopg/_connection_info.py:50:22: error[unresolved-attribute] Type `<module 'psycopg.pq'>` has no attribute `misc`
- tests/types/test_numeric.py:589:23: error[unresolved-attribute] Type `<module 'psycopg.psycopg.types'>` has no attribute `numeric`
+ tests/types/test_numeric.py:589:23: error[unresolved-attribute] Type `<module 'psycopg.types'>` has no attribute `numeric`
- tests/types/test_numeric.py:600:23: error[unresolved-attribute] Type `<module 'psycopg.psycopg.types'>` has no attribute `numeric`
+ tests/types/test_numeric.py:600:23: error[unresolved-attribute] Type `<module 'psycopg.types'>` has no attribute `numeric`
- tests/types/test_numeric.py:612:23: error[unresolved-attribute] Type `<module 'psycopg.psycopg.types'>` has no attribute `numeric`
+ tests/types/test_numeric.py:612:23: error[unresolved-attribute] Type `<module 'psycopg.types'>` has no attribute `numeric`
- tests/types/test_string.py:152:40: error[unresolved-attribute] Type `<module 'psycopg.psycopg.types'>` has no attribute `string`
+ tests/types/test_string.py:152:40: error[unresolved-attribute] Type `<module 'psycopg.types'>` has no attribute `string`

pyodide (https://github.com/pyodide/pyodide)
- src/py/pyodide/_package_loader.py:18:30: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsArray`
+ src/py/pyodide/_package_loader.py:18:30: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsArray`
- src/py/pyodide/_package_loader.py:18:39: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsBuffer`
+ src/py/pyodide/_package_loader.py:18:39: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsBuffer`
- src/py/pyodide/_package_loader.py:18:49: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `to_js`
+ src/py/pyodide/_package_loader.py:18:49: error[unresolved-import] Module `py.pyodide.ffi` has no member `to_js`
- src/py/pyodide/_state.py:8:18: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsProxy`
+ src/py/pyodide/_state.py:8:18: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsProxy`
- src/py/pyodide/ffi/wrappers.py:6:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsCallableDoubleProxy`
+ src/py/pyodide/ffi/wrappers.py:6:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsCallableDoubleProxy`
- src/py/pyodide/ffi/wrappers.py:7:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsDomElement`
+ src/py/pyodide/ffi/wrappers.py:7:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsDomElement`
- src/py/pyodide/ffi/wrappers.py:8:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsProxy`
+ src/py/pyodide/ffi/wrappers.py:8:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsProxy`
- src/py/pyodide/ffi/wrappers.py:9:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsWeakRef`
+ src/py/pyodide/ffi/wrappers.py:9:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsWeakRef`
- src/py/pyodide/ffi/wrappers.py:10:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `create_once_callable`
+ src/py/pyodide/ffi/wrappers.py:10:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_once_callable`
- src/py/pyodide/ffi/wrappers.py:11:5: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `create_proxy`
+ src/py/pyodide/ffi/wrappers.py:11:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_proxy`
- src/py/pyodide/http/_exceptions.py:3:19: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsException`
+ src/py/pyodide/http/_exceptions.py:3:19: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsException`
- src/py/pyodide/http/_pyfetch.py:12:31: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsBuffer`
+ src/py/pyodide/http/_pyfetch.py:12:31: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsBuffer`
- src/py/pyodide/http/_pyfetch.py:12:41: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsException`
+ src/py/pyodide/http/_pyfetch.py:12:41: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsException`
- src/py/pyodide/http/_pyfetch.py:12:54: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `JsFetchResponse`
+ src/py/pyodide/http/_pyfetch.py:12:54: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsFetchResponse`
- src/py/pyodide/http/_pyfetch.py:12:71: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `to_js`
+ src/py/pyodide/http/_pyfetch.py:12:71: error[unresolved-import] Module `py.pyodide.ffi` has no member `to_js`
- src/py/pyodide/webloop.py:15:30: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `can_run_sync`
+ src/py/pyodide/webloop.py:15:30: error[unresolved-import] Module `py.pyodide.ffi` has no member `can_run_sync`
- src/py/pyodide/webloop.py:15:44: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `create_once_callable`
+ src/py/pyodide/webloop.py:15:44: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_once_callable`
- src/py/pyodide/webloop.py:15:66: error[unresolved-import] Module `src.py.pyodide.ffi` has no member `run_sync`
+ src/py/pyodide/webloop.py:15:66: error[unresolved-import] Module `py.pyodide.ffi` has no member `run_sync`

trio (https://github.com/python-trio/trio)
- src/trio/_core/__init__.py:81:9: warning[possibly-missing-import] Member `current_iocp` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:81:9: warning[possibly-missing-import] Member `current_iocp` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:82:9: warning[possibly-missing-import] Member `monitor_completion_key` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:82:9: warning[possibly-missing-import] Member `monitor_completion_key` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:83:9: warning[possibly-missing-import] Member `readinto_overlapped` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:83:9: warning[possibly-missing-import] Member `readinto_overlapped` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:84:9: warning[possibly-missing-import] Member `register_with_iocp` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:84:9: warning[possibly-missing-import] Member `register_with_iocp` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:85:9: warning[possibly-missing-import] Member `wait_overlapped` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:85:9: warning[possibly-missing-import] Member `wait_overlapped` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:86:9: warning[possibly-missing-import] Member `write_overlapped` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:86:9: warning[possibly-missing-import] Member `write_overlapped` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:92:23: warning[possibly-missing-import] Member `current_kqueue` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:92:23: warning[possibly-missing-import] Member `current_kqueue` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:92:39: warning[possibly-missing-import] Member `monitor_kevent` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:92:39: warning[possibly-missing-import] Member `monitor_kevent` of module `trio._core._run` may be missing
- src/trio/_core/__init__.py:92:55: warning[possibly-missing-import] Member `wait_kevent` of module `src.trio._core._run` may be missing
+ src/trio/_core/__init__.py:92:55: warning[possibly-missing-import] Member `wait_kevent` of module `trio._core._run` may be missing
- src/trio/_core/_parking_lot.py:101:15: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_exceptions`
+ src/trio/_core/_parking_lot.py:101:15: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_exceptions`
- src/trio/_core/_tests/test_asyncgen.py:200:44: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_entry_queue`
+ src/trio/_core/_tests/test_asyncgen.py:200:44: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_entry_queue`
- src/trio/_core/_tests/test_asyncgen.py:201:18: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_run`
+ src/trio/_core/_tests/test_asyncgen.py:201:18: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_run`
- src/trio/_core/_tests/test_run.py:2341:9: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_run`
+ src/trio/_core/_tests/test_run.py:2341:9: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_run`
- src/trio/_core/_tests/test_run.py:2352:27: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_run`
+ src/trio/_core/_tests/test_run.py:2352:27: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_run`
- src/trio/_tests/test_testing_raisesgroup.py:1106:9: error[unresolved-attribute] Type `<module 'src.trio.testing'>` has no attribute `_raises_group`
+ src/trio/_tests/test_testing_raisesgroup.py:1106:9: error[unresolved-attribute] Type `<module 'trio.testing'>` has no attribute `_raises_group`
- src/trio/_tests/test_testing_raisesgroup.py:1108:9: error[unresolved-attribute] Type `<module 'src.trio.testing'>` has no attribute `_raises_group`
+ src/trio/_tests/test_testing_raisesgroup.py:1108:9: error[unresolved-attribute] Type `<module 'trio.testing'>` has no attribute `_raises_group`
- src/trio/_tests/test_threads.py:407:25: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_thread_cache`
+ src/trio/_tests/test_threads.py:407:25: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_thread_cache`
- src/trio/_tests/test_threads.py:615:25: error[unresolved-attribute] Type `<module 'src.trio._core'>` has no attribute `_thread_cache`
+ src/trio/_tests/test_threads.py:615:25: error[unresolved-attribute] Type `<module 'trio._core'>` has no attribute `_thread_cache`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/__init__.py:6:15: error[unresolved-import] Module `src.prefect` has no member `_build_info`
+ src/prefect/__init__.py:6:15: error[unresolved-import] Module `prefect` has no member `_build_info`
- src/prefect/server/api/artifacts.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `src.prefect.server.schemas.core.Artifact`, found `src.prefect.server.database.orm_models.Artifact`
+ src/prefect/server/api/artifacts.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `prefect.server.schemas.core.Artifact`, found `prefect.server.database.orm_models.Artifact`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/dataspec.py:56:5: error[unresolved-import] Module `src.bokeh.core.property.visual` has no member `CSS_LENGTH_RE`
+ src/bokeh/core/property/dataspec.py:56:5: error[unresolved-import] Module `bokeh.core.property.visual` has no member `CSS_LENGTH_RE`
- src/bokeh/model/model.py:28:40: error[unresolved-import] Module `src.bokeh.core.has_props` has no member `_default_resolver`
+ src/bokeh/model/model.py:28:40: error[unresolved-import] Module `bokeh.core.has_props` has no member `_default_resolver`
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[src.bokeh.model.model.Model]`, found `set[src.bokeh.model.model.Model]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model]`, found `set[bokeh.model.model.Model]`
- src/bokeh/models/__init__.py:111:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.axes'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:111:6: error[unresolved-attribute] Type `<module 'bokeh.models.axes'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:112:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.callbacks'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:112:6: error[unresolved-attribute] Type `<module 'bokeh.models.callbacks'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:113:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.canvas'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:113:6: error[unresolved-attribute] Type `<module 'bokeh.models.canvas'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:114:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.comparisons'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:114:6: error[unresolved-attribute] Type `<module 'bokeh.models.comparisons'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:115:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.coordinates'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:115:6: error[unresolved-attribute] Type `<module 'bokeh.models.coordinates'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:116:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.css'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:116:6: error[unresolved-attribute] Type `<module 'bokeh.models.css'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:117:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.expressions'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:117:6: error[unresolved-attribute] Type `<module 'bokeh.models.expressions'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:118:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.filters'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:118:6: error[unresolved-attribute] Type `<module 'bokeh.models.filters'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:119:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.formatters'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:119:6: error[unresolved-attribute] Type `<module 'bokeh.models.formatters'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:120:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.glyphs'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:120:6: error[unresolved-attribute] Type `<module 'bokeh.models.glyphs'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:121:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.graphs'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:121:6: error[unresolved-attribute] Type `<module 'bokeh.models.graphs'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:122:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.grids'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:122:6: error[unresolved-attribute] Type `<module 'bokeh.models.grids'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:123:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.labeling'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:123:6: error[unresolved-attribute] Type `<module 'bokeh.models.labeling'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:124:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.layouts'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:124:6: error[unresolved-attribute] Type `<module 'bokeh.models.layouts'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:125:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.map_plots'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:125:6: error[unresolved-attribute] Type `<module 'bokeh.models.map_plots'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:126:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.mappers'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:126:6: error[unresolved-attribute] Type `<module 'bokeh.models.mappers'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:128:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.nodes'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:128:6: error[unresolved-attribute] Type `<module 'bokeh.models.nodes'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:129:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.plots'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:129:6: error[unresolved-attribute] Type `<module 'bokeh.models.plots'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:130:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ranges'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:130:6: error[unresolved-attribute] Type `<module 'bokeh.models.ranges'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:132:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.scales'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:132:6: error[unresolved-attribute] Type `<module 'bokeh.models.scales'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:133:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.selections'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:133:6: error[unresolved-attribute] Type `<module 'bokeh.models.selections'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:134:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.selectors'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:134:6: error[unresolved-attribute] Type `<module 'bokeh.models.selectors'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:135:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.sources'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:135:6: error[unresolved-attribute] Type `<module 'bokeh.models.sources'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:136:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.text'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:136:6: error[unresolved-attribute] Type `<module 'bokeh.models.text'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:137:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.textures'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:137:6: error[unresolved-attribute] Type `<module 'bokeh.models.textures'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:138:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.tickers'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:138:6: error[unresolved-attribute] Type `<module 'bokeh.models.tickers'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:139:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.tiles'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:139:6: error[unresolved-attribute] Type `<module 'bokeh.models.tiles'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:140:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.tools'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:140:6: error[unresolved-attribute] Type `<module 'bokeh.models.tools'>` has no attribute `__all__`
- src/bokeh/models/__init__.py:141:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.transforms'>` has no attribute `__all__`
+ src/bokeh/models/__init__.py:141:6: error[unresolved-attribute] Type `<module 'bokeh.models.transforms'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:45:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.annotation'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:45:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.annotation'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:46:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.arrows'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:46:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.arrows'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:47:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.dimensional'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:47:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.dimensional'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:48:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.geometry'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:48:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.geometry'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:50:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.html.labels'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:50:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.html.labels'>` has no attribute `__all__`
- src/bokeh/models/annotations/__init__.py:51:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.legends'>` has no attribute `__all__`
+ src/bokeh/models/annotations/__init__.py:51:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.legends'>` has no attribute `__all__`
- src/bokeh/models/annotations/html/__init__.py:33:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.html.html_annotation'>` has no attribute `__all__`
+ src/bokeh/models/annotations/html/__init__.py:33:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.html.html_annotation'>` has no attribute `__all__`
- src/bokeh/models/annotations/html/__init__.py:34:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.html.labels'>` has no attribute `__all__`
+ src/bokeh/models/annotations/html/__init__.py:34:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.html.labels'>` has no attribute `__all__`
- src/bokeh/models/annotations/html/__init__.py:35:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.annotations.html.toolbars'>` has no attribute `__all__`
+ src/bokeh/models/annotations/html/__init__.py:35:6: error[unresolved-attribute] Type `<module 'bokeh.models.annotations.html.toolbars'>` has no attribute `__all__`
- src/bokeh/models/axes.py:54:5: error[unresolved-import] Module `src.bokeh.models.formatters` has no member `CONTEXTUAL_DATETIME_FORMATTER`
+ src/bokeh/models/axes.py:54:5: error[unresolved-import] Module `bokeh.models.formatters` has no member `CONTEXTUAL_DATETIME_FORMATTER`
- src/bokeh/models/axes.py:55:5: error[unresolved-import] Module `src.bokeh.models.formatters` has no member `CONTEXTUAL_TIMEDELTA_FORMATTER`
+ src/bokeh/models/axes.py:55:5: error[unresolved-import] Module `bokeh.models.formatters` has no member `CONTEXTUAL_TIMEDELTA_FORMATTER`
- src/bokeh/models/axes.py:72:5: error[unresolved-import] Module `src.bokeh.models.tickers` has no member `TimedeltaTicker`
+ src/bokeh/models/axes.py:72:5: error[unresolved-import] Module `bokeh.models.tickers` has no member `TimedeltaTicker`
- src/bokeh/models/glyphs.pyi:18:5: error[unresolved-import] Module `src.bokeh._specs` has no member `NonNegative`
+ src/bokeh/models/glyphs.pyi:18:5: error[unresolved-import] Module `bokeh._specs` has no member `NonNegative`
- src/bokeh/models/mappers.pyi:17:21: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `Glyph`
+ src/bokeh/models/mappers.pyi:17:21: error[unresolved-import] Module `bokeh.models.glyphs` has no member `Glyph`
- src/bokeh/models/misc/__init__.py:30:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.misc.group_by'>` has no attribute `__all__`
+ src/bokeh/models/misc/__init__.py:30:6: error[unresolved-attribute] Type `<module 'bokeh.models.misc.group_by'>` has no attribute `__all__`
- src/bokeh/models/nodes.pyi:16:21: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `Glyph`
+ src/bokeh/models/nodes.pyi:16:21: error[unresolved-import] Module `bokeh.models.glyphs` has no member `Glyph`
- src/bokeh/models/plots.py:75:21: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `Glyph`
+ src/bokeh/models/plots.py:75:21: error[unresolved-import] Module `bokeh.models.glyphs` has no member `Glyph`
- src/bokeh/models/renderers/__init__.py:41:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.renderers.contour_renderer'>` has no attribute `__all__`
+ src/bokeh/models/renderers/__init__.py:41:6: error[unresolved-attribute] Type `<module 'bokeh.models.renderers.contour_renderer'>` has no attribute `__all__`
- src/bokeh/models/renderers/__init__.py:42:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.renderers.glyph_renderer'>` has no attribute `__all__`
+ src/bokeh/models/renderers/__init__.py:42:6: error[unresolved-attribute] Type `<module 'bokeh.models.renderers.glyph_renderer'>` has no attribute `__all__`
- src/bokeh/models/renderers/__init__.py:43:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.renderers.graph_renderer'>` has no attribute `__all__`
+ src/bokeh/models/renderers/__init__.py:43:6: error[unresolved-attribute] Type `<module 'bokeh.models.renderers.graph_renderer'>` has no attribute `__all__`
- src/bokeh/models/renderers/__init__.py:44:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.renderers.renderer'>` has no attribute `__all__`
+ src/bokeh/models/renderers/__init__.py:44:6: error[unresolved-attribute] Type `<module 'bokeh.models.renderers.renderer'>` has no attribute `__all__`
- src/bokeh/models/renderers/__init__.py:45:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.renderers.tile_renderer'>` has no attribute `__all__`
+ src/bokeh/models/renderers/__init__.py:45:6: error[unresolved-attribute] Type `<module 'bokeh.models.renderers.tile_renderer'>` has no attribute `__all__`
- src/bokeh/models/renderers/glyph_renderer.py:191:13: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `FillGlyph`
+ src/bokeh/models/renderers/glyph_renderer.py:191:13: error[unresolved-import] Module `bokeh.models.glyphs` has no member `FillGlyph`
- src/bokeh/models/renderers/glyph_renderer.py:194:13: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `LineGlyph`
+ src/bokeh/models/renderers/glyph_renderer.py:194:13: error[unresolved-import] Module `bokeh.models.glyphs` has no member `LineGlyph`
- src/bokeh/models/renderers/glyph_renderer.py:195:13: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `TextGlyph`
+ src/bokeh/models/renderers/glyph_renderer.py:195:13: error[unresolved-import] Module `bokeh.models.glyphs` has no member `TextGlyph`
- src/bokeh/models/renderers/glyph_renderer.pyi:20:22: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `Glyph`
+ src/bokeh/models/renderers/glyph_renderer.pyi:20:22: error[unresolved-import] Module `bokeh.models.glyphs` has no member `Glyph`
- src/bokeh/models/tools.pyi:56:5: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `XYGlyph`
+ src/bokeh/models/tools.pyi:56:5: error[unresolved-import] Module `bokeh.models.glyphs` has no member `XYGlyph`
- src/bokeh/models/ui/__init__.py:48:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.dialogs'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:48:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.dialogs'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:49:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.icons'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:49:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.icons'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:50:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.examiner'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:50:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.examiner'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:51:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.floating'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:51:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.floating'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:52:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.menus'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:52:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.menus'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:53:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.panels'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:53:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.panels'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:54:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.panes'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:54:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.panes'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:55:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.tooltips'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:55:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.tooltips'>` has no attribute `__all__`
- src/bokeh/models/ui/__init__.py:56:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.ui.ui_element'>` has no attribute `__all__`
+ src/bokeh/models/ui/__init__.py:56:6: error[unresolved-attribute] Type `<module 'bokeh.models.ui.ui_element'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:47:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.buttons'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:47:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.buttons'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:48:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.groups'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:48:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.groups'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:49:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.indicators'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:49:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.indicators'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:50:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.inputs'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:50:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.inputs'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:51:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.markups'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:51:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.markups'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:52:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.pickers'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:52:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.pickers'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:53:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.sliders'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:53:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.sliders'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:54:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.tables'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:54:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.tables'>` has no attribute `__all__`
- src/bokeh/models/widgets/__init__.py:55:6: error[unresolved-attribute] Type `<module 'src.bokeh.models.widgets.widget'>` has no attribute `__all__`
+ src/bokeh/models/widgets/__init__.py:55:6: error[unresolved-attribute] Type `<module 'bokeh.models.widgets.widget'>` has no attribute `__all__`
- src/bokeh/plotting/__init__.py:58:22: error[unresolved-import] Module `src.bokeh.plotting._figure` has no member `DEFAULT_TOOLS`
+ src/bokeh/plotting/__init__.py:58:22: error[unresolved-import] Module `bokeh.plotting._figure` has no member `DEFAULT_TOOLS`
- src/bokeh/plotting/_figure.py:65:24: error[unresolved-import] Module `src.bokeh.plotting.glyph_api` has no member `_MARKER_SHORTCUTS`
+ src/bokeh/plotting/_figure.py:65:24: error[unresolved-import] Module `bokeh.plotting.glyph_api` has no member `_MARKER_SHORTCUTS`
- src/bokeh/plotting/_plot.py:52:5: error[unresolved-import] Module `src.bokeh.models` has no member `TimedeltaAxis`
+ src/bokeh/plotting/_plot.py:52:5: error[unresolved-import] Module `bokeh.models` has no member `TimedeltaAxis`
- src/bokeh/plotting/glyph_api.pyi:28:5: error[unresolved-import] Module `src.bokeh._specs` has no member `NonNegative`
+ src/bokeh/plotting/glyph_api.pyi:28:5: error[unresolved-import] Module `bokeh._specs` has no member `NonNegative`
- src/bokeh/themes/theme.py:191:37: error[unresolved-import] Module `src.bokeh.models.glyphs` has no member `Glyph`
+ src/bokeh/themes/theme.py:191:37: error[unresolved-import] Module `bokeh.models.glyphs` has no member `Glyph`

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-09-29 13:13_

> I suspect this is the cause of [#20603 (comment)](https://github.com/astral-sh/ruff/pull/20603#issuecomment-3345317444)

Okay, well, maybe not, but it at least leads to better diagnostics 🙃

---

_Label `diagnostics` added by @AlexWaygood on 2025-09-29 13:18_

---

_Marked ready for review by @AlexWaygood on 2025-09-29 13:43_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-29 13:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-29 13:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-29 13:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-29 13:43_

---

_@MichaReiser reviewed on 2025-09-29 17:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:252 on 2025-09-29 17:54_

Should we move this to where we add the default search paths. I feel like we shouldn't apply this sorting when it's a user-provided list (which you're also arguing in favor of?)

---

_@AlexWaygood reviewed on 2025-09-29 17:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:252 on 2025-09-29 17:57_

Ah, we only add the implicit `src` root if there was no explicit user configuration? That makes sense; I forgot how this worked.

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-29 18:51_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-29 19:11_

---

_@MichaReiser approved on 2025-09-29 20:08_

---

_Merged by @AlexWaygood on 2025-09-29 20:19_

---

_Closed by @AlexWaygood on 2025-09-29 20:19_

---

_Branch deleted on 2025-09-29 20:19_

---
