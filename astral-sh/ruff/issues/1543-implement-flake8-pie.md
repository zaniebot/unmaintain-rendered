---
number: 1543
title: Implement flake8-pie
type: issue
state: closed
author: sbdchd
labels:
  - good first issue
  - plugin
assignees: []
created_at: 2023-01-02T01:25:26Z
updated_at: 2023-02-07T02:22:34Z
url: https://github.com/astral-sh/ruff/issues/1543
synced_at: 2026-01-10T01:22:39Z
---

# Implement flake8-pie

---

_Issue opened by @sbdchd on 2023-01-02 01:25_

There might be some overlap with these rules and pylint / flake8-simplify, but I think I eliminated most of the dupes / unnecessary rules:


- [x] PIE810: [single-starts-ends-with](https://github.com/sbdchd/flake8-pie#pie810-single-starts-ends-with)
Instead of calling startswith or endswith on the same string for multiple prefixes, pass the prefixes as a tuple in a single startswith or endswith call.

```python
# error
foo.startswith("foo") or foo.startswith("bar")

# error
foo.endswith("foo") or foo.endswith("bar")

# error
foo.startswith("foo") or str.startswith(foo, "bar")

# ok
foo.startswith(("foo",  "bar"))

# ok
foo.endswith(("foo",  "bar"))

# ok
foo.startswith("foo") or foo.endswith("bar")
```


- [x] PIE790: [no-unnecessary-pass](https://github.com/sbdchd/flake8-pie#pie790-no-unnecessary-pass)

`pass` is unnecessary when definining a class or function with an empty body.
```python
# error
class BadError(Exception):
    """
    some doc comment
    """
    pass

def foo() -> None:
    """
    some function
    """
    pass

# ok
class BadError(Exception):
    """
    some doc comment
    """

def foo() -> None:
    """
    some function
    """
```

- [x] PIE794: [dupe-class-field-definitions](https://github.com/sbdchd/flake8-pie#pie794-dupe-class-field-definitions)

Finds duplicate definitions for the same field, which can occur in large ORM model definitions.

```python
# error
class User(BaseModel):
    email = fields.EmailField()
    # ...80 more properties...
    email = fields.EmailField()

# ok
class User(BaseModel):
    email = fields.EmailField()
    # ...80 more properties...
```

- [x] [PIE796: prefer-unique-enums](https://github.com/sbdchd/flake8-pie#pie796-prefer-unique-enums)

By default the stdlib enum allows multiple field names to map to the same value, this lint requires each enum value be unique.

```python
# error
class Foo(enum.Enum):
    A = "A"
    B = "B"
    C = "C"
    D = "C"

# ok
class Foo(enum.Enum):
    A = "A"
    B = "B"
    C = "C"
    D = "D"
```

- [x] [PIE800: no-unnecessary-spread](https://github.com/sbdchd/flake8-pie#pie800-no-unnecessary-spread)

Check for unnecessary dict unpacking.

```python
# error
{**foo, **{"bar": 10}}

# ok
{**foo, "bar": 10}
```

- [x] [PIE804: no-unnecessary-dict-kwargs](https://github.com/sbdchd/flake8-pie#pie804-no-unnecessary-dict-kwargs)

As long as the keys of the dict are valid Python identifier names, we can safely remove the surrounding dict.

```python
# error
foo(**{"bar": True})

# ok
foo(bar=True)
foo(**buzz)
foo(**{"bar foo": True})
```


- [x] [PIE807: prefer-list-builtin](https://github.com/sbdchd/flake8-pie#pie807-prefer-list-builtin)

`lambda: []` is equivalent to the builtin list

```python
# error
@dataclass
class Foo:
    foo: List[str] = field(default_factory=lambda: [])

# ok
@dataclass
class Foo:
    foo: List[str] = field(default_factory=list)
```


Do these all sound good / are there any I should leave out? I skipped a few different ones from the original package that were too specific / awkward.

Also skipped rules that overlapped with:


- https://github.com/charliermarsh/ruff/issues/970
- https://github.com/charliermarsh/ruff/issues/998

---

_Comment by @charliermarsh on 2023-01-02 02:23_

Yeah these are all quite nice. I can try to implement one of them tomorrow to lay the groundwork for the plugin and we can run from there.

---

_Comment by @charliermarsh on 2023-01-02 02:23_

(Thanks for this thoughtful collation.)

---

_Label `plugin` added by @charliermarsh on 2023-01-02 02:23_

---

_Comment by @charliermarsh on 2023-01-02 19:54_

I didn't realize you actually wrote that plugin, very cool.

---

_Referenced in [astral-sh/ruff#1578](../../astral-sh/ruff/pulls/1578.md) on 2023-01-03 02:37_

---

_Referenced in [astral-sh/ruff#1580](../../astral-sh/ruff/pulls/1580.md) on 2023-01-03 03:08_

---

_Referenced in [astral-sh/ruff#1581](../../astral-sh/ruff/pulls/1581.md) on 2023-01-03 03:25_

---

_Label `good first issue` added by @charliermarsh on 2023-01-12 03:34_

---

_Renamed from "Misc rules from flake8-pie" to "Implement flake8-pie" by @charliermarsh on 2023-01-14 04:07_

---

_Referenced in [astral-sh/ruff#1881](../../astral-sh/ruff/pulls/1881.md) on 2023-01-15 03:07_

---

_Referenced in [astral-sh/ruff#1884](../../astral-sh/ruff/pulls/1884.md) on 2023-01-15 04:25_

---

_Referenced in [astral-sh/ruff#1922](../../astral-sh/ruff/pulls/1922.md) on 2023-01-16 21:12_

---

_Comment by @ljesparis on 2023-01-16 21:47_

I was passing by and was wondering if can i write some of these ? I'm actually learning rust, but, these plugins look easy enough.

I actually got around to developing the **prefer_unique_enums rule** last night 😅  and i made a mistake with a PR so i closed it 😓 

---

_Referenced in [astral-sh/ruff#1923](../../astral-sh/ruff/pulls/1923.md) on 2023-01-16 21:48_

---

_Comment by @charliermarsh on 2023-01-16 22:02_

@ljesparis - Yes please, thanks!

---

_Comment by @charliermarsh on 2023-01-23 02:53_

@sbdchd - Are you planning to do PIE810 to close this one out? Just trying to keep tabs on who's working on what. Thanks!

---

_Comment by @sbdchd on 2023-01-23 19:10_

Yeah I can do that one as well

---

_Comment by @charliermarsh on 2023-02-06 19:08_

Gonna close this for now, can always do that final rule if anyone wants to grab it.

---

_Closed by @charliermarsh on 2023-02-06 19:08_

---

_Referenced in [astral-sh/ruff#2616](../../astral-sh/ruff/pulls/2616.md) on 2023-02-07 01:32_

---
