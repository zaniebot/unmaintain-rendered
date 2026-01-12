```yaml
number: 12820
title: Split UV_INDEX on all whitespace
type: pull_request
state: merged
author: winstonallo
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: allow-newline-separator-in-UV_INDEX
created_at: 2025-04-10T18:32:35Z
updated_at: 2025-06-27T09:12:52Z
url: https://github.com/astral-sh/uv/pull/12820
synced_at: 2026-01-12T16:10:24Z
```

# Split UV_INDEX on all whitespace

---

_@winstonallo_

## Summary
Closes #12806

Split `UV_INDEX` by any whitespace rather than only ASCII 32, which does not align with the behavior of `PIP_EXTRA_INDEX_URL` and can possibly lead to difficulties when migrating from pip to uv.

Clap unfortunately does not support passing multiple delimiters, writing a custom parsing function involved parsing index into a Vec<Vec<Index>> and flattening it afterwards in order to avoid breaking the --index command line option. 

There might be a prettier solution I overlooked, let me know if there is anything I should change! 


---

_Review requested from @Gankra by @Gankra on 2025-04-10 19:02_

---

_@Gankra approved on 2025-04-10 19:30_

Could you add a test that's a copy of repeated_index_cli that tests the new whitespace support?

https://github.com/astral-sh/uv/blob/7a18e4429d6750365c2f2ad98907a90d97e67fb7/crates/uv/tests/it/edit.rs#L9948-L9956

(see `repeated_index_cli_environment_variable` for an example of how to set env vars in invocations -- note that it uses UV_DEFAULT_INDEX and you want UV_INDEX)

---

_@Gankra reviewed on 2025-04-10 19:32_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:929 on 2025-04-10 19:32_

Oops this looks like a mistake

---

_@Gankra reviewed on 2025-04-10 19:35_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:910 on 2025-04-10 19:35_

Might be nice to add some more context here to note that this function exists because clap value_delimiter only lets you split on a single character, and we need to split on all whitespace (notably spaces OR newlines). 

And also that the --index arguments have to have a `Vec<Vec<Maybe<Index>>>` because when clap parses an environment variable with `value_parser` it expects to only parse one value.

---

_Label `bug` added by @Gankra on 2025-04-10 19:54_

---

_Label `compatibility` added by @Gankra on 2025-04-10 19:54_

---

_@winstonallo reviewed on 2025-04-10 20:00_

---

_Review comment by @winstonallo on `crates/uv-cli/src/lib.rs`:929 on 2025-04-10 20:00_

Yes haha, fixed

---

_Review requested from @Gankra by @winstonallo on 2025-04-10 20:02_

---

_@Gankra reviewed on 2025-04-10 20:07_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4910 on 2025-04-10 20:07_

```suggestion
    // 
    // The nested Vec structure (`Vec<Vec<Maybe<Index>>>`) is required for clap's
    // value parsing mechanism, which processes one value at a time, in order to handle 
    // `UV_INDEX` the same way pip handles `PIP_EXTRA_INDEX_URL`.
```

---

_@Gankra reviewed on 2025-04-10 20:08_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4910 on 2025-04-10 20:08_

(This kind of detail shouldn't be part of `--help`)

---

_Comment by @Gankra on 2025-04-10 20:22_

Thanks for the quick ~~and evil~~ fix!

---

_Merged by @Gankra on 2025-04-10 20:22_

---

_Closed by @Gankra on 2025-04-10 20:22_

---

_Comment by @winstonallo on 2025-04-10 20:22_

> Thanks for the quick ~and evil~ fix!

Thank you for your quick feedback :))

---

_Branch deleted on 2025-06-27 09:12_

---
