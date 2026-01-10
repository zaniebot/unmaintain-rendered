```yaml
number: 3979
title: "feat: support `NO_COLOR` and `FORCE_COLOR` env vars"
type: pull_request
state: merged
author: samypr100
labels:
  - configuration
  - cli
assignees: []
merged: true
base: main
head: adjust-color-settings
created_at: 2024-06-03T02:48:59Z
updated_at: 2025-05-05T12:49:03Z
url: https://github.com/astral-sh/uv/pull/3979
synced_at: 2026-01-10T11:10:33Z
```

# feat: support `NO_COLOR` and `FORCE_COLOR` env vars

---

_Pull request opened by @samypr100 on 2024-06-03 02:48_

## Summary

Closes #3955

Adds explicit support to `NO_COLOR` and `FORCE_COLOR` via GlobalSettings.

The order, per specs is now `NO_COLOR` > `FORCE_COLOR` > `color`.

This PR is a backup plan pending rust-cli/anstyle#192.

## Test Plan

Tested all cases locally for now; I didn't see existing tests for GlobalSettings parsing.

---

_Comment by @codspeed-hq[bot] on 2024-06-03 02:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100:adjust-color-settings)

### Merging #3979 will **not alter performance**

<sub>Comparing <code>samypr100:adjust-color-settings</code> (affba38) with <code>main</code> (40e0ddd)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @charliermarsh on 2024-06-03 13:22_

Should we add `CLICOLOR_FORCE` as well so that it's respected over the color CLI argument?

---

_Comment by @charliermarsh on 2024-06-03 13:22_

I'm fine to merge this regardless of anstream changes.

---

_@epage reviewed on 2024-06-03 18:42_

---

_Review comment by @epage on `crates/uv/src/cli.rs`:74 on 2024-06-03 18:42_

Implementation wise, I find this dubious to have hidden arguments for pulling in environment variables.  Is this a standard practiced in Ruff?

---

_@charliermarsh reviewed on 2024-06-03 19:12_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:74 on 2024-06-03 19:12_

No, in Ruff IIRC we use `colored`. It detects some of these for us, and then we check for `FORCE_COLOR` with:

```rust
// support FORCE_COLOR env var
if let Some(force_color) = std::env::var_os("FORCE_COLOR") {
    if force_color.len() > 0 {
        colored::control::set_override(true);
    }
}
```

---

_@charliermarsh reviewed on 2024-06-04 00:08_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:74 on 2024-06-04 00:08_

@samypr100 - Let's change to read these env vars directly, I think?

---

_@epage reviewed on 2024-06-04 00:17_

---

_Review comment by @epage on `crates/uv/src/cli.rs`:74 on 2024-06-04 00:17_

I think i saw a reference to uv using anstream. The equivalent of `colored` for setting the global is https://docs.rs/anstream/latest/anstream/enum.ColorChoice.html#method.write_global

---

_@charliermarsh reviewed on 2024-06-04 00:28_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:74 on 2024-06-04 00:28_

That's right, we use anstream in uv, but colored in Ruff!

We actually looked at migrating Ruff to anstream: https://github.com/astral-sh/ruff/pull/10784

---

_@samypr100 reviewed on 2024-06-04 02:08_

---

_Review comment by @samypr100 on `crates/uv/src/cli.rs`:74 on 2024-06-04 02:08_

> @samypr100 - Let's change to read these env vars directly, I think?

Done in 1c451f6d44c9e99b8af699e736e36dc09c17c27f

---

_Review comment by @epage on `crates/uv/src/settings.rs`:54 on 2024-06-04 02:08_

NO_COLOR says it should not be an empty string.  Same with FORCE_COLOR.  CLICOLOR *says* it should just be set but the example Python code checks that its non-empty.

---

_@epage reviewed on 2024-06-04 02:08_

---

_@samypr100 reviewed on 2024-06-04 02:15_

---

_Review comment by @samypr100 on `crates/uv/src/settings.rs`:54 on 2024-06-04 02:15_

Good point, I was looking at https://github.com/python/cpython/blob/3.13/Lib/_colorize.py#L53 too much

---

_@samypr100 reviewed on 2024-06-04 02:33_

---

_Review comment by @samypr100 on `crates/uv/src/settings.rs`:54 on 2024-06-04 02:33_

affba38815b1b4535dab9a6211f211a24c316f61, is this more along the lines of what you were thinking? @epage 

Thanks for the review btw.

---

_@zanieb reviewed on 2024-06-04 14:46_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:74 on 2024-06-04 14:46_

Thanks Ed!

---

_@charliermarsh approved on 2024-06-04 19:51_

@samypr100 - This looks good to me. Ok to merge?

---

_Marked ready for review by @samypr100 on 2024-06-04 20:35_

---

_Merged by @charliermarsh on 2024-06-04 21:00_

---

_Closed by @charliermarsh on 2024-06-04 21:00_

---

_Label `configuration` added by @charliermarsh on 2024-06-04 21:00_

---

_Label `cli` added by @charliermarsh on 2024-06-04 21:00_

---

_Branch deleted on 2024-06-04 21:35_

---

_Comment by @bluss on 2024-06-05 15:53_

The way I read the examples on those two pages, https://force-color.org/ http://no-color.org/  they both suggest that command line flags that turn on color should have precedence over NO_COLOR.  i.e "do getopt(3) and/or config-file parsing to possibly turn color back on" and so on.  Not sure if I'm confusing myself, or if there are contradicting specs.

---

_Comment by @samypr100 on 2024-06-05 16:18_

@bluss They're examples, not the standard. The standard itself (the sentence at the top of the pages) doesn't go into detail on how they are meant to interact with each other.

---

_Comment by @webknjaz on 2025-05-05 12:49_

@samypr100 I was trying to untangle that interaction problem because Rich happened to break some expectations and ended up getting another var called `TTY_COMPATIBLE` implemented over there: https://github.com/donatj/force-color.org/issues/8#issuecomment-2850408360. If it takes off, this would be something to take into account too.

Does the toggle in this PR influence non-color text decorations, by the way?

---
