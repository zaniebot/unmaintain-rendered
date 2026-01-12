```yaml
number: 14262
title: Update Rust crate thiserror to v2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/thiserror-2.x
created_at: 2024-11-11T03:27:09Z
updated_at: 2024-11-11T10:00:54Z
url: https://github.com/astral-sh/ruff/pull/14262
synced_at: 2026-01-12T15:55:47Z
```

# Update Rust crate thiserror to v2

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [thiserror](https://redirect.github.com/dtolnay/thiserror) | workspace.dependencies | major | `1.0.58` -> `2.0.0` |

---

### Release Notes

<details>
<summary>dtolnay/thiserror (thiserror)</summary>

### [`v2.0.3`](https://redirect.github.com/dtolnay/thiserror/releases/tag/2.0.3)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/2.0.2...2.0.3)

-   Support the same Path field being repeated in both Debug and Display representation in error message ([#&#8203;383](https://redirect.github.com/dtolnay/thiserror/issues/383))
-   Improve error message when a format trait used in error message is not implemented by some field ([#&#8203;384](https://redirect.github.com/dtolnay/thiserror/issues/384))

### [`v2.0.2`](https://redirect.github.com/dtolnay/thiserror/releases/tag/2.0.2)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/2.0.1...2.0.2)

-   Fix hang on invalid input inside #\[error(...)] attribute ([#&#8203;382](https://redirect.github.com/dtolnay/thiserror/issues/382))

### [`v2.0.1`](https://redirect.github.com/dtolnay/thiserror/releases/tag/2.0.1)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/2.0.0...2.0.1)

-   Support errors that contain a dynamically sized final field ([#&#8203;375](https://redirect.github.com/dtolnay/thiserror/issues/375))
-   Improve inference of trait bounds for fields that are interpolated multiple times in an error message ([#&#8203;377](https://redirect.github.com/dtolnay/thiserror/issues/377))

### [`v2.0.0`](https://redirect.github.com/dtolnay/thiserror/releases/tag/2.0.0)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/1.0.69...2.0.0)

#### Breaking changes

-   Referencing keyword-named fields by a raw identifier like `{r#type}` inside a format string is no longer accepted; simply use the unraw name like `{type}` ([#&#8203;347](https://redirect.github.com/dtolnay/thiserror/issues/347))

    This aligns thiserror with the standard library's formatting macros, which gained support for implicit argument capture later than the release of this feature in thiserror 1.x.

    ```rust
    #[derive(Error, Debug)]
    #[error("... {type} ...")]  // Before: {r#type}
    pub struct Error {
        pub r#type: Type,
    }
    ```

-   Trait bounds are no longer inferred on fields whose value is shadowed by an explicit named argument in a format message ([#&#8203;345](https://redirect.github.com/dtolnay/thiserror/issues/345))

    ```rust
    // Before: impl<T: Octal> Display for Error<T>
    // After: impl<T> Display for Error<T>
    #[derive(Error, Debug)]
    #[error("{thing:o}", thing = "...")]
    pub struct Error<T> {
        thing: T,
    }
    ```

-   Tuple structs and tuple variants can no longer use numerical `{0}` `{1}` access at the same time as supplying extra positional arguments for a format message, as this makes it ambiguous whether the number refers to a tuple field vs a different positional arg ([#&#8203;354](https://redirect.github.com/dtolnay/thiserror/issues/354))

    ```rust
    #[derive(Error, Debug)]
    #[error("ambiguous: {0} {}", $N)]
    //                  ^^^ Not allowed, use #[error("... {0} {n}", n = $N)]
    pub struct TupleError(i32);
    ```

-   Code containing invocations of thiserror's `derive(Error)` must now have a direct dependency on the `thiserror` crate regardless of the error data structure's contents ([#&#8203;368](https://redirect.github.com/dtolnay/thiserror/issues/368), [#&#8203;369](https://redirect.github.com/dtolnay/thiserror/issues/369), [#&#8203;370](https://redirect.github.com/dtolnay/thiserror/issues/370), [#&#8203;372](https://redirect.github.com/dtolnay/thiserror/issues/372))

#### Features

-   Support disabling thiserror's standard library dependency by disabling the default "std" Cargo feature: `thiserror = { version = "2", default-features = false }` ([#&#8203;373](https://redirect.github.com/dtolnay/thiserror/issues/373))

-   Support using `r#source` as field name to opt out of a field named "source" being treated as an error's `Error::source()` ([#&#8203;350](https://redirect.github.com/dtolnay/thiserror/issues/350))

    ```rust
    #[derive(Error, Debug)]
    #[error("{source} ==> {destination}")]
    pub struct Error {
        r#source: char,
        destination: char,
    }

    let error = Error { source: 'S', destination: 'D' };
    ```

-   Infinite recursion in a generated Display impl now produces an `unconditional_recursion` warning ([#&#8203;359](https://redirect.github.com/dtolnay/thiserror/issues/359))

    ```rust
    #[derive(Error, Debug)]
    #[error("??? {self}")]
    pub struct Error;
    ```

-   A new attribute `#[error(fmt = path::to::myfmt)]` can be used to write formatting logic for an enum variant out-of-line ([#&#8203;367](https://redirect.github.com/dtolnay/thiserror/issues/367))

    ```rust
    #[derive(Error, Debug)]
    pub enum Error {
        #[error(fmt = demo_fmt)]
        Demo { code: u16, message: Option<String> },
    }

    fn demo_fmt(code: &u16, message: &Option<String>, formatter: &mut fmt::Formatter) -> fmt::Result {
        write!(formatter, "{code}")?;
        if let Some(msg) = message {
            write!(formatter, " - {msg}")?;
        }
        Ok(())
    }
    ```

-   Enums with an enum-level format message are now able to have individual variants that are `transparent` to supersede the enum-level message ([#&#8203;366](https://redirect.github.com/dtolnay/thiserror/issues/366))

    ```rust
    #[derive(Error, Debug)]
    #[error("my error {0}")]
    pub enum Error {
        Json(#[from] serde_json::Error),
        Yaml(#[from] serde_yaml::Error),
        #[error(transparent)]
        Other(#[from] anyhow::Error),
    }
    ```

### [`v1.0.69`](https://redirect.github.com/dtolnay/thiserror/releases/tag/1.0.69)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/1.0.68...1.0.69)

-   Backport 2.0.2 fixes

### [`v1.0.68`](https://redirect.github.com/dtolnay/thiserror/releases/tag/1.0.68)

[Compare Source](https://redirect.github.com/dtolnay/thiserror/compare/1.0.67...1.0.68)

-   Handle incomplete expressions more robustly in format arguments, such as while code is being typed ([#&#8203;341](https://redirect.github.com/dtolnay/thiserror/issues/341), [#&#8203;344](https://redirect.github.com/dtolnay/thiserror/issues/344))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS43LjEiLCJ1cGRhdGVkSW5WZXIiOiIzOS43LjEiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Comment by @renovate[bot] on 2024-11-11 03:27_

### ⚠️ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package thiserror@1.0.67 --precise 2.0.3
    Updating crates.io index
error: failed to select a version for the requirement `thiserror = "^1.0.38"`
candidate versions found which didn't match: 2.0.3
location searched: crates.io index
required by package `clearscreen v3.0.0`
    ... which satisfies dependency `clearscreen = "^3.0.0"` (locked to 3.0.0) of package `ruff v0.7.3 (/tmp/renovate/repos/github/astral-sh/ruff/crates/ruff)`

```



---

_Label `internal` added by @renovate[bot] on 2024-11-11 03:27_

---

_Comment by @github-actions[bot] on 2024-11-11 03:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-11-11 09:41_

* I did a quick search for `{r#` and didn't find any usages belonging to thiserror. 
* `cargo test` passes


---

_Merged by @MichaReiser on 2024-11-11 09:46_

---

_Closed by @MichaReiser on 2024-11-11 09:46_

---

_Branch deleted on 2024-11-11 09:46_

---
