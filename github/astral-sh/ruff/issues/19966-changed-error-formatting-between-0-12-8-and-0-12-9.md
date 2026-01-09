---
number: 19966
title: Changed error formatting between 0.12.8.and 0.12.9
type: issue
state: closed
author: MeggyCal
labels:
  - question
assignees: []
created_at: 2025-08-18T11:34:33Z
updated_at: 2025-09-17T07:18:37Z
url: https://github.com/astral-sh/ruff/issues/19966
synced_at: 2026-01-07T13:12:16-06:00
---

# Changed error formatting between 0.12.8.and 0.12.9

---

_Issue opened by @MeggyCal on 2025-08-18 11:34_

### Question

Hi, thank you for maintaining this useful tool! I am a packager in openSUSE, with an update to 0.12.9 we are observing a test failure in `pytest-examples` (the tests ran fine with `ruff` 0.12.8).

It looks like `ruff` stopped printing pointers to the place where the error occurred. The patch to `pytest-examples` would be trivial, but I just wanted to make sure that it was intentional (nothing in the release highlights sounded like this change to me).

The log:
```
[   14s] =================================== FAILURES ===================================
[   14s] _______________________________ test_ruff_offset _______________________________
[   14s] 
[   14s]     def test_ruff_offset():
[   14s]         code = 'print(x)\n'
[   14s]         example = CodeExample.create(code)
[   14s]         with pytest.raises(FormatError, match='testing.md:1:7: F821 Undefined name'):
[   14s] >           ruff_check(example, ExamplesConfig())
[   14s] 
[   14s] tests/test_lint.py:28: 
[   14s] _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
[   14s] 
[   14s] example = CodeExample(source='print(x)\n', path=PosixPath('testing.md'), start_line=0, end_line=1, start_index=0, end_index=9, prefix='', indent=0, group=None, test_id=None)
[   14s] config = ExamplesConfig(line_length=88, quotes='either', magic_trailing_comma=True, target_version='py37', upgrade=False, isort=False, ruff_line_length=None, ruff_select=None, ruff_ignore=None, white_space_dot=False)
[   14s] 
[   14s]     def ruff_check(
[   14s]         example: CodeExample,
[   14s]         config: ExamplesConfig,
[   14s]         *,
[   14s]         extra_ruff_args: tuple[str, ...] = (),
[   14s]     ) -> str:
[   14s]         ruff = find_ruff_bin()
[   14s]         args = ruff, 'check', '-', *config.ruff_config(), *extra_ruff_args
[   14s]     
[   14s]         p = Popen(args, stdin=PIPE, stdout=PIPE, stderr=PIPE, universal_newlines=True)
[   14s]         stdout, stderr = p.communicate(example.source, timeout=10)
[   14s]         if p.returncode == 1 and stdout:
[   14s]     
[   14s]             def replace_offset(m: re.Match[str]):
[   14s]                 line_number = int(m.group(1))
[   14s]                 return f'{example.path}:{line_number + example.start_line}'
[   14s]     
[   14s]             output = re.sub(r'^-:(\d+)', replace_offset, stdout, flags=re.M)
[   14s] >           raise FormatError(f'ruff failed:\n{indent(output, "  ")}')
[   14s] E           pytest_examples.lint.FormatError: ruff failed:
[   14s] E             F821 Undefined name `x`
[   14s] E              --> -:1:7
[   14s] E               |
[   14s] E             1 | print(x)
[   14s] E               |       ^
[   14s] E               |
[   14s] E           
[   14s] E             Found 1 error.
[   14s] 
[   14s] ../BUILDROOT/usr/lib/python3.11/site-packages/pytest_examples/lint.py:63: FormatError
[   14s] 
[   14s] During handling of the above exception, another exception occurred:
[   14s] 
[   14s]     def test_ruff_offset():
[   14s]         code = 'print(x)\n'
[   14s]         example = CodeExample.create(code)
[   14s] >       with pytest.raises(FormatError, match='testing.md:1:7: F821 Undefined name'):
[   14s] E       AssertionError: Regex pattern did not match.
[   14s] E        Regex: 'testing.md:1:7: F821 Undefined name'
[   14s] E        Input: 'ruff failed:\n  F821 Undefined name `x`\n   --> -:1:7\n    |\n  1 | print(x)\n    |       ^\n    |\n\n  Found 1 error.\n'
[   14s] 
[   14s] tests/test_lint.py:27: AssertionError
[   14s] _______________________________ test_ruff_error ________________________________
[   14s] 
[   14s] pytester = <Pytester PosixPath('/tmp/pytest-of-abuild/pytest-0/test_ruff_error0')>
[   14s] 
[   14s]     def test_ruff_error(pytester: pytest.Pytester):
[   14s]         pytester.makefile(
[   14s]             '.md',
[   14s]             my_file='```py\nimport sys\nprint(missing)\n```',
[   14s]         )
[   14s]         # language=Python
[   14s]         pytester.makepyfile(
[   14s]             """
[   14s]     from pytest_examples import find_examples, CodeExample, EvalExample
[   14s]     import pytest
[   14s]     
[   14s]     @pytest.mark.parametrize('example', find_examples('.'), ids=str)
[   14s]     def test_find_run_examples(example: CodeExample, eval_example: EvalExample):
[   14s]         eval_example.lint_ruff(example)
[   14s]     """
[   14s]         )
[   14s]     
[   14s]         result = pytester.runpytest('-p', 'no:pretty', '-v')
[   14s]         result.assert_outcomes(failed=1)
[   14s]     
[   14s]         output = '\n'.join(result.outlines)
[   14s]         output = re.sub(r'(=|_){3,}', r'\1\1\1', output)
[   14s]         for phrase in [
[   14s]             '=== FAILURES ===\n',
[   14s]             '___ test_find_run_examples[my_file.md:1-4] ___\n',
[   14s]             'ruff failed:\n',
[   14s]             '  my_file.md:2:8: F401 [*] `sys` imported but unused\n',
[   14s]             '  my_file.md:3:7: F821 Undefined name `missing`\n',
[   14s]             '  Found 2 errors.\n',
[   14s]             '  [*] 1 fixable with the `--fix` option.\n',
[   14s]             '=== short test summary info ===\n',
[   14s]         ]:
[   14s] >           assert phrase in output
[   14s] E           AssertionError: assert '  my_file.md:2:8: F401 [*] `sys` imported but unused\n' in '=== test session starts ===\nplatform linux -- Python 3.11.13, pytest-8.4.1, pluggy-1.6.0 -- /usr/bin/python3.11\ncac... info ===\nFAILED test_ruff_error.py::test_find_run_examples[my_file.md:1-4] - Failed: r...\n=== 1 failed in 0.01s ==='
[   14s] 
[   14s] /home/abuild/rpmbuild/BUILD/python-pytest-examples-0.0.18-build/pytest_examples-0.0.18/tests/test_run_examples.py:120: AssertionError
[   14s] ----------------------------- Captured stdout call -----------------------------
[   14s] ============================= test session starts ==============================
[   14s] platform linux -- Python 3.11.13, pytest-8.4.1, pluggy-1.6.0 -- /usr/bin/python3.11
[   14s] cachedir: .pytest_cache
[   14s] rootdir: /tmp/pytest-of-abuild/pytest-0/test_ruff_error0
[   14s] plugins: examples-0.0.18
[   14s] collecting ... collected 1 item
[   14s] 
[   14s] test_ruff_error.py::test_find_run_examples[my_file.md:1-4] FAILED        [100%]
[   14s] 
[   14s] =================================== FAILURES ===================================
[   14s] ____________________ test_find_run_examples[my_file.md:1-4] ____________________
[   14s] ruff failed:
[   14s]   F401 [*] `sys` imported but unused
[   14s]    --> -:1:8
[   14s]     |
[   14s]   1 | import sys
[   14s]     |        ^^^
[   14s]   2 | print(missing)
[   14s]     |
[   14s]   help: Remove unused import: `sys`
[   14s] 
[   14s]   F821 Undefined name `missing`
[   14s]    --> -:2:7
[   14s]     |
[   14s]   1 | import sys
[   14s]   2 | print(missing)
[   14s]     |       ^^^^^^^
[   14s]     |
[   14s] 
[   14s]   Found 2 errors.
[   14s]   [*] 1 fixable with the `--fix` option.
[   14s] 
[   14s] =========================== short test summary info ============================
[   14s] FAILED test_ruff_error.py::test_find_run_examples[my_file.md:1-4] - Failed: r...
[   14s] ============================== 1 failed in 0.01s ===============================
[   14s] =========================== short test summary info ============================
[   14s] FAILED tests/test_lint.py::test_ruff_offset - AssertionError: Regex pattern d...
[   14s] FAILED tests/test_run_examples.py::test_ruff_error - AssertionError: assert '...
[   14s] ================== 2 failed, 70 passed, 1 deselected in 1.08s ==================
```

### Version

_No response_

---

_Label `question` added by @MeggyCal on 2025-08-18 11:34_

---

_Comment by @ntBre on 2025-08-18 12:38_

Thanks for the report and the kind words!

Yes, this was intentional, but it looks like I mistakenly tagged the PR that made the change (https://github.com/astral-sh/ruff/pull/19415) as `internal`, so it was omitted from the release notes. Sorry about that.

The range of the error should still be printed, but not on the same line as the error message, as expected by the test assertions. For example:

```
F401 [*] `sys` imported but unused
 --> -:1:8
```

instead of ``-:1:8: F401 [*] `sys` imported but unused``.

That might make the regex a bit tricky, so another option would be to use the `concise` output format, which should still match the old one, as long as there aren't any additional assertions on the code snippet after the header.

---

_Referenced in [astral-sh/ruff#19968](../../astral-sh/ruff/pulls/19968.md) on 2025-08-18 13:06_

---

_Comment by @MeggyCal on 2025-08-18 14:12_

Tagging @samuelcolvin here so he is aware of the issue. My hotfix would be just to delete the location pointers, but that is a bit dirty.

---

_Comment by @samuelcolvin on 2025-08-18 15:40_

Thanks @MeggyCal for letting me know.

Pr welcome to either fix pytest-examples and bump ruff, or pin ruff.

---

_Comment by @ntBre on 2025-08-18 16:32_

@samuelcolvin I'm happy to open a PR! Would you prefer one of those options over passing `--output-format=concise`? That would avoid needing to bump or pin ruff since the concise output hasn't changed and still matches the old regexes, but it looks like the full output is also user-facing, so I'm not sure if that's actually preferable for you.

---

_Comment by @samuelcolvin on 2025-08-18 16:47_

I think increasing the minimum ruff version is fine.

You'll need a multiline regex, but that would be fine.

---

_Referenced in [pydantic/pytest-examples#65](../../pydantic/pytest-examples/pulls/65.md) on 2025-08-18 17:42_

---

_Referenced in [astral-sh/ruff#19983](../../astral-sh/ruff/issues/19983.md) on 2025-08-19 13:21_

---

_Comment by @MichaReiser on 2025-09-17 07:18_

I'll mark this as closed on the ruff side. 

---

_Closed by @MichaReiser on 2025-09-17 07:18_

---
