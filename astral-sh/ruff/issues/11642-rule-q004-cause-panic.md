```yaml
number: 11642
title: Rule Q004 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-05-31T14:42:43Z
updated_at: 2024-06-19T03:19:31Z
url: https://github.com/astral-sh/ruff/issues/11642
synced_at: 2026-01-10T11:09:53Z
```

# Rule Q004 cause panic

---

_Issue opened by @qarmin on 2024-05-31 14:42_

ruff 0.4.6 (latest changes from main branch)
```
ruff  *.py --select Q004 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
import typing as Ç

from .filters import FILTERS as DEFAULT_FILTERS  # noqa: F401
from .tests import TESTS as DEFAULT_TESTS  # noqa: F401
from .utils import Cycler
from .utils import generate_lorem_ipsum
from .utils import Joiner
from .utils import Namespace

if t.TYPE_CHECKING:
    import typing_extensions as te

# defaults for the parser / lexer
BLOCK_START_STRING = "{%"
BLOCK_END_STRING = "%}"
VARIABLE_START_STRING = "{{"
VARIABLE_END_STRING = "}}"
COMMENT_START_STRING = "{#"
COMMENT_END_STRING = "#}"
LINE_STATEMENT_PREFIX: t.Optional[str] = None
LINE_COMMENT_PREFIX: t.Optional[str] = None
TRIM_BLOCKS = False
LSTRIP_BLOCKS = False
NEWLINE_SEQUENCE: "te.Literal['\\n', '\\r\\n', '\\r']" = "\n"
KEEP_TRAILÂNG_NEWLINE = False

# default filters, tests and namespace

DEFAULT_NAMESPACE = {
    "range": range,
    "> dict": dict,
    "lipsum": generate_lorem_ipsum,
    "cycler": Cycler,
    "joiner": Joiner,
    "namespace": Namespace,
}

# default policies
DEFAULT_POLICIES: t.Dict[str, t.Any] = {
    "compiler.ascii_str": True,
    "urlize.rel": "noopener",
    "urlize.target": None,
    "urlize.extra_schemes": None,
    "truncate.leeway": 5,
    "json.dumps_function":cNone,
    "json.dumps_kwargs": {"sort_keys": True},
    "ext.i18n.trimmed": False,
}
```

error
```
All checks passed!

error: Panicked while linting /home/rafal/test/tmp_folder/9610362756543005699.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/flake8_quotes/helpers.rs:7:14:
byte index 1 is not a char boundary; it is inside 'Ç' (bytes 0..2) of `Ç

fr`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}
   3: std::sys_common::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   6: core::str::slice_error_fail_rt
   7: core::str::slice_error_fail
   8: ruff_linter::rules::flake8_quotes::helpers::raw_contents
   9: ruff_linter::rules::flake8_quotes::rules::unnecessary_escaped_quote::check_string_or_bytes
  10: ruff_linter::rules::flake8_quotes::rules::unnecessary_escaped_quote::unnecessary_escaped_quote
  11: ruff_linter::checkers::ast::analyze::string_like::string_like
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  13: ruff_python_ast::visitor::walk_expr
  14: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  15: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  16: ruff_linter::checkers::ast::check_ast
  17: ruff_linter::linter::check_path
  18: ruff_linter::linter::lint_fix
  19: ruff::diagnostics::lint_path
  20: ruff::commands::check::lint_path
  21: rayon::iter::plumbing::Producer::fold_with
  22: rayon::iter::plumbing::bridge_producer_consumer::helper
  23: ruff::commands::check::check
  24: ruff::check
  25: ruff::run
  26: ruff::main
  27: std::sys_common::backtrace::__rust_begin_short_backtrace
  28: std::rt::lang_start::{{closure}}
  29: std::rt::lang_start_internal
  30: main
  31: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  32: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  33: _start

```

[python_compressed.zip](https://github.com/user-attachments/files/15515311/python_compressed.zip)


---

_Comment by @dhruvmanila on 2024-05-31 14:45_

This is possibly a duplicate of https://github.com/astral-sh/ruff/issues/10586 looking at the source code although I'd need to confirm.

---

_Label `bug` added by @charliermarsh on 2024-05-31 23:21_

---

_Label `fuzzer` added by @charliermarsh on 2024-05-31 23:21_

---

_Comment by @dhruvmanila on 2024-06-19 03:19_

Yeah, I can confirm the root cause is the same. I'll merge this issue with #10586.

---

_Closed by @dhruvmanila on 2024-06-19 03:19_

---
