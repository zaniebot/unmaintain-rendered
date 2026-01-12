```yaml
number: 11687
title: Handle non-printable characters in diff view
type: pull_request
state: merged
author: pilleye
labels:
  - cli
assignees: []
merged: true
base: main
head: pilleye/nonprinting
created_at: 2024-06-02T02:58:29Z
updated_at: 2024-06-09T19:51:53Z
url: https://github.com/astral-sh/ruff/pull/11687
synced_at: 2026-01-12T15:55:38Z
```

# Handle non-printable characters in diff view

---

_@pilleye_

## Summary

Fixes #10841 by handling non-printable characters in the same way as GNU coreutils cat's `--show-nonprinting` flag. One downside of this approach is that unicode characters are no longer printed as their actual character, which could be problematic for non-Latin languages.

## Test Plan

Tested with the same example in #10841, but would appreciate a pointer to where I can programmatically add some tests for various non-printable ASCII characters. 

![Screenshot 2024-06-01 at 7 49 00 PM](https://github.com/astral-sh/ruff/assets/29032680/9864b3a8-2767-4bcc-a8fd-5563edd5e845)

## Example of Unicode Changes

![Screenshot 2024-06-01 at 7 56 31 PM](https://github.com/astral-sh/ruff/assets/29032680/91360017-3ae3-4458-be82-c04810031389)

## Additional Considerations

As @carljm pointed out, this would make the code diverge from git diff.

Would appreciate any feedback on this quick, slightly hacky diff!


---

_Comment by @github-actions[bot] on 2024-06-02 03:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:225 on 2024-06-02 13:18_

It would be nice if we could return a `Cow` here to avoid allocating a string if it doesn't contain any non printable characters. 

We may be able to do something similar to 
https://github.com/mitsuhiko/insta/blob/569bbade9d9ff4bd43fe4138bdcbde00a6bf34c4/insta/src/output.rs#L325-L339



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:228 on 2024-06-02 13:21_

Do I understand this correctly, that this will escape all non-ascii characters (in addition to some ascii characters)? I think that's undesirable. I would be very confused to see ä escaped in a Diff and wouldn't understand where this is coming from. 

I recommend focusing on the non printable characters and would consider using unicode characters for them (similar to what insta does see link above) or biomejs (https://github.com/MichaReiser/biome/blob/96bbf507a006274518e8f5ae5af57c56b7771fb0/crates/biome_diagnostics/src/display/frame.rs#L394-L406)

---

_@MichaReiser reviewed on 2024-06-02 13:23_

Thank you for looking into this. 

I'm a bit surprised that we don't see a single test failure. I think we may also need to make this change in 

https://github.com/astral-sh/ruff/blob/8a25531a7144fd4a6b62c54efde1ef28e2dc18c4/crates/ruff_linter/src/message/text.rs#L173

and 

https://github.com/astral-sh/ruff/blob/8a25531a7144fd4a6b62c54efde1ef28e2dc18c4/crates/ruff_linter/src/message/diff.rs#L22

---

_Comment by @pilleye on 2024-06-05 19:20_

Fixed the unicode issues and Cow issues: ![Screenshot 2024-06-05 at 3 19 46 PM](https://github.com/astral-sh/ruff/assets/29032680/17498313-5795-424a-a3a6-25fd2e52e92a)

Working on adding it to the other spots in the repo. 

---

_@pilleye reviewed on 2024-06-05 19:21_

---

_Review comment by @pilleye on `crates/ruff_linter/src/source_kind.rs`:228 on 2024-06-05 19:21_

Yes, that was correct. Fixed in the recent version.

---

_@pilleye reviewed on 2024-06-05 19:21_

---

_Review comment by @pilleye on `crates/ruff_linter/src/source_kind.rs`:225 on 2024-06-05 19:21_

Done!

---

_Comment by @pilleye on 2024-06-05 19:33_

Updated the snapshot tests, they look to be much more clear since the `^` is actually pointing to the correct location.

---

_Review requested from @MichaReiser by @pilleye on 2024-06-05 19:50_

---

_Comment by @zanieb on 2024-06-05 20:24_

Thanks for contributing!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:250 on 2024-06-08 06:12_

Nit, I would introduce a variable with the cleaned-up code to reduce the diff size and reduce the risk that one path misses the call.

```rust
let change = change.value().show_nonprinting();

match change.tag {
	ChangeTag::Equal => write!(f, " {}", change)?,
	...
}
```


---

_@MichaReiser approved on 2024-06-08 06:15_

Nice, this is a huge improvement over what we used to have before. 



---

_Label `cli` added by @MichaReiser on 2024-06-08 06:19_

---

_Merged by @MichaReiser on 2024-06-08 06:22_

---

_Closed by @MichaReiser on 2024-06-08 06:22_

---

_Branch deleted on 2024-06-09 19:51_

---
