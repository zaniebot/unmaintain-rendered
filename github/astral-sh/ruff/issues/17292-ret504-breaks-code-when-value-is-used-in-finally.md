---
number: 17292
title: RET504 breaks code when value is used in finally
type: issue
state: open
author: wikti
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-04-08T10:28:55Z
updated_at: 2025-07-05T15:57:07Z
url: https://github.com/astral-sh/ruff/issues/17292
synced_at: 2026-01-07T13:12:16-06:00
---

# RET504 breaks code when value is used in finally

---

_Issue opened by @wikti on 2025-04-08 10:28_

### Summary

Had problem with rule RET504. Autofix broke code. 

```
def log_out():
    out = ""
    try:
        out = foo()
        return out
    except Exception as e:
        out = str(e)
    finally:
        log(out)
```

Fix made `return foo()`, which broke `log(out)`.

Tested on version `0.11.4`.

Thanks for great project.

---

_Label `bug` added by @dylwil3 on 2025-04-08 10:58_

---

_Label `fixes` added by @dylwil3 on 2025-04-08 10:58_

---

_Label `bug` removed by @dylwil3 on 2025-04-08 11:12_

---

_Comment by @dylwil3 on 2025-04-08 11:19_

Good catch! I was going to suggest that we make the fix unsafe if we detect this lint in a `try` block or in an `except` block for a try statement that also has a `finally` clause (e.g. by modifying the [`ReturnVisitor`](https://github.com/astral-sh/ruff/blob/b662c3ff7ea7ccf9bb8bd4d0814a20c082c7c244/crates/ruff_linter/src/rules/flake8_return/visitor.rs#L40) to collect this information). But it turns out the fix is _always_ marked as unsafe, it's just not documented (an instance of #15584).

So I think first of all we should document the fix safety.

But then I'm a bit conflicted about resolving it further. We could:

1. Offer the lint but not the fix if we are in this situation described above
2. Try to do a finer analysis where we check for uses of the binding in later blocks
3. Do nothing aside from adding documentation
4. Something else

(1) has the downside that there may be lots of situations with a `try` statement where the fix does not break the code, and where now no quick fix is offered. (2) stretches the limit of Ruff's current control flow abilities (hopefully not for long #17065
), and may lead to further bugs or edge cases. (3) perhaps feels unsatisfying.

Let's get the documentation sorted and I'll leave this here for further discussion.

---

_Label `documentation` added by @dylwil3 on 2025-04-08 11:19_

---

_Comment by @MichaReiser on 2025-04-08 11:25_

I think this is simply a false positive because the assignment isn't unnecessary. One possibility is to make the rule a deferred rule and use the semantic model to verify that the return is the only read. Another alternative is to extend the `ReturnsVisitor` to possibly track the symbols used inside of `except` or `finally` blocks.

---

_Label `bug` added by @dylwil3 on 2025-04-08 11:42_

---

_Comment by @dylwil3 on 2025-04-08 11:54_

You're right, I got mixed up thinking about handling the general situation of safely spotting `RET504` in try statements: of course here there should be no lint at all.

---

_Label `documentation` removed by @dhruvmanila on 2025-04-14 14:07_

---

_Comment by @joaoe on 2025-05-23 08:43_

+1 I was just about to report this.

My case
```
def execute_something()
    result, err, start = (), None, time.monotonic()
    try:
        logging.info("Starting...")
        result = do_some_long_operation()
        return result ## <- RET504 HERE
    except Exception as e:
        err = repr(e)
        raise
    finally:
        end = time.monotonic()
        logging.log(logging.ERROR if err else logging.INFO, "Result %s, elapsed %.3f secs", err if err else f"{len(result)} rows", end - start)
```



---

_Comment by @mxmlnkn on 2025-07-05 15:57_

I have a encountered this problem in [fusepy](https://github.com/fusepy/fusepy/blob/5d997d6706cc0204e1b3ca679651485a7e7dda49/fuse.py#L1251).

---
