---
number: 13607
title: "Support for `unsafe-binary-first` or `unsafe-wheel-first` in `tool.uv.index-strategy`"
type: issue
state: open
author: gbdlin
labels:
  - enhancement
assignees: []
created_at: 2025-05-22T23:29:48Z
updated_at: 2025-05-23T08:14:49Z
url: https://github.com/astral-sh/uv/issues/13607
synced_at: 2026-01-07T13:12:18-06:00
---

# Support for `unsafe-binary-first` or `unsafe-wheel-first` in `tool.uv.index-strategy`

---

_Issue opened by @gbdlin on 2025-05-22 23:29_

### Summary

Right now there are 3 strategies of resolving packages with multiple indexes: 
- `first-index` always picking first index where package exists
- `unsafe-first-match` allows moving to the next index if package exist in previous one, but no version satisfies requirements
- `unsafe-best-match` resolves versions from all indexes and picks the best matching version from any index.

But sometimes there is need for a 4th option: pick first version from any index that does have binary resolution for the current platform (either matching first found, or matching best found). This is very useful especially when developing for 

### Example

Given example project:

```toml
[project]                          
name = "project-name"                                                                                                                                                                                                                                                                                
version = "0.1.0"                                                                                                                                                                                                                                                                               
description = "Add your description here"                                                                                                                                                                                                                                                              
readme = "README.md"                                                                                                                                                                                                                                                                                   
requires-python = ">=3.11"
dependencies = [ 
    "pydantic~=2.11.5"                        
]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
          
[[tool.uv.index]]                                                                                                                                                                             
name = "pypi"                                                                                                                                                                                 
url = "https://pypi.org/simple"
                                                 
[[tool.uv.index]]                                                                                                                                                                                                                                                                         
name = "piwheels"                                                                                                                                                                                                                                                                                    
url = "https://piwheels.org/simple"                                                                                                                                                                                                                                                             
```

When installing it on 32 bit Raspberry PI system, uv will not use wheel for `pydantic-core` available on piwheels, but instead will compile the package from source. When we swap the order of used indexes or use any other index strategy, we will either fetch all packages from `piwheels` index, where newest versions of packages can be published with a delay, so if a universal wheel for any package exists, normal pypi index would be preferred.

With an option to prefer sources with binary distribution whenever possible, uv could resolve only `pydantic-core` from `piwheels` index and rest of it from `pypi` and manual pinning won't be necessary.
      

---

_Label `enhancement` added by @gbdlin on 2025-05-22 23:29_

---

_Assigned to @jtfmumm by @konstin on 2025-05-23 08:14_

---
