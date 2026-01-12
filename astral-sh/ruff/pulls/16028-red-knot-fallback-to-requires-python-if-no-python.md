```yaml
number: 16028
title: "[red-knot] Fallback to `requires-python` if no `python-version` is specified"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/requires-python
created_at: 2025-02-07T18:58:12Z
updated_at: 2025-02-12T11:48:44Z
url: https://github.com/astral-sh/ruff/pull/16028
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Fallback to `requires-python` if no `python-version` is specified

---

_@MichaReiser_

## Summary

Add support for the `project.requires-python` field in `pyproject.toml` files. 

Fall back to the resolved lower bound of `project.requires-python` if the `environment.python-version` field is `None` (or more accurately, initialize `environment.python-version with `requires-python`'s lower bound if left unspecified). 

## UX design

There are two options on how we can handle the fallback to `requires-python`'s lower bound:

1. Store the resolved lower bound in `environment.python-version` if that field is `None` (Implemented in this PR)
2. Store the `requires-python` constraint separately. 

There's no observed difference unless a user-level configuration (or any other inherited configuration is used). Let's discuss it on the given example


**User configuration**

```toml
[environment]
python-version = "3.10"
```

**Project configuration (`pyproject.toml`)**

```toml
[project]
name = "test"
requires-python = ">= 3.12"

[tool.knot]
# No environment table
```

The resolved version for 1. is 3.12 because the `requires-python` constraint precedence takes precedence over the `python-version` in the user configuration. 2. resolves to 3.10 because all `python-version` constraints take precedence before falling back to `requires-python`. 

Ruff implements 1. It's also the easier to implement and it does seem intuitive to me that the more local `requires-python` constraint takes precedence.


## Test plan

Added CLI and unit tests.

---

_@MichaReiser reviewed on 2025-02-08 16:32_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/pyproject.rs`:69 on 2025-02-08 16:32_

Thanks to @konstin for pointing me toward the logic in uv. This matches what uv does. Thankfully, most of the work is done by the pep0440 crate.

---

_Renamed from "Fallback to requires python if no python-version is specified" to "[red-knot] Fallback to requires python if no python-version is specified" by @MichaReiser on 2025-02-08 16:33_

---

_Label `configuration` added by @MichaReiser on 2025-02-08 16:33_

---

_Label `red-knot` added by @MichaReiser on 2025-02-08 16:33_

---

_Marked ready for review by @MichaReiser on 2025-02-08 16:41_

---

_Review requested from @carljm by @MichaReiser on 2025-02-08 16:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-08 16:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-08 16:41_

---

_Renamed from "[red-knot] Fallback to requires python if no python-version is specified" to "[red-knot] Fallback to `requires-python` if no `python-version` is specified" by @MichaReiser on 2025-02-08 16:48_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:77 on 2025-02-11 08:43_

Maybe something like
```suggestion
        // If the options don't specify a python version but the `project.requires-python` field is set and includes a lower bound, use that instead
```

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:87 on 2025-02-11 08:45_

Should we do something like building the minimum between the `requires_python` lower bound and our minimum supported version here?

I find it surprising that I get a Python version of 3.9 if I don't specify anything, but a Python version of 3.6 if I set `requires-python = ">= 3.6"`.

Conversely, it might be surprising that upper bounds are ignored completely. If I set `requires-python = "<3.6"`, I still get a Python version of 3.9.

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:593 on 2025-02-11 08:48_

Why does this have a `_no_python_version` suffix, while the other tests below do not? Remove the suffix? Maybe call this
```suggestion
    fn requires_python_major_minor() -> anyhow::Result<()> {
```

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata.rs`:610 on 2025-02-11 08:59_

Minor: do these tests need to be snapshot tests? I would find them more readable if they would include a simple `assert_eq!`, I think. Something like
```suggestion
assert_eq!(
            root.options
                .environment
                .unwrap()
                .python_version
                .unwrap()
                .to_string(),
            "3.12"
        );
```

---

_@sharkdp approved on 2025-02-11 09:00_

Thank you. A few minor comments and one higher-level question.

---

_@MichaReiser reviewed on 2025-02-11 09:59_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata.rs`:87 on 2025-02-11 09:59_

That's a great catch. I'm not sure if it's worth special casing because:

* https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#python-requires Calls it out that it is the minimum python version supported and using `<` doesn't make sense
* https://discuss.python.org/t/requires-python-upper-limits/12663 and later https://discuss.python.org/t/requires-python-upper-limits/12663/84 both mention that the field is intended as lower bound and not an upper bound. Pip doesn't seem to support the upper bound. (Shared by @konstin )

We could error if we see a requires python constraint without a lower bound over silently ignoring. I'd be interested in @AlexWaygood or @carljm take on this.

---

_@AlexWaygood reviewed on 2025-02-11 10:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/metadata.rs`:87 on 2025-02-11 10:49_

Interesting questions.

I wondered whether it might be possible for red-knot to have users who have a pyproject.toml file (and a `requires-python` field in that pyproject.toml file) without actually ever intending their project to be built as a package while a wheel. If so, the packaging.python.org guide might not be relevant here; they might _only_ be using the `requires-python` field as a configuration setting for tools such as Ruff and red-knot. But on second thought, I don't think this is worth worrying about _too_ much. The `requires-python` field is part of the `[project]` table in `pyproject.toml`; it seems reasonable to assume that users who include a `[project]` table are intending their code to be built and distributed as a wheel. So I think we _can_ safely assume "packaging semantics" for the `requires-python` field.

It might be good to print a warning to the terminal if we resolve the user's configuration settings to a `requires-python` version less than typeshed's oldest supported version. Typeshed exposes this information in [this field here](https://github.com/python/typeshed/blob/24c78b9e0dff457275092025a14387dac206f5d6/pyproject.toml#L173-L174) in its pyproject.toml file (the intention is that it could be used by tools vendoring typeshed as well as by typeshed's internal tools). It doesn't look like we currently vendor typeshed's pyproject.toml file as part of our typeshed syncs, but we could easily start doing so. The reason why I think we should print a warning is that people might think that we'll accurately understand the Python 3.7 stdlib if they put `requires-python = ">= 3.7"` in their pyproject.toml file -- but we won't, because typeshed deleted the pre-Python-3.8 branches when it dropped support for Python 3.7. The reason why it should be a suppressible warning rather than an error is that the user might have Python <3.8 branches in their own code.

If we see `requires-python = "<3.6"`, I'd prefer us to error (with a helpful error message telling them to set the red-knot `python-version` setting) rather than fallback to the default of Python 3.9. Falling back to Python 3.9 here feels too much like guesswork.

---

_@carljm reviewed on 2025-02-11 17:59_

---

_Review comment by @carljm on `crates/red_knot_project/src/metadata.rs`:87 on 2025-02-11 17:59_

I think there are two separate issue raised in @sharkdp 's comment.

To me the more important issue is, what happens if someone specifies a `requires-python` lower bound that is lower than our minimum supported Python version? I think in this case we have to just check with our minimum supported Python version, but maybe we should also emit a warning (which would be silenced by explicitly setting a red-knot python version), so people don't mistakenly think they are getting red-knot coverage of Python 3.6?

The second issue is, what happens if someone specifies a `requires-python` with no lower bound? I agree that this is not a supported use of `requires-python` generally, but I do also think that means we should emit a warning on it.

---

_@MichaReiser reviewed on 2025-02-11 18:07_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata.rs`:87 on 2025-02-11 18:07_

I created an issue for the first case (by simply citing Alex) because it's unrelated to this PR (it doesn't matter if you use requires python or python-version, the problem applies to both settings). 

I'll go and add an error for the "no lower bound case" because erroring at this stage is easier than emitting a warning and I don't feel like this edge case is worth complicating the architecture to make it a warning.

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/metadata/pyproject.rs`:114 on 2025-02-12 10:37_

nit: I'd find it a bit easier to see what's different between the branches if we moved `source` out

```suggestion
        let python_version = PythonVersion::from((major, minor));
        let source = requires_python.source().clone();

        Ok(Some(if let Some(range) = requires_python.range() {
            RangedValue::with_range(python_version, source, range)
        } else {
            RangedValue::new(python_version, source)
        }))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/metadata.rs`:877 on 2025-02-12 10:40_

It would be good to also mention in the error message (and probably most of these error messages!) that they can fix the error by providing a value for the red-knot-specific `python-version` setting in the `tool.knot` table. They _might_ have good reason for using a `requires-python` setting with an upper bound but no lower bound because of how other tools use the field (even though the packaging guide tells you not to do this!)

---

_@AlexWaygood approved on 2025-02-12 10:41_

Nice!

---

_Merged by @MichaReiser on 2025-02-12 11:47_

---

_Closed by @MichaReiser on 2025-02-12 11:48_

---

_Branch deleted on 2025-02-12 11:48_

---

_@AlexWaygood reviewed on 2025-02-12 11:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/metadata/pyproject.rs`:122 on 2025-02-12 11:48_

They might not have a `tool.knot` table at all, right? This could be quite confusing if they aren't familiar with how to provide this red-knot-specific setting

```suggestion
    #[error("value `{0}` does not contain a lower bound. Add a lower bound to indicate the minimum compatible Python version (e.g., `>=3.13`) or specify a version in `tool.knot.environment.python-version`.")]
```

---
