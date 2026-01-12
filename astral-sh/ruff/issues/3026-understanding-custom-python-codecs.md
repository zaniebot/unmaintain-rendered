```yaml
number: 3026
title: Understanding custom python codecs
type: issue
state: closed
author: delfick
labels:
  - wish
assignees: []
created_at: 2023-02-19T08:57:02Z
updated_at: 2024-04-19T05:26:17Z
url: https://github.com/astral-sh/ruff/issues/3026
synced_at: 2026-01-12T15:54:43Z
```

# Understanding custom python codecs

---

_@delfick_

Hello,

Python has a feature where you can register a codec such that when python imports a file it can run the contents of that file through your codec to translate it before it's imported.

I use this technique for my testing library [nose-of-yeti](https://noseofyeti.readthedocs.io) which lets me write my tests using an rspec inspired nested "describe" and "it" syntax.

Currently it [supports](https://noseofyeti.readthedocs.io/en/latest/api/usage.html) tooling like black, pylama and mypy.

I'm wondering if it were possible for ruff to be able to understand python codecs? (when the file starts with a `# coding: <codec_name>` line). I imagine ruff seeing that it uses a codec and asking python to find that codec and decode the file before ruff reads it. I understand there is a speed cost to that as this is an existing cost I'm fine with.

Thanks!

---

_Label `question` added by @charliermarsh on 2023-02-19 15:46_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:17_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:17_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:17_

---

_Label `rule` removed by @charliermarsh on 2023-07-10 01:18_

---

_Label `needs-decision` removed by @charliermarsh on 2023-07-10 01:18_

---

_Label `wish` added by @charliermarsh on 2023-07-10 01:18_

---

_Comment by @delfick on 2024-04-05 09:06_

@charliermarsh I'm wondering if this would even be possible (with or without ruff having some kind of plugin ability). It stops me using ruff for most of my projects. I feel if ruff supported it, it would make the biggest drawback of it go away (the time cost for linting/formatting adds up and becomes noticeable with a big enough file).

I don't have any rust knowledge (and definitely not enough time to try and learn it) so I don't know if this kind of support could be optionally added or added in a way that doesn't cause too much undue effort on ruff maintainers.

It essentially boils down that

this:

```
describe "Thing":
```

Is equivalent to `class TestThing:`

and:

```
it "does stuff", one: int, two: str:
```

is equivalent to `def test_does_stuff(one: int, two: str)` (with a self argument if nested under a describe)

and:

```
ignore "does things":
```

becomes `def ignore_does_things`

and:

```
async it "does more stuff":
```

is `async def test_does_more_stuff():`

And describes can be nested under other describes and get flattened out

so 

```
describe "One":
    describe "Two":
        pass
```

becomes

```
class TestOne:
    pass

class TestTwo(TestOne):
    pass
```

(technically it also has `context` as an alias for `describe` and `after_each`/`before_each` but I don't use any of that anymore and should probably deprecate that functionality)

Currently for my black support I have a [slightly modified Grammar.txt](https://github.com/delfick/nose-of-yeti/blob/7bcb9327071f1591f9684c2ff90c398125f8ed2a/noseOfYeti/black/Grammar.noy.txt) where this is the difference

```patch
--- b	2024-04-05 20:00:19
+++ a	2024-04-05 19:59:45
@@ -20,8 +20,8 @@
 
 decorator: '@' namedexpr_test NEWLINE
 decorators: decorator+
-decorated: decorators (classdef | funcdef | async_funcdef)
-async_funcdef: ASYNC funcdef
+decorated: decorators (classdef | funcdef | async_funcdef | noy_stmt)
+async_funcdef: ASYNC (funcdef | it_stmt | setup_teardown_stmts)
 funcdef: 'def' NAME [typeparams] parameters ['->' test] ':' suite
 parameters: '(' [typedargslist] ')'
 
@@ -110,8 +110,8 @@
 assert_stmt: 'assert' test [',' test]
 type_stmt: "type" NAME [typeparams] '=' test
 
-compound_stmt: if_stmt | while_stmt | for_stmt | try_stmt | with_stmt | funcdef | classdef | decorated | async_stmt | match_stmt
-async_stmt: ASYNC (funcdef | with_stmt | for_stmt)
+compound_stmt: if_stmt | while_stmt | for_stmt | try_stmt | with_stmt | funcdef | classdef | decorated | async_stmt | match_stmt | noy_stmt
+async_stmt: ASYNC (funcdef | with_stmt | for_stmt | it_stmt | setup_teardown_stmts)
 if_stmt: 'if' namedexpr_test ':' suite ('elif' namedexpr_test ':' suite)* ['else' ':' suite]
 while_stmt: 'while' namedexpr_test ':' suite ['else' ':' suite]
 for_stmt: 'for' exprlist 'in' testlist_star_expr ':' suite ['else' ':' suite]
@@ -254,3 +254,8 @@
 guard: 'if' namedexpr_test
 patterns: pattern (',' pattern)* [',']
 pattern: (expr|star_expr) ['as' expr]
+
+it_stmt: ('it' | 'ignore') STRING (',' NAME [':' [test]])* ':' ['->' test] NEWLINE INDENT stmt+ DEDENT
+setup_teardown_stmts: ('before_each' | 'after_each') ':' suite
+describe_stmt: ('context' | 'describe') [dotted_name ','] STRING ':' suite
+noy_stmt: setup_teardown_stmts | it_stmt  | describe_stmt['as' expr]

```

---

_Comment by @MichaReiser on 2024-04-05 10:08_

I don't think this fits well into Ruff because it requires executing Python code, which adds a dependency on Python. 

I still don't fully understand the workflow but wouldn't it be possible to integrate this around Ruff by using a Python script that:

* Loads the file
* Executes the codec 
* Uses subprocess to call ruff and passes the decoded text via stdin
* Does something with the output

It's not entirely clear to me how "does something with the output" would work in the formatter case. 

So I think this functionality is better suited outside of Ruff, rather than being integrated into Ruff (also considering that it isn't a very common ask)

---

_Comment by @delfick on 2024-04-05 10:22_

@MichaReiser yeah. the way I make it work for black is I monkeypatch the grammar such that it considers the grammar to be valid (and [modify some visit hooks](https://github.com/delfick/nose-of-yeti/blob/main/noseOfYeti/plugins/black_compat.py#L107-L140)) and it seems to deal with that fine because everything else is still valid python.

For pylama I [replace how it reads the file](https://github.com/delfick/nose-of-yeti/blob/main/noseOfYeti/plugins/pylama.py#L57) so that when it reads the file, the translation happens as part of that and so as long as I disable formatting related lint rules, it lints it like normal python and works fine.

I imagine for ruff check it could work for the latter and be no worse than the current situation (except that ruff check itself is much faster and integrates with my editor better, so would in fact be better)

For ruff formatter however, it would require the ability to extend ruff's understanding of the python grammar cause of that "what do with the output". I imagine though it's not as simple as "extend ruff's grammar"!

> (also considering that it isn't a very common ask)

This project of mine (which I've actively worked on and used since 2010) definitely gets a wide range of reactions hahhah

---

_Comment by @MichaReiser on 2024-04-05 10:35_

I see. Yeah I don't think we have capacity to add support for a custom grammar today (that also requires transformation). We do support Python code embedded in Jupyter notebook but that's different because the code can be used as is, without the need for any transformation. 


---

_Closed by @MichaReiser on 2024-04-05 10:35_

---

_Comment by @delfick on 2024-04-05 11:37_

Fair enough. Hopefully black/pylama at least continue to exist for a long time yet

---

_Comment by @delfick on 2024-04-19 05:26_

@MichaReiser I have one more question sorry. If I made it so that it doesn't do any dedents would that make it easier?

So essentially all that needs supporting is "it" and "describe" as syntax sugar for `def` and `class` and everything else could be ignored. So:

```
it "does stuff":
```

would be syntax sugar for

```
def test_it_does_stuff():
```

and 

```
it "does stuff", one: int, two: str -> None:
```

would be syntax sugar for

```
def test_it_does_stuff(one: int, two: str) -> None:
```

and

```
describe "Things":
```

would be syntax sugar for

```
class TestThings:
```

and 

```
describe "Things", Parent:
```

would be syntax sugar for

```
class TestThings(Parent):
```

and no other transformations would be required. (how it currently transforms into a flat class structure is no longer required now I don't use nosetests)

---
