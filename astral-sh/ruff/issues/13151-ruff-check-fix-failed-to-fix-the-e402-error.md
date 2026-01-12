```yaml
number: 13151
title: "`ruff check --fix ` failed to fix the E402 error."
type: issue
state: closed
author: hongyi-zhao
labels: []
assignees: []
created_at: 2024-08-29T13:05:41Z
updated_at: 2024-08-30T03:28:45Z
url: https://github.com/astral-sh/ruff/issues/13151
synced_at: 2026-01-12T15:54:52Z
```

# `ruff check --fix ` failed to fix the E402 error.

---

_@hongyi-zhao_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

See below:

```shell
$ ruff --version
ruff 0.6.2

$ jupyter nbconvert --to python Untitled2.ipynb  
$ ruff check --fix --isolated Untitled2.py
Untitled2.py:75:1: E402 Module level import not at top of file
   |
73 | # In[1]:
74 | 
75 | from mp_api.client import MPRester
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
76 | 
77 | with MPRester() as mpr:
   |

Untitled2.py:87:1: E402 Module level import not at top of file
   |
85 | # In[3]:
86 | 
87 | from mp_api.client import MPRester
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
88 | from pymatgen.io.cif import CifWriter
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   |

Untitled2.py:88:1: E402 Module level import not at top of file
   |
87 | from mp_api.client import MPRester
88 | from pymatgen.io.cif import CifWriter
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   |

Untitled2.py:89:1: E402 Module level import not at top of file
   |
87 | from mp_api.client import MPRester
88 | from pymatgen.io.cif import CifWriter
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
90 | 
91 | # 使用你的API密钥初始化MPRester
   |

Found 4 errors.
```

On the other hand, the following command will fix these problems:

```shell
$ autopep8 --in-place --aggressive --aggressive Untitled2.py
```

Below are all the mentioned files here.

[Untitled2.zip](https://github.com/user-attachments/files/16799555/Untitled2.zip)

Regards,
Zhao

---

_Comment by @nathanjmcdougall on 2024-08-30 02:22_

In general, import statements can have side effects, so a safe autofix is not possible.

Support for an unsafe fix is tracked here:
https://github.com/astral-sh/ruff/issues/6514

---

_Comment by @hongyi-zhao on 2024-08-30 02:37_

But even I use the `--unsafe-fixes`, it still doesn't work:
```shell
(datasci) werner@x13dai-t:~/Desktop/mp_workflow$ ruff check --fix --unsafe-fixes Untitled2.py 
Untitled2.py:75:1: E402 Module level import not at top of file
   |
73 | # In[1]:
74 | 
75 | from mp_api.client import MPRester
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
76 | 
77 | with MPRester() as mpr:
   |

Untitled2.py:87:1: E402 Module level import not at top of file
   |
85 | # In[3]:
86 | 
87 | from mp_api.client import MPRester
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
88 | from pymatgen.io.cif import CifWriter
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   |

Untitled2.py:88:1: E402 Module level import not at top of file
   |
87 | from mp_api.client import MPRester
88 | from pymatgen.io.cif import CifWriter
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   |

Untitled2.py:89:1: E402 Module level import not at top of file
   |
87 | from mp_api.client import MPRester
88 | from pymatgen.io.cif import CifWriter
89 | from pymatgen.symmetry.analyzer import SpacegroupAnalyzer
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E402
90 | 
91 | # 使用你的API密钥初始化MPRester
   |

Found 4 errors.
```


---

_Comment by @nathanjmcdougall on 2024-08-30 02:50_

ruff doesn't currently have an unsafe fix implemented for this rule.

Discussion about adding one is here:
https://github.com/astral-sh/ruff/issues/6514

---

_Comment by @hongyi-zhao on 2024-08-30 03:27_

Thank you for your comments.

---

_Closed by @hongyi-zhao on 2024-08-30 03:27_

---

_Closed by @hongyi-zhao on 2024-08-30 03:28_

---

_Reopened by @hongyi-zhao on 2024-08-30 03:28_

---

_Closed by @hongyi-zhao on 2024-08-30 03:28_

---
