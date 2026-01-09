---
number: 389
title: Implement flake8-bugbear
type: issue
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2022-10-10T14:28:32Z
updated_at: 2023-04-30T02:23:35Z
url: https://github.com/astral-sh/ruff/issues/389
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement flake8-bugbear

---

_Issue opened by @charliermarsh on 2022-10-10 14:28_

This is a big set of rules but `flake8-bugbear` is extremely popular (~1.5M downloads per month).


---

_Label `rule` added by @charliermarsh on 2022-10-10 14:28_

---

_Referenced in [astral-sh/ruff#390](../../astral-sh/ruff/pulls/390.md) on 2022-10-10 14:55_

---

_Referenced in [astral-sh/ruff#391](../../astral-sh/ruff/pulls/391.md) on 2022-10-10 16:18_

---

_Referenced in [astral-sh/ruff#392](../../astral-sh/ruff/pulls/392.md) on 2022-10-10 16:53_

---

_Referenced in [astral-sh/ruff#300](../../astral-sh/ruff/issues/300.md) on 2022-10-18 21:09_

---

_Comment by @tgross35 on 2022-10-25 21:35_

Link to bugbear warnings for reference: https://github.com/PyCQA/flake8-bugbear#list-of-warnings

B001, B002, B006, B007, B008, B017, B023, and B904 are the most useful for catching real bugs imho, out of the warnings that are not yet implemented.

B950 covers line length - it is not really necessary since `ruff` is already designed to work with `black`. However, it may be good thing to mention somewhere since [black mentions configuring it explicitly](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#line-length).

(awesome project by the way!)

---

_Comment by @charliermarsh on 2022-10-25 22:44_

Cool, those are all pretty straightforward! Will prioritize them over the others.

---

_Referenced in [astral-sh/ruff#467](../../astral-sh/ruff/pulls/467.md) on 2022-10-26 00:54_

---

_Referenced in [astral-sh/ruff#468](../../astral-sh/ruff/pulls/468.md) on 2022-10-26 01:10_

---

_Referenced in [astral-sh/ruff#473](../../astral-sh/ruff/pulls/473.md) on 2022-10-26 16:01_

---

_Referenced in [astral-sh/ruff#503](../../astral-sh/ruff/pulls/503.md) on 2022-10-29 11:53_

---

_Comment by @harupy on 2022-10-30 01:31_

I'm working on B006 implementation.

---

_Referenced in [astral-sh/ruff#515](../../astral-sh/ruff/pulls/515.md) on 2022-10-30 02:46_

---

_Referenced in [astral-sh/ruff#582](../../astral-sh/ruff/pulls/582.md) on 2022-11-04 14:48_

---

_Referenced in [astral-sh/ruff#587](../../astral-sh/ruff/pulls/587.md) on 2022-11-04 16:25_

---

_Comment by @tgross35 on 2022-11-04 20:03_

Quick checklist for convenience:

- [x] B001: bare except
- [x] B002: ++ -> +=
- [x] B003: os.environ assigning
- [x] B004: hasattr(x, '\_\_call\_\_') to test if x is callable
- [x] B005: .strip() with multi-character strings
- [x] B006: mutable data structures for argument defaults
- [x] B007: Loop control variable not used within the loop body
- [x] B008: function calls in argument defaults
- [x] B009: getattr(x, 'attr') -> x.attr
- [x] B010: setattr(x, 'attr', val) -> x.attr = val
- [x] B011: `assert False` calls
- [x] B012: break, continue or return inside finally blocks
- [x] B013: length-one tuple literal is redundant
- [x] B014: Redundant exception types in except (Exception, TypeError)
- [x] B015: Pointless comparison
- [x] B016: Cannot raise a literal
- [x] B017: self.assertRaises(Exception)
- [x] B018: Found useless expression
- [x] B019: Use of functools.lru_cache or functools.cache on methods
- [x] B020: Loop control variable overrides iterable it iterates
- [x] B021: f-string used as docstring
- [x] B022: arguments passed to contextlib.suppress
- [x] B023: functions defined inside a loop using variables redefined in the loop
- [x] B024: Abstract base class with no abstract method
- [x] B025: try-except block with duplicate exceptions
- [x] B026: Star-arg unpacking after a keyword argument is strongly discouraged
- [x] B027: Empty method in abstract base class

Opinionated:

- [ ] B901: return x in a generator function
- [x] B902: invalid first argument for method. Use self for instance methods, and cls for class methods
- [ ] B903: Use collections.namedtuple (or typing.NamedTuple) for data classes that only set attributes in an __init__ method
- [x] B904: Within an except clause, raise exceptions with raise ... from err or raise ... from None
- [x] B950: Line too long, generous option

---

_Comment by @charliermarsh on 2022-11-04 20:39_

That's awesome, thank you for collating it!

I'm tentatively marking B001 and B950 as complete, since IIUC B001 is the same as E722, and B950 is close enough to E501 that we may not support it independently (I know they're not exactly the same).


---

_Referenced in [astral-sh/ruff#594](../../astral-sh/ruff/pulls/594.md) on 2022-11-05 05:02_

---

_Referenced in [astral-sh/ruff#595](../../astral-sh/ruff/pulls/595.md) on 2022-11-05 10:04_

---

_Referenced in [astral-sh/ruff#596](../../astral-sh/ruff/pulls/596.md) on 2022-11-05 11:18_

---

_Referenced in [astral-sh/ruff#638](../../astral-sh/ruff/pulls/638.md) on 2022-11-07 14:13_

---

_Comment by @charliermarsh on 2022-11-07 14:26_

Just updated the list as @harupy is making awesome progress here. We're now at 50% coverage.

---

_Referenced in [astral-sh/ruff#643](../../astral-sh/ruff/pulls/643.md) on 2022-11-07 15:03_

---

_Referenced in [astral-sh/ruff#668](../../astral-sh/ruff/pulls/668.md) on 2022-11-10 15:40_

---

_Referenced in [astral-sh/ruff#669](../../astral-sh/ruff/pulls/669.md) on 2022-11-10 16:27_

---

_Referenced in [astral-sh/ruff#683](../../astral-sh/ruff/pulls/683.md) on 2022-11-11 12:59_

---

_Referenced in [astral-sh/ruff#695](../../astral-sh/ruff/pulls/695.md) on 2022-11-12 06:37_

---

_Comment by @grillazz on 2022-11-12 17:40_

@charliermarsh for `B008` is there any simple way to setup `extend-immutable-calls = fastapi.Depends, fastapi.params.Depends` ?


---

_Comment by @edgarrmondragon on 2022-11-12 19:53_

@grillazz It's rather straightforward to add. ~I'm working on it~. PR is https://github.com/charliermarsh/ruff/pull/706 🙂


---

_Referenced in [astral-sh/ruff#706](../../astral-sh/ruff/pulls/706.md) on 2022-11-12 20:06_

---

_Comment by @charliermarsh on 2022-11-12 21:48_

@grillazz - This is now supported thanks to @edgarrmondragon. So now you can add this to your `pyproject.toml`:

```toml
[tool.ruff.flake8-bugbear]
extend-immutable-calls = ["fastapi.Depends", "fastapi.Query"]
```

(Going out in v0.0.115 which is building now.)


---

_Referenced in [astral-sh/ruff#718](../../astral-sh/ruff/pulls/718.md) on 2022-11-13 14:11_

---

_Referenced in [astral-sh/ruff#719](../../astral-sh/ruff/pulls/719.md) on 2022-11-13 14:42_

---

_Comment by @grillazz on 2022-11-14 08:35_

@charliermarsh and @edgarrmondragon thx.
I testing and have question. In version `0.0.114` when I spec `select = ["E", "F", "U", "N", "C", "B"]` i get `B008 Do not perform function calls in argument defaults.`

Next I adding `ignore = ["B008",]` and lint is passing.

After upgrade to `0.0.117` lint is passing without explicitly specifying 
```toml
[tool.ruff.flake8-bugbear]
extend-immutable-calls = ["fastapi.Depends", "fastapi.Query"]
```
Ideas ?

ps. Also I can't spec ruff = "0.0.115" only 116 and 117 working

---

_Referenced in [astral-sh/ruff#734](../../astral-sh/ruff/pulls/734.md) on 2022-11-14 10:53_

---

_Comment by @harupy on 2022-11-14 11:03_

@grillazz 

> After upgrade to 0.0.117 lint is passing without explicitly specifying

Did you remove `ignore = ["B008",]` after upgrading to 0.0.117?

---

_Comment by @grillazz on 2022-11-14 11:24_

@harupy yes I removed this line. Please find full `pyproject.toml` below

```toml
[tool.poetry]
name = "fastapi-sqlalchemy-asyncpg"
version = "0.0.2"
description = ""
authors = ["Jakub Miazek <the@grillazz.com>"]
packages = []
license = "MIT"

[tool.poetry.dependencies]
python = "^3.10"
alembic = "*"
asyncpg = "*"
httpx = "*"
pydantic = "*"
sqlalchemy = "*"
fastapi = "*"
uvicorn = { extras = ["standard"], version = "*" }
uvloop = "*"
httptools = "*"
rich = "*"
devtools = { extras = ["pygments"], version = "*" }
black = "*"
safety = "*"
pyupgrade = "*"
ipython = "*"
pytest-cov = "*"
pytest-asyncio = "*"
ruff = "*"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.ruff]
line-length = 120

select = ["E", "F", "U", "N", "C", "B"]

# Exclude a variety of commonly ignored directories.
exclude = ["alembic",]
# Assume Python 3.10.
target-version = "py310"

[tool.ruff.flake8-quotes]
docstring-quotes = "double"

[tool.pytest.ini_options]
addopts = "-v --doctest-modules --doctest-glob=*.md --ignore=alembic"
asyncio_mode = "strict"
env_files = [".env"]
```


---

_Referenced in [astral-sh/ruff#738](../../astral-sh/ruff/pulls/738.md) on 2022-11-14 15:56_

---

_Comment by @harupy on 2022-11-14 16:14_

@grillazz Can you try running `ruff --no-cache ...`?

---

_Comment by @edgarrmondragon on 2022-11-14 16:22_

@grillazz +1 to the above suggestion. This is the result I get from running on https://github.com/grillazz/fastapi-sqlalchemy-asyncpg's `main` branch with the configuration you shared:

```console
$ ruff .
Found 10 error(s).
app/api/nonsense.py:12:79: B008 Do not perform function call `Depends` in argument defaults
app/api/nonsense.py:21:32: B008 Do not perform function call `Depends` in argument defaults
app/api/nonsense.py:27:65: B008 Do not perform function call `Depends` in argument defaults
app/api/nonsense.py:36:32: B008 Do not perform function call `Depends` in argument defaults
app/api/shakespeare.py:15:32: B008 Do not perform function call `Depends` in argument defaults
app/api/stuff.py:16:85: B008 Do not perform function call `Depends` in argument defaults
app/api/stuff.py:30:73: B008 Do not perform function call `Depends` in argument defaults
app/api/stuff.py:39:32: B008 Do not perform function call `Depends` in argument defaults
app/api/stuff.py:45:62: B008 Do not perform function call `Depends` in argument defaults
app/api/stuff.py:54:32: B008 Do not perform function call `Depends` in argument defaults
```

---

_Comment by @charliermarsh on 2022-11-14 16:23_

I may have forgotten to include the bugbear settings in the hash key. (I’m on the subway but can fix in a bit, or if someone wants to PR, check out the hash trait implementation on Settings :))

---

_Referenced in [astral-sh/ruff#739](../../astral-sh/ruff/pulls/739.md) on 2022-11-14 17:29_

---

_Comment by @charliermarsh on 2022-11-14 17:29_

If that _is_ the issue, it'll be fixed by https://github.com/charliermarsh/ruff/pull/739.

---

_Comment by @grillazz on 2022-11-15 07:41_

Test 0.0.120 and problem solved. Thx

---

_Referenced in [astral-sh/ruff#753](../../astral-sh/ruff/pulls/753.md) on 2022-11-15 14:40_

---

_Comment by @charliermarsh on 2022-11-15 21:18_

@harupy is dangerously close to implementing the entire plugin ;)

---

_Comment by @harupy on 2022-11-16 00:28_

Nice to see a task list with lots of ✅ :)

---

_Referenced in [astral-sh/ruff#823](../../astral-sh/ruff/pulls/823.md) on 2022-11-20 08:06_

---

_Referenced in [astral-sh/ruff#824](../../astral-sh/ruff/pulls/824.md) on 2022-11-20 08:47_

---

_Comment by @charliermarsh on 2022-11-22 22:06_

I think if we implement B023, we should feel comfortable closing this.

---

_Referenced in [astral-sh/ruff#892](../../astral-sh/ruff/pulls/892.md) on 2022-11-23 14:04_

---

_Comment by @harupy on 2022-11-24 05:20_

> I think if we implement B023, we should feel comfortable closing this.

Agreed! Feel free to implement it if anyone wants to :)


---

_Comment by @charliermarsh on 2022-11-25 04:41_

@harupy - Sounds good, I can give it a try!

---

_Comment by @harupy on 2022-11-25 04:41_

Awesome, thank you!

---

_Comment by @harupy on 2022-11-25 04:43_

Is `B902` covered by `N804` and `N805` in pep8-namiing?

---

_Comment by @charliermarsh on 2022-11-25 04:44_

@harupy - Yeah I believe so.

---

_Referenced in [astral-sh/ruff#907](../../astral-sh/ruff/pulls/907.md) on 2022-11-25 23:29_

---

_Closed by @charliermarsh on 2022-11-25 23:30_

---

_Comment by @g-as on 2022-12-07 09:52_

B905 was just [added](https://github.com/PyCQA/flake8-bugbear/releases/tag/22.12.6):
> B905: zip() without an explicit strict= parameter set. strict=True causes the resulting iterator to raise a ValueError if the arguments are exhausted at differing lengths. The strict= argument was added in Python 3.10, so don't enable this flag for code that should work on <3.10. For more information: https://peps.python.org/pep-0618/

---

_Referenced in [astral-sh/ruff#1120](../../astral-sh/ruff/issues/1120.md) on 2022-12-07 14:25_

---

_Comment by @charliermarsh on 2022-12-07 15:01_

@g-as -- Added in https://github.com/charliermarsh/ruff/pull/1122.

---

_Comment by @SRv6d on 2023-01-15 15:02_

@charliermarsh Are there still plans on supporting `B950` ? Enforcing a line length while at the same time giving some headroom is often a pretty good compromise between sane line length and unnecessarily ugly code resulting from a hard limit.

---

_Comment by @charliermarsh on 2023-01-15 19:25_

We _could_ support it. I was a bit hesitant but we could. Can you talk me through the rationale of using B950 vs. just configuring E501 to use a 10% increase? I think they're exactly equivalent, so I'm guessing it's more of a philosophical thing?

---

_Comment by @SRv6d on 2023-01-15 19:41_

I wouldn't call it philosophical, it mostly comes down to personal preference and experience. I think a line length of `88` is a great middle ground, but personally find my code to often be right around that or slightly over. Reformatting those lines of code just because they are a couple of characters over the limit often looks silly and increasing the limit by 10% we are getting close to `100` which results in an entirely different looking codebase just for a couple of outliers.

---

_Comment by @charliermarsh on 2023-01-15 20:01_

Sorry -- didn't intend "philosophical" to come off as pejorative so apologies if it did. Was mostly trying to confirm that, functionally, using `B950` with `line-length` of `88` is identical to using `E501` with a `line-length` of `96.8`, right?

Assuming that's correct, is the difference here that if you treat `88` as the line length (when in reality, the enforced limit is `96.8`), then folks set that as the line-length in their editors, tend to format around it, Black gets configured with `88`, and so on?

---

_Comment by @SRv6d on 2023-01-15 20:03_

No offense taken whatsoever. The latter is exactly what I meant to say. 

---

_Comment by @SRv6d on 2023-01-27 22:54_

@charliermarsh Would you be open to implementing `B950` ? It's the only thing missing for me coming from flake8. I could give it a try, but I have no rust experience (yet?).

---

_Referenced in [materialsproject/atomate2#250](../../materialsproject/atomate2/pulls/250.md) on 2023-02-23 16:34_

---

_Comment by @jamesbraza on 2023-04-30 02:22_

@SRv6d to make [`E501`](https://beta.ruff.rs/docs/rules/line-too-long/) behave like [`B950`](https://github.com/PyCQA/flake8-bugbear#opinionated-warnings) I am just using this:

```toml
[tool.ruff]

# Line length to use when enforcing long-lines violations (like `E501`).
line-length = 97  # ceil(1.1 * 88) makes `E501` equivalent to `B950`
```

---

_Referenced in [xpublish-community/xpublish-opendap#5](../../xpublish-community/xpublish-opendap/pulls/5.md) on 2023-04-30 15:04_

---

_Referenced in [astral-sh/ruff#4429](../../astral-sh/ruff/issues/4429.md) on 2023-05-14 11:22_

---

_Referenced in [opentargets/gentropy#154](../../opentargets/gentropy/pulls/154.md) on 2023-10-03 11:54_

---

_Referenced in [VOICEVOX/voicevox_engine#1563](../../VOICEVOX/voicevox_engine/pulls/1563.md) on 2025-03-22 01:14_

---

_Referenced in [astral-sh/ruff#17439](../../astral-sh/ruff/issues/17439.md) on 2025-04-17 01:30_

---
