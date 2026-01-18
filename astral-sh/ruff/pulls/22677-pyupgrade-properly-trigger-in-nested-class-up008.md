```yaml
number: 22677
title: "[`pyupgrade`] properly trigger in nested class (`UP008`)"
type: pull_request
state: open
author: leandrobbraga
labels: []
assignees: []
base: main
head: bugfix/up008-inner-class
created_at: 2026-01-18T13:05:36Z
updated_at: 2026-01-18T13:17:55Z
url: https://github.com/astral-sh/ruff/pull/22677
synced_at: 2026-01-18T14:22:04Z
```

# [`pyupgrade`] properly trigger in nested class (`UP008`)

---

_@leandrobbraga_

I also tested it in multi-level nested classes and it worked as well.

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
