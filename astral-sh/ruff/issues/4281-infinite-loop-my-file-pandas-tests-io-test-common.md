---
number: 4281
title: "[Infinite loop] My File: pandas\\tests\\io\\test_common.py"
type: issue
state: closed
author: FishAlchemist
labels: []
assignees: []
created_at: 2023-05-08T12:15:07Z
updated_at: 2023-05-08T12:39:59Z
url: https://github.com/astral-sh/ruff/issues/4281
synced_at: 2026-01-10T01:22:43Z
---

# [Infinite loop] My File: pandas\tests\io\test_common.py

---

_Issue opened by @FishAlchemist on 2023-05-08 12:15_

## Version(ruff --version)
**ruff 0.0.265**
## Command(Powershell)
```powershell
ruff check --select=ALL .\test_common.py
```
**If you add --isolated , there will be no problem about Infinite loop**
## Terminal Content
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.

error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `test_common.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

test_common.py:27:9: D107 Missing docstring in `__init__`
test_common.py:27:18: ANN101 Missing type annotation for `self` in method
test_common.py:27:24: ANN001 Missing type annotation for function argument `path`
test_common.py:30:9: ANN204 Missing return type annotation for special method `__fspath__`
test_common.py:30:9: D105 Missing docstring in magic method
test_common.py:30:20: ANN101 Missing type annotation for `self` in method
test_common.py:38:25: N812 Lowercase `local` imported as non-lowercase `LocalPath`
test_common.py:44:8: PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
test_common.py:44:24: PTH120 `os.path.dirname()` should be replaced by `Path.parent`
test_common.py:48:7: D101 Missing docstring in public class
test_common.py:58:9: ANN201 Missing return type annotation for public function `test_expand_user`
test_common.py:58:9: D102 Missing docstring in public method
test_common.py:58:26: ANN101 Missing type annotation for `self` in method
test_common.py:60:25: SLF001 Private member accessed: `_expand_user`
test_common.py:62:9: S101 Use of `assert` detected
test_common.py:63:9: S101 Use of `assert` detected
test_common.py:63:16: PTH117 `os.path.isabs()` should be replaced by `Path.is_absolute()`
test_common.py:64:9: S101 Use of `assert` detected
test_common.py:64:16: PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
test_common.py:66:9: ANN201 Missing return type annotation for public function `test_expand_user_normal_path`
test_common.py:66:9: D102 Missing docstring in public method
test_common.py:66:38: ANN101 Missing type annotation for `self` in method
test_common.py:68:25: SLF001 Private member accessed: `_expand_user`
test_common.py:70:9: S101 Use of `assert` detected
test_common.py:71:9: S101 Use of `assert` detected
test_common.py:71:16: PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
test_common.py:73:9: ANN201 Missing return type annotation for public function `test_stringify_path_pathlib`
test_common.py:73:9: D102 Missing docstring in public method
test_common.py:73:37: ANN101 Missing type annotation for `self` in method
test_common.py:75:9: S101 Use of `assert` detected
test_common.py:77:9: S101 Use of `assert` detected
test_common.py:77:34: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:80:9: ANN201 Missing return type annotation for public function `test_stringify_path_localpath`
test_common.py:80:9: D102 Missing docstring in public method
test_common.py:80:39: ANN101 Missing type annotation for `self` in method
test_common.py:81:16: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:82:20: PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
test_common.py:83:17: PTH124 `py.path` is in maintenance mode, use `pathlib` instead
test_common.py:84:9: S101 Use of `assert` detected
test_common.py:86:9: ANN201 Missing return type annotation for public function `test_stringify_path_fspath`
test_common.py:86:9: D102 Missing docstring in public method
test_common.py:86:36: ANN101 Missing type annotation for `self` in method
test_common.py:89:9: S101 Use of `assert` detected
test_common.py:91:9: ANN201 Missing return type annotation for public function `test_stringify_file_and_path_like`
test_common.py:91:9: D102 Missing docstring in public method
test_common.py:91:43: ANN101 Missing type annotation for `self` in method
test_common.py:94:9: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
test_common.py:96:17: S101 Use of `assert` detected
test_common.py:99:9: ANN201 Missing return type annotation for public function `test_infer_compression_from_path`
test_common.py:99:9: D102 Missing docstring in public method
test_common.py:99:42: ANN101 Missing type annotation for `self` in method
test_common.py:99:48: ANN001 Missing type annotation for function argument `compression_format`
test_common.py:99:68: ANN001 Missing type annotation for function argument `path_type`
test_common.py:103:9: S101 Use of `assert` detected
test_common.py:106:9: ANN201 Missing return type annotation for public function `test_get_handle_with_path`
test_common.py:106:9: D102 Missing docstring in public method
test_common.py:106:35: ANN101 Missing type annotation for `self` in method
test_common.py:106:41: ANN001 Missing type annotation for function argument `path_type`
test_common.py:111:17: S101 Use of `assert` detected
test_common.py:112:17: S101 Use of `assert` detected
test_common.py:112:24: PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
test_common.py:114:9: ANN201 Missing return type annotation for public function `test_get_handle_with_buffer`
test_common.py:114:9: D102 Missing docstring in public method
test_common.py:114:37: ANN101 Missing type annotation for `self` in method
test_common.py:117:17: S101 Use of `assert` detected
test_common.py:118:13: S101 Use of `assert` detected
test_common.py:119:9: S101 Use of `assert` detected
test_common.py:122:9: ANN201 Missing return type annotation for public function `test_bytesiowrapper_returns_correct_bytes`
test_common.py:122:9: D102 Missing docstring in public method
test_common.py:122:51: ANN101 Missing type annotation for `self` in method
test_common.py:134:17: S101 Use of `assert` detected
test_common.py:138:21: S101 Use of `assert` detected
test_common.py:142:13: S101 Use of `assert` detected
test_common.py:146:9: ANN201 Missing return type annotation for public function `test_get_handle_pyarrow_compat`
test_common.py:146:9: D102 Missing docstring in public method
test_common.py:146:40: ANN101 Missing type annotation for `self` in method
test_common.py:155:88: E501 Line too long (89 > 88 characters)
test_common.py:159:13: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:161:13: S101 Use of `assert` detected
test_common.py:163:9: ANN201 Missing return type annotation for public function `test_iterator`
test_common.py:163:9: D102 Missing docstring in public method
test_common.py:163:23: ANN101 Missing type annotation for `self` in method
test_common.py:189:9: ANN201 Missing return type annotation for public function `test_read_non_existent`
test_common.py:189:9: D102 Missing docstring in public method
test_common.py:189:32: ANN101 Missing type annotation for `self` in method
test_common.py:189:38: ANN001 Missing type annotation for function argument `reader`
test_common.py:189:46: ANN001 Missing type annotation for function argument `module`
test_common.py:189:54: ANN001 Missing type annotation for function argument `error_class`
test_common.py:189:67: ANN001 Missing type annotation for function argument `fn_ext`
test_common.py:192:16: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:227:9: ANN201 Missing return type annotation for public function `test_write_missing_parent_directory`
test_common.py:227:9: D102 Missing docstring in public method
test_common.py:227:45: ANN101 Missing type annotation for `self` in method
test_common.py:227:51: ANN001 Missing type annotation for function argument `method`
test_common.py:227:59: ANN001 Missing type annotation for function argument `module`
test_common.py:227:67: ANN001 Missing type annotation for function argument `error_class`
test_common.py:227:80: ANN001 Missing type annotation for function argument `fn_ext`
test_common.py:232:16: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:255:9: PLR0913 Too many arguments to function call (6 > 5)
test_common.py:255:9: ANN201 Missing return type annotation for public function `test_read_expands_user_home_dir`
test_common.py:255:9: D102 Missing docstring in public method
test_common.py:256:9: ANN101 Missing type annotation for `self` in method
test_common.py:256:15: ANN001 Missing type annotation for function argument `reader`
test_common.py:256:23: ANN001 Missing type annotation for function argument `module`
test_common.py:256:31: ANN001 Missing type annotation for function argument `error_class`
test_common.py:256:44: ANN001 Missing type annotation for function argument `fn_ext`
test_common.py:256:52: ANN001 Missing type annotation for function argument `monkeypatch`
test_common.py:260:16: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:261:61: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
test_common.py:314:9: ANN201 Missing return type annotation for public function `test_read_fspath_all`
test_common.py:314:9: D102 Missing docstring in public method
test_common.py:314:30: ANN101 Missing type annotation for `self` in method
test_common.py:314:36: ANN001 Missing type annotation for function argument `reader`
test_common.py:314:44: ANN001 Missing type annotation for function argument `module`
test_common.py:314:52: ANN001 Missing type annotation for function argument `path`
test_common.py:314:58: ANN001 Missing type annotation for function argument `datapath`
test_common.py:341:9: ANN201 Missing return type annotation for public function `test_write_fspath_all`
test_common.py:341:9: D102 Missing docstring in public method
test_common.py:341:31: ANN101 Missing type annotation for `self` in method
test_common.py:341:37: ANN001 Missing type annotation for function argument `writer_name`
test_common.py:341:50: ANN001 Missing type annotation for function argument `writer_kwargs`
test_common.py:341:65: ANN001 Missing type annotation for function argument `module`
test_common.py:346:9: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:355:18: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:355:47: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:365:21: S101 Use of `assert` detected
test_common.py:367:9: ANN201 Missing return type annotation for public function `test_write_fspath_hdf5`
test_common.py:367:9: D102 Missing docstring in public method
test_common.py:367:32: ANN101 Missing type annotation for `self` in method
test_common.py:373:9: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:389:5: ANN201 Missing return type annotation for public function `mmap_file`
test_common.py:389:5: D103 Missing docstring in public function
test_common.py:389:15: ANN001 Missing type annotation for function argument `datapath`
test_common.py:393:7: D101 Missing docstring in public class
test_common.py:394:9: ANN201 Missing return type annotation for public function `test_constructor_bad_file`
test_common.py:394:9: D102 Missing docstring in public method
test_common.py:394:35: ANN101 Missing type annotation for `self` in method
test_common.py:394:41: ANN001 Missing type annotation for function argument `mmap_file`
test_common.py:407:13: SLF001 Private member accessed: `_maybe_memory_map`
test_common.py:407:46: FBT003 Boolean positional value in function call
test_common.py:409:14: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:414:13: SLF001 Private member accessed: `_maybe_memory_map`
test_common.py:414:44: FBT003 Boolean positional value in function call
test_common.py:416:9: ANN201 Missing return type annotation for public function `test_next`
test_common.py:416:9: D102 Missing docstring in public method
test_common.py:416:19: ANN101 Missing type annotation for `self` in method
test_common.py:416:25: ANN001 Missing type annotation for function argument `mmap_file`
test_common.py:417:14: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:424:17: S101 Use of `assert` detected
test_common.py:428:21: S101 Use of `assert` detected
test_common.py:433:9: ANN201 Missing return type annotation for public function `test_unknown_engine`
test_common.py:433:9: D102 Missing docstring in public method
test_common.py:433:29: ANN101 Missing type annotation for `self` in method
test_common.py:435:13: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:440:9: ANN201 Missing return type annotation for public function `test_binary_mode`
test_common.py:440:26: ANN101 Missing type annotation for `self` in method
test_common.py:441:9: D403 First word of the first line should be capitalized: `'encoding'` -> `'encoding'`
test_common.py:446:13: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:452:9: ANN201 Missing return type annotation for public function `test_warning_missing_utf_bom`
test_common.py:452:38: ANN101 Missing type annotation for `self` in method
test_common.py:452:44: ANN001 Missing type annotation for function argument `encoding`
test_common.py:452:54: ANN001 Missing type annotation for function argument `compression_`
test_common.py:459:9: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:470:5: ANN201 Missing return type annotation for public function `test_is_fsspec_url`
test_common.py:470:5: D103 Missing docstring in public function
test_common.py:471:5: S101 Use of `assert` detected
test_common.py:472:5: S101 Use of `assert` detected
test_common.py:474:5: S101 Use of `assert` detected
test_common.py:475:5: S101 Use of `assert` detected
test_common.py:476:5: S101 Use of `assert` detected
test_common.py:477:5: S101 Use of `assert` detected
test_common.py:479:5: S101 Use of `assert` detected
test_common.py:480:5: S101 Use of `assert` detected
test_common.py:482:5: S101 Use of `assert` detected
test_common.py:487:5: ANN201 Missing return type annotation for public function `test_codecs_encoding`
test_common.py:487:5: D103 Missing docstring in public function
test_common.py:487:26: ANN001 Missing type annotation for function argument `encoding`
test_common.py:487:36: A002 Argument `format` is shadowing a python builtin
test_common.py:487:36: ANN001 Missing type annotation for function argument `format`
test_common.py:495:17: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:497:17: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:501:5: ANN201 Missing return type annotation for public function `test_codecs_get_writer_reader`
test_common.py:501:5: D103 Missing docstring in public function
test_common.py:505:14: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:507:14: PTH123 `open()` should be replaced by `Path.open()`
test_common.py:508:13: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:519:5: ANN201 Missing return type annotation for public function `test_explicit_encoding`
test_common.py:519:5: D103 Missing docstring in public function
test_common.py:519:28: ANN001 Missing type annotation for function argument `io_class`
test_common.py:519:38: ANN001 Missing type annotation for function argument `mode`
test_common.py:519:44: ANN001 Missing type annotation for function argument `msg`
test_common.py:530:5: ANN201 Missing return type annotation for public function `test_encoding_errors`
test_common.py:530:5: D103 Missing docstring in public function
test_common.py:530:26: ANN001 Missing type annotation for function argument `encoding_errors`
test_common.py:530:43: A002 Argument `format` is shadowing a python builtin
test_common.py:530:43: ANN001 Missing type annotation for function argument `format`
test_common.py:557:13: PD901 `df` is a bad variable name. Be kinder to your future self.
test_common.py:563:5: ANN201 Missing return type annotation for public function `test_bad_encdoing_errors`
test_common.py:563:5: D103 Missing docstring in public function
test_common.py:565:5: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
test_common.py:570:5: ANN201 Missing return type annotation for public function `test_errno_attribute`
test_common.py:570:5: D103 Missing docstring in public function
test_common.py:572:5: PT012 `pytest.raises()` block should contain a single simple statement
test_common.py:574:9: S101 Use of `assert` detected
test_common.py:577:5: ANN201 Missing return type annotation for public function `test_fail_mmap`
test_common.py:577:5: D103 Missing docstring in public function
test_common.py:582:5: ANN201 Missing return type annotation for public function `test_close_on_error`
test_common.py:582:5: D103 Missing docstring in public function
test_common.py:585:13: ANN202 Missing return type annotation for private function `close`
test_common.py:585:19: ANN101 Missing type annotation for `self` in method
test_common.py:589:5: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
test_common.py:589:5: PT012 `pytest.raises()` block should contain a single simple statement
test_common.py:608:5: ANN201 Missing return type annotation for public function `test_pickle_reader`
test_common.py:608:5: D103 Missing docstring in public function
test_common.py:608:24: ANN001 Missing type annotation for function argument `reader`
Found 316 errors (101 fixed, 215 remaining).
```
### Command(Powershell): --isolated
```powershell
ruff check --select=ALL --isolated .\test_common.py
```
[full_terminal_content_isolated.txt](https://github.com/charliermarsh/ruff/files/11420978/full_terminal_content_isolated.txt)
## Python Code
**Please change the file extension from txt to py**
[test_common.txt](https://github.com/charliermarsh/ruff/files/11420981/test_common.txt)

## pyproject.toml
**Please change the file extension from txt to toml**
[pyproject.txt](https://github.com/charliermarsh/ruff/files/11420985/pyproject.txt)




---

_Comment by @MichaReiser on 2023-05-08 12:39_

Thanks for reporting this issue. 

I'll close it because it has the same root cause as #4279 

```
Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:
D403-test.py:441:9: D403 [*] First word of the first line should be capitalized: `'encoding'` -> `'encoding'`
    |
441 |       def test_binary_mode(self):
442 |           """'encoding' shouldn't be passed to 'open' in binary mode.
    |  _________^
443 | | 
444 | |         GH 35058
445 | |         """
    | |___________^ D403
446 |           with tm.ensure_clean() as path:
447 |               df = tm.makeDataFrame()
    |
    = help: Capitalize `'encoding'` to `'encoding'`

ℹ Suggested fix
```

---

_Closed by @MichaReiser on 2023-05-08 12:39_

---
