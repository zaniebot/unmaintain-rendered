```yaml
number: 22677
title: "[`pyupgrade`] properly trigger in nested class (`UP008`)"
type: pull_request
state: open
author: leandrobbraga
labels:
  - bug
  - rule
assignees: []
base: main
head: bugfix/up008-inner-class
created_at: 2026-01-18T13:05:36Z
updated_at: 2026-01-21T04:50:53Z
url: https://github.com/astral-sh/ruff/pull/22677
synced_at: 2026-01-21T04:56:55Z
```

# [`pyupgrade`] properly trigger in nested class (`UP008`)

---

_@leandrobbraga_

While debugging, I noticed that the function parameters were of type `Expr::Name` in the normal case and `Expr::Attribute` when the function was in a nested class. Reading the [documentation](https://docs.python.org/3/library/ast.html#ast.expr), it seems that being an `Expr::Attribute` makes sense because `Inner` is an attribute of `Outer`.

I did some thinking and could not identify any additional `Expr` types I should be pattern-matching here, but I would like some input from you.

The change seems to fix the issue without any regression.

```console
echo 'class Base:
      def __init__(self, foo):
          self.foo = foo


  class Outer(Base):
      def __init__(self, foo):
          super().__init__(foo)  # Should not trigger UP008

      class Inner(Base):
          def __init__(self, foo):
              super().__init__(foo)  # Should not trigger UP008

      class InnerInner(Base):
          def __init__(self, foo):
              super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008

  ' | cargo run -p ruff -- check --select UP008 -
UP008 Use `super()` instead of `super(__class__, self)`
  --> -:16:18
   |
14 |     class InnerInner(Base):
15 |         def __init__(self, foo):
16 |             super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
   |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
help: Remove `super()` parameters

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Closes #22597 

---

_Renamed from "[`pyupgrade`] properly trigger UP008 in nested class" to "[`pyupgrade`] properly trigger in nested class (`UP008`)" by @leandrobbraga on 2026-01-18 13:06_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:17_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/superset/sql/dialects/pinot.py#L172'>superset/sql/dialects/pinot.py:172:25:</a> UP008 Use `super()` instead of `super(__class__, self)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP008 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/superset/sql/dialects/pinot.py#L172'>superset/sql/dialects/pinot.py:172:25:</a> UP008 [*] Use `super()` instead of `super(__class__, self)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP008 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>





---

_Review requested from @ntBre by @ntBre on 2026-01-20 22:48_

---

_Label `bug` added by @ntBre on 2026-01-20 22:48_

---

_Label `rule` added by @ntBre on 2026-01-20 22:48_

---

_Comment by @leandrobbraga on 2026-01-20 23:40_

This bug seems easy to introduce into the codebase. I did a quick search and found another example.

PLC1802 won’t trigger if the list is an attribute.

https://play.ruff.rs/f9f8f76b-c468-4f4f-a3b8-0bca5850daf1

```python
class Fruits:
    fruits: list[str]

    def __init__(self, fruits: list[str]):
        self.fruits = fruits

fruits_class = Fruits(["orange", "apple"])
fruits_list = ["a", "b"]

if len(fruits_class.fruits):
    ...

if len(fruits_list):
    ...
```

I opened an [issue](https://github.com/astral-sh/ruff/issues/22780) and I can't think on how to prevent this class of bug.

---

_@amyreese approved on 2026-01-21 00:28_

---

_@generalmimon reviewed on 2026-01-21 04:50_

---

_Review comment by @generalmimon on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP008.py`:314 on 2026-01-21 04:50_

I'm not sure what you intended here with the 1-argument `super(self)` call, but it actually raises an error at runtime:

```pycon
Python 3.14.2 (tags/v3.14.2:df79316, Dec  5 2025, 17:18:21) [MSC v.1944 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> class Base:
...     def __init__(self, foo):
...         self.foo = foo
...
>>> class Outer(Base):
...     def __init__(self, foo):
...         super(self).__init__(foo)  # Should not trigger UP008
...
>>> o = Outer(5)
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    o = Outer(5)
  File "<python-input-1>", line 3, in __init__
    super(self).__init__(foo)  # Should not trigger UP008
    ~~~~~^^^^^^
TypeError: super() argument 1 must be a type, not Outer
```

Perhaps you wanted to use either a 2-argument or 0-argument call instead? Also, if the `super` call doesn't have two arguments, the lint will return early, so I don't know if you wanted to verify that there is no panic in that case, for example?

https://github.com/astral-sh/ruff/blob/6f8ae1ba7c62d8197371c43c5501fc005b91824c/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs#L91-L96

---
