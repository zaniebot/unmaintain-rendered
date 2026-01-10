```yaml
number: 7183
title: Improve Python version resolution UI
type: issue
state: closed
author: lmmx
labels:
  - error messages
assignees: []
created_at: 2024-09-07T21:52:44Z
updated_at: 2024-09-10T02:51:19Z
url: https://github.com/astral-sh/uv/issues/7183
synced_at: 2026-01-10T04:45:10Z
```

# Improve Python version resolution UI

---

_Issue opened by @lmmx on 2024-09-07 21:52_

I just resurrected an old project (I suspect over 10 years old, changed hands entirely at one point and was rewritten from Python 2 to 3)

I managed to install it OK, but when I added another unrelated dependency, I got errors.

It turned out it was to do with an overly relaxed (and thus irreconcilable) NumPy constraint.

NumPy dropped support for Python 3.8 around v1.25, and demanded support for Python 3.10 as of v2.1.0

PDM gives a huge list of messages that hint to the user that the problem is (if they read between the lines) that you can't satisfy an unpinned numpy with a requires-python TOML config of ">=3.8,<3.13", too ambiguous

<details><summaryClick to show some of said PDM messages</summary>

```
(page-dewarp) louis 🚶 ~/lab/other/page-dewarp $ pdm install                                                                                                                                                                                                                    
WARNING: Lockfile does not exist                                                                                                                                                                                                                                                
Updating the lock file...                                                                                                                                                                                                                                                       
INFO: Inside an active virtualenv /home/louis/lab/other/page-dewarp/.venv, reusing it.                                                                                                                                                                                          
Set env var PDM_IGNORE_ACTIVE_VENV to ignore it.                                                                                                                                                                                                                                
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@2.1.1 because it requires Python>=3.10 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@2.1.1, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.10" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@2.1.0 because it requires Python>=3.10 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@2.1.0, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.10" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@2.0.2 because it requires Python>=3.9 but                                                                                                                     
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@2.0.2, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                    
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@2.0.1 because it requires Python>=3.9 but                                                                                                                     
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@2.0.1, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                    
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@2.0.0 because it requires Python>=3.9 but                                                                                                                     
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@2.0.0, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                    
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.26.4 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.26.4, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.26.3 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.26.3, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.26.2 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.26.2, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.26.1 because it requires Python<3.13,>=3.9                                                                                                                  
but the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                            
If you want to install numpy@1.26.1, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.26.0 because it requires Python<3.13,>=3.9                                                                                                                  
but the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                            
If you want to install numpy@1.26.0, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.25.2 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.25.2, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.25.1 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.25.1, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping numpy@1.25.0 because it requires Python>=3.9 but                                                                                                                    
the lock targets to work with Python<3.13,>=3.8. Instead, another version of numpy that supports Python<3.13,>=3.8 will be used.                                                                                                                                                
If you want to install numpy@1.25.0, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.                                                                                                                                   
  return self.repository.find_candidates(                                                                                                                                                                                                                                       
/home/louis/miniconda3/lib/python3.12/site-packages/pdm/resolver/providers.py:200: PackageWarning: Skipping matplotlib@3.9.2 because it requires Python>=3.9                                                                                                                    
but the lock targets to work with Python<3.13,>=3.8. Instead, another version of matplotlib that supports Python<3.13,>=3.8 will be used.                                                                                                                                       
If you want to install matplotlib@3.9.2, narrow down the `requires-python` range to include this version. For example, "<3.13,>=3.9" should work.           
```

</details>

uv (latest version) however is showing a relatively concise and uninformative error.

If you inspect the traceback you might assume you're dealing with a setuptools issue (red herring: this is the build backend for numpy, the package doesn't use that backend).

It looks like 1.24.4 was chosen in this case, but when the python-requires was amended as PDM suggested, the uv installation succeeded.

```
(page-dewarp) louis 🚶 ~/lab/other/page-dewarp $ uv add msgspec                                                                         
Resolved 23 packages in 39ms                                        
error: Failed to prepare distributions                              
  Caused by: Failed to fetch wheel: numpy==1.24.4                                                                                       
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1                                                                                                                                                                          
--- stdout:                                                         

--- stderr:                                                         
Traceback (most recent call last):                                  
  File "<string>", line 8, in <module>                              
  File "/home/louis/.cache/uv/builds-v0/.tmpr2bEaK/lib/python3.12/site-packages/setuptools/__init__.py", line 10, in <module>                                                                                                                                                   
    import distutils.core                                           
ModuleNotFoundError: No module named 'distutils'                                                                                        
---                                   
```

I imagine the solution here is for uv to be on the lookout for this category of error and fail fast sooner (or at least give helpful messages before failing)?

Let me know if I haven't explained any of the setup here clearly: for reference the pathological python-requires pin was [here](https://github.com/lmmx/page-dewarp/blob/feb0d7af28b2e54adc440384ebb791813c86c93a/pyproject.toml#L43)

---

_Comment by @charliermarsh on 2024-09-08 15:16_

I think we should add some custom messages for `ModuleNotFoundError: No module named 'distutils' ` and similar errors, at the very least.

---

_Comment by @charliermarsh on 2024-09-08 15:17_

We do have messages like the PDM messages above, but we only show them on failure. I guess we could show them with `--verbose` while solving?

---

_Label `error messages` added by @charliermarsh on 2024-09-08 15:17_

---

_Comment by @charliermarsh on 2024-09-08 15:21_

I'm also sort of wondering if we should treat these as non-fatal errors so that we can continue backtracking... But it's not great, would be pretty inefficient if we had to test a lot of versions this way.

---

_Comment by @charliermarsh on 2024-09-09 14:29_

Just to clarify, what was the solution here? You added `requires-python = ">=3.8, <=3.10"`? Something else?

---

_Comment by @lmmx on 2024-09-09 18:59_

[Yes yes](https://github.com/lmmx/page-dewarp/blob/405c65b73b1fcff7266375ac47b1fa1a47128df8/pyproject.toml#L41)

```toml
requires-python = ">=3.9,<3.13"
```

I also [pinned numpy](https://github.com/lmmx/page-dewarp/blob/405c65b73b1fcff7266375ac47b1fa1a47128df8/pyproject.toml#L20-L22) in the `project.dependencies` with a comment:

```toml
dependencies = [
  "numpy>1.25",
  # NumPy 1.25+ requires Python >= 3.9
```

The PDM UI of "here are all these constraints, which make it plain to see why there's ambiguity, which may in fact prevent resolution entirely".

It is more of a clue, I noted that some combos of constraints **would** resolve but would show these conflicts along the way (so it's not an error so much as a warning of a not-ideal situation, as in _"potential degradation ahead, review this part if it turns out to be fatal"_)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 19:04_

---

_Closed by @charliermarsh on 2024-09-10 02:51_

---

_Closed by @charliermarsh on 2024-09-10 02:51_

---
