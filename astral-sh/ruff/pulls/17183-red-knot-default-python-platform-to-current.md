```yaml
number: 17183
title: "[red-knot] Default `python-platform` to current platform"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/default-to-current-platform
created_at: 2025-04-03T19:39:58Z
updated_at: 2025-04-09T10:05:20Z
url: https://github.com/astral-sh/ruff/pull/17183
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Default `python-platform` to current platform

---

_@MichaReiser_

## Summary

As discussed in https://github.com/astral-sh/ty/issues/160 and "mitigate" said issue for the alpha. 

This PR changes the default for `PythonPlatform` to be the current platform rather than `all`. 

I'm not sure if we should be as sophisticated as supporting `ios` and `android` as defaults but it was easy... 

## Test Plan

Some mdtests started failing because they now defaulted to `darwin`. 


---

_Label `red-knot` added by @MichaReiser on 2025-04-03 19:41_

---

_Comment by @github-actions[bot] on 2025-04-03 19:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:95:38: Attribute `confstr` on type `<module 'os'>` is possibly unbound
- Found 349 diagnostics
+ Found 348 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/async-utils/src/async_utils/sig_service.py:128:21: Attribute `siginterrupt` on type `<module 'signal'>` is possibly unbound
- Found 39 diagnostics
+ Found 38 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:106:22: Type `<module 'winreg'>` has no attribute `OpenKey`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:107:21: Type `<module 'winreg'>` has no attribute `HKEY_LOCAL_MACHINE`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:21: Type `<module 'winreg'>` has no attribute `KEY_READ`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:39: Type `<module 'winreg'>` has no attribute `KEY_WOW64_64KEY`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:116:37: Type `<module 'winreg'>` has no attribute `REG_SZ`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:435:18: Attribute `tcgetattr` on type `<module 'termios'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:437:28: Attribute `ECHO` on type `<module 'termios'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:438:26: Attribute `ECHO` on type `<module 'termios'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:439:9: Attribute `tcsetattr` on type `<module 'termios'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:439:38: Attribute `TCSANOW` on type `<module 'termios'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:106:22: Attribute `OpenKey` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:107:21: Attribute `HKEY_LOCAL_MACHINE` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:21: Attribute `KEY_READ` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:39: Attribute `KEY_WOW64_64KEY` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Attribute `QueryValueEx` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Attribute `QueryValueEx` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Attribute `QueryValueEx` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:116:37: Attribute `REG_SZ` on type `<module 'winreg'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/conftest.py:27:35: Attribute `AF_UNIX` on type `<module 'socket'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/serving.py:69:20: Attribute `ForkingMixIn` on type `<module 'socketserver'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/serving.py:77:15: Attribute `AF_UNIX` on type `<module 'socket'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/serving.py:659:16: Attribute `AF_UNIX` on type `<module 'socket'>` is possibly unbound
- Found 779 diagnostics
+ Found 770 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:515:5: Type `<module 'ctypes'>` has no attribute `windll`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:526:12: Type `<module 'ctypes'>` has no attribute `windll`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:559:28: Module `ctypes` has no member `windll`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:515:5: Attribute `windll` on type `<module 'ctypes'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:526:12: Attribute `windll` on type `<module 'ctypes'>` is possibly unbound
- warning[lint:possibly-unbound-import] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:559:28: Member `windll` of module `ctypes` is possibly unbound

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:33:24: Name `ILLEGAL_PATH_CHARS` used when possibly not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:33:24: Name `ILLEGAL_PATH_CHARS` used when not defined

rich (https://github.com/Textualize/rich)
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:17: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:47: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:38:32: Name `_win32_console` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:44:34: Name `CURSOR_SIZE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:48:13: Name `_win32_console` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:50:26: Name `StubScreenBufferInfo` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:52:13: Name `_win32_console` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:60:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:40: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:63: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:77: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:64:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:65:36: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:17: Name `SCREEN_HEIGHT` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:36: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:71:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:87:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:103:47: Name `DEFAULT_STYLE_ATTRIBUTE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:111:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:128:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:145:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:162:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:179:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:200:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:17: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:40: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:204:39: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:59: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:218:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:39: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:54: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:70: Name `CURSOR_POSITION` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:226:13: Name `DEFAULT_STYLE_ATTRIBUTE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:20: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:35: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:228:19: Name `CURSOR_POSITION` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:239:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:17: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:36: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:245:39: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:59: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:255:18: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:256:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:266:18: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:267:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:277:18: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:278:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:288:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:57: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:75: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:300:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:57: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:75: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:312:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:57: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:71: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:324:33: Name `StubScreenBufferInfo` used when possibly not defined
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:13: Argument `dwCursorPosition` does not match any known parameter of bound method `__init__`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:30: Name `COORD` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:36: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:54: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:330:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:57: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:341:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:53: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:351:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:34: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:57: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:71: Name `CURSOR_X` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:361:35: Name `StubScreenBufferInfo` used when possibly not defined
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:13: Argument `dwCursorPosition` does not match any known parameter of bound method `__init__`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:30: Name `COORD` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:39: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:367:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:20: Name `WindowsCoordinates` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:43: Name `CURSOR_Y` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:61: Name `SCREEN_WIDTH` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:376:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:385:48: Name `CURSOR_SIZE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:389:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:398:48: Name `CURSOR_SIZE` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:402:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:409:16: Name `LegacyWindowsTerm` used when possibly not defined
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:20:32: Attribute `WinDLL` on type `<module 'ctypes'>` is possibly unbound
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/rich/_windows.py:46:18: Name `GetStdHandle` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/rich/_windows.py:48:28: Name `GetConsoleMode` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/rich/_windows.py:50:16: Name `LegacyWindowsError` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/rich/rich/_windows.py:53:46: Name `ENABLE_VIRTUAL_TERMINAL_PROCESSING` used when possibly not defined
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:56:27: Attribute `getwindowsversion` on type `<module 'sys'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/ansi.py:229:5: Attribute `spawn` on type `<module 'pty'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:12:35: Attribute `WinDLL` on type `<module 'ctypes'>` is possibly unbound
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:20:32: Type `<module 'ctypes'>` has no attribute `WinDLL`
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_windows.py:40:43: Variable of type `Never` is not allowed in a type expression
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:56:27: Type `<module 'sys'>` has no attribute `getwindowsversion`
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:17: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:47: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:38:32: Name `_win32_console` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:44:34: Name `CURSOR_SIZE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:48:13: Name `_win32_console` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:50:26: Name `StubScreenBufferInfo` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:52:13: Name `_win32_console` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:60:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:40: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:63: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:77: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:64:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:65:36: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:17: Name `SCREEN_HEIGHT` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:36: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:71:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:87:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:103:47: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:111:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:128:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:145:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:162:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:179:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:200:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:17: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:40: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:204:39: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:59: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:218:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:39: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:54: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:70: Name `CURSOR_POSITION` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:226:13: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:20: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:35: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:228:19: Name `CURSOR_POSITION` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:239:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:17: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:36: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:245:39: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:59: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:255:18: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:256:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:266:18: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:267:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:277:18: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:278:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:288:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:57: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:75: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:300:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:57: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:75: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:312:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:57: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:71: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:324:33: Name `StubScreenBufferInfo` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:30: Name `COORD` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:36: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:54: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:330:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:57: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:341:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:53: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:351:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:34: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:57: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:71: Name `CURSOR_X` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:361:35: Name `StubScreenBufferInfo` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:30: Name `COORD` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:39: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:367:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:20: Name `WindowsCoordinates` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:43: Name `CURSOR_Y` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:61: Name `SCREEN_WIDTH` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:376:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:385:48: Name `CURSOR_SIZE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:389:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:398:48: Name `CURSOR_SIZE` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:402:16: Name `LegacyWindowsTerm` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:409:16: Name `LegacyWindowsTerm` used when not defined
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:12:35: Type `<module 'ctypes'>` has no attribute `WinDLL`
+ error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:33:26: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:44:57: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:57:34: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:67:27: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:78:43: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:95:32: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:129:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:132:12: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:170:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:173:12: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:205:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:205:46: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:229:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:230:6: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:253:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:253:42: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:276:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:276:47: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:300:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:300:47: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:377:34: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:387:30: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:405:46: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:444:44: Variable of type `Never` is not allowed in a type expression
- Found 868 diagnostics
+ Found 886 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ossignal.py:38:23: Type `<module 'signal'>` has no attribute `SIGBREAK`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/debug.py:37:66: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/debug.py:38:66: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/debug.py:75:66: Unused blanket `type: ignore` directive
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_same_loop.py:8:35: Attribute `WindowsSelectorEventLoopPolicy` on type `<module 'asyncio'>` is possibly unbound
- error[lint:missing-argument] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_same_loop.py:8:35: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_same_loop.py:8:35: Object of type `Literal[WindowsSelectorEventLoopPolicy]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:892:61: Attribute `SIGBREAK` on type `<module 'signal'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:906:61: Attribute `SIGBREAK` on type `<module 'signal'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ossignal.py:38:23: Attribute `SIGBREAK` on type `<module 'signal'>` is possibly unbound
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:84:17: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:86:18: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_different_loop.py:8:35: Attribute `WindowsSelectorEventLoopPolicy` on type `<module 'asyncio'>` is possibly unbound
- error[lint:missing-argument] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_different_loop.py:8:35: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_different_loop.py:8:35: Object of type `Literal[WindowsSelectorEventLoopPolicy]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:84:17: Attribute `WindowsSelectorEventLoopPolicy` on type `<module 'asyncio'>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:86:18: Attribute `WindowsSelectorEventLoopPolicy` on type `<module 'asyncio'>` is possibly unbound
- error[lint:missing-argument] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:86:18: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:86:18: Object of type `Literal[WindowsSelectorEventLoopPolicy]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_different_loop.py:8:35: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_same_loop.py:8:35: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:892:61: Type `<module 'signal'>` has no attribute `SIGBREAK`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:906:61: Type `<module 'signal'>` has no attribute `SIGBREAK`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/scrapy/scrapy/utils/console.py:85:50: Unused blanket `type: ignore` directive
- Found 1594 diagnostics
+ Found 1592 diagnostics

```
</details>


---

_Comment by @MichaReiser on 2025-04-03 19:44_

Funny, this looks worse when just looking at mypy primer. Would @sharkdp's other unreachable work help to address some of those warnings?

---

_Comment by @sharkdp on 2025-04-03 20:22_

> Funny, this looks worse when just looking at mypy primer. Would @sharkdp's other unreachable work help to address some of those warnings?

Yes, it definitely will. You're mostly just turning "possibly unbound" diagnostics into "definitely unbound" diagnostics (which are now in unreachable sections of code, which is what we want from this change). *Silencing* these "definitely unbound" errors inside unreachable sections remains an open topic that I am still working on, even after that first batch of problems has been solved.

If we want to see the effect of this change in isolation, we should consider waiting with this PR until I have fixed that.

---

_Marked ready for review by @sharkdp on 2025-04-09 09:57_

---

_Review requested from @carljm by @sharkdp on 2025-04-09 09:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-09 09:57_

---

_Review requested from @sharkdp by @sharkdp on 2025-04-09 09:57_

---

_Review requested from @dcreager by @sharkdp on 2025-04-09 09:57_

---

_@sharkdp approved on 2025-04-09 10:00_

Thank you!

I have analyzed all ecosystem diagnostic changes and added new test cases for those that we don't handle properly yet (#17306). Merging this will help me analyze the upcoming changes around unreachable-code diagnostics in a better way.

---

_Merged by @sharkdp on 2025-04-09 10:05_

---

_Closed by @sharkdp on 2025-04-09 10:05_

---

_Branch deleted on 2025-04-09 10:05_

---
