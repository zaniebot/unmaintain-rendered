```yaml
number: 5350
title: "Allow comments in `.python-version[s]`"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: empty-line
created_at: 2024-07-23T16:47:47Z
updated_at: 2024-07-23T23:47:24Z
url: https://github.com/astral-sh/uv/pull/5350
synced_at: 2026-01-12T16:06:46Z
```

# Allow comments in `.python-version[s]`

---

_@j178_

## Summary

Do the same thing as https://github.com/astral-sh/rye/pull/1038.



---

_Comment by @charliermarsh on 2024-07-23 16:48_

Check out `python_pin.rs`?

---

_Marked ready for review by @j178 on 2024-07-23 17:31_

---

_Comment by @j178 on 2024-07-23 17:32_

Thanks, I added a test.

---

_@zanieb reviewed on 2024-07-23 17:46_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:642 on 2024-07-23 17:46_

It'd be nice to use a `r#` string and split this over multiple lines for readability.

---

_@zanieb reviewed on 2024-07-23 17:47_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:639 on 2024-07-23 17:47_

Any reason we need a child directory? The `context.temp_dir` should be fine, right? Then you don't need to set the working directory.

---

_Assigned to @zanieb by @zanieb on 2024-07-23 17:51_

---

_@j178 reviewed on 2024-07-23 17:51_

---

_Review comment by @j178 on `crates/uv/tests/python_pin.rs`:639 on 2024-07-23 17:51_

I'm testing both `.python-version` and `.python-versions` in the same test, so I want to separate them by using different subdirectory, otherwise only one of them will be utilized.

---

_@zanieb reviewed on 2024-07-23 17:53_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:639 on 2024-07-23 17:53_

You could just test the one that has higher precedence first. edit: second* :)

I'd recommend separate tests though over creating subdirectories for mixed state in a single test.

---

_@eth3lbert reviewed on 2024-07-23 18:37_

---

_Review comment by @eth3lbert on `crates/uv/tests/python_pin.rs`:638 on 2024-07-23 18:37_

```suggestion
    let content = indoc::indoc! {r"
        3.12
        # 3.11
        3.10
    "};
```

---

_@zanieb reviewed on 2024-07-23 18:54_

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:638 on 2024-07-23 18:54_

We use r#"..."# all over the place, idk if I have a preference.

---

_@eth3lbert reviewed on 2024-07-23 19:24_

---

_Review comment by @eth3lbert on `crates/uv/tests/python_pin.rs`:638 on 2024-07-23 19:24_

Do you prefer the following format?

```rust
    let content = r"
3.12

# 3.11
3.10
";

```

I made that suggestion because of a clippy error, `error: unnecessary hashes around raw string literal` encountered in the CI[^1], as well as inconsistencies in indentation.

I don't have a strong preference but lean towards indentation with indoc, though.

[^1]: https://github.com/astral-sh/uv/actions/runs/10064000423/job/27820018371#step:5:54

---

_Review comment by @zanieb on `crates/uv/tests/python_pin.rs`:638 on 2024-07-23 19:37_

Sorry, just using the hashes as a stand-in for our general format (as in the example you shared). We usually skip using indoc to remove indentation since the tests are generally only singly-nested. I don't really have a preference though.

---

_@zanieb reviewed on 2024-07-23 19:37_

---

_Merged by @zanieb on 2024-07-23 19:56_

---

_Closed by @zanieb on 2024-07-23 19:56_

---

_Label `enhancement` added by @zanieb on 2024-07-23 19:57_

---

_Label `preview` added by @zanieb on 2024-07-23 19:57_

---

_Branch deleted on 2024-07-23 23:47_

---
