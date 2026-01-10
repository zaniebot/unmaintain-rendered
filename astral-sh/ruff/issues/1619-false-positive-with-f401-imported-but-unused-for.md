```yaml
number: 1619
title: False positive with F401 (imported but unused) for type comments
type: issue
state: open
author: sbdchd
labels:
  - core
assignees: []
created_at: 2023-01-04T02:49:40Z
updated_at: 2024-08-20T10:09:05Z
url: https://github.com/astral-sh/ruff/issues/1619
synced_at: 2026-01-10T11:09:43Z
```

# False positive with F401 (imported but unused) for type comments

---

_Issue opened by @sbdchd on 2023-01-04 02:49_

minimal example ([of some project code](https://github.com/chdsbd/kodiak/blob/0c1171bc8f574d7ee11ab6174036edadfbc5ec09/bot/kodiak/pull_request.py#L76-L76)):
```python
from typing import List

x = [] # type: List[str]
x.append("foo")
print(x)
```

This isn't a big deal since `# type: List[str]` can be replaced with `# type: list[str]` and mypy warns about `List` being undefined with the auto fixer removes the import

---

_Comment by @charliermarsh on 2023-01-04 02:50_

Is this in reference to the type comment? Like, that `# type: List[str]` should be considered a valid reference to `List`? Just verifying.

---

_Comment by @sbdchd on 2023-01-04 02:50_

Yeah exactly! mypy will complain that it isn't defined if the import is missing, i.e.

```
kodiak/pull_request.py:76:5: error: Name "List" is not defined  [name-defined]
kodiak/pull_request.py:76:5: note: Did you forget to import it from "typing"? (Suggestion: "from typing import List")
```

---

_Label `core` added by @charliermarsh on 2023-01-04 16:04_

---

_Comment by @charliermarsh on 2023-01-04 16:06_

Yeah we don't really support type comments right now, since they were made obsolete in Python 3.6 AFAIK. I know some projects still use them for compatibility... I'll think on it.

---

_Comment by @phillipuniverse on 2023-01-27 17:38_

@charliermarsh maybe 1 area that this is still useful in Python 3.6+: type hinting the result of a context manager. Example:

```python
from types_aiobotocore_s3 import S3ServiceResource

async with self.boto_session.resource("s3") as s3:  # type: S3ServiceResource
    obj = await s3.Object(self.bucket_name, key)
    return (await obj.get())["Body"]
```

The `from types_aiobotocore_s3 import S3ServiceResource` part is removed by ruff.

Looks like leaving it off or on is ok by Mypy, but type hinting at least in Pycharm is better with the type comment.

---

_Comment by @spaceone on 2023-02-06 11:17_

Maybe the issue title can be change to `False positive with F401 (imported but unused) with mypy type comments` ? can faster be found when searching for this issue.

Affects me as well.

---

_Comment by @jaymegordo on 2023-04-21 18:12_

+1 would also be very helpful to be able to use type comments. IMO they're much nicer than normal type annotations in a lot of places, because they can "hide" as a comment out of site of the actual line of code

<img width="764" alt="image" src="https://user-images.githubusercontent.com/29611875/233706960-a3ac763e-9e9b-4d67-83a6-80f1557b2537.png">


---

_Comment by @uranusjr on 2023-04-24 22:02_

I’d say the comment looking nicer than a type annotation is an editor/highlighter problem, but the general point of this being useful is correct.

---

_Comment by @johnthagen on 2023-05-01 16:05_

Similar to https://github.com/charliermarsh/ruff/issues/1619#issuecomment-1406845028, I still use type comments in some very rare cases where things like `for` loops don't have syntax to express a "real" Python 3 type hint and the type checkers can't infer the type due to limitations in the underlying libraries being used.

```py
from models import Wheel

for wheel in car.wheels.all():  # type: Wheel
    ...
```

I realize this is non-standard, but PyCharm will interpret this and provide intellisense/auto-complete for the loop variable, which is practically useful in these niche cases.

---

_Renamed from "False positive with F401 (imported but unused)" to "False positive with F401 (imported but unused) for type comments" by @charliermarsh on 2023-05-20 18:38_

---

_Comment by @JonathanPlasse on 2023-05-22 08:43_

All type comments can be replaced (c.f.: https://peps.python.org/pep-0526/#where-annotations-aren-t-allowed)
```python
from types_aiobotocore_s3 import S3ServiceResource

async with self.boto_session.resource("s3") as s3:  # type: S3ServiceResource
    obj = await s3.Object(self.bucket_name, key)
    return (await obj.get())["Body"]

# Becomes
s3: S3ServiceResource
async with self.boto_session.resource("s3") as s3:
    obj = await s3.Object(self.bucket_name, key)
    return (await obj.get())["Body"]
```
```python
from models import Wheel

for wheel in car.wheels.all():  # type: Wheel
    ...
# Becomes
wheel: Wheel
for wheel in car.wheels.all():
    ...
```

---

_Comment by @johnthagen on 2023-05-22 11:02_

@JonathanPlasse Good point bringing that up. I've generally been a little resistant to this particular fix because it complicates the actual code just to add the hint:

- It creates a new unbound local (if the code gets refactored later it could open up the possibility that `s3: S3ServiceResource` gets "separated" from the loop accidentally)
- Python leaks loop variables into the outer scope already, but the new `s3: S3ServiceResource` could be misunderstood as _implying intent_ to leak the iterator
- There is a DRY issue where if the loop variable `s3` is renamed but the outer typed variable isn't

But you're right that this is the officially prescribed way of dealing with it, but hopefully the above points help explain why people may be hesitant to do it just for a hint. 

---

_Comment by @V3RGANz on 2023-05-24 11:56_

Also, it's worth mentioning that if you unpack values in this manner:
```python
n: str
p: Tensor
r: Tensor
f1: Tensor
for n, p, r, f1 in zip(names, precision, recall, f1_micro):
    ...
```
it seems unnecessarily complicated for such a simple functionality compared to a simpler approach like:
```python
for n, p, r, f1 in zip(names, precision, recall, f1_micro):  # type: str, Tensor, Tensor, Tensor
    ...
```

---

_Comment by @jaraco on 2023-07-08 03:35_

I stumbled onto this issue in the configparser project. I worked around it by converting the type comments to native type hints (jaraco/configparser@4990ba132450e218d9695ba74d4a139913459b17).

---

_Comment by @vors on 2023-07-24 18:36_

> Yeah we don't really support type comments right now, since they were made obsolete in Python 3.6 AFAIK. I know some projects still use them for compatibility... I'll think on it.

They are not obsolete, they are part of PEP 484 specification and are supported by all major type checkers (pyright, mypy, pyre, etc)
https://peps.python.org/pep-0484/#type-comments

There are a ton of projects that leverage them, i.e. numpy.

---

_Comment by @charliermarsh on 2023-07-24 18:48_

To be clear, "obsolete" is different from "unused". I'm not claiming that type comments aren't used in existing projects or that type checkers do not support them. It would be nice to support them! But it's a matter of prioritization, and my understanding is that [PEP 526](https://peps.python.org/pep-0526/) had the explicit goal of introducing "a more readable syntax to replace type comments."

Perhaps another way of framing my lack of urgency to prioritize this: is there any reason to use type comments when writing Python code _today_? (This is not a rhetorical question, I am genuinely asking.)


---

_Comment by @jaymegordo on 2023-07-24 18:51_

@charliermarsh personally I use them exclusively, (see my previous comment https://github.com/astral-sh/ruff/issues/1619#issuecomment-1518178907), purely because I think they make code much more readable. But thats just my 2c 🤷🏼‍♂️

---

_Comment by @johnthagen on 2023-07-25 19:01_

@charliermarsh 

> is there any reason to use type comments when writing Python code today?

As explained in the link below, for the rare cases of needing to type hint loop variables or `with` block variables, I find type comments avoid several drawbacks:

- https://github.com/astral-sh/ruff/issues/1619#issuecomment-1557012758

---

_Comment by @vors on 2023-07-26 02:14_

> is there any reason to use type comments when writing Python code today?

The company where I work has multi-million line codebase and we just migrated to using `ruff` as THE linter. (thank you for the great project!)

It's not possible to migrate the whole codebase in a one go to the py3 comments unfortunately (there are technical reasons why we cannot do that).

---

_Comment by @charliermarsh on 2023-07-26 02:21_

(Thank you, these comments are all welcome and helpful.)

---

_Comment by @vors on 2023-09-15 01:21_

FWIW flake8 has the same problem (tested on flake8 6.1.0)

---

_Comment by @johnthagen on 2023-09-15 01:40_

pyflakes (what Flake8 runs under the hood) used to support this (I used the feature for years before moving to Ruff). Perhaps there was a regression.

- https://github.com/PyCQA/pyflakes/pull/400

---

_Comment by @charliermarsh on 2023-09-15 01:47_

It looks like it was removed about a year ago: https://github.com/PyCQA/pyflakes/pull/684

---

_Comment by @ndevenish on 2024-08-20 10:09_

We have some scripts that we explicitly maintain compatibility with python 2 (generally bootstrapping-related scripts that people can run on python2 and take care of bootstrapping a full environment).

This is _very mildly_ annoying but easily fixed with a `noqa: F401` on the import, and is a narrow enough use case that this causes no pain.

---
