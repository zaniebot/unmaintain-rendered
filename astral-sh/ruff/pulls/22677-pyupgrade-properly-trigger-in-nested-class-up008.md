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
updated_at: 2026-01-21T23:16:57Z
url: https://github.com/astral-sh/ruff/pull/22677
synced_at: 2026-01-22T00:08:57Z
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
+ <a href='https://github.com/apache/superset/blob/25647942fd774d8c193cb570ba5fc3d74f037999/superset/sql/dialects/pinot.py#L172'>superset/sql/dialects/pinot.py:172:25:</a> UP008 Use `super()` instead of `super(__class__, self)`
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
+ <a href='https://github.com/apache/superset/blob/25647942fd774d8c193cb570ba5fc3d74f037999/superset/sql/dialects/pinot.py#L172'>superset/sql/dialects/pinot.py:172:25:</a> UP008 [*] Use `super()` instead of `super(__class__, self)`
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

_Comment by @generalmimon on 2026-01-21 05:09_

@leandrobbraga (https://github.com/astral-sh/ruff/pull/22677#issue-3826762723):

> ```console
> echo 'class Base:
>       def __init__(self, foo):
>           self.foo = foo
> 
> 
>   class Outer(Base):
>       def __init__(self, foo):
>           super().__init__(foo)  # Should not trigger UP008
> 
>       class Inner(Base):
>           def __init__(self, foo):
>               super().__init__(foo)  # Should not trigger UP008
> 
>       class InnerInner(Base):
>           def __init__(self, foo):
>               super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
> 
>   ' | cargo run -p ruff -- check --select UP008 -
> UP008 Use `super()` instead of `super(__class__, self)`
>   --> -:16:18
>    |
> 14 |     class InnerInner(Base):
> 15 |         def __init__(self, foo):
> 16 |             super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
>    |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>    |
> help: Remove `super()` parameters
> 
> Found 1 error.
> No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
> ```

This doesn't work either - I suppose the indentation isn't what you intended?

```console
$ cat test.py
class Base:
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


o = Outer(5)
i = Outer.Inner(5)
ii = Outer.InnerInner(5)

$ python3 --version
Python 3.14.2

$ python3 test.py
Traceback (most recent call last):
  File "/home/pp/test.py", line 21, in <module>
    ii = Outer.InnerInner(5)
  File "/home/pp/test.py", line 16, in __init__
    super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
          ^^^^^^^^^^^^^^^^^^^^^^
AttributeError: type object 'Inner' has no attribute 'InnerInner'
```

---

_@leandrobbraga reviewed on 2026-01-21 09:32_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP008.py`:314 on 2026-01-21 09:32_

It was supposed to be with 0-argument, thanks for pointing it.

---

_Comment by @leandrobbraga on 2026-01-21 09:36_

This was a copy-paste indentation error, for some reason I lost the indentation when I copy-pasted, but it was tested properly.

Try linting this code:

```python
class Base:
    def __init__(self, foo):
        self.foo = foo


class Outer(Base):
    def __init__(self, foo):
        super().__init__(foo)  # Should not trigger UP008

    class Inner(Base):
        def __init__(self, foo):
            super(Outer.Inner, self).__init__(foo)  # Should trigger UP008

        class InnerInner(Base):
            def __init__(self, foo):
                super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008

oii = Outer.Inner.InnerInner(1)
```
Output:

```console
> echo 'class Base:
      def __init__(self, foo):
          self.foo = foo


  class Outer(Base):
      def __init__(self, foo):
          super().__init__(foo)  # Should not trigger UP008

      class Inner(Base):
          def __init__(self, foo):
              super(Outer.Inner, self).__init__(foo)  # Should trigger UP008

          class InnerInner(Base):
              def __init__(self, foo):
                  super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008

  oii = Outer.Inner.InnerInner(1)' | cargo run -p ruff -- check --select UP008 -
UP008 Use `super()` instead of `super(__class__, self)`
  --> -:12:18
   |
10 |     class Inner(Base):
11 |         def __init__(self, foo):
12 |             super(Outer.Inner, self).__init__(foo)  # Should trigger UP008
   |                  ^^^^^^^^^^^^^^^^^^^
13 |
14 |         class InnerInner(Base):
   |
help: Remove `super()` parameters

UP008 Use `super()` instead of `super(__class__, self)`
  --> -:16:22
   |
14 |         class InnerInner(Base):
15 |             def __init__(self, foo):
16 |                 super(Outer.Inner.InnerInner, self).__init__(foo)  # Should trigger UP008
```

---

_@generalmimon reviewed on 2026-01-21 10:05_

---

_Review comment by @generalmimon on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:140 on 2026-01-21 10:05_

What's the point of accepting an attribute in the second argument? This `second_arg_id` is required to be equal to the name of the "parent argument" (`parent_arg`), which is typically `self`:

https://github.com/astral-sh/ruff/blob/ff11db466dc5fae8272a026e86638b04505855cb/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs#L147

I think it doesn't make sense to accept wild stuff like `super(..., a.b.c.self)`.

---

_@leandrobbraga reviewed on 2026-01-21 10:44_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:140 on 2026-01-21 10:44_

That's correct, I removed the match on `Expr::Attribute` for the second parameter.

---

_@generalmimon reviewed on 2026-01-21 20:42_

---

_Review comment by @generalmimon on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:134 on 2026-01-21 20:42_

This assumes that whenever `super(A.B, self)` appears inside a class named `B`, it must be the `super(__class__, self)` case and an upgrade can be performed. Unfortunately, this is not necessarily true. Consider this:

```python
class Base:
    def __init__(self, foo):
        print(f"Base.__init__({foo}) called")
        self.foo = foo


class Outer:
    class Inner(Base):
        def __init__(self, foo):
            print(f"Outer.Inner.__init__({foo}) called")
            super().__init__(foo)


class Inner(Outer.Inner):
    def __init__(self, foo):
        super(Outer.Inner, self).__init__(foo)


i = Inner(5)
```

Note that the `super` call in the `Inner.__init__` method body is `super(Outer.Inner, self)`, not `super(Inner, self)`. If you run the above code, the output is the following:

```console
root@d2a45e4529f7:~# python3 --version
Python 3.13.5
root@d2a45e4529f7:~# python3 test.py
Base.__init__(5) called
```

As you can see, only `Base.__init__` was called, not `Outer.Inner.__init__`.

Nevertheless, your current implementation at https://github.com/astral-sh/ruff/commit/a9b58e1d3610ef14430569af480d9a5fd0b4d209 still suggests omitting arguments to this `super` call:

```console
root@d2a45e4529f7:~/ruff# git status
On branch bugfix/up008-inner-class
Your branch is up to date with 'origin/bugfix/up008-inner-class'.

nothing to commit, working tree clean
root@d2a45e4529f7:~/ruff# git rev-parse HEAD
a9b58e1d3610ef14430569af480d9a5fd0b4d209
root@d2a45e4529f7:~/ruff# cargo run -p ruff -- check --select UP ../test.py --no-cache
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.27s
     Running `target/debug/ruff check --select UP ../test.py --no-cache`
UP008 Use `super()` instead of `super(__class__, self)`
  --> /root/test.py:16:14
   |
14 | class Inner(Outer.Inner):
15 |     def __init__(self, foo):
16 |         super(Outer.Inner, self).__init__(foo)
   |              ^^^^^^^^^^^^^^^^^^^
   |
help: Remove `super()` parameters

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

In preview, this is even offered as a safe fix:

```console
root@d2a45e4529f7:~/ruff# cargo run -p ruff -- check --preview --select UP ../test.py --no-cache
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
     Running `target/debug/ruff check --preview --select UP ../test.py --no-cache`
UP008 [*] Use `super()` instead of `super(__class__, self)`
  --> /root/test.py:16:14
   |
14 | class Inner(Outer.Inner):
15 |     def __init__(self, foo):
16 |         super(Outer.Inner, self).__init__(foo)
   |              ^^^^^^^^^^^^^^^^^^^
   |
help: Remove `super()` parameters
13 |
14 | class Inner(Outer.Inner):
15 |     def __init__(self, foo):
   -         super(Outer.Inner, self).__init__(foo)
16 +         super().__init__(foo)
17 |
18 |
19 | i = Inner(5)

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

However, if you apply it, the behavior of the program changes - notice the additional message `Outer.Inner.__init__(5) called`, which the original program did not print:

```console
root@d2a45e4529f7:~# cat test.py
class Base:
    def __init__(self, foo):
        print(f"Base.__init__({foo}) called")
        self.foo = foo


class Outer:
    class Inner(Base):
        def __init__(self, foo):
            print(f"Outer.Inner.__init__({foo}) called")
            super().__init__(foo)


class Inner(Outer.Inner):
    def __init__(self, foo):
        super().__init__(foo)


i = Inner(5)
root@d2a45e4529f7:~# python3 test.py
Outer.Inner.__init__(5) called
Base.__init__(5) called
```

I admit that this is perhaps an unusual scenario. Calling `super(Outer.Inner, self)` in the `Inner` class looks rather unintentional or at least suspicious, but from Python's point of view, it is a valid program that does not raise any runtime errors.

So I believe that Ruff should verify whether the entire attribute chain (in this case `Outer.Inner`) matches, not just its last segment. This means that instead of just finding the closest enclosing class:

https://github.com/astral-sh/ruff/blob/a9b58e1d3610ef14430569af480d9a5fd0b4d209/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs#L120-L128

... and comparing its name with the last segment in the attribute chain:

https://github.com/astral-sh/ruff/blob/a9b58e1d3610ef14430569af480d9a5fd0b4d209/crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs#L148

..., Ruff should also inspect further (nested) enclosing classes and compare the second-to-last segment in the attribute chain, third-to-last segment, etc. If anything doesn't match, UP008 should not be triggered.

---

_@leandrobbraga reviewed on 2026-01-21 20:56_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:134 on 2026-01-21 20:56_

Thank you for your review, I'll try to think about the corner cases and come up with a solution in the next days. 

---

_@leandrobbraga reviewed on 2026-01-21 20:56_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:134 on 2026-01-21 20:56_

Thank you for your review, I'll try to think about the corner cases and come up with a solution in the next days. 

---

_@generalmimon reviewed on 2026-01-21 23:16_

---

_Review comment by @generalmimon on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:134 on 2026-01-21 23:16_

See https://github.com/astral-sh/ruff/issues/22597#issuecomment-3781530191 - I've just found out that the situation I described above is already covered in this pyupgrade test case:

[asottile / **pyupgrade**](https://github.com/asottile/pyupgrade) / [`tests/features/super_test.py:32-35`](https://github.com/asottile/pyupgrade/blob/369aea6489e38ca74a18df494f191857ba484950/tests/features/super_test.py#L32-L35)

```py
        'class Outer:\n'  # super arg1 nested in unrelated name
        '    class C(Base):\n'
        '        def f(self):\n'
        '            super(some_module.Outer.C, self).f()\n',
```

Honestly, I think it's worth looking at the entire [`tests/features/super_test.py`](https://github.com/asottile/pyupgrade/blob/369aea6489e38ca74a18df494f191857ba484950/tests/features/super_test.py) file and ideally porting all test cases that are not yet covered by the Ruff test suite. There aren't that many, but Ruff still seems to be missing a few important ones.

---
