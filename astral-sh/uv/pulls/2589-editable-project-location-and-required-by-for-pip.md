```yaml
number: 2589
title: "`Editable project location` and `Required-by` for `pip show`"
type: pull_request
state: merged
author: eth3lbert
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: pip-show
created_at: 2024-03-21T14:00:50Z
updated_at: 2024-03-26T11:18:47Z
url: https://github.com/astral-sh/uv/pull/2589
synced_at: 2026-01-10T14:49:08Z
```

# `Editable project location` and `Required-by` for `pip show`

---

_Pull request opened by @eth3lbert on 2024-03-21 14:00_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- Displays missing packages as single-line warnings.
- Adds support for `Editable project location` and `Required-by` fields in `pip show`.

Part of #2526.


---

_Renamed from "`editable project location` and `required-by` for `pip show`" to "`Editable project location` and `Required-by` for `pip show`" by @eth3lbert on 2024-03-21 14:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_show.rs`:161 on 2024-03-21 14:39_

Can we avoid this `unwrap`?

---

_@zanieb reviewed on 2024-03-21 14:39_

---

_@zanieb reviewed on 2024-03-21 14:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_show.rs`:161 on 2024-03-21 14:40_

Do you need to call `to_string`? Isn't display going to be called in `writeln!`?

---

_@zanieb reviewed on 2024-03-21 14:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_show.rs`:99 on 2024-03-21 14:42_

@charliermarsh or @konstin could you review this section?

---

_@zanieb reviewed on 2024-03-21 14:43_

---

_Review comment by @zanieb on `crates/uv/tests/pip_show.rs`:518 on 2024-03-21 14:43_

I wonder if we should omit these when they're empty

---

_Comment by @zanieb on 2024-03-21 14:43_

Thanks for contributing!

---

_@eth3lbert reviewed on 2024-03-21 14:48_

---

_Review comment by @eth3lbert on `crates/uv/src/commands/pip_show.rs`:161 on 2024-03-21 14:48_

Does this make more sense to you?
```rust
       if let Some(path) = distribution
            .as_editable()
            .and_then(|url| url.to_file_path().ok())
        {
            writeln!(
                printer.stdout(),
                "Editable project location: {}",
                path.simplified_display()
            )?;
        }

```

---

_@eth3lbert reviewed on 2024-03-21 14:48_

---

_Review comment by @eth3lbert on `crates/uv/tests/pip_show.rs`:518 on 2024-03-21 14:48_

I followed the approach used by pip. I have no preference, either option is fine.

---

_Review comment by @danielhollas on `crates/uv/tests/pip_show.rs`:518 on 2024-03-21 15:01_

I'd vote to keep pip bwhaviour, otherwise users can wonder if it is even implemented.

---

_@danielhollas reviewed on 2024-03-21 15:01_

---

_Comment by @danielhollas on 2024-03-21 15:04_

Awesome.

One small thing from the linked issue: if it isn't too much extra work it would be nice to display Summary and Home page as well. Those are nice for packages with which one is not familiar.

---

_@zanieb reviewed on 2024-03-21 15:13_

---

_Review comment by @zanieb on `crates/uv/tests/pip_show.rs`:518 on 2024-03-21 15:13_

I don't think being worried that people will wonder if it it's implemented makes sense as a long-term choice. It seems likely if you care about this feature that you'll have at least one package with `Required-by` set. I don't have strong feelings but I'd lean towards omitting empty fields. Curious for others' thoughts though cc @konstin 

---

_Comment by @zanieb on 2024-03-21 15:14_

@danielhollas I wouldn't expand the scope of this pull request, it's better to implement things separately.

---

_@zanieb reviewed on 2024-03-21 15:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_show.rs`:161 on 2024-03-21 15:14_

I think so! 

---

_Comment by @danielhollas on 2024-03-21 15:16_

Totally agree! But then please don't close the issue since it's not fully resolved : 😅

---

_Comment by @eth3lbert on 2024-03-21 15:28_

The implementation itself shouldn't be difficult. However, those fields also come from metadata, which are not stored in `Metadata23` (perhaps to keep the footprint size small?). In this case, we need to decide how to proceed: should we simply include more fields here, or introduce a separate struct that provides the full metadata?


---

_@zanieb approved on 2024-03-25 19:44_

Thanks!

---

_Merged by @zanieb on 2024-03-25 19:45_

---

_Closed by @zanieb on 2024-03-25 19:45_

---

_Label `cli` added by @zanieb on 2024-03-25 19:45_

---

_Label `enhancement` added by @zanieb on 2024-03-25 19:45_

---

_Branch deleted on 2024-03-25 19:46_

---

_@konstin reviewed on 2024-03-26 11:17_

---

_Review comment by @konstin on `crates/uv/src/commands/pip_show.rs`:99 on 2024-03-26 11:17_

Sorry i missed the ping, code looks good

---

_@konstin reviewed on 2024-03-26 11:18_

---

_Review comment by @konstin on `crates/uv/tests/pip_show.rs`:518 on 2024-03-26 11:18_

I don't have an opinion on that, we could add `(none)` as placeholder if it helps

---
