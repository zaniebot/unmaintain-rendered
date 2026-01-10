```yaml
number: 17177
title: Improve reliability of certain managed python tests on Windows CI
type: pull_request
state: merged
author: EliteTK
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: tk/remedy_windows_ci_timeouts
created_at: 2025-12-18T15:44:34Z
updated_at: 2025-12-29T13:24:51Z
url: https://github.com/astral-sh/uv/pull/17177
synced_at: 2026-01-10T05:49:14Z
```

# Improve reliability of certain managed python tests on Windows CI

---

_Pull request opened by @EliteTK on 2025-12-18 15:44_

### Summary

Results:

| Test                                                         | Time before | Time after |       Change |
| ------------------------------------------------------------ | -----------:| ----------:| ------------:|
| install_lower_patch_automatically                            | 79.317s     | 20.093s    | **-59.200s** |
| install_multiple_patches                                     | 70.086s     | 20.126s    | **-50.000s** |
| install_no_transparent_upgrade_with_venv_patch_specification | 47.971s     |  9.607s    | **-38.400s** |
| install_transparent_patch_upgrade_uv_venv                    | 74.886s     | 12.600s    | **-62.300s** |
| install_transparent_patch_upgrade_venv_module                | 57.888s     | 10.854s    | **-47.000s** |
| python_find_prerelease                                       | 78.424s     | 18.302s    | **-60.100s** |
| python_install                                               | 47.827s     |  4.229s    | **-43.600s** |
| python_install_automatic                                     | 71.138s     | 11.836s    | **-59.300s** |
| python_install_build_version                                 | 35.983s     |  3.214s    | **-32.800s** |
| python_install_build_version_pypy                            | 80.750s     | 17.774s    | **-63.000s** |
| python_install_cached                                        |  6.752s     |  2.247s    |   -04.500s   |
| python_install_default                                       |  3.748s     |  4.202s    |   +00.454s   |
| python_install_default_from_env                              |  2.829s     |  2.190s    |   -00.639s   |
| python_install_default_prerelease                            |  1.136s     |  0.911s    |   -00.225s   |
| python_install_default_preview                               |  5.709s     |  3.601s    |   -02.110s   |
| python_install_emulated_windows_x86_on_x64                   | 90.025s     | 35.652s    | **-54.400s** |
| python_install_force                                         |  1.523s     |  1.365s    |   -00.158s   |
| python_install_freethreaded                                  | 36.136s     |  4.793s    | **-31.300s** |
| python_install_invalid_request                               |  0.299s     |  0.269s    |   -00.030s   |
| python_install_minor                                         |  1.496s     |  1.309s    |   -00.187s   |
| python_install_multiple_patch                                |  3.033s     |  1.415s    |   -01.620s   |
| python_install_no_cache                                      |  2.853s     |  2.311s    |   -00.542s   |
| python_install_prerelease                                    |  2.018s     |  2.006s    |   -00.012s   |
| python_install_preview                                       | 40.594s     |  6.870s    | **-33.700s** |
| python_install_preview_no_bin                                |  1.080s     |  1.177s    |   +00.097s   |
| python_install_preview_upgrade                               |  6.609s     |  6.295s    |   -00.314s   |
| python_install_unknown                                       |  0.211s     |  0.206s    |   -00.005s   |
| python_install_upgrade                                       |  9.153s     | 10.648s    |   +01.490s   |
| python_install_upgrade_version_file                          | 55.920s     | 12.943s    | **-43.000s** |
| python_reinstall                                             |  4.939s     |  6.225s    |   +01.290s   |
| python_reinstall_patch                                       |  2.716s     |  2.899s    |   +00.183s   |
| python_upgrade_not_allowed                                   |  0.215s     |  0.215s    |   +00.000s   |
| regression_cpython                                           | 30.779s     |  4.403s    | **-26.400s** |
| uninstall_highest_patch                                      | 40.716s     |  3.790s    | **-36.900s** |
| uninstall_last_patch                                         | 19.093s     |  2.965s    | **-16.100s** |

Comparing https://github.com/astral-sh/uv/actions/runs/20342580132/job/58446553156?pr=17177 against https://github.com/astral-sh/uv/actions/runs/20338690535/job/58431797575.

Overall test time went down from 279.163s to 258.427s - presumably not related though as this should be marginally slower. Here are some local tests (on linux):

* just python_install:
  * current config:  
    `Summary [  34.452s] 45 tests run: 44 passed (12 slow), 1 failed, 3314 skipped`
  * `threads-required = "num-test-threads"`:  
    `Summary [ 117.991s] 45 tests run: 44 passed (1 slow), 1 failed, 3314 skipped`
  * `threads-required = 4`:  
    `Summary [  48.793s] 45 tests run: 44 passed (4 slow), 1 failed, 3314 skipped`
* all tests:
  * current config:  
    `Summary [ 371.911s] 3357 tests run: 3356 passed (49 slow), 1 failed, 2 skipped`
  * `threads-required = "num-test-threads"`:  
    `Summary [ 451.323s] 3357 tests run: 3356 passed (26 slow), 1 failed, 2 skipped`
  * `threads-required = 4`:  
    `Summary [ 386.213s] 3357 tests run: 3356 passed (31 slow), 1 failed, 2 skipped`
    
(Failed test is not related)

### Conclusion

I think this is a good way to gain more reliability for these tests on all platforms, but I am not certain that we should be setting this override in the config. I think realistically you want something more like `num-test-threads / 3` rather than just `4`.

---

_Label `no-build` added by @EliteTK on 2025-12-18 15:44_

---

_Renamed from "Attempt to improve reliability of python_install tests" to "Improve `python_install` test reliability on Windows" by @EliteTK on 2025-12-18 17:09_

---

_Label `internal` added by @EliteTK on 2025-12-18 17:09_

---

_Label `no-build` removed by @EliteTK on 2025-12-18 17:09_

---

_Marked ready for review by @EliteTK on 2025-12-18 17:24_

---

_Review requested from @zanieb by @EliteTK on 2025-12-18 17:24_

---

_@zanieb reviewed on 2025-12-18 17:31_

---

_Review comment by @zanieb on `.config/nextest.toml`:22 on 2025-12-18 17:31_

There are also python upgrade and python find tests that use managed versions. Can we tie this to the `managed-python` feature instead somehow?

---

_Review comment by @EliteTK on `.config/nextest.toml`:22 on 2025-12-18 17:34_

I can't see a way to do it per-feature `https://nexte.st/docs/filtersets/reference/`.

We could just select more tests manually.

---

_@EliteTK reviewed on 2025-12-18 17:34_

---

_Comment by @EliteTK on 2025-12-18 17:36_

Looks like it's only half working now...?

(after the change to using `max-threads`)

---

_@zanieb reviewed on 2025-12-18 17:39_

---

_Review comment by @zanieb on `.config/nextest.toml`:22 on 2025-12-18 17:39_

I think we'd need to move the tests to a specific package or something. I just worry this is brittle long-term.

---

_@zanieb reviewed on 2025-12-18 17:39_

---

_Review comment by @zanieb on `.config/nextest.toml`:22 on 2025-12-18 17:39_

(the `native_auth` filter is also bothersome and janky)

---

_Comment by @EliteTK on 2025-12-18 18:17_

Okay, it looks like `max-threads = N` will limit the number of concurrent runs of that group of tests. But that limit only applies to that group. So if you're currently executing `N` threads worth of work from a group, nextest will fill `num-test-threads - N` worth of slots with other work, which isn't exactly what we want here...

<img width="2764" height="466" alt="image" src="https://github.com/user-attachments/assets/f907831b-b5e6-45ce-a0f6-9aa51cdfe52d" />
 
Whereas, if you have a bunch of tests with `threads-required = num-test-threads / N`, they will prevent anything else from running until nextest reaches almost the end of that block of tests. (which is actually not exactly what we want either, but it's closer)

It would be good if nextest had a way of running these completely separately... I think for now maybe we can just use `threads-required = 4`...

---

_Converted to draft by @EliteTK on 2025-12-18 18:31_

---

_Comment by @EliteTK on 2025-12-18 19:05_

Original results with an extra columns for max-threads and for serialising all the python_install tests:

| Test                                         | Current |  Heavy | Change | max-threads | Change | Serial | Change |
| -------------------------------------------- | -------:| ------:| ------:| -----------:| ------:| ------:| ------:|
| `install_lower_patch_automatically`          | 79.317s | 20.093 | -74.7% |      37.393 | -52.9% | 40.937 | -48.4% |
| `install_multiple_patches`                   | 70.086s | 20.126 | -71.3% |      61.599 | -12.1% | 12.926 | -81.6% |
| `install_no_transparent_...`                 | 47.971s |  9.607 | -80.0% |      53.257 |  11.0% |  5.193 | -89.2% |
| `install_transparent_..._uv_venv`            | 74.886s | 12.600 | -83.2% |      20.155 | -73.1% |  8.228 | -89.0% |
| `install_transparent_..._venv_module`        | 57.888s | 10.854 | -81.2% |      16.659 | -71.2% | 18.331 | -68.3% |
| `python_find_prerelease`                     | 78.424s | 18.302 | -76.7% |      21.267 | -72.9% | 14.981 | -80.9% |
| `python_install`                             | 47.827s |  4.229 | -91.2% |       6.091 | -87.3% |  3.238 | -93.2% |
| `python_install_automatic`                   | 71.138s | 11.836 | -83.4% |      19.308 | -72.9% |  4.717 | -93.4% |
| `python_install_build_version`               | 35.983s |  3.214 | -91.1% |       6.757 | -81.2% |  1.910 | -94.7% |
| `python_install_build_version_pypy`          | 80.750s | 17.774 | -78.0% |      22.177 | -72.5% |  1.736 | -97.9% |
| `python_install_cached`                      |  6.752s |  2.247 | -66.7% |       3.543 | -47.5% |  1.960 | -71.0% |
| `python_install_default`                     |  3.748s |  4.202 |  12.1% |       3.846 |   2.6% |  2.599 | -30.7% |
| `python_install_default_from_env`            |  2.829s |  2.190 | -22.6% |       3.402 |  20.3% |  1.860 | -34.3% |
| `python_install_default_prerelease`          |  1.136s |  0.911 | -19.8% |       1.799 |  58.4% |  0.840 | -26.1% |
| `python_install_default_preview`             |  5.709s |  3.601 | -36.9% |       5.788 |   1.4% |  3.421 | -40.1% |
| `python_install_emulated_windows_x86_on_x64` | 90.025s | 35.652 | -60.4% |      48.517 | -46.1% |  3.824 | -95.8% |
| `python_install_force`                       |  1.523s |  1.365 | -10.4% |       2.037 |  33.7% |  1.041 | -31.6% |
| `python_install_freethreaded`                | 36.136s |  4.793 | -86.7% |      10.325 | -71.4% |  2.656 | -92.6% |
| `python_install_invalid_request`             |  0.299s |  0.269 | -10.0% |       0.802 | 168.2% |  0.253 | -15.4% |
| `python_install_minor`                       |  1.496s |  1.309 | -12.5% |       2.391 |  59.8% |  1.255 | -16.1% |
| `python_install_multiple_patch`              |  3.033s |  1.415 | -53.3% |       3.268 |   7.7% |  1.421 | -53.1% |
| `python_install_no_cache`                    |  2.853s |  2.311 | -19.0% |       3.893 |  36.5% |  1.865 | -34.6% |
| `python_install_prerelease`                  |  2.018s |  2.006 |  -0.6% |       3.546 |  75.7% |  1.657 | -17.9% |
| `python_install_preview`                     | 40.594s |  6.870 | -83.1% |      32.439 | -20.1% |  4.958 | -87.8% |
| `python_install_preview_no_bin`              |  1.080s |  1.177 |   9.0% |       1.479 |  36.9% |  0.881 | -18.4% |
| `python_install_preview_upgrade`             |  6.609s |  6.295 |  -4.8% |       7.962 |  20.5% |  3.882 | -41.3% |
| `python_install_unknown`                     |  0.211s |  0.206 |  -2.4% |       0.285 |  35.1% |  0.197 |  -6.6% |
| `python_install_upgrade`                     |  9.153s | 10.648 |  16.3% |      13.040 |  42.5% |  7.188 | -21.5% |
| `python_install_upgrade_version_file`        | 55.920s | 12.943 | -76.9% |      33.699 | -39.7% |  2.494 | -95.5% |
| `python_reinstall`                           |  4.939s |  6.225 |  26.0% |       7.859 |  59.1% |  3.988 | -19.3% |
| `python_reinstall_patch`                     |  2.716s |  2.899 |   6.7% |       4.737 |  74.4% |  2.568 |  -5.4% |
| `python_upgrade_not_allowed`                 |  0.215s |  0.215 |   0.0% |       0.281 |  30.7% |  0.198 |  -7.9% |
| `regression_cpython`                         | 30.779s |  4.403 | -85.7% |      14.943 | -51.5% |  1.106 | -96.4% |
| `uninstall_highest_patch`                    | 40.716s |  3.790 | -90.7% |      17.303 | -57.5% |  2.263 | -94.4% |
| `uninstall_last_patch`                       | 19.093s |  2.965 | -84.5% |       4.293 | -77.5% |  1.674 | -91.2% |

Some of this could be noise, but some tests seem to be slow running when ran with a bunch of other tests, and some seem to be very slow only with other python_install tests. But most seem to fall into the latter group.

As a bonus, however, in a full run with no filters, there seems to be almost no overhead to running these all as serialised.

(Updated with change %)

---

_@EliteTK reviewed on 2025-12-18 19:12_

---

_Review comment by @EliteTK on `.config/nextest.toml`:22 on 2025-12-18 19:12_

Yeah putting them in a specific package would let us filter on them. Maybe something for an issue and a separate PR though?

I will add `python_upgrade`. But for `python_find`, there are only 8 tests in there which seem to install python. Seems kind of heavy handed to add the whole module. There are also a bunch of other places in the tests where `python_install` or `python_upgrade` are called. 

One option I guess we could consider is tagging the tests via their name. E.g. by putting any such tests within a `py_install` sub module and then matching on "::py_install::"?

---

_Label `windows` added by @EliteTK on 2025-12-18 19:20_

---

_@EliteTK reviewed on 2025-12-22 21:00_

---

_Review comment by @EliteTK on `.config/nextest.toml`:22 on 2025-12-22 21:00_

For now I've just added python_upgrade and python_find and made the test more specific but I think adding markers to the test module paths wouldn't be a terrible idea.

---

_Renamed from "Improve `python_install` test reliability on Windows" to "Improve reliability of certain managed python tests on Windows CI" by @EliteTK on 2025-12-22 21:04_

---

_Marked ready for review by @EliteTK on 2025-12-22 21:39_

---

_Review requested from @zanieb by @EliteTK on 2025-12-22 21:39_

---

_@zanieb approved on 2025-12-22 21:40_

---

_Comment by @zanieb on 2025-12-22 21:41_

Unfortunately this will miss, e.g., your new test case in https://github.com/astral-sh/uv/pull/17218

We should think about how we can do better in the future. Maybe we should even just write a tool that generates the nextest config from feature flagged tests, if we can?

---

_Merged by @EliteTK on 2025-12-29 13:24_

---

_Closed by @EliteTK on 2025-12-29 13:24_

---

_Branch deleted on 2025-12-29 13:24_

---
