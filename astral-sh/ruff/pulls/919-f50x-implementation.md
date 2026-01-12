```yaml
number: 919
title: F50x implementation
type: pull_request
state: merged
author: olliemath
labels: []
assignees: []
merged: true
base: main
head: f50x_implementation
created_at: 2022-11-27T02:57:34Z
updated_at: 2022-11-28T10:23:54Z
url: https://github.com/astral-sh/ruff/pull/919
synced_at: 2026-01-12T05:48:46Z
```

# F50x implementation

---

_Pull request opened by @olliemath on 2022-11-27 02:57_

Closes https://github.com/charliermarsh/ruff/issues/905

This uses a vendored copy of `cformat.rs` from rustpython-vm to parse the format string. Then we perform our checks in a similar manner to the F52x checks.

### Extensions?

I wrote a version of this code that looked inside splatted expressions like `{"a": 1, **b, "c": 2}` (which the interpreter parses identically to `{"a": 1, **b, **{"c": 2}}`. It could detect if e.g. `"c"` from the earlier example was unused in a format string, however it seemed overly complicated for such a small edge-case, which is not handled in pyflakes.

Along with `CFormatString`, rustpython-vm provides a `CFormatBytes` that could easily be used to perform the same checks on `b"..." % ...` format expressions. On the other hand, I don't think this is super valuable: pyflakes doesn't provide it and without a lot of thought the error messages may look horrible (raw bytes).

---

_Comment by @charliermarsh on 2022-11-27 04:52_

This looks _great_! Anything else you want to do here or good to merge from your perspective?

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:148 on 2022-11-27 04:53_

We might be able to delay these `cloned` calls until the `else` block? Like make this `Vec<&str>` and then call `cloned` when creating (e.g.) `CheckKind::PercentFormatMissingArgument`? Really minor though (and didn't try it myself).

---

_@charliermarsh reviewed on 2022-11-27 04:53_

---

_Comment by @olliemath on 2022-11-27 11:57_

> This looks _great_! Anything else you want to do here or good to merge from your perspective?

Right now the code picks up some issues pyflakes doesn't - I think the particular cases are useful, unless you'd prefer to have direct parity?

The only one I'm unsure about is in F504, where we are able to detect this while pyflakes is not:
```python
"%(a)d" % {"b": 1, **d}  # b is unused in the formatting - flagged by ruff
```

For the average user of ruff, I think the fact that the above is detected but the following is _not detected_ violates the principal of least surprise:
```python
"%(a)d" % {**d, "b": 1}  # b is unused in the formatting - not flagged by ruff
```
Because of the way this is parsed, we would have to check the keys 1 level down in any splatted dict literals to spot this.  What do you think of adding this extra check, or else giving up on splatted dicts entirely to keep the linting behaviour predictable?







---

_@olliemath reviewed on 2022-11-27 12:06_

---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:148 on 2022-11-27 12:06_

Yeah, that's possible - we actually get a `Vec<&String>` and then put `missing.iter().map(|&s| s.clone()).collect()` into the check. 

I had assumed (possibly naively) that `cloned` would have zero overhead if the filter produces an empty iterator (which is what the if/else block is checking), if that's the case it's probably simpler to keep cloned where it is? 

---

_@olliemath reviewed on 2022-11-27 12:09_

---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:148 on 2022-11-27 12:09_

Unless the move is more about signalling intent than performance?

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:148 on 2022-11-27 14:48_

Yeah I'm sure the difference is negligible if existent at all given that we're only cloning the empty `vec` anyway in that case. It's more about signaling intent, locally and broadly ("clone as late as possible"), but honestly don't feel strongly about changing it.

---

_@charliermarsh reviewed on 2022-11-27 14:48_

---

_Comment by @charliermarsh on 2022-11-27 14:54_

Yeah I'd probably vote to ignore splatted dict literals given your reasoning above.


---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:148 on 2022-11-28 00:08_

Makes sense - I've moved cloning later in the 3 functions where it was clear how to do so

---

_@olliemath reviewed on 2022-11-28 00:08_

---

_Comment by @olliemath on 2022-11-28 00:09_

Cool, so I've disabled the introspection of splatted dicts, which gives behaviour more consistent (both internally and with pyflakes).

---

_Renamed from "Draft: F50x implementation" to "F50x implementation" by @olliemath on 2022-11-28 00:09_

---

_Comment by @olliemath on 2022-11-28 00:09_

I'd say this is good to go from my side if the pipeline passes

---

_Comment by @charliermarsh on 2022-11-28 00:10_

Awesome, will merge tonight.

---

_Merged by @charliermarsh on 2022-11-28 02:30_

---

_Closed by @charliermarsh on 2022-11-28 02:30_

---

_Branch deleted on 2022-11-28 10:23_

---
