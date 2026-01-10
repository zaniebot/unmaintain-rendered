```yaml
number: 18137
title: "[ty] Support `import <namespace>` and `from <namespace> import module`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/namespace-package-import
created_at: 2025-05-16T15:12:27Z
updated_at: 2025-05-26T21:24:09Z
url: https://github.com/astral-sh/ruff/pull/18137
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Support `import <namespace>` and `from <namespace> import module`

---

_Pull request opened by @MichaReiser on 2025-05-16 15:12_

## Summary

The module resolver incorrectly returned `None` if a module name resolves to a namespace package (it did correctly handle the case where a module name resolves to a file inside a namespace package).

Supporting this requires a smaller refactor because `Module` assumed that `file` and `search_path` are always known. However, neither are present for a namespace package (it doesn't resolve to a file AND the package can exist on multiple search paths).



Fixes https://github.com/astral-sh/ty/issues/375
Fixes https://github.com/astral-sh/ty/issues/363

## Test plan

Added regression tests. I verified that the imports in the linked issues resolve successfully.

## Ecosystem review

* `optuna`: Correct, [`auto_generated`](https://github.com/optuna/optuna/tree/master/optuna/storages/_grpc/auto_generated) is a namespace package
* `rotki`: Correct, the project has a [`packaging`](https://github.com/rotki/rotki/tree/develop/packaging?rgh-link-date=2025-05-16T17%3A27%3A03Z) folder. It doesn't contain any python code but it isn't wrong for ty to pick it up as a namespace package.
* ~~`psycopg`~~: All the code is in `psycopg/psycopg` (it's a src layout but the `src` folder is called `psycopg`). We weren't able to resolve any imports before. We now resolve `psycopg` as a namespace package which is *correct* from a module resolution point. The solution here is to change how we detect `src.root` or that they change their configuration to set `src.root`
* [`rclip`](https://github.com/yurijmikhalevich/rclip/tree/main/rclip): Is a namespace package and we are now able to resolve the imports 
* `dd-trace-py`: `benchmarks` is a namespace package and we're now able to resolve imports. They also use other namespace packages that now resolve correctly
* `scipy`: They use submodules for some of the modules in [`_lib`](https://github.com/scipy/scipy/tree/main/scipy/_lib) but they point to the [project root](https://github.com/data-apis/array-api-compat/tree/8005d6d02c0f1717881de37a710871bb955eb5cd) and not the package folder? We now recognize them as namespace packages. 


Unsure: 

**`schema_salad`**: 

The relevant lines are:

```py
import ruamel.yaml
from ruamel.yaml.comments import CommentedBase, CommentedMap, CommentedSeq

def _add_lc_filename(r: ruamel.yaml.comments.CommentedBase, source: AnyStr) -> None:
```

`ruamel` is a namespace package, `ruamel.yaml` is a regular package. 

Before: We inferred `Unknown` for `ruamel` and `yaml`
Now: We infer `ruamel.yaml` but it seems we don't resolve the `comments.py` module?

I think this is unrelated to my changes.

**cwltool**: Same as `schema_salad`

---

_Label `ty` added by @MichaReiser on 2025-05-16 15:12_

---

_@MichaReiser reviewed on 2025-05-16 15:12_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:82 on 2025-05-16 15:12_

Double check if this is needed

---

_Comment by @github-actions[bot] on 2025-05-16 15:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
schema_salad (https://github.com/common-workflow-language/schema_salad)
+ error[unresolved-attribute] schema_salad/sourceline.py:12:25: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] schema_salad/sourceline.py:13:22: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] schema_salad/sourceline.py:30:24: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] schema_salad/sourceline.py:271:38: Type `<module 'ruamel.yaml'>` has no attribute `comments`
- Found 247 diagnostics
+ Found 251 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[unresolved-import] examples/contrib/test_jsondump.py:6:6: Cannot resolve imported module `mitmproxy.test`
+ error[unresolved-attribute] examples/contrib/test_jsondump.py:10:15: Type `<module 'mitmproxy.test.tutils'>` has no attribute `test_data`
- error[unresolved-import] examples/contrib/test_jsondump.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/test_jsondump.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/test_xss_scanner.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/test_xss_scanner.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_mapping.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_mapping.py:9:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_mapping.py:66:9: Object of type `Literal[b"Test"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:69:16: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_mapping.py:78:9: Object of type `Literal[b"<body> Test </body>"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:82:16: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:82:16: Attribute `decode` on type `bytes | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_mapping.py:91:9: Object of type `Literal[b"<body> Test </body>"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:93:9: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:96:16: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_mapping.py:105:9: Object of type `Literal[b"<title> Test </title>"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_mapping.py:108:16: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_mapping.py:143:9: Object of type `Literal[b"<title> Test </title>"]` is not assignable to attribute `content` on type `Response | None`
- error[unresolved-import] examples/contrib/webscanner_helper/test_proxyauth_selenium.py:11:6: Cannot resolve imported module `mitmproxy.test`
+ error[unresolved-attribute] examples/contrib/webscanner_helper/test_proxyauth_selenium.py:57:9: Unresolved attribute `is_replay` on type `Request`.
- error[unresolved-import] examples/contrib/webscanner_helper/test_proxyauth_selenium.py:12:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urldict.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urldict.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urlindex.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urlindex.py:15:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urlinjection.py:11:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_urlinjection.py:12:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlindex.py:131:16: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlindex.py:146:16: Attribute `status_code` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_urlindex.py:172:9: Object of type `Literal[404]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:39:41: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:41:37: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_urlinjection.py:46:9: Object of type `Literal["<body></body>"]` is not assignable to attribute `text` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:47:41: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:49:37: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_urlinjection.py:54:9: Object of type `Literal[404]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:55:41: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:57:37: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_urlinjection.py:72:9: Object of type `Literal[404]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:73:42: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:75:38: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_urlinjection.py:90:9: Object of type `Literal[404]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:92:13: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:95:70: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:112:51: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] examples/contrib/webscanner_helper/test_urlinjection.py:114:47: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] examples/contrib/webscanner_helper/test_watchdog.py:56:9: Object of type `Literal["Test Error"]` is not assignable to attribute `error` of type `Error | None`
- error[unresolved-import] examples/contrib/webscanner_helper/test_watchdog.py:10:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] examples/contrib/webscanner_helper/test_watchdog.py:11:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/examples/test_examples.py:63:20: Attribute `content` on type `Response | None` is possibly unbound
- error[unresolved-import] test/examples/test_examples.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/examples/test_examples.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/examples/test_examples.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/helper_tools/dumperview.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/helper_tools/dumperview.py:8:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/helper_tools/dumperview.py:54:5: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/helper_tools/dumperview.py:55:5: Object of type `bytes` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] test/helper_tools/dumperview.py:63:5: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/helper_tools/dumperview.py:64:5: Object of type `Literal[b"<html><body>Hello!</body></html>"]` is not assignable to attribute `content` on type `Response | None`
- error[unresolved-import] test/mitmproxy/addons/test_anticache.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_anticache.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_anticomp.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_anticomp.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_asgiapp.py:10:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_block.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_blocklist.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_blocklist.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_browser.py:4:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_blocklist.py:44:24: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_blocklist.py:57:20: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_blocklist.py:76:20: Attribute `msg` on type `Error | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_blocklist.py:76:35: Attribute `KILLED_MESSAGE` on type `Error | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_clientplayback.py:122:20: Attribute `status_code` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_clientplayback.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_clientplayback.py:15:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] test/mitmproxy/addons/test_clientplayback.py:182:5: Object of type `None` is not assignable to attribute `request` of type `Request`
- error[unresolved-import] test/mitmproxy/addons/test_command_history.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_comment.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_comment.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_core.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_core.py:6:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_core.py:92:16: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_core.py:94:16: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_core.py:95:16: Attribute `reason` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_core.py:99:16: Attribute `reason` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_core.py:101:16: Attribute `reason` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_cut.py:10:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_cut.py:11:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_disable_h2c.py:39:20: Attribute `msg` on type `Error | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_disable_h2c.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_disable_h2c.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_disable_h2c.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_dns_resolver.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_dns_resolver.py:15:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_dns_resolver.py:16:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:126:24: Attribute `response_code` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:128:24: Attribute `response_code` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:132:28: Attribute `response_code` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:134:28: Attribute `response_code` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:135:28: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:141:24: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:143:24: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:146:21: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:151:21: Attribute `answers` on type `DNSMessage | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_dumper.py:13:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_dumper.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_dumper.py:15:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:26:9: Object of type `Literal[b"foo"]` is not assignable to attribute `content` on type `Response | None`
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:77:9: Object of type `Literal[300]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dumper.py:84:9: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:85:9: Object of type `Literal[400]` is not assignable to attribute `status_code` on type `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dumper.py:101:5: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:102:5: Object of type `bytes` is not assignable to attribute `content` on type `Response | None`
+ error[invalid-argument-type] test/mitmproxy/addons/test_dumper.py:108:25: Argument to bound method `_echo_message` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage`, found `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dumper.py:115:5: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:116:5: Object of type `bytes` is not assignable to attribute `content` on type `Response | None`
+ error[invalid-argument-type] test/mitmproxy/addons/test_dumper.py:123:25: Argument to bound method `_echo_message` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage`, found `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dumper.py:143:9: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dumper.py:144:9: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:145:9: Object of type `bytes` is not assignable to attribute `content` on type `Response | None`
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:146:9: Object of type `Headers` is not assignable to attribute `trailers` on type `Response | None`
+ error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:289:9: Object of type `Literal[b"HTTP/2.0"]` is not assignable to attribute `http_version` on type `Response | None`
- error[unresolved-import] test/mitmproxy/addons/test_export.py:10:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_export.py:11:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_export.py:12:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_intercept.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_intercept.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_keepserving.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_maplocal.py:9:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_maplocal.py:10:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_maplocal.py:154:20: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_maplocal.py:162:20: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_maplocal.py:172:20: Attribute `content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_maplocal.py:183:20: Attribute `status_code` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_mapremote.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_mapremote.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_modifybody.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_modifybody.py:6:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] test/mitmproxy/addons/test_modifybody.py:41:13: Object of type `Literal[b"foo"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifybody.py:43:20: Attribute `content` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_modifybody.py:58:13: Object of type `Literal[b"foo"]` is not assignable to attribute `content` on type `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifybody.py:62:21: Attribute `content` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_modifyheaders.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_modifyheaders.py:6:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:48:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:50:20: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:55:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:57:20: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:73:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:75:33: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:84:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:86:33: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_modifyheaders.py:116:21: Attribute `headers` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_next_layer.py:36:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_onboarding.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_proxyauth.py:11:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_proxyauth.py:12:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_proxyauth.py:107:20: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_proxyauth.py:119:20: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_proxyauth.py:223:20: Attribute `status_code` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_proxyauth.py:229:20: Attribute `status_code` on type `Response | None` is possibly unbound
+ error[invalid-assignment] test/mitmproxy/addons/test_proxyauth.py:243:13: Object of type `Literal[True]` is not assignable to attribute `is_replay` of type `str | None`
- error[unresolved-import] test/mitmproxy/addons/test_proxyserver.py:37:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_proxyserver.py:38:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_readfile.py:10:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_readfile.py:11:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_save.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_save.py:8:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_savehar.py:251:13: Attribute `content` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_savehar.py:17:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_savehar.py:18:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_savehar.py:19:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_script.py:13:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_script.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_serverplayback.py:9:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_serverplayback.py:10:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_serverplayback.py:359:16: Attribute `data` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_serverplayback.py:359:36: Attribute `data` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_serverplayback.py:419:16: Attribute `status_code` on type `Response | None` is possibly unbound
- error[unresolved-attribute] test/mitmproxy/addons/test_serverplayback.py:353:22: Type `<module 'mitmproxy'>` has no attribute `test`
- error[unresolved-attribute] test/mitmproxy/addons/test_serverplayback.py:374:22: Type `<module 'mitmproxy'>` has no attribute `test`
- error[unresolved-attribute] test/mitmproxy/addons/test_serverplayback.py:389:22: Type `<module 'mitmproxy'>` has no attribute `test`
- error[unresolved-attribute] test/mitmproxy/addons/test_serverplayback.py:413:22: Type `<module 'mitmproxy'>` has no attribute `test`
- error[unresolved-import] test/mitmproxy/addons/test_stickyauth.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_stickyauth.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_stickycookie.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_stickycookie.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_stickycookie.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_strip_dns_https_records.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_strip_dns_https_records.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_strip_dns_https_records.py:8:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_stickycookie.py:31:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_stickycookie.py:45:9: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_stickycookie.py:96:17: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_strip_dns_https_records.py:51:31: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_strip_dns_https_records.py:77:20: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_strip_dns_https_records.py:81:20: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_strip_dns_https_records.py:83:13: Attribute `answers` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_strip_dns_https_records.py:85:20: Attribute `answers` on type `DNSMessage | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_termlog.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_tlsconfig.py:21:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_update_alt_svc.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_update_alt_svc.py:5:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_update_alt_svc.py:28:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_update_alt_svc.py:36:13: Attribute `headers` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_update_alt_svc.py:43:13: Attribute `headers` on type `Response | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/addons/test_upstream_auth.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_upstream_auth.py:9:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_view.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/addons/test_view.py:8:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-argument-type] test/mitmproxy/addons/test_view.py:54:35: Argument to function `len` is incorrect: Expected `Sized`, found `bytes | None`
+ error[invalid-argument-type] test/mitmproxy/addons/test_view.py:54:65: Argument to function `len` is incorrect: Expected `Sized`, found `bytes | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_view.py:54:65: Attribute `raw_content` on type `Response | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/addons/test_view.py:71:31: Attribute `size` on type `DNSMessage | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/contentviews/test___init__.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test___init__.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test__api.py:9:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test__utils.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test__utils.py:9:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/contentviews/test__utils.py:22:13: Attribute `headers` on type `Response | None` is possibly unbound
+ error[invalid-argument-type] test/mitmproxy/contentviews/test__utils.py:23:38: Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`
+ warning[possibly-unbound-attribute] test/mitmproxy/contentviews/test__utils.py:47:19: Attribute `messages` on type `WebSocketData | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/contentviews/test__view_http3.py:6:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test__view_query.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/contentviews/test__view_socketio.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/proxy/bench.py:14:7: Cannot resolve imported module `.layers.http`
- error[unresolved-import] test/mitmproxy/proxy/bench.py:15:7: Cannot resolve imported module `.layers.http`
- error[unresolved-import] test/mitmproxy/proxy/layers/test_modes.py:35:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/proxy/layers/test_modes.py:36:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/proxy/test_context.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/proxy/test_context.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/proxy/test_mode_servers.py:20:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/script/test_concurrent.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/script/test_concurrent.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_addonmanager.py:13:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_addonmanager.py:14:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_command.py:11:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_command.py:12:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_dns.py:9:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_dns.py:10:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/test_dns.py:338:16: Attribute `get_state` on type `DNSMessage | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/test_dns.py:346:16: Attribute `get_state` on type `Error | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/test_eventsequence.py:5:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/test_eventsequence.py:39:16: Attribute `messages` on type `WebSocketData | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/test_eventsequence.py:41:16: Attribute `messages` on type `WebSocketData | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/test_eventsequence.py:43:16: Attribute `messages` on type `WebSocketData | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/mitmproxy/test_eventsequence.py:45:16: Attribute `messages` on type `WebSocketData | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/test_flow.py:13:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_flow.py:14:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] test/mitmproxy/test_flow.py:39:9: Object of type `Literal[True]` is not assignable to attribute `marked` of type `str`
+ error[invalid-assignment] test/mitmproxy/test_flow.py:62:9: Object of type `Literal[200]` is not assignable to attribute `status_code` on type `Response | None`
+ error[invalid-assignment] test/mitmproxy/test_flow.py:66:9: Object of type `Literal[201]` is not assignable to attribute `status_code` on type `Response | None`
- error[unresolved-import] test/mitmproxy/test_flowfilter.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_taddons.py:1:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_tcp.py:5:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/test_tcp.py:40:16: Attribute `get_state` on type `Error | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/test_types.py:12:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_types.py:13:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/test_udp.py:5:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/test_udp.py:40:16: Attribute `get_state` on type `Error | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/test_websocket.py:6:6: Cannot resolve imported module `mitmproxy.test`
+ warning[possibly-unbound-attribute] test/mitmproxy/test_websocket.py:20:30: Attribute `_get_formatted_messages` on type `WebSocketData | None` is possibly unbound
- error[unresolved-import] test/mitmproxy/tools/console/test_commander.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/console/test_common.py:3:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/console/test_contentview.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/console/test_flowview.py:2:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/console/test_keymap.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/web/test_app.py:17:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] test/mitmproxy/tools/web/test_app.py:69:9: Object of type `None` is not assignable to attribute `content` on type `Response | None`
- error[unresolved-import] test/mitmproxy/tools/web/test_static_viewer.py:7:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/web/test_static_viewer.py:8:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/tools/web/test_webaddons.py:4:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] test/mitmproxy/utils/test_magisk.py:5:6: Cannot resolve imported module `mitmproxy.test`
- error[unresolved-import] web/gen/tflow_js.py:10:6: Cannot resolve imported module `mitmproxy.test`
+ error[invalid-assignment] web/gen/tflow_js.py:31:5: Object of type `Headers` is not assignable to attribute `trailers` on type `Response | None`
- Found 2124 diagnostics
+ Found 2112 diagnostics

optuna (https://github.com/optuna/optuna)
- error[unresolved-import] optuna/storages/_grpc/client.py:30:10: Cannot resolve imported module `optuna.storages._grpc.auto_generated`
- error[unresolved-import] optuna/storages/_grpc/client.py:31:10: Cannot resolve imported module `optuna.storages._grpc.auto_generated`
- error[unresolved-import] optuna/storages/_grpc/server.py:16:10: Cannot resolve imported module `optuna.storages._grpc.auto_generated`
- error[unresolved-import] optuna/storages/_grpc/servicer.py:23:10: Cannot resolve imported module `optuna.storages._grpc.auto_generated`
- error[unresolved-import] optuna/storages/_grpc/servicer.py:24:10: Cannot resolve imported module `optuna.storages._grpc.auto_generated`
- Found 1072 diagnostics
+ Found 1067 diagnostics

rclip (https://github.com/yurijmikhalevich/rclip)
- error[unresolved-import] rclip/main.py:13:6: Cannot resolve imported module `rclip`
- error[unresolved-import] rclip/main.py:17:6: Cannot resolve imported module `rclip.utils`
- error[unresolved-import] rclip/model.py:8:6: Cannot resolve imported module `rclip.utils`
- Found 16 diagnostics
+ Found 13 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ error[unresolved-attribute] cwltool/main.py:188:17: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] cwltool/main.py:189:22: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] cwltool/main.py:245:23: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] cwltool/main.py:331:16: Type `<module 'ruamel.yaml'>` has no attribute `comments`
+ error[unresolved-attribute] cwltool/main.py:747:5: Type `<module 'ruamel.yaml'>` has no attribute `representer`
- Found 297 diagnostics
+ Found 302 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-import] benchmarks/bm/iast_utils/aspects_benchmarks_generate.py:11:6: Cannot resolve imported module `benchmarks.bm.iast_fixtures`
- error[unresolved-import] benchmarks/bm/iast_utils/aspects_benchmarks_generate.py:12:6: Cannot resolve imported module `benchmarks.bm.iast_fixtures`
- error[unresolved-import] benchmarks/bm/iast_utils/aspects_benchmarks_generate.py:13:6: Cannot resolve imported module `benchmarks.bm.iast_fixtures`
- error[unresolved-import] ddtrace/contrib/internal/celery/signals.py:11:6: Cannot resolve imported module `ddtrace.contrib.internal.celery`
- error[unresolved-import] ddtrace/contrib/internal/django/patch.py:206:5: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/django/patch.py:430:5: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/django/patch.py:467:5: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/django/patch.py:614:5: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/django/patch.py:906:5: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/grpc/aio_client_interceptor.py:21:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/aio_client_interceptor.py:22:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/aio_server_interceptor.py:23:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/client_interceptor.py:14:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/client_interceptor.py:15:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/patch.py:5:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/patch.py:6:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/server_interceptor.py:11:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/grpc/utils.py:6:6: Cannot resolve imported module `ddtrace.contrib.internal.grpc`
- error[unresolved-import] ddtrace/contrib/internal/openai/patch.py:7:6: Cannot resolve imported module `ddtrace.contrib.internal.openai`
- error[unresolved-import] ddtrace/contrib/internal/protobuf/patch.py:1:6: Cannot resolve imported module `google`
+ error[unresolved-attribute] ddtrace/contrib/internal/protobuf/patch.py:30:5: Unresolved attribute `_datadog_patch` on type `<module 'google.protobuf'>`.
+ error[unresolved-attribute] ddtrace/contrib/internal/protobuf/patch.py:40:9: Unresolved attribute `_datadog_patch` on type `<module 'google.protobuf'>`.
+ error[unresolved-attribute] ddtrace/contrib/internal/protobuf/patch.py:42:16: Type `<module 'google.protobuf'>` has no attribute `internal`
- error[unresolved-import] ddtrace/contrib/internal/tornado/application.py:7:6: Cannot resolve imported module `ddtrace.contrib.internal.tornado`
- error[unresolved-import] ddtrace/contrib/internal/tornado/patch.py:12:1: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/tornado/patch.py:13:1: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/tornado/patch.py:14:1: Cannot resolve imported module `.`
- error[unresolved-import] ddtrace/contrib/internal/tornado/patch.py:15:1: Cannot resolve imported module `.`
- error[invalid-assignment] ddtrace/contrib/internal/tornado/patch.py:54:5: Object of type `Unknown` is not assignable to attribute `_wrap_executor` on type `Unknown | Tracer`
+ error[invalid-assignment] ddtrace/contrib/internal/tornado/patch.py:54:5: Object of type `def wrap_executor(tracer, fn, args, kwargs, span_name, service=None, resource=None, span_type=None) -> Unknown` is not assignable to attribute `_wrap_executor` on type `Unknown | Tracer`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:10:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:30:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:31:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:32:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:45:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:46:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_suite_level.py:47:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:8:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:26:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:27:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:28:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:42:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:43:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_all_itr_skip_test_level.py:44:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_atr_mix_fail.py:17:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_atr_mix_pass.py:13:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_efd_all_pass.py:17:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_efd_faulty_session.py:15:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_efd_mix_fail.py:17:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_efd_mix_pass.py:17:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:28:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:56:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:57:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:58:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:60:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:63:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:66:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:86:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:87:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:90:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:93:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:96:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:99:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:103:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:126:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:127:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:128:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:130:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:131:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:132:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:163:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:164:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:165:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:167:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:168:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_suite_level.py:169:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-import] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:25:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:52:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:53:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:54:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:56:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:59:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:62:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:82:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:83:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:86:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:89:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:92:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:95:40: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:99:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:122:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:123:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:124:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:126:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:127:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:128:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:159:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:160:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:161:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:163:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:164:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/fake_runner_mix_fail_itr_test_level.py:165:25: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-import] tests/ci_visibility/api/test_internal_test_visibility_api.py:5:6: Cannot resolve imported module `ddtrace.internal.test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:49:33: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:52:37: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:57:53: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:83:37: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:86:41: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/api/test_internal_test_visibility_api.py:92:21: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1262:9: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1293:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1294:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1295:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1296:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1297:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1300:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1303:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1307:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1308:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1309:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1310:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1311:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1314:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1317:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1324:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1325:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1326:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1327:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1328:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1331:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1334:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1338:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1339:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1340:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1341:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1342:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1345:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1348:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1353:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1354:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1355:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1356:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1357:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1360:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1363:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1366:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1367:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1368:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1369:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1370:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1373:16: Type `<module 'ddtrace.internal'>` has no attribute `test_visibility`
- error[unresolved-attribute] tests/ci_visibility/test_ci_visibility.py:1376:16: Type `<...*[Comment body truncated]*

---

_Comment by @MichaReiser on 2025-05-16 15:25_

Looks like I messed something up 😆 

---

_Comment by @github-actions[bot] on 2025-05-16 16:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-05-16 17:27_

Rotki: The output looks correct. There's a local folder called [`packaging`](https://github.com/rotki/rotki/tree/develop/packaging) that doesn't contain any python files (docker image).

The problem for `psycopg` is that its `src.root` is [`psycopg`](https://github.com/psycopg/psycopg/tree/master/psycopg) (the code is in `psycopg/psycopg`). This obviously confuses the module resolver a good deal because it now assumes that `psycopg` is a namespace package. This isn't worse than before where all imports simply failed. The solution here is to teach mypy primer to pass the `src.root` config (CC: @sharkdp ?).

---

_Comment by @MichaReiser on 2025-05-16 17:36_

Mypy primer now hangs :(

---

_Marked ready for review by @MichaReiser on 2025-05-16 17:36_

---

_Review requested from @carljm by @MichaReiser on 2025-05-16 17:36_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-16 17:36_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-16 17:36_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-16 17:36_

---

_Comment by @MichaReiser on 2025-05-16 17:38_

Something we could try is to change `src.root` to default to `.` and `<project-name>` if such a folder exists (and contains a `__init__.py`?), given that this isn't an entirely uncommon project structure.

---

_Renamed from "[ty] Support `import <namespace>` and `from <namespace> import module`"" to "[ty] Support `import <namespace>` and `from <namespace> import module`" by @MichaReiser on 2025-05-16 17:48_

---

_@MichaReiser reviewed on 2025-05-19 11:54_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:72 on 2025-05-19 11:54_

For a more meaningful mypy primer result (pulls in https://github.com/hauntsaninja/mypy_primer/pull/171)

TODO: Revert before landing

---

_@sharkdp reviewed on 2025-05-19 14:24_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer.yaml`:72 on 2025-05-19 14:24_

[PR](https://github.com/hauntsaninja/mypy_primer/pull/171) has been merged:

```suggestion
            --from="git+https://github.com/hauntsaninja/mypy_primer@01a7ca325f674433c58e02416a867178d1571128" \
```

---

_Review request for @carljm removed by @carljm on 2025-05-19 19:37_

---

_Comment by @AlexWaygood on 2025-05-19 19:48_

The `schema_salad` primer hits are because we do not currently model that when an import like this occurs:

```py
import foo
from foo.bar import baz
```

then an implicit `foo.bar = <module foo.bar>` assignment occurs as a result of the second import. This is indeed unrelated to your PR -- a similar issue is discussed in https://github.com/astral-sh/ty/issues/133.

---

_Comment by @AlexWaygood on 2025-05-19 19:56_

> ```diff
> + error[unresolved-attribute] examples/contrib/test_jsondump.py:10:15: Type `<module 'mitmproxy.test.tutils'>` has no attribute `test_data`
> ```

This seems like a true positive...?

```pycon
% uv run --no-project --with=mitmproxy python
      Built pyperclip==1.9.0
Installed 44 packages in 45ms
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from mitmproxy.test import tutils
>>> tutils.test_data
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    tutils.test_data
AttributeError: module 'mitmproxy.test.tutils' has no attribute 'test_data'
```

I don't really understand what's (meant to be) going on in mitmproxy's `examples/` directory at all. I think it's okay to just ignore all the (many) primer hits that are coming out of that directory...

---

_@AlexWaygood reviewed on 2025-05-19 20:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/module.rs`:110 on 2025-05-19 20:04_

it's possible we might want namespace packages without `File`s to be recognized as `KnownModule`s in the future? But I can't think of a use case right now, so we can probably figure that out if and when it actually comes up.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:787 on 2025-05-19 20:59_

Excluding the standard library implicitly here implicitly depends on the invariant that there will never be any namespace packages added to the standard library. But it does seem very unlikely that CPython would ever add any namespace packages to the stdlib, so this is probably okay. Though I'd prefer it if we made it explicit in this comment that we're relying on that invariant.

I'm also totally sure I understand why this exclusion is necessary, though. When you say "the standard library [...] has conditional modules", do you mean something like this?

https://github.com/astral-sh/ruff/blob/b302d89da3325c705f87a8343a16aad1723b67ab/crates/ty_vendored/vendor/typeshed/stdlib/VERSIONS#L293-L294

I'm not sure I understand why we'd ever accidentally infer a module in the stdlib as being a namespace package... when single-file modules are converted to become packages with submodules in later versions, typeshed generally pretends that they were always non-namespace packages and marks the submodule as newly added in the `VERSIONS` file

---

_@AlexWaygood reviewed on 2025-05-19 20:59_

---

_@AlexWaygood reviewed on 2025-05-19 21:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:797 on 2025-05-19 21:06_

we return `Ok()` here if the system is case-sensitive _or_ the path exists according to a case-sensitive check...? That doesn't seem correct

---

_@AlexWaygood reviewed on 2025-05-19 21:07_

The primer hits look great!! I left some comments in `resolver.rs` -- not sure about some of the details there

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/module.rs`:110 on 2025-05-20 05:47_

That's possible if we want to special case some library. I haven't looked at `KnownModule` itself but I suspect that it won't be too hard to add but I don't see a need to do it today (and agree with you that we can do it in the future)

---

_@MichaReiser reviewed on 2025-05-20 05:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:787 on 2025-05-20 05:50_

> I'm also totally sure I understand why this exclusion is necessary, though. When you say "the standard library [...] has conditional modules", do you mean something like this?

Sort of, but for a package, the problem is that the `resolve_file_module` calls for `__init__` may fail but only because the module isn't available for this Python version. We would need to repeat the version check for namespace packages to avoid classifying the folder as a namespace package. There's actually a test that fails if we remove this check where we test that you can't import `xml` on an older python version (or newer?) and it started succeeding.



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:797 on 2025-05-20 05:52_

Can you tell me why you think this isn't correct? 

The check here ensures that we don't return `true` if the module name and the name on disk use different casing:

* If the file system is case sensitive: The `is_directory` call would have failed if the casing doesn't match. That's why we can skip the extra check
* If the file system is case sensitive: Perform a case sensitive lookup and only return `Ok` if the path exists too.

---

_@MichaReiser reviewed on 2025-05-20 16:30_

Hmm, I just noticed that my comments are still pending

---

_@AlexWaygood reviewed on 2025-05-20 17:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:787 on 2025-05-20 17:44_

I see, okay. It would be great if we could expand this comment a bit to make this clearer

---

_Comment by @MichaReiser on 2025-05-20 18:16_

@AlexWaygood Is this good to go once I improved the comment or is there something else?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:797 on 2025-05-20 18:43_

OK, I think I understand. Sorry for being dense!

---

_@AlexWaygood reviewed on 2025-05-20 18:43_

---

_@AlexWaygood approved on 2025-05-20 18:43_

---

_Merged by @MichaReiser on 2025-05-21 07:28_

---

_Closed by @MichaReiser on 2025-05-21 07:28_

---

_Branch deleted on 2025-05-21 07:28_

---

_Comment by @karlicoss on 2025-05-26 14:24_

I have what seems like a related issue -- tried with latest ty but seems like it's still happening.


```
$ uv add --dev 'ty @ https://github.com/astral-sh/ty.git'
$ uv run ty version
ty 0.0.1-alpha.6 (55b412037 2025-05-23)
```

Wondering, has this fix made it into `ty` git version/main branch? I guess ty reuses some of the core with ruff so perhaps it's not made it through the dependencies yet?

Happy to file a separate issue in ty bugtracker if this is somehow unrelated!

# What happens
Consider the following hierarchy:
```
$ find src/testty
src/testty
src/testty/b.py
src/testty/a.py
src/testty/py.typed
```

This is the only file with code:
```
$ cat src/testty/a.py 
from testty import b
from . import b
```

This results in:
```
$ uv run ty check src/
error[unresolved-import]: Cannot resolve imported module `testty`
 --> src/testty/a.py:1:6
  |
1 | from testty import b
  |      ^^^^^^
2 | from . import b
  |
info: make sure your Python environment is properly configured: https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `.`
 --> src/testty/a.py:2:1
  |
1 | from testty import b
2 | from . import b
  | ^^^^^^^^^^^^^^^
  |
info: rule `unresolved-import` is enabled by default

Found 2 diagnostics
```

# Expected behaviour

- this works under mypy:
  ```
  $ uv run -m mypy -p testty 
  Success: no issues found in 3 source files
  ```

- this works in ty if we convert a package from a namespace to regular one by adding `__init__.py`
  ```
  $ touch src/testty/__init__.py
  $ uv run ty check src/
  WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
  All checks passed!
  ```

---

_Comment by @nojaf on 2025-05-26 14:26_

@karlicoss I don't believe a new version with this fix has been released yet.

---

_Comment by @sharkdp on 2025-05-26 18:58_

@karlicoss Should be available in [ty 0.0.1-alpha.7](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.7).

---

_Comment by @karlicoss on 2025-05-26 21:24_

Thank you, that was fast! Can confirm it works

---
