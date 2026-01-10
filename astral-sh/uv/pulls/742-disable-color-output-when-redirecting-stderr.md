```yaml
number: 742
title: Disable color output when redirecting stderr
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: tty-color-redirect
created_at: 2024-01-02T11:08:19Z
updated_at: 2024-01-04T15:43:45Z
url: https://github.com/astral-sh/uv/pull/742
synced_at: 2026-01-10T15:44:44Z
```

# Disable color output when redirecting stderr

---

_Pull request opened by @konstin on 2024-01-02 11:08_

I'm still confused about it, but this seems to do the right thing?

`HierarchicalLayer` internally has [`let ansi = io::stderr().is_terminal();`](https://github.com/davidbarsky/tracing-tree/blob/fcd9eed2528e41cd776b19b9ba45d338ae62a5fc/src/lib.rs#L74), so the logging itself is already correctly uncolored, but errors in the log weren't.

Test command, ran with network deactivated:

```shell
RUST_LOG=debug cargo run --bin puffin -- pip-compile -v ./scripts/popular_packages/pypi_8k_downloads.txt 2> log.txt
```

**Before**

```
[1;31merror[0m: Request error: error sending request for url (https://pypi.org/simple/apache-airflow-providers-dbt-cloud/): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  [1;31mCaused by[0m: error sending request for url (https://pypi.org/simple/apache-airflow-providers-dbt-cloud/): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  [1;31mCaused by[0m: error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  [1;31mCaused by[0m: dns error: failed to lookup address information: Temporary failure in name resolution
  [1;31mCaused by[0m: failed to lookup address information: Temporary failure in name resolution
  ```

  **After**

  ```
  error: Request error: error sending request for url (https://pypi.org/simple/fissix/): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
    Caused by: error sending request for url (https://pypi.org/simple/fissix/): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
    Caused by: error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
    Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
    Caused by: failed to lookup address information: Temporary failure in name resolution
```

---

_Review requested from @BurntSushi by @konstin on 2024-01-02 11:08_

---

_Review requested from @charliermarsh by @konstin on 2024-01-02 11:08_

---

_Comment by @charliermarsh on 2024-01-03 20:37_

Can we find a way to do this with anstream?

---

_@BurntSushi approved on 2024-01-04 14:24_

I'm fine with this if it fixes the issue, but from a quick look at `puffin`'s dependencies, it seems like we're using no fewer than three different terminal coloring libraries? (I see `anstream`, `colored` and `nu-ansi-term`.) It'd be nice to fix that, and I agree that we should pick `anstream`.

---

_Merged by @konstin on 2024-01-04 15:43_

---

_Closed by @konstin on 2024-01-04 15:43_

---

_Branch deleted on 2024-01-04 15:43_

---
