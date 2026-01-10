```yaml
number: 13023
title: "[async-function-with-timeout] Disable check for asyncio before Python 3.11 (ASYNC109)"
type: pull_request
state: merged
author: jessekv
labels:
  - rule
assignees: []
merged: true
base: main
head: main
created_at: 2024-08-21T07:28:55Z
updated_at: 2024-08-23T08:12:14Z
url: https://github.com/astral-sh/ruff/pull/13023
synced_at: 2026-01-10T21:38:32Z
```

# [async-function-with-timeout] Disable check for asyncio before Python 3.11 (ASYNC109)

---

_Pull request opened by @jessekv on 2024-08-21 07:28_

Some (occasionally tangential) discussion is found here: https://github.com/astral-sh/ruff/issues/12353

## Summary

Before, the diagnostic was raised for all supported versions of Python. However, `asyncio` only introduced `asyncio.timeout` in python version 3.11.

Now, the diagnostic is not raised when using `asyncio` with Python versions before 3.11

## Test Plan

It's a policy tweak leveraging well-tested library features. I don't see an obvious need for an additional test, but I would be happy to receive guidance on how to add a test.




---

_Comment by @codspeed-hq[bot] on 2024-08-21 07:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vdwees:main)

### Merging #13023 will **improve performances by 10.62%**

<sub>Comparing <code>vdwees:main</code> (ad23c48) with <code>main</code> (cfe25ab)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `vdwees:main` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 911 µs | 823.5 µs | +10.62% |


---

_Comment by @github-actions[bot] on 2024-08-21 07:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-08-22 07:59_

Thanks. This does make sense to me. However, it does mean that the rule now deviates from the upstream rule (I'm not sure if `flake8-async` has access to the target Python version). @AlexWaygood what's your take on this?

https://github.com/python-trio/flake8-async/blob/225f15a98db4bb8ce4600127fd0fe90bc5c9c04b/flake8_async/visitors/visitors.py#L43C7-L60


It would be great if we can add a test demonstrating that this is working as expected (flags asyncio but not anyio)

You can add the test here

https://github.com/astral-sh/ruff/blob/45f459bafd0663fd650dcd5750247f10a9f9126b/crates/ruff_linter/src/rules/flake8_async/mod.rs#L18-L39

and you can use the code below (you have to change the path to the python file)

```rust
    #[test]
    fn async109_python_310_or_odler() -> Result<()> {
        let diagnostics = test_path(
            Path::new("flake8_async").join("path_to_test_File"),
            &LinterSettings {
                target_version: PythonVersion::Py310,
                ..LinterSettings::for_rule(Rule::AsyncFunctionWithTimeout)
            },
        )?;
        assert_messages!(diagnostics);
        Ok(())
    }
```

---

_Label `rule` added by @MichaReiser on 2024-08-22 07:59_

---

_Comment by @AlexWaygood on 2024-08-22 15:42_

> Thanks. This does make sense to me. However, it does mean that the rule now deviates from the upstream rule (I'm not sure if `flake8-async` has access to the target Python version). @AlexWaygood what's your take on this?

Yeah, this change definitely makes sense to me! There are third-party libraries backporting `asyncio.timeout` to earlier Python versions, but I think that's irrelevant to the changes this PR is making. So this looks good, once it has a test added!

---

_@MichaReiser requested changes on 2024-08-22 16:08_

Perfect. Let's add a test and then merge this

---

_Assigned to @MichaReiser by @MichaReiser on 2024-08-23 07:38_

---

_@MichaReiser approved on 2024-08-23 07:42_

---

_Merged by @MichaReiser on 2024-08-23 08:02_

---

_Closed by @MichaReiser on 2024-08-23 08:02_

---
