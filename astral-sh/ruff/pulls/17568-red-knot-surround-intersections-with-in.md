```yaml
number: 17568
title: "[red-knot] Surround intersections with `()` in potentially ambiguous contexts"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: union-intersection-display
created_at: 2025-04-23T00:44:40Z
updated_at: 2025-04-23T08:38:16Z
url: https://github.com/astral-sh/ruff/pull/17568
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Surround intersections with `()` in potentially ambiguous contexts

---

_Pull request opened by @MatthewMckee4 on 2025-04-23 00:44_

## Summary

Add parentheses to multi-element intersections, when displayed in a context that's otherwise potentially ambiguous.

## Test Plan

Update mdtest files


---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-23 00:44_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-23 00:44_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-23 00:44_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-23 00:44_

---

_Comment by @github-actions[bot] on 2025-04-23 00:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `int & tuple & ~float | tuple[int | float, int | float] & ~float | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/enhance/adjust.py:722:42: Argument to this function is incorrect: Expected `Sized`, found `int & ~float | Unknown & ~None & ~float | Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kornia/kornia/enhance/adjust.py:722:42: Argument to this function is incorrect: Expected `Sized`, found `(int & ~float) | (Unknown & ~None & ~float) | Unknown`

pip (https://github.com/pypa/pip)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pip/src/pip/_vendor/typing_extensions.py:3329:20: Operator `<` is not supported for types `int` and `_Sentinel`, in comparing `int` with `Unknown & ~AlwaysFalsy | _Sentinel & ~AlwaysFalsy | int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pip/src/pip/_vendor/typing_extensions.py:3329:20: Operator `<` is not supported for types `int` and `_Sentinel`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pip/src/pip/_vendor/typing_extensions.py:3346:46: Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `Unknown & ~AlwaysFalsy | _Sentinel & ~AlwaysFalsy | int | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pip/src/pip/_vendor/typing_extensions.py:3346:46: Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int | @Todo(map_with_boundness: intersections with negative contributions)`

nox (https://github.com/wntrblm/nox)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/nox/nox/registry.py:108:26: Attribute `__name__` on type `((...) -> Any) & ~None | Func` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/nox/nox/registry.py:108:26: Attribute `__name__` on type `(((...) -> Any) & ~None) | Func` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nox/nox/registry.py:111:9: Argument to this function is incorrect: Expected `(...) -> Any`, found `((...) -> Any) & ~None | Func`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nox/nox/registry.py:111:9: Argument to this function is incorrect: Expected `(...) -> Any`, found `(((...) -> Any) & ~None) | Func`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nox/nox/registry.py:111:9: Argument to this function is incorrect: Expected `(...) -> Any`, found `((...) -> Any) & ~None | Func`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nox/nox/registry.py:111:9: Argument to this function is incorrect: Expected `(...) -> Any`, found `(((...) -> Any) & ~None) | Func`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/nox/nox/registry.py:121:23: Attribute `__name__` on type `((...) -> Any) & ~None | Func` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/nox/nox/registry.py:121:23: Attribute `__name__` on type `(((...) -> Any) & ~None) | Func` is possibly unbound

sockeye (https://github.com/awslabs/sockeye)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/sockeye/sockeye/inference.py:350:50: Argument to this function is incorrect: Expected `Sized`, found `Unknown & ~list | None | @Todo(list comprehension type)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/sockeye/sockeye/inference.py:350:50: Argument to this function is incorrect: Expected `Sized`, found `(Unknown & ~list) | None | @Todo(list comprehension type)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/sockeye/sockeye/inference.py:352:34: Argument to this function is incorrect: Expected `Sized`, found `Unknown & ~list | None | @Todo(list comprehension type)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/sockeye/sockeye/inference.py:352:34: Argument to this function is incorrect: Expected `Sized`, found `(Unknown & ~list) | None | @Todo(list comprehension type)`

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/SinbadCogs/relays/helpers.py:106:28: Attribute `group` on type `Unknown | Unknown & ~AlwaysFalsy | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/SinbadCogs/relays/helpers.py:106:28: Attribute `group` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` is possibly unbound

pylint (https://github.com/pycqa/pylint)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pylint/pylint/checkers/variables.py:2535:13: Return type does not match returned value: Expected `bool`, found `@Todo(Support for `typing.TypeVar` instances in type expressions) & ~AlwaysTruthy | None | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/pylint/pylint/checkers/variables.py:2535:13: Return type does not match returned value: Expected `bool`, found `(@Todo(Support for `typing.TypeVar` instances in type expressions) & ~AlwaysTruthy) | None | @Todo(Support for `typing.TypeVar` instances in type expressions)`

dulwich (https://github.com/dulwich/dulwich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dulwich/dulwich/diff_tree.py:155:51: Argument to this function is incorrect: Expected `Tree`, found `Unknown & ~AlwaysTruthy & ~AlwaysFalsy | Unknown & ~AlwaysFalsy | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dulwich/dulwich/diff_tree.py:155:51: Argument to this function is incorrect: Expected `Tree`, found `(Unknown & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dulwich/dulwich/diff_tree.py:155:58: Argument to this function is incorrect: Expected `Tree`, found `Unknown & ~AlwaysTruthy & ~AlwaysFalsy | Unknown & ~AlwaysFalsy | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dulwich/dulwich/diff_tree.py:155:58: Argument to this function is incorrect: Expected `Tree`, found `(Unknown & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | None`

python-chess (https://github.com/niklasf/python-chess)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1670:16: Return type does not match returned value: Expected `BinaryIO`, found `Unknown & ~None | TextIOWrapper`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1670:16: Return type does not match returned value: Expected `BinaryIO`, found `(Unknown & ~None) | TextIOWrapper`

operator (https://github.com/canonical/operator)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/operator/ops/_private/harness.py:3892:77: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `@Todo(specialized non-generic class) & None | None | frozenset`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/operator/ops/_private/harness.py:3892:77: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `(@Todo(specialized non-generic class) & None) | None | frozenset`

pydantic (https://github.com/pydantic/pydantic)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/validators.py:326:26: Argument to this function is incorrect: Expected `bytes | None`, found `(Any & bytes & ~str) | (Any & bytearray & ~str) | UUID`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/validators.py:326:26: Argument to this function is incorrect: Expected `bytes | None`, found `Any & bytes & ~str | Any & bytearray & ~str | UUID`

urllib3 (https://github.com/urllib3/urllib3)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/urllib3/src/urllib3/util/ssl_.py:301:9: Object of type `int | @Todo(specialized non-generic class) & ~None` is not assignable to attribute `minimum_version` on type `Unknown | SSLContext`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/urllib3/src/urllib3/util/ssl_.py:301:9: Object of type `int | (@Todo(specialized non-generic class) & ~None)` is not assignable to attribute `minimum_version` on type `Unknown | SSLContext`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/urllib3/src/urllib3/util/ssl_.py:306:9: Object of type `int | @Todo(specialized non-generic class) & ~None` is not assignable to attribute `maximum_version` on type `Unknown | SSLContext`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/urllib3/src/urllib3/util/ssl_.py:306:9: Object of type `int | (@Todo(specialized non-generic class) & ~None)` is not assignable to attribute `maximum_version` on type `Unknown | SSLContext`

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/schema_salad/schema_salad/jsonld_context.py:210:16: Operator `in` is not supported for types `@Todo(specialized non-generic class)` and `int`, in comparing `@Todo(specialized non-generic class)` with `CommentedMap & MutableMapping | int & MutableMapping | float & MutableMapping | str & MutableMapping | CommentedSeq & MutableMapping`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/schema_salad/schema_salad/jsonld_context.py:210:16: Operator `in` is not supported for types `@Todo(specialized non-generic class)` and `int`, in comparing `@Todo(specialized non-generic class)` with `(CommentedMap & MutableMapping) | (int & MutableMapping) | (float & MutableMapping) | (str & MutableMapping) | (CommentedSeq & MutableMapping)`

websockets (https://github.com/aaugustin/websockets)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/websockets/src/websockets/legacy/client.py:459:13: Object of type `Unknown & ~None | Literal[WebSocketClientProtocol]` is not assignable to `((...) -> WebSocketClientProtocol) | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/websockets/src/websockets/legacy/client.py:459:13: Object of type `(Unknown & ~None) | Literal[WebSocketClientProtocol]` is not assignable to `((...) -> WebSocketClientProtocol) | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/websockets/src/websockets/legacy/server.py:1028:13: Object of type `Unknown & ~None | Literal[WebSocketServerProtocol]` is not assignable to `((...) -> WebSocketServerProtocol) | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/websockets/src/websockets/legacy/server.py:1028:13: Object of type `(Unknown & ~None) | Literal[WebSocketServerProtocol]` is not assignable to `((...) -> WebSocketServerProtocol) | None`

PyGithub (https://github.com/PyGithub/PyGithub)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/Milestone.py:200:41: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & date)` is possibly unbound
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:94:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `(Unknown & dict) | (Unknown & AdvisoryCredit & dict)`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:95:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `(Unknown & dict) | (Unknown & AdvisoryCredit & dict)`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:106:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `(Unknown & dict) | (Unknown & AdvisoryCredit & dict)`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:107:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `(Unknown & dict) | (Unknown & AdvisoryCredit & dict)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:94:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `Unknown & dict | Unknown & AdvisoryCredit & dict`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:95:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `Unknown & dict | Unknown & AdvisoryCredit & dict`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:106:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `Unknown & dict | Unknown & AdvisoryCredit & dict`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/PyGithub/github/AdvisoryCredit.py:107:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `Unknown & dict | Unknown & AdvisoryCredit & dict`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/NamedUser.py:444:39: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/Milestone.py:200:41: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType & Unknown | _NotSetType | @Todo(unknown type subscript) & date` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/NamedUser.py:444:39: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType & Unknown | _NotSetType | @Todo(unknown type subscript) & datetime` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/MainClass.py:542:39: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType & Unknown | _NotSetType | @Todo(unknown type subscript) & datetime` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/PyGithub/github/MainClass.py:542:39: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound

psycopg (https://github.com/psycopg/psycopg)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/sql.py:167:26: Argument to this function is incorrect: Expected `LiteralString`, found `SQL & str | LiteralString`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/sql.py:167:26: Argument to this function is incorrect: Expected `LiteralString`, found `(SQL & str) | LiteralString`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts.py:101:15: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str & ~AlwaysFalsy | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts.py:101:15: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `(str & ~AlwaysFalsy) | (ParamDef & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts_async.py:105:19: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `(str & ~AlwaysFalsy) | (ParamDef & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts_async.py:105:19: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str & ~AlwaysFalsy | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`

beartype (https://github.com/beartype/beartype)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/beartype/beartype/_cave/_cavemap.py:144:18: Operator `in` is not supported for types `type` and `str`, in comparing `type` with `(str & tuple & ~type & ~AlwaysFalsy) | (@Todo(Inference of subscript on special form) & tuple & ~type & ~AlwaysFalsy)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/beartype/beartype/_cave/_cavemap.py:144:18: Operator `in` is not supported for types `type` and `str`, in comparing `type` with `str & tuple & ~type & ~AlwaysFalsy | @Todo(Inference of subscript on special form) & tuple & ~type & ~AlwaysFalsy`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/proxy/mode_servers.py:214:52: Attribute `get_extra_info` on type `StreamWriter | (Unknown & ~None) | StreamReader | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/io/har.py:106:32: Attribute `encode` on type `Unknown & str | None` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/mitmproxy/examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: Expected `HTTPFlow`, found `@Todo(map_with_boundness: intersections with negative contributions) & HTTPFlow | None`
- error[lint:type-assertion-failure] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/proxy/layers/modes.py:82:17: Expected type `Never`, got `Unknown & Literal["http"] | Unknown & Literal["https"] | Unknown & Literal["http3"] | Unknown & Literal["tls"] | Unknown & Literal["dtls"] | Unknown & Literal["tcp"] | Unknown & Literal["udp"] | Unknown & Literal["dns"] | Unknown & Literal["quic"]` instead
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/proxy/mode_servers.py:214:52: Attribute `get_extra_info` on type `StreamWriter | Unknown & ~None | StreamReader | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/io/har.py:106:32: Attribute `encode` on type `(Unknown & str) | None` is possibly unbound
+ error[lint:type-assertion-failure] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/proxy/layers/modes.py:82:17: Expected type `Never`, got `(Unknown & Literal["http"]) | (Unknown & Literal["https"]) | (Unknown & Literal["http3"]) | (Unknown & Literal["tls"]) | (Unknown & Literal["dtls"]) | (Unknown & Literal["tcp"]) | (Unknown & Literal["udp"]) | (Unknown & Literal["dns"]) | (Unknown & Literal["quic"])` instead
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/mitmproxy/examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: Expected `HTTPFlow`, found `(@Todo(map_with_boundness: intersections with negative contributions) & HTTPFlow) | None`

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/cwltool/cwltool/main.py:452:9: Attribute `update` on type `Unknown & None | None | Unknown | MutableMapping` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/cwltool/cwltool/main.py:452:9: Attribute `update` on type `(Unknown & None) | None | Unknown | MutableMapping` is possibly unbound

scrapy (https://github.com/scrapy/scrapy)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/logformatter.py:103:19: Attribute `getErrorMessage` on type `Response | (Unknown & ~None)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:206:24: Attribute `callback` on type `Request | None | (Unknown & Request) | (Unknown & None)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:210:53: Attribute `cb_kwargs` on type `Request | None | (Unknown & Request) | (Unknown & None)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/logformatter.py:103:19: Attribute `getErrorMessage` on type `Response | Unknown & ~None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:206:24: Attribute `callback` on type `Request | None | Unknown & Request | Unknown & None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/core/scraper.py:210:53: Attribute `cb_kwargs` on type `Request | None | Unknown & Request | Unknown & None` is possibly unbound

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_make_custom_builds.py:333:9: Method `__getitem__` of type `@Todo(dict comprehension type) & ~None | DataclassOptions | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_make_custom_builds.py:333:9: Method `__getitem__` of type `(@Todo(dict comprehension type) & ~None) | DataclassOptions | None` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:928:24: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `((...) -> Any) & type[HydraConf] | DataClass_ & type[HydraConf] | @Todo(specialized non-generic class) & type[HydraConf] | ListConfig & type[HydraConf] | DictConfig & type[HydraConf]`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:928:24: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `(((...) -> Any) & type[HydraConf]) | (DataClass_ & type[HydraConf]) | (@Todo(specialized non-generic class) & type[HydraConf]) | (ListConfig & type[HydraConf]) | (DictConfig & type[HydraConf])`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `((...) -> Any) & type & ~type[HydraConf] | DataClass_ & type & ~type[HydraConf] | @Todo(specialized non-generic class) & type & ~type[HydraConf] | ListConfig & type & ~type[HydraConf] | DictConfig & type & ~type[HydraConf]`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf]) | (@Todo(specialized non-generic class) & type & ~type[HydraConf]) | (ListConfig & type & ~type[HydraConf]) | (DictConfig & type & ~type[HydraConf])`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `((...) -> Any) & ~type | DataClass_ & ~type | @Todo(specialized non-generic class) & ~type | ListConfig & ~type | DictConfig & ~type`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: Expected `DataClass_ | type[DataClass_] | ListConfig | DictConfig`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | (@Todo(specialized non-generic class) & ~type) | (ListConfig & ~type) | (DictConfig & ~type)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_implementations.py:1130:34: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `~None & ~type | Unknown & ~type`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/hydra-zen/src/hydra_zen/structured_configs/_implementations.py:1130:34: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `(~None & ~type) | (Unknown & ~type)`

werkzeug (https://github.com/pallets/werkzeug)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/routing/exceptions.py:102:50: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | (@Todo(specialized non-generic class) & None) | None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/routing/exceptions.py:102:50: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | @Todo(specialized non-generic class) & None | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/http.py:553:34: Argument to this function is incorrect: Expected `str`, found `@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/http.py:553:34: Argument to this function is incorrect: Expected `str`, found `(@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy) | None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `@Todo(specialized non-generic class) & str | @Todo(specialized non-generic class) & None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | ((str | None, /) -> int | None) & ~None | Unknown & ~None` with `Literal[0]`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/utils.py:512:9: Object of type `int | ((str | None, /) -> int | None) & ~None | Unknown & ~None` is not assignable to attribute `max_age` of type `int | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` with `Literal[0]`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/utils.py:512:9: Object of type `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `(@Todo(specialized non-generic class) & str) | (@Todo(specialized non-generic class) & None)`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:601:5: Attribute `add` on type `Unknown | @Todo(specialized non-generic class) & None | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:601:5: Attribute `add` on type `Unknown | (@Todo(specialized non-generic class) & None) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:603:5: Attribute `discard` on type `Unknown | @Todo(specialized non-generic class) & None | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:603:5: Attribute `discard` on type `Unknown | (@Todo(specialized non-generic class) & None) | None` is possibly unbound

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mongo-python-driver/bson/codec_options.py:390:45: Attribute `__origin__` on type `(@Todo(unsupported nested subscript in type[X]) & ~AlwaysFalsy) | Literal[dict]` is possibly unbound
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/synchronous/database.py:925:13: Object of type `(ClientSession & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy) | Unknown | Primary` is not assignable to `_ServerMode | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mongo-python-driver/bson/binary.py:476:39: Attribute `value` on type `BinaryVectorDtype | None | Unknown | @Todo(specialized non-generic class) & BinaryVectorDtype` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/asynchronous/database.py:925:13: Object of type `AsyncClientSession & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy | Unknown | Primary` is not assignable to `_ServerMode | None`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mongo-python-driver/bson/binary.py:476:39: Attribute `value` on type `BinaryVectorDtype | None | Unknown | (@Todo(specialized non-generic class) & BinaryVectorDtype)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/mongo-python-driver/bson/codec_options.py:390:45: Attribute `__origin__` on type `@Todo(unsupported nested subscript in type[X]) & ~AlwaysFalsy | Literal[dict]` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/synchronous/database.py:925:13: Object of type `ClientSession & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy | Unknown | Primary` is not assignable to `_ServerMode | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/asynchronous/database.py:925:13: Object of type `(AsyncClientSession & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy) | Unknown | Primary` is not assignable to `_ServerMode | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/asynchronous/encryption.py:189:13: Argument to this function is incorrect: Expected `SSLContext | None`, found `(Unknown & ~None) | SSLContext | SSLContext | Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/synchronous/encryption.py:188:13: Argument to this function is incorrect: Expected `SSLContext | None`, found `Unknown & ~None | SSLContext | SSLContext | Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/synchronous/encryption.py:188:13: Argument to this function is incorrect: Expected `SSLContext | None`, found `(Unknown & ~None) | SSLContext | SSLContext | Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/asynchronous/encryption.py:189:13: Argument to this function is incorrect: Expected `SSLContext | None`, found `Unknown & ~None | SSLContext | SSLContext | Unknown`

isort (https://github.com/pycqa/isort)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/isort/isort/api.py:161:5: Object of type `str & ~AlwaysFalsy | Path & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy` is not assignable to `str | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/isort/isort/api.py:161:5: Object of type `(str & ~AlwaysFalsy) | (Path & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy)` is not assignable to `str | None`

paasta (https://github.com/yelp/paasta)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/paasta/paasta_tools/api/views/autoscaler.py:92:17: Argument to this function is incorrect: Expected `int`, found `Unknown & int | int | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/paasta/paasta_tools/api/views/autoscaler.py:92:17: Argument to this function is incorrect: Expected `int`, found `(Unknown & int) | int | None`

cloud-init (https://github.com/canonical/cloud-init)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cloud-init/tests/unittests/sources/test_azure_helper.py:186:44: Argument to this function is incorrect: Expected `AzureEndpointHttpClient`, found `(Unknown & ~None) | MagicMock`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/cloud-init/cloudinit/config/cc_growpart.py:214:12: Return type does not match returned value: Expected `Resizer`, found `None | (Unknown & ~AlwaysFalsy)`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/cloud-init/cloudinit/config/cc_growpart.py:214:12: Return type does not match returned value: Expected `Resizer`, found `None | Unknown & ~AlwaysFalsy`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/cloud-init/tests/unittests/sources/test_azure.py:1248:9: Object of type `Mock` is not assignable to attribute `get_tmp_exec_path` on type `Unknown & ~str | Distro`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cloud-init/tests/unittests/sources/test_azure_helper.py:186:44: Argument to this function is incorrect: Expected `AzureEndpointHttpClient`, found `Unknown & ~None | MagicMock`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/cloud-init/tests/unittests/sources/test_azure.py:1248:9: Object of type `Mock` is not assignable to attribute `get_tmp_exec_path` on type `(Unknown & ~str) | Distro`

graphql-core (https://github.com/graphql-python/graphql-core)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:44:16: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `Unknown & int | Unknown & str & int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:44:16: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `(Unknown & int) | (Unknown & str & int)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:44:21: Operator `<=` is not supported for types `str` and `int`, in comparing `Unknown & int | Unknown & str & int` with `int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:44:21: Operator `<=` is not supported for types `str` and `int`, in comparing `(Unknown & int) | (Unknown & str & int)` with `int`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:64:24: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `@Todo(Type::Intersection.call()) & int | @Todo(Type::Intersection.call()) & str & int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:64:24: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `(@Todo(Type::Intersection.call()) & int) | (@Todo(Type::Intersection.call()) & str & int)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:64:29: Operator `<=` is not supported for types `str` and `int`, in comparing `@Todo(Type::Intersection.call()) & int | @Todo(Type::Intersection.call()) & str & int` with `int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/graphql-core/tests/language/test_visitor.py:64:29: Operator `<=` is not supported for types `str` and `int`, in comparing `(@Todo(Type::Intersection.call()) & int) | (@Todo(Type::Intersection.call()) & str & int)` with `int`

arviz (https://github.com/arviz-devs/arviz)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:231:33: Attribute `coords` on type `(Unknown & ~str) | None | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:231:52: Attribute `dims` on type `(Unknown & ~str) | None | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:235:29: Attribute `coords` on type `(Unknown & ~str) | None | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:231:33: Attribute `coords` on type `Unknown & ~str | None | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:231:52: Attribute `dims` on type `Unknown & ~str | None | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/tsplot.py:235:29: Attribute `coords` on type `Unknown & ~str | None | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/lmplot.py:229:32: Attribute `sizes` on type `Unknown & ~str | None | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/lmplot.py:229:57: Attribute `sizes` on type `Unknown & ~str | None | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/lmplot.py:229:32: Attribute `sizes` on type `(Unknown & ~str) | None | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arviz/arviz/plots/lmplot.py:229:57: Attribute `sizes` on type `(Unknown & ~str) | None | Unknown` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/arviz/arviz/stats/stats.py:2020:41: Argument to this function is incorrect: Expected `Sized`, found `Unknown | None | (Unknown & ~None) | tuple[()]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/arviz/arviz/stats/stats.py:2020:41: Argument to this function is incorrect: Expected `Sized`, found `Unknown | None | Unknown & ~None | tuple[()]`

setuptools (https://github.com/pypa/setuptools)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/wheel/metadata.py:20:12: Return type does not match returned value: Expected `bool | Literal[""]`, found `str & ~AlwaysTruthy | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/wheel/metadata.py:20:12: Return type does not match returned value: Expected `bool | Literal[""]`, found `(str & ~AlwaysTruthy) | @Todo(map_with_boundness: intersections with negative contributions)`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:38:25: Attribute `loader` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:40:9: Attribute `origin` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:42:24: Attribute `loader` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:48:9: Attribute `origin` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:50:24: Attribute `loader` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:55:10: Attribute `has_location` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:56:16: Attribute `origin` on type `Unknown & ~None | ModuleSpec | None` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/pkg_resources/tests/test_resources.py:165:9: Object of type `None` is not assignable to attribute `test_attr` on type `Unknown | @Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy | EmptyProvider`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/pkg_resources/tests/test_resources.py:175:9: Object of type `None` is not assignable to attribute `_test_attr` on type `Unknown | @Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy | EmptyProvider`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:38:25: Attribute `loader` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:40:9: Attribute `origin` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:42:24: Attribute `loader` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:48:9: Attribute `origin` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:50:24: Attribute `loader` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:55:10: Attribute `has_location` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_imp.py:56:16: Attribute `origin` on type `(Unknown & ~None) | ModuleSpec | None` is possibly unbound
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/pkg_resources/tests/test_resources.py:165:9: Object of type `None` is not assignable to attribute `test_attr` on type `Unknown | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | EmptyProvider`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/pkg_resources/tests/test_resources.py:175:9: Object of type `None` is not assignable to attribute `_test_attr` on type `Unknown | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | EmptyProvider`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:978:16: Attribute `finalized` on type `(str & Command) | (Command & Command) | Unknown` is possibly unbound
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:979:20: Return type does not match returned value: Expected `Command`, found `(str & Command) | (Command & Command) | Unknown`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:981:9: Object of type `Literal[False]` is not assignable to attribute `finalized` on type `(str & Command) | (Command & Command) | Unknown`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:989:16: Return type does not match returned value: Expected `Command`, found `(str & Command) | (Command & Command) | Unknown`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:978:16: Attribute `finalized` on type `str & Command | Command & Command | Unknown` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:979:20: Return type does not match returned value: Expected `Command`, found `str & Command | Command & Command | Unknown`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:981:9: Object of type `Literal[False]` is not assignable to attribute `finalized` on type `str & Command | Command & Command | Unknown`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:989:16: Return type does not match returned value: Expected `Command`, found `str & Command | Command & Command | Unknown`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/typing_extensions.py:2894:20: Operator `<` is not supported for types `int` and `_Sentinel`, in comparing `int` with `Unknown & ~AlwaysFalsy | _Sentinel & ~AlwaysFalsy | int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/typing_extensions.py:2894:20: Operator `<` is not supported for types `int` and `_Sentinel`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/typing_extensions.py:2911:46: Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `Unknown & ~AlwaysFalsy | _Sentinel & ~AlwaysFalsy | int | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/typing_extensions.py:2911:46: Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int | @Todo(map_with_boundness: intersections with negative contributions)`

pwndbg (https://github.com/pwndbg/pwndbg)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/enhance.py:165:16: Argument to this function is incorrect: Expected `Sized`, found `str & ~AlwaysFalsy | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:77:16: Argument to this function is incorrect: Expected `Sized`, found `Unknown & ~str | None | bytes`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:85:25: Attribute `hex` on type `Unknown & ~str | None | bytes` is possibly unbound
- error[lint:not-iterable] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:92:43: Object of type `Unknown & ~str | None | bytes` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/enhance.py:165:16: Argument to this function is incorrect: Expected `Sized`, found `(str & ~AlwaysFalsy) | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:77:16: Argument to this function is incorrect: Expected `Sized`, found `(Unknown & ~str) | None | bytes`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:85:25: Attribute `hex` on type `(Unknown & ~str) | None | bytes` is possibly unbound
+ error[lint:not-iterable] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/cyclic.py:92:43: Object of type `(Unknown & ~str) | None | bytes` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/aglib/onegadget.py:372:104: Argument to this function is incorrect: Expected `int`, found `(int & ~Literal[0]) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/aglib/onegadget.py:372:104: Argument to this function is incorrect: Expected `int`, found `int & ~Literal[0] | None`

openlibrary (https://github.com/internetarchive/openlibrary)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/openlibrary/openlibrary/utils/bulkimport.py:346:31: Operator `not in` is not supported for types `Unknown` and `None`, in comparing `(Unknown & ~AlwaysTruthy) | Unknown` with `Unknown | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/openlibrary/openlibrary/core/lending.py:608:42: Argument to this function is incorrect: Expected `str`, found `None | (OpenLibraryAccount & ~AlwaysTruthy) | Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/openlibrary/openlibrary/core/lending.py:613:42: Argument to this function is incorrect: Expected `str`, found `None | (OpenLibraryAccount & ~AlwaysTruthy) | @Todo(map_with_boundness: intersections with negative contributions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/openlibrary/openlibrary/core/lending.py:608:42: Argument to this function is incorrect: Expected `str`, found `None | OpenLibraryAccount & ~AlwaysTruthy | Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/openlibrary/openlibrary/core/lending.py:613:42: Argument to this function is incorrect: Expected `str`, found `None | OpenLibraryAccount & ~AlwaysTruthy | @Todo(map_with_boundness: intersections with negative contributions)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/openlibrary/openlibrary/utils/bulkimport.py:346:31: Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown & ~AlwaysTruthy | Unknown` with `Unknown | None`
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:643:24: Method `__getitem__` of type `Link & ~AlwaysFalsy | Literal[True]` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:643:24: Method `__getitem__` of type `(Link & ~AlwaysFalsy) | Literal[True]` is possibly unbound
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:644:21: Method `__getitem__` of type `Link & ~AlwaysFalsy | Literal[True]` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:644:21: Method `__getitem__` of type `(Link & ~AlwaysFalsy) | Literal[True]` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:645:13: Attribute `delete` on type `Link & ~AlwaysFalsy | Literal[True]` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:645:13: Attribute `delete` on type `(Link & ~AlwaysFalsy) | Literal[True]` is possibly unbound
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:758:20: Method `__getitem__` of type `Link & ~AlwaysFalsy | Literal[True]` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:758:20: Method `__getitem__` of type `(Link & ~AlwaysFalsy) | Literal[True]` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:762:9: Attribute `delete` on type `Link & ~AlwaysFalsy | Literal[True]` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/openlibrary/openlibrary/plugins/upstream/account.py:762:9: Attribute `delete` on type `(Link & ~AlwaysFalsy) | Literal[True]` is possibly unbound

apprise (https://github.com/caronc/apprise)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/apprise/apprise/plugins/ses.py:433:17: Argument to this function is incorrect: Expected `tuple[str | None, str]`, found `tuple[Unknown & ~AlwaysFalsy | Literal[False], Unknown]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/apprise/apprise/plugins/ses.py:433:17: Argument to this function is incorrect: Expected `tuple[str | None, str]`, found `tuple[(Unknown & ~AlwaysFalsy) | Literal[False], Unknown]`

django-stubs (https://github.com/typeddjango/django-stubs)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:34:61: Attribute `item` on type `Unknown & ~None | Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:35:16: Attribute `item` on type `Unknown & ~None | Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:38:47: Attribute `ret_type` on type `Unknown & ~None | Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/django/context.py:331:64: Attribute `base_field` on type `@Todo(specialized non-generic class) | ForeignObjectRel | @Todo(specialized non-generic class) & Field | AutoField` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/django/context.py:331:64: Attribute `base_field` on type `@Todo(specialized non-generic class) | ForeignObjectRel | (@Todo(specialized non-generic class) & Field) | AutoField` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:34:61: Attribute `item` on type `(Unknown & ~None) | Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:35:16: Attribute `item` on type `(Unknown & ~None) | Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/django-stubs/mypy_django_plugin/transformers/forms.py:38:47: Attribute `ret_type` on type `(Unknown & ~None) | Unknown | None` is possibly unbound

ignite (https://github.com/pytorch/ignite)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/ignite/ignite/engine/events.py:94:57: Operator `>` is not supported for types `Integral` and `int`, in comparing `(int & Integral) | (list & Integral)` with `Literal[0]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ignite/ignite/contrib/engines/common.py:174:65: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `ParamScheduler | (Unknown & ~None)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ignite/ignite/contrib/engines/common.py:174:65: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `ParamScheduler | Unknown & ~None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ignite/ignite/engine/events.py:94:57: Operator `>` is not supported for types `Integral` and `int`, in comparing `int & Integral | list & Integral` with `Literal[0]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ignite/tests/ignite/handlers/test_param_scheduler.py:1282:57: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown & ParamScheduler | LRScheduler`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ignite/tests/ignite/handlers/test_param_scheduler.py:1282:57: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `(Unknown & ParamScheduler) | LRScheduler`

bokeh (https://github.com/bokeh/bokeh)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/bokeh/src/bokeh/core/property/visual.py:206:22: Attribute `format` on type `Any & ~ndarray & ~str | Unknown & ~str | Path | Unknown` is possibly unbound
- warning[l...*[Comment body truncated]*

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6997 on 2025-04-23 04:10_

We shouldn't use `is_single_valued` for this; that has a different meaning already (a type where all inhabitants compare equal to each other). I would probably name this `element_count()` and have it return a `usize` -- the caller can check if it equals `1`.

---

_Renamed from "[red-knot] Surround union and intersection with `()`" to "[red-knot] Surround intersections with `()` in potentially ambiguous contexts" by @carljm on 2025-04-23 04:12_

---

_@carljm reviewed on 2025-04-23 04:12_

Looks good! Just one method naming nit, which I can update and then merge.

---

_Merged by @carljm on 2025-04-23 04:18_

---

_Closed by @carljm on 2025-04-23 04:18_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 04:33_

---

_Branch deleted on 2025-04-23 08:38_

---
