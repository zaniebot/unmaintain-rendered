```yaml
number: 13752
title: "[C416 unnecessary-comprehension] Suggests a fix leading to RecursionError"
type: issue
state: open
author: echoix
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-10-14T14:58:11Z
updated_at: 2024-10-17T08:04:33Z
url: https://github.com/astral-sh/ruff/issues/13752
synced_at: 2026-01-12T15:54:53Z
```

# [C416 unnecessary-comprehension] Suggests a fix leading to RecursionError

---

_@echoix_

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

Rule [unnecessary-comprehension (C416)](https://docs.astral.sh/ruff/rules/unnecessary-comprehension/#unnecessary-comprehension-c416) suggests replacing comprehensions by the constructor instead. However, in some cases, when the replacement is done in code inside or called by `__len__`, it causes a recursion error.

After searching a bit on SO, the list constructor calls `__len__` as an optimization when available, https://stackoverflow.com/questions/41474829/why-does-list-ask-about-len https://github.com/python/cpython/blob/67f6e08147bc005e460d82fcce85bf5d56009cf5/Objects/listobject.c#L1164

What I would expect:
- (Easier) A mention of general cases where the rule is unsafe in the docs, rather than suggesting that the rule is unsafe only because of comments possibly lost
- (Harder, but not absolutely required) Track if the change to calling list constructor is done inside the `__len__` method of the same object that the list is called on.
- (Hardest) Also track through function calls, not only inside the same method.

### List of keywords searched:
`C416`, `list len` (all open and open closed issues for all keywords)

### A stripped-down repro:

- File is named: `c416_recursionerror.py`
- The command you invoked: `ruff check --isolated c416_recursionerror.py --select C416 --fix --unsafe-fixes`

Take note that some other instances of the same problem in our project is wrapping some wxPython, not files, and can't directly be reshaped in a pythonic way easily, in order to store the items in the class and cache the length; it needs to iterate. Stripped down an easier case here, but the others had `def __len__(self):  return len([layer for layer in self])` but I couldn't isolate it.

<details><summary>File c416_recursionerror.py</summary>
<p>

```python
from abc import abstractmethod
from os import listdir
from os.path import join, isdir
import fnmatch


def is_valid(value, path, type):
    """Private function to check the correctness of a value."""
    return True


def return_bool(argument):
    return True


def always_true_for_repro():
    return True


class Mapset:
    def __init__(self, mapset="", location="", gisdbase=""):
        self.gisdbase = gisdbase
        self.location = location
        self.name = mapset


class LocationBase:
    """Location object ::

        >>> from grass.script.core import gisenv
        >>> location = Location()
        >>> location                                      # doctest: +ELLIPSIS
        Location(...)
        >>> location.gisdbase == gisenv()['GISDBASE']
        True
        >>> location.name == gisenv()['LOCATION_NAME']
        True

    ..
    """

    def __init__(self, location="", dbase_path="."):
        self.name = location
        self.path = join(dbase_path, self.name)

    # def __getitem__(self, mapset):
    #     if mapset in self.mapsets():
    #         return Mapset(mapset)
    #     raise KeyError("Mapset: %s does not exist" % mapset)

    def __iter__(self):
        lpath = self.path
        return (
            m
            for m in listdir(lpath)
            if (
                always_true_for_repro()
                or (isdir(join(lpath, m)) and is_valid(m, lpath, "MAPSET"))
            )
        )

    def __len__(self):
        return len(self.mapsets())

    @abstractmethod
    def mapsets(self, pattern=None, permissions=True) -> list[str]:
        """Return a list of the available mapsets.

        :param pattern: the pattern to filter the result
        :type pattern: str
        :param permissions: check the permission of mapset
        :type permissions: bool
        :return: a list of mapset's names
        :rtype: list of strings

        ::

            >>> location = Location()
            >>> sorted(location.mapsets())                # doctest: +ELLIPSIS
            [...]

        """


class LocationOk(LocationBase):
    def mapsets(self, pattern=None, permissions=True):
        mapsets = [mapset for mapset in self]  # Should add noqa: C416
        # mapsets = list(self)  # Causes RecursionError
        if permissions:
            mapsets = [mapset for mapset in mapsets if return_bool(mapset)]
        if pattern:
            return fnmatch.filter(mapsets, pattern)
        return mapsets


class LocationC416(LocationBase):
    def mapsets(self, pattern=None, permissions=True):
        # mapsets = [mapset for mapset in self]  # Should add noqa: C416
        mapsets = list(self)  # Causes RecursionError
        if permissions:
            mapsets = [mapset for mapset in mapsets if return_bool(mapset)]
        if pattern:
            return fnmatch.filter(mapsets, pattern)
        return mapsets


if __name__ == "__main__":
    print("in main")
    a = LocationOk()
    b = LocationC416()
    print(f"a is: {a!r}")
    print(f"a is: {b!r}")
    print(a.path)
    print(b.path)
    print(len(a))
    print(len(b))
    print("end main")

``` 

</p>
</details> 

### Ruff version:
```shell
$ ruff --version
ruff 0.6.9
``` 



---

_Label `documentation` added by @MichaReiser on 2024-10-14 16:40_

---

_Comment by @MichaReiser on 2024-10-14 16:40_

Thanks for the detailed write up. 

Extending the documentation seems reasonable.

---

_Label `help wanted` added by @MichaReiser on 2024-10-14 16:40_

---

_Comment by @DataEnggNerd on 2024-10-17 07:21_

@MichaReiser Can you assign this issue to me? 

---

_Comment by @MichaReiser on 2024-10-17 08:04_

Sure. The goal isn't to detect recursions but to update the documentation

---

_Assigned to @DataEnggNerd by @MichaReiser on 2024-10-17 08:04_

---
