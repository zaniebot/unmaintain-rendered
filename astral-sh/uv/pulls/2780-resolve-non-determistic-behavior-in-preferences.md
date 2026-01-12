```yaml
number: 2780
title: Resolve non-determistic behavior in preferences due to site-packages ordering
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/test-installed-multiple
created_at: 2024-04-02T16:50:20Z
updated_at: 2024-04-02T20:14:47Z
url: https://github.com/astral-sh/uv/pull/2780
synced_at: 2026-01-12T16:05:13Z
```

# Resolve non-determistic behavior in preferences due to site-packages ordering

---

_@zanieb_

Originally a regression test for #2779 but we found out that there's some weird behavior where different `anyio` versions were preferred based on the platform.

---

_Label `testing` added by @zanieb on 2024-04-02 16:50_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3566 on 2024-04-02 16:50_

If I do not request 4.0.0, 3.7.0 is reinstalled without panic on my machine 🤯 

---

_@zanieb reviewed on 2024-04-02 16:50_

---

_Comment by @zanieb on 2024-04-02 16:57_

Cherry-picked over to https://github.com/astral-sh/uv/pull/2779 — we can discuss the weirdness of this test here if necessary but I'll merge the clear fix anyway.

---

_Renamed from "Add regression test for #2779" to "Resolve non-determistic behavior in preferences due to site-packages ordering" by @zanieb on 2024-04-02 18:03_

---

_@zanieb reviewed on 2024-04-02 18:05_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:05_

I can't figure out how to write this as a single iterator operation because it's weird working with nested results.

---

_@zanieb reviewed on 2024-04-02 18:06_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:69 on 2024-04-02 18:06_

This is the call we'd move into the iterator chain above

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3571 on 2024-04-02 18:06_

Before sorting the site-packages, this preferred `anyio==3.7.0` on macOS and `anyio==4.0.0` on Ubuntu

---

_@zanieb reviewed on 2024-04-02 18:06_

---

_Review requested from @charliermarsh by @zanieb on 2024-04-02 18:08_

---

_Label `testing` removed by @zanieb on 2024-04-02 18:08_

---

_Label `bug` added by @zanieb on 2024-04-02 18:08_

---

_@charliermarsh approved on 2024-04-02 18:09_

---

_@charliermarsh reviewed on 2024-04-02 18:10_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:10_

Do you mean, not collecting the `entries` here at all?

---

_Marked ready for review by @zanieb on 2024-04-02 18:13_

---

_@zanieb reviewed on 2024-04-02 18:17_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:17_

We must collect them to sort them afaik. I just couldn't filter to directories here.

---

_@charliermarsh reviewed on 2024-04-02 18:35_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:35_

Yeah. You would need to filter prior to the `collect` above.

---

_@zanieb reviewed on 2024-04-02 18:48_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:48_

Yeah I tried that haha it's awkward because of the nested `Result` types, it's beyond my understanding of iterators in Rust.

---

_Merged by @zanieb on 2024-04-02 18:48_

---

_Closed by @zanieb on 2024-04-02 18:48_

---

_Branch deleted on 2024-04-02 18:48_

---

_@zanieb reviewed on 2024-04-02 18:48_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 18:48_

cc @snowsignal maybe you have a suggestion

---

_@snowsignal reviewed on 2024-04-02 19:34_

---

_Review comment by @snowsignal on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 19:34_

@zanieb Here's one way you could do it:

```rust
let mut entries: Vec<_> = site_packages
                        .filter(|read_dir| match read_dir {
                            Ok(entry) => entry
                                .file_type()
                                .map(|file_type| file_type.is_dir())
                                .unwrap_or_default(),
                            Err(_) => true,
                        })
                        .collect::<std::io::Result<_>>()?;
```

Also, if you tweak the code to collect into a `BTreeSet<PathBuf>`, the sorting will happen as part of `collect()` and you don't have to call `sort_by_key` afterwards.

---

_@snowsignal reviewed on 2024-04-02 19:44_

---

_Review comment by @snowsignal on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 19:44_

Here's how you could do that:

```rust
                    let paths: BTreeSet<_> = site_packages
                        .filter_map(|read_dir| match read_dir {
                            Ok(entry) => {
                                entry.file_type().ok()?.is_dir().then_some(Ok(entry.path()))
                            }
                            Err(err) => Some(Err(err)),
                        })
                        .collect::<Result<_>>()?;
```

---

_@zanieb reviewed on 2024-04-02 20:14_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:53 on 2024-04-02 20:14_

Cool thank you!

---
