```yaml
number: 12427
title: "When raising `NotImplementedError `, ARG002 and EM101 are incompatible."
type: issue
state: closed
author: DaniBodor
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-07-21T12:31:38Z
updated_at: 2024-10-18T06:44:24Z
url: https://github.com/astral-sh/ruff/issues/12427
synced_at: 2026-01-10T11:09:54Z
```

# When raising `NotImplementedError `, ARG002 and EM101 are incompatible.

---

_Issue opened by @DaniBodor on 2024-07-21 12:31_

[ARG002](https://docs.astral.sh/ruff/rules/unused-method-argument/) is usually not raised in methods that immediately raise a `NotImplementedError`, but does if there is some other code in the method. However, [EM101](https://docs.astral.sh/ruff/rules/raw-string-in-exception/) enforces adding the error message into a separate variable, which is then seen as "code".

The result of this is that there is no way to write such a method without raising one or the other issue. See example below.

```python
class EM101_found:
    def test_method(self, **kwargs):
        raise NotImplementedError("error message.")


class ARG002_found:
    def test_method(self, **kwargs):
        msg = "error message"
        raise NotImplementedError(msg)
```

running `ruff check test_file.py --select ARG002 --select EM101` on the code snippet above results in
> test_file.py:3:35: EM101 Exception must not use a string literal, assign to variable first
> test_file.py:7:29: ARG002 Unused method argument: `kwargs`
> Found 2 errors.

---

_Label `bug` added by @charliermarsh on 2024-07-21 12:47_

---

_Comment by @ast0815 on 2024-09-02 11:55_

It's a bit of potentially unnecessary faff, but you can write these methods like this:
```Python
    def method(self, x):
        _ = x
        msg = "Something"
        raise NotImplementedError(msg)
```

---

_Comment by @DaniBodor on 2024-09-06 12:31_

> It's a bit of potentially unnecessary faff, but you can write these methods like this:
> 
> ```python
>     def method(self, x):
>         _ = x
>         msg = "Something"
>         raise NotImplementedError(msg)
> ```

I think rather than this, I would prefer suppressing one or the other of the rules (which is what I am doing now).

---

_Comment by @ast0815 on 2024-09-09 11:33_

I switched to `abc.ABC` and the `@abstractmethod` decorator. Yet another way around this.

---

_Comment by @Rupt on 2024-09-20 05:18_

[Google's style guide](https://google.github.io/styleguide/pyguide.html#214-decision) recommends `del`ing the unused argument.

```python
    def method(self, x):
        del x  # unused
        msg = "Something"
        raise NotImplementedError(msg)
```

---

_Comment by @DaniBodor on 2024-09-20 08:31_

Interesting. I guess this should be at least mentioned in the ruff documentation of these errors as well.
For the example I gave above, I think this would require looping through the kwargs to delete them all, which also feels clunky.

Also, these all feel like workarounds for an issue that I feel doesn't really needs to be flagged in the first place.

---

_Label `help wanted` added by @zanieb on 2024-09-24 19:52_

---

_Comment by @zanieb on 2024-09-24 19:52_

It seems reasonable to avoid this false positive in ARG002. I'm not sure how hard it will be.

---

_Comment by @Watercycle on 2024-09-30 05:48_

Here's [the rule](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs#L315), here's the original Python [is_stub_function](https://github.com/nhoad/flake8-unused-arguments/blob/master/flake8_unused_arguments.py#L238-L273), and here's Ruff's [is_stub](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/analyze/function_type.rs#L127-L146).

It seems like a simple fix would be expanding the definition of `is_stub` to allow primitive string assignment with something along the lines of the below code.

But, going down that route, it's likely worth documenting what the cut-off for a "stub" is. That if you need things like `str + str`, `f-string`, and/or other functional calls, you should really just use alternatives like ABC (added in 3.4) or `# noqa` to disable one of the rules.

```rust
Stmt::Assign(StmtAssign { value, .. }) => {
    matches!(value.as_ref(), Expr::StringLiteral(_))
}
```

Does that sound reasonable?

---

_Comment by @dhruvmanila on 2024-10-01 03:49_

> It seems like a simple fix would be expanding the definition of `is_stub` to allow primitive string assignment with something along the lines of the below code.

I don't think this will work completely because it'll start giving false negatives. What we need to do is to add a new condition which will be checked if the first condition is false. This will check whether there are exactly two statements and they match the following pattern:
```py
variable = <string | f-string>
raise NotImplementedError(variable)
```
Note that this condition should only be checked locally to `ARG002` implementation and not be added to `is_stub` function as that's being used in other places as well. So, a new function can be added in `unused_arguments.rs` and that can be used in `ARG002` implementation. Does this make sense?

---

_Comment by @Watercycle on 2024-10-01 21:56_

Yes, that's fair! My inclination to modify `is_stub` is mostly because of naming.

But, to your point, it's probably better not to pollute the definition of `is_stub` until enough rules have common logic to warrant abstracting that out.

How does having a very specific `is_implementation_stub` function in `unused_arguments.rs` for now sound?

---

_Comment by @dhruvmanila on 2024-10-04 04:34_

> How does having a very specific `is_implementation_stub` function in `unused_arguments.rs` for now sound?

We can discuss the naming in the PR but yeah that sounds about right

---

_Closed by @dhruvmanila on 2024-10-18 06:44_

---

_Closed by @dhruvmanila on 2024-10-18 06:44_

---
