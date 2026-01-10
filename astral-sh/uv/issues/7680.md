```yaml
number: 7680
title: Regression in prefetching unbounded-lower-bound packages
type: issue
state: closed
author: topherinternational
labels:
  - bug
assignees: []
created_at: 2024-09-25T07:49:03Z
updated_at: 2024-09-25T12:35:42Z
url: https://github.com/astral-sh/uv/issues/7680
synced_at: 2026-01-10T04:45:10Z
```

# Regression in prefetching unbounded-lower-bound packages

---

_Issue opened by @topherinternational on 2024-09-25 07:49_

#4136 (implemented by #4149) addressed an issue where the prefetcher was fetching the lowest-ever version of a dependency, which wasn't compatible with the Python version, and the wheel build for that version failed and killed the resolver process.

It looks like this has regressed in uv 0.4.8 with the same behavior seen in #4136. Failures were seen when Airflow tried to upgrade to 0.4.8+ in https://github.com/apache/airflow/pull/42274.

I have a more minimal repro that doesn't require checking out airflow (these are the key packages causing the conflict in the `-e ".[google]"` install from #4136):

1. Create and activate a fresh Python 3.8 environment
2. Run `uv pip install --resolution lowest --upgrade pandas-gbq==0.7.0 "pandas==1.5.3 ; python_version < \"3.9\"" --dry-run --verbose`
```
(venv38) ~/code/airflow (main) % uv pip install --resolution lowest --upgrade pandas-gbq==0.7.0 "pandas==1.5.3 ; python_version < \"3.9\"" --dry-run
error: Failed to download and build `pandas==0.1`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/cpanderson/Library/Caches/uv/builds-v0/.tmpWkAeOB/lib/python3.8/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/Users/cpanderson/Library/Caches/uv/builds-v0/.tmpWkAeOB/lib/python3.8/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/cpanderson/Library/Caches/uv/builds-v0/.tmpWkAeOB/lib/python3.8/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/cpanderson/Library/Caches/uv/builds-v0/.tmpWkAeOB/lib/python3.8/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'numpy'
---
```

I repro'd from source bisecting between 0.4.7 and 0.4.8 and found the exact change that causes the regression: #7226 (@charliermarsh). 

`pandas-gbq` 0.7.0 has an unbounded dependency on `pandas`, so with `--resolution lowest` the prefetcher tries to load the earliest pandas version of 0.1 which doesn't build properly. #4149 was written to prevent fetching if a dependency has an unbounded lower-bound, iow this exact scenario.

#7226 changes the line that detects an unbounded source dist:

```diff
                 // Avoid prefetching source distributions with unbounded lower-bound ranges. This
                 // often leads to failed attempts to build legacy versions of packages that are
                 // incompatible with modern build tools.
-                if !dist.prefetchable() {
+                if dist.wheel().is_some() {
                     if !self.selector.use_highest_version(&package_name) {
                         if let Some((lower, _)) = range.iter().next() {
                             if lower == &Bound::Unbounded {
```

@charliermarsh is it possible this comparison is backwards? It seems like this block should be entered if the dist is NOT a wheel, this may have been inadvertently reversed when #7226 was written?

cc @potiuk @dirrao


---

_Closed by @charliermarsh on 2024-09-25 12:35_

---

_Comment by @charliermarsh on 2024-09-25 12:35_

Thank you for the thorough analysis... Yes, I think you're right.

---

_Label `bug` added by @charliermarsh on 2024-09-25 12:35_

---
