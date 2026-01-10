```yaml
number: 16611
title: "[red-knot] Handle gradual intersection types in assignability"
type: pull_request
state: merged
author: jgeralnik
labels:
  - ty
assignees: []
merged: true
base: main
head: joey/assign_intersection_any
created_at: 2025-03-10T22:03:48Z
updated_at: 2025-03-11T14:58:56Z
url: https://github.com/astral-sh/ruff/pull/16611
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Handle gradual intersection types in assignability

---

_Pull request opened by @jgeralnik on 2025-03-10 22:03_

## Summary

This mostly fixes #14899

My motivation was similar to the last comment by @sharkdp there. I ran red_knot on a codebase and the most common error was patterns like this failing:

```
def foo(x: str): ...

x: Any = ...
if isinstance(x, str):
    foo(x) # Object of type `Any & str` cannot be assigned to parameter 1 (`x`) of function `foo`; expected type `str`
```

The desired behavior is pretty much to ignore Any/Unknown when resolving intersection assignability - `Any & str` should be assignable to `str`, and `str` should be assignable to `str & Any`
 
The fix is actually very similar to the existing code in `is_subtype_of`, we need to correctly handle intersections on either side, while being careful to handle dynamic types as desired.

This does not fix the second test case from that issue:

```
static_assert(is_assignable_to(Intersection[Unrelated, Any], Not[tuple[Unrelated, Any]]))
```

but that's misleading because the root cause there has nothing to do with gradual types. I added a simpler test case that also fails:

```
static_assert(is_assignable_to(Unrelated, Not[tuple[Unrelated]]))
```
This is because we don't determine that Unrelated does not subclass from tuple so we can't rule out this relation. If that logic is improved then this fix should also handle the case of the intersection

## Test Plan

Added a bunch of is_assignable_to tests, most of which failed before this fix.

---

_Review requested from @carljm by @jgeralnik on 2025-03-10 22:03_

---

_Review requested from @MichaReiser by @jgeralnik on 2025-03-10 22:03_

---

_Review requested from @AlexWaygood by @jgeralnik on 2025-03-10 22:03_

---

_Review requested from @sharkdp by @jgeralnik on 2025-03-10 22:03_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-10 22:05_

---

_Comment by @github-actions[bot] on 2025-03-10 22:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
```diff
git-revise (https://github.com/mystor/git-revise)
- error: lint:invalid-raise
-    --> /tmp/mypy_primer/projects/git-revise/tests/conftest.py:239:19
-     |
- 238 |         if result_future.done() or not result_future.set_running_or_notify_cancel():
- 239 |             raise result_future.exception() or CancelledError()
-     |                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Cannot raise object of type `Unknown & ~AlwaysFalsy | CancelledError` (must be a `BaseException` subclass or instance)
- 240 |
- 241 |         try:
-     |
- 
- Found 53 diagnostics
+ Found 52 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-    |
- 
- error: lint:invalid-argument-type
-   --> /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:66:56
-    |
- 64 |         return repo_dir
- 65 |     revision = (await revision_like(repo_dir)) if callable(revision_like) else revision_like
- 66 |     revision = await get_revision_for_revision_or_date(revision, repo_dir)
-    |                                                        ^^^^^^^^ Object of type `@Todo | @Todo & ~None` cannot be assigned to parameter 1 (`revision`) of function `get_revision_for_revision_or_date`; expected type `str`
- 67 |
- 68 |     for retry in (True, False):
-    |
-   ::: /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:41:45
-    |
- 41 | async def get_revision_for_revision_or_date(revision: str, repo_dir: Path) -> str:
-    |                                             ------------- info: parameter declared in function definition here
- 42 |     try:
- 43 |         # try and interpret revision as an isoformatted date
- 
-     |
-     |
-     |
-     |
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:303:60
- 301 |     await run(["git", "bisect", "start"], cwd=repo_dir, output=True)
- 302 |     await run(["git", "bisect", "good"], cwd=repo_dir, output=True)
- 303 |     new_revision = await get_revision_for_revision_or_date(ARGS.new or "origin/HEAD", repo_dir)
-     |                                                            ^^^^^^^^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~AlwaysFalsy | Literal["origin/HEAD"]` cannot be assigned to parameter 1 (`revision`) of function `get_revision_for_revision_or_date`; expected type `str`
- 304 |     await run(["git", "bisect", "bad", new_revision], cwd=repo_dir, output=True)
-    ::: /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:41:45
-  41 | async def get_revision_for_revision_or_date(revision: str, repo_dir: Path) -> str:
-     |                                             ------------- info: parameter declared in function definition here
-  42 |     try:
-  43 |         # try and interpret revision as an isoformatted date
- Found 52 diagnostics
+ Found 50 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:211:48
-     |
- 209 |         if arg_count == 0:
- 210 |             if isinstance(tz, str):
- 211 |                 tz = parser.TzinfoParser.parse(tz)
-     |                                                ^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 212 |                 return self.type.now(tzinfo=tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:253:62
-     |
- 251 |             # (str) -> parse @ tzinfo
- 252 |             elif isinstance(arg, str):
- 253 |                 dt = parser.DateTimeParser(locale).parse_iso(arg, normalize_whitespace)
-     |                                                              ^^^ Object of type `@Todo & str & ~Decimal & ~tzinfo & ~Arrow & ~date | float & str & ~Arrow & ~date & ~tzinfo` cannot be assigned to parameter 2 (`datetime_string`) of bound method `parse_iso`; expected type `str`
- 254 |                 return self.type.fromdatetime(dt, tzinfo=tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:250:15
-     |
- 248 |     # IDEA: break into multiple functions
- 249 |     def parse_iso(
- 250 |         self, datetime_string: str, normalize_whitespace: bool = False
-     |               -------------------- info: parameter declared in function definition here
- 251 |     ) -> datetime:
- 252 |         """
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:342:44
-     |
- 340 |             tz = dateutil_tz.tzlocal()
- 341 |         elif not isinstance(tz, dt_tzinfo):
- 342 |             tz = parser.TzinfoParser.parse(tz)
-     |                                            ^^ Object of type `@Todo & ~None & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 343 |
- 344 |         return self.type.now(tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
-     |
- 
-     |
- 168 |             tzinfo = parser.TzinfoParser.parse(tzinfo.zone)
-     |
- 
-     |
- 169 |         elif isinstance(tzinfo, str):
- 170 |             tzinfo = parser.TzinfoParser.parse(tzinfo)
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:170:48
-     |
-     |                                                ^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 171 |
- 172 |         fold = kwargs.get("fold", 0)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 89git-revise (https://github.com/mystor/git-revise)
- error: lint:invalid-raise
-    --> /tmp/mypy_primer/projects/git-revise/tests/conftest.py:239:19
-     |
- 238 |         if result_future.done() or not result_future.set_running_or_notify_cancel():
- 239 |             raise result_future.exception() or CancelledError()
-     |                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Cannot raise object of type `Unknown & ~AlwaysFalsy | CancelledError` (must be a `BaseException` subclass or instance)
- 240 |
- 241 |         try:
-     |
- 
- Found 53 diagnostics
+ Found 52 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-    |
- 
- error: lint:invalid-argument-type
-   --> /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:66:56
-    |
- 64 |         return repo_dir
- 65 |     revision = (await revision_like(repo_dir)) if callable(revision_like) else revision_like
- 66 |     revision = await get_revision_for_revision_or_date(revision, repo_dir)
-    |                                                        ^^^^^^^^ Object of type `@Todo | @Todo & ~None` cannot be assigned to parameter 1 (`revision`) of function `get_revision_for_revision_or_date`; expected type `str`
- 67 |
- 68 |     for retry in (True, False):
-    |
-   ::: /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:41:45
-    |
- 41 | async def get_revision_for_revision_or_date(revision: str, repo_dir: Path) -> str:
-    |                                             ------------- info: parameter declared in function definition here
- 42 |     try:
- 43 |         # try and interpret revision as an isoformatted date
- 
-     |
-     |
-     |
-     |
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:303:60
- 301 |     await run(["git", "bisect", "start"], cwd=repo_dir, output=True)
- 302 |     await run(["git", "bisect", "good"], cwd=repo_dir, output=True)
- 303 |     new_revision = await get_revision_for_revision_or_date(ARGS.new or "origin/HEAD", repo_dir)
-     |                                                            ^^^^^^^^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~AlwaysFalsy | Literal["origin/HEAD"]` cannot be assigned to parameter 1 (`revision`) of function `get_revision_for_revision_or_date`; expected type `str`
- 304 |     await run(["git", "bisect", "bad", new_revision], cwd=repo_dir, output=True)
-    ::: /tmp/mypy_primer/projects/mypy_primer/mypy_primer/git_utils.py:41:45
-  41 | async def get_revision_for_revision_or_date(revision: str, repo_dir: Path) -> str:
-     |                                             ------------- info: parameter declared in function definition here
-  42 |     try:
-  43 |         # try and interpret revision as an isoformatted date
- Found 52 diagnostics
+ Found 50 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:211:48
-     |
- 209 |         if arg_count == 0:
- 210 |             if isinstance(tz, str):
- 211 |                 tz = parser.TzinfoParser.parse(tz)
-     |                                                ^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 212 |                 return self.type.now(tzinfo=tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:253:62
-     |
- 251 |             # (str) -> parse @ tzinfo
- 252 |             elif isinstance(arg, str):
- 253 |                 dt = parser.DateTimeParser(locale).parse_iso(arg, normalize_whitespace)
-     |                                                              ^^^ Object of type `@Todo & str & ~Decimal & ~tzinfo & ~Arrow & ~date | float & str & ~Arrow & ~date & ~tzinfo` cannot be assigned to parameter 2 (`datetime_string`) of bound method `parse_iso`; expected type `str`
- 254 |                 return self.type.fromdatetime(dt, tzinfo=tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:250:15
-     |
- 248 |     # IDEA: break into multiple functions
- 249 |     def parse_iso(
- 250 |         self, datetime_string: str, normalize_whitespace: bool = False
-     |               -------------------- info: parameter declared in function definition here
- 251 |     ) -> datetime:
- 252 |         """
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:342:44
-     |
- 340 |             tz = dateutil_tz.tzlocal()
- 341 |         elif not isinstance(tz, dt_tzinfo):
- 342 |             tz = parser.TzinfoParser.parse(tz)
-     |                                            ^^ Object of type `@Todo & ~None & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 343 |
- 344 |         return self.type.now(tz)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
-     |
- 
-     |
- 168 |             tzinfo = parser.TzinfoParser.parse(tzinfo.zone)
-     |
- 
-     |
- 169 |         elif isinstance(tzinfo, str):
- 170 |             tzinfo = parser.TzinfoParser.parse(tzinfo)
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:170:48
-     |
-     |                                                ^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 171 |
- 172 |         fold = kwargs.get("fold", 0)
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
- 252 |             tzinfo = dateutil_tz.tzlocal()
- 253 |         elif isinstance(tzinfo, str):
- 254 |             tzinfo = parser.TzinfoParser.parse(tzinfo)
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:254:48
-     |
-     |                                                ^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 255 |
- 256 |         if not util.is_timestamp(timestamp):
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
-     |
-      |
-      |
-      |
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1076:44
-      |
- 1075 |         if not isinstance(tz, dt_tzinfo):
- 1076 |             tz = parser.TzinfoParser.parse(tz)
-      |                                            ^^ Object of type `@Todo & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 1077 |
- 1078 |         dt = self._datetime.astimezone(tz)
-      |
-     ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-      |
-  896 |     @classmethod
-  897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-      |                    ------------------ info: parameter declared in function definition here
-  898 |         """
-  899 |         Parse a timezone string and return a datetime timezone object.
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1800:50
- 1798 |         else:
- 1799 |             try:
- 1800 |                 return parser.TzinfoParser.parse(tz_expr)
-      |                                                  ^^^^^^^ Object of type `@Todo & ~None & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 1801 |             except parser.ParserError:
- 1802 |                 raise ValueError(f"{tz_expr!r} not recognized as a timezone.")
-     ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-  896 |     @classmethod
-  897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-      |                    ------------------ info: parameter declared in function definition here
-  898 |         """
-  899 |         Parse a timezone string and return a datetime timezone object.
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:730:43
- 729 |         if timestamp is not None:
- 730 |             return datetime.fromtimestamp(timestamp, tz=tz.tzutc())
-     |                                           ^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 2 (`timestamp`) of bound method `fromtimestamp`; expected type `int | float`
- 731 |
- 732 |         expanded_timestamp = parts.get("expanded_timestamp")
-    ::: vendored://stdlib/datetime.pyi:269:32
- 267 |     if sys.version_info >= (3, 12):
- 268 |         @classmethod
- 269 |         def fromtimestamp(cls, timestamp: float, tz: _TzInfo | None = ...) -> Self: ...
-     |                                ---------------- info: parameter declared in function definition here
- 270 |     else:
- 271 |         @classmethod
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:736:37
- 734 |         if expanded_timestamp is not None:
- 735 |             return datetime.fromtimestamp(
- 736 |                 normalize_timestamp(expanded_timestamp),
-     |                                     ^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 1 (`timestamp`) of function `normalize_timestamp`; expected type `int | float`
- 737 |                 tz=tz.tzutc(),
- 738 |             )
-    ::: /tmp/mypy_primer/projects/arrow/arrow/util.py:73:25
-  73 | def normalize_timestamp(timestamp: float) -> float:
-     |                         ---------------- info: parameter declared in function definition here
-  74 |     """Normalize millisecond and microsecond timestamps into normal timestamps."""
-  75 |     if timestamp > MAX_TIMESTAMP:
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:780:72
- 778 |             # dddd MM YYYY => first day of week in specified year and month
- 779 |             # dddd MM => first day after epoch in specified month
- 780 |             next_weekday_dt = next_weekday(datetime(year, month, day), day_of_week)
-     |                                                                        ^^^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 2 (`weekday`) of function `next_weekday`; expected type `int`
- 781 |             parts["year"] = next_weekday_dt.year
- 782 |             parts["month"] = next_weekday_dt.month
-    ::: /tmp/mypy_primer/projects/arrow/arrow/util.py:18:42
-  17 | def next_weekday(
-  18 |     start_date: Optional[datetime.date], weekday: int
-     |                                          ------------ info: parameter declared in function definition here
-  19 | ) -> datetime.datetime:
-  20 |     """Get next weekday from the specified start date.
- Found 83 diagnostics
+ Found 73 diagnostics7 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
- 252 |             tzinfo = dateutil_tz.tzlocal()
- 253 |         elif isinstance(tzinfo, str):
- 254 |             tzinfo = parser.TzinfoParser.parse(tzinfo)
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:254:48
-     |
-     |                                                ^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 255 |
- 256 |         if not util.is_timestamp(timestamp):
-     |
-    ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-     |
- 896 |     @classmethod
- 897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-     |                    ------------------ info: parameter declared in function definition here
- 898 |         """
- 899 |         Parse a timezone string and return a datetime timezone object.
-     |
- 
-     |
-      |
-      |
-      |
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1076:44
-      |
- 1075 |         if not isinstance(tz, dt_tzinfo):
- 1076 |             tz = parser.TzinfoParser.parse(tz)
-      |                                            ^^ Object of type `@Todo & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 1077 |
- 1078 |         dt = self._datetime.astimezone(tz)
-      |
-     ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-      |
-  896 |     @classmethod
-  897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-      |                    ------------------ info: parameter declared in function definition here
-  898 |         """
-  899 |         Parse a timezone string and return a datetime timezone object.
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1800:50
- 1798 |         else:
- 1799 |             try:
- 1800 |                 return parser.TzinfoParser.parse(tz_expr)
-      |                                                  ^^^^^^^ Object of type `@Todo & ~None & ~tzinfo` cannot be assigned to parameter 2 (`tzinfo_string`) of bound method `parse`; expected type `str`
- 1801 |             except parser.ParserError:
- 1802 |                 raise ValueError(f"{tz_expr!r} not recognized as a timezone.")
-     ::: /tmp/mypy_primer/projects/arrow/arrow/parser.py:897:20
-  896 |     @classmethod
-  897 |     def parse(cls, tzinfo_string: str) -> dt_tzinfo:
-      |                    ------------------ info: parameter declared in function definition here
-  898 |         """
-  899 |         Parse a timezone string and return a datetime timezone object.
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:730:43
- 729 |         if timestamp is not None:
- 730 |             return datetime.fromtimestamp(timestamp, tz=tz.tzutc())
-     |                                           ^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 2 (`timestamp`) of bound method `fromtimestamp`; expected type `int | float`
- 731 |
- 732 |         expanded_timestamp = parts.get("expanded_timestamp")
-    ::: vendored://stdlib/datetime.pyi:269:32
- 267 |     if sys.version_info >= (3, 12):
- 268 |         @classmethod
- 269 |         def fromtimestamp(cls, timestamp: float, tz: _TzInfo | None = ...) -> Self: ...
-     |                                ---------------- info: parameter declared in function definition here
- 270 |     else:
- 271 |         @classmethod
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:736:37
- 734 |         if expanded_timestamp is not None:
- 735 |             return datetime.fromtimestamp(
- 736 |                 normalize_timestamp(expanded_timestamp),
-     |                                     ^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 1 (`timestamp`) of function `normalize_timestamp`; expected type `int | float`
- 737 |                 tz=tz.tzutc(),
- 738 |             )
-    ::: /tmp/mypy_primer/projects/arrow/arrow/util.py:73:25
-  73 | def normalize_timestamp(timestamp: float) -> float:
-     |                         ---------------- info: parameter declared in function definition here
-  74 |     """Normalize millisecond and microsecond timestamps into normal timestamps."""
-  75 |     if timestamp > MAX_TIMESTAMP:
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/arrow/arrow/parser.py:780:72
- 778 |             # dddd MM YYYY => first day of week in specified year and month
- 779 |             # dddd MM => first day after epoch in specified month
- 780 |             next_weekday_dt = next_weekday(datetime(year, month, day), day_of_week)
-     |                                                                        ^^^^^^^^^^^ Object of type `Unknown & ~None` cannot be assigned to parameter 2 (`weekday`) of function `next_weekday`; expected type `int`
- 781 |             parts["year"] = next_weekday_dt.year
- 782 |             parts["month"] = next_weekday_dt.month
-    ::: /tmp/mypy_primer/projects/arrow/arrow/util.py:18:42
-  17 | def next_weekday(
-  18 |     start_date: Optional[datetime.date], weekday: int
-     |                                          ------------ info: parameter declared in function definition here
-  19 | ) -> datetime.datetime:
-  20 |     """Get next weekday from the specified start date.
- Found 83 diagnostics
+ Found 73 diagnostics

black (https://github.com/psf/black)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/lines.py:442:49
-     |
- 441 |             if subscript_start.type == syms.subscriptlist:
- 442 |                 subscript_start = child_towards(subscript_start, leaf)
-     |                                                 ^^^^^^^^^^^^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`ancestor`) of function `child_towards`; expected type `Node`
- 443 |
- 444 |         return subscript_start is not None and any(
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:478:19
-     |
- 478 | def child_towards(ancestor: Node, descendant: LN) -> Optional[LN]:
-     |                   -------------- info: parameter declared in function definition here
- 479 |     """Return the child of `ancestor` that contains `descendant`."""
- 480 |     node: Optional[LN] = descendant
-     |
- 
- error: lint:invalid-assignment
-    --> /tmp/mypy_primer/projects/black/src/black/lines.py:723:17
-     |
- 721 |                 and slc.before <= 1
- 722 |             ):
- 723 |                 comment_to_add_newlines = slc
-     |                 ^^^^^^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~None` is not assignable to `LinesBlock | None`
- 724 |             else:
- 725 |                 return 0, 0
-     |
- 
-    |
-    |
-    |
- 
-    |
- 
-     |
-     |
-     |
-     |
-     |
-     |
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/files.py:152:45
-     |
- 150 |     if requires_python is not None:
- 151 |         try:
- 152 |             return parse_req_python_version(requires_python)
-     |                                             ^^^^^^^^^^^^^^^ Object of type `@Todo & ~None` cannot be assigned to parameter 1 (`requires_python`) of function `parse_req_python_version`; expected type `str`
- 153 |         except InvalidVersion:
- 154 |             pass
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/files.py:163:30
-     |
- 163 | def parse_req_python_version(requires_python: str) -> Optional[list[TargetVersion]]:
-     |                              -------------------- info: parameter declared in function definition here
- 164 |     """Parse a version string (i.e. ``"3.7"``) to a list of TargetVersion.
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/files.py:156:47
-     |
- 154 |             pass
- 155 |         try:
- 156 |             return parse_req_python_specifier(requires_python)
-     |                                               ^^^^^^^^^^^^^^^ Object of type `@Todo & ~None` cannot be assigned to parameter 1 (`requires_python`) of function `parse_req_python_specifier`; expected type `str`
- 157 |         except (InvalidSpecifier, InvalidVersion):
- 158 |             pass
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/files.py:178:32
-     |
- 178 | def parse_req_python_specifier(requires_python: str) -> Optional[list[TargetVersion]]:
-     |                                -------------------- info: parameter declared in function definition here
- 179 |     """Parse a specifier string (i.e. ``">=3.7,<3.10"``) to a list of TargetVersion.
-     |
- 
-     |
-     |
- 
- 
- 
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/tokenize.py:422:28
-     |
- 420 |         return default, []
- 421 |
- 422 |     encoding = find_cookie(first)
-     |                            ^^^^^ Object of type `bytes & ~AlwaysFalsy | @Todo & ~AlwaysFalsy` cannot be assigned to parameter 1 (`line`) of function `find_cookie`; expected type `bytes`
- 423 |     if encoding:
- 424 |         return encoding, [first]
-     |
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/tokenize.py:392:21
-     |
- 390 |             return b""
- 391 |
- 392 |     def find_cookie(line: bytes) -> Optional[str]:
-     |                     ----------- info: parameter declared in function definition here
- 393 |         try:
- 394 |             line_string = line.decode("ascii")
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-      |
-      |
- error: lint:call-non-callable
-    --> /tmp/mypy_primer/projects/black/src/blackd/__init__.py:203:37
-     |
- 201 |         if piece:
- 202 |             try:
- 203 |                 enable_features.add(black.Preview[piece])
-     |                                     ^^^^^^^^^^^^^ Method `__getitem__` of type `<bound method `__getitem__` of `Literal[Preview]`>` is not callable on object of type `Literal[Preview]`
- 204 |             except KeyError:
- 205 |                 raise HeaderError(
-     |
-   --> /tmp/mypy_primer/projects/black/src/black/debug.py:29:31
- 27 |         indent = " " * (2 * self.tree_depth)
- 28 |         if isinstance(node, Node):
- 29 |             _type = type_repr(node.type)
-    |                               ^^^^^^^^^ Object of type `@Todo & Unknown | @Todo & int` cannot be assigned to parameter 1 (`type_num`) of function `type_repr`; expected type `int`
- 30 |             self.out(f"{indent}{_type}", fg="yellow")
- 31 |             self.tree_depth += 1
-   ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:30:15
- 30 | def type_repr(type_num: int) -> Union[str, int]:
-    |               ------------- info: parameter declared in function definition here
- 31 |     global _type_reprs
- 32 |     if not _type_reprs:
-    --> /tmp/mypy_primer/projects/black/src/black/comments.py:137:52
- 135 |         nl_count = remainder.count("\n")
- 136 |         form_feed = "\f" in remainder and remainder.endswith("\n")
- 137 |         leaf.prefix = make_simple_prefix(nl_count, form_feed)
-     |                                                    ^^^^^^^^^ Object of type `@Todo & ~AlwaysTruthy | @Todo` cannot be assi

black (https://github.com/psf/black)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/lines.py:442:49
-     |
- 441 |             if subscript_start.type == syms.subscriptlist:
- 442 |                 subscript_start = child_towards(subscript_start, leaf)
-     |                                                 ^^^^^^^^^^^^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`ancestor`) of function `child_towards`; expected type `Node`
- 443 |
- 444 |         return subscript_start is not None and any(
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:478:19
-     |
- 478 | def child_towards(ancestor: Node, descendant: LN) -> Optional[LN]:
-     |                   -------------- info: parameter declared in function definition here
- 479 |     """Return the child of `ancestor` that contains `descendant`."""
- 480 |     node: Optional[LN] = descendant
-     |
- 
- error: lint:invalid-assignment
-    --> /tmp/mypy_primer/projects/black/src/black/lines.py:723:17
-     |
- 721 |                 and slc.before <= 1
- 722 |             ):
- 723 |                 comment_to_add_newlines = slc
-     |                 ^^^^^^^^^^^^^^^^^^^^^^^ Object of type `Unknown & ~None` is not assignable to `LinesBlock | None`
- 724 |             else:
- 725 |                 return 0, 0
-     |
- 
-    |
-    |
-    |
- 
-    |
- 
-     |
-     |
-     |
-     |
-     |
-     |
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/files.py:152:45
-     |
- 150 |     if requires_python is not None:
- 151 |         try:
- 152 |             return parse_req_python_version(requires_python)
-     |                                             ^^^^^^^^^^^^^^^ Object of type `@Todo & ~None` cannot be assigned to parameter 1 (`requires_python`) of function `parse_req_python_version`; expected type `str`
- 153 |         except InvalidVersion:
- 154 |             pass
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/files.py:163:30
-     |
- 163 | def parse_req_python_version(requires_python: str) -> Optional[list[TargetVersion]]:
-     |                              -------------------- info: parameter declared in function definition here
- 164 |     """Parse a version string (i.e. ``"3.7"``) to a list of TargetVersion.
-     |
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/files.py:156:47
-     |
- 154 |             pass
- 155 |         try:
- 156 |             return parse_req_python_specifier(requires_python)
-     |                                               ^^^^^^^^^^^^^^^ Object of type `@Todo & ~None` cannot be assigned to parameter 1 (`requires_python`) of function `parse_req_python_specifier`; expected type `str`
- 157 |         except (InvalidSpecifier, InvalidVersion):
- 158 |             pass
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/files.py:178:32
-     |
- 178 | def parse_req_python_specifier(requires_python: str) -> Optional[list[TargetVersion]]:
-     |                                -------------------- info: parameter declared in function definition here
- 179 |     """Parse a specifier string (i.e. ``">=3.7,<3.10"``) to a list of TargetVersion.
-     |
- 
-     |
-     |
- 
- 
- 
- 
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/tokenize.py:422:28
-     |
- 420 |         return default, []
- 421 |
- 422 |     encoding = find_cookie(first)
-     |                            ^^^^^ Object of type `bytes & ~AlwaysFalsy | @Todo & ~AlwaysFalsy` cannot be assigned to parameter 1 (`line`) of function `find_cookie`; expected type `bytes`
- 423 |     if encoding:
- 424 |         return encoding, [first]
-     |
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/tokenize.py:392:21
-     |
- 390 |             return b""
- 391 |
- 392 |     def find_cookie(line: bytes) -> Optional[str]:
-     |                     ----------- info: parameter declared in function definition here
- 393 |         try:
- 394 |             line_string = line.decode("ascii")
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-     |
-     |
- 
-      |
-      |
- error: lint:call-non-callable
-    --> /tmp/mypy_primer/projects/black/src/blackd/__init__.py:203:37
-     |
- 201 |         if piece:
- 202 |             try:
- 203 |                 enable_features.add(black.Preview[piece])
-     |                                     ^^^^^^^^^^^^^ Method `__getitem__` of type `<bound method `__getitem__` of `Literal[Preview]`>` is not callable on object of type `Literal[Preview]`
- 204 |             except KeyError:
- 205 |                 raise HeaderError(
-     |
-   --> /tmp/mypy_primer/projects/black/src/black/debug.py:29:31
- 27 |         indent = " " * (2 * self.tree_depth)
- 28 |         if isinstance(node, Node):
- 29 |             _type = type_repr(node.type)
-    |                               ^^^^^^^^^ Object of type `@Todo & Unknown | @Todo & int` cannot be assigned to parameter 1 (`type_num`) of function `type_repr`; expected type `int`
- 30 |             self.out(f"{indent}{_type}", fg="yellow")
- 31 |             self.tree_depth += 1
-   ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:30:15
- 30 | def type_repr(type_num: int) -> Union[str, int]:
-    |               ------------- info: parameter declared in function definition here
- 31 |     global _type_reprs
- 32 |     if not _type_reprs:
-    --> /tmp/mypy_primer/projects/black/src/black/comments.py:137:52
- 135 |         nl_count = remainder.count("\n")
- 136 |         form_feed = "\f" in remainder and remainder.endswith("\n")
- 137 |         leaf.prefix = make_simple_prefix(nl_count, form_feed)
-     |                                                    ^^^^^^^^^ Object of type `@Todo & ~AlwaysTruthy | @Todo` cannot be assigned to parameter 2 (`form_feed`) of function `make_simple_prefix`; expected type `bool`
- 138 |         return
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:424:39
- 424 | def make_simple_prefix(nl_count: int, form_feed: bool, empty_line: str = "\n") -> str:
-     |                                       --------------- info: parameter declared in function definition here
- 425 |     """Generate a normalized prefix string."""
- 426 |     if form_feed:
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-    --> /tmp/mypy_primer/projects/black/src/black/brackets.py:346:21
-     |
- 344 |     for c in node.children[1:-1]:
- 345 |         if isinstance(c, Leaf):
- 346 |             bt.mark(c)
-     |                     ^ Object of type `@Todo & Leaf` cannot be assigned to parameter 2 (`leaf`) of bound method `mark`; expected type `Leaf`
- 347 |         else:
- 348 |             for leaf in c.leaves():
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/brackets.py:71:20
-     |
-  69 |     invisible: list[Leaf] = field(default_factory=list)
-  70 |
-  71 |     def mark(self, leaf: Leaf) -> None:
-     |                    ---------- info: parameter declared in function definition here
-  72 |         """Mark `leaf` with bracket-related metadata. Keep track of delimiters.
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:553:36
- 551 |             return False
- 552 |
- 553 |         prefix = get_string_prefix(node.value)
-     |                                    ^^^^^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 1 (`string`) of function `get_string_prefix`; expected type `str`
- 554 |         if set(prefix).intersection("bBfF"):
- 555 |             return False
-    ::: /tmp/mypy_primer/projects/black/src/black/strings.py:89:23
-  89 | def get_string_prefix(string: str) -> str:
-     |                       ----------- info: parameter declared in function definition here
-  90 |     """
-  91 |     Pre-conditions:
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:791:46
- 789 | def is_multiline_string(node: LN) -> bool:
- 790 |     """Return True if `leaf` is a multiline string that actually spans many lines."""
- 791 |     if isinstance(node, Node) and is_fstring(node):
-     |                                              ^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`node`) of function `is_fstring`; expected type `Node`
- 792 |         leaf = fstring_to_string(node)
- 793 |     elif isinstance(node, Leaf):
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:776:16
- 776 | def is_fstring(node: Node) -> bool:
-     |                ---------- info: parameter declared in function definition here
- 777 |     """Return True if the node is an f-string"""
- 778 |     return node.type == syms.fstring
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:792:34
- 790 |     """Return True if `leaf` is a multiline string that actually spans many lines."""
- 791 |     if isinstance(node, Node) and is_fstring(node):
- 792 |         leaf = fstring_to_string(node)
-     |                                  ^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`node`) of function `fstring_to_string`; expected type `Node`
- 793 |     elif isinstance(node, Leaf):
- 794 |         leaf = node
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:781:23
- 781 | def fstring_to_string(node: Node) -> Leaf:
-     |                       ---------- info: parameter declared in function definition here
- 782 |     """Converts an fstring node back to a string node."""
- 783 |     string_without_prefix = str(node)[len(node.prefix) :]
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:798:30
- 796 |         return False
- 797 |
- 798 |     return has_triple_quotes(leaf.value) and "\n" in leaf.value
-     |                              ^^^^^^^^^^ Object of type `str | @Todo & str` cannot be assigned to parameter 1 (`string`) of function `has_triple_quotes`; expected type `str`
-    ::: /tmp/mypy_primer/projects/black/src/black/strings.py:38:23
-  38 | def has_triple_quotes(string: str) -> bool:
-     |                       ----------- info: parameter declared in function definition here
-  39 |     """
-  40 |     Returns:
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:961:25
- 959 |     new_child = Node(syms.atom, [lpar, child, rpar])
- 960 |     new_child.prefix = prefix
- 961 |     parent.insert_child(index, new_child)
-     |                         ^^^^^ Object of type `@Todo & ~AlwaysFalsy | Literal[0]` cannot be assigned to parameter 2 (`i`) of bound method `insert_child`; expected type `int`
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:335:28
- 333 |         self.invalidate_sibling_maps()
- 334 |
- 335 |     def insert_child(self, i: int, child: NL) -> None:
-     |                            ------ info: parameter declared in function definition here
- 336 |         """
- 337 |         Equivalent to 'node.children.insert(i, child)'. This method also sets
-      |
-      |
-      |
-      |
-      |
-      |
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:222:63
- 221 |                 elif isinstance(item, ast.AST):
- 222 |                     yield from _stringify_ast_with_new_parent(item, parent_stack, node)
-     |                                                               ^^^^ Object of type `@Todo & AST` cannot be assigned to parameter 1 (`node`) of function `_stringify_ast_with_new_parent`; expected type `AST`
- 223 |
- 224 |         elif isinstance(value, ast.AST):
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:175:5
- 174 | def _stringify_ast_with_new_parent(
- 175 |     node: ast.AST, parent_stack: list[ast.AST], new_parent: ast.AST
-     |     ------------- info: parameter declared in function definition here
- 176 | ) -> Iterator[str]:
- 177 |     parent_stack.append(new_parent)
- error: lint:invagned to parameter 2 (`form_feed`) of function `make_simple_prefix`; expected type `bool`
- 138 |         return
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:424:39
- 424 | def make_simple_prefix(nl_count: int, form_feed: bool, empty_line: str = "\n") -> str:
-     |                                       --------------- info: parameter declared in function definition here
- 425 |     """Generate a normalized prefix string."""
- 426 |     if form_feed:
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-     |
-    --> /tmp/mypy_primer/projects/black/src/black/brackets.py:346:21
-     |
- 344 |     for c in node.children[1:-1]:
- 345 |         if isinstance(c, Leaf):
- 346 |             bt.mark(c)
-     |                     ^ Object of type `@Todo & Leaf` cannot be assigned to parameter 2 (`leaf`) of bound method `mark`; expected type `Leaf`
- 347 |         else:
- 348 |             for leaf in c.leaves():
-     |
-    ::: /tmp/mypy_primer/projects/black/src/black/brackets.py:71:20
-     |
-  69 |     invisible: list[Leaf] = field(default_factory=list)
-  70 |
-  71 |     def mark(self, leaf: Leaf) -> None:
-     |                    ---------- info: parameter declared in function definition here
-  72 |         """Mark `leaf` with bracket-related metadata. Keep track of delimiters.
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:553:36
- 551 |             return False
- 552 |
- 553 |         prefix = get_string_prefix(node.value)
-     |                                    ^^^^^^^^^^ Object of type `@Todo & str` cannot be assigned to parameter 1 (`string`) of function `get_string_prefix`; expected type `str`
- 554 |         if set(prefix).intersection("bBfF"):
- 555 |             return False
-    ::: /tmp/mypy_primer/projects/black/src/black/strings.py:89:23
-  89 | def get_string_prefix(string: str) -> str:
-     |                       ----------- info: parameter declared in function definition here
-  90 |     """
-  91 |     Pre-conditions:
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:791:46
- 789 | def is_multiline_string(node: LN) -> bool:
- 790 |     """Return True if `leaf` is a multiline string that actually spans many lines."""
- 791 |     if isinstance(node, Node) and is_fstring(node):
-     |                                              ^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`node`) of function `is_fstring`; expected type `Node`
- 792 |         leaf = fstring_to_string(node)
- 793 |     elif isinstance(node, Leaf):
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:776:16
- 776 | def is_fstring(node: Node) -> bool:
-     |                ---------- info: parameter declared in function definition here
- 777 |     """Return True if the node is an f-string"""
- 778 |     return node.type == syms.fstring
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:792:34
- 790 |     """Return True if `leaf` is a multiline string that actually spans many lines."""
- 791 |     if isinstance(node, Node) and is_fstring(node):
- 792 |         leaf = fstring_to_string(node)
-     |                                  ^^^^ Object of type `@Todo & Node` cannot be assigned to parameter 1 (`node`) of function `fstring_to_string`; expected type `Node`
- 793 |     elif isinstance(node, Leaf):
- 794 |         leaf = node
-    ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:781:23
- 781 | def fstring_to_string(node: Node) -> Leaf:
-     |                       ---------- info: parameter declared in function definition here
- 782 |     """Converts an fstring node back to a string node."""
- 783 |     string_without_prefix = str(node)[len(node.prefix) :]
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:798:30
- 796 |         return False
- 797 |
- 798 |     return has_triple_quotes(leaf.value) and "\n" in leaf.value
-     |                              ^^^^^^^^^^ Object of type `str | @Todo & str` cannot be assigned to parameter 1 (`string`) of function `has_triple_quotes`; expected type `str`
-    ::: /tmp/mypy_primer/projects/black/src/black/strings.py:38:23
-  38 | def has_triple_quotes(string: str) -> bool:
-     |                       ----------- info: parameter declared in function definition here
-  39 |     """
-  40 |     Returns:
-    --> /tmp/mypy_primer/projects/black/src/black/nodes.py:961:25
- 959 |     new_child = Node(syms.atom, [lpar, child, rpar])
- 960 |     new_child.prefix = prefix
- 961 |     parent.insert_child(index, new_child)
-     |                         ^^^^^ Object of type `@Todo & ~AlwaysFalsy | Literal[0]` cannot be assigned to parameter 2 (`i`) of bound method `insert_child`; expected type `int`
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:335:28
- 333 |         self.invalidate_sibling_maps()
- 334 |
- 335 |     def insert_child(self, i: int, child: NL) -> None:
-     |                            ------ info: parameter declared in function definition here
- 336 |         """
- 337 |         Equivalent to 'node.children.insert(i, child)'. This method also sets
-      |
-      |
-      |
-      |
-      |
-      |
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:222:63
- 221 |                 elif isinstance(item, ast.AST):
- 222 |                     yield from _stringify_ast_with_new_parent(item, parent_stack, node)
-     |                                                               ^^^^ Object of type `@Todo & AST` cannot be assigned to parameter 1 (`node`) of function `_stringify_ast_with_new_parent`; expected type `AST`
- 223 |
- 224 |         elif isinstance(value, ast.AST):
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:175:5
- 174 | def _stringify_ast_with_new_parent(
- 175 |     node: ast.AST, parent_stack: list[ast.AST], new_parent: ast.AST
-     |     ------------- info: parameter declared in function definition here
- 176 | ) -> Iterator[str]:
- 177 |     parent_stack.append(new_parent)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:225:55
- 224 |         elif isinstance(value, ast.AST):
- 225 |             yield from _stringify_ast_with_new_parent(value, parent_stack, node)
-     |                                                       ^^^^^ Object of type `@Todo & AST & ~list` cannot be assigned to parameter 1 (`node`) of function `_stringify_ast_with_new_parent`; expected type `AST`
- 226 |
- 227 |         else:
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:175:5
- 174 | def _stringify_ast_with_new_parent(
- 175 |     node: ast.AST, parent_stack: list[ast.AST], new_parent: ast.AST
-     |     ------------- info: parameter declared in function definition here
- 176 | ) -> Iterator[str]:
- 177 |     parent_stack.append(new_parent)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:241:47
- 239 |                 # docstrings; fold spaces after newlines when comparing. Similarly,
- 240 |                 # trailing and leading space may be removed.
- 241 |                 normalized = _normalize("\n", value)
-     |                                               ^^^^^ Object of type `@Todo & str & ~list & ~AST` cannot be assigned to parameter 2 (`value`) of function `_normalize`; expected type `str`
- 242 |             elif field == "type_comment" and isinstance(value, str):
- 243 |                 # Trailing whitespace in type comments is removed.
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:159:30
- 159 | def _normalize(lineend: str, value: str) -> str:
-     |                              ---------- info: parameter declared in function definition here
- 160 |     # To normalize, we strip any leading and trailing space from
- 161 |     # each line...
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/ranges.py:403:58
- 401 |         node = node_or_nodes
- 402 |         if isinstance(node, Leaf):
- 403 |             return set(range(node.lineno, _leaf_line_end(node) + 1))
-     |                                                          ^^^^ Object of type `@Todo & Leaf & ~list` cannot be assigned to parameter 1 (`leaf`) of function `_leaf_line_end`; expected type `Leaf`
- 404 |         else:
- 405 |             first = first_leaf(node)
-    ::: /tmp/mypy_primer/projects/black/src/black/ranges.py:377:20
- 377 | def _leaf_line_end(leaf: Leaf) -> int:
-     |                    ---------- info: parameter declared in function definition here
- 378 |     """Returns the line number of the leaf node's last line."""
- 379 |     if leaf.type == NEWLINE:
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/linegen.py:396:31
- 394 |             rpar = Leaf(token.RPAR, ")")
- 395 |             index = operand.remove() or 0
- 396 |             node.insert_child(index, Node(syms.atom, [lpar, operand, rpar]))
-     |                               ^^^^^ Object of type `Unknown & ~AlwaysFalsy | @Todo & ~AlwaysFalsy | Literal[0]` cannot be assigned to parameter 2 (`i`) of bound method `insert_child`; expected type `int`
- 397 |         yield from self.visit_default(node)
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:335:28
- 333 |         self.invalidate_sibling_maps()
- 334 |
- 335 |     def insert_child(self, i: int, child: NL) -> None:
-     |                            ------ info: parameter declared in function definition here
- 336 |         """
- 337 |         Equivalent to 'node.children.insert(i, child)'. This method also sets
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:809:28
-      |
-  808 |     head = bracket_split_build_line(
-  809 |         head_leaves, line, matching_bracket, component=_BracketSplitComponent.head
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  810 |     )
-  811 |     body = bracket_split_build_line(
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
- 1154 |     opening_bracket: Leaf,
-      |     --------------------- info: parameter declared in function definition here
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-  810 |     )
-  811 |     body = bracket_split_build_line(
-      |
-      |
- 1154 |     opening_bracket: Leaf,
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:812:28
-      |
-  812 |         body_leaves, line, matching_bracket, component=_BracketSplitComponent.body
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  813 |     )
-  814 |     tail = bracket_split_build_line(
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
- 1154 |     opening_bracket: Leaf,
-      |     --------------------- info: parameter declared in function definition here
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-  813 |     )
-  814 |     tail = bracket_split_build_line(
-      |
-      |
- 1154 |     opening_bracket: Leaf,
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:815:28
-      |
-  815 |         tail_leaves, line, matching_bracket, component=_BracketSplitComponent.tail
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  816 |     )
-  817 |     bracket_split_succeeded_or_raise(head, body, tail)
-    lid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:225:55
- 224 |         elif isinstance(value, ast.AST):
- 225 |             yield from _stringify_ast_with_new_parent(value, parent_stack, node)
-     |                                                       ^^^^^ Object of type `@Todo & AST & ~list` cannot be assigned to parameter 1 (`node`) of function `_stringify_ast_with_new_parent`; expected type `AST`
- 226 |
- 227 |         else:
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:175:5
- 174 | def _stringify_ast_with_new_parent(
- 175 |     node: ast.AST, parent_stack: list[ast.AST], new_parent: ast.AST
-     |     ------------- info: parameter declared in function definition here
- 176 | ) -> Iterator[str]:
- 177 |     parent_stack.append(new_parent)
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/parsing.py:241:47
- 239 |                 # docstrings; fold spaces after newlines when comparing. Similarly,
- 240 |                 # trailing and leading space may be removed.
- 241 |                 normalized = _normalize("\n", value)
-     |                                               ^^^^^ Object of type `@Todo & str & ~list & ~AST` cannot be assigned to parameter 2 (`value`) of function `_normalize`; expected type `str`
- 242 |             elif field == "type_comment" and isinstance(value, str):
- 243 |                 # Trailing whitespace in type comments is removed.
-    ::: /tmp/mypy_primer/projects/black/src/black/parsing.py:159:30
- 159 | def _normalize(lineend: str, value: str) -> str:
-     |                              ---------- info: parameter declared in function definition here
- 160 |     # To normalize, we strip any leading and trailing space from
- 161 |     # each line...
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/ranges.py:403:58
- 401 |         node = node_or_nodes
- 402 |         if isinstance(node, Leaf):
- 403 |             return set(range(node.lineno, _leaf_line_end(node) + 1))
-     |                                                          ^^^^ Object of type `@Todo & Leaf & ~list` cannot be assigned to parameter 1 (`leaf`) of function `_leaf_line_end`; expected type `Leaf`
- 404 |         else:
- 405 |             first = first_leaf(node)
-    ::: /tmp/mypy_primer/projects/black/src/black/ranges.py:377:20
- 377 | def _leaf_line_end(leaf: Leaf) -> int:
-     |                    ---------- info: parameter declared in function definition here
- 378 |     """Returns the line number of the leaf node's last line."""
- 379 |     if leaf.type == NEWLINE:
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
-    --> /tmp/mypy_primer/projects/black/src/black/linegen.py:396:31
- 394 |             rpar = Leaf(token.RPAR, ")")
- 395 |             index = operand.remove() or 0
- 396 |             node.insert_child(index, Node(syms.atom, [lpar, operand, rpar]))
-     |                               ^^^^^ Object of type `Unknown & ~AlwaysFalsy | @Todo & ~AlwaysFalsy | Literal[0]` cannot be assigned to parameter 2 (`i`) of bound method `insert_child`; expected type `int`
- 397 |         yield from self.visit_default(node)
-    ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:335:28
- 333 |         self.invalidate_sibling_maps()
- 334 |
- 335 |     def insert_child(self, i: int, child: NL) -> None:
-     |                            ------ info: parameter declared in function definition here
- 336 |         """
- 337 |         Equivalent to 'node.children.insert(i, child)'. This method also sets
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:809:28
-      |
-  808 |     head = bracket_split_build_line(
-  809 |         head_leaves, line, matching_bracket, component=_BracketSplitComponent.head
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  810 |     )
-  811 |     body = bracket_split_build_line(
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
- 1154 |     opening_bracket: Leaf,
-      |     --------------------- info: parameter declared in function definition here
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-  810 |     )
-  811 |     body = bracket_split_build_line(
-      |
-      |
- 1154 |     opening_bracket: Leaf,
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:812:28
-      |
-  812 |         body_leaves, line, matching_bracket, component=_BracketSplitComponent.body
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  813 |     )
-  814 |     tail = bracket_split_build_line(
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
- 1154 |     opening_bracket: Leaf,
-      |     --------------------- info: parameter declared in function definition here
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-  813 |     )
-  814 |     tail = bracket_split_build_line(
-      |
-      |
- 1154 |     opening_bracket: Leaf,
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:815:28
-      |
-  815 |         tail_leaves, line, matching_bracket, component=_BracketSplitComponent.tail
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  816 |     )
-  817 |     bracket_split_succeeded_or_raise(head, body, tail)
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
- 1154 |     opening_bracket: Leaf,
-      |     --------------------- info: parameter declared in function definition here
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-      |
-      |
- 1154 |     opening_bracket: Leaf,
- 1155 |     *,
- 1156 |     component: _BracketSplitComponent,
-      |
- error: lint:invalid-argument-type
-      |
-      |
-      |
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:933:28
-      |
-  932 |     head = bracket_split_build_line(
-  933 |         head_leaves, line, opening_bracket, component=_BracketSplitComponent.head
-      |                            ^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  934 |     )
-  935 |     if body is None:
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
-      |     --------------------- info: parameter declared in function definition here
-      |
- error: lint:invalid-argument-type
-      |
-  935 |     if body is None:
-      |
-      |
-      |
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:937:32
-      |
-  936 |         body = bracket_split_build_line(
-  937 |             body_leaves, line, opening_bracket, component=_BracketSplitComponent.body
-      |                                ^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  938 |         )
-  939 |     tail = bracket_split_build_line(
-      |
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
-      |
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
-      |     --------------------- info: parameter declared in function definition here
-      |
- error: lint:invalid-argument-type
-  938 |         )
-  939 |     tail = bracket_split_build_line(
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:940:28
-  940 |         tail_leaves, line, opening_bracket, component=_BracketSplitComponent.tail
-      |                            ^^^^^^^^^^^^^^^ Object of type `@Todo & ~AlwaysFalsy` cannot be assigned to parameter 3 (`opening_bracket`) of function `bracket_split_build_line`; expected type `Leaf`
-  941 |     )
-  942 |     bracket_split_succeeded_or_raise(head, body, tail)
-     ::: /tmp/mypy_primer/projects/black/src/black/linegen.py:1154:5
- 1152 |     leaves: list[Leaf],
- 1153 |     original: Line,
-      |     --------------------- info: parameter declared in function definition here
- error: lint:invalid-argument-type
- error: lint:invalid-argument-type
-     --> /tmp/mypy_primer/projects/black/src/black/linegen.py:1139:28
- 1137 |         return True
- 1138 |     # Don't add commas inside parenthesized return annotations
- 1139 |     if get_annotation_type(leaf_with_parent) == "return":
-      |                            ^^^^^^^^^^^^^^^^ Object of type `@Todo & ~None` cannot be assigned to parameter 1 (`leaf`) of function `get_annotation_type`; expected type `Leaf`
- 1140 |         return False
- 1141 |     # Don't add commas inside PEP 604 unions
-     ::: /tmp/mypy_primer/projects/black/src/black/nodes.py:1006:25
- 1006 | def get_annotation_type(leaf: Leaf) -> Literal["return", "param", None]:
-      |                         ---------- info: parameter declared in function definition here
- 1007 |     """Returns the type of annotation this leaf is part of, if any."""
- 1008 |     ancestor = leaf.parent
- error: lint:i...*[Comment body truncated]*

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:292 on 2025-03-10 23:58_

Nice work on figuring out the real root cause here!

I think, however, that as written these errors are correct. We cannot say that `Unrelated` is disjoint from `tuple` (even though it doesn't itself inherit `tuple`) because there may be a subclass of `Unrelated` that also multiply-inherits `tuple`. We could only say this if we also know that `Unrelated` is `final`.

This is already implemented correctly by `is_disjoint_from`, and also tested in its tests. Since these examples aren't demonstrating a behavior that is specific to `is_assignable_to`, I think we should just remove these examples from this test.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:268 on 2025-03-11 00:06_

This assertion is pre-existing, and wasn't labeled with a TODO, but it is wrong (which I'm sure made things confusing in terms of the desired behavior). `Any` can materialize to any type, which means that `Any & Parent` could be `Never & Parent` (that is, `Never`), or it could be `Unrelated & Parent`. Both of those are assignable to `Unrelated`. So `Any & Parent` should be assignable to `Unrelated`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:276 on 2025-03-11 00:07_

This is a duplicate of the line commented above, and is also wrong; this should be assignable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:272 on 2025-03-11 00:08_

This comment isn't right. Intersection with `Any` is not ignored, rather it dominates, in the sense that `Any & T` is assignable to any type, regardless of `T`. (`Any & T` is even assignable to a type `S` that is disjoint from `T`, because `Any` could represent `S`, and `S & T` is `Never`, which is assignable to `S`.)
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:286 on 2025-03-11 00:10_

Similar to the above cases, this should be assignable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:826 on 2025-03-11 00:12_

As discussed above, the special handling of dynamic types here is not correct. I think the right behavior falls out from just simplifying this code to eliminate that special case.
```suggestion
            // Any element of S is assignable to T (e.g. `int & Any` is assignable to `int` (but not to `str`))
            // Negative elements do not have an effect on assignability - if S is assignable to T then S & ~P is also assignable to T.
            (Type::Intersection(intersection), ty) => {
                intersection.positive(db).iter().any(|&elem_ty| {
                    elem_ty.is_assignable_to(db, ty)
                })
            }
```

---

_@carljm reviewed on 2025-03-11 00:12_

Nice work, and thank you for the contribution!

This mostly looks right, I think there's just one area that needs tweaking.

---

_Review comment by @jgeralnik on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:268 on 2025-03-11 05:08_

I'm not 100% sure I agree. Take the following code:

```
def f(x: str): ...

def g(x: Any):
    if isinstance(x, str):
         f(x)

def h(x: Any):
    if isinstance(x, int):
        f(x)
```

The goal of this PR is to solve the call in g, which is done because we allow assigning str & Any to str.

Should the call in h also succeed or should we raise an error?

I can see the argument you're making - just because x is an int doesn't mean it's not a str, so in theory a caller could pass an str & int into h, and since h is untyped we can't assume what arguments it gets.

On the other hand, the more helpful thing to the enduser is probably to effectively assume that x is an int here and not allow the assignment, since it's almost certainly incorrect.

For what it's worth mypy seems to agree with me, and raises an error here:

```
error: Argument 1 to "f" has incompatible type "int"; expected "str"  [arg-type]
```

---

_@jgeralnik reviewed on 2025-03-11 05:08_

---

_@carljm reviewed on 2025-03-11 05:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:268 on 2025-03-11 05:22_

Technically, the call in `h` should fail, once we add the ability (which we don't yet have) to recognize that certain C extension or builtin types (or Python types with `__slots__`) can't be multiply-inherited together at runtime due to incompatible memory layout. 

But with two arbitrary types `A` and `B` (which are neither final nor use `__slots__`) in place of `int` and `str`, the call in `h` should succeed. That's simply the nature of `Any`; there's no consistent theoretical basis for any other behavior. Not implementing this assignability breaks the gradual guarantee (which is a foundational -- perhaps the foundational -- principle of gradual typing): that replacing a static type annotation with `Any` should not cause new type errors to appear. It's easy to see that in the following code the gradual guarantee would be broken:

```py
class A: pass
class B: pass

def f(x: B):
    pass

def h(x: B):
    if isinstance(x, A):
        f(x)
```

This code should type-check as-is, because `A & B` is assignable to `B`. If we don't consistently implement `Any` in intersections, replacing the annotation `x: B` with `x: Any` would cause a new type error to appear. This violates the gradual guarantee.

Mypy does not really implement intersection types, its narrowing behavior is more ad-hoc, and it has decided to incorrectly eliminate `Any` in intersections, but we are not going to follow that behavior.

---

_@jgeralnik reviewed on 2025-03-11 05:30_

---

_Review comment by @jgeralnik on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:268 on 2025-03-11 05:30_

Alright, makes sense as a design philosophy. The alternative philosophy is "provide as much value as reasonably possible to a codebase without typing" but I can understand why it makes sense to optimize on soundness.

Will update the PR

---

_Comment by @codspeed-hq[bot] on 2025-03-11 06:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jgeralnik%3Ajoey%2Fassign_intersection_any)

### Merging #16611 will **improve performances by 14.88%**

<sub>Comparing <code>jgeralnik:joey/assign_intersection_any</code> (fee7230) with <code>main</code> (da069aa)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.5 ms | 4.8 ms | +14.88% |


---

_Comment by @jgeralnik on 2025-03-11 06:11_

Fixed the code, which also fixed a diagnostic we were raising in ruff_benchmark. This fixed diagnostic completely convinced me that you are right about the desired behavior, by the way - the code was effectively:

```
def f(x: str): ...

x: Any
if x:
    reveal_type(x) # Any & ~AlwaysFalsy
    f(x)
```

which my original version incorrectly typed as effectively equivalent to ~AlwaysFalsy and failed on because ~AlwaysFalsy is not assignable to str.

---

_Comment by @github-actions[bot] on 2025-03-11 06:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2025-03-11 14:58_

Awesome, thank you so much for the PR!

---

_Merged by @carljm on 2025-03-11 14:58_

---

_Closed by @carljm on 2025-03-11 14:58_

---
