```yaml
number: 15343
title: Add comma-separated support for --extra and --group flags
type: pull_request
state: open
author: yumeminami
labels: []
assignees: []
base: main
head: feat/add_comma_separated_support_for_extra_and_group_flags
created_at: 2025-08-18T08:01:35Z
updated_at: 2025-08-19T18:15:20Z
url: https://github.com/astral-sh/uv/pull/15343
synced_at: 2026-01-12T16:11:42Z
```

# Add comma-separated support for --extra and --group flags

---

_@yumeminami_



<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Support https://github.com/astral-sh/uv/issues/15332, Adds comma-separated support for `--extra` and `--group` flags in uv commands. Users can now specify multiple extras or groups with syntax like `--extra dev,test,docs` instead of requiring separate `--extra dev --extra test --extra docs` flags. Maintains full backward compatibility with existing individual flag usage.

## Test Plan
- Added comprehensive unit tests covering basic comma-separated usage, mixed syntax (combinin comma-separated and individual flags), and error handling for non-existent extras/groups- Tests verify functionality across sync, export, and run commands
- All existing tests continue to pass, ensuring backward compatibility



---

_Comment by @paduszyk on 2025-08-18 12:16_

@yumeminami Thanks for dealing with it so quickly. I would only suggest to link this PR by using one of the linking [keywords](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword), so that the community can see that somebody works on the ticket.

---

_Comment by @yumeminami on 2025-08-18 13:06_

@paduszyk 

I really like this tool, so I'd like to contribute to the community.

I think using a comma instead of writing '--extra' or '--group' again would improve the user experience.

---

_@zanieb reviewed on 2025-08-18 15:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4167 on 2025-08-18 15:59_

Can you move this down so it fits with the existing comment about multiples? e.g., 

```
/// Multiple values may be provided with comma separated values or by repeating the flag.
```

(this applies throughout)

---

_@zanieb reviewed on 2025-08-18 17:26_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4167 on 2025-08-18 17:26_

Thanks! It's not moved down though.

---

_@zanieb reviewed on 2025-08-18 17:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4167 on 2025-08-18 17:28_

i.e., it reads

```
    /// Install the specified dependency group from a `pyproject.toml`.
    /// Multiple values may be provided with comma separated values or by repeating the flag.
    ///
    /// If no path is provided, the `pyproject.toml` in the working directory is used.
    ///
    /// May be provided multiple times.
```

instead of

```
    /// Install the specified dependency group from a `pyproject.toml`.
    ///
    /// If no path is provided, the `pyproject.toml` in the working directory is used.
    ///
    /// Multiple values may be provided with comma separated values or by repeating the flag.
```

On re-reading though, we should probably say...

> Multiple values may be provided separated by commas or by repeating the flag.

---

_@zanieb reviewed on 2025-08-18 17:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:96 on 2025-08-18 17:28_

Can you explain why we need this?

---

_@zanieb reviewed on 2025-08-18 17:29_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 17:29_

Here we're just dropping the error that `GroupName::from_str` returns and constructing a new one? Why?

---

_@zanieb reviewed on 2025-08-18 17:30_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 17:30_

I guess this matches `extra_name_with_clap_error`?

---

_@yumeminami reviewed on 2025-08-18 18:07_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 18:07_

I think `Invalid extra name` and `Invalid group name` will be more intuitive for CLI users than "Not a valid package or extra name" - it's more direct and context-aware.

```
current output: ✗ Invalid extra name 'invalid#name': Extra names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters
original output: ✗ Not a valid package or extra name: "invalid#name". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

Should we just use the original errors like `pip_group_name_with_clap_error` does?

```rust
GroupName::from_str(trimmed).map_err(|err| anyhow!("{}", err))
```

This would be simpler and consistent across all functions.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 18:11_

Thanks for explaining!

I would expect to use the original error. I think the problem is that the original error is bad? e.g., at

https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-normalize/src/group_name.rs#L38-L45

we should not just be using `InvalidNameError` because it has a generic message

https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-normalize/src/lib.rs#L111-L120

We should probably introduce a dedicated error variant and map the error? Or we should make `InvalidNameError` more advanced?

I'm fine opening an issue to track this work — this pull request doesn't make it worse.

---

_@zanieb reviewed on 2025-08-18 18:11_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:96 on 2025-08-18 18:18_

We need it because `PipGroupName` supports path-prefixed syntax like `"pyproject.toml:dev"` that `GroupName` doesn't handle.

- `GroupName`: `"dev"`, `"test"`
- `PipGroupName`: `"dev"`, `"test"`, `"pyproject.toml:dev"`, `"path/to/pyproject.toml:test"`

---

_@yumeminami reviewed on 2025-08-18 18:18_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:96 on 2025-08-18 18:21_

Why does that mean we need an extra function? I think the answer is that this implements trimming? but that could just be implemented in `PipGroupName::from_str`? I guess the question is whether we want that or not? (it seems reasonable _not_ to trim there).

---

_@zanieb reviewed on 2025-08-18 18:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1877 on 2025-08-18 18:21_

This is inconsistent with the other text.

---

_@zanieb reviewed on 2025-08-18 18:21_

---

_@yumeminami reviewed on 2025-08-18 18:23_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 18:23_

how about this changes? 

```rust
fn name_with_clap_error<T>(arg: &str, name_type: &str) -> Result<T>
where
    T: FromStr,
    T::Err: std::fmt::Display,
{
    let trimmed = arg.trim();
    if trimmed.is_empty() {
        return Err(anyhow!("{} cannot be empty", name_type));
    }
    T::from_str(trimmed).map_err(|err| anyhow!("{}", err))
}

fn extra_name_with_clap_error(arg: &str) -> Result<ExtraName> {
    name_with_clap_error::<ExtraName>(arg, "Extra name")
}

fn group_name_with_clap_error(arg: &str) -> Result<GroupName> {
    name_with_clap_error::<GroupName>(arg, "Group name")
}

fn pip_group_name_with_clap_error(arg: &str) -> Result<PipGroupName> {
    name_with_clap_error::<PipGroupName>(arg, "Group name")
}
```

---

_@yumeminami reviewed on 2025-08-18 18:24_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:1877 on 2025-08-18 18:24_

I will update all about the comment issue tomorrow, thanks your review!

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:99 on 2025-08-18 18:27_

> Thanks for explaining!
> 
> I would expect to use the original error. I think the problem is that the original error is bad? e.g., at
> 
> https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-normalize/src/group_name.rs#L38-L45
> 
> we should not just be using `InvalidNameError` because it has a generic message
> 
> https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-normalize/src/lib.rs#L111-L120
> 
> We should probably introduce a dedicated error variant and map the error? Or we should make `InvalidNameError` more advanced?
> 
> I'm fine opening an issue to track this work — this pull request doesn't make it worse.

yeah! if we can map the error, for users who mix `--group` and `--extra`, this will make it quicker and more straightforward to see where the error occurred. I’m happy to contribute to improving the user experience.

---

_@yumeminami reviewed on 2025-08-18 18:27_

---

_@yumeminami reviewed on 2025-08-18 18:35_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:96 on 2025-08-18 18:35_

I think `trimming` is more of a CLI-layer concern. The type-level `PipGroupName::from_str` should focus on pure validation logic

What do you think?

---

_@SeanOverton approved on 2025-08-18 21:33_

---

_Comment by @yumeminami on 2025-08-19 02:02_

1. I've refactored the implementation of `extra/group/pip_group_name_with_clap_error` so that their error output is consistent.
2. I've modified all related comments.

---

_Comment by @yumeminami on 2025-08-19 17:58_

hey! some one continue to review the pr?

---

_Comment by @zanieb on 2025-08-19 18:15_

There's no need to ping us again less than 24 hours after making a change, we're busy and have a lot to review.

---
