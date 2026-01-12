```yaml
number: 8662
title: Tests are timing out locally
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-10-29T15:15:35Z
updated_at: 2024-11-04T12:31:01Z
url: https://github.com/astral-sh/uv/issues/8662
synced_at: 2026-01-12T15:59:31Z
```

# Tests are timing out locally

---

_@konstin_

On my machine, `cargo nextest run --cargo-profile fast-build --failure-output immediate-final --no-fail-fast` is timing out regularly. I assume that's because we added tests that do a lot more downloading.

```
     Summary [ 144.656s] 1842 tests run: 1833 passed (35 slow), 9 timed out, 4 skipped
     TIMEOUT [  90.013s] uv::it pip_tree::depth

--- STDOUT:              uv::it pip_tree::depth ---

running 1 test
test pip_tree::depth has been running for over 60 seconds

     TIMEOUT [  90.012s] uv::it pip_tree::invert

--- STDOUT:              uv::it pip_tree::invert ---

running 1 test
test pip_tree::invert has been running for over 60 seconds

     TIMEOUT [  90.012s] uv::it pip_tree::nested_dependencies

--- STDOUT:              uv::it pip_tree::nested_dependencies ---

running 1 test
test pip_tree::nested_dependencies has been running for over 60 seconds

     TIMEOUT [  90.011s] uv::it pip_tree::package_flag

--- STDOUT:              uv::it pip_tree::package_flag ---

running 1 test
test pip_tree::package_flag has been running for over 60 seconds

     TIMEOUT [  90.012s] uv::it pip_tree::prune

--- STDOUT:              uv::it pip_tree::prune ---

running 1 test
test pip_tree::prune has been running for over 60 seconds

     TIMEOUT [  90.012s] uv::it pip_tree::reverse

--- STDOUT:              uv::it pip_tree::reverse ---

running 1 test
test pip_tree::reverse has been running for over 60 seconds

     TIMEOUT [  90.011s] uv::it pip_tree::show_version_specifiers_with_invert

--- STDOUT:              uv::it pip_tree::show_version_specifiers_with_invert ---

running 1 test
test pip_tree::show_version_specifiers_with_invert has been running for over 60 seconds

     TIMEOUT [  90.012s] uv::it pip_tree::show_version_specifiers_with_package

--- STDOUT:              uv::it pip_tree::show_version_specifiers_with_package ---

running 1 test
test pip_tree::show_version_specifiers_with_package has been running for over 60 seconds

     TIMEOUT [  90.010s] uv::it python_install::python_install_freethreaded

--- STDOUT:              uv::it python_install::python_install_freethreaded ---

running 1 test
test python_install::python_install_freethreaded has been running for over 60 seconds

error: test run failed
```

---

_Label `internal` added by @konstin on 2024-10-29 15:15_

---

_Comment by @zanieb on 2024-10-29 15:34_

Wow >60s? Is your internet super slow? (not a dig, just wondering — the freethreaded test takes <10s on my machine)

---

_Comment by @charliermarsh on 2024-10-29 15:36_

I think we should get rid of scikit-learn in those `pip_tree` tests. They're slow for me too. I'll change them.

---

_Comment by @zanieb on 2024-10-29 16:10_

Related #878 

---

_Comment by @charliermarsh on 2024-10-29 22:02_

The `pip_tree` tests should be "fixed" now.

---

_Comment by @zanieb on 2024-10-29 22:10_

We could ignore the free-threaded test by default if it's a problem. (Until we add some cached mirroring for the tests). I don't understand your local tests being slower than CI though.

---

_Comment by @konstin on 2024-11-03 15:46_

The tests aren't timing out anymore, the install freethreaded test are very heavy though, it would be great if we could mock them (or cache the binary and use a mock server for the tests)

```
        PASS [  45.068s] uv::it python_install::python_install
        PASS [  48.313s] uv::it python_install::python_install_preview
        PASS [  55.449s] uv::it python_install::python_install_freethreaded
```

---

_Closed by @konstin on 2024-11-03 15:46_

---

_Comment by @zanieb on 2024-11-04 12:31_

Yes that's an explicit goal tracked in https://github.com/astral-sh/uv/issues/5939

Unfortunately we need the test coverage immediately.

---
