```yaml
number: 9564
title: "Add a rule/autofix to sort `__slots__` and `__match_args__`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: sort-dunder-slots
created_at: 2024-01-17T15:06:26Z
updated_at: 2024-01-22T12:32:36Z
url: https://github.com/astral-sh/ruff/pull/9564
synced_at: 2026-01-12T15:55:29Z
```

# Add a rule/autofix to sort `__slots__` and `__match_args__`

---

_@AlexWaygood_

## Summary

This PR introduces a new rule to sort `__slots__` and `__match_args__` according to a [natural sort](https://en.wikipedia.org/wiki/Natural_sort_order), as was requested in https://github.com/astral-sh/ruff/issues/1198#issuecomment-1881418365.

The implementation here generalises some of the machinery introduced in https://github.com/astral-sh/ruff/commit/3aae16f1bdef51e2f64ea4e9e64f9201ad4d9020 so that different kinds of sorts can be applied to lists of string literals. (We use an "isort-style" sort for `__all__`, but that isn't really appropriate for `__slots__` and `__match_args__`, where nearly all items will be snake_case.) Several sections of code have been moved from `sort_dunder_all.rs` to a new module, `sorting_helpers.rs`, which `sort_dunder_all.rs` and `sort_dunder_slots.rs` both make use of.

`__match_args__` is very similar to `__all__`, in that it can only be a tuple or a list. `__slots__` differs from the other two, however, in that it can be any iterable of strings. If slots is a dictionary, the values are used by the builtin `help()` function as per-attribute docstrings that show up in the output of `help()`. (There's no particular use-case for making `__slots__` a set, but it's perfectly legal at runtime, so there's no reason for us not to handle it in this rule.)

Currently I've only implemented an autofix for `__match_args__` and `__slots__` if the definition is on a single line. That's for two reasons:

1. I wanted to see what people thought of this before going any further!
2. Multiline `__slots__` and `__match_args__` definitions are much rarer than multiline `__all__` definitions, so there's probably less value in implementing the autofix for those.

Implementing an autofix for multiline `__match_args__` wouldn't be too hard: I'd just have to move more code from `sort_dunder_all.rs` to `sorting_helpers.rs`. The same is true for `__slots__` when it's a tuple, list or set. I don't feel enthused about implementing an autofix for multiline `__slots__` definitions where `__slots__` is a dict, however. That would require a lot of extra code, and using a dict for `__slots__` is pretty rare.

If we're interested in autofixes for multiline `__slots__` and `__match_args__` definitions, let me know whether you'd prefer it in this PR or a followup!

## Test Plan

`cargo test` / `cargo insta review`.

I also ran this rule on CPython. The autofix fixed 45 violations:

<details>
<summary>Diff from the autofix</summary>

```diff
diff --git a/Lib/_pydatetime.py b/Lib/_pydatetime.py
index bca2acf1fc..657544aa47 100644
--- a/Lib/_pydatetime.py
+++ b/Lib/_pydatetime.py
@@ -600,7 +600,7 @@ class timedelta:
     # arbitrarily; the exact rationale originally specified in the docstring
     # was "Because I felt like it."
 
-    __slots__ = '_days', '_seconds', '_microseconds', '_hashcode'
+    __slots__ = '_days', '_hashcode', '_microseconds', '_seconds'
 
     def __new__(cls, days=0, seconds=0, microseconds=0,
                 milliseconds=0, minutes=0, hours=0, weeks=0):
@@ -931,7 +931,7 @@ class date:
     Properties (readonly):
     year, month, day
     """
-    __slots__ = '_year', '_month', '_day', '_hashcode'
+    __slots__ = '_day', '_hashcode', '_month', '_year'
 
     def __new__(cls, year, month=None, day=None):
         """Constructor.
@@ -1346,7 +1346,7 @@ class time:
     Properties (readonly):
     hour, minute, second, microsecond, tzinfo, fold
     """
-    __slots__ = '_hour', '_minute', '_second', '_microsecond', '_tzinfo', '_hashcode', '_fold'
+    __slots__ = '_fold', '_hashcode', '_hour', '_microsecond', '_minute', '_second', '_tzinfo'
 
     def __new__(cls, hour=0, minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0):
         """Constructor.
@@ -2328,7 +2328,7 @@ def _isoweek1monday(year):
 
 
 class timezone(tzinfo):
-    __slots__ = '_offset', '_name'
+    __slots__ = '_name', '_offset'
 
     # Sentinel value to disallow None
     _Omitted = object()
diff --git a/Lib/_pydecimal.py b/Lib/_pydecimal.py
index 2692f2fcba..cd7929407d 100644
--- a/Lib/_pydecimal.py
+++ b/Lib/_pydecimal.py
@@ -523,7 +523,7 @@ def sin(x):
 class Decimal(object):
     """Floating point class for decimal arithmetic."""
 
-    __slots__ = ('_exp','_int','_sign', '_is_special')
+    __slots__ = ('_exp', '_int', '_is_special', '_sign')
     # Generally, the value of the Decimal instance is given by
     #  (-1)**_sign * _int * 10**_exp
     # Special values are signified by _is_special == True
@@ -5626,7 +5626,7 @@ def to_integral_value(self, a):
     to_integral = to_integral_value
 
 class _WorkRep(object):
-    __slots__ = ('sign','int','exp')
+    __slots__ = ('exp', 'int', 'sign')
     # sign: 0 or 1
     # int:  int
     # exp:  None, int, or string
diff --git a/Lib/_threading_local.py b/Lib/_threading_local.py
index b006d76c4e..3acba9e055 100644
--- a/Lib/_threading_local.py
+++ b/Lib/_threading_local.py
@@ -145,7 +145,7 @@
 
 class _localimpl:
     """A class managing thread-local dicts"""
-    __slots__ = 'key', 'dicts', 'localargs', 'locallock', '__weakref__'
+    __slots__ = '__weakref__', 'dicts', 'key', 'localargs', 'locallock'
 
     def __init__(self):
         # The key used in the Thread objects' attribute dicts.
@@ -202,7 +202,7 @@ def _patch(self):
 
 
 class local:
-    __slots__ = '_local__impl', '__dict__'
+    __slots__ = '__dict__', '_local__impl'
 
     def __new__(cls, /, *args, **kw):
         if (args or kw) and (cls.__init__ is object.__init__):
diff --git a/Lib/asyncio/transports.py b/Lib/asyncio/transports.py
index 30fd41d49a..f3e641cb53 100644
--- a/Lib/asyncio/transports.py
+++ b/Lib/asyncio/transports.py
@@ -265,7 +265,7 @@ class _FlowControlMixin(Transport):
     resume_writing() may be called.
     """
 
-    __slots__ = ('_loop', '_protocol_paused', '_high_water', '_low_water')
+    __slots__ = ('_high_water', '_loop', '_low_water', '_protocol_paused')
 
     def __init__(self, extra=None, loop=None):
         super().__init__(extra)
diff --git a/Lib/collections/__init__.py b/Lib/collections/__init__.py
index 2e527dfd81..62f76b903d 100644
--- a/Lib/collections/__init__.py
+++ b/Lib/collections/__init__.py
@@ -78,7 +78,7 @@ def __reversed__(self):
             yield self._mapping[key]
 
 class _Link(object):
-    __slots__ = 'prev', 'next', 'key', '__weakref__'
+    __slots__ = '__weakref__', 'key', 'next', 'prev'
 
 class OrderedDict(dict):
     'Dictionary that remembers insertion order'
diff --git a/Lib/fractions.py b/Lib/fractions.py
index 389ab386b6..f4f524d66e 100644
--- a/Lib/fractions.py
+++ b/Lib/fractions.py
@@ -197,7 +197,7 @@ class Fraction(numbers.Rational):
 
     """
 
-    __slots__ = ('_numerator', '_denominator')
+    __slots__ = ('_denominator', '_numerator')
 
     # We're immutable, so use __new__ not __init__
     def __new__(cls, numerator=0, denominator=None):
diff --git a/Lib/functools.py b/Lib/functools.py
index 55990e742b..a936d85185 100644
--- a/Lib/functools.py
+++ b/Lib/functools.py
@@ -279,7 +279,7 @@ class partial:
     and keywords.
     """
 
-    __slots__ = "func", "args", "keywords", "__dict__", "__weakref__"
+    __slots__ = "__dict__", "__weakref__", "args", "func", "keywords"
 
     def __new__(cls, func, /, *args, **keywords):
         if not callable(func):
diff --git a/Lib/inspect.py b/Lib/inspect.py
index f0b72662a9..d27ff1a2fc 100644
--- a/Lib/inspect.py
+++ b/Lib/inspect.py
@@ -2751,7 +2751,7 @@ class Parameter:
         `Parameter.KEYWORD_ONLY`, `Parameter.VAR_KEYWORD`.
     """
 
-    __slots__ = ('_name', '_kind', '_default', '_annotation')
+    __slots__ = ('_annotation', '_default', '_kind', '_name')
 
     POSITIONAL_ONLY         = _POSITIONAL_ONLY
     POSITIONAL_OR_KEYWORD   = _POSITIONAL_OR_KEYWORD
@@ -2906,7 +2906,7 @@ class BoundArguments:
         Dict of keyword arguments values.
     """
 
-    __slots__ = ('arguments', '_signature', '__weakref__')
+    __slots__ = ('__weakref__', '_signature', 'arguments')
 
     def __init__(self, signature, arguments):
         self.arguments = arguments
@@ -3042,7 +3042,7 @@ class Signature:
         to parameters (simulating 'functools.partial' behavior.)
     """
 
-    __slots__ = ('_return_annotation', '_parameters')
+    __slots__ = ('_parameters', '_return_annotation')
 
     _parameter_cls = Parameter
     _bound_arguments_cls = BoundArguments
diff --git a/Lib/ipaddress.py b/Lib/ipaddress.py
index e398cc1383..e7e6e20f79 100644
--- a/Lib/ipaddress.py
+++ b/Lib/ipaddress.py
@@ -1277,7 +1277,7 @@ class IPv4Address(_BaseV4, _BaseAddress):
 
     """Represent and manipulate single IPv4 Addresses."""
 
-    __slots__ = ('_ip', '__weakref__')
+    __slots__ = ('__weakref__', '_ip')
 
     def __init__(self, address):
 
@@ -1891,7 +1891,7 @@ class IPv6Address(_BaseV6, _BaseAddress):
 
     """Represent and manipulate single IPv6 Addresses."""
 
-    __slots__ = ('_ip', '_scope_id', '__weakref__')
+    __slots__ = ('__weakref__', '_ip', '_scope_id')
 
     def __init__(self, address):
         """Instantiate a new IPv6 address object.
diff --git a/Lib/multiprocessing/managers.py b/Lib/multiprocessing/managers.py
index 76b915de74..60d6c622c1 100644
--- a/Lib/multiprocessing/managers.py
+++ b/Lib/multiprocessing/managers.py
@@ -63,7 +63,7 @@ class Token(object):
     '''
     Type to uniquely identify a shared object
     '''
-    __slots__ = ('typeid', 'address', 'id')
+    __slots__ = ('address', 'id', 'typeid')
 
     def __init__(self, typeid, address, id):
         (self.typeid, self.address, self.id) = (typeid, address, id)
diff --git a/Lib/operator.py b/Lib/operator.py
index 30116c1189..2f7371f42a 100644
--- a/Lib/operator.py
+++ b/Lib/operator.py
@@ -274,7 +274,7 @@ class itemgetter:
     After f = itemgetter(2), the call f(r) returns r[2].
     After g = itemgetter(2, 5, 3), the call g(r) returns (r[2], r[5], r[3])
     """
-    __slots__ = ('_items', '_call')
+    __slots__ = ('_call', '_items')
 
     def __init__(self, item, *items):
         if not items:
@@ -306,7 +306,7 @@ class methodcaller:
     After g = methodcaller('name', 'date', foo=1), the call g(r) returns
     r.name('date', foo=1).
     """
-    __slots__ = ('_name', '_args', '_kwargs')
+    __slots__ = ('_args', '_kwargs', '_name')
 
     def __init__(self, name, /, *args, **kwargs):
         self._name = name
diff --git a/Lib/pathlib/__init__.py b/Lib/pathlib/__init__.py
index f14d35bb00..7dbf627702 100644
--- a/Lib/pathlib/__init__.py
+++ b/Lib/pathlib/__init__.py
@@ -45,7 +45,7 @@
 class _PathParents(Sequence):
     """This object provides sequence-like access to the logical ancestors
     of a path.  Don't try to construct it yourself."""
-    __slots__ = ('_path', '_drv', '_root', '_tail')
+    __slots__ = ('_drv', '_path', '_root', '_tail')
 
     def __init__(self, path):
         self._path = path
diff --git a/Lib/socket.py b/Lib/socket.py
index 77986fc2e4..909a881808 100644
--- a/Lib/socket.py
+++ b/Lib/socket.py
@@ -216,7 +216,7 @@ class socket(_socket.socket):
 
     """A subclass of _socket.socket adding the makefile() method."""
 
-    __slots__ = ["__weakref__", "_io_refs", "_closed"]
+    __slots__ = ["__weakref__", "_closed", "_io_refs"]
 
     def __init__(self, family=-1, type=-1, proto=-1, fileno=None):
         # For user code address family and type values are IntEnum members, but
diff --git a/Lib/test/datetimetester.py b/Lib/test/datetimetester.py
index 8bda17358d..1d84157323 100644
--- a/Lib/test/datetimetester.py
+++ b/Lib/test/datetimetester.py
@@ -150,7 +150,7 @@ def __init__(self, offset=None, name=None, dstoffset=None):
         FixedOffset.__init__(self, offset, name, dstoffset)
 
 class PicklableFixedOffsetWithSlots(PicklableFixedOffset):
-    __slots__ = '_FixedOffset__offset', '_FixedOffset__name', 'spam'
+    __slots__ = '_FixedOffset__name', '_FixedOffset__offset', 'spam'
 
 class _TZInfo(tzinfo):
     def utcoffset(self, datetime_module):
diff --git a/Lib/test/test_binop.py b/Lib/test/test_binop.py
index 299af09c49..19fafe865d 100644
--- a/Lib/test/test_binop.py
+++ b/Lib/test/test_binop.py
@@ -29,7 +29,7 @@ class Rat(object):
 
     """Rational number implemented as a normalized pair of ints."""
 
-    __slots__ = ['_Rat__num', '_Rat__den']
+    __slots__ = ['_Rat__den', '_Rat__num']
 
     def __init__(self, num=0, den=1):
         """Constructor: Rat([num[, den]]).
diff --git a/Lib/test/test_bytes.py b/Lib/test/test_bytes.py
index a3804a945f..50f162cb2b 100644
--- a/Lib/test/test_bytes.py
+++ b/Lib/test/test_bytes.py
@@ -2092,7 +2092,7 @@ class ByteArraySubclass(bytearray):
     pass
 
 class ByteArraySubclassWithSlots(bytearray):
-    __slots__ = ('x', 'y', '__dict__')
+    __slots__ = ('__dict__', 'x', 'y')
 
 class BytesSubclass(bytes):
     pass
diff --git a/Lib/test/test_deque.py b/Lib/test/test_deque.py
index ae1dfacd72..1d6199e3f4 100644
--- a/Lib/test/test_deque.py
+++ b/Lib/test/test_deque.py
@@ -782,7 +782,7 @@ class Deque(deque):
     pass
 
 class DequeWithSlots(deque):
-    __slots__ = ('x', 'y', '__dict__')
+    __slots__ = ('__dict__', 'x', 'y')
 
 class DequeWithBadIter(deque):
     def __iter__(self):
diff --git a/Lib/test/test_gc.py b/Lib/test/test_gc.py
index 1d71dd9e26..6b5ff082f6 100644
--- a/Lib/test/test_gc.py
+++ b/Lib/test/test_gc.py
@@ -1042,7 +1042,7 @@ def test_trash_weakref_clear(self):
         callback = unittest.mock.Mock()
 
         class A:
-            __slots__ = ['a', 'y', 'wz']
+            __slots__ = ['a', 'wz', 'y']
 
         class Z:
             pass
diff --git a/Lib/test/test_patma.py b/Lib/test/test_patma.py
index 298e78ccee..ab5b856991 100644
--- a/Lib/test/test_patma.py
+++ b/Lib/test/test_patma.py
@@ -3303,7 +3303,7 @@ class Class:
 
     def test_match_args_must_be_a_tuple_2(self):
         class Class:
-            __match_args__ = ["spam", "eggs"]
+            __match_args__ = ["eggs", "spam"]
             spam = 0
             eggs = 1
         x = Class()
diff --git a/Lib/test/test_property.py b/Lib/test/test_property.py
index c12c908d2e..cf7d424ea9 100644
--- a/Lib/test/test_property.py
+++ b/Lib/test/test_property.py
@@ -275,7 +275,7 @@ def documented_getter():
     def test_property_with_slots_and_doc_slot_docstring_present(self):
         # https://github.com/python/cpython/issues/98963#issuecomment-1574413319
         class slotted_prop(property):
-            __slots__ = ("foo", "__doc__")
+            __slots__ = ("__doc__", "foo")
 
         p = slotted_prop(doc="what's up")
         self.assertEqual("what's up", p.__doc__)  # new in 3.12: This gets set.
diff --git a/Lib/test/test_set.py b/Lib/test/test_set.py
index d9102eb98a..d19a377b85 100644
--- a/Lib/test/test_set.py
+++ b/Lib/test/test_set.py
@@ -809,7 +809,7 @@ def test_singleton_empty_frozenset(self):
 
 
 class SetSubclassWithSlots(set):
-    __slots__ = ('x', 'y', '__dict__')
+    __slots__ = ('__dict__', 'x', 'y')
 
 class TestSetSubclassWithSlots(unittest.TestCase):
     thetype = SetSubclassWithSlots
@@ -817,7 +817,7 @@ class TestSetSubclassWithSlots(unittest.TestCase):
     test_pickling = TestJointOps.test_pickling
 
 class FrozenSetSubclassWithSlots(frozenset):
-    __slots__ = ('x', 'y', '__dict__')
+    __slots__ = ('__dict__', 'x', 'y')
 
 class TestFrozenSetSubclassWithSlots(TestSetSubclassWithSlots):
     thetype = FrozenSetSubclassWithSlots
diff --git a/Lib/tracemalloc.py b/Lib/tracemalloc.py
index cec99c5970..aca59617cf 100644
--- a/Lib/tracemalloc.py
+++ b/Lib/tracemalloc.py
@@ -32,7 +32,7 @@ class Statistic:
     Statistic difference on memory allocations between two Snapshot instance.
     """
 
-    __slots__ = ('traceback', 'size', 'count')
+    __slots__ = ('count', 'size', 'traceback')
 
     def __init__(self, traceback, size, count):
         self.traceback = traceback
@@ -72,7 +72,7 @@ class StatisticDiff:
     Statistic difference on memory allocations between an old and a new
     Snapshot instance.
     """
-    __slots__ = ('traceback', 'size', 'size_diff', 'count', 'count_diff')
+    __slots__ = ('count', 'count_diff', 'size', 'size_diff', 'traceback')
 
     def __init__(self, traceback, size, size_diff, count, count_diff):
         self.traceback = traceback
diff --git a/Lib/typing.py b/Lib/typing.py
index d278b4effc..85c1b269c2 100644
--- a/Lib/typing.py
+++ b/Lib/typing.py
@@ -448,7 +448,7 @@ def __iter__(self): raise TypeError()
 # Internal indicator of special typing constructs.
 # See __doc__ instance attribute for specific docs.
 class _SpecialForm(_Final, _NotIterable, _root=True):
-    __slots__ = ('_name', '__doc__', '_getitem')
+    __slots__ = ('__doc__', '_getitem', '_name')
 
     def __init__(self, getitem):
         self._getitem = getitem
diff --git a/Lib/uuid.py b/Lib/uuid.py
index 470bc0d685..4b405c35b9 100644
--- a/Lib/uuid.py
+++ b/Lib/uuid.py
@@ -134,7 +134,7 @@ class UUID:
                     uuid_generate_time_safe(3).
     """
 
-    __slots__ = ('int', 'is_safe', '__weakref__')
+    __slots__ = ('__weakref__', 'int', 'is_safe')
 
     def __init__(self, hex=None, bytes=None, bytes_le=None, fields=None,
                        int=None, version=None,
diff --git a/Lib/weakref.py b/Lib/weakref.py
index 25b70927e2..1d20272ce3 100644
--- a/Lib/weakref.py
+++ b/Lib/weakref.py
@@ -41,7 +41,7 @@ class WeakMethod(ref):
     a bound method, working around the lifetime problem of bound methods.
     """
 
-    __slots__ = "_func_ref", "_meth_type", "_alive", "__weakref__"
+    __slots__ = "__weakref__", "_alive", "_func_ref", "_meth_type"
 
     def __new__(cls, meth, callback=None):
         try:
@@ -563,7 +563,7 @@ class finalize:
     _registered_with_atexit = False
 
     class _Info:
-        __slots__ = ("weakref", "func", "args", "kwargs", "atexit", "index")
+        __slots__ = ("args", "atexit", "func", "index", "kwargs", "weakref")
 
     def __init__(self, obj, func, /, *args, **kwargs):
         if not self._registered_with_atexit:
diff --git a/Lib/xml/dom/expatbuilder.py b/Lib/xml/dom/expatbuilder.py
index 7dd667bf3f..812e00f1b2 100644
--- a/Lib/xml/dom/expatbuilder.py
+++ b/Lib/xml/dom/expatbuilder.py
@@ -509,7 +509,7 @@ def acceptNode(self, node):
 
 
 class FilterCrutch(object):
-    __slots__ = '_builder', '_level', '_old_start', '_old_end'
+    __slots__ = '_builder', '_level', '_old_end', '_old_start'
 
     def __init__(self, builder):
         self._level = 0
diff --git a/Lib/xml/dom/minidom.py b/Lib/xml/dom/minidom.py
index db51f350ea..7aed19c1c2 100644
--- a/Lib/xml/dom/minidom.py
+++ b/Lib/xml/dom/minidom.py
@@ -656,7 +656,7 @@ def __setstate__(self, state):
 
 
 class TypeInfo(object):
-    __slots__ = 'namespace', 'name'
+    __slots__ = 'name', 'namespace'
 
     def __init__(self, namespace, name):
         self.namespace = namespace
@@ -1007,7 +1007,7 @@ def replaceChild(self, newChild, oldChild):
 
 class ProcessingInstruction(Childless, Node):
     nodeType = Node.PROCESSING_INSTRUCTION_NODE
-    __slots__ = ('target', 'data')
+    __slots__ = ('data', 'target')
 
     def __init__(self, target, data):
         self.target = target
@@ -1032,7 +1032,7 @@ def writexml(self, writer, indent="", addindent="", newl=""):
 
 
 class CharacterData(Childless, Node):
-    __slots__=('_data', 'ownerDocument','parentNode', 'previousSibling', 'nextSibling')
+    __slots__=('_data', 'nextSibling', 'ownerDocument', 'parentNode', 'previousSibling')
 
     def __init__(self):
         self.ownerDocument = self.parentNode = None
diff --git a/Lib/zoneinfo/_zoneinfo.py b/Lib/zoneinfo/_zoneinfo.py
index b77dc0ed39..01f1303540 100644
--- a/Lib/zoneinfo/_zoneinfo.py
+++ b/Lib/zoneinfo/_zoneinfo.py
@@ -394,7 +394,7 @@ def _ts_to_local(trans_idx, trans_list_utc, utcoffsets):
 
 
 class _ttinfo:
-    __slots__ = ["utcoff", "dstoff", "tzname"]
+    __slots__ = ["dstoff", "tzname", "utcoff"]
 
     def __init__(self, utcoff, dstoff, tzname):
         self.utcoff = utcoff
@@ -514,7 +514,7 @@ def _post_epoch_days_before_year(year):
 
 
 class _DayOffset:
-    __slots__ = ["d", "julian", "hour", "minute", "second"]
+    __slots__ = ["d", "hour", "julian", "minute", "second"]
 
     def __init__(self, d, julian, hour=2, minute=0, second=0):
         min_day = 0 + julian  # convert bool to int
@@ -541,7 +541,7 @@ def year_to_epoch(self, year):
 
 
 class _CalendarOffset:
-    __slots__ = ["m", "w", "d", "hour", "minute", "second"]
+    __slots__ = ["d", "hour", "m", "minute", "second", "w"]
 
     _DAYS_BEFORE_MONTH = (
         -1,
diff --git a/Tools/c-analyzer/c_common/clsutil.py b/Tools/c-analyzer/c_common/clsutil.py
index aa5f6b9831..8b61321355 100644
--- a/Tools/c-analyzer/c_common/clsutil.py
+++ b/Tools/c-analyzer/c_common/clsutil.py
@@ -9,7 +9,7 @@ class Slot:
     e.g. tuple subclasses.
     """
 
-    __slots__ = ('initial', 'default', 'readonly', 'instances', 'name')
+    __slots__ = ('default', 'initial', 'instances', 'name', 'readonly')
 
     def __init__(self, initial=_NOT_SET, *,
                  default=_NOT_SET,
```

</details>

The following 17 violations were flagged but left unfixed by the rule as it currently stands. (They were unfixed because they're multiline definitions):

```
cpython\Lib\asyncio\events.py:32:17: RUF023 `Handle.__slots__` is not sorted
cpython\Lib\dataclasses.py:296:17: RUF023 `Field.__slots__` is not sorted
cpython\Lib\dataclasses.py:360:17: RUF023 `_DataclassParams.__slots__` is not sorted
cpython\Lib\pathlib\__init__.py:87:17: RUF023 `PurePath.__slots__` is not sorted
cpython\Lib\pickletools.py:175:17: RUF023 `ArgumentDescriptor.__slots__` is not sorted
cpython\Lib\pickletools.py:949:17: RUF023 `StackObject.__slots__` is not sorted
cpython\Lib\pickletools.py:1095:17: RUF023 `OpcodeInfo.__slots__` is not sorted
cpython\Lib\test\test_inspect\test_inspect.py:526:17: RUF023 `SlotUser.__slots__` is not sorted
cpython\Lib\traceback.py:313:17: RUF023 `FrameSummary.__slots__` is not sorted
cpython\Lib\typing.py:857:17: RUF023 `ForwardRef.__slots__` is not sorted
cpython\Lib\xml\dom\minidom.py:362:15: RUF023 `Attr.__slots__` is not sorted
cpython\Lib\xml\dom\minidom.py:681:15: RUF023 `Element.__slots__` is not sorted
cpython\Lib\xml\dom\minidom.py:1563:17: RUF023 `Document.__slots__` is not sorted
cpython\Lib\xml\dom\xmlbuilder.py:257:17: RUF023 `DOMInputSource.__slots__` is not sorted
cpython\Lib\zipfile\__init__.py:377:17: RUF023 `ZipInfo.__slots__` is not sorted
cpython\Lib\zoneinfo\_common.py:128:17: RUF023 `_TZifHeader.__slots__` is not sorted
cpython\Lib\zoneinfo\_zoneinfo.py:422:17: RUF023 `_TZStr.__slots__` is not sorted
```

---

_Label `rule` added by @AlexWaygood on 2024-01-17 15:06_

---

_Comment by @github-actions[bot] on 2024-01-17 15:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+150 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+111 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/abc.py#L201'>disnake/abc.py:201:17:</a> RUF023 [*] `_Overwrites.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/activity.py#L289'>disnake/activity.py:289:17:</a> RUF023 [*] `Activity.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/activity.py#L530'>disnake/activity.py:530:17:</a> RUF023 [*] `Streaming.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/activity.py#L619'>disnake/activity.py:619:17:</a> RUF023 [*] `Spotify.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/activity.py#L803'>disnake/activity.py:803:17:</a> RUF023 [*] `CustomActivity.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/app_commands.py#L1017'>disnake/app_commands.py:1017:17:</a> RUF023 [*] `ApplicationCommandPermissions.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/app_commands.py#L1075'>disnake/app_commands.py:1075:17:</a> RUF023 [*] `GuildApplicationCommandPermissions.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/app_commands.py#L241'>disnake/app_commands.py:241:17:</a> RUF023 [*] `Option.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/appinfo.py#L156'>disnake/appinfo.py:156:17:</a> RUF023 [*] `AppInfo.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/appinfo.py#L297'>disnake/appinfo.py:297:17:</a> RUF023 [*] `PartialAppInfo.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/appinfo.py#L43'>disnake/appinfo.py:43:17:</a> RUF023 [*] `InstallParams.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/application_role_connection.py#L50'>disnake/application_role_connection.py:50:17:</a> RUF023 [*] `ApplicationRoleConnectionMetadata.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/asset.py#L190'>disnake/asset.py:190:34:</a> RUF023 [*] `Asset.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/automod.py#L283'>disnake/automod.py:283:17:</a> RUF023 [*] `AutoModTriggerMetadata.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/automod.py#L455'>disnake/automod.py:455:17:</a> RUF023 [*] `AutoModRule.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/automod.py#L729'>disnake/automod.py:729:17:</a> RUF023 [*] `AutoModActionExecution.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/automod.py#L83'>disnake/automod.py:83:17:</a> RUF023 [*] `AutoModAction.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L1101'>disnake/channel.py:1101:17:</a> RUF023 [*] `VocalGuildChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L1291'>disnake/channel.py:1291:17:</a> RUF023 [*] `VoiceChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L179'>disnake/channel.py:179:17:</a> RUF023 [*] `TextChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L1947'>disnake/channel.py:1947:17:</a> RUF023 [*] `StageChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L2737'>disnake/channel.py:2737:17:</a> RUF023 [*] `CategoryChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L3226'>disnake/channel.py:3226:17:</a> RUF023 [*] `ForumChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L4222'>disnake/channel.py:4222:17:</a> RUF023 [*] `DMChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/channel.py#L4388'>disnake/channel.py:4388:17:</a> RUF023 [*] `GroupChannel.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/client.py#L173'>disnake/client.py:173:34:</a> RUF023 [*] `SessionStartLimit.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/components.py#L189'>disnake/components.py:189:34:</a> RUF023 [*] `Button.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/components.py#L269'>disnake/components.py:269:34:</a> RUF023 [*] `BaseSelectMenu.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/components.py#L535'>disnake/components.py:535:34:</a> RUF023 [*] `SelectOption.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/components.py#L646'>disnake/components.py:646:34:</a> RUF023 [*] `TextInput.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/embeds.py#L170'>disnake/embeds.py:170:17:</a> RUF023 [*] `Embed.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/emoji.py#L76'>disnake/emoji.py:76:34:</a> RUF023 [*] `Emoji.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/entitlement.py#L81'>disnake/entitlement.py:81:17:</a> RUF023 [*] `Entitlement.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/ext/commands/cooldowns.py#L282'>disnake/ext/commands/cooldowns.py:282:17:</a> RUF023 [*] `_Semaphore.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/ext/commands/cooldowns.py#L330'>disnake/ext/commands/cooldowns.py:330:17:</a> RUF023 [*] `MaxConcurrency.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/ext/commands/cooldowns.py#L71'>disnake/ext/commands/cooldowns.py:71:17:</a> RUF023 [*] `Cooldown.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/ext/tasks/__init__.py#L53'>disnake/ext/tasks/__init__.py:53:17:</a> RUF023 [*] `SleepHandle.__slots__` is not sorted
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2b4da0101f0314989d148c3c8a02c87e87048974/airflow/io/path.py#L88'>airflow/io/path.py:88:17:</a> RUF023 [*] `ObjectStoragePath.__slots__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/2b4da0101f0314989d148c3c8a02c87e87048974/airflow/providers_manager.py#L100'>airflow/providers_manager.py:100:17:</a> RUF023 [*] `LazyDictWithCache.__slots__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+37 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/backends/pandas/aggcontext.py#L254'>ibis/backends/pandas/aggcontext.py:254:17:</a> RUF023 [*] `AggregationContext.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/annotations.py#L103'>ibis/common/annotations.py:103:17:</a> RUF023 [*] `Annotation.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/annotations.py#L200'>ibis/common/annotations.py:200:17:</a> RUF023 [*] `Argument.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/annotations.py#L40'>ibis/common/annotations.py:40:17:</a> RUF023 [*] `AttributeValidationError.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/annotations.py#L52'>ibis/common/annotations.py:52:17:</a> RUF023 [*] `ReturnValidationError.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/annotations.py#L64'>ibis/common/annotations.py:64:17:</a> RUF023 [*] `SignatureValidationError.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/collections.py#L288'>ibis/common/collections.py:288:17:</a> RUF023 [*] `FrozenDict.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/collections.py#L347'>ibis/common/collections.py:347:17:</a> RUF023 [*] `RewindableIterator.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/deferred.py#L324'>ibis/common/deferred.py:324:17:</a> RUF023 [*] `Attr.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/deferred.py#L344'>ibis/common/deferred.py:344:17:</a> RUF023 [*] `Item.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/deferred.py#L378'>ibis/common/deferred.py:378:17:</a> RUF023 [*] `Call.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/899dce1b92d08987b3cfbcb29b31729f0328df5f/ibis/common/deferred.py#L442'>ibis/common/deferred.py:442:17:</a> RUF023 [*] `UnaryOperator.__slots__` is not sorted
... 25 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF023 | 150 | 150 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[Proof of concept] Add a rule/autofix to sort __slots__ and `__match_args__`" to "Add a rule/autofix to sort __slots__ and `__match_args__`" by @AlexWaygood on 2024-01-18 17:55_

---

_Comment by @AlexWaygood on 2024-01-18 17:56_

Okay, after the latest push, we now fix all but one violation in CPython (there's only one place in CPython where a multiline dict is used for `__slots__`). Here's the diff from the autofix running on CPython: [cpython_diff.txt](https://github.com/astral-sh/ruff/files/13980154/cpython_diff.txt)


---

_Comment by @codspeed-hq[bot] on 2024-01-18 18:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:sort-dunder-slots)

### Merging #9564 will **not alter performance**

<sub>Comparing <code>AlexWaygood:sort-dunder-slots</code> (333fe62) with <code>main</code> (a3d667e)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@AlexWaygood reviewed on 2024-01-19 10:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:93 on 2024-01-19 10:38_

This could also be written as:

```suggestion
pub(super) fn sort_single_line_elements_sequence(
    kind: &SequenceKind,
    elts: &[ast::Expr],
    elements: &[&str],
    locator: &Locator,
    mut cmp_fn: impl FnMut(&str, &str) -> Ordering,
) -> String {
```

Don't know which is preferred in terms of style!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:6 on 2024-01-19 16:09_

Nit: It would have been helpful to extract `sequence_sorting` as part of its own PR or at least commit because there's now no tooling telling me which parts of it are new (and need review) and which parts are unchanged.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:40 on 2024-01-19 16:09_

Nit, but could safe someone lifetime issues further down
```suggestion
    fn surrounding_parens(&self, source: &str) -> (&'static str, &'static str) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:63 on 2024-01-19 16:11_

Nit: Do you need it to be a `Tok` or could it be a `TokenKind`. `Tok` has the disadvantage that it has variants that contain heap allocated data. That means, Rust needs to call the `Tok::drop` implementation (at least if it isn't able to figure out that the returned variants never allocate)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:54 on 2024-01-19 16:11_

Nit: You can mark this and the closing functions as `const`, so that Rust will evaluate the content at compile time. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:93 on 2024-01-19 16:12_

I don't think there's a consistent style in our repo. I prefer `where` because it can help to keep the parameters on a single line and simplifies the types of the parameters. But that's just me and even I don't do it very consistently. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:94 on 2024-01-19 16:14_

Nit: It would be nice if we could encode this constraint by a type that gets returned by the function extracting the `elements`.

Having both `elts` and `elements` wrapped by a new type could also make this function simpler because you could implement it as a method instead (where you only need to provide the locator and comparator)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:89 on 2024-01-19 16:19_

Nit: Allowing arbitrary functions is one of the most flexible solution but it has a few downsides:

* Rust monomorphizes `sort_single_line_elements_sequence` everytime it is called and a different `F` type is passed. It means, Rust will duplicate your code. This isn't as much of a problem here because the function isn't too large and only used a few times. But it can be a big problem if the implementation is large. 
* This doesn't really apply here because `cmp_fn` is a very common pattern (maybe rename the function to `sort_single_line_elements_by`). However, accepting generic functions doesn't provide any addtional information to me as reader without reading different call-sites. 

An alternative is to introduce a new enum `SortKeyKind` or `Sortable` that has two variants, one for slots and one for dunder all. You can then match on the sort key kind to determine how the elements should be sorted. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:120 on 2024-01-19 16:22_

It seems this function is only used by `sort_dunder_slots`. I would co-locate it with that rule because it allows you to make the function non-generic.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:130 on 2024-01-19 16:23_

Nit: Splitting the assertion into two has the benefit that it generates more useful error messages (especially when using `assert_eq`), when it triggers (it otherwise leaves me wondering if the key length or values length is incorrect)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:169 on 2024-01-19 16:25_

Nit: I assume they are in order. Moving the documentation to the variants removes the need for numbers and ensures the documentation stays up to date even when re-ordering variants
```suggestion

/// 3. It's an unsorted display of string literals,
///    and it's possible we could generate a fix for it
/// 4. The display contains one or more items that are not string
///    literals.
#[derive(Debug, is_macro::Is)]
pub(super) enum SortClassification<'a> {
    /// A display of string literals that is already sorted
    Sorted,
    /// An unsorted display of string literals, but ruff isn't be able to autofix it.
    UnsortedButUnfixable,
    UnsortedAndMaybeFixable { items: Vec<&'a str> },
    NotAListOfStringLiterals,
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:172 on 2024-01-19 16:26_

Okay, monomorphization because of `F` is a problem because most functions are parametrized with `F`. I recommend defining an enum instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:159 on 2024-01-19 16:30_

Nit: The name here isn't telling me much. What kind of analysis is it? Is it like the `SortDunderAnalysis`? Or can we give that structure another name, because the structure isn't the analysis. What does it represent?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:227 on 2024-01-19 16:36_

You could create a `Keys(&[Option<Expr>])` newtype where the `new` function returns `None` when any value is `None` and `Some(Self)` otherwise. 

The `Keys` implementation can than expose functions to access the elements: 

```rust
impl<'a> Keys<'a> {
    fn iter() -> impl Iterator<Item=&'a Expr> {
        // SAFETY: Safe because the constructor guarantees that all values are non-None. 
        self.0.iter().map(Option::unwrap)
    }

    ... 
}
```

But yeah, don't know if it's worth it. It probably depends on the boilerplate necessary.

---

_@MichaReiser approved on 2024-01-19 16:37_

Nice work. My only concern is that the use of `impl FnMut` for the comparator leads multiple copies of the same code because of monomorpization. I recommend introducing an `enum` with variants for the two sort options that you need. 

---

_@AlexWaygood reviewed on 2024-01-19 18:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:6 on 2024-01-19 18:02_

Sorry about that :(

The reason it ended up like this was that I moved material from `sort_dunder_all.rs` piecemeal -- I wasn't sure at first how much I'd need to move over, so I did it in a slightly ad-hoc way. I appreciate that this made it a messy thing to review, though -- I'll try to do better in the future.

---

_@AlexWaygood reviewed on 2024-01-19 18:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:89 on 2024-01-19 18:07_

Would the enum solution require moving all the "isort-style" sorting logic from `sort_dunder_all.rs` into `sequence_sorting.rs`? I don't mind that too much if it's necessary, but conceptually I'd rather keep the isort-style sorting logic in `sort_dunder_all.rs`, since it's specific to that rule

---

_@MichaReiser reviewed on 2024-01-19 18:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:89 on 2024-01-19 18:11_

Yeah, it would require moving the sorting logic. 

---

_@AlexWaygood reviewed on 2024-01-19 18:25_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:63 on 2024-01-19 18:25_

Switched it to use `TokenKind` 👍

---

_@AlexWaygood reviewed on 2024-01-19 19:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:227 on 2024-01-19 19:08_

Oh, that's clever! Hey, why not. Doesn't seem _too_ hard to do.

---

_@AlexWaygood reviewed on 2024-01-19 19:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:94 on 2024-01-19 19:12_

Do you mean something like this:

```rs
struct Whatever<'a> {
    elts: Vec<&'a ast::Expr>,
    elements: Vec<&'a str>,
}

impl<'a> Whatever<'a> {
    fn new(elts: Vec<&'a ast::Expr>, elements: Vec<&'a str>) -> Self {
        assert_eq!(elts.len(), elements.len());
        Self { elts, elements }
```

Or is there a way to actually encode in the type system "these two fields can be of arbitrary length, but they have to be of the _same_ arbitrary length"?

---

_@AlexWaygood reviewed on 2024-01-19 19:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:227 on 2024-01-19 19:33_

Hmm, okay. Having tried to implement it, it's harder than I thought. Tempted to leave this for now, since, as I say, this is an edge case that it's really unlikely we'd hit very often

---

_@AlexWaygood reviewed on 2024-01-19 19:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:159 on 2024-01-19 19:47_

Pushed a rename and some more docs in cf6ca7564eb268ab3098c3a11d3787c36501e40f -- any better?

I really struggled with naming this the other day, but coming back to it after a few days made it seem obvious :)

---

_Comment by @Bibo-Joshi on 2024-01-20 20:03_

I'm really astonished how quickly you closed #1198 and brought to live this follow-up PR - big kudos!

> Implementing an autofix for multiline `__match_args__` wouldn't be too hard […]. The same is true for `__slots__` when it's a tuple, list or set. I don't feel enthused about implementing an autofix for multiline `__slots__` definitions where `__slots__` is a dict, however. […]
> 
> If we're interested in autofixes for multiline `__slots__` and `__match_args__` definitions, let me know whether you'd prefer it in this PR or a followup!

Just wanted to let you know that I'd be very interested in autofixes for multline tuple- `__slots__`. Not implementing that for dicts is completely understandable IMO (and I don't have a use case etiher …)

---

_Comment by @AlexWaygood on 2024-01-20 20:15_

> Just wanted to let you know that I'd be very interested in autofixes for multline tuple- `__slots__`. Not implementing that for dicts is completely understandable IMO (and I don't have a use case etiher …)

Have no fear, the description in my PR summary is already slightly out of date — I've already implemented the autofix for multiline tuple `__slots__` ;) 

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:244 on 2024-01-21 19:49_

I think in our codebase we'd tend to call this `current`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:240 on 2024-01-21 19:50_

Should this be a method on `StringLiteralDisplay`?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:26 on 2024-01-21 19:52_

Nit: can you document each variant?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:56 on 2024-01-21 19:53_

It would be nice if we could use this in the `isort` rules... But way out-of-scope. And probably really complicated, actually, since the rules there are so precise and take into account all the different "parts" of an import (vs. this which is much simpler and clearer for just-strings). So ignore this :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:81 on 2024-01-21 20:00_

@BurntSushi - Do these `Ord` / `PartialOrd` / `PartialEq` / `Eq` implementations look right / consistent to you? (I've never properly learned the relationships between them.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:110 on 2024-01-21 20:01_

I would typically call this `from_value`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:142 on 2024-01-21 20:02_

I think we typically call these "brackets", i.e., brackets are the set that includes parentheses (`(`), square brackets (`[`), and braces (`{`).

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:179 on 2024-01-21 20:25_

You typically want `&[ast::Expr]` instead of `&Vec<ast::Expr>`. There are other ways to create a slice, and any function that accepts `&[Expr]` will accept `&Vec<Expr>`, but if you have an `&[Expr]`, you can't convert it the other way to `&Vec<Expr>` without allocating. So, you likely want the most permissive type here (`&[Expr]`). It's similar to `&String` vs. `&str`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:94 on 2024-01-21 20:26_

I think he's suggesting creating a dedicated struct like that. Since you're allocating below anyway with `collect_vec`, perhaps you want to do something like:
```rust
struct Whatever<'a>(Vec<(&'a Expr, &'a str)>)
```

That is: store the zipped elements, so that the constraint is always enforced once you've created the struct.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:258 on 2024-01-21 20:30_

Could we possibly return `Self::UnsortedButUnfixable` when a later element in `elements` is not a string literal? Like what if the first two elements are string literals, and the second element is implicitly concatenated, and then the third element is not a string. (I suspect we should always return `NotAListOfStringLiterals` if any element is not a string, but I'm wondering if that's the case with the coed as-written.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:251 on 2024-01-21 20:31_

So this is like: we found something that's out-of-order, so now we return all elements? If so, can you add a comment here to explain the logic.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:280 on 2024-01-21 20:31_

Nit: I'd expect this to be called `len` (and for this to also implement `is_empty`).

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:457 on 2024-01-21 20:32_

How much of the below is new code vs. moved from the isort rule?

---

_@charliermarsh reviewed on 2024-01-21 20:32_

Very nice, a couple questions and comments!

---

_@AlexWaygood reviewed on 2024-01-21 20:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:56 on 2024-01-21 20:38_

Yeah — and the isort rules have loads of complex configuration logic, which I'd definitely rather keep out of these rules if at all possible :)

---

_@AlexWaygood reviewed on 2024-01-21 20:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:81 on 2024-01-21 20:39_

(FWIW: these haven't changed in this PR, just moved to a different file, and Micha reviewed these in my previous PR (my initial implementations in my previous PR had some issues, which Micha pointed out))

---

_@AlexWaygood reviewed on 2024-01-21 22:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:110 on 2024-01-21 22:17_

This is definitely a silly hill for me to die on, but I quite like the way `InferredMemberType::of(member)` reads like an English sentence. Happy to change it if you feel strongly, though!

---

_@AlexWaygood reviewed on 2024-01-21 22:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:258 on 2024-01-21 22:20_

Hmm, good point

---

_@AlexWaygood reviewed on 2024-01-21 22:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:280 on 2024-01-21 22:21_

I've renamed `num_elems` to `len`. I tried implementing `is_empty()`, but I get a clippy warning about unused code, because it's never used currently

---

_@AlexWaygood reviewed on 2024-01-21 22:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:258 on 2024-01-21 22:30_

Fixed in https://github.com/astral-sh/ruff/pull/9564/commits/606d85a74cfb8be467be0882fe94081c08463542, and I added a test!

---

_@AlexWaygood reviewed on 2024-01-21 22:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:240 on 2024-01-21 22:55_

Not sure... it makes the signature cleaner, but I was thinking of this struct as just a pretty simple container for heterogenous data; this breaks my mental model of that slightly. Anyway, I implemented it as a method in https://github.com/astral-sh/ruff/pull/9564/commits/e64879a91ea16b09cf04ede8c6645cea3ba47757 :)

---

_@AlexWaygood reviewed on 2024-01-21 22:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:457 on 2024-01-21 22:58_

I think everything below this point is entirely unchanged, just moved, _except_ that a few things have been renamed and a few comments tweaked (e.g. `collect_dunder_all_lines()` has been renamed to `collect_string_sequence_lines()`, etc.)

---

_@AlexWaygood reviewed on 2024-01-22 00:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:94 on 2024-01-22 00:04_

Nice -- I gave this a go in https://github.com/astral-sh/ruff/pull/9564/commits/6645934e0f2d3e53071edc1b30a4d17e835ed76e.

I experimented with doing this:

```diff
--- a/crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs
@@ -258,7 +258,7 @@ pub(super) enum SortClassification<'a> {
     /// and it's possible we could generate a fix for it;
     /// here's the values of the elts so we can use them to
     /// generate an autofix:
-    UnsortedAndMaybeFixable { items: Vec<&'a str> },
+    UnsortedAndMaybeFixable { items: SequenceElements<'a> },
     /// The display contains one or more items that are not string
     /// literals.
     NotAListOfStringLiterals,
@@ -309,7 +309,7 @@ impl<'a> SortClassification<'a> {
                 if any_implicit_concatenation {
                     return Self::UnsortedButUnfixable;
                 }
-                return Self::UnsortedAndMaybeFixable { items };
+                return Self::UnsortedAndMaybeFixable { items: SequenceElements::new(items, elements) };
             }
             current = next;
```

Unfortunately that doesn't really work, however, due to the fact that for single-line dicts, we also want to zip the dict-value nodes up along with the dict-key nodes and the string values of the dict keys

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-22 00:04_

---

_Renamed from "Add a rule/autofix to sort __slots__ and `__match_args__`" to "Add a rule/autofix to sort `__slots__` and `__match_args__`" by @AlexWaygood on 2024-01-22 00:33_

---

_@charliermarsh reviewed on 2024-01-22 01:47_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:280 on 2024-01-22 01:47_

I thought Clippy warned when you had `len` but no `is_empty`, so I must be misremembering!

---

_@charliermarsh approved on 2024-01-22 01:47_

Looks good to me!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:22 on 2024-01-22 02:08_

Nit: I think you should make this `Debug, Copy, Clone`. Then you can refer to it without references everywhere, e.g., you shouldn't need `const SORTING_STYLE: &SortingStyle = &SortingStyle::Natural;` -- you can just inline `SortingStyle::Natural` everywhere. If a struct is `Copy`, you don't need to call `.clone()` in order to pass an owned reference to it, since Rust will just copy it automatically. This struct is so tiny (smaller than a reference, even) that it's fine for it to be `Copy`.

---

_@charliermarsh reviewed on 2024-01-22 02:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:315 on 2024-01-22 04:50_

I feel like I'd find it clearer to just have `.len()` on this and do `if i < element_trios.len() - 1`, since that's such a common pattern in iteration.

---

_@charliermarsh reviewed on 2024-01-22 04:50_

---

_@MichaReiser reviewed on 2024-01-22 07:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:244 on 2024-01-22 07:35_

It reminded me of the good old js days:

```javascript
var _this = this;
element.addEventListener('click', function () {
	_this.onClick();
});
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:22 on 2024-01-22 07:37_

Oh of course, thanks!

---

_@AlexWaygood reviewed on 2024-01-22 07:37_

---

_@MichaReiser reviewed on 2024-01-22 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:142 on 2024-01-22 07:42_

Actually, we don't :wink: . At least not in the formater. We call these `parentheses` (I avoid short forms wherever possible). 

---

_@AlexWaygood reviewed on 2024-01-22 08:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:315 on 2024-01-22 08:06_

I definitely know what you mean, but here specifically I do quite like having this logic right next to the assertions in the constructor method that maintain the invariant that protect us from underflow here

---

_Comment by @AlexWaygood on 2024-01-22 12:17_

Thanks both for the reviews! :D

I've had two approvals, so I'm landing this now, but very happy to tackle any further comments in followup PRs :)

---

_Merged by @AlexWaygood on 2024-01-22 12:21_

---

_Closed by @AlexWaygood on 2024-01-22 12:21_

---

_Branch deleted on 2024-01-22 12:32_

---
