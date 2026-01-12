```yaml
number: 10723
title: B007 guidance when variable used outside the loop only
type: issue
state: open
author: jaraco
labels:
  - needs-decision
assignees: []
created_at: 2024-04-01T21:34:57Z
updated_at: 2025-09-28T18:17:27Z
url: https://github.com/astral-sh/ruff/issues/10723
synced_at: 2026-01-12T15:54:50Z
```

# B007 guidance when variable used outside the loop only

---

_@jaraco_

Bugbear returns a B007 for [this loop](https://github.com/jaraco/jaraco.postgres/blob/bf844a039822da656422e578460385b5b2fee266/jaraco/postgres/__init__.py#L507-L512) when `i` is used solely outside the loop, a usage that appears to be valid and have [endorsement from the community](https://stackoverflow.com/a/3611987/70170).

```
jaraco/postgres/__init__.py:507:13: B007 Loop control variable `i` not used within loop body
```

Somewhat strangely, renaming the variable to `_i` in both places does suppress the error, but that feels like the wrong "fix", since prefixing with `_` is an indicator of "unused".

In this particular example, it's legacy code that I've inherited and I plan to rewrite it using a retry wrapper and avoiding an index variable altogether. 

Nevertheless, it seems to me that in general, B007 should not alert here or if it does, the documentation should include guidance on what to do in this scenario.

Related: #2509.
Ruff 0.3.4

---

_Label `needs-decision` added by @charliermarsh on 2024-04-02 03:44_

---

_Comment by @charliermarsh on 2024-04-02 03:47_

There's some discussion around this pattern in https://github.com/PyCQA/flake8-bugbear/issues/68. It looks like bugbear explicitly decided not to support this. I welcome more opinions here.

---

_Comment by @jaraco on 2024-04-02 15:15_

In my opinion, since it's language feature that's been extensively discussed and deemed supported by consensus, default linter settings should be lenient to that position. It feels a bit tyrannical to disallow a pattern just because one tool developer finds the pattern risky.

It also feels like B007 is trying to do too much and too little at the same time:

- It raises when the loop variable is not used inside the loop, but
- It allows the variable to be marked as private, then not used in the loop, but still used outside the loop, and
- It doesn't protect against masking another variable in the local namespace, and
- If the variable is used in the loop, there's no longer any concern with it being used outside the loop.

I feel like this rule should be refactored into its separate concerns and check against:

- Loop variable overrides variable in the local scope.
- Loop variable is not used at all in the surrounding scope.
- Loop variable is used solely outside the loop (and thus may be unbound).

I'd further argue that only the second of those should be enabled by default and the other two should be opt-in, as that use is legitimized by consensus, but even if they're on by default, that would give users the ability to opt out of that behavior. Currently, a user or system cannot opt out of B007 for outside access without opting out for inside access.

---

_Comment by @Nodd on 2024-04-02 15:48_

I agree that B007 is smelly, but maybe it should be kept as-is for compatibility, and new rules added following @jaraco's proposal ?

The first one could still yield false positives :

> Loop variable overrides variable in the local scope.

Wouldn't it raise if there are two loops with the same variable name ?

```python
for i in range(10):
    f(i)
# i exists here

# This loop triggers "Loop variable overrides variable in the local scope."
for i in range(10, 20):
    g(i)
```



---

_Comment by @charliermarsh on 2024-04-02 16:37_

@AlexWaygood - wondering if you have an opinion here?

---

_Comment by @AlexWaygood on 2024-04-02 17:34_

Thanks for opening the issue @jaraco!

I'm not _sure_ I agree that this pattern has been "supported by consensus". If anything, I feel like I see more and more linters coming out against this kind of style because it can be difficult for static-analysis tools to reason about. For example, take the following snippet. A static-analysis tool such as Ruff or mypy cannot know whether `i` is bound after the loop has completed or not, as an empty iterable might have been passed to the function (in which case `i` will be unbound), or a nonempty iterable might have been passed (in which case `i` will be bound):

```py
from collections.abc import Iterable

def foo(it: Iterable[int]) -> int:
    for i in it:
        pass
    return i + 42
```
```pycon
(env) % python -i flake8-pyi/foo.py                                                                                                                                                                              ~/dev
>>> foo([])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/alexw/dev/flake8-pyi/foo.py", line 6, in foo
    return i + 42
           ^
UnboundLocalError: cannot access local variable 'i' where it is not associated with a value
>>> foo([1])
43
```

It's for this reason that pyright [will emit an error](https://pyright-play.net/?code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgElUkRjkAoVgEyWCmDDAAU6VAC56jZkWQBtdFlQBdAJRQAtAD4oAOWxIRrKId7UyZLGVEGj1igRw4rhiiDmohS1kA) on this snippet (`reportPossiblyUnboundVariable`). (Mypy's [`possibly-undefined` optional error code](https://mypy.readthedocs.io/en/stable/error_code_list2.html#warn-about-variables-that-are-defined-only-in-some-execution-paths-possibly-undefined) _doesn't_ emit an error on this exact snippet -- I actually think that's possibly a mypy bug, but I digress -- but it does emit an error on a small variation of it.)

<details>
<summary>A small variation which mypy will also complain about if you pass `--enable-error-code=possibly-undefined` to mypy on the command line</summary>

```py
from collections.abc import Iterable

def foo(it: Iterable[int]) -> int:
    for i in it:
        x = i
    return x + 42
```

</details>

A safer pattern (albeit a slightly annoying one for situations where _you_ know the iterable is non-empty even though the static-analysis tool _doesn't_, such as in your original snippet) is to do something like this:

```py
from collections.abc import Iterable

def foo(it: Iterable[int]) -> int:
    i: int | None = None
    for i in it:
        pass
    assert i is not None, "Empty iterable was passed to foo()!"
    return i + 42
```

The flake8-bugbear rules are intended to be opinionated rules, they're not enabled by default, and I think there are good reasons to want to avoid this pattern. So I would personally vote for leaving this rule as-is. (We could certainly explore improving the documentation for the rule, however.)

---

_Comment by @jaraco on 2024-04-02 20:27_

> [tools] cannot know that `i` is bound

Indeed, and using the variable in the loop doesn't help. Modifying your example slightly:

```python
from collections.abc import Iterable

def foo(it: Iterable[int]) -> int:
    for i in it:
        log(i)
    return i + 42
```

```python
from collections.abc import Iterable

def foo(it: Iterable[int]) -> int:
    for _i in it:
        pass
    return _i + 42
```

In either case, the code no longer fails `B007`, but still is subject to the same failure with `foo([])`. If the goal is to ensure people don't access a possibly unbound loop variable outside of the loop, that should probably be a different check.


> they're not enabled by default

Oh. Maybe that's my mistake. I encountered this issue by setting `--select B`, and I was assuming that meant opt into the recommended B rules, but maybe it means opt into all B rules, in which case, my opt-in suggestion was misguided. Reading the docs briefly, that seems to be the case.



> We could certainly explore improving the documentation for the rule, however.

I'd be happy to help draft the change. Let me know if that would be valuable or assign it to me and I'll make a PR.

---

_Comment by @AlexWaygood on 2024-04-03 13:58_

> If the goal is to ensure people don't access a possibly unbound loop variable outside of the loop, that should probably be a different check.

Agreed that if this were the goal, then it should be enforced in a more uniform way via a separate rule. It doesn't look like we currently have such a rule, but I think we should consider adding one.

However, the question (as I see it) isn't whether it should be the goal of this check to enforce that people don't access a possibly-unbound loop variable outside of the loop -- B007 is not currently _complaining_ on the use of the variable outside of the loop. The original report was that using the variable after the loop did not _affect_ whether ruff (via B007) complained that the loop variable was unused within the loop. So the question, as I see it, is whether we should add complexity -- both complexity in terms of the rule's implementation, and conceptual complexity in what the rule is doing -- in order to account for a coding pattern that is reasonably rare, and is probably not advisable in the first place (accessing a loop variable outside the loop).

Why do I say this would add complexity to the implementation? Because currently, the rule only analyses the loop itself to see whether the variable is used within the loop:

https://github.com/astral-sh/ruff/blob/d467aa78c228d7447187486167feeb6fa3faaf99/crates/ruff_linter/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs#L88-L94

If we wanted to make sure that the rule was not emitted if the variable was used after the loop, we would have to analyse the whole scope in which the loop was defined to see whether it was used after the loop without having previously been redefined after the loop.

Why do I say this would add conceptual complexity? Because, while the documentation might be lacking, I feel like this rule is currently quite easy to explain: "As a matter of style, loop control variables should always be used inside the loop". If we start making special cases for things like this, it becomes harder to explain to users. (If we make a change here without flake8-bugbear also making this change, then we also need to start explaining to users why our implementation differs from theirs.)

> I encountered this issue by setting `--select B`, and I was assuming that meant opt into the recommended B rules, but maybe it means opt into all B rules

Yeah, that will enable all `B` rules. I'm currently working on a proposal for recategorising ruff's rules, which should help with this kind of confusion.

> I'd be happy to help draft the change. Let me know if that would be valuable or assign it to me and I'll make a PR.

Yeah, I think it would be valuable to add a short note explaining that this rule doesn't care if the loop variable is used outside the loop, and illustrating alternative, safer patterns people can use if they encounter this problem.

---

_Comment by @MichaReiser on 2024-04-04 09:13_

> If the goal is to ensure people don't access a possibly unbound loop variable outside of the loop, that should probably be a different check.

Agree, although I'm not sure if that should be a lint rule. I think that's something that fits nicely into a type checker (or other correctness static analysis tooling). E.g. I think Pyright can do that for you

---

_Comment by @LincolnPuzey on 2024-10-23 08:27_

> If the goal is to ensure people don't access a possibly unbound loop variable outside of the loop, that should probably be a different check.

I was going to open an issue requesting this rule, then saw #13742 already did so.

---

_Comment by @xixixao on 2024-11-12 14:39_

Can we tell people:

> Loop control variable `foo` not used within loop body. If it's needed outside of it, assign it explicitly to another variable.

As presumably the simple fix is going from:
```py
for a in foo:
  ... # some code here I'm not showing
print(a)
```
to:
```py
a = None
for b in foo:
  a = b
  ... # some code here I'm not showing
print(a)
```

(although "simple" is perhaps inaccurate, as the order of the code in the loop and the assignment matters)


---

_Comment by @smurfix on 2025-09-28 18:17_

I just got bitten by this.

While there's a lot of disagreement how exactly to proceed here, IMHO something we all can agree on is that
```
for a in some_iter:
    ... # do whatever but don't look at a
work_with(a)
```
is a legitimate, if perhaps somewhat objectionable, piece of code (*if* you can ensure that the iterator is not empty).

My problem here is that ruff complains that the loop variable is unused, but does *not* complain that `a` in line three is (or may be) unset- It thus helpfully tells us to replace it with `_a` or whatever … which breaks the code.

Shouldn't we, at the very least, fix *that* little problem?

---
