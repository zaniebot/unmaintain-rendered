```yaml
number: 9988
title: Dependency Resolver Ignores Platform-Specific Version Constraints
type: issue
state: closed
author: merajhashemi
labels:
  - resolver
assignees: []
created_at: 2024-12-18T03:35:04Z
updated_at: 2025-02-15T20:35:06Z
url: https://github.com/astral-sh/uv/issues/9988
synced_at: 2026-01-12T16:00:04Z
```

# Dependency Resolver Ignores Platform-Specific Version Constraints

---

_@merajhashemi_

Hello,  

I want to bring up an issue again about PyTorch dependency resolution on Intel-based macOS. This follows the discussion in #9515, which I wasn’t able to reopen.

I really appreciate the updates with the `fork-strategy` option and the closure of issues #8686 and #7190, but the main problem I brought up is still there: the dependency resolver enforces a single version of PyTorch across all platforms, even when conditional version constraints are specified in `pyproject.toml`.  

Here’s a minimal example of my configuration:  

```toml  
[project]  
name = "test"  
version = "0.1.0"  
description = "Add your description here"  
readme = "README.md"  
requires-python = ">=3.11"  
dependencies = [  
    "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",  
    "torch>1.13; sys_platform != 'darwin' or platform_machine != 'x86_64'",  
]  

[tool.uv]  
fork-strategy = "requires-python"  
```  

The expectation here is that the resolver should pick PyTorch version 2.2.2 for Intel MacOS, and a different version (e.g., 2.5.1) for other platforms. However, the current behavior enforces a single version (2.2.2) across all platforms.  

I was hoping the introduction of the `fork-strategy` option would address this limitation, but it doesn’t seem to allow opting out of this unified resolution behavior.  

Would it be possible to consider adding a new `fork-strategy` option (or another mechanism) to handle cases like this? This would allow for platform-specific resolution of dependencies as defined in the `pyproject.toml`. 

---

_Comment by @charliermarsh on 2024-12-18 17:27_

That PR is unrelated -- you're looking for https://github.com/astral-sh/uv/issues/9711 and the linked PRs. I don't know if we'll support this specific case by default though. If you want to be getting an old version of a package for a platform like Intel macOS (which is no longer supported in new PyTorch versions), you'll likely be required to opt-in in some way.


---

_Label `resolver` added by @charliermarsh on 2024-12-18 17:27_

---

_Comment by @merajhashemi on 2024-12-18 17:31_

@konstin @charliermarsh I noticed your discussion regarding this issue in #9711. I have a suggestion that might help address the problem, although I'm not fully aware of its potential implications. Perhaps you could consider adding an option for the resolver to function similarly to the resolver in the pip interface.

---

_Comment by @charliermarsh on 2024-12-18 17:34_

It's actually plausible that your case could be supported since you're willing to define multiple version constraints.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-18 17:36_

---

_Comment by @charliermarsh on 2024-12-18 18:05_

Can I ask why you don't do:

```
dependencies = [  
    "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",  
    "torch>=2.3; sys_platform != 'darwin' or platform_machine != 'x86_64'",  
]  
```


---

_Comment by @merajhashemi on 2024-12-18 20:17_

> It's actually plausible that your case could be supported since you're willing to define multiple version constraints.

That’s true, but ideally, I’d prefer if it worked the same way as pip, where a simple `dependencies = ["torch>1.13"]` would resolve correctly across all platforms without needing platform-specific constraints.


> Can I ask why you don't do:
> 
> ```
> dependencies = [  
>     "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",  
>     "torch>=2.3; sys_platform != 'darwin' or platform_machine != 'x86_64'",  
> ]  
> ```

This is for a library I’m working on, and we want to support all PyTorch versions above 1.13. Forcing users to install version 2.3 or higher on some platforms wouldn’t work well since it limits compatibility for no good reason.

---

_Comment by @charliermarsh on 2025-02-15 20:26_

I still think we can make this better, but in the next version of uv, you can use:

```toml
[project]  
name = "test"  
version = "0.1.0"  
description = "Add your description here"  
readme = "README.md"  
requires-python = ">=3.11"  
dependencies = [  
    "torch>1.13"
]  

[tool.uv]  
fork-strategy = "requires-python"  
required-environments = [
    "sys_platform == 'darwin' and platform_machine == 'x86_64'"
]
```

---

_Closed by @charliermarsh on 2025-02-15 20:26_

---
