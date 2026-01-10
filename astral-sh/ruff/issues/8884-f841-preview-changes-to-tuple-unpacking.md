```yaml
number: 8884
title: F841 preview changes to tuple unpacking
type: issue
state: closed
author: hauntsaninja
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2023-11-28T23:25:18Z
updated_at: 2025-03-03T09:51:37Z
url: https://github.com/astral-sh/ruff/issues/8884
synced_at: 2026-01-10T11:09:51Z
```

# F841 preview changes to tuple unpacking

---

_Issue opened by @hauntsaninja on 2023-11-28 23:25_

https://github.com/astral-sh/ruff/pull/8489 changes F841 so that it complains about unused variables resulting from tuple unpacking.

There are a few issues:
- It's not clear to all folks that this can be disabled by prefixing the variable with an underscore (and unlike other cases, you can't just get rid of the variable / var name is often good documentation)
- ~An autofix to automatically add the underscore prefix would help!~ Implemented in https://github.com/astral-sh/ruff/pull/9107
- We actually currently mark F841 as unfixable, because it often keeps the RHS around (in case of side effect), and this is usually not the fix we want. So maybe both autofix and a custom error message?
- Maybe best option is separate error code. A good parallel is how `B007` is separate from `F841`. This also helps if you don't find the new lint useful, which I don't really

---

_Comment by @zanieb on 2023-11-29 04:02_

Thanks for issue.

I think the first step may be to just change the violation message to suggest using a `_`.

It seems like a separate error code might help with your `unfixable` goal, but I'm a little hesitant. Like... what if you have

```python
unused_a, unused_b = foo()
```

We should probably still allow a fix for this to remove both variables as they would for

```python
unused_c = foo()
```

Perhaps you could try demoting the fix for F841 to unsafe?




---

_Label `good first issue` added by @zanieb on 2023-11-29 04:02_

---

_Label `rule` added by @zanieb on 2023-11-29 04:02_

---

_Label `preview` added by @zanieb on 2023-11-29 04:02_

---

_Comment by @hauntsaninja on 2023-11-29 05:31_

Thanks, yup, custom error message mentioning underscore prefix is definitely a good step and is maybe enough to solve this issue.

Note one reason why separate error code is viable here is that flake8 doesn't complain about this tuple unpacking case.

The reason I mention the `unfixable` thing is that we use ruff's autofixes in pre-commit, and I would always want the (not yet written) autofix for adding an underscore in tuple unpacking, but I never want the autofix that only removes LHS, and this is probably a weak argument in favour of separate error code.

---

_Comment by @bluetech on 2023-12-04 16:46_

I'd like to voice my support a separate error code for unused variable in unpacking. The regular F841 is unambiguously useful, but having to add underscores for the unpacking case is just noise in most cases.

There are *some* cases where it might be useful, e.g. if trying to mimic Go-style error handling in Python, `foo, err = func()`, don't want `err` to go unused. So I'm not suggesting to remove the feature entirely.

Other languages have a special treatment for a `_` identifier which just skips the binding, but there's no such thing in Python. Two issues with using `_` anyway: it is often used as a `gettext()` alias, and if it's used for more than one type then mypy complains.

---

_Comment by @zanieb on 2023-12-04 16:54_

@bluetech I think the proposal here is that we'd avoid raising a violation for `x, _y = func()` as well as `x, _ = func()` which should avoid the rebinding issue.


---

_Comment by @bluetech on 2023-12-04 18:27_

@zanieb I may be misunderstanding your message, but as far as I can see, `x, _y = func()` and `x, _ = func()` already don't raise a violation. My main trouble is with `a, unused_b = func()`, the current fixes for this are:

- `a, _unused_b = func()` - noise, I prefer to just keep it `a, unused_b = func()` in this case -- also helpful in case `unused_b` actually does find some use.

- `a, _ = func()` - conflict with `gettext`, rebinding issue (in case there's more than one tuple unpacking, or multiple `_` in the same unpacking). Also I think it's nice to let the reader know what they're missing, sometimes.

(To be clear, I'll completely accept an answer that my preference is too specific and not worth the complexity and I should just use `a, _unused_b = func()` and get over it :grinning: )

---

_Comment by @zanieb on 2023-12-05 16:34_

Hm. Personally, I don't see a violation for `a, unused_b = func()` as any noiser than the one for `unused = func()` — I'm not sure why tuple unpacking deserves a special case for unused variables. Are there other ecosystems that use separate rule codes for these cases? I find prefixing unused variables from unpacked tuples in Rust as very clear and consistent with other unused variable diagnostics.

---

_Comment by @hauntsaninja on 2023-12-05 17:35_

The difference is that (in codebases I work on) it's pretty common to unpack tuples and not use all the elements. There isn't dead code to be removed and so the signal to noise ratio ends up being different (especially without autofix).

The other difference is that the autofix for the old behaviour is something I don't usually want (it's too conservative and can end up leaving dead code confusingly looking like it has side effects), whereas an autofix for new behaviour is something I'd always want. 

Note that ruff already has different codes for different kinds of unused variables: B007 is already a different error code. 

---

_Comment by @bluetech on 2023-12-05 17:37_

To add my answer:

The difference is, with `unused = func()` the obvious fix is to just remove `unused = `, which is clearly better. But with `a, unused_b = func()`, you have to fix in one of the two ways I mentioned above, and I tried to explain why I don't like them.

In Rust `_` is special non-binding identifier ([underscore expression](https://doc.rust-lang.org/reference/expressions/underscore-expr.html)), and gettext `_('my string')` alias is not a thing there AFAIK, so the main Python downsides are fixed.

---

_Comment by @TeamSpen210 on 2023-12-23 07:23_

For a separate error code, there is an edge case which may want to be treated differently - if all members in the unpacking are unused, it probably should still produce `F841`. 

---

_Comment by @hauntsaninja on 2023-12-23 19:01_

Maybe... note such an unpacking isn't safe to remove entirely because it acts as an assertion on length.

@zanieb @charliermarsh would you accept a PR moving this check to a different error code? (There's no other way of getting my ideal autofix behaviour here is there?)

---

_Comment by @charliermarsh on 2023-12-23 19:26_

@hauntsaninja -- I'm fine with moving this to a separate rule like `unused-tuple-element` since B007 is something of a precedent there and the fix is indeed different. Others may disagree (dunno if @zanieb will agree) but at least from my perspective it seems reasonable. I probably would want F841 to continue flagging tuples that are _entirely_ unused (as it does on stable, IIRC).


---

_Comment by @charliermarsh on 2023-12-23 19:26_

To be clear, your "ideal autofix" is that we _do_ offer the "prefix unused tuple elements" fix but have a way to disable the "remove the left-hand side" autofix?

---

_Comment by @hauntsaninja on 2023-12-23 19:37_

Yes, that is my ideal autofix behaviour.

Context:
We're a big user of autofixing (including unsafe fixes) and the "remove the left hand side" autofix for normal unused variables is a little too conservative for us. It makes pure function calls look impure, which then becomes confusing to future readers of the code, so we had F841 marked as unfixable. Whereas the unpacking lint is always fine to autofix (and in my opinion, is only really tolerable when autofixed, given how commonly it occurs for us and the tiny value it provides).

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:22_

---

_Removed from milestone `v0.2.0` by @dhruvmanila on 2024-02-12 05:16_

---

_Added to milestone `v0.3.0` by @dhruvmanila on 2024-02-12 05:16_

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-14 16:28_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-14 16:28_

---

_Label `good first issue` removed by @MichaReiser on 2024-03-28 17:25_

---

_Label `preview` removed by @MichaReiser on 2024-03-28 17:25_

---

_Label `needs-decision` added by @MichaReiser on 2024-03-28 17:25_

---

_Comment by @MichaReiser on 2024-03-28 17:28_

I'm returning this to "needs-decision" after we've discussed this more internally. From my notes:

- It’s unclear to me why we should have multiple rules to track unused variables. I agree that `B007` sets some precedence but as far as I understand it’s more narrow in that the variable might be used outside of the loop, but it isn’t used inside of the loop.
- I understand that the main motivation for a separate rule is to allow different fixability. I don't know if that's reason enough and could set a bad precedence. I think we should consider alternative options to address this specific concern that doesn't require splitting rules. 

We concluded that we need to develop a guideline that specifies when it is okay to have multiple rules that detect the same or very similar code smell and when it is not. 

---

_Removed from milestone `v0.4.0` by @MichaReiser on 2024-04-04 09:08_

---

_Comment by @AlexWaygood on 2024-08-12 11:17_

I would support splitting this out into a separate check. In general, I think it's very useful for users for rules to be as fine-grained as possible so that they can easily select exactly which diagnostics and fixes they find to be helpful for them.

This also seems to be to be distinct enough that I think users would understand why this would be a separate check. For an assignment that doesn't involve unpacking, such as `x = foo()`, or `x: str = foo()`, it's nearly always a mistake to leave the variable unused. You'll either want to delete the line entirely, or (if the function has side effects), call the function without assigning it to a variable. But unused variables that were assigned via unpacking are really more a stylistic error than something that you can say with confidence was a "mistake". Giving unpacked variables descriptive names (`(a, b, c, foo)`) _can_ be considered much more readable than unpacking a tuple as `(_, _, _, foo)`.

> * I understand that the main motivation for a separate rule is to allow different fixability. I don't know if that's reason enough and could set a bad precedence.

I think for a lot of our users, the ability we have to provide autofixes is a key value add over other tools in the Python ecosystem. I know when I first started using Ruff, this was a far bigger attraction than the performance gains that Ruff boasted. I think changes that make it easier for people to enable autofixes on their code are pretty valuable.

---

_Comment by @mergu on 2024-11-15 08:00_

We ran into this issue while migrating from flake8 to ruff, and support this change being split into another rule. F841 hasn't been a controversial error code to fix for us under flake8. In addition to what's been said above, I'll also add:
- Many pycodestyle (E) rules are locked behind preview mode, so migrating from flake8 to ruff requires enabling preview mode for parity. This means we have to choose between fixing many new F841 violations or turn off a previously useful rule.
- Prefixing these vars with _ is going to propagate the underscored vars through the repos when engineers decide to use them in the future. We'll end up with `x, _y, z = foo()` and `_y` being passed around and logged without a good understanding of why it was prefixed in the past.
- On a similar note, giving all unpacked variables a name (instead of naming them `_`) is helpful for readability and future maintenance.

Our best solution for now is to ignore F841 in our preview-enabled config and run it separately via `ruff check . --config ruff.toml -q && ruff check . --config ruff.toml --no-preview --select F841` .. a new rule that can be optionally ignored would be more ideal

---

_Comment by @un-pogaz on 2025-01-12 10:22_

I'm added to the idea of splitting this rule for tuple unpacking. I can understand the reluctance to create different rules for similar situations, but I think that the all of this discussion proves that the unpacking of tuple is a behavior specific enough to have a distinc implementation.

---

_Comment by @AlexWaygood on 2025-01-13 12:27_

Okay, let's go ahead and split this out into a separate check (probably in the RUF category). Thanks for the discussion, all! PRs for this are welcome.

---

_Label `needs-decision` removed by @AlexWaygood on 2025-01-13 12:27_

---

_Label `help wanted` added by @AlexWaygood on 2025-01-13 12:27_

---

_Label `accepted` added by @AlexWaygood on 2025-01-13 12:27_

---

_Closed by @MichaReiser on 2025-03-03 09:51_

---
