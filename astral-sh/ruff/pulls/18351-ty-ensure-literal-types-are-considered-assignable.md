```yaml
number: 18351
title: "[ty] Ensure `Literal` types are considered assignable to anything their `Instance` supertypes are assignable to"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: literal-assignability-with-nominal-instance
created_at: 2025-05-28T10:56:47Z
updated_at: 2025-06-28T23:12:39Z
url: https://github.com/astral-sh/ruff/pull/18351
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Ensure `Literal` types are considered assignable to anything their `Instance` supertypes are assignable to

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/531

## Test Plan

Update `is_assignable_to.md`


---

_Review requested from @carljm by @MatthewMckee4 on 2025-05-28 10:56_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-05-28 10:56_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-05-28 10:56_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-05-28 10:56_

---

_Comment by @MatthewMckee4 on 2025-05-28 10:59_

I think there's room for more tests here but I'm not exactly sure what tests to add

---

_Renamed from "Literal assignability with nominal instance" to "[ty] Literal assignability with nominal instance" by @MatthewMckee4 on 2025-05-28 10:59_

---

_Label `ty` added by @MichaReiser on 2025-05-28 11:05_

---

_Comment by @AlexWaygood on 2025-05-28 11:11_

This looks correct to me but mypy_primer crashed on this branch when checking `mypy-protobuf`: https://github.com/astral-sh/ruff/actions/runs/15298365669/job/43032721889?pr=18351. I reran the job and it crashed again with the same assertion (which is https://github.com/salsa-rs/salsa/issues/831), this time when checking `zulip`: https://github.com/astral-sh/ruff/actions/runs/15298365669/job/43033038495?pr=18351. My understanding was that we'd usually only hit that Salsa bug if we introduced deeply nested cycles, but I don't see why this PR would introduce any more cycles. Curious if @MichaReiser has any ideas about what might be going on here!

---

_Comment by @MatthewMckee4 on 2025-05-28 11:12_

thanks for looking into that

---

_Comment by @MichaReiser on 2025-05-28 11:42_

Nothing obvious. You'd have to look into where `is_assignable_to` now runs more queries. There's also a chance that you were just unlucky and re-running helps :)

---

_Comment by @github-actions[bot] on 2025-05-28 11:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[no-matching-overload] src/trio/_tests/test_subprocess.py:506:19: No overload of function `open_process` matches arguments
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/subprocesses.py:15:70: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/subprocesses.py:22:60: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/subprocesses.py:23:70: Unused blanket `type: ignore` directive
- error[no-matching-overload] src/trio/_tests/type_tests/subprocesses.py:9:11: No overload of function `run_process` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/subprocesses.py:10:11: No overload of function `open_process` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/subprocesses.py:14:11: No overload of function `run_process` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/subprocesses.py:18:15: No overload of function `run_process` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/subprocesses.py:19:15: No overload of function `open_process` matches arguments
- Found 1090 diagnostics
+ Found 1087 diagnostics

rich (https://github.com/Textualize/rich)
- error[invalid-argument-type] tests/test_measure.py:24:58: Argument to function `measure_renderables` is incorrect: Expected `Sequence[Unknown]`, found `Literal[""]`
- error[invalid-argument-type] tests/test_measure.py:26:51: Argument to function `measure_renderables` is incorrect: Expected `Sequence[Unknown]`, found `Literal["hello"]`
- Found 393 diagnostics
+ Found 391 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] testing/acceptance_test.py:217:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *--version*\n            *warning*conftest.py*\n        "]`
- error[invalid-argument-type] testing/acceptance_test.py:482:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *unrecognized*\n        "]`
- error[invalid-argument-type] testing/acceptance_test.py:1061:13: Argument to bound method `fnmatch_lines_random` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *durations*\n            5.00s call *test_1*\n            2.00s setup *test_1*\n        "]`
- error[invalid-argument-type] testing/acceptance_test.py:1536:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*1 passed*"]`
- error[invalid-argument-type] testing/logging/test_reporting.py:1387:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["INFO     test:test_log_disabling_works_with_log_cli.py:6 Visible text!"]`
- error[invalid-argument-type] testing/python/collect.py:250:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*cannot collect test class 'TestCase' because it has a __new__ constructor*"]`
- error[invalid-argument-type] testing/python/fixtures.py:140:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *def test_func(some)*\n            *fixture*some*not found*\n            *xyzsomething*\n            "]`
- error[invalid-argument-type] testing/python/fixtures.py:1092:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *fixtures defined*conftest*\n            *arg1*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:1329:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*Fixture 'badscope' from test_invalid_scope.py got an unexpected scope value 'functions'"]`
- error[invalid-argument-type] testing/python/fixtures.py:1405:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *pytest.fixture()*\n            *def call_fail(fail)*\n            *pytest.fixture()*\n            *def fail*\n            *fixture*'missing'*not found*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:1467:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*PytestWarning: usefixtures() in test_empty_usefixtures_marker.py::test_one without arguments has no effect"]`
- error[invalid-argument-type] testing/python/fixtures.py:1503:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *usefixtures(fixturename1*mark tests*fixtures*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:2070:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *1 passed*1 error*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:2714:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["Fixture 'fixture' from test_dynamic_scope_bad_return.py got an unexpected scope value 'wrong-scope'"]`
- error[invalid-argument-type] testing/python/fixtures.py:2842:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            test_mod1.py::test_func[s1] PASSED\n            test_mod2.py::test_func2[s1] PASSED\n            test_mod2.py::test_func3[s1-m1] PASSED\n            test_mod2.py::test_func3b[s1-m1] PASSED\n            test_mod2.py::test_func3[s1-m2] PASSED\n            test_mod2.py::test_func3b[s1-m2] PASSED\n            test_mod1.py::test_func[s2] PASSED\n            test_mod2.py::test_func2[s2] PASSED\n            test_mod2.py::test_func3[s2-m1] PASSED\n            test_mod2.py::test_func3b[s2-m1] PASSED\n            test_mod2.py::test_func4[m1] PASSED\n            test_mod2.py::test_func3[s2-m2] PASSED\n            test_mod2.py::test_func3b[s2-m2] PASSED\n            test_mod2.py::test_func4[m2] PASSED\n            test_mod1.py::test_func1[m1] PASSED\n            test_mod1.py::test_func1[m2] PASSED\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:2899:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            test_dynamic_parametrized_ordering.py::test[flavor1-vxlan] PASSED\n            test_dynamic_parametrized_ordering.py::test2[flavor1-vxlan] PASSED\n            test_dynamic_parametrized_ordering.py::test[flavor1-vlan] PASSED\n            test_dynamic_parametrized_ordering.py::test2[flavor1-vlan] PASSED\n            test_dynamic_parametrized_ordering.py::test[flavor2-vlan] PASSED\n            test_dynamic_parametrized_ordering.py::test2[flavor2-vlan] PASSED\n            test_dynamic_parametrized_ordering.py::test[flavor2-vxlan] PASSED\n            test_dynamic_parametrized_ordering.py::test2[flavor2-vxlan] PASSED\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:2955:13: Argument to bound method `re_match_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            test_class_ordering.py::TestClass2::test_1\\[a-1\\] PASSED\n            test_class_ordering.py::TestClass2::test_1\\[a-2\\] PASSED\n            test_class_ordering.py::TestClass2::test_2\\[a-1\\] PASSED\n            test_class_ordering.py::TestClass2::test_2\\[a-2\\] PASSED\n            test_class_ordering.py::TestClass2::test_1\\[b-1\\] PASSED\n            test_class_ordering.py::TestClass2::test_1\\[b-2\\] PASSED\n            test_class_ordering.py::TestClass2::test_2\\[b-1\\] PASSED\n            test_class_ordering.py::TestClass2::test_2\\[b-2\\] PASSED\n            test_class_ordering.py::TestClass::test_3\\[a-1\\] PASSED\n            test_class_ordering.py::TestClass::test_3\\[a-2\\] PASSED\n            test_class_ordering.py::TestClass::test_3\\[b-1\\] PASSED\n            test_class_ordering.py::TestClass::test_3\\[b-2\\] PASSED\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3084:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *3 passed*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3522:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *ERROR*teardown*test_1*\n            *KeyError*\n            *ERROR*teardown*test_2*\n            *KeyError*\n            *3 pass*2 errors*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3616:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *tmp_path -- *\n            *fixtures defined from*\n            *arg1 -- test_show_fixtures_testmodule.py:6*\n            *hello world*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3644:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *tmp_path*\n            *fixtures defined from*conftest*\n            *arg1*\n            *hello world*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3802:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            * fixtures defined from test_a *\n            fix_a -- test_a.py:4\n                Fixture A\n\n            * fixtures defined from test_b *\n            fix_b -- test_b.py:4\n                Fixture B\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3842:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            * fixtures defined from conftest *\n            arg1 -- conftest.py:3\n                Hello World in conftest.py\n\n            * fixtures defined from test_show_fixtures_with_same_name *\n            arg1 -- test_show_fixtures_with_same_name.py:3\n                Hi from test module\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3882:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *setup*\n            *test1 1*\n            *teardown*\n            *setup*\n            *test2 1*\n            *teardown*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3909:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *setup*\n            *test1 1*\n            *test2 1*\n            *teardown*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3931:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *pytest.fail*setup*\n            *1 error*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3951:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *pytest.fail*teardown*\n            *1 passed*1 error*\n        "]`
- error[invalid-argument-type] testing/python/fixtures.py:3971:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *fixture function*\n            *test_yields*:2*\n        "]`
- error[invalid-argument-type] testing/python/integration.py:254:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *fix count 0*\n            *fix count 1*\n        "]`
- error[invalid-argument-type] testing/python/integration.py:260:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *2 passed*\n        "]`
- error[invalid-argument-type] testing/python/metafunc.py:1111:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *test_1*1*\n            *test_2*1*\n            *test_3*1*\n            *test_1*2*\n            *test_2*2*\n            *test_3*2*\n            *6 passed*\n        "]`
- error[invalid-argument-type] testing/python/metafunc.py:1339:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *test_function*1-b0*\n            *test_function*1.3-b1*\n        "]`
- error[invalid-argument-type] testing/test_assertion.py:1544:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *E*assert 1 == 2*\n            *1 failed*\n    "]`
- error[invalid-argument-type] testing/test_assertion.py:1860:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *def test_hello*\n        *assert 0, (x,y)*\n        *AssertionError: (1, 2)*\n    "]`
- error[invalid-argument-type] testing/test_assertion.py:1878:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *assert 'asdf' == 'asdf\\n'\n        *  - asdf\n        *  ?     -\n        *  + asdf\n    "]`
- error[invalid-argument-type] testing/test_assertrewrite.py:2042:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*Assertion Passed: a+b == c+d (1 + 2) == (3 + 0) at line 7*"]`
- error[invalid-argument-type] testing/test_assertrewrite.py:2054:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*Assertion Passed: f() 1"]`
- error[invalid-argument-type] testing/test_capture.py:815:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *def test_func*\n        *assert 0*\n        *Captured*\n        *1 failed*\n    "]`
- error[invalid-argument-type] testing/test_capture.py:1249:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *test_x*\n            *assert 0*\n            *Captured stdout*\n        "]`
- error[invalid-argument-type] testing/test_capture.py:1404:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *test_capture_again*\n        *assert 0*\n        *stdout*\n        *hello*\n    "]`
- error[invalid-argument-type] testing/test_capture.py:1443:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        suspend, captured*hello1*\n        suspend2, captured*WARNING:root:hello3*\n    "]`
- error[invalid-argument-type] testing/test_capture.py:1449:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        WARNING:root:hello2\n    "]`
- error[invalid-argument-type] testing/test_collection.py:1377:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["INTERNALERROR* Exception: pytest_sessionstart hook successfully run"]`
- error[invalid-argument-type] testing/test_collection.py:1726:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*1 passed in*"]`
- error[invalid-argument-type] testing/test_config.py:203:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ERROR: *pytest.ini:1: no section header defined"]`
- error[invalid-argument-type] testing/test_config.py:213:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ERROR: *pyproject.toml: Invalid statement*"]`
- error[invalid-argument-type] testing/test_config.py:337:41: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `LiteralString`
- error[invalid-argument-type] testing/test_config.py:535:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* no tests ran in *"]`
- error[invalid-argument-type] testing/test_conftest.py:528:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *--hello-world*\n    "]`
- error[invalid-argument-type] testing/test_helpconfig.py:37:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n          -m MARKEXPR           Only run tests matching given mark expression. For\n                                example: -m 'mark1 and not mark2'.\n        Reporting:\n          --durations=N *\n          -V, --version         Display pytest version and information about plugins.\n                                When given twice, also display information about\n                                plugins.\n        *setup.cfg*\n        *minversion*\n        *to see*markers*pytest --markers*\n        *to see*fixtures*pytest --fixtures*\n    "]`
- error[invalid-argument-type] testing/test_main.py:106:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*basetemp must not be*"]`
- error[invalid-argument-type] testing/test_main.py:317:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* 1 passed *"]`
- error[invalid-argument-type] testing/test_monkeypatch.py:315:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *1 passed*\n    "]`
- error[invalid-argument-type] testing/test_monkeypatch.py:344:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *import error in package.a: No module named 'doesnotexist'*\n    "]`
- error[invalid-argument-type] testing/test_pathlib.py:947:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* 1 passed *"]`
- error[invalid-argument-type] testing/test_pathlib.py:1578:32: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*No module named 'com.company.calc*"]`
- error[invalid-argument-type] testing/test_pytester.py:491:22: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal[""]`
- error[invalid-argument-type] testing/test_session.py:453:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*session.shouldfail cannot be unset*"]`
- error[invalid-argument-type] testing/test_session.py:483:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*session.shouldstop cannot be unset*"]`
- error[invalid-argument-type] testing/test_skipping.py:516:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *def test_this*\n            *1 fail*\n        "]`
- error[invalid-argument-type] testing/test_skipping.py:1239:9: Argument to bound method `fnmatch_lines_random` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *SKIP*abc*\n        *SKIP*condition: True*\n        *2 skipped*\n    "]`
- error[invalid-argument-type] testing/test_skipping.py:1262:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *1 passed*1 skipped*\n        "]`
- error[invalid-argument-type] testing/test_skipping.py:1278:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *1 error*\n        "]`
- error[invalid-argument-type] testing/test_skipping.py:1294:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n            *XFAIL*True123*\n            *1 xfail*\n        "]`
- error[invalid-argument-type] testing/test_skipping.py:1500:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*_pytest.outcomes.Exit: foo*"]`
- error[invalid-argument-type] testing/test_stepwise.py:210:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*error during collection*"]`
- error[invalid-argument-type] testing/test_stepwise.py:292:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*Implicitly enables --stepwise."]`
- error[invalid-argument-type] testing/test_terminal.py:586:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 3 tests collected in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:589:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 1 test collected in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:592:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 2/3 tests collected (1 deselected) in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:595:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 1/3 tests collected (2 deselected) in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:598:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== no tests collected (3 deselected) in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:602:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== no tests collected in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:610:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 3 tests collected, 1 error in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:613:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*== 2/3 tests collected (1 deselected), 1 error in * ==*"]`
- error[invalid-argument-type] testing/test_terminal.py:1745:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *==== hello ====*\n        world\n        exitstatus: 5\n    "]`
- error[invalid-argument-type] testing/test_terminal.py:2812:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ERROR: PYTEST_THEME environment variable has an invalid value: 'invalid'. Hint: See available pygments styles with `pygmentize -L styles`."]`
- error[invalid-argument-type] testing/test_terminal.py:2828:13: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ERROR: PYTEST_THEME_MODE environment variable has an invalid value: 'invalid'. The allowed values are 'dark' (default) and 'light'."]`
- error[invalid-argument-type] testing/test_threadexception.py:220:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ValueError: Oops"]`
- error[invalid-argument-type] testing/test_tmpdir.py:257:37: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["*ValueError*"]`
- error[invalid-argument-type] testing/test_unittest.py:44:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *MyTestCaseWithRunTest::runTest*\n        *MyTestCaseWithoutRunTest::test_something*\n        *2 passed*\n    "]`
- error[invalid-argument-type] testing/test_unittest.py:992:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *SKIP*[1]*test_foo.py*skipping due to reasons*\n        *1 skipped*\n    "]`
- error[invalid-argument-type] testing/test_unittest.py:1012:9: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["\n        *SKIP*[1]*skipping due to reasons*\n        *1 skipped*\n    "]`
- error[invalid-argument-type] testing/test_unittest.py:1240:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* 2 passed in *"]`
- error[invalid-argument-type] testing/test_unittest.py:1278:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* 1 skipped in *"]`
- error[invalid-argument-type] testing/test_unittest.py:1313:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["* 1 skipped in *"]`
- error[invalid-argument-type] testing/test_unraisableexception.py:266:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ValueError: del is broken"]`
- error[invalid-argument-type] testing/test_unraisableexception.py:297:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ValueError: del is broken"]`
- error[invalid-argument-type] testing/test_unraisableexception.py:328:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["RuntimeWarning: coroutine 'my_task' was never awaited"]`
- error[invalid-argument-type] testing/test_unraisableexception.py:364:33: Argument to bound method `fnmatch_lines` is incorrect: Expected `Sequence[Unknown]`, found `Literal["ValueError: del is broken"]`
- Found 886 diagnostics
+ Found 796 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:311:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["cancel() cannot be used with a prepared two-phase transaction"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:529:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:571:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["Explicit commit() forbidden within a Transaction context. (Transaction will be automatically committed on successful exit from context.)"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:577:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["commit() cannot be used during a two-phase transaction"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:591:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["Explicit rollback() forbidden within a Transaction context. (Either raise Rollback() or allow an exception to propagate out of the context.)"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:597:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["rollback() cannot be used during a two-phase transaction"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:641:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["can't use two-phase transactions in autocommit mode"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:650:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["'tpc_prepare()' must be called inside a two-phase transaction"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_base.py:654:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["'tpc_prepare()' cannot be used during a prepared two-phase transaction"]`
- error[invalid-argument-type] psycopg/psycopg/_connection_info.py:53:31: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["couldn't find the connection port"]`
- error[invalid-argument-type] psycopg/psycopg/_copy_base.py:264:21: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["binary copy doesn't start with the expected signature"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:358:21: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the operation in stream() didn't produce a result"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:373:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the cursor is closed"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:389:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["COPY cannot be used in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:403:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["COPY cannot be mixed with other operations"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:468:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["got no result from the query"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:482:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["pipeline aborted"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:485:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["COPY cannot be used with this method; use copy() instead"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:574:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the cursor is closed"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:577:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["no result available"]`
- error[invalid-argument-type] psycopg/psycopg/_cursor_base.py:584:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["pipeline aborted"]`
- error[invalid-argument-type] psycopg/psycopg/_pipeline.py:172:41: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["pipeline aborted"]`
- error[invalid-argument-type] psycopg/psycopg/_py_transformer.py:273:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["None dumper not found"]`
- error[invalid-argument-type] psycopg/psycopg/_py_transformer.py:298:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["result not set"]`
- error[invalid-argument-type] psycopg/psycopg/_py_transformer.py:317:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["result not set"]`
- error[invalid-argument-type] psycopg/psycopg/_py_transformer.py:351:40: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["unknown oid loader not found"]`
- error[invalid-argument-type] psycopg/psycopg/_queries.py:392:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["incomplete placeholder: '%'; if you want to use '%' as an operator you can double it up, i.e. use '%%'"]`
- error[invalid-argument-type] psycopg/psycopg/_queries.py:409:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["positional and named placeholders cannot be mixed"]`
- error[invalid-argument-type] psycopg/psycopg/_server_cursor.py:99:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["server-side cursors not supported in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/_server_cursor.py:114:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["executemany not supported on server-side cursors"]`
- error[invalid-argument-type] psycopg/psycopg/_server_cursor_async.py:99:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["server-side cursors not supported in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/_server_cursor_async.py:114:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["executemany not supported on server-side cursors"]`
- error[invalid-argument-type] psycopg/psycopg/_server_cursor_base.py:153:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the cursor is closed"]`
- error[invalid-argument-type] psycopg/psycopg/_struct.py:49:9: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["cannot dump Float4: Python affected by bug #304. Note that the psycopg-c and psycopg-binary packages are not affected by this issue. See https://github.com/psycopg/psycopg/issues/304"]`
- error[invalid-argument-type] psycopg/psycopg/client_cursor.py:62:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["client-side cursors don't support binary results"]`
- error[invalid-argument-type] psycopg/psycopg/connection_async.py:104:25: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["Psycopg cannot use the 'ProactorEventLoop' to run in async mode. Please use a compatible event loop, for instance by setting 'asyncio.set_event_loop_policy(WindowsSelectorEventLoopPolicy())'"]`
- error[invalid-argument-type] psycopg/psycopg/crdb/connection.py:52:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["CockroachDB doesn't support prepared statements"]`
- error[invalid-argument-type] psycopg/psycopg/crdb/connection.py:90:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["'crdb_version' parameter status not set"]`
- error[invalid-argument-type] psycopg/psycopg/cursor.py:146:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["stream() cannot be used in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/cursor_async.py:146:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["stream() cannot be used in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/errors.py:77:32: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/generators.py:92:47: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["connection timeout expired"]`
- error[invalid-argument-type] psycopg/psycopg/generators.py:117:41: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["cancellation timeout expired"]`
- error[invalid-argument-type] psycopg/psycopg/generators.py:274:47: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["COPY cannot be used in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/generators.py:326:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["you cannot mix COPY with other operations"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:164:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["couldn't reset connection"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:246:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is lost"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:600:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["flushing failed: the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:608:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["setting single row mode failed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:612:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["setting chunked rows mode failed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:621:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["couldn't create cancelConn object"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:631:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["couldn't create cancel object"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:681:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["currently only supported on Linux"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:753:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["failed to enter pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:772:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["connection not in pipeline mode"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:774:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["failed to sync pipeline"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:791:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:800:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:808:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:813:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:938:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["PQsetResultAttrs failed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:989:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["cancel connection not opened"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:1017:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the cancel connection is closed"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:1126:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["escape_literal failed: no connection provided"]`
- error[invalid-argument-type] psycopg/psycopg/pq/pq_ctypes.py:1143:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["escape_identifier failed: no connection provided"]`
- error[invalid-argument-type] psycopg/psycopg/rows.py:216:38: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["at least one column expected"]`
- error[invalid-argument-type] psycopg/psycopg/rows.py:234:28: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["the cursor doesn't have a result"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:91:31: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["cannot dump a recursive list"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:260:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["lists cannot contain empty lists"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:269:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["nested lists have inconsistent lengths"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:285:43: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["nested lists have inconsistent depths"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:396:31: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["malformed array: no '=' after dimension information"]`
- error[invalid-argument-type] psycopg/psycopg/types/array.py:409:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["malformed array: unexpected '}'"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:283:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["date too small (before year 1)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:285:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["date too large (after year 10K)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:474:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["timestamp too small (before year 1)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:476:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["timestamp too large (after year 10K)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:589:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["timestamp too small (before year 1)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:591:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["timestamp too large (after year 10K)"]`
- error[invalid-argument-type] psycopg/psycopg/types/datetime.py:700:26: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["timestamp too small (before year 1): {s!r}"]`
- error[invalid-argument-type] psycopg/psycopg/types/hstore.py:66:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["hstore keys can only be strings"]`
- error[invalid-argument-type] psycopg/psycopg/types/hstore.py:74:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["hstore keys can only be strings"]`
- error[invalid-argument-type] psycopg/psycopg/types/hstore.py:132:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["error parsing hstore pair at char 0"]`
- error[invalid-argument-type] psycopg/psycopg/types/json.py:212:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["unknown jsonb binary format: {data[0]}"]`
- error[invalid-argument-type] psycopg/psycopg/types/multirange.py:52:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["multirange types are only available from PostgreSQL 14"]`
- error[invalid-argument-type] psycopg/psycopg/types/multirange.py:325:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["malformed multirange: data after closing brace"]`
- error[invalid-argument-type] psycopg/psycopg/types/multirange.py:333:31: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["malformed multirange: separator missing"]`
- error[invalid-argument-type] psycopg/psycopg/types/multirange.py:357:31: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["unexpected trailing data in multirange"]`
- error[invalid-argument-type] psycopg/psycopg/types/range.py:440:27: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["trying to dump a range element without information"]`
- error[invalid-argument-type] psycopg/psycopg/types/string.py:52:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["PostgreSQL text fields cannot contain NUL (0x00) bytes"]`
- error[invalid-argument-type] psycopg_pool/psycopg_pool/base.py:137:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `Literal["pool has already been opened/closed and cannot be reused"]`
- Found 1112 diagnostics
+ Found 1021 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1452:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1452:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1452:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1457:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1457:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1457:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1465:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1465:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1465:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1473:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1473:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1478:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1478:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1484:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1489:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1489:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1494:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1494:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1494:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1499:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1499:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1499:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1507:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1507:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1507:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1512:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1512:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1517:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1522:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1522:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1527:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1527:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1527:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1532:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1532:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1532:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1532:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1537:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1537:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1537:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["f"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1542:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["x"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1542:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["y"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1547:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1552:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1557:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1557:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1557:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1557:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1563:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1563:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1563:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1563:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1563:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1568:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1568:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1568:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1568:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1568:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1573:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1573:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1573:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1573:44: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1573:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["e"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1590:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1652:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1652:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1677:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1680:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2018-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1683:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-02-05"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1683:48: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2018-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1686:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2021-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1689:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2021-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1730:88: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1730:107: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1732:88: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1732:107: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1732:183: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1732:188: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1734:88: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1734:107: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1734:183: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1737:92: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1737:111: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1740:77: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1740:96: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1740:172: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1746:77: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2022-01-03"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1746:96: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["2023-04-02"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1765:61: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1765:66: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1768:61: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1768:66: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1784:101: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1787:102: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["d"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1803:67: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1820:30: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1820:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1820:40: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1823:30: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1823:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1823:40: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1826:30: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1826:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1826:40: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1836:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["c"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1836:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1836:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1841:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["a"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1841:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `Literal["b"]`
- error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:1841:39: Argument to bound method `__init__` is incorrect: Expected ...*[Comment body truncated]*

---

_Comment by @AlexWaygood on 2025-05-28 12:02_

It succeeded on the third attempt, so I guess we were just unlucky here!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1660 on 2025-05-28 12:57_

I think we can be a bit more general here -- this will make the logic more robust if we add literal-instance-fallbacks to any other types in the future

```suggestion
            _ if self.literal_fallback_instance(db))
                .is_some_and(|instance| instance.is_assignable_to(db, target) =>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 12:58_

I think we also need to add the same logic for `LiteralString`:

```suggestion
            (Type::LiteralString, _) if KnownClass::Str.to_instance(db).is_assignable_to(db, target) => true,

            (Type::ClassLiteral(class_literal), Type::Callable(_)) => {
```

---

_@AlexWaygood reviewed on 2025-05-28 12:59_

thank you!

---

_@AlexWaygood reviewed on 2025-05-28 13:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:00_

(need to add a test for this as well)

---

_@MatthewMckee4 reviewed on 2025-05-28 13:02_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:02_

There is a `Type::LiteralString` arm in `literal_fallback_instance` so i think this is covered

---

_@MatthewMckee4 reviewed on 2025-05-28 13:04_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:04_

i added a test for it

---

_@AlexWaygood reviewed on 2025-05-28 13:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:04_

> There is a `Type::LiteralString` arm in `literal_fallback_instance` so i think this is covered

Oh, I missed that! Thanks

---

_@MatthewMckee4 reviewed on 2025-05-28 13:05_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:05_

oh i see how this is different sorry

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1665 on 2025-05-28 13:06_

oh no i think i we do cover it

---

_@MatthewMckee4 reviewed on 2025-05-28 13:06_

---

_Comment by @AlexWaygood on 2025-05-28 15:24_

There's a bunch of weird primer hits in the diff that look like we no longer understand that `Collection` is assignable to `Iterable` in some situations... but I'm _really_ struggling to create a minimal repro for that with your branch (or to figure out why this PR might be having that effect) ☹️

---

_Comment by @carljm on 2025-05-28 18:45_

When I run primer locally, I always get 173 diagnostics on mkdocs (on main or on this PR branch), but the primer diff here is showing 207 on main and 210 on this PR branch. No idea yet why they are different.

---

_Comment by @carljm on 2025-05-28 18:46_

Going to try rebasing this on latest main and pushing again to see how that impacts primer results.

---

_Comment by @carljm on 2025-05-28 18:57_

Definitely seems like for whatever reason this PR causes us to hit the cycle assertion more often. Hopefully we're closing in on a fix for that issue, so let's just hold off on this for a moment.

---

_Closed by @AlexWaygood on 2025-06-01 15:27_

---

_Reopened by @AlexWaygood on 2025-06-01 15:27_

---

_@AlexWaygood reviewed on 2025-06-01 15:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:13 on 2025-06-01 15:32_

```suggestion
# TODO: This should not be an error:
# error: [invalid-assignment]
alice: Person = {"name": "Alice", "age": 30}
```

---

_@AlexWaygood approved on 2025-06-01 15:33_

Thank you, this is great. Now that https://github.com/astral-sh/ruff/commit/54f597658ce986b140598fa7db65b99335f90cdf has landed on `main`, the primer changes look fantastic, and it appears we no longer get any new primer crashes from this PR 

---

_Comment by @AlexWaygood on 2025-06-01 15:35_

I also checked the property tests locally on this branch, and all looks good.

---

_Renamed from "[ty] Literal assignability with nominal instance" to "[ty] Ensure `Literal` types are considered assignable to anything their `Instance` supertypes are assignable to" by @AlexWaygood on 2025-06-01 15:39_

---

_Merged by @AlexWaygood on 2025-06-01 15:39_

---

_Closed by @AlexWaygood on 2025-06-01 15:39_

---

_Label `bug` added by @dhruvmanila on 2025-06-02 11:14_

---

_Branch deleted on 2025-06-28 23:12_

---
