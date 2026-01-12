```yaml
number: 7875
title: "Remove min and max range on `line-length` JSON schema"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/line-length
created_at: 2023-10-09T18:45:17Z
updated_at: 2023-10-09T23:51:29Z
url: https://github.com/astral-sh/ruff/pull/7875
synced_at: 2026-01-12T15:55:25Z
```

# Remove min and max range on `line-length` JSON schema

---

_@charliermarsh_

## Summary

This was introduced in https://github.com/astral-sh/ruff/pull/7412, but SchemaStore doesn't accept it. I manually edited the JSON schema last time, then tried to fix this, then gave up -- so removing for now.

(See, e.g., https://github.com/SchemaStore/schemastore/pull/3278, which failed prior to removing the min and max.)


---

_Review requested from @zanieb by @charliermarsh on 2023-10-09 18:45_

---

_Label `internal` added by @charliermarsh on 2023-10-09 18:45_

---

_Comment by @zanieb on 2023-10-09 18:57_

Can we not just add exceptions in the schema store schema validator similar to what I did at https://github.com/SchemaStore/schemastore/pull/3221/files#diff-5b41cf0105f054a82fd2932cf8e4af10ec697a315a9fe61d3f1956c0a47cdafc? We're already not producing a schema that passes their strict validation.

I don't think we should remove correct validation on our end if avoidable.

---

_Comment by @charliermarsh on 2023-10-09 19:06_

I'll give it a quick try on SchemaStore! If it doesn't work, we should merge this and revisit later. From a procedural perspective, this is a clean revert of a change that broke the release. (Users already aren't benefiting from the change because we can't release the updated schema anyway.)


---

_Comment by @github-actions[bot] on 2023-10-09 19:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-09 19:21_

This is the change we need in the schema file: https://github.com/SchemaStore/schemastore/pull/3278/commits/75d4ec66a64dd36b176e24d6193ecb25e8f85414 (I made it manually). I believe I've tried to get schemars to output this in the past and failed. (I couldn't get our current schema to pass by adding exclusions on the SchemaStore side.) I believe we should merge this in the absence of an owner that wants to prioritize fixing the root cause.


---

_@zanieb approved on 2023-10-09 19:23_

Sounds good to me. My main concern is that the range is relatively restricted and I worry users will encounter invalid values.

It'd be great to open an issue somewhere to track restoring this if we merge without resolving or to explore what happens if users set the length above the support range and how we show errors for this.

My naive test with

```
❯ ruff check example.py --show-settings | grep line_length -A 2
        line_length: LineLength(
            2000,
        ),
```

didn't error which surprises me?

edit: okay I did succeed with a larger value

```
  Cause: Failed to parse `/Users/mz/eng/src/astral-sh/ruff/ruff.toml`: TOML parse error at line 2, column 15
  |
2 | line-length = 1000000
  |               ^^^^^^^
invalid value: integer `1000000`, expected a nonzero u16
```

---

_Comment by @charliermarsh on 2023-10-09 19:31_

Maybe I can try post-processing the JSON. Sounds like a pain in Rust though 😬 

---

_Comment by @charliermarsh on 2023-10-09 19:36_

Tracking in https://github.com/astral-sh/ruff/issues/7877.

---

_Merged by @charliermarsh on 2023-10-09 19:36_

---

_Closed by @charliermarsh on 2023-10-09 19:36_

---

_Branch deleted on 2023-10-09 19:36_

---

_Comment by @henryiii on 2023-10-09 21:02_

For posterity, this is not SchemaStore being overly strict, it's invalid JSONSchema. This following is invalid:

```json
    "line-length": {
      "description": "The line length to use when enforcing long-lines violations (like `E501`). Must be greater than `0` and less than or equal to `320`.",
      "anyOf": [
        {
          "$ref": "#/definitions/LineLength"
        },
        {
          "type": "null"
        }
      ],
      "maximum": 320.0,
      "minimum": 1.0
    }
```

An anyOf can't have a minimum/maximum length. Only a `"type": "number"` has those properties. So it has to go inside the definition at `"#/definitions/LineLength"`.

If schemars is making the definitions directly then I'd guess it's a schemars bug?

---

_Comment by @charliermarsh on 2023-10-09 21:03_

Thanks @henryiii, much appreciated!

---

_@henryiii reviewed on 2023-10-09 21:06_

---

_Review comment by @henryiii on `crates/ruff_workspace/src/options.rs`:365 on 2023-10-09 21:06_

FWICT, it looks like this is applying to the `Option<LineLength>`, rather than the `LineLength` inside the `Option`?

---

_@henryiii reviewed on 2023-10-09 23:46_

---

_Review comment by @henryiii on `crates/ruff_workspace/src/options.rs`:365 on 2023-10-09 23:46_

FWIW, I can replicate it with a MWE:

```rust
use schemars::{schema_for, JsonSchema};

#[derive(JsonSchema)]
pub struct MyStruct {
    pub my_int: i32,
}

#[derive(JsonSchema)]
pub struct MyConfig {
    #[schemars(range(min = 1, max = 320))]
    my_struct: Option<MyStruct>,
}

fn main() {
    let schema = schema_for!(MyConfig);
    println!("{}", serde_json::to_string_pretty(&schema).unwrap());
}
```

And one fix is to move the location of the attribute:

```rust
use schemars::{schema_for, JsonSchema};

#[derive(JsonSchema)]
pub struct MyStruct {
    #[schemars(range(min = 1, max = 320))]
    pub my_int: i32,
}

#[derive(JsonSchema)]
pub struct MyConfig {
    my_struct: Option<MyStruct>,
}

fn main() {
    let schema = schema_for!(MyConfig);
    println!("{}", serde_json::to_string_pretty(&schema).unwrap());
}

```

But not sure how that works from the outer location, ~~and not sure what `cfg_attrs` is~~ (nevermind, I do). I see an `inner()`, but it seems to only apply to things like Vector, not to Enums.

---

_@henryiii reviewed on 2023-10-09 23:51_

---

_Review comment by @henryiii on `crates/ruff_workspace/src/options.rs`:365 on 2023-10-09 23:51_

Ahh, I know what that is, and how to fix it (I think).

---
