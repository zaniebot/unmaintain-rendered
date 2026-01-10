```yaml
number: 10779
title: "[`pyflakes`] Allow forward references in class bases in stub files (`F821`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - linter
assignees: []
merged: true
base: main
head: f821-just-class-bases
created_at: 2024-04-04T20:43:24Z
updated_at: 2024-04-07T08:49:26Z
url: https://github.com/astral-sh/ruff/pull/10779
synced_at: 2026-01-10T22:47:03Z
```

# [`pyflakes`] Allow forward references in class bases in stub files (`F821`)

---

_Pull request opened by @AlexWaygood on 2024-04-04 20:43_

## Summary

Fixes #3011.

Type checkers currently allow forward references in all contexts in stub files, and stubs frequently make use of this capability (although it doesn't actually seem to be specc'd anywhere --neither in PEP 484, nor https://typing.readthedocs.io/en/latest/source/stubs.html#id6, nor the CPython typing docs). Implementing it so that Ruff allows forward references in _all contexts_ in stub files seems non-trivial, however (or at least, I couldn't figure out how to do it easily), so this PR does not do that. Perhaps it _should_; if we think this apporach isn't principled enough, I'm happy to close it and postpone changing anything here.

However, this does reduce the number of F821 errors Ruff emits on typeshed down from 76 to 2, which would mean that we could enable the rule at typeshed. The remaining 2 F821 errors can be trivially fixed at typeshed by moving definitions around; forward references in class bases were really the only remaining places where there was a real _use case_ for forward references in stub files that Ruff wasn't yet allowing.

## Test plan

`cargo test`. I also ran this PR branch on typeshed to check to see if there were any new false positives caused by the changes here; there were none.

---

_Review requested from @carljm by @AlexWaygood on 2024-04-04 20:48_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-04-04 20:48_

---

_Label `rule` added by @AlexWaygood on 2024-04-04 20:51_

---

_Label `linter` added by @AlexWaygood on 2024-04-04 20:51_

---

_Comment by @github-actions[bot] on 2024-04-04 20:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-04-04 20:59_

> ✅ ecosystem check detected no linter changes.

Well, _locally_ it seems to get rid of 74 F821 hits on typeshed :(

Does the ecosystem check respect projects' `pyproject.toml` files? We currently have F821 completely disabled at typeshed: https://github.com/python/typeshed/blob/130a04905c0cab48604ac7be1f3d18ce96567c68/pyproject.toml#L95 (I tested locally with that line removed from the `pyproject.toml` file)

---

_Comment by @AlexWaygood on 2024-04-05 09:16_

To be more specific about why deferring all expressions in stub files seems trickier: here's a patch that does that: https://github.com/astral-sh/ruff/compare/main...AlexWaygood:ruff:f821-forward-refs-pyi?expand=1.

And here's the impact it has on typeshed:

<details>

```
stdlib/_collections_abc.pyi:65:5: F821 Undefined name `__all__`
stdlib/_ctypes.pyi:166:9: F811 Redefinition of unused `_length_` from line 164
stdlib/_ctypes.pyi:171:9: F811 Redefinition of unused `_type_` from line 169
stdlib/_ctypes.pyi:176:9: F811 Redefinition of unused `raw` from line 174
stdlib/argparse.pyi:29:5: F821 Undefined name `__all__`
stdlib/base64.pyi:27:5: F821 Undefined name `__all__`
stdlib/builtins.pyi:102:9: F811 Redefinition of unused `__class__` from line 100
stdlib/calendar.pyi:38:5: F821 Undefined name `__all__`
stdlib/calendar.pyi:40:5: F821 Undefined name `__all__`
stdlib/contextlib.pyi:26:5: F821 Undefined name `__all__`
stdlib/contextlib.pyi:29:5: F821 Undefined name `__all__`
stdlib/csv.pyi:60:5: F821 Undefined name `__all__`
stdlib/dataclasses.pyi:32:5: F821 Undefined name `__all__`
stdlib/enum.pyi:13:5: F821 Undefined name `__all__`
stdlib/enum.pyi:37:5: F821 Undefined name `__all__`
stdlib/functools.pyi:28:5: F821 Undefined name `__all__`
stdlib/genericpath.pyi:22:5: F821 Undefined name `__all__`
stdlib/gettext.pyi:27:5: F821 Undefined name `__all__`
stdlib/importlib/abc.pyi:24:9: F821 Undefined name `__all__`
stdlib/importlib/metadata/__init__.pyi:33:5: F821 Undefined name `__all__`
stdlib/importlib/resources/__init__.pyi:16:5: F821 Undefined name `__all__`
stdlib/importlib/resources/__init__.pyi:19:5: F821 Undefined name `__all__`
stdlib/inspect.pyi:132:9: F821 Undefined name `__all__`
stdlib/io.pyi:35:5: F821 Undefined name `__all__`
stdlib/locale.pyi:119:5: F821 Undefined name `__all__`
stdlib/locale.pyi:122:5: F821 Undefined name `__all__`
stdlib/locale.pyi:125:5: F821 Undefined name `__all__`
stdlib/logging/__init__.pyi:62:5: F821 Undefined name `__all__`
stdlib/logging/__init__.pyi:64:5: F821 Undefined name `__all__`
stdlib/multiprocessing/context.pyi:138:9: F811 Redefinition of unused `reducer` from line 136
stdlib/multiprocessing/resource_sharer.pyi:7:5: F821 Undefined name `__all__`
stdlib/multiprocessing/resource_sharer.pyi:14:5: F821 Undefined name `__all__`
stdlib/ntpath.pyi:92:5: F821 Undefined name `__all__`
stdlib/opcode.pyi:20:5: F821 Undefined name `__all__`
stdlib/opcode.pyi:22:5: F821 Undefined name `__all__`
stdlib/pkgutil.pyi:21:5: F821 Undefined name `__all__`
stdlib/plistlib.pyi:11:5: F821 Undefined name `__all__`
stdlib/posixpath.pyi:62:5: F821 Undefined name `__all__`
stdlib/random.pyi:36:5: F821 Undefined name `__all__`
stdlib/random.pyi:38:5: F821 Undefined name `__all__`
stdlib/re.pyi:46:5: F821 Undefined name `__all__`
stdlib/socketserver.pyi:22:5: F821 Undefined name `__all__`
stdlib/socketserver.pyi:32:9: F821 Undefined name `__all__`
stdlib/sqlite3/dbapi2.pyi:367:13: F811 Redefinition of unused `autocommit` from line 365
stdlib/statistics.pyi:30:5: F821 Undefined name `__all__`
stdlib/subprocess.pyi:29:5: F821 Undefined name `__all__`
stdlib/tarfile.pyi:30:5: F821 Undefined name `__all__`
stdlib/tarfile.pyi:407:9: F811 Redefinition of unused `linkpath` from line 405
stdlib/threading.pyi:39:5: F821 Undefined name `__all__`
stdlib/threading.pyi:42:5: F821 Undefined name `__all__`
stdlib/token.pyi:76:5: F821 Undefined name `__all__`
stdlib/token.pyi:79:5: F821 Undefined name `__all__`
stdlib/tty.pyi:9:9: F821 Undefined name `__all__`
stdlib/turtle.pyi:133:5: F821 Undefined name `__all__`
stdlib/types.pyi:52:5: F821 Undefined name `__all__`
stdlib/types.pyi:55:5: F821 Undefined name `__all__`
stdlib/types.pyi:58:5: F821 Undefined name `__all__`
stdlib/typing.pyi:107:5: F821 Undefined name `__all__`
stdlib/typing.pyi:110:5: F821 Undefined name `__all__`
stdlib/typing.pyi:113:5: F821 Undefined name `__all__`
stdlib/typing.pyi:130:5: F821 Undefined name `__all__`
stdlib/urllib/error.pyi:15:9: F811 Redefinition of unused `headers` from line 13
stdlib/urllib/request.pyi:88:9: F811 Redefinition of unused `full_url` from line 86
stdlib/urllib/request.pyi:90:9: F811 Redefinition of unused `full_url` from line 88
stdlib/xml/etree/ElementTree.pyi:37:5: F821 Undefined name `__all__`
stubs/JACK-Client/jack/__init__.pyi:95:9: F811 Redefinition of unused `blocksize` from line 93
stubs/JACK-Client/jack/__init__.pyi:128:9: F811 Redefinition of unused `transport_frame` from line 126
stubs/JACK-Client/jack/__init__.pyi:188:9: F811 Redefinition of unused `shortname` from line 186
stubs/Pillow/PIL/IcnsImagePlugin.pyi:33:9: F811 Redefinition of unused `size` from line 31
stubs/Pillow/PIL/IcoImagePlugin.pyi:23:9: F811 Redefinition of unused `size` from line 21
stubs/WebOb/webob/cookies.pyi:56:9: F811 Redefinition of unused `path` from line 54
stubs/WebOb/webob/cookies.pyi:60:9: F811 Redefinition of unused `domain` from line 58
stubs/WebOb/webob/cookies.pyi:64:9: F811 Redefinition of unused `comment` from line 62
stubs/WebOb/webob/cookies.pyi:70:9: F811 Redefinition of unused `httponly` from line 68
stubs/WebOb/webob/cookies.pyi:74:9: F811 Redefinition of unused `secure` from line 72
stubs/WebOb/webob/request.pyi:61:9: F811 Redefinition of unused `body_file` from line 59
stubs/WebOb/webob/request.pyi:71:9: F811 Redefinition of unused `scheme` from line 69
stubs/WebOb/webob/request.pyi:75:9: F811 Redefinition of unused `http_version` from line 73
stubs/WebOb/webob/request.pyi:83:9: F811 Redefinition of unused `server_name` from line 81
stubs/WebOb/webob/request.pyi:87:9: F811 Redefinition of unused `server_port` from line 85
stubs/WebOb/webob/request.pyi:92:9: F811 Redefinition of unused `path_info` from line 90
stubs/WebOb/webob/request.pyi:97:9: F811 Redefinition of unused `upath_info` from line 95
stubs/WebOb/webob/request.pyi:142:9: F811 Redefinition of unused `is_body_readable` from line 140
stubs/WebOb/webob/request.pyi:206:9: F811 Redefinition of unused `uscript_name` from line 204
stubs/WebOb/webob/request.pyi:210:9: F811 Redefinition of unused `upath_info` from line 208
stubs/aiofiles/aiofiles/os.pyi:33:5: F821 Undefined name `__all__`
stubs/antlr4-python3-runtime/antlr4/Lexer.pyi:36:9: F811 Redefinition of unused `inputStream` from line 34
stubs/antlr4-python3-runtime/antlr4/Lexer.pyi:45:9: F811 Redefinition of unused `type` from line 43
stubs/antlr4-python3-runtime/antlr4/Lexer.pyi:49:9: F811 Redefinition of unused `line` from line 47
stubs/antlr4-python3-runtime/antlr4/Lexer.pyi:53:9: F811 Redefinition of unused `column` from line 51
stubs/antlr4-python3-runtime/antlr4/Lexer.pyi:58:9: F811 Redefinition of unused `text` from line 56
stubs/antlr4-python3-runtime/antlr4/Recognizer.pyi:27:9: F811 Redefinition of unused `state` from line 25
stubs/antlr4-python3-runtime/antlr4/Token.pyi:22:9: F811 Redefinition of unused `text` from line 20
stubs/antlr4-python3-runtime/antlr4/Token.pyi:48:9: F811 Redefinition of unused `text` from line 46
stubs/aws-xray-sdk/aws_xray_sdk/core/context.pyi:27:9: F811 Redefinition of unused `context_missing` from line 25
stubs/aws-xray-sdk/aws_xray_sdk/core/lambda_launcher.pyi:23:9: F811 Redefinition of unused `context_missing` from line 21
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:73:9: F811 Redefinition of unused `enabled` from line 71
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:77:9: F811 Redefinition of unused `sampling` from line 75
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:81:9: F811 Redefinition of unused `sampler` from line 79
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:85:9: F811 Redefinition of unused `service` from line 83
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:89:9: F811 Redefinition of unused `dynamic_naming` from line 87
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:93:9: F811 Redefinition of unused `context` from line 91
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:97:9: F811 Redefinition of unused `emitter` from line 95
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:101:9: F811 Redefinition of unused `streaming` from line 99
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:105:9: F811 Redefinition of unused `streaming_threshold` from line 103
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:109:9: F811 Redefinition of unused `max_trace_back` from line 107
stubs/aws-xray-sdk/aws_xray_sdk/core/recorder.pyi:113:9: F811 Redefinition of unused `stream_sql` from line 111
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/connector.pyi:9:9: F811 Redefinition of unused `context` from line 7
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/rule_cache.pyi:13:9: F811 Redefinition of unused `rules` from line 11
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/rule_cache.pyi:17:9: F811 Redefinition of unused `last_updated` from line 15
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/sampler.pyi:22:9: F811 Redefinition of unused `xray_client` from line 20
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/sampling_rule.pyi:28:9: F811 Redefinition of unused `rate` from line 26
stubs/aws-xray-sdk/aws_xray_sdk/core/sampling/sampling_rule.pyi:36:9: F811 Redefinition of unused `reservoir` from line 34
stubs/aws-xray-sdk/aws_xray_sdk/core/streaming/default_streaming.pyi:8:9: F811 Redefinition of unused `streaming_threshold` from line 6
stubs/beautifulsoup4/bs4/element.pyi:271:9: F811 Redefinition of unused `string` from line 269
stubs/boltons/boltons/urlutils.pyi:56:9: F811 Redefinition of unused `path` from line 54
stubs/console-menu/consolemenu/format/menu_margins.pyi:6:9: F811 Redefinition of unused `left` from line 4
stubs/console-menu/consolemenu/format/menu_margins.pyi:10:9: F811 Redefinition of unused `right` from line 8
stubs/console-menu/consolemenu/format/menu_margins.pyi:14:9: F811 Redefinition of unused `top` from line 12
stubs/console-menu/consolemenu/format/menu_margins.pyi:18:9: F811 Redefinition of unused `bottom` from line 16
stubs/console-menu/consolemenu/format/menu_padding.pyi:6:9: F811 Redefinition of unused `left` from line 4
stubs/console-menu/consolemenu/format/menu_padding.pyi:10:9: F811 Redefinition of unused `right` from line 8
stubs/console-menu/consolemenu/format/menu_padding.pyi:14:9: F811 Redefinition of unused `top` from line 12
stubs/console-menu/consolemenu/format/menu_padding.pyi:18:9: F811 Redefinition of unused `bottom` from line 16
stubs/console-menu/consolemenu/format/menu_style.pyi:17:9: F811 Redefinition of unused `margins` from line 15
stubs/console-menu/consolemenu/format/menu_style.pyi:21:9: F811 Redefinition of unused `padding` from line 19
stubs/console-menu/consolemenu/format/menu_style.pyi:25:9: F811 Redefinition of unused `border_style` from line 23
stubs/console-menu/consolemenu/format/menu_style.pyi:29:9: F811 Redefinition of unused `border_style_factory` from line 27
stubs/console-menu/consolemenu/menu_component.pyi:81:9: F811 Redefinition of unused `items` from line 79
stubs/console-menu/consolemenu/menu_component.pyi:98:9: F811 Redefinition of unused `prompt` from line 96
stubs/fpdf2/fpdf/drawing.pyi:143:9: F811 Redefinition of unused `allow_transparency` from line 141
stubs/fpdf2/fpdf/drawing.pyi:147:9: F811 Redefinition of unused `paint_rule` from line 145
stubs/fpdf2/fpdf/drawing.pyi:151:9: F811 Redefinition of unused `auto_close` from line 149
stubs/fpdf2/fpdf/drawing.pyi:155:9: F811 Redefinition of unused `intersection_rule` from line 153
stubs/fpdf2/fpdf/drawing.pyi:159:9: F811 Redefinition of unused `fill_color` from line 157
stubs/fpdf2/fpdf/drawing.pyi:163:9: F811 Redefinition of unused `fill_opacity` from line 161
stubs/fpdf2/fpdf/drawing.pyi:167:9: F811 Redefinition of unused `stroke_color` from line 165
stubs/fpdf2/fpdf/drawing.pyi:171:9: F811 Redefinition of unused `stroke_opacity` from line 169
stubs/fpdf2/fpdf/drawing.pyi:175:9: F811 Redefinition of unused `blend_mode` from line 173
stubs/fpdf2/fpdf/drawing.pyi:179:9: F811 Redefinition of unused `stroke_width` from line 177
stubs/fpdf2/fpdf/drawing.pyi:183:9: F811 Redefinition of unused `stroke_cap_style` from line 181
stubs/fpdf2/fpdf/drawing.pyi:187:9: F811 Redefinition of unused `stroke_join_style` from line 185
stubs/fpdf2/fpdf/drawing.pyi:191:9: F811 Redefinition of unused `stroke_miter_limit` from line 189
stubs/fpdf2/fpdf/drawing.pyi:195:9: F811 Redefinition of unused `stroke_dash_pattern` from line 193
stubs/fpdf2/fpdf/drawing.pyi:199:9: F811 Redefinition of unused `stroke_dash_phase` from line 197
stubs/fpdf2/fpdf/drawing.pyi:339:9: F811 Redefinition of unused `transform` from line 337
stubs/fpdf2/fpdf/drawing.pyi:343:9: F811 Redefinition of unused `auto_close` from line 341
stubs/fpdf2/fpdf/drawing.pyi:347:9: F811 Redefinition of unused `paint_rule` from line 345
stubs/fpdf2/fpdf/drawing.pyi:351:9: F811 Redefinition of unused `clipping_path` from line 349
stubs/fpdf2/fpdf/drawing.pyi:395:9: F811 Redefinition of unused `transform` from line 393
stubs/fpdf2/fpdf/drawing.pyi:399:9: F811 Redefinition of unused `clipping_path` from line 397
stubs/fpdf2/fpdf/graphics_state.pyi:25:9: F811 Redefinition of unused `draw_color` from line 23
stubs/fpdf2/fpdf/graphics_state.pyi:29:9: F811 Redefinition of unused `fill_color` from line 27
stubs/fpdf2/fpdf/graphics_state.pyi:33:9: F811 Redefinition of unused `text_color` from line 31
stubs/fpdf2/fpdf/graphics_state.pyi:37:9: F811 Redefinition of unused `underline` from line 35
stubs/fpdf2/fpdf/graphics_state.pyi:41:9: F811 Redefinition of unused `font_style` from line 39
stubs/fpdf2/fpdf/graphics_state.pyi:45:9: F811 Redefinition of unused `font_stretching` from line 43
stubs/fpdf2/fpdf/graphics_state.pyi:49:9: F811 Redefinition of unused `char_spacing` from line 47
stubs/fpdf2/fpdf/graphics_state.pyi:53:9: F811 Redefinition of unused `font_family` from line 51
stubs/fpdf2/fpdf/graphics_state.pyi:57:9: F811 Redefinition of unused `font_size_pt` from line 55
stubs/fpdf2/fpdf/graphics_state.pyi:61:9: F811 Redefinition of unused `font_size` from line 59
stubs/fpdf2/fpdf/graphics_state.pyi:65:9: F811 Redefinition of unused `current_font` from line 63
stubs/fpdf2/fpdf/graphics_state.pyi:69:9: F811 Redefinition of unused `dash_pattern` from line 67
stubs/fpdf2/fpdf/graphics_state.pyi:73:9: F811 Redefinition of unused `line_width` from line 71
stubs/fpdf2/fpdf/graphics_state.pyi:77:9: F811 Redefinition of unused `text_mode` from line 75
stubs/fpdf2/fpdf/graphics_state.pyi:81:9: F811 Redefinition of unused `char_vpos` from line 79
stubs/fpdf2/fpdf/graphics_state.pyi:85:9: F811 Redefinition of unused `sub_scale` from line 83
stubs/fpdf2/fpdf/graphics_state.pyi:89:9: F811 Redefinition of unused `sup_scale` from line 87
stubs/fpdf2/fpdf/graphics_state.pyi:93:9: F811 Redefinition of unused `nom_scale` from line 91
stubs/fpdf2/fpdf/graphics_state.pyi:97:9: F811 Redefinition of unused `denom_scale` from line 95
stubs/fpdf2/fpdf/graphics_state.pyi:101:9: F811 Redefinition of unused `sub_lift` from line 99
stubs/fpdf2/fpdf/graphics_state.pyi:105:9: F811 Redefinition of unused `sup_lift` from line 103
stubs/fpdf2/fpdf/graphics_state.pyi:109:9: F811 Redefinition of unused `nom_lift` from line 107
stubs/fpdf2/fpdf/graphics_state.pyi:113:9: F811 Redefinition of unused `denom_lift` from line 111
stubs/fpdf2/fpdf/graphics_state.pyi:117:9: F811 Redefinition of unused `text_shaping` from line 115
stubs/fpdf2/fpdf/line_break.pyi:25:9: F811 Redefinition of unused `font` from line 23
stubs/fpdf2/fpdf/prefs.pyi:23:9: F811 Redefinition of unused `non_full_screen_page_mode` from line 21
stubs/fpdf2/fpdf/syntax.pyi:37:9: F811 Redefinition of unused `id` from line 35
stubs/gevent/gevent/_ffi/watcher.pyi:30:9: F811 Redefinition of unused `priority` from line 28
stubs/gevent/gevent/_greenlet_primitives.pyi:14:9: F811 Redefinition of unused `loop` from line 12
stubs/gevent/gevent/hub.pyi:52:9: F811 Redefinition of unused `ref` from line 50
stubs/gevent/gevent/libev/watcher.pyi:25:9: F811 Redefinition of unused `ref` from line 23
stubs/gevent/gevent/libev/watcher.pyi:34:9: F811 Redefinition of unused `fd` from line 32
stubs/gevent/gevent/libev/watcher.pyi:38:9: F811 Redefinition of unused `events` from line 36
stubs/gevent/gevent/libev/watcher.pyi:57:9: F811 Redefinition of unused `rpid` from line 55
stubs/gevent/gevent/libev/watcher.pyi:61:9: F811 Redefinition of unused `rstatus` from line 59
stubs/gevent/gevent/libuv/watcher.pyi:8:9: F811 Redefinition of unused `ref` from line 6
stubs/gevent/gevent/libuv/watcher.pyi:15:9: F811 Redefinition of unused `events` from line 13
stubs/gevent/gevent/threadpool.pyi:28:9: F811 Redefinition of unused `maxsize` from line 26
stubs/gevent/gevent/threadpool.pyi:32:9: F811 Redefinition of unused `size` from line 30
stubs/google-cloud-ndb/google/cloud/ndb/context.pyi:12:9: F811 Redefinition of unused `context` from line 10
stubs/google-cloud-ndb/google/cloud/ndb/context.pyi:16:9: F811 Redefinition of unused `toplevel_context` from line 14
stubs/greenlet/greenlet/_greenlet.pyi:34:9: F811 Redefinition of unused `gr_context` from line 32
stubs/greenlet/greenlet/_greenlet.pyi:45:9: F811 Redefinition of unused `run` from line 43
stubs/hvac/hvac/api/secrets_engines/kv.pyi:17:9: F811 Redefinition of unused `default_kv_version` from line 15
stubs/hvac/hvac/api/vault_api_category.pyi:18:9: F811 Redefinition of unused `adapter` from line 16
stubs/hvac/hvac/v1/__init__.pyi:27:9: F811 Redefinition of unused `adapter` from line 25
stubs/hvac/hvac/v1/__init__.pyi:31:9: F811 Redefinition of unused `url` from line 29
stubs/hvac/hvac/v1/__init__.pyi:35:9: F811 Redefinition of unused `token` from line 33
stubs/hvac/hvac/v1/__init__.pyi:39:9: F811 Redefinition of unused `session` from line 37
stubs/hvac/hvac/v1/__init__.pyi:43:9: F811 Redefinition of unused `allow_redirects` from line 41
stubs/influxdb-client/influxdb_client/_async/api_client.pyi:26:9: F811 Redefinition of unused `user_agent` from line 24
stubs/influxdb-client/influxdb_client/_sync/api_client.pyi:26:9: F811 Redefinition of unused `user_agent` from line 24
stubs/influxdb-client/influxdb_client/configuration.pyi:36:9: F811 Redefinition of unused `logger_file` from line 34
stubs/influxdb-client/influxdb_client/configuration.pyi:40:9: F811 Redefinition of unused `debug` from line 38
stubs/influxdb-client/influxdb_client/configuration.pyi:44:9: F811 Redefinition of unused `logger_format` from line 42
stubs/influxdb-client/influxdb_client/domain/add_resource_member_request_body.pyi:11:9: F811 Redefinition of unused `id` from line 9
stubs/influxdb-client/influxdb_client/domain/add_resource_member_request_body.pyi:15:9: F811 Redefinition of unused `name` from line 13
stubs/influxdb-client/influxdb_client/domain/analyze_query_response.pyi:11:9: F811 Redefinition of unused `errors` from line 9
stubs/influxdb-client/influxdb_client/domain/analyze_query_response_errors.pyi:17:9: F811 Redefinition of unused `line` from line 15
stubs/influxdb-client/influxdb_client/domain/analyze_query_response_errors.pyi:21:9: F811 Redefinition of unused `column` from line 19
stubs/influxdb-client/influxdb_client/domain/analyze_query_response_errors.pyi:25:9: F811 Redefinition of unused `character` from line 23
stubs/influxdb-client/influxdb_client/domain/analyze_query_response_errors.pyi:29:9: F811 Redefinition of unused `message` from line 27
stubs/influxdb-client/influxdb_client/domain/array_expression.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/array_expression.pyi:17:9: F811 Redefinition of unused `elements` from line 15
stubs/influxdb-client/influxdb_client/domain/ast_response.pyi:11:9: F811 Redefinition of unused `ast` from line 9
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:27:9: F811 Redefinition of unused `created_at` from line 25
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:31:9: F811 Redefinition of unused `updated_at` from line 29
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:35:9: F811 Redefinition of unused `org_id` from line 33
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:39:9: F811 Redefinition of unused `permissions` from line 37
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:43:9: F811 Redefinition of unused `id` from line 41
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:47:9: F811 Redefinition of unused `token` from line 45
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:51:9: F811 Redefinition of unused `user_id` from line 49
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:55:9: F811 Redefinition of unused `user` from line 53
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:59:9: F811 Redefinition of unused `org` from line 57
stubs/influxdb-client/influxdb_client/domain/authorization.pyi:63:9: F811 Redefinition of unused `links` from line 61
stubs/influxdb-client/influxdb_client/domain/authorization_post_request.pyi:20:9: F811 Redefinition of unused `org_id` from line 18
stubs/influxdb-client/influxdb_client/domain/authorization_post_request.pyi:24:9: F811 Redefinition of unused `user_id` from line 22
stubs/influxdb-client/influxdb_client/domain/authorization_post_request.pyi:28:9: F811 Redefinition of unused `permissions` from line 26
stubs/influxdb-client/influxdb_client/domain/authorization_update_request.pyi:11:9: F811 Redefinition of unused `status` from line 9
stubs/influxdb-client/influxdb_client/domain/authorization_update_request.pyi:15:9: F811 Redefinition of unused `description` from line 13
stubs/influxdb-client/influxdb_client/domain/authorizations.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/authorizations.pyi:15:9: F811 Redefinition of unused `authorizations` from line 13
stubs/influxdb-client/influxdb_client/domain/axes.pyi:11:9: F811 Redefinition of unused `x` from line 9
stubs/influxdb-client/influxdb_client/domain/axes.pyi:15:9: F811 Redefinition of unused `y` from line 13
stubs/influxdb-client/influxdb_client/domain/axis.pyi:19:9: F811 Redefinition of unused `bounds` from line 17
stubs/influxdb-client/influxdb_client/domain/axis.pyi:23:9: F811 Redefinition of unused `label` from line 21
stubs/influxdb-client/influxdb_client/domain/axis.pyi:27:9: F811 Redefinition of unused `prefix` from line 25
stubs/influxdb-client/influxdb_client/domain/axis.pyi:31:9: F811 Redefinition of unused `suffix` from line 29
stubs/influxdb-client/influxdb_client/domain/axis.pyi:35:9: F811 Redefinition of unused `base` from line 33
stubs/influxdb-client/influxdb_client/domain/axis.pyi:39:9: F811 Redefinition of unused `scale` from line 37
stubs/influxdb-client/influxdb_client/domain/bad_statement.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/bad_statement.pyi:17:9: F811 Redefinition of unused `text` from line 15
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:45:9: F811 Redefinition of unused `time_format` from line 43
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:49:9: F811 Redefinition of unused `type` from line 47
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:53:9: F811 Redefinition of unused `queries` from line 51
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:57:9: F811 Redefinition of unused `colors` from line 55
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:61:9: F811 Redefinition of unused `shape` from line 59
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:65:9: F811 Redefinition of unused `note` from line 63
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:69:9: F811 Redefinition of unused `show_note_when_empty` from line 67
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:73:9: F811 Redefinition of unused `axes` from line 71
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:77:9: F811 Redefinition of unused `static_legend` from line 75
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:81:9: F811 Redefinition of unused `x_column` from line 79
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:85:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 83
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:89:9: F811 Redefinition of unused `x_total_ticks` from line 87
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:93:9: F811 Redefinition of unused `x_tick_start` from line 91
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:97:9: F811 Redefinition of unused `x_tick_step` from line 95
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:101:9: F811 Redefinition of unused `y_column` from line 99
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:105:9: F811 Redefinition of unused `generate_y_axis_ticks` from line 103
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:109:9: F811 Redefinition of unused `y_total_ticks` from line 107
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:113:9: F811 Redefinition of unused `y_tick_start` from line 111
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:117:9: F811 Redefinition of unused `y_tick_step` from line 115
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:121:9: F811 Redefinition of unused `upper_column` from line 119
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:125:9: F811 Redefinition of unused `main_column` from line 123
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:129:9: F811 Redefinition of unused `lower_column` from line 127
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:133:9: F811 Redefinition of unused `hover_dimension` from line 131
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:137:9: F811 Redefinition of unused `geom` from line 135
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:141:9: F811 Redefinition of unused `legend_colorize_rows` from line 139
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:145:9: F811 Redefinition of unused `legend_hide` from line 143
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:149:9: F811 Redefinition of unused `legend_opacity` from line 147
stubs/influxdb-client/influxdb_client/domain/band_view_properties.pyi:153:9: F811 Redefinition of unused `legend_orientation_threshold` from line 151
stubs/influxdb-client/influxdb_client/domain/binary_expression.pyi:19:9: F811 Redefinition of unused `type` from line 17
stubs/influxdb-client/influxdb_client/domain/binary_expression.pyi:23:9: F811 Redefinition of unused `operator` from line 21
stubs/influxdb-client/influxdb_client/domain/binary_expression.pyi:27:9: F811 Redefinition of unused `left` from line 25
stubs/influxdb-client/influxdb_client/domain/binary_expression.pyi:31:9: F811 Redefinition of unused `right` from line 29
stubs/influxdb-client/influxdb_client/domain/block.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/block.pyi:17:9: F811 Redefinition of unused `body` from line 15
stubs/influxdb-client/influxdb_client/domain/boolean_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/boolean_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:25:9: F811 Redefinition of unused `links` from line 23
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:29:9: F811 Redefinition of unused `id` from line 27
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:33:9: F811 Redefinition of unused `type` from line 31
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:37:9: F811 Redefinition of unused `name` from line 35
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:41:9: F811 Redefinition of unused `description` from line 39
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:45:9: F811 Redefinition of unused `org_id` from line 43
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:49:9: F811 Redefinition of unused `rp` from line 47
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:53:9: F811 Redefinition of unused `schema_type` from line 51
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:57:9: F811 Redefinition of unused `created_at` from line 55
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:61:9: F811 Redefinition of unused `updated_at` from line 59
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:65:9: F811 Redefinition of unused `retention_rules` from line 63
stubs/influxdb-client/influxdb_client/domain/bucket.pyi:69:9: F811 Redefinition of unused `labels` from line 67
stubs/influxdb-client/influxdb_client/domain/bucket_links.pyi:19:9: F811 Redefinition of unused `labels` from line 17
stubs/influxdb-client/influxdb_client/domain/bucket_links.pyi:23:9: F811 Redefinition of unused `members` from line 21
stubs/influxdb-client/influxdb_client/domain/bucket_links.pyi:27:9: F811 Redefinition of unused `org` from line 25
stubs/influxdb-client/influxdb_client/domain/bucket_links.pyi:31:9: F811 Redefinition of unused `owners` from line 29
stubs/influxdb-client/influxdb_client/domain/bucket_links.pyi:35:9: F811 Redefinition of unused `write` from line 33
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:20:9: F811 Redefinition of unused `organization_id` from line 18
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:24:9: F811 Redefinition of unused `organization_name` from line 22
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:28:9: F811 Redefinition of unused `bucket_id` from line 26
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:32:9: F811 Redefinition of unused `bucket_name` from line 30
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:36:9: F811 Redefinition of unused `description` from line 34
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:40:9: F811 Redefinition of unused `default_retention_policy` from line 38
stubs/influxdb-client/influxdb_client/domain/bucket_metadata_manifest.pyi:44:9: F811 Redefinition of unused `retention_policies` from line 42
stubs/influxdb-client/influxdb_client/domain/bucket_retention_rules.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/bucket_retention_rules.pyi:18:9: F811 Redefinition of unused `shard_group_duration_seconds` from line 16
stubs/influxdb-client/influxdb_client/domain/bucket_shard_mapping.pyi:11:9: F811 Redefinition of unused `old_id` from line 9
stubs/influxdb-client/influxdb_client/domain/bucket_shard_mapping.pyi:15:9: F811 Redefinition of unused `new_id` from line 13
stubs/influxdb-client/influxdb_client/domain/buckets.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/buckets.pyi:15:9: F811 Redefinition of unused `buckets` from line 13
stubs/influxdb-client/influxdb_client/domain/builder_config.pyi:17:9: F811 Redefinition of unused `buckets` from line 15
stubs/influxdb-client/influxdb_client/domain/builder_config.pyi:21:9: F811 Redefinition of unused `tags` from line 19
stubs/influxdb-client/influxdb_client/domain/builder_config.pyi:25:9: F811 Redefinition of unused `functions` from line 23
stubs/influxdb-client/influxdb_client/domain/builder_config.pyi:29:9: F811 Redefinition of unused `aggregate_window` from line 27
stubs/influxdb-client/influxdb_client/domain/builder_config_aggregate_window.pyi:11:9: F811 Redefinition of unused `period` from line 9
stubs/influxdb-client/influxdb_client/domain/builder_config_aggregate_window.pyi:15:9: F811 Redefinition of unused `fill_values` from line 13
stubs/influxdb-client/influxdb_client/domain/builder_functions_type.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/builder_tags_type.pyi:13:9: F811 Redefinition of unused `key` from line 11
stubs/influxdb-client/influxdb_client/domain/builder_tags_type.pyi:17:9: F811 Redefinition of unused `values` from line 15
stubs/influxdb-client/influxdb_client/domain/builder_tags_type.pyi:21:9: F811 Redefinition of unused `aggregate_function_type` from line 19
stubs/influxdb-client/influxdb_client/domain/builtin_statement.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/builtin_statement.pyi:17:9: F811 Redefinition of unused `id` from line 15
stubs/influxdb-client/influxdb_client/domain/call_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/call_expression.pyi:19:9: F811 Redefinition of unused `callee` from line 17
stubs/influxdb-client/influxdb_client/domain/call_expression.pyi:23:9: F811 Redefinition of unused `arguments` from line 21
stubs/influxdb-client/influxdb_client/domain/cell.pyi:20:9: F811 Redefinition of unused `id` from line 18
stubs/influxdb-client/influxdb_client/domain/cell.pyi:24:9: F811 Redefinition of unused `links` from line 22
stubs/influxdb-client/influxdb_client/domain/cell.pyi:28:9: F811 Redefinition of unused `x` from line 26
stubs/influxdb-client/influxdb_client/domain/cell.pyi:32:9: F811 Redefinition of unused `y` from line 30
stubs/influxdb-client/influxdb_client/domain/cell.pyi:36:9: F811 Redefinition of unused `w` from line 34
stubs/influxdb-client/influxdb_client/domain/cell.pyi:40:9: F811 Redefinition of unused `h` from line 38
stubs/influxdb-client/influxdb_client/domain/cell.pyi:44:9: F811 Redefinition of unused `view_id` from line 42
stubs/influxdb-client/influxdb_client/domain/cell_links.pyi:11:9: F811 Redefinition of unused `view` from line 9
stubs/influxdb-client/influxdb_client/domain/cell_update.pyi:13:9: F811 Redefinition of unused `x` from line 11
stubs/influxdb-client/influxdb_client/domain/cell_update.pyi:17:9: F811 Redefinition of unused `y` from line 15
stubs/influxdb-client/influxdb_client/domain/cell_update.pyi:21:9: F811 Redefinition of unused `w` from line 19
stubs/influxdb-client/influxdb_client/domain/cell_update.pyi:25:9: F811 Redefinition of unused `h` from line 23
stubs/influxdb-client/influxdb_client/domain/cell_with_view_properties.pyi:24:9: F811 Redefinition of unused `name` from line 22
stubs/influxdb-client/influxdb_client/domain/cell_with_view_properties.pyi:28:9: F811 Redefinition of unused `properties` from line 26
stubs/influxdb-client/influxdb_client/domain/check.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:28:9: F811 Redefinition of unused `id` from line 26
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:32:9: F811 Redefinition of unused `name` from line 30
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:36:9: F811 Redefinition of unused `org_id` from line 34
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:40:9: F811 Redefinition of unused `task_id` from line 38
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:44:9: F811 Redefinition of unused `owner_id` from line 42
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:48:9: F811 Redefinition of unused `created_at` from line 46
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:52:9: F811 Redefinition of unused `updated_at` from line 50
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:56:9: F811 Redefinition of unused `query` from line 54
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:60:9: F811 Redefinition of unused `status` from line 58
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:64:9: F811 Redefinition of unused `description` from line 62
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:68:9: F811 Redefinition of unused `latest_completed` from line 66
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:72:9: F811 Redefinition of unused `last_run_status` from line 70
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:76:9: F811 Redefinition of unused `last_run_error` from line 74
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:80:9: F811 Redefinition of unused `labels` from line 78
stubs/influxdb-client/influxdb_client/domain/check_base.pyi:84:9: F811 Redefinition of unused `links` from line 82
stubs/influxdb-client/influxdb_client/domain/check_base_links.pyi:18:9: F811 Redefinition of unused `labels` from line 16
stubs/influxdb-client/influxdb_client/domain/check_base_links.pyi:22:9: F811 Redefinition of unused `members` from line 20
stubs/influxdb-client/influxdb_client/domain/check_base_links.pyi:26:9: F811 Redefinition of unused `owners` from line 24
stubs/influxdb-client/influxdb_client/domain/check_base_links.pyi:30:9: F811 Redefinition of unused `query` from line 28
stubs/influxdb-client/influxdb_client/domain/check_patch.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/check_patch.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/check_patch.pyi:21:9: F811 Redefinition of unused `status` from line 19
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:27:9: F811 Redefinition of unused `type` from line 25
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:31:9: F811 Redefinition of unused `shape` from line 29
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:35:9: F811 Redefinition of unused `check_id` from line 33
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:39:9: F811 Redefinition of unused `check` from line 37
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:43:9: F811 Redefinition of unused `queries` from line 41
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:47:9: F811 Redefinition of unused `colors` from line 45
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:51:9: F811 Redefinition of unused `legend_colorize_rows` from line 49
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:55:9: F811 Redefinition of unused `legend_hide` from line 53
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:59:9: F811 Redefinition of unused `legend_opacity` from line 57
stubs/influxdb-client/influxdb_client/domain/check_view_properties.pyi:63:9: F811 Redefinition of unused `legend_orientation_threshold` from line 61
stubs/influxdb-client/influxdb_client/domain/checks.pyi:11:9: F811 Redefinition of unused `checks` from line 9
stubs/influxdb-client/influxdb_client/domain/checks.pyi:15:9: F811 Redefinition of unused `links` from line 13
stubs/influxdb-client/influxdb_client/domain/conditional_expression.pyi:19:9: F811 Redefinition of unused `type` from line 17
stubs/influxdb-client/influxdb_client/domain/conditional_expression.pyi:23:9: F811 Redefinition of unused `test` from line 21
stubs/influxdb-client/influxdb_client/domain/conditional_expression.pyi:27:9: F811 Redefinition of unused `alternate` from line 25
stubs/influxdb-client/influxdb_client/domain/conditional_expression.pyi:31:9: F811 Redefinition of unused `consequent` from line 29
stubs/influxdb-client/influxdb_client/domain/config.pyi:11:9: F811 Redefinition of unused `config` from line 9
stubs/influxdb-client/influxdb_client/domain/constant_variable_properties.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/constant_variable_properties.pyi:17:9: F811 Redefinition of unused `values` from line 15
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:23:9: F811 Redefinition of unused `x` from line 21
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:27:9: F811 Redefinition of unused `y` from line 25
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:31:9: F811 Redefinition of unused `w` from line 29
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:35:9: F811 Redefinition of unused `h` from line 33
stubs/influxdb-client/influxdb_client/domain/create_cell.pyi:39:9: F811 Redefinition of unused `using_view` from line 37
stubs/influxdb-client/influxdb_client/domain/create_dashboard_request.pyi:13:9: F811 Redefinition of unused `org_id` from line 11
stubs/influxdb-client/influxdb_client/domain/create_dashboard_request.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/create_dashboard_request.pyi:21:9: F811 Redefinition of unused `description` from line 19
stubs/influxdb-client/influxdb_client/domain/custom_check.pyi:31:9: F811 Redefinition of unused `type` from line 29
stubs/influxdb-client/influxdb_client/domain/dashboard.pyi:23:9: F811 Redefinition of unused `links` from line 21
stubs/influxdb-client/influxdb_client/domain/dashboard.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/dashboard.pyi:31:9: F811 Redefinition of unused `meta` from line 29
stubs/influxdb-client/influxdb_client/domain/dashboard.pyi:35:9: F811 Redefinition of unused `cells` from line 33
stubs/influxdb-client/influxdb_client/domain/dashboard.pyi:39:9: F811 Redefinition of unused `labels` from line 37
stubs/influxdb-client/influxdb_client/domain/dashboard_color.pyi:18:9: F811 Redefinition of unused `id` from line 16
stubs/influxdb-client/influxdb_client/domain/dashboard_color.pyi:22:9: F811 Redefinition of unused `type` from line 20
stubs/influxdb-client/influxdb_client/domain/dashboard_color.pyi:26:9: F811 Redefinition of unused `hex` from line 24
stubs/influxdb-client/influxdb_client/domain/dashboard_color.pyi:30:9: F811 Redefinition of unused `name` from line 28
stubs/influxdb-client/influxdb_client/domain/dashboard_color.pyi:34:9: F811 Redefinition of unused `value` from line 32
stubs/influxdb-client/influxdb_client/domain/dashboard_query.pyi:17:9: F811 Redefinition of unused `text` from line 15
stubs/influxdb-client/influxdb_client/domain/dashboard_query.pyi:21:9: F811 Redefinition of unused `edit_mode` from line 19
stubs/influxdb-client/influxdb_client/domain/dashboard_query.pyi:25:9: F811 Redefinition of unused `name` from line 23
stubs/influxdb-client/influxdb_client/domain/dashboard_query.pyi:29:9: F811 Redefinition of unused `builder_config` from line 27
stubs/influxdb-client/influxdb_client/domain/dashboard_with_view_properties.pyi:23:9: F811 Redefinition of unused `links` from line 21
stubs/influxdb-client/influxdb_client/domain/dashboard_with_view_properties.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/dashboard_with_view_properties.pyi:31:9: F811 Redefinition of unused `meta` from line 29
stubs/influxdb-client/influxdb_client/domain/dashboard_with_view_properties.pyi:35:9: F811 Redefinition of unused `cells` from line 33
stubs/influxdb-client/influxdb_client/domain/dashboard_with_view_properties.pyi:39:9: F811 Redefinition of unused `labels` from line 37
stubs/influxdb-client/influxdb_client/domain/dashboards.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/dashboards.pyi:15:9: F811 Redefinition of unused `dashboards` from line 13
stubs/influxdb-client/influxdb_client/domain/date_time_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/date_time_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/dbr_ps.pyi:11:9: F811 Redefinition of unused `content` from line 9
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:21:9: F811 Redefinition of unused `id` from line 19
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:25:9: F811 Redefinition of unused `org_id` from line 23
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:29:9: F811 Redefinition of unused `bucket_id` from line 27
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:33:9: F811 Redefinition of unused `database` from line 31
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:37:9: F811 Redefinition of unused `retention_policy` from line 35
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:41:9: F811 Redefinition of unused `default` from line 39
stubs/influxdb-client/influxdb_client/domain/dbrp.pyi:46:9: F811 Redefinition of unused `links` from line 44
stubs/influxdb-client/influxdb_client/domain/dbrp_create.pyi:21:9: F811 Redefinition of unused `bucket_id` from line 19
stubs/influxdb-client/influxdb_client/domain/dbrp_create.pyi:25:9: F811 Redefinition of unused `database` from line 23
stubs/influxdb-client/influxdb_client/domain/dbrp_create.pyi:29:9: F811 Redefinition of unused `retention_policy` from line 27
stubs/influxdb-client/influxdb_client/domain/dbrp_create.pyi:33:9: F811 Redefinition of unused `default` from line 31
stubs/influxdb-client/influxdb_client/domain/dbrp_get.pyi:11:9: F811 Redefinition of unused `content` from line 9
stubs/influxdb-client/influxdb_client/domain/dbrp_update.pyi:11:9: F811 Redefinition of unused `retention_policy` from line 9
stubs/influxdb-client/influxdb_client/domain/dbrp_update.pyi:15:9: F811 Redefinition of unused `default` from line 13
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:39:9: F811 Redefinition of unused `type` from line 37
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:43:9: F811 Redefinition of unused `time_since` from line 41
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:47:9: F811 Redefinition of unused `stale_time` from line 45
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:51:9: F811 Redefinition of unused `report_zero` from line 49
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:55:9: F811 Redefinition of unused `level` from line 53
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:59:9: F811 Redefinition of unused `every` from line 57
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:63:9: F811 Redefinition of unused `offset` from line 61
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:67:9: F811 Redefinition of unused `tags` from line 65
stubs/influxdb-client/influxdb_client/domain/deadman_check.pyi:71:9: F811 Redefinition of unused `status_message_template` from line 69
stubs/influxdb-client/influxdb_client/domain/decimal_places.pyi:11:9: F811 Redefinition of unused `is_enforced` from line 9
stubs/influxdb-client/influxdb_client/domain/decimal_places.pyi:15:9: F811 Redefinition of unused `digits` from line 13
stubs/influxdb-client/influxdb_client/domain/delete_predicate_request.pyi:13:9: F811 Redefinition of unused `start` from line 11
stubs/influxdb-client/influxdb_client/domain/delete_predicate_request.pyi:17:9: F811 Redefinition of unused `stop` from line 15
stubs/influxdb-client/influxdb_client/domain/delete_predicate_request.pyi:21:9: F811 Redefinition of unused `predicate` from line 19
stubs/influxdb-client/influxdb_client/domain/dialect.pyi:18:9: F811 Redefinition of unused `header` from line 16
stubs/influxdb-client/influxdb_client/domain/dialect.pyi:22:9: F811 Redefinition of unused `delimiter` from line 20
stubs/influxdb-client/influxdb_client/domain/dialect.pyi:26:9: F811 Redefinition of unused `annotations` from line 24
stubs/influxdb-client/influxdb_client/domain/dialect.pyi:30:9: F811 Redefinition of unused `comment_prefix` from line 28
stubs/influxdb-client/influxdb_client/domain/dialect.pyi:34:9: F811 Redefinition of unused `date_time_format` from line 32
stubs/influxdb-client/influxdb_client/domain/dict_expression.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/dict_expression.pyi:17:9: F811 Redefinition of unused `elements` from line 15
stubs/influxdb-client/influxdb_client/domain/dict_item.pyi:11:9: F811 Redefinition of unused `type` from line 9
stubs/influxdb-client/influxdb_client/domain/dict_item.pyi:15:9: F811 Redefinition of unused `key` from line 13
stubs/influxdb-client/influxdb_client/domain/dict_item.pyi:19:9: F811 Redefinition of unused `val` from line 17
stubs/influxdb-client/influxdb_client/domain/duration.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/duration.pyi:17:9: F811 Redefinition of unused `magnitude` from line 15
stubs/influxdb-client/influxdb_client/domain/duration.pyi:21:9: F811 Redefinition of unused `unit` from line 19
stubs/influxdb-client/influxdb_client/domain/duration_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/duration_literal.pyi:17:9: F811 Redefinition of unused `values` from line 15
stubs/influxdb-client/influxdb_client/domain/error.pyi:17:9: F811 Redefinition of unused `code` from line 15
stubs/influxdb-client/influxdb_client/domain/error.pyi:21:9: F811 Redefinition of unused `message` from line 19
stubs/influxdb-client/influxdb_client/domain/error.pyi:25:9: F811 Redefinition of unused `op` from line 23
stubs/influxdb-client/influxdb_client/domain/error.pyi:29:9: F811 Redefinition of unused `err` from line 27
stubs/influxdb-client/influxdb_client/domain/expression_statement.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/expression_statement.pyi:17:9: F811 Redefinition of unused `expression` from line 15
stubs/influxdb-client/influxdb_client/domain/field.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/field.pyi:21:9: F811 Redefinition of unused `type` from line 19
stubs/influxdb-client/influxdb_client/domain/field.pyi:25:9: F811 Redefinition of unused `alias` from line 23
stubs/influxdb-client/influxdb_client/domain/field.pyi:29:9: F811 Redefinition of unused `args` from line 27
stubs/influxdb-client/influxdb_client/domain/file.pyi:18:9: F811 Redefinition of unused `type` from line 16
stubs/influxdb-client/influxdb_client/domain/file.pyi:22:9: F811 Redefinition of unused `name` from line 20
stubs/influxdb-client/influxdb_client/domain/file.pyi:26:9: F811 Redefinition of unused `package` from line 24
stubs/influxdb-client/influxdb_client/domain/file.pyi:30:9: F811 Redefinition of unused `imports` from line 28
stubs/influxdb-client/influxdb_client/domain/file.pyi:34:9: F811 Redefinition of unused `body` from line 32
stubs/influxdb-client/influxdb_client/domain/float_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/float_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/flux_response.pyi:11:9: F811 Redefinition of unused `flux` from line 9
stubs/influxdb-client/influxdb_client/domain/flux_suggestion.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/flux_suggestion.pyi:15:9: F811 Redefinition of unused `params` from line 13
stubs/influxdb-client/influxdb_client/domain/flux_suggestions.pyi:11:9: F811 Redefinition of unused `funcs` from line 9
stubs/influxdb-client/influxdb_client/domain/function_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/function_expression.pyi:19:9: F811 Redefinition of unused `params` from line 17
stubs/influxdb-client/influxdb_client/domain/function_expression.pyi:23:9: F811 Redefinition of unused `body` from line 21
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:26:9: F811 Redefinition of unused `type` from line 24
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:30:9: F811 Redefinition of unused `queries` from line 28
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:34:9: F811 Redefinition of unused `colors` from line 32
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:38:9: F811 Redefinition of unused `shape` from line 36
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:42:9: F811 Redefinition of unused `note` from line 40
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:46:9: F811 Redefinition of unused `show_note_when_empty` from line 44
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:50:9: F811 Redefinition of unused `prefix` from line 48
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:54:9: F811 Redefinition of unused `tick_prefix` from line 52
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:58:9: F811 Redefinition of unused `suffix` from line 56
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:62:9: F811 Redefinition of unused `tick_suffix` from line 60
stubs/influxdb-client/influxdb_client/domain/gauge_view_properties.pyi:66:9: F811 Redefinition of unused `decimal_places` from line 64
stubs/influxdb-client/influxdb_client/domain/greater_threshold.pyi:19:9: F811 Redefinition of unused `type` from line 17
stubs/influxdb-client/influxdb_client/domain/greater_threshold.pyi:23:9: F811 Redefinition of unused `value` from line 21
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:23:9: F811 Redefinition of unused `message` from line 21
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:27:9: F811 Redefinition of unused `checks` from line 25
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:31:9: F811 Redefinition of unused `status` from line 29
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:35:9: F811 Redefinition of unused `version` from line 33
stubs/influxdb-client/influxdb_client/domain/health_check.pyi:39:9: F811 Redefinition of unused `commit` from line 37
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:47:9: F811 Redefinition of unused `time_format` from line 45
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:51:9: F811 Redefinition of unused `type` from line 49
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:55:9: F811 Redefinition of unused `queries` from line 53
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:59:9: F811 Redefinition of unused `colors` from line 57
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:63:9: F811 Redefinition of unused `shape` from line 61
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:67:9: F811 Redefinition of unused `note` from line 65
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:71:9: F811 Redefinition of unused `show_note_when_empty` from line 69
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:75:9: F811 Redefinition of unused `x_column` from line 73
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:79:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 77
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:83:9: F811 Redefinition of unused `x_total_ticks` from line 81
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:87:9: F811 Redefinition of unused `x_tick_start` from line 85
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:91:9: F811 Redefinition of unused `x_tick_step` from line 89
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:95:9: F811 Redefinition of unused `y_column` from line 93
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:99:9: F811 Redefinition of unused `generate_y_axis_ticks` from line 97
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:103:9: F811 Redefinition of unused `y_total_ticks` from line 101
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:107:9: F811 Redefinition of unused `y_tick_start` from line 105
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:111:9: F811 Redefinition of unused `y_tick_step` from line 109
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:115:9: F811 Redefinition of unused `x_domain` from line 113
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:119:9: F811 Redefinition of unused `y_domain` from line 117
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:123:9: F811 Redefinition of unused `x_axis_label` from line 121
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:127:9: F811 Redefinition of unused `y_axis_label` from line 125
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:131:9: F811 Redefinition of unused `x_prefix` from line 129
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:135:9: F811 Redefinition of unused `x_suffix` from line 133
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:139:9: F811 Redefinition of unused `y_prefix` from line 137
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:143:9: F811 Redefinition of unused `y_suffix` from line 141
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:147:9: F811 Redefinition of unused `bin_size` from line 145
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:151:9: F811 Redefinition of unused `legend_colorize_rows` from line 149
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:155:9: F811 Redefinition of unused `legend_hide` from line 153
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:159:9: F811 Redefinition of unused `legend_opacity` from line 157
stubs/influxdb-client/influxdb_client/domain/heatmap_view_properties.pyi:163:9: F811 Redefinition of unused `legend_orientation_threshold` from line 161
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:31:9: F811 Redefinition of unused `type` from line 29
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:35:9: F811 Redefinition of unused `queries` from line 33
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:39:9: F811 Redefinition of unused `colors` from line 37
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:43:9: F811 Redefinition of unused `shape` from line 41
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:47:9: F811 Redefinition of unused `note` from line 45
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:51:9: F811 Redefinition of unused `show_note_when_empty` from line 49
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:55:9: F811 Redefinition of unused `x_column` from line 53
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:59:9: F811 Redefinition of unused `fill_columns` from line 57
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:63:9: F811 Redefinition of unused `x_domain` from line 61
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:67:9: F811 Redefinition of unused `x_axis_label` from line 65
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:71:9: F811 Redefinition of unused `position` from line 69
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:75:9: F811 Redefinition of unused `bin_count` from line 73
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:79:9: F811 Redefinition of unused `legend_colorize_rows` from line 77
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:83:9: F811 Redefinition of unused `legend_hide` from line 81
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:87:9: F811 Redefinition of unused `legend_opacity` from line 85
stubs/influxdb-client/influxdb_client/domain/histogram_view_properties.pyi:91:9: F811 Redefinition of unused `legend_orientation_threshold` from line 89
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:34:9: F811 Redefinition of unused `url` from line 32
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:38:9: F811 Redefinition of unused `username` from line 36
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:42:9: F811 Redefinition of unused `password` from line 40
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:46:9: F811 Redefinition of unused `token` from line 44
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:50:9: F811 Redefinition of unused `method` from line 48
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:54:9: F811 Redefinition of unused `auth_method` from line 52
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:58:9: F811 Redefinition of unused `content_template` from line 56
stubs/influxdb-client/influxdb_client/domain/http_notification_endpoint.pyi:62:9: F811 Redefinition of unused `headers` from line 60
stubs/influxdb-client/influxdb_client/domain/http_notification_rule_base.pyi:40:9: F811 Redefinition of unused `type` from line 38
stubs/influxdb-client/influxdb_client/domain/http_notification_rule_base.pyi:44:9: F811 Redefinition of unused `url` from line 42
stubs/influxdb-client/influxdb_client/domain/identifier.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/identifier.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/import_declaration.pyi:11:9: F811 Redefinition of unused `type` from line 9
stubs/influxdb-client/influxdb_client/domain/import_declaration.pyi:15:9: F811 Redefinition of unused `path` from line 13
stubs/influxdb-client/influxdb_client/domain/index_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/index_expression.pyi:19:9: F811 Redefinition of unused `array` from line 17
stubs/influxdb-client/influxdb_client/domain/index_expression.pyi:23:9: F811 Redefinition of unused `index` from line 21
stubs/influxdb-client/influxdb_client/domain/integer_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/integer_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/is_onboarding.pyi:11:9: F811 Redefinition of unused `allowed` from line 9
stubs/influxdb-client/influxdb_client/domain/label.pyi:17:9: F811 Redefinition of unused `id` from line 15
stubs/influxdb-client/influxdb_client/domain/label.pyi:21:9: F811 Redefinition of unused `org_id` from line 19
stubs/influxdb-client/influxdb_client/domain/label.pyi:25:9: F811 Redefinition of unused `name` from line 23
stubs/influxdb-client/influxdb_client/domain/label.pyi:29:9: F811 Redefinition of unused `properties` from line 27
stubs/influxdb-client/influxdb_client/domain/label_create_request.pyi:13:9: F811 Redefinition of unused `org_id` from line 11
stubs/influxdb-client/influxdb_client/domain/label_create_request.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/label_create_request.pyi:21:9: F811 Redefinition of unused `properties` from line 19
stubs/influxdb-client/influxdb_client/domain/label_mapping.pyi:11:9: F811 Redefinition of unused `label_id` from line 9
stubs/influxdb-client/influxdb_client/domain/label_response.pyi:11:9: F811 Redefinition of unused `label` from line 9
stubs/influxdb-client/influxdb_client/domain/label_response.pyi:15:9: F811 Redefinition of unused `links` from line 13
stubs/influxdb-client/influxdb_client/domain/label_update.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/label_update.pyi:15:9: F811 Redefinition of unused `properties` from line 13
stubs/influxdb-client/influxdb_client/domain/labels_response.pyi:11:9: F811 Redefinition of unused `labels` from line 9
stubs/influxdb-client/influxdb_client/domain/labels_response.pyi:15:9: F811 Redefinition of unused `links` from line 13
stubs/influxdb-client/influxdb_client/domain/language_request.pyi:11:9: F811 Redefinition of unused `query` from line 9
stubs/influxdb-client/influxdb_client/domain/legacy_authorization_post_request.pyi:21:9: F811 Redefinition of unused `org_id` from line 19
stubs/influxdb-client/influxdb_client/domain/legacy_authorization_post_request.pyi:25:9: F811 Redefinition of unused `user_id` from line 23
stubs/influxdb-client/influxdb_client/domain/legacy_authorization_post_request.pyi:29:9: F811 Redefinition of unused `token` from line 27
stubs/influxdb-client/influxdb_client/domain/legacy_authorization_post_request.pyi:33:9: F811 Redefinition of unused `permissions` from line 31
stubs/influxdb-client/influxdb_client/domain/lesser_threshold.pyi:19:9: F811 Redefinition of unused `type` from line 17
stubs/influxdb-client/influxdb_client/domain/lesser_threshold.pyi:23:9: F811 Redefinition of unused `value` from line 21
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:46:9: F811 Redefinition of unused `time_format` from line 44
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:50:9: F811 Redefinition of unused `type` from line 48
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:54:9: F811 Redefinition of unused `queries` from line 52
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:58:9: F811 Redefinition of unused `colors` from line 56
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:62:9: F811 Redefinition of unused `shape` from line 60
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:66:9: F811 Redefinition of unused `note` from line 64
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:70:9: F811 Redefinition of unused `show_note_when_empty` from line 68
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:74:9: F811 Redefinition of unused `axes` from line 72
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:78:9: F811 Redefinition of unused `static_legend` from line 76
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:82:9: F811 Redefinition of unused `x_column` from line 80
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:86:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 84
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:90:9: F811 Redefinition of unused `x_total_ticks` from line 88
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:94:9: F811 Redefinition of unused `x_tick_start` from line 92
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:98:9: F811 Redefinition of unused `x_tick_step` from line 96
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:102:9: F811 Redefinition of unused `y_column` from line 100
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:106:9: F811 Redefinition of unused `generate_y_axis_ticks` from line 104
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:110:9: F811 Redefinition of unused `y_total_ticks` from line 108
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:114:9: F811 Redefinition of unused `y_tick_start` from line 112
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:118:9: F811 Redefinition of unused `y_tick_step` from line 116
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:122:9: F811 Redefinition of unused `shade_below` from line 120
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:126:9: F811 Redefinition of unused `hover_dimension` from line 124
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:130:9: F811 Redefinition of unused `position` from line 128
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:134:9: F811 Redefinition of unused `prefix` from line 132
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:138:9: F811 Redefinition of unused `suffix` from line 136
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:142:9: F811 Redefinition of unused `decimal_places` from line 140
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:146:9: F811 Redefinition of unused `legend_colorize_rows` from line 144
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:150:9: F811 Redefinition of unused `legend_hide` from line 148
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:154:9: F811 Redefinition of unused `legend_opacity` from line 152
stubs/influxdb-client/influxdb_client/domain/line_plus_single_stat_properties.pyi:158:9: F811 Redefinition of unused `legend_orientation_threshold` from line 156
stubs/influxdb-client/influxdb_client/domain/line_protocol_error.pyi:18:9: F811 Redefinition of unused `code` from line 16
stubs/influxdb-client/influxdb_client/domain/line_protocol_error.pyi:22:9: F811 Redefinition of unused `message` from line 20
stubs/influxdb-client/influxdb_client/domain/line_protocol_error.pyi:26:9: F811 Redefinition of unused `op` from line 24
stubs/influxdb-client/influxdb_client/domain/line_protocol_error.pyi:30:9: F811 Redefinition of unused `err` from line 28
stubs/influxdb-client/influxdb_client/domain/line_protocol_error.pyi:34:9: F811 Redefinition of unused `line` from line 32
stubs/influxdb-client/influxdb_client/domain/line_protocol_length_error.pyi:11:9: F811 Redefinition of unused `code` from line 9
stubs/influxdb-client/influxdb_client/domain/line_protocol_length_error.pyi:15:9: F811 Redefinition of unused `message` from line 13
stubs/influxdb-client/influxdb_client/domain/links.pyi:13:9: F811 Redefinition of unused `next` from line 11
stubs/influxdb-client/influxdb_client/domain/links.pyi:17:9: F811 Redefinition of unused `prev` from line 15
stubs/influxdb-client/influxdb_client/domain/list_stacks_response.pyi:11:9: F811 Redefinition of unused `stacks` from line 9
stubs/influxdb-client/influxdb_client/domain/log_event.pyi:13:9: F811 Redefinition of unused `time` from line 11
stubs/influxdb-client/influxdb_client/domain/log_event.pyi:17:9: F811 Redefinition of unused `message` from line 15
stubs/influxdb-client/influxdb_client/domain/log_event.pyi:21:9: F811 Redefinition of unused `run_id` from line 19
stubs/influxdb-client/influxdb_client/domain/logical_expression.pyi:19:9: F811 Redefinition of unused `type` from line 17
stubs/influxdb-client/influxdb_client/domain/logical_expression.pyi:23:9: F811 Redefinition of unused `operator` from line 21
stubs/influxdb-client/influxdb_client/domain/logical_expression.pyi:27:9: F811 Redefinition of unused `left` from line 25
stubs/influxdb-client/influxdb_client/domain/logical_expression.pyi:31:9: F811 Redefinition of unused `right` from line 29
stubs/influxdb-client/influxdb_client/domain/logs.pyi:11:9: F811 Redefinition of unused `events` from line 9
stubs/influxdb-client/influxdb_client/domain/map_variable_properties.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/map_variable_properties.pyi:17:9: F811 Redefinition of unused `values` from line 15
stubs/influxdb-client/influxdb_client/domain/markdown_view_properties.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/markdown_view_properties.pyi:19:9: F811 Redefinition of unused `shape` from line 17
stubs/influxdb-client/influxdb_client/domain/markdown_view_properties.pyi:23:9: F811 Redefinition of unused `note` from line 21
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:20:9: F811 Redefinition of unused `id` from line 18
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:24:9: F811 Redefinition of unused `org_id` from line 22
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:28:9: F811 Redefinition of unused `bucket_id` from line 26
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:32:9: F811 Redefinition of unused `name` from line 30
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:36:9: F811 Redefinition of unused `columns` from line 34
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:40:9: F811 Redefinition of unused `created_at` from line 38
stubs/influxdb-client/influxdb_client/domain/measurement_schema.pyi:44:9: F811 Redefinition of unused `updated_at` from line 42
stubs/influxdb-client/influxdb_client/domain/measurement_schema_column.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/measurement_schema_column.pyi:17:9: F811 Redefinition of unused `type` from line 15
stubs/influxdb-client/influxdb_client/domain/measurement_schema_column.pyi:21:9: F811 Redefinition of unused `data_type` from line 19
stubs/influxdb-client/influxdb_client/domain/measurement_schema_create_request.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/measurement_schema_create_request.pyi:15:9: F811 Redefinition of unused `columns` from line 13
stubs/influxdb-client/influxdb_client/domain/measurement_schema_list.pyi:11:9: F811 Redefinition of unused `measurement_schemas` from line 9
stubs/influxdb-client/influxdb_client/domain/measurement_schema_update_request.pyi:11:9: F811 Redefinition of unused `columns` from line 9
stubs/influxdb-client/influxdb_client/domain/member_assignment.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/member_assignment.pyi:19:9: F811 Redefinition of unused `member` from line 17
stubs/influxdb-client/influxdb_client/domain/member_assignment.pyi:23:9: F811 Redefinition of unused `init` from line 21
stubs/influxdb-client/influxdb_client/domain/member_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/member_expression.pyi:19:9: F811 Redefinition of unused `object` from line 17
stubs/influxdb-client/influxdb_client/domain/metadata_backup.pyi:13:9: F811 Redefinition of unused `kv` from line 11
stubs/influxdb-client/influxdb_client/domain/metadata_backup.pyi:17:9: F811 Redefinition of unused `sql` from line 15
stubs/influxdb-client/influxdb_client/domain/metadata_backup.pyi:21:9: F811 Redefinition of unused `buckets` from line 19
stubs/influxdb-client/influxdb_client/domain/model_property.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/model_property.pyi:17:9: F811 Redefinition of unused `key` from line 15
stubs/influxdb-client/influxdb_client/domain/model_property.pyi:21:9: F811 Redefinition of unused `value` from line 19
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:44:9: F811 Redefinition of unused `time_format` from line 42
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:48:9: F811 Redefinition of unused `type` from line 46
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:52:9: F811 Redefinition of unused `queries` from line 50
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:56:9: F811 Redefinition of unused `colors` from line 54
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:60:9: F811 Redefinition of unused `shape` from line 58
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:64:9: F811 Redefinition of unused `note` from line 62
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:68:9: F811 Redefinition of unused `show_note_when_empty` from line 66
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:72:9: F811 Redefinition of unused `x_column` from line 70
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:76:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 74
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:80:9: F811 Redefinition of unused `x_total_ticks` from line 78
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:84:9: F811 Redefinition of unused `x_tick_start` from line 82
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:88:9: F811 Redefinition of unused `x_tick_step` from line 86
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:92:9: F811 Redefinition of unused `y_label_column_separator` from line 90
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:96:9: F811 Redefinition of unused `y_label_columns` from line 94
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:100:9: F811 Redefinition of unused `y_series_columns` from line 98
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:104:9: F811 Redefinition of unused `fill_columns` from line 102
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:108:9: F811 Redefinition of unused `x_domain` from line 106
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:112:9: F811 Redefinition of unused `y_domain` from line 110
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:116:9: F811 Redefinition of unused `x_axis_label` from line 114
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:120:9: F811 Redefinition of unused `y_axis_label` from line 118
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:124:9: F811 Redefinition of unused `x_prefix` from line 122
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:128:9: F811 Redefinition of unused `x_suffix` from line 126
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:132:9: F811 Redefinition of unused `y_prefix` from line 130
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:136:9: F811 Redefinition of unused `y_suffix` from line 134
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:140:9: F811 Redefinition of unused `hover_dimension` from line 138
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:144:9: F811 Redefinition of unused `legend_colorize_rows` from line 142
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:148:9: F811 Redefinition of unused `legend_hide` from line 146
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:152:9: F811 Redefinition of unused `legend_opacity` from line 150
stubs/influxdb-client/influxdb_client/domain/mosaic_view_properties.pyi:156:9: F811 Redefinition of unused `legend_orientation_threshold` from line 154
stubs/influxdb-client/influxdb_client/domain/notification_endpoint.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:24:9: F811 Redefinition of unused `id` from line 22
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:28:9: F811 Redefinition of unused `org_id` from line 26
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:32:9: F811 Redefinition of unused `user_id` from line 30
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:36:9: F811 Redefinition of unused `created_at` from line 34
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:40:9: F811 Redefinition of unused `updated_at` from line 38
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:44:9: F811 Redefinition of unused `description` from line 42
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:48:9: F811 Redefinition of unused `name` from line 46
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:52:9: F811 Redefinition of unused `status` from line 50
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:56:9: F811 Redefinition of unused `labels` from line 54
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:60:9: F811 Redefinition of unused `links` from line 58
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base.pyi:64:9: F811 Redefinition of unused `type` from line 62
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base_links.pyi:17:9: F811 Redefinition of unused `labels` from line 15
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base_links.pyi:21:9: F811 Redefinition of unused `members` from line 19
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_base_links.pyi:25:9: F811 Redefinition of unused `owners` from line 23
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_update.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_update.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/notification_endpoint_update.pyi:21:9: F811 Redefinition of unused `status` from line 19
stubs/influxdb-client/influxdb_client/domain/notification_endpoints.pyi:11:9: F811 Redefinition of unused `notification_endpoints` from line 9
stubs/influxdb-client/influxdb_client/domain/notification_endpoints.pyi:15:9: F811 Redefinition of unused `links` from line 13
stubs/influxdb-client/influxdb_client/domain/notification_rule.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:36:9: F811 Redefinition of unused `latest_completed` from line 34
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:40:9: F811 Redefinition of unused `last_run_status` from line 38
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:44:9: F811 Redefinition of unused `last_run_error` from line 42
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:48:9: F811 Redefinition of unused `id` from line 46
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:52:9: F811 Redefinition of unused `endpoint_id` from line 50
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:56:9: F811 Redefinition of unused `org_id` from line 54
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:60:9: F811 Redefinition of unused `task_id` from line 58
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:64:9: F811 Redefinition of unused `owner_id` from line 62
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:68:9: F811 Redefinition of unused `created_at` from line 66
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:72:9: F811 Redefinition of unused `updated_at` from line 70
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:76:9: F811 Redefinition of unused `status` from line 74
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:80:9: F811 Redefinition of unused `name` from line 78
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:84:9: F811 Redefinition of unused `sleep_until` from line 82
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:88:9: F811 Redefinition of unused `every` from line 86
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:92:9: F811 Redefinition of unused `offset` from line 90
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:96:9: F811 Redefinition of unused `runbook_link` from line 94
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:100:9: F811 Redefinition of unused `limit_every` from line 98
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:104:9: F811 Redefinition of unused `limit` from line 102
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:108:9: F811 Redefinition of unused `tag_rules` from line 106
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:112:9: F811 Redefinition of unused `description` from line 110
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:116:9: F811 Redefinition of unused `status_rules` from line 114
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:120:9: F811 Redefinition of unused `labels` from line 118
stubs/influxdb-client/influxdb_client/domain/notification_rule_base.pyi:124:9: F811 Redefinition of unused `links` from line 122
stubs/influxdb-client/influxdb_client/domain/notification_rule_base_links.pyi:18:9: F811 Redefinition of unused `labels` from line 16
stubs/influxdb-client/influxdb_client/domain/notification_rule_base_links.pyi:22:9: F811 Redefinition of unused `members` from line 20
stubs/influxdb-client/influxdb_client/domain/notification_rule_base_links.pyi:26:9: F811 Redefinition of unused `owners` from line 24
stubs/influxdb-client/influxdb_client/domain/notification_rule_base_links.pyi:30:9: F811 Redefinition of unused `query` from line 28
stubs/influxdb-client/influxdb_client/domain/notification_rule_update.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/notification_rule_update.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/notification_rule_update.pyi:21:9: F811 Redefinition of unused `status` from line 19
stubs/influxdb-client/influxdb_client/domain/notification_rules.pyi:11:9: F811 Redefinition of unused `notification_rules` from line 9
stubs/influxdb-client/influxdb_client/domain/notification_rules.pyi:15:9: F811 Redefinition of unused `links` from line 13
stubs/influxdb-client/influxdb_client/domain/object_expression.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/object_expression.pyi:17:9: F811 Redefinition of unused `properties` from line 15
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:20:9: F811 Redefinition of unused `username` from line 18
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:24:9: F811 Redefinition of unused `password` from line 22
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:28:9: F811 Redefinition of unused `org` from line 26
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:32:9: F811 Redefinition of unused `bucket` from line 30
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:36:9: F811 Redefinition of unused `retention_period_seconds` from line 34
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:40:9: F811 Redefinition of unused `retention_period_hrs` from line 38
stubs/influxdb-client/influxdb_client/domain/onboarding_request.pyi:44:9: F811 Redefinition of unused `token` from line 42
stubs/influxdb-client/influxdb_client/domain/onboarding_response.pyi:17:9: F811 Redefinition of unused `user` from line 15
stubs/influxdb-client/influxdb_client/domain/onboarding_response.pyi:21:9: F811 Redefinition of unused `org` from line 19
stubs/influxdb-client/influxdb_client/domain/onboarding_response.pyi:25:9: F811 Redefinition of unused `bucket` from line 23
stubs/influxdb-client/influxdb_client/domain/onboarding_response.pyi:29:9: F811 Redefinition of unused `auth` from line 27
stubs/influxdb-client/influxdb_client/domain/option_statement.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/option_statement.pyi:17:9: F811 Redefinition of unused `assignment` from line 15
stubs/influxdb-client/influxdb_client/domain/organization.pyi:21:9: F811 Redefinition of unused `links` from line 19
stubs/influxdb-client/influxdb_client/domain/organization.pyi:25:9: F811 Redefinition of unused `id` from line 23
stubs/influxdb-client/influxdb_client/domain/organization.pyi:29:9: F811 Redefinition of unused `name` from line 27
stubs/influxdb-client/influxdb_client/domain/organization.pyi:34:9: F811 Redefinition of unused `description` from line 32
stubs/influxdb-client/influxdb_client/domain/organization.pyi:38:9: F811 Redefinition of unused `created_at` from line 36
stubs/influxdb-client/influxdb_client/domain/organization.pyi:42:9: F811 Redefinition of unused `updated_at` from line 40
stubs/influxdb-client/influxdb_client/domain/organization.pyi:46:9: F811 Redefinition of unused `status` from line 44
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:21:9: F811 Redefinition of unused `members` from line 19
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:25:9: F811 Redefinition of unused `owners` from line 23
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:29:9: F811 Redefinition of unused `labels` from line 27
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:33:9: F811 Redefinition of unused `secrets` from line 31
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:37:9: F811 Redefinition of unused `buckets` from line 35
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:41:9: F811 Redefinition of unused `tasks` from line 39
stubs/influxdb-client/influxdb_client/domain/organization_links.pyi:45:9: F811 Redefinition of unused `dashboards` from line 43
stubs/influxdb-client/influxdb_client/domain/organizations.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/organizations.pyi:15:9: F811 Redefinition of unused `orgs` from line 13
stubs/influxdb-client/influxdb_client/domain/package.pyi:17:9: F811 Redefinition of unused `type` from line 15
stubs/influxdb-client/influxdb_client/domain/package.pyi:21:9: F811 Redefinition of unused `path` from line 19
stubs/influxdb-client/influxdb_client/domain/package.pyi:25:9: F811 Redefinition of unused `package` from line 23
stubs/influxdb-client/influxdb_client/domain/package.pyi:29:9: F811 Redefinition of unused `files` from line 27
stubs/influxdb-client/influxdb_client/domain/package_clause.pyi:11:9: F811 Redefinition of unused `type` from line 9
stubs/influxdb-client/influxdb_client/domain/package_clause.pyi:15:9: F811 Redefinition of unused `name` from line 13
stubs/influxdb-client/influxdb_client/domain/pager_duty_notification_endpoint.pyi:28:9: F811 Redefinition of unused `client_url` from line 26
stubs/influxdb-client/influxdb_client/domain/pager_duty_notification_endpoint.pyi:32:9: F811 Redefinition of unused `routing_key` from line 30
stubs/influxdb-client/influxdb_client/domain/pager_duty_notification_rule_base.pyi:40:9: F811 Redefinition of unused `type` from line 38
stubs/influxdb-client/influxdb_client/domain/pager_duty_notification_rule_base.pyi:44:9: F811 Redefinition of unused `message_template` from line 42
stubs/influxdb-client/influxdb_client/domain/paren_expression.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/paren_expression.pyi:17:9: F811 Redefinition of unused `expression` from line 15
stubs/influxdb-client/influxdb_client/domain/password_reset_body.pyi:11:9: F811 Redefinition of unused `password` from line 9
stubs/influxdb-client/influxdb_client/domain/patch_bucket_request.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/patch_bucket_request.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/patch_bucket_request.pyi:21:9: F811 Redefinition of unused `retention_rules` from line 19
stubs/influxdb-client/influxdb_client/domain/patch_dashboard_request.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/patch_dashboard_request.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/patch_dashboard_request.pyi:21:9: F811 Redefinition of unused `cells` from line 19
stubs/influxdb-client/influxdb_client/domain/patch_organization_request.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/patch_organization_request.pyi:15:9: F811 Redefinition of unused `description` from line 13
stubs/influxdb-client/influxdb_client/domain/patch_retention_rule.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/patch_retention_rule.pyi:18:9: F811 Redefinition of unused `shard_group_duration_seconds` from line 16
stubs/influxdb-client/influxdb_client/domain/patch_stack_request.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/patch_stack_request.pyi:21:9: F811 Redefinition of unused `description` from line 19
stubs/influxdb-client/influxdb_client/domain/patch_stack_request.pyi:25:9: F811 Redefinition of unused `template_ur_ls` from line 23
stubs/influxdb-client/influxdb_client/domain/patch_stack_request.pyi:29:9: F811 Redefinition of unused `additional_resources` from line 27
stubs/influxdb-client/influxdb_client/domain/patch_stack_request_additional_resources.pyi:13:9: F811 Redefinition of unused `resource_id` from line 11
stubs/influxdb-client/influxdb_client/domain/patch_stack_request_additional_resources.pyi:17:9: F811 Redefinition of unused `kind` from line 15
stubs/influxdb-client/influxdb_client/domain/patch_stack_request_additional_resources.pyi:21:9: F811 Redefinition of unused `template_meta_name` from line 19
stubs/influxdb-client/influxdb_client/domain/permission.pyi:11:9: F811 Redefinition of unused `action` from line 9
stubs/influxdb-client/influxdb_client/domain/permission.pyi:15:9: F811 Redefinition of unused `resource` from line 13
stubs/influxdb-client/influxdb_client/domain/permission_resource.pyi:18:9: F811 Redefinition of unused `type` from line 16
stubs/influxdb-client/influxdb_client/domain/permission_resource.pyi:22:9: F811 Redefinition of unused `id` from line 20
stubs/influxdb-client/influxdb_client/domain/permission_resource.pyi:26:9: F811 Redefinition of unused `name` from line 24
stubs/influxdb-client/influxdb_client/domain/permission_resource.pyi:30:9: F811 Redefinition of unused `org_id` from line 28
stubs/influxdb-client/influxdb_client/domain/permission_resource.pyi:34:9: F811 Redefinition of unused `org` from line 32
stubs/influxdb-client/influxdb_client/domain/pipe_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/pipe_expression.pyi:19:9: F811 Redefinition of unused `argument` from line 17
stubs/influxdb-client/influxdb_client/domain/pipe_expression.pyi:23:9: F811 Redefinition of unused `call` from line 21
stubs/influxdb-client/influxdb_client/domain/pipe_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/post_bucket_request.pyi:19:9: F811 Redefinition of unused `org_id` from line 17
stubs/influxdb-client/influxdb_client/domain/post_bucket_request.pyi:23:9: F811 Redefinition of unused `name` from line 21
stubs/influxdb-client/influxdb_client/domain/post_bucket_request.pyi:27:9: F811 Redefinition of unused `description` from line 25
stubs/influxdb-client/influxdb_client/domain/post_bucket_request.pyi:32:9: F811 Redefinition of unused `retention_rules` from line 30
stubs/influxdb-client/influxdb_client/domain/post_bucket_request.pyi:36:9: F811 Redefinition of unused `schema_type` from line 34
stubs/influxdb-client/influxdb_client/domain/post_check.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/post_notification_endpoint.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/post_notification_rule.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/post_organization_request.pyi:11:9: F811 Redefinition of unused `name` from line 9
stubs/influxdb-client/influxdb_client/domain/post_organization_request.pyi:15:9: F811 Redefinition of unused `description` from line 13
stubs/influxdb-client/influxdb_client/domain/post_restore_kv_response.pyi:11:9: F811 Redefinition of unused `token` from line 9
stubs/influxdb-client/influxdb_client/domain/post_stack_request.pyi:17:9: F811 Redefinition of unused `org_id` from line 15
stubs/influxdb-client/influxdb_client/domain/post_stack_request.pyi:21:9: F811 Redefinition of unused `name` from line 19
stubs/influxdb-client/influxdb_client/domain/post_stack_request.pyi:25:9: F811 Redefinition of unused `description` from line 23
stubs/influxdb-client/influxdb_client/domain/post_stack_request.pyi:29:9: F811 Redefinition of unused `urls` from line 27
stubs/influxdb-client/influxdb_client/domain/query.pyi:19:9: F811 Redefinition of unused `extern` from line 17
stubs/influxdb-client/influxdb_client/domain/query.pyi:23:9: F811 Redefinition of unused `query` from line 21
stubs/influxdb-client/influxdb_client/domain/query.pyi:27:9: F811 Redefinition of unused `type` from line 25
stubs/influxdb-client/influxdb_client/domain/query.pyi:31:9: F811 Redefinition of unused `params` from line 29
stubs/influxdb-client/influxdb_client/domain/query.pyi:35:9: F811 Redefinition of unused `dialect` from line 33
stubs/influxdb-client/influxdb_client/domain/query.pyi:39:9: F811 Redefinition of unused `now` from line 37
stubs/influxdb-client/influxdb_client/domain/query_variable_properties.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/query_variable_properties.pyi:17:9: F811 Redefinition of unused `values` from line 15
stubs/influxdb-client/influxdb_client/domain/query_variable_properties_values.pyi:11:9: F811 Redefinition of unused `query` from line 9
stubs/influxdb-client/influxdb_client/domain/query_variable_properties_values.pyi:15:9: F811 Redefinition of unused `language` from line 13
stubs/influxdb-client/influxdb_client/domain/range_threshold.pyi:21:9: F811 Redefinition of unused `type` from line 19
stubs/influxdb-client/influxdb_client/domain/range_threshold.pyi:25:9: F811 Redefinition of unused `min` from line 23
stubs/influxdb-client/influxdb_client/domain/range_threshold.pyi:29:9: F811 Redefinition of unused `max` from line 27
stubs/influxdb-client/influxdb_client/domain/range_threshold.pyi:33:9: F811 Redefinition of unused `within` from line 31
stubs/influxdb-client/influxdb_client/domain/ready.pyi:13:9: F811 Redefinition of unused `status` from line 11
stubs/influxdb-client/influxdb_client/domain/ready.pyi:17:9: F811 Redefinition of unused `started` from line 15
stubs/influxdb-client/influxdb_client/domain/ready.pyi:21:9: F811 Redefinition of unused `up` from line 19
stubs/influxdb-client/influxdb_client/domain/regexp_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/regexp_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:20:9: F811 Redefinition of unused `id` from line 18
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:24:9: F811 Redefinition of unused `name` from line 22
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:28:9: F811 Redefinition of unused `org_id` from line 26
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:32:9: F811 Redefinition of unused `description` from line 30
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:36:9: F811 Redefinition of unused `remote_url` from line 34
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:40:9: F811 Redefinition of unused `remote_org_id` from line 38
stubs/influxdb-client/influxdb_client/domain/remote_connection.pyi:44:9: F811 Redefinition of unused `allow_insecure_tls` from line 42
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:20:9: F811 Redefinition of unused `name` from line 18
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:24:9: F811 Redefinition of unused `description` from line 22
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:28:9: F811 Redefinition of unused `org_id` from line 26
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:32:9: F811 Redefinition of unused `remote_url` from line 30
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:36:9: F811 Redefinition of unused `remote_api_token` from line 34
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:40:9: F811 Redefinition of unused `remote_org_id` from line 38
stubs/influxdb-client/influxdb_client/domain/remote_connection_creation_request.pyi:44:9: F811 Redefinition of unused `allow_insecure_tls` from line 42
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:23:9: F811 Redefinition of unused `description` from line 21
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:27:9: F811 Redefinition of unused `remote_url` from line 25
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:31:9: F811 Redefinition of unused `remote_api_token` from line 29
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:35:9: F811 Redefinition of unused `remote_org_id` from line 33
stubs/influxdb-client/influxdb_client/domain/remote_connection_update_request.pyi:39:9: F811 Redefinition of unused `allow_insecure_tls` from line 37
stubs/influxdb-client/influxdb_client/domain/remote_connections.pyi:11:9: F811 Redefinition of unused `remotes` from line 9
stubs/influxdb-client/influxdb_client/domain/renamable_field.pyi:13:9: F811 Redefinition of unused `internal_name` from line 11
stubs/influxdb-client/influxdb_client/domain/renamable_field.pyi:17:9: F811 Redefinition of unused `display_name` from line 15
stubs/influxdb-client/influxdb_client/domain/renamable_field.pyi:21:9: F811 Redefinition of unused `visible` from line 19
stubs/influxdb-client/influxdb_client/domain/replication.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/replication.pyi:31:9: F811 Redefinition of unused `name` from line 29
stubs/influxdb-client/influxdb_client/domain/replication.pyi:35:9: F811 Redefinition of unused `description` from line 33
stubs/influxdb-client/influxdb_client/domain/replication.pyi:39:9: F811 Redefinition of unused `org_id` from line 37
stubs/influxdb-client/influxdb_client/domain/replication.pyi:43:9: F811 Redefinition of unused `remote_id` from line 41
stubs/influxdb-client/influxdb_client/domain/replication.pyi:47:9: F811 Redefinition of unused `local_bucket_id` from line 45
stubs/influxdb-client/influxdb_client/domain/replication.pyi:51:9: F811 Redefinition of unused `remote_bucket_id` from line 49
stubs/influxdb-client/influxdb_client/domain/replication.pyi:56:9: F811 Redefinition of unused `max_queue_size_bytes` from line 54
stubs/influxdb-client/influxdb_client/domain/replication.pyi:60:9: F811 Redefinition of unused `current_queue_size_bytes` from line 58
stubs/influxdb-client/influxdb_client/domain/replication.pyi:65:9: F811 Redefinition of unused `latest_response_code` from line 63
stubs/influxdb-client/influxdb_client/domain/replication.pyi:69:9: F811 Redefinition of unused `latest_error_message` from line 67
stubs/influxdb-client/influxdb_client/domain/replication.pyi:73:9: F811 Redefinition of unused `drop_non_retryable_data` from line 71
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:23:9: F811 Redefinition of unused `name` from line 21
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:27:9: F811 Redefinition of unused `description` from line 25
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:31:9: F811 Redefinition of unused `org_id` from line 29
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:35:9: F811 Redefinition of unused `remote_id` from line 33
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:39:9: F811 Redefinition of unused `local_bucket_id` from line 37
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:43:9: F811 Redefinition of unused `remote_bucket_id` from line 41
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:48:9: F811 Redefinition of unused `max_queue_size_bytes` from line 46
stubs/influxdb-client/influxdb_client/domain/replication_creation_request.pyi:52:9: F811 Redefinition of unused `drop_non_retryable_data` from line 50
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:21:9: F811 Redefinition of unused `name` from line 19
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:25:9: F811 Redefinition of unused `description` from line 23
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:29:9: F811 Redefinition of unused `remote_id` from line 27
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:33:9: F811 Redefinition of unused `remote_bucket_id` from line 31
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:38:9: F811 Redefinition of unused `max_queue_size_bytes` from line 36
stubs/influxdb-client/influxdb_client/domain/replication_update_request.pyi:42:9: F811 Redefinition of unused `drop_non_retryable_data` from line 40
stubs/influxdb-client/influxdb_client/domain/replications.pyi:11:9: F811 Redefinition of unused `replications` from line 9
stubs/influxdb-client/influxdb_client/domain/resource_member.pyi:20:9: F811 Redefinition of unused `role` from line 18
stubs/influxdb-client/influxdb_client/domain/resource_members.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/resource_members.pyi:15:9: F811 Redefinition of unused `users` from line 13
stubs/influxdb-client/influxdb_client/domain/resource_owner.pyi:20:9: F811 Redefinition of unused `role` from line 18
stubs/influxdb-client/influxdb_client/domain/resource_owners.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/resource_owners.pyi:15:9: F811 Redefinition of unused `users` from line 13
stubs/influxdb-client/influxdb_client/domain/restored_bucket_mappings.pyi:13:9: F811 Redefinition of unused `id` from line 11
stubs/influxdb-client/influxdb_client/domain/restored_bucket_mappings.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/restored_bucket_mappings.pyi:21:9: F811 Redefinition of unused `shard_mappings` from line 19
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:23:9: F811 Redefinition of unused `replica_n` from line 21
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:27:9: F811 Redefinition of unused `duration` from line 25
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:31:9: F811 Redefinition of unused `shard_group_duration` from line 29
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:35:9: F811 Redefinition of unused `shard_groups` from line 33
stubs/influxdb-client/influxdb_client/domain/retention_policy_manifest.pyi:39:9: F811 Redefinition of unused `subscriptions` from line 37
stubs/influxdb-client/influxdb_client/domain/return_statement.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/return_statement.pyi:17:9: F811 Redefinition of unused `argument` from line 15
stubs/influxdb-client/influxdb_client/domain/routes.pyi:31:9: F811 Redefinition of unused `authorizations` from line 29
stubs/influxdb-client/influxdb_client/domain/routes.pyi:35:9: F811 Redefinition of unused `buckets` from line 33
stubs/influxdb-client/influxdb_client/domain/routes.pyi:39:9: F811 Redefinition of unused `dashboards` from line 37
stubs/influxdb-client/influxdb_client/domain/routes.pyi:43:9: F811 Redefinition of unused `external` from line 41
stubs/influxdb-client/influxdb_client/domain/routes.pyi:47:9: F811 Redefinition of unused `variables` from line 45
stubs/influxdb-client/influxdb_client/domain/routes.pyi:51:9: F811 Redefinition of unused `me` from line 49
stubs/influxdb-client/influxdb_client/domain/routes.pyi:55:9: F811 Redefinition of unused `flags` from line 53
stubs/influxdb-client/influxdb_client/domain/routes.pyi:59:9: F811 Redefinition of unused `orgs` from line 57
stubs/influxdb-client/influxdb_client/domain/routes.pyi:63:9: F811 Redefinition of unused `query` from line 61
stubs/influxdb-client/influxdb_client/domain/routes.pyi:67:9: F811 Redefinition of unused `setup` from line 65
stubs/influxdb-client/influxdb_client/domain/routes.pyi:71:9: F811 Redefinition of unused `signin` from line 69
stubs/influxdb-client/influxdb_client/domain/routes.pyi:75:9: F811 Redefinition of unused `signout` from line 73
stubs/influxdb-client/influxdb_client/domain/routes.pyi:79:9: F811 Redefinition of unused `sources` from line 77
stubs/influxdb-client/influxdb_client/domain/routes.pyi:83:9: F811 Redefinition of unused `system` from line 81
stubs/influxdb-client/influxdb_client/domain/routes.pyi:87:9: F811 Redefinition of unused `tasks` from line 85
stubs/influxdb-client/influxdb_client/domain/routes.pyi:91:9: F811 Redefinition of unused `telegrafs` from line 89
stubs/influxdb-client/influxdb_client/domain/routes.pyi:95:9: F811 Redefinition of unused `users` from line 93
stubs/influxdb-client/influxdb_client/domain/routes.pyi:99:9: F811 Redefinition of unused `write` from line 97
stubs/influxdb-client/influxdb_client/domain/routes_external.pyi:11:9: F811 Redefinition of unused `status_feed` from line 9
stubs/influxdb-client/influxdb_client/domain/routes_query.pyi:17:9: F811 Redefinition of unused `ast` from line 15
stubs/influxdb-client/influxdb_client/domain/routes_query.pyi:21:9: F811 Redefinition of unused `analyze` from line 19
stubs/influxdb-client/influxdb_client/domain/routes_query.pyi:25:9: F811 Redefinition of unused `suggestions` from line 23
stubs/influxdb-client/influxdb_client/domain/routes_system.pyi:13:9: F811 Redefinition of unused `metrics` from line 11
stubs/influxdb-client/influxdb_client/domain/routes_system.pyi:17:9: F811 Redefinition of unused `debug` from line 15
stubs/influxdb-client/influxdb_client/domain/routes_system.pyi:21:9: F811 Redefinition of unused `health` from line 19
stubs/influxdb-client/influxdb_client/domain/run.pyi:23:9: F811 Redefinition of unused `id` from line 21
stubs/influxdb-client/influxdb_client/domain/run.pyi:27:9: F811 Redefinition of unused `task_id` from line 25
stubs/influxdb-client/influxdb_client/domain/run.pyi:31:9: F811 Redefinition of unused `status` from line 29
stubs/influxdb-client/influxdb_client/domain/run.pyi:35:9: F811 Redefinition of unused `scheduled_for` from line 33
stubs/influxdb-client/influxdb_client/domain/run.pyi:39:9: F811 Redefinition of unused `log` from line 37
stubs/influxdb-client/influxdb_client/domain/run.pyi:44:9: F811 Redefinition of unused `started_at` from line 42
stubs/influxdb-client/influxdb_client/domain/run.pyi:48:9: F811 Redefinition of unused `finished_at` from line 46
stubs/influxdb-client/influxdb_client/domain/run.pyi:52:9: F811 Redefinition of unused `requested_at` from line 50
stubs/influxdb-client/influxdb_client/domain/run.pyi:56:9: F811 Redefinition of unused `links` from line 54
stubs/influxdb-client/influxdb_client/domain/run_links.pyi:13:9: F811 Redefinition of unused `task` from line 11
stubs/influxdb-client/influxdb_client/domain/run_links.pyi:17:9: F811 Redefinition of unused `retry` from line 15
stubs/influxdb-client/influxdb_client/domain/run_manually.pyi:11:9: F811 Redefinition of unused `scheduled_for` from line 9
stubs/influxdb-client/influxdb_client/domain/runs.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/runs.pyi:15:9: F811 Redefinition of unused `runs` from line 13
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:48:9: F811 Redefinition of unused `time_format` from line 46
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:52:9: F811 Redefinition of unused `type` from line 50
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:56:9: F811 Redefinition of unused `queries` from line 54
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:60:9: F811 Redefinition of unused `colors` from line 58
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:64:9: F811 Redefinition of unused `shape` from line 62
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:68:9: F811 Redefinition of unused `note` from line 66
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:72:9: F811 Redefinition of unused `show_note_when_empty` from line 70
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:76:9: F811 Redefinition of unused `x_column` from line 74
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:80:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 78
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:84:9: F811 Redefinition of unused `x_total_ticks` from line 82
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:88:9: F811 Redefinition of unused `x_tick_start` from line 86
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:92:9: F811 Redefinition of unused `x_tick_step` from line 90
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:96:9: F811 Redefinition of unused `y_column` from line 94
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:100:9: F811 Redefinition of unused `generate_y_axis_ticks` from line 98
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:104:9: F811 Redefinition of unused `y_total_ticks` from line 102
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:108:9: F811 Redefinition of unused `y_tick_start` from line 106
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:112:9: F811 Redefinition of unused `y_tick_step` from line 110
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:116:9: F811 Redefinition of unused `fill_columns` from line 114
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:120:9: F811 Redefinition of unused `symbol_columns` from line 118
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:124:9: F811 Redefinition of unused `x_domain` from line 122
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:128:9: F811 Redefinition of unused `y_domain` from line 126
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:132:9: F811 Redefinition of unused `x_axis_label` from line 130
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:136:9: F811 Redefinition of unused `y_axis_label` from line 134
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:140:9: F811 Redefinition of unused `x_prefix` from line 138
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:144:9: F811 Redefinition of unused `x_suffix` from line 142
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:148:9: F811 Redefinition of unused `y_prefix` from line 146
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:152:9: F811 Redefinition of unused `y_suffix` from line 150
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:156:9: F811 Redefinition of unused `legend_colorize_rows` from line 154
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:160:9: F811 Redefinition of unused `legend_hide` from line 158
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:164:9: F811 Redefinition of unused `legend_opacity` from line 162
stubs/influxdb-client/influxdb_client/domain/scatter_view_properties.pyi:168:9: F811 Redefinition of unused `legend_orientation_threshold` from line 166
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:23:9: F811 Redefinition of unused `type` from line 21
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:27:9: F811 Redefinition of unused `url` from line 25
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:31:9: F811 Redefinition of unused `org_id` from line 29
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:35:9: F811 Redefinition of unused `bucket_id` from line 33
stubs/influxdb-client/influxdb_client/domain/scraper_target_request.pyi:39:9: F811 Redefinition of unused `allow_insecure` from line 37
stubs/influxdb-client/influxdb_client/domain/scraper_target_response.pyi:25:9: F811 Redefinition of unused `id` from line 23
stubs/influxdb-client/influxdb_client/domain/scraper_target_response.pyi:29:9: F811 Redefinition of unused `org` from line 27
stubs/influxdb-client/influxdb_client/domain/scraper_target_response.pyi:33:9: F811 Redefinition of unused `bucket` from line 31
stubs/influxdb-client/influxdb_client/domain/scraper_target_response.pyi:37:9: F811 Redefinition of unused `links` from line 35
stubs/influxdb-client/influxdb_client/domain/scraper_target_responses.pyi:11:9: F811 Redefinition of unused `configurations` from line 9
stubs/influxdb-client/influxdb_client/domain/script.pyi:22:9: F811 Redefinition of unused `id` from line 20
stubs/influxdb-client/influxdb_client/domain/script.pyi:26:9: F811 Redefinition of unused `name` from line 24
stubs/influxdb-client/influxdb_client/domain/script.pyi:30:9: F811 Redefinition of unused `description` from line 28
stubs/influxdb-client/influxdb_client/domain/script.pyi:34:9: F811 Redefinition of unused `org_id` from line 32
stubs/influxdb-client/influxdb_client/domain/script.pyi:38:9: F811 Redefinition of unused `script` from line 36
stubs/influxdb-client/influxdb_client/domain/script.pyi:42:9: F811 Redefinition of unused `language` from line 40
stubs/influxdb-client/influxdb_client/domain/script.pyi:46:9: F811 Redefinition of unused `url` from line 44
stubs/influxdb-client/influxdb_client/domain/script.pyi:50:9: F811 Redefinition of unused `created_at` from line 48
stubs/influxdb-client/influxdb_client/domain/script.pyi:54:9: F811 Redefinition of unused `updated_at` from line 52
stubs/influxdb-client/influxdb_client/domain/script_create_request.pyi:17:9: F811 Redefinition of unused `name` from line 15
stubs/influxdb-client/influxdb_client/domain/script_create_request.pyi:21:9: F811 Redefinition of unused `description` from line 19
stubs/influxdb-client/influxdb_client/domain/script_create_request.pyi:25:9: F811 Redefinition of unused `script` from line 23
stubs/influxdb-client/influxdb_client/domain/script_create_request.pyi:29:9: F811 Redefinition of unused `language` from line 27
stubs/influxdb-client/influxdb_client/domain/script_invocation_params.pyi:11:9: F811 Redefinition of unused `params` from line 9
stubs/influxdb-client/influxdb_client/domain/script_update_request.pyi:11:9: F811 Redefinition of unused `description` from line 9
stubs/influxdb-client/influxdb_client/domain/script_update_request.pyi:15:9: F811 Redefinition of unused `script` from line 13
stubs/influxdb-client/influxdb_client/domain/scripts.pyi:11:9: F811 Redefinition of unused `scripts` from line 9
stubs/influxdb-client/influxdb_client/domain/secret_keys.pyi:11:9: F811 Redefinition of unused `secrets` from line 9
stubs/influxdb-client/influxdb_client/domain/secret_keys_response.pyi:13:9: F811 Redefinition of unused `links` from line 11
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:19:9: F811 Redefinition of unused `id` from line 17
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:23:9: F811 Redefinition of unused `start_time` from line 21
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:27:9: F811 Redefinition of unused `end_time` from line 25
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:31:9: F811 Redefinition of unused `deleted_at` from line 29
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:35:9: F811 Redefinition of unused `truncated_at` from line 33
stubs/influxdb-client/influxdb_client/domain/shard_group_manifest.pyi:39:9: F811 Redefinition of unused `shards` from line 37
stubs/influxdb-client/influxdb_client/domain/shard_manifest.pyi:11:9: F811 Redefinition of unused `id` from line 9
stubs/influxdb-client/influxdb_client/domain/shard_manifest.pyi:15:9: F811 Redefinition of unused `shard_owners` from line 13
stubs/influxdb-client/influxdb_client/domain/shard_owner.pyi:11:9: F811 Redefinition of unused `node_id` from line 9
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:21:9: F811 Redefinition of unused `type` from line 19
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:25:9: F811 Redefinition of unused `show_all` from line 23
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:29:9: F811 Redefinition of unused `queries` from line 27
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:33:9: F811 Redefinition of unused `shape` from line 31
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:37:9: F811 Redefinition of unused `note` from line 35
stubs/influxdb-client/influxdb_client/domain/simple_table_view_properties.pyi:41:9: F811 Redefinition of unused `show_note_when_empty` from line 39
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:27:9: F811 Redefinition of unused `type` from line 25
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:31:9: F811 Redefinition of unused `queries` from line 29
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:35:9: F811 Redefinition of unused `colors` from line 33
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:39:9: F811 Redefinition of unused `shape` from line 37
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:43:9: F811 Redefinition of unused `note` from line 41
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:47:9: F811 Redefinition of unused `show_note_when_empty` from line 45
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:51:9: F811 Redefinition of unused `prefix` from line 49
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:55:9: F811 Redefinition of unused `tick_prefix` from line 53
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:59:9: F811 Redefinition of unused `suffix` from line 57
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:63:9: F811 Redefinition of unused `tick_suffix` from line 61
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:67:9: F811 Redefinition of unused `static_legend` from line 65
stubs/influxdb-client/influxdb_client/domain/single_stat_view_properties.pyi:71:9: F811 Redefinition of unused `decimal_places` from line 69
stubs/influxdb-client/influxdb_client/domain/slack_notification_endpoint.pyi:28:9: F811 Redefinition of unused `url` from line 26
stubs/influxdb-client/influxdb_client/domain/slack_notification_endpoint.pyi:32:9: F811 Redefinition of unused `token` from line 30
stubs/influxdb-client/influxdb_client/domain/slack_notification_rule_base.pyi:41:9: F811 Redefinition of unused `type` from line 39
stubs/influxdb-client/influxdb_client/domain/slack_notification_rule_base.pyi:45:9: F811 Redefinition of unused `channel` from line 43
stubs/influxdb-client/influxdb_client/domain/slack_notification_rule_base.pyi:49:9: F811 Redefinition of unused `message_template` from line 47
stubs/influxdb-client/influxdb_client/domain/smtp_notification_rule_base.pyi:42:9: F811 Redefinition of unused `type` from line 40
stubs/influxdb-client/influxdb_client/domain/smtp_notification_rule_base.pyi:46:9: F811 Redefinition of unused `subject_template` from line 44
stubs/influxdb-client/influxdb_client/domain/smtp_notification_rule_base.pyi:50:9: F811 Redefinition of unused `body_template` from line 48
stubs/influxdb-client/influxdb_client/domain/smtp_notification_rule_base.pyi:54:9: F811 Redefinition of unused `to` from line 52
stubs/influxdb-client/influxdb_client/domain/source.pyi:29:9: F811 Redefinition of unused `links` from line 27
stubs/influxdb-client/influxdb_client/domain/source.pyi:33:9: F811 Redefinition of unused `id` from line 31
stubs/influxdb-client/influxdb_client/domain/source.pyi:37:9: F811 Redefinition of unused `org_id` from line 35
stubs/influxdb-client/influxdb_client/domain/source.pyi:41:9: F811 Redefinition of unused `default` from line 39
stubs/influxdb-client/influxdb_client/domain/source.pyi:45:9: F811 Redefinition of unused `name` from line 43
stubs/influxdb-client/influxdb_client/domain/source.pyi:49:9: F811 Redefinition of unused `type` from line 47
stubs/influxdb-client/influxdb_client/domain/source.pyi:53:9: F811 Redefinition of unused `url` from line 51
stubs/influxdb-client/influxdb_client/domain/source.pyi:57:9: F811 Redefinition of unused `insecure_skip_verify` from line 55
stubs/influxdb-client/influxdb_client/domain/source.pyi:61:9: F811 Redefinition of unused `telegraf` from line 59
stubs/influxdb-client/influxdb_client/domain/source.pyi:65:9: F811 Redefinition of unused `token` from line 63
stubs/influxdb-client/influxdb_client/domain/source.pyi:69:9: F811 Redefinition of unused `username` from line 67
stubs/influxdb-client/influxdb_client/domain/source.pyi:73:9: F811 Redefinition of unused `password` from line 71
stubs/influxdb-client/influxdb_client/domain/source.pyi:77:9: F811 Redefinition of unused `shared_secret` from line 75
stubs/influxdb-client/influxdb_client/domain/source.pyi:81:9: F811 Redefinition of unused `meta_url` from line 79
stubs/influxdb-client/influxdb_client/domain/source.pyi:85:9: F811 Redefinition of unused `default_rp` from line 83
stubs/influxdb-client/influxdb_client/domain/source.pyi:89:9: F811 Redefinition of unused `languages` from line 87
stubs/influxdb-client/influxdb_client/domain/source_links.pyi:17:9: F811 Redefinition of unused `query` from line 15
stubs/influxdb-client/influxdb_client/domain/source_links.pyi:21:9: F811 Redefinition of unused `health` from line 19
stubs/influxdb-client/influxdb_client/domain/source_links.pyi:25:9: F811 Redefinition of unused `buckets` from line 23
stubs/influxdb-client/influxdb_client/domain/sources.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/sources.pyi:15:9: F811 Redefinition of unused `sources` from line 13
stubs/influxdb-client/influxdb_client/domain/stack.pyi:17:9: F811 Redefinition of unused `id` from line 15
stubs/influxdb-client/influxdb_client/domain/stack.pyi:21:9: F811 Redefinition of unused `org_id` from line 19
stubs/influxdb-client/influxdb_client/domain/stack.pyi:25:9: F811 Redefinition of unused `created_at` from line 23
stubs/influxdb-client/influxdb_client/domain/stack.pyi:29:9: F811 Redefinition of unused `events` from line 27
stubs/influxdb-client/influxdb_client/domain/stack_associations.pyi:11:9: F811 Redefinition of unused `kind` from line 9
stubs/influxdb-client/influxdb_client/domain/stack_associations.pyi:15:9: F811 Redefinition of unused `meta_name` from line 13
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:20:9: F811 Redefinition of unused `event_type` from line 18
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:24:9: F811 Redefinition of unused `name` from line 22
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:28:9: F811 Redefinition of unused `description` from line 26
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:32:9: F811 Redefinition of unused `sources` from line 30
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:36:9: F811 Redefinition of unused `resources` from line 34
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:40:9: F811 Redefinition of unused `urls` from line 38
stubs/influxdb-client/influxdb_client/domain/stack_events.pyi:44:9: F811 Redefinition of unused `updated_at` from line 42
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:19:9: F811 Redefinition of unused `api_version` from line 17
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:23:9: F811 Redefinition of unused `resource_id` from line 21
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:27:9: F811 Redefinition of unused `kind` from line 25
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:35:9: F811 Redefinition of unused `associations` from line 33
stubs/influxdb-client/influxdb_client/domain/stack_resources.pyi:39:9: F811 Redefinition of unused `links` from line 37
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:20:9: F811 Redefinition of unused `colorize_rows` from line 18
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:24:9: F811 Redefinition of unused `height_ratio` from line 22
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:28:9: F811 Redefinition of unused `show` from line 26
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:32:9: F811 Redefinition of unused `opacity` from line 30
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:36:9: F811 Redefinition of unused `orientation_threshold` from line 34
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:40:9: F811 Redefinition of unused `value_axis` from line 38
stubs/influxdb-client/influxdb_client/domain/static_legend.pyi:44:9: F811 Redefinition of unused `width_ratio` from line 42
stubs/influxdb-client/influxdb_client/domain/status_rule.pyi:17:9: F811 Redefinition of unused `current_level` from line 15
stubs/influxdb-client/influxdb_client/domain/status_rule.pyi:21:9: F811 Redefinition of unused `previous_level` from line 19
stubs/influxdb-client/influxdb_client/domain/status_rule.pyi:25:9: F811 Redefinition of unused `count` from line 23
stubs/influxdb-client/influxdb_client/domain/status_rule.pyi:29:9: F811 Redefinition of unused `period` from line 27
stubs/influxdb-client/influxdb_client/domain/string_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/string_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/subscription_manifest.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/subscription_manifest.pyi:17:9: F811 Redefinition of unused `mode` from line 15
stubs/influxdb-client/influxdb_client/domain/subscription_manifest.pyi:21:9: F811 Redefinition of unused `destinations` from line 19
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:25:9: F811 Redefinition of unused `type` from line 23
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:29:9: F811 Redefinition of unused `queries` from line 27
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:33:9: F811 Redefinition of unused `colors` from line 31
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:37:9: F811 Redefinition of unused `shape` from line 35
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:41:9: F811 Redefinition of unused `note` from line 39
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:45:9: F811 Redefinition of unused `show_note_when_empty` from line 43
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:49:9: F811 Redefinition of unused `table_options` from line 47
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:53:9: F811 Redefinition of unused `field_options` from line 51
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:57:9: F811 Redefinition of unused `time_format` from line 55
stubs/influxdb-client/influxdb_client/domain/table_view_properties.pyi:61:9: F811 Redefinition of unused `decimal_places` from line 59
stubs/influxdb-client/influxdb_client/domain/table_view_properties_table_options.pyi:17:9: F811 Redefinition of unused `vertical_time_axis` from line 15
stubs/influxdb-client/influxdb_client/domain/table_view_properties_table_options.pyi:21:9: F811 Redefinition of unused `sort_by` from line 19
stubs/influxdb-client/influxdb_client/domain/table_view_properties_table_options.pyi:25:9: F811 Redefinition of unused `wrapping` from line 23
stubs/influxdb-client/influxdb_client/domain/table_view_properties_table_options.pyi:29:9: F811 Redefinition of unused `fix_first_column` from line 27
stubs/influxdb-client/influxdb_client/domain/tag_rule.pyi:13:9: F811 Redefinition of unused `key` from line 11
stubs/influxdb-client/influxdb_client/domain/tag_rule.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/tag_rule.pyi:21:9: F811 Redefinition of unused `operator` from line 19
stubs/influxdb-client/influxdb_client/domain/task.pyi:32:9: F811 Redefinition of unused `id` from line 30
stubs/influxdb-client/influxdb_client/domain/task.pyi:36:9: F811 Redefinition of unused `org_id` from line 34
stubs/influxdb-client/influxdb_client/domain/task.pyi:40:9: F811 Redefinition of unused `org` from line 38
stubs/influxdb-client/influxdb_client/domain/task.pyi:44:9: F811 Redefinition of unused `name` from line 42
stubs/influxdb-client/influxdb_client/domain/task.pyi:48:9: F811 Redefinition of unused `owner_id` from line 46
stubs/influxdb-client/influxdb_client/domain/task.pyi:52:9: F811 Redefinition of unused `description` from line 50
stubs/influxdb-client/influxdb_client/domain/task.pyi:56:9: F811 Redefinition of unused `status` from line 54
stubs/influxdb-client/influxdb_client/domain/task.pyi:60:9: F811 Redefinition of unused `labels` from line 58
stubs/influxdb-client/influxdb_client/domain/task.pyi:64:9: F811 Redefinition of unused `authorization_id` from line 62
stubs/influxdb-client/influxdb_client/domain/task.pyi:68:9: F811 Redefinition of unused `flux` from line 66
stubs/influxdb-client/influxdb_client/domain/task.pyi:72:9: F811 Redefinition of unused `every` from line 70
stubs/influxdb-client/influxdb_client/domain/task.pyi:76:9: F811 Redefinition of unused `cron` from line 74
stubs/influxdb-client/influxdb_client/domain/task.pyi:80:9: F811 Redefinition of unused `offset` from line 78
stubs/influxdb-client/influxdb_client/domain/task.pyi:84:9: F811 Redefinition of unused `latest_completed` from line 82
stubs/influxdb-client/influxdb_client/domain/task.pyi:88:9: F811 Redefinition of unused `last_run_status` from line 86
stubs/influxdb-client/influxdb_client/domain/task.pyi:92:9: F811 Redefinition of unused `last_run_error` from line 90
stubs/influxdb-client/influxdb_client/domain/task.pyi:96:9: F811 Redefinition of unused `created_at` from line 94
stubs/influxdb-client/influxdb_client/domain/task.pyi:100:9: F811 Redefinition of unused `updated_at` from line 98
stubs/influxdb-client/influxdb_client/domain/task.pyi:104:9: F811 Redefinition of unused `links` from line 102
stubs/influxdb-client/influxdb_client/domain/task_create_request.pyi:18:9: F811 Redefinition of unused `org_id` from line 16
stubs/influxdb-client/influxdb_client/domain/task_create_request.pyi:22:9: F811 Redefinition of unused `org` from line 20
stubs/influxdb-client/influxdb_client/domain/task_create_request.pyi:26:9: F811 Redefinition of unused `status` from line 24
stubs/influxdb-client/influxdb_client/domain/task_create_request.pyi:30:9: F811 Redefinition of unused `flux` from line 28
stubs/influxdb-client/influxdb_client/domain/task_create_request.pyi:34:9: F811 Redefinition of unused `description` from line 32
stubs/influxdb-client/influxdb_client/domain/task_links.pyi:19:9: F811 Redefinition of unused `owners` from line 17
stubs/influxdb-client/influxdb_client/domain/task_links.pyi:23:9: F811 Redefinition of unused `members` from line 21
stubs/influxdb-client/influxdb_client/domain/task_links.pyi:27:9: F811 Redefinition of unused `runs` from line 25
stubs/influxdb-client/influxdb_client/domain/task_links.pyi:31:9: F811 Redefinition of unused `logs` from line 29
stubs/influxdb-client/influxdb_client/domain/task_links.pyi:35:9: F811 Redefinition of unused `labels` from line 33
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:20:9: F811 Redefinition of unused `status` from line 18
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:24:9: F811 Redefinition of unused `flux` from line 22
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:28:9: F811 Redefinition of unused `name` from line 26
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:32:9: F811 Redefinition of unused `every` from line 30
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:36:9: F811 Redefinition of unused `cron` from line 34
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:40:9: F811 Redefinition of unused `offset` from line 38
stubs/influxdb-client/influxdb_client/domain/task_update_request.pyi:44:9: F811 Redefinition of unused `description` from line 42
stubs/influxdb-client/influxdb_client/domain/telegraf.pyi:23:9: F811 Redefinition of unused `id` from line 21
stubs/influxdb-client/influxdb_client/domain/telegraf.pyi:27:9: F811 Redefinition of unused `links` from line 25
stubs/influxdb-client/influxdb_client/domain/telegraf.pyi:31:9: F811 Redefinition of unused `labels` from line 29
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin.pyi:17:9: F811 Redefinition of unused `type` from line 15
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin.pyi:21:9: F811 Redefinition of unused `name` from line 19
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin.pyi:25:9: F811 Redefinition of unused `description` from line 23
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin.pyi:29:9: F811 Redefinition of unused `config` from line 27
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:23:9: F811 Redefinition of unused `description` from line 21
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:27:9: F811 Redefinition of unused `plugins` from line 25
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:31:9: F811 Redefinition of unused `metadata` from line 29
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:35:9: F811 Redefinition of unused `config` from line 33
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request.pyi:39:9: F811 Redefinition of unused `org_id` from line 37
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request_plugins.pyi:18:9: F811 Redefinition of unused `type` from line 16
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request_plugins.pyi:22:9: F811 Redefinition of unused `name` from line 20
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request_plugins.pyi:26:9: F811 Redefinition of unused `alias` from line 24
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request_plugins.pyi:30:9: F811 Redefinition of unused `description` from line 28
stubs/influxdb-client/influxdb_client/domain/telegraf_plugin_request_plugins.pyi:34:9: F811 Redefinition of unused `config` from line 32
stubs/influxdb-client/influxdb_client/domain/telegraf_plugins.pyi:13:9: F811 Redefinition of unused `version` from line 11
stubs/influxdb-client/influxdb_client/domain/telegraf_plugins.pyi:17:9: F811 Redefinition of unused `os` from line 15
stubs/influxdb-client/influxdb_client/domain/telegraf_plugins.pyi:21:9: F811 Redefinition of unused `plugins` from line 19
stubs/influxdb-client/influxdb_client/domain/telegraf_request.pyi:18:9: F811 Redefinition of unused `name` from line 16
stubs/influxdb-client/influxdb_client/domain/telegraf_request.pyi:22:9: F811 Redefinition of unused `description` from line 20
stubs/influxdb-client/influxdb_client/domain/telegraf_request.pyi:26:9: F811 Redefinition of unused `metadata` from line 24
stubs/influxdb-client/influxdb_client/domain/telegraf_request.pyi:30:9: F811 Redefinition of unused `config` from line 28
stubs/influxdb-client/influxdb_client/domain/telegraf_request.pyi:34:9: F811 Redefinition of unused `org_id` from line 32
stubs/influxdb-client/influxdb_client/domain/telegraf_request_metadata.pyi:11:9: F811 Redefinition of unused `buckets` from line 9
stubs/influxdb-client/influxdb_client/domain/telegrafs.pyi:11:9: F811 Redefinition of unused `configurations` from line 9
stubs/influxdb-client/influxdb_client/domain/telegram_notification_endpoint.pyi:28:9: F811 Redefinition of unused `token` from line 26
stubs/influxdb-client/influxdb_client/domain/telegram_notification_endpoint.pyi:32:9: F811 Redefinition of unused `channel` from line 30
stubs/influxdb-client/influxdb_client/domain/telegram_notification_rule_base.pyi:42:9: F811 Redefinition of unused `type` from line 40
stubs/influxdb-client/influxdb_client/domain/telegram_notification_rule_base.pyi:46:9: F811 Redefinition of unused `message_template` from line 44
stubs/influxdb-client/influxdb_client/domain/telegram_notification_rule_base.pyi:50:9: F811 Redefinition of unused `parse_mode` from line 48
stubs/influxdb-client/influxdb_client/domain/telegram_notification_rule_base.pyi:54:9: F811 Redefinition of unused `disable_web_page_preview` from line 52
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:22:9: F811 Redefinition of unused `dry_run` from line 20
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:26:9: F811 Redefinition of unused `org_id` from line 24
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:30:9: F811 Redefinition of unused `stack_id` from line 28
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:34:9: F811 Redefinition of unused `template` from line 32
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:38:9: F811 Redefinition of unused `templates` from line 36
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:42:9: F811 Redefinition of unused `env_refs` from line 40
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:46:9: F811 Redefinition of unused `secrets` from line 44
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:50:9: F811 Redefinition of unused `remotes` from line 48
stubs/influxdb-client/influxdb_client/domain/template_apply.pyi:54:9: F811 Redefinition of unused `actions` from line 52
stubs/influxdb-client/influxdb_client/domain/template_apply_remotes.pyi:11:9: F811 Redefinition of unused `url` from line 9
stubs/influxdb-client/influxdb_client/domain/template_apply_remotes.pyi:15:9: F811 Redefinition of unused `content_type` from line 13
stubs/influxdb-client/influxdb_client/domain/template_apply_template.pyi:13:9: F811 Redefinition of unused `content_type` from line 11
stubs/influxdb-client/influxdb_client/domain/template_apply_template.pyi:17:9: F811 Redefinition of unused `sources` from line 15
stubs/influxdb-client/influxdb_client/domain/template_apply_template.pyi:21:9: F811 Redefinition of unused `contents` from line 19
stubs/influxdb-client/influxdb_client/domain/template_chart.pyi:18:9: F811 Redefinition of unused `x_pos` from line 16
stubs/influxdb-client/influxdb_client/domain/template_chart.pyi:22:9: F811 Redefinition of unused `y_pos` from line 20
stubs/influxdb-client/influxdb_client/domain/template_chart.pyi:26:9: F811 Redefinition of unused `height` from line 24
stubs/influxdb-client/influxdb_client/domain/template_chart.pyi:30:9: F811 Redefinition of unused `width` from line 28
stubs/influxdb-client/influxdb_client/domain/template_chart.pyi:34:9: F811 Redefinition of unused `properties` from line 32
stubs/influxdb-client/influxdb_client/domain/template_export_by_id.pyi:13:9: F811 Redefinition of unused `stack_id` from line 11
stubs/influxdb-client/influxdb_client/domain/template_export_by_id.pyi:17:9: F811 Redefinition of unused `org_ids` from line 15
stubs/influxdb-client/influxdb_client/domain/template_export_by_id.pyi:21:9: F811 Redefinition of unused `resources` from line 19
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_org_ids.pyi:11:9: F811 Redefinition of unused `org_id` from line 9
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_org_ids.pyi:15:9: F811 Redefinition of unused `resource_filters` from line 13
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_resource_filters.pyi:11:9: F811 Redefinition of unused `by_label` from line 9
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_resource_filters.pyi:15:9: F811 Redefinition of unused `by_resource_kind` from line 13
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_resources.pyi:11:9: F811 Redefinition of unused `id` from line 9
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_resources.pyi:15:9: F811 Redefinition of unused `kind` from line 13
stubs/influxdb-client/influxdb_client/domain/template_export_by_id_resources.pyi:19:9: F811 Redefinition of unused `name` from line 17
stubs/influxdb-client/influxdb_client/domain/template_export_by_name.pyi:13:9: F811 Redefinition of unused `stack_id` from line 11
stubs/influxdb-client/influxdb_client/domain/template_export_by_name.pyi:17:9: F811 Redefinition of unused `org_ids` from line 15
stubs/influxdb-client/influxdb_client/domain/template_export_by_name.pyi:21:9: F811 Redefinition of unused `resources` from line 19
stubs/influxdb-client/influxdb_client/domain/template_export_by_name_resources.pyi:11:9: F811 Redefinition of unused `kind` from line 9
stubs/influxdb-client/influxdb_client/domain/template_export_by_name_resources.pyi:15:9: F811 Redefinition of unused `name` from line 13
stubs/influxdb-client/influxdb_client/domain/template_summary.pyi:18:9: F811 Redefinition of unused `sources` from line 16
stubs/influxdb-client/influxdb_client/domain/template_summary.pyi:22:9: F811 Redefinition of unused `stack_id` from line 20
stubs/influxdb-client/influxdb_client/domain/template_summary.pyi:26:9: F811 Redefinition of unused `summary` from line 24
stubs/influxdb-client/influxdb_client/domain/template_summary.pyi:30:9: F811 Redefinition of unused `diff` from line 28
stubs/influxdb-client/influxdb_client/domain/template_summary.pyi:34:9: F811 Redefinition of unused `errors` from line 32
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:23:9: F811 Redefinition of unused `buckets` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:27:9: F811 Redefinition of unused `checks` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:31:9: F811 Redefinition of unused `dashboards` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:35:9: F811 Redefinition of unused `labels` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:39:9: F811 Redefinition of unused `label_mappings` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:43:9: F811 Redefinition of unused `notification_endpoints` from line 41
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:47:9: F811 Redefinition of unused `notification_rules` from line 45
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:51:9: F811 Redefinition of unused `tasks` from line 49
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:55:9: F811 Redefinition of unused `telegraf_configs` from line 53
stubs/influxdb-client/influxdb_client/domain/template_summary_diff.pyi:59:9: F811 Redefinition of unused `variables` from line 57
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets_new_old.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets_new_old.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_buckets_new_old.pyi:21:9: F811 Redefinition of unused `retention_rules` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_checks.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:19:9: F811 Redefinition of unused `state_status` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:23:9: F811 Redefinition of unused `id` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:27:9: F811 Redefinition of unused `kind` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards_new_old.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards_new_old.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_dashboards_new_old.pyi:21:9: F811 Redefinition of unused `charts` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:21:9: F811 Redefinition of unused `status` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:25:9: F811 Redefinition of unused `resource_type` from line 23
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:29:9: F811 Redefinition of unused `resource_id` from line 27
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:33:9: F811 Redefinition of unused `resource_template_meta_name` from line 31
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:37:9: F811 Redefinition of unused `resource_name` from line 35
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:41:9: F811 Redefinition of unused `label_id` from line 39
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:45:9: F811 Redefinition of unused `label_template_meta_name` from line 43
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_label_mappings.pyi:49:9: F811 Redefinition of unused `label_name` from line 47
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:19:9: F811 Redefinition of unused `state_status` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:23:9: F811 Redefinition of unused `kind` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels_new_old.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels_new_old.pyi:17:9: F811 Redefinition of unused `color` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_labels_new_old.pyi:21:9: F811 Redefinition of unused `description` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_endpoints.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:24:9: F811 Redefinition of unused `name` from line 22
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:28:9: F811 Redefinition of unused `description` from line 26
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:32:9: F811 Redefinition of unused `endpoint_name` from line 30
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:36:9: F811 Redefinition of unused `endpoint_id` from line 34
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:40:9: F811 Redefinition of unused `endpoint_type` from line 38
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:44:9: F811 Redefinition of unused `every` from line 42
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:48:9: F811 Redefinition of unused `offset` from line 46
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:52:9: F811 Redefinition of unused `message_template` from line 50
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:56:9: F811 Redefinition of unused `status` from line 54
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:60:9: F811 Redefinition of unused `status_rules` from line 58
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_notification_rules_new_old.pyi:64:9: F811 Redefinition of unused `tag_rules` from line 62
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:20:9: F811 Redefinition of unused `name` from line 18
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:24:9: F811 Redefinition of unused `cron` from line 22
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:28:9: F811 Redefinition of unused `description` from line 26
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:32:9: F811 Redefinition of unused `every` from line 30
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:36:9: F811 Redefinition of unused `offset` from line 34
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:40:9: F811 Redefinition of unused `query` from line 38
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_tasks_new_old.pyi:44:9: F811 Redefinition of unused `status` from line 42
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_telegraf_configs.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:19:9: F811 Redefinition of unused `kind` from line 17
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:23:9: F811 Redefinition of unused `state_status` from line 21
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:31:9: F811 Redefinition of unused `template_meta_name` from line 29
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:35:9: F811 Redefinition of unused `new` from line 33
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables.pyi:39:9: F811 Redefinition of unused `old` from line 37
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables_new_old.pyi:13:9: F811 Redefinition of unused `name` from line 11
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables_new_old.pyi:17:9: F811 Redefinition of unused `description` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_diff_variables_new_old.pyi:21:9: F811 Redefinition of unused `args` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_errors.pyi:17:9: F811 Redefinition of unused `kind` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_errors.pyi:21:9: F811 Redefinition of unused `reason` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_errors.pyi:25:9: F811 Redefinition of unused `fields` from line 23
stubs/influxdb-client/influxdb_client/domain/template_summary_errors.pyi:29:9: F811 Redefinition of unused `indexes` from line 27
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:20:9: F811 Redefinition of unused `id` from line 18
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:24:9: F811 Redefinition of unused `org_id` from line 22
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:28:9: F811 Redefinition of unused `kind` from line 26
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:32:9: F811 Redefinition of unused `template_meta_name` from line 30
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:36:9: F811 Redefinition of unused `name` from line 34
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:40:9: F811 Redefinition of unused `properties` from line 38
stubs/influxdb-client/influxdb_client/domain/template_summary_label.pyi:44:9: F811 Redefinition of unused `env_references` from line 42
stubs/influxdb-client/influxdb_client/domain/template_summary_label_properties.pyi:11:9: F811 Redefinition of unused `color` from line 9
stubs/influxdb-client/influxdb_client/domain/template_summary_label_properties.pyi:15:9: F811 Redefinition of unused `description` from line 13
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:25:9: F811 Redefinition of unused `buckets` from line 23
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:29:9: F811 Redefinition of unused `checks` from line 27
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:33:9: F811 Redefinition of unused `dashboards` from line 31
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:37:9: F811 Redefinition of unused `labels` from line 35
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:41:9: F811 Redefinition of unused `label_mappings` from line 39
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:45:9: F811 Redefinition of unused `missing_env_refs` from line 43
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:49:9: F811 Redefinition of unused `missing_secrets` from line 47
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:53:9: F811 Redefinition of unused `notification_endpoints` from line 51
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:57:9: F811 Redefinition of unused `notification_rules` from line 55
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:61:9: F811 Redefinition of unused `tasks` from line 59
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:65:9: F811 Redefinition of unused `telegraf_configs` from line 63
stubs/influxdb-client/influxdb_client/domain/template_summary_summary.pyi:69:9: F811 Redefinition of unused `variables` from line 67
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:22:9: F811 Redefinition of unused `id` from line 20
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:26:9: F811 Redefinition of unused `org_id` from line 24
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:30:9: F811 Redefinition of unused `kind` from line 28
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:34:9: F811 Redefinition of unused `template_meta_name` from line 32
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:38:9: F811 Redefinition of unused `name` from line 36
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:42:9: F811 Redefinition of unused `description` from line 40
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:46:9: F811 Redefinition of unused `retention_period` from line 44
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:50:9: F811 Redefinition of unused `label_associations` from line 48
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_buckets.pyi:54:9: F811 Redefinition of unused `env_references` from line 52
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:22:9: F811 Redefinition of unused `id` from line 20
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:26:9: F811 Redefinition of unused `org_id` from line 24
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:30:9: F811 Redefinition of unused `kind` from line 28
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:34:9: F811 Redefinition of unused `template_meta_name` from line 32
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:38:9: F811 Redefinition of unused `name` from line 36
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:42:9: F811 Redefinition of unused `description` from line 40
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:46:9: F811 Redefinition of unused `label_associations` from line 44
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:50:9: F811 Redefinition of unused `charts` from line 48
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_dashboards.pyi:54:9: F811 Redefinition of unused `env_references` from line 52
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:21:9: F811 Redefinition of unused `status` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:25:9: F811 Redefinition of unused `resource_template_meta_name` from line 23
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:29:9: F811 Redefinition of unused `resource_name` from line 27
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:33:9: F811 Redefinition of unused `resource_id` from line 31
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:37:9: F811 Redefinition of unused `resource_type` from line 35
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:41:9: F811 Redefinition of unused `label_template_meta_name` from line 39
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:45:9: F811 Redefinition of unused `label_name` from line 43
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_label_mappings.pyi:49:9: F811 Redefinition of unused `label_id` from line 47
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:28:9: F811 Redefinition of unused `kind` from line 26
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:32:9: F811 Redefinition of unused `template_meta_name` from line 30
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:36:9: F811 Redefinition of unused `name` from line 34
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:40:9: F811 Redefinition of unused `description` from line 38
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:44:9: F811 Redefinition of unused `endpoint_template_meta_name` from line 42
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:48:9: F811 Redefinition of unused `endpoint_id` from line 46
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:52:9: F811 Redefinition of unused `endpoint_type` from line 50
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:56:9: F811 Redefinition of unused `every` from line 54
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:60:9: F811 Redefinition of unused `offset` from line 58
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:64:9: F811 Redefinition of unused `message_template` from line 62
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:68:9: F811 Redefinition of unused `status` from line 66
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:72:9: F811 Redefinition of unused `status_rules` from line 70
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:76:9: F811 Redefinition of unused `tag_rules` from line 74
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:80:9: F811 Redefinition of unused `label_associations` from line 78
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_notification_rules.pyi:84:9: F811 Redefinition of unused `env_references` from line 82
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_status_rules.pyi:11:9: F811 Redefinition of unused `current_level` from line 9
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_status_rules.pyi:15:9: F811 Redefinition of unused `previous_level` from line 13
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tag_rules.pyi:13:9: F811 Redefinition of unused `key` from line 11
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tag_rules.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tag_rules.pyi:21:9: F811 Redefinition of unused `operator` from line 19
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:24:9: F811 Redefinition of unused `kind` from line 22
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:28:9: F811 Redefinition of unused `template_meta_name` from line 26
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:32:9: F811 Redefinition of unused `id` from line 30
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:36:9: F811 Redefinition of unused `name` from line 34
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:40:9: F811 Redefinition of unused `cron` from line 38
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:44:9: F811 Redefinition of unused `description` from line 42
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:48:9: F811 Redefinition of unused `every` from line 46
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:52:9: F811 Redefinition of unused `offset` from line 50
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:56:9: F811 Redefinition of unused `query` from line 54
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:60:9: F811 Redefinition of unused `status` from line 58
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_tasks.pyi:64:9: F811 Redefinition of unused `env_references` from line 62
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:22:9: F811 Redefinition of unused `kind` from line 20
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:26:9: F811 Redefinition of unused `template_meta_name` from line 24
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:30:9: F811 Redefinition of unused `id` from line 28
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:34:9: F811 Redefinition of unused `org_id` from line 32
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:38:9: F811 Redefinition of unused `name` from line 36
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:42:9: F811 Redefinition of unused `description` from line 40
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:46:9: F811 Redefinition of unused `arguments` from line 44
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:50:9: F811 Redefinition of unused `label_associations` from line 48
stubs/influxdb-client/influxdb_client/domain/template_summary_summary_variables.pyi:54:9: F811 Redefinition of unused `env_references` from line 52
stubs/influxdb-client/influxdb_client/domain/test_statement.pyi:10:9: F811 Redefinition of unused `type` from line 8
stubs/influxdb-client/influxdb_client/domain/test_statement.pyi:14:9: F811 Redefinition of unused `assignment` from line 12
stubs/influxdb-client/influxdb_client/domain/threshold.pyi:12:9: F811 Redefinition of unused `type` from line 10
stubs/influxdb-client/influxdb_client/domain/threshold_base.pyi:11:9: F811 Redefinition of unused `level` from line 9
stubs/influxdb-client/influxdb_client/domain/threshold_base.pyi:15:9: F811 Redefinition of unused `all_values` from line 13
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:36:9: F811 Redefinition of unused `type` from line 34
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:40:9: F811 Redefinition of unused `thresholds` from line 38
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:44:9: F811 Redefinition of unused `every` from line 42
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:48:9: F811 Redefinition of unused `offset` from line 46
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:52:9: F811 Redefinition of unused `tags` from line 50
stubs/influxdb-client/influxdb_client/domain/threshold_check.pyi:56:9: F811 Redefinition of unused `status_message_template` from line 54
stubs/influxdb-client/influxdb_client/domain/unary_expression.pyi:15:9: F811 Redefinition of unused `type` from line 13
stubs/influxdb-client/influxdb_client/domain/unary_expression.pyi:19:9: F811 Redefinition of unused `operator` from line 17
stubs/influxdb-client/influxdb_client/domain/unary_expression.pyi:23:9: F811 Redefinition of unused `argument` from line 21
stubs/influxdb-client/influxdb_client/domain/unsigned_integer_literal.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/unsigned_integer_literal.pyi:17:9: F811 Redefinition of unused `value` from line 15
stubs/influxdb-client/influxdb_client/domain/user.pyi:11:9: F811 Redefinition of unused `id` from line 9
stubs/influxdb-client/influxdb_client/domain/user.pyi:15:9: F811 Redefinition of unused `name` from line 13
stubs/influxdb-client/influxdb_client/domain/user.pyi:19:9: F811 Redefinition of unused `status` from line 17
stubs/influxdb-client/influxdb_client/domain/user_response.pyi:17:9: F811 Redefinition of unused `id` from line 15
stubs/influxdb-client/influxdb_client/domain/user_response.pyi:21:9: F811 Redefinition of unused `name` from line 19
stubs/influxdb-client/influxdb_client/domain/user_response.pyi:25:9: F811 Redefinition of unused `status` from line 23
stubs/influxdb-client/influxdb_client/domain/user_response.pyi:29:9: F811 Redefinition of unused `links` from line 27
stubs/influxdb-client/influxdb_client/domain/users.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/users.pyi:15:9: F811 Redefinition of unused `users` from line 13
stubs/influxdb-client/influxdb_client/domain/variable.pyi:23:9: F811 Redefinition of unused `links` from line 21
stubs/influxdb-client/influxdb_client/domain/variable.pyi:27:9: F811 Redefinition of unused `id` from line 25
stubs/influxdb-client/influxdb_client/domain/variable.pyi:31:9: F811 Redefinition of unused `org_id` from line 29
stubs/influxdb-client/influxdb_client/domain/variable.pyi:35:9: F811 Redefinition of unused `name` from line 33
stubs/influxdb-client/influxdb_client/domain/variable.pyi:39:9: F811 Redefinition of unused `description` from line 37
stubs/influxdb-client/influxdb_client/domain/variable.pyi:43:9: F811 Redefinition of unused `selected` from line 41
stubs/influxdb-client/influxdb_client/domain/variable.pyi:47:9: F811 Redefinition of unused `labels` from line 45
stubs/influxdb-client/influxdb_client/domain/variable.pyi:51:9: F811 Redefinition of unused `arguments` from line 49
stubs/influxdb-client/influxdb_client/domain/variable.pyi:55:9: F811 Redefinition of unused `created_at` from line 53
stubs/influxdb-client/influxdb_client/domain/variable.pyi:59:9: F811 Redefinition of unused `updated_at` from line 57
stubs/influxdb-client/influxdb_client/domain/variable_assignment.pyi:13:9: F811 Redefinition of unused `type` from line 11
stubs/influxdb-client/influxdb_client/domain/variable_assignment.pyi:17:9: F811 Redefinition of unused `id` from line 15
stubs/influxdb-client/influxdb_client/domain/variable_assignment.pyi:21:9: F811 Redefinition of unused `init` from line 19
stubs/influxdb-client/influxdb_client/domain/variable_links.pyi:13:9: F811 Redefinition of unused `org` from line 11
stubs/influxdb-client/influxdb_client/domain/variable_links.pyi:17:9: F811 Redefinition of unused `labels` from line 15
stubs/influxdb-client/influxdb_client/domain/variables.pyi:11:9: F811 Redefinition of unused `variables` from line 9
stubs/influxdb-client/influxdb_client/domain/view.pyi:17:9: F811 Redefinition of unused `links` from line 15
stubs/influxdb-client/influxdb_client/domain/view.pyi:21:9: F811 Redefinition of unused `id` from line 19
stubs/influxdb-client/influxdb_client/domain/view.pyi:25:9: F811 Redefinition of unused `name` from line 23
stubs/influxdb-client/influxdb_client/domain/view.pyi:29:9: F811 Redefinition of unused `properties` from line 27
stubs/influxdb-client/influxdb_client/domain/views.pyi:11:9: F811 Redefinition of unused `links` from line 9
stubs/influxdb-client/influxdb_client/domain/views.pyi:15:9: F811 Redefinition of unused `views` from line 13
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:45:9: F811 Redefinition of unused `time_format` from line 43
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:49:9: F811 Redefinition of unused `type` from line 47
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:53:9: F811 Redefinition of unused `queries` from line 51
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:57:9: F811 Redefinition of unused `colors` from line 55
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:61:9: F811 Redefinition of unused `color_mapping` from line 59
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:65:9: F811 Redefinition of unused `shape` from line 63
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:69:9: F811 Redefinition of unused `note` from line 67
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:73:9: F811 Redefinition of unused `show_note_when_empty` from line 71
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:77:9: F811 Redefinition of unused `axes` from line 75
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:81:9: F811 Redefinition of unused `static_legend` from line 79
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:85:9: F811 Redefinition of unused `x_column` from line 83
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:89:9: F811 Redefinition of unused `generate_x_axis_ticks` from line 87
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:93:9: F811 Redefinition of unused `x_total_ticks` from line 91
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:97:9: F811 Redefinition of unused `x_tick_start` from line 95
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:101:9: F811 Redefinition of unused `x_tick_step` from line 99
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:105:9: F811 Redefinition of unused `y_column` from line 103
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:109:9: F811 Redefinition of unused `generate_y_axis_ticks` from line 107
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:113:9: F811 Redefinition of unused `y_total_ticks` from line 111
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:117:9: F811 Redefinition of unused `y_tick_start` from line 115
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:121:9: F811 Redefinition of unused `y_tick_step` from line 119
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:125:9: F811 Redefinition of unused `shade_below` from line 123
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:129:9: F811 Redefinition of unused `hover_dimension` from line 127
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:133:9: F811 Redefinition of unused `position` from line 131
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:137:9: F811 Redefinition of unused `geom` from line 135
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:141:9: F811 Redefinition of unused `legend_colorize_rows` from line 139
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:145:9: F811 Redefinition of unused `legend_hide` from line 143
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:149:9: F811 Redefinition of unused `legend_opacity` from line 147
stubs/influxdb-client/influxdb_client/domain/xy_view_properties.pyi:153:9: F811 Redefinition of unused `legend_orientation_threshold` from line 151
stubs/ldap3/ldap3/abstract/cursor.pyi:66:9: F811 Redefinition of unused `query` from line 64
stubs/ldap3/ldap3/abstract/cursor.pyi:70:9: F811 Redefinition of unused `components_in_and` from line 68
stubs/ldap3/ldap3/core/connection.pyi:116:9: F811 Redefinition of unused `stream` from line 114
stubs/netaddr/netaddr/eui/__init__.pyi:41:9: F811 Redefinition of unused `value` from line 39
stubs/netaddr/netaddr/eui/__init__.pyi:45:9: F811 Redefinition of unused `dialect` from line 43
stubs/netaddr/netaddr/ip/__init__.pyi:15:9: F811 Redefinition of unused `value` from line 13
stubs/netaddr/netaddr/ip/__init__.pyi:102:9: F811 Redefinition of unused `prefixlen` from line 100
stubs/netaddr/netaddr/ip/__init__.pyi:116:9: F811 Redefinition of unused `netmask` from line 114
stubs/netaddr/netaddr/ip/glob.pyi:17:9: F811 Redefinition of unused `glob` from line 15
stubs/networkx/networkx/utils/backends.pyi:45:9: F811 Redefinition of unused `__doc__` from line 43
stubs/oauthlib/oauthlib/oauth2/rfc6749/endpoints/base.pyi:10:9: F811 Redefinition of unused `valid_request_methods` from line 8
stubs/oauthlib/oauthlib/oauth2/rfc6749/endpoints/base.pyi:14:9: F811 Redefinition of unused `available` from line 12
stubs/oauthlib/oauthlib/oauth2/rfc6749/endpoints/base.pyi:18:9: F811 Redefinition of unused `catch_errors` from line 16
stubs/openpyxl/openpyxl/cell/cell.pyi:69:9: F811 Redefinition of unused `value` from line 67
stubs/openpyxl/openpyxl/cell/cell.pyi:75:9: F811 Redefinition of unused `hyperlink` from line 73
stubs/openpyxl/openpyxl/cell/cell.pyi:82:9: F811 Redefinition of unused `comment` from line 80
stubs/openpyxl/openpyxl/cell/read_only.pyi:58:9: F811 Redefinition of unused `value` from line 56
stubs/openpyxl/openpyxl/comments/comments.pyi:19:9: F811 Redefinition of unused `text` from line 17
stubs/openpyxl/openpyxl/drawing/drawing.pyi:21:9: F811 Redefinition of unused `width` from line 19
stubs/openpyxl/openpyxl/drawing/drawing.pyi:25:9: F811 Redefinition of unused `height` from line 23
stubs/openpyxl/openpyxl/styles/colors.pyi:51:9: F811 Redefinition of unused `value` from line 49
stubs/openpyxl/openpyxl/workbook/child.pyi:23:9: F811 Redefinition of unused `title` from line 21
stubs/openpyxl/openpyxl/workbook/child.pyi:27:9: F811 Redefinition of unused `oddHeader` from line 25
stubs/openpyxl/openpyxl/workbook/child.pyi:31:9: F811 Redefinition of unused `oddFooter` from line 29
stubs/openpyxl/openpyxl/workbook/child.pyi:35:9: F811 Redefinition of unused `evenHeader` from line 33
stubs/openpyxl/openpyxl/workbook/child.pyi:39:9: F811 Redefinition of unused `evenFooter` from line 37
stubs/openpyxl/openpyxl/workbook/child.pyi:43:9: F811 Redefinition of unused `firstHeader` from line 41
stubs/openpyxl/openpyxl/workbook/child.pyi:47:9: F811 Redefinition of unused `firstFooter` from line 45
stubs/openpyxl/openpyxl/workbook/protection.pyi:58:9: F811 Redefinition of unused `workbookPassword` from line 56
stubs/openpyxl/openpyxl/workbook/protection.pyi:68:9: F811 Redefinition of unused `revisionsPassword` from line 66
stubs/openpyxl/openpyxl/workbook/workbook.pyi:45:9: F811 Redefinition of unused `epoch` from line 43
stubs/openpyxl/openpyxl/workbook/workbook.pyi:57:9: F811 Redefinition of unused `active` from line 55
stubs/openpyxl/openpyxl/worksheet/_write_only.pyi:26:9: F811 Redefinition of unused `print_title_cols` from line 24
stubs/openpyxl/openpyxl/worksheet/_write_only.pyi:30:9: F811 Redefinition of unused `print_title_rows` from line 28
stubs/openpyxl/openpyxl/worksheet/_write_only.pyi:34:9: F811 Redefinition of unused `freeze_panes` from line 32
stubs/openpyxl/openpyxl/worksheet/_write_only.pyi:38:9: F811 Redefinition of unused `print_area` from line 36
stubs/openpyxl/openpyxl/worksheet/page.pyi:64:9: F811 Redefinition of unused `fitToPage` from line 62
stubs/openpyxl/openpyxl/worksheet/page.pyi:68:9: F811 Redefinition of unused `autoPageBreaks` from line 66
stubs/openpyxl/openpyxl/worksheet/protection.pyi:17:9: F811 Redefinition of unused `password` from line 15
stubs/openpyxl/openpyxl/worksheet/worksheet.pyi:88:9: F811 Redefinition of unused `freeze_panes` from line 86
stubs/openpyxl/openpyxl/worksheet/worksheet.pyi:252:9: F811 Redefinition of unused `print_title_rows` from line 250
stubs/openpyxl/openpyxl/worksheet/worksheet.pyi:256:9: F811 Redefinition of unused `print_title_cols` from line 254
stubs/openpyxl/openpyxl/worksheet/worksheet.pyi:262:9: F811 Redefinition of unused `print_area` from line 260
stubs/paho-mqtt/paho/mqtt/client.pyi:145:9: F811 Redefinition of unused `topic` from line 143
stubs/paho-mqtt/paho/mqtt/client.pyi:251:9: F811 Redefinition of unused `on_log` from line 249
stubs/paho-mqtt/paho/mqtt/client.pyi:256:9: F811 Redefinition of unused `on_connect` from line 254
stubs/paho-mqtt/paho/mqtt/client.pyi:261:9: F811 Redefinition of unused `on_connect_fail` from line 259
stubs/paho-mqtt/paho/mqtt/client.pyi:266:9: F811 Redefinition of unused `on_subscribe` from line 264
stubs/paho-mqtt/paho/mqtt/client.pyi:271:9: F811 Redefinition of unused `on_message` from line 269
stubs/paho-mqtt/paho/mqtt/client.pyi:276:9: F811 Redefinition of unused `on_publish` from line 274
stubs/paho-mqtt/paho/mqtt/client.pyi:281:9: F811 Redefinition of unused `on_unsubscribe` from line 279
stubs/paho-mqtt/paho/mqtt/client.pyi:286:9: F811 Redefinition of unused `on_disconnect` from line 284
stubs/paho-mqtt/paho/mqtt/client.pyi:291:9: F811 Redefinition of unused `on_socket_open` from line 289
stubs/paho-mqtt/paho/mqtt/client.pyi:296:9: F811 Redefinition of unused `on_socket_close` from line 294
stubs/paho-mqtt/paho/mqtt/client.pyi:301:9: F811 Redefinition of unused `on_socket_register_write` from line 299
stubs/paho-mqtt/paho/mqtt/client.pyi:306:9: F811 Redefinition of unused `on_socket_unregister_write` from line 304
stubs/paramiko/paramiko/_winapi.pyi:90:13: F811 Redefinition of unused `descriptor` from line 88
stubs/paramiko/paramiko/transport.pyi:177:9: F811 Redefinition of unused `ciphers` from line 175
stubs/paramiko/paramiko/transport.pyi:181:9: F811 Redefinition of unused `digests` from line 179
stubs/paramiko/paramiko/transport.pyi:185:9: F811 Redefinition of unused `key_types` from line 183
stubs/paramiko/paramiko/transport.pyi:189:9: F811 Redefinition of unused `kex` from line 187
stubs/paramiko/paramiko/transport.pyi:193:9: F811 Redefinition of unused `compression` from line 191
stubs/passlib/passlib/apache.pyi:24:9: F811 Redefinition of unused `path` from line 22
stubs/passlib/passlib/totp.pyi:74:9: F811 Redefinition of unused `key` from line 72
stubs/passlib/passlib/totp.pyi:78:9: F811 Redefinition of unused `encrypted_key` from line 76
stubs/peewee/peewee.pyi:311:9: F811 Redefinition of unused `name` from line 309
stubs/peewee/peewee.pyi:603:9: F811 Redefinition of unused `selected_columns` from line 601
stubs/peewee/peewee.pyi:880:9: F811 Redefinition of unused `timeout` from line 878
stubs/peewee/peewee.pyi:1420:9: F811 Redefinition of unused `through_model` from line 1418
stubs/peewee/peewee.pyi:1465:9: F811 Redefinition of unused `database` from line 1463
stubs/peewee/peewee.pyi:1539:9: F811 Redefinition of unused `table` from line 1537
stubs/peewee/peewee.pyi:1543:9: F811 Redefinition of unused `schema` from line 1541
stubs/pexpect/pexpect/pty_spawn.pyi:60:9: F811 Redefinition of unused `flag_eof` from line 58
stubs/pexpect/pexpect/spawnbase.pyi:83:9: F811 Redefinition of unused `buffer` from line 81
stubs/pika/pika/connection.pyi:45:9: F811 Redefinition of unused `blocked_connection_timeout` from line 43
stubs/pika/pika/connection.pyi:49:9: F811 Redefinition of unused `channel_max` from line 47
stubs/pika/pika/connection.pyi:53:9: F811 Redefinition of unused `client_properties` from line 51
stubs/pika/pika/connection.pyi:57:9: F811 Redefinition of unused `connection_attempts` from line 55
stubs/pika/pika/connection.pyi:61:9: F811 Redefinition of unused `credentials` from line 59
stubs/pika/pika/connection.pyi:65:9: F811 Redefinition of unused `frame_max` from line 63
stubs/pika/pika/connection.pyi:69:9: F811 Redefinition of unused `heartbeat` from line 67
stubs/pika/pika/connection.pyi:73:9: F811 Redefinition of unused `host` from line 71
stubs/pika/pika/connection.pyi:77:9: F811 Redefinition of unused `locale` from line 75
stubs/pika/pika/connection.pyi:81:9: F811 Redefinition of unused `port` from line 79
stubs/pika/pika/connection.pyi:85:9: F811 Redefinition of unused `retry_delay` from line 83
stubs/pika/pika/connection.pyi:89:9: F811 Redefinition of unused `socket_timeout` from line 87
stubs/pika/pika/connection.pyi:93:9: F811 Redefinition of unused `stack_timeout` from line 91
stubs/pika/pika/connection.pyi:97:9: F811 Redefinition of unused `ssl_options` from line 95
stubs/pika/pika/connection.pyi:101:9: F811 Redefinition of unused `virtual_host` from line 99
stubs/pika/pika/connection.pyi:105:9: F811 Redefinition of unused `tcp_options` from line 103
stubs/psycopg2/psycopg2/_psycopg.pyi:426:9: F811 Redefinition of unused `isolation_level` from line 424
stubs/psycopg2/psycopg2/_psycopg.pyi:436:9: F811 Redefinition of unused `deferrable` from line 434
stubs/psycopg2/psycopg2/_psycopg.pyi:440:9: F811 Redefinition of unused `readonly` from line 438
stubs/pyflakes/pyflakes/checker.pyi:198:9: F811 Redefinition of unused `futuresAllowed` from line 196
stubs/pyflakes/pyflakes/checker.pyi:202:9: F811 Redefinition of unused `annotationsFutureEnabled` from line 200
stubs/pygit2/pygit2/settings.pyi:14:9: F811 Redefinition of unused `mwindow_size` from line 12
stubs/pygit2/pygit2/settings.pyi:18:9: F811 Redefinition of unused `mwindow_mapped_limit` from line 16
stubs/pygit2/pygit2/settings.pyi:28:9: F811 Redefinition of unused `ssl_cert_file` from line 26
stubs/pygit2/pygit2/settings.pyi:30:9: F811 Redefinition of unused `ssl_cert_file` from line 28
stubs/pygit2/pygit2/settings.pyi:34:9: F811 Redefinition of unused `ssl_cert_dir` from line 32
stubs/pygit2/pygit2/settings.pyi:36:9: F811 Redefinition of unused `ssl_cert_dir` from line 34
stubs/pynput/pynput/mouse/_base.pyi:52:9: F811 Redefinition of unused `position` from line 50
stubs/pyserial/serial/serialutil.pyi:85:9: F811 Redefinition of unused `port` from line 83
stubs/pyserial/serial/serialutil.pyi:89:9: F811 Redefinition of unused `baudrate` from line 87
stubs/pyserial/serial/serialutil.pyi:93:9: F811 Redefinition of unused `bytesize` from line 91
stubs/pyserial/serial/serialutil.pyi:97:9: F811 Redefinition of unused `exclusive` from line 95
stubs/pyserial/serial/serialutil.pyi:101:9: F811 Redefinition of unused `parity` from line 99
stubs/pyserial/serial/serialutil.pyi:105:9: F811 Redefinition of unused `stopbits` from line 103
stubs/pyserial/serial/serialutil.pyi:109:9: F811 Redefinition of unused `timeout` from line 107
stubs/pyserial/serial/serialutil.pyi:113:9: F811 Redefinition of unused `write_timeout` from line 111
stubs/pyserial/serial/serialutil.pyi:117:9: F811 Redefinition of unused `inter_byte_timeout` from line 115
stubs/pyserial/serial/serialutil.pyi:121:9: F811 Redefinition of unused `xonxoff` from line 119
stubs/pyserial/serial/serialutil.pyi:125:9: F811 Redefinition of unused `rtscts` from line 123
stubs/pyserial/serial/serialutil.pyi:129:9: F811 Redefinition of unused `dsrdtr` from line 127
stubs/pyserial/serial/serialutil.pyi:133:9: F811 Redefinition of unused `rts` from line 131
stubs/pyserial/serial/serialutil.pyi:137:9: F811 Redefinition of unused `dtr` from line 135
stubs/pyserial/serial/serialutil.pyi:141:9: F811 Redefinition of unused `break_condition` from line 139
stubs/pyserial/serial/serialutil.pyi:145:9: F811 Redefinition of unused `rs485_mode` from line 143
stubs/pysftp/pysftp/__init__.pyi:126:9: F811 Redefinition of unused `timeout` from line 124
stubs/pysftp/pysftp/helpers.pyi:16:9: F811 Redefinition of unused `flist` from line 14
stubs/pysftp/pysftp/helpers.pyi:20:9: F811 Redefinition of unused `dlist` from line 18
stubs/pysftp/pysftp/helpers.pyi:24:9: F811 Redefinition of unused `ulist` from line 22
stubs/python-dateutil/dateutil/relativedelta.pyi:64:9: F811 Redefinition of unused `weeks` from line 62
stubs/pywin32/_win32typing.pyi:82:9: F811 Redefinition of unused `Callname` from line 80
stubs/pywin32/_win32typing.pyi:91:9: F811 Redefinition of unused `Name` from line 89
stubs/pywin32/_win32typing.pyi:813:9: F811 Redefinition of unused `DriverData` from line 811
stubs/pyxdg/xdg/Menu.pyi:79:9: F811 Redefinition of unused `order` from line 77
stubs/requests-oauthlib/requests_oauthlib/oauth1_session.pyi:58:9: F811 Redefinition of unused `token` from line 56
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:58:9: F811 Redefinition of unused `scope` from line 56
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:63:9: F811 Redefinition of unused `client_id` from line 61
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:65:9: F811 Redefinition of unused `client_id` from line 63
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:69:9: F811 Redefinition of unused `token` from line 67
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:73:9: F811 Redefinition of unused `access_token` from line 71
stubs/requests-oauthlib/requests_oauthlib/oauth2_session.pyi:75:9: F811 Redefinition of unused `access_token` from line 73
stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi:25:9: F811 Redefinition of unused `prereleases` from line 23
stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi:36:9: F811 Redefinition of unused `prereleases` from line 34
stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi:53:9: F811 Redefinition of unused `prereleases` from line 51
stubs/setuptools/setuptools/command/egg_info.pyi:36:9: F811 Redefinition of unused `tag_svn_revision` from line 34
stubs/tensorflow/tensorflow/keras/layers/__init__.pyi:52:9: F811 Redefinition of unused `trainable` from line 50
stubs/tensorflow/tensorflow/keras/optimizers/legacy/__init__.pyi:40:9: F811 Redefinition of unused `iterations` from line 38
stubs/tqdm/tqdm/notebook.pyi:28:9: F811 Redefinition of unused `colour` from line 26
Found 1647 errors.
```

</details>

Looking more closely at those new errors this morning, however, I think most fall into two buckets:
- Ruff starts thinking a bunch of `__all__`s are undefined. Presumably this is because `__all__` is special-case somewhere in the semantic model, and deferring all expression resolution in stubs breaks that special casing. F821 starts being emitted on things like this:
  
  ```py
  __all__ = ["foo"]
  __all__ += ["bar"]  # F821 here: Ruff now thinks __all__ is undefined
  ```

- Ruff starts thinking properties with setters count as "redefinitions" of the same symbol (strictly correct, actually, but obviously we want properties to be special-cased by F811, and it seems like deferring all expression resolution in stubs also breaks that special-casing). I.e., F811 starts being emitted on things like this:

  ```py
  class Foo:
      @property
      def bar(self) -> int: ...
      @bar.setter
      def bar(self, val: int) -> None: ...  # F811 here
  ```

So, perhaps if those two issues were resolved, then the patch that defers resolution of all expressions might be a more principled basis for a fix for this issue?

---

_Comment by @AlexWaygood on 2024-04-05 09:22_

It is also worth noting that, while this PR doesn't get us to the generalised support for forward references that type checkers have for stubs, the approach here is actually very similar to the monkey-patching flake8-pyi does. Flake8-pyi monkey-patches flake8 to do three things:
1. Set PEP-563 semantics to always being enabled in stub files (we already do this).
2. Allows forward references in class bases (this PR does this).
3. Allow forward references on the r.h.s. of simple assignments such as `foo = bar` (this PR does not do this -- though it could be easily expanded to do that as well -- but there's no real use case for it, it should probably be discouraged as a matter of style, and typeshed only does it twice across its whole codebase).

---

_@charliermarsh approved on 2024-04-06 16:44_

This seems reasonable to me.

---

_Comment by @AlexWaygood on 2024-04-06 16:45_

gah, why did I go make merge conflicts for myself?

---

_Comment by @AlexWaygood on 2024-04-07 00:15_

Although this doesn't directly match the behaviour of type checkers with regards to stub files, I think I will merge this for now:
- It solves all known _use cases_ for forward references in stub files, significantly reducing false positives for users of ruff
- It's pretty non-invasive, so if we _do_ decide to implement generalised support for forward references everywhere in stub files in the future, it should be pretty easy to adjust or revert these changes
- The current behaviour of major Python type checkers, where they allow forward references in all contexts, doesn't actually appear to be specc'd anywhere, so we won't be in violation of any spec detailing how stub files should work — and it's not clear to me that it's even desirable to allow forward references in _all_ contexts, even if that's what mypy and pyright do currently.
- There's value for us in keeping special cases for stubs to a minimum in our semantic model, so that stubs and runtime files broadly work the same way. This lowers the conceptual complexity of dealing with stub files for us, and means that we don't need to have duplicate test files for stubs for every fixture

---

_Merged by @AlexWaygood on 2024-04-07 00:15_

---

_Closed by @AlexWaygood on 2024-04-07 00:15_

---

_Branch deleted on 2024-04-07 00:16_

---
