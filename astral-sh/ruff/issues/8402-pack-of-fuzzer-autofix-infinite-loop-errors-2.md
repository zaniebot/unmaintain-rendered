```yaml
number: 8402
title: "Pack of fuzzer Autofix/Infinite loop errors [2]"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-11-01T11:10:35Z
updated_at: 2024-07-29T13:05:06Z
url: https://github.com/astral-sh/ruff/issues/8402
synced_at: 2026-01-12T15:54:48Z
```

# Pack of fuzzer Autofix/Infinite loop errors [2]

---

_@qarmin_

Previous part(https://github.com/astral-sh/ruff/issues/7455) exceeded 50 reported issues, and is hard to search/add new problems, so decided to create new list

- [x] - D300 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788782750
- [x] - E223 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788784190
- [x] - D301 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788783287
- [x] - FURB171 -https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788784721
- [x] - PIE804 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788785766
- [x] - UP014 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788787357
- [x] - PLR1706 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810809305
- [x] - PD002 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810813194
- [ ] - FURB171 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810816035
- [ ] - E223 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810818804
- [ ] - RUF015 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810827697
- [x] - C405 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810829481
- [x] - TRIO115 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810838129
- [x] - RUF022 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916203707
- [x] - RUF023 -  https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916213693
- [x] - SIM114 -  https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916215124
- [ ] - PLR1706 -  https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916218206
- [x] - RET505 -  https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916223126
- [ ] - C413 -  https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916224737
- [ ] - RET505 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1956261091
- [x] - PLR0203 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-1956263418
- [ ] - UP032 - https://github.com/astral-sh/ruff/issues/8402#issuecomment-2041178222

Not all errors are reported here, but more can errors with nice format can be obtained:
- directly on site - https://github.com/qarmin/Automated-Fuzzer/actions/workflows/ruff_all.yml - this runs automatically every day, so bugs should be very fresh - just open latest completed job and download all artifacts that starts from `reports-` - inside, should be grouped available file that causes problems with info which rule is needed to trigger them

06.04.2024 results - 11673 broken files - 
[tmp_folder.7z.zip](https://github.com/astral-sh/ruff/files/14894810/tmp_folder.7z.zip)


Rules that causes problems(for some files, more than one rule needs to be enabled)


```
+--------------+----------------+
| Rule         | Number         |
+--------------+----------------+
| RET505       | 7730           |
| RET503       | 6865           |
| UP032        | 1720           |
| PD002        | 345            |
| PLC2801      | 280            |
| C403         | 253            |
| F841         | 241            |
| COM812       | 205            |
| UP037        | 201            |
| PIE800       | 176            |
| PLW0120      | 153            |
| UP036        | 143            |
| FA102        | 133            |
| RET506       | 107            |
| E502         | 70             |
| EM101        | 67             |
| FURB152      | 63             |
| D202         | 55             |
| UP028        | 43             |
| T201         | 42             |
| F401         | 38             |
| E251         | 34             |
| FURB129      | 33             |
| PLR5501      | 25             |
| W291         | 25             |
| EM102        | 19             |
| I001         | 17             |
| SIM108       | 17             |
| E225         | 16             |
| PT006        | 15             |
| E203         | 14             |
| RET507       | 14             |
| UP031        | 14             |
| W293         | 14             |
| E275         | 13             |
| UP007        | 12             |
| D211         | 10             |
| N805         | 10             |
| PLR1722      | 10             |
| RET508       | 10             |
| RUF010       | 10             |
| EM103        | 8              |
| PT027        | 8              |
| E262         | 7              |
| C417         | 6              |
| PLR1736      | 6              |
| PLR2044      | 6              |
| E713         | 5              |
| RUF013       | 5              |
| TCH002       | 5              |
| UP009        | 5              |
| PIE804       | 4              |
| Q002         | 4              |
| SIM222       | 4              |
| D201         | 3              |
| E201         | 3              |
| FURB140      | 3              |
| RET504       | 3              |
| UP018        | 3              |
| UP025        | 3              |
| B006         | 2              |
| C408         | 2              |
| C418         | 2              |
| D415         | 2              |
| E271         | 2              |
| INT001       | 2              |
| N806         | 2              |
| PLR0206      | 2              |
| RUF100       | 2              |
| SIM102       | 2              |
| W292         | 2              |
| ANN201       | 1              |
| ANN202       | 1              |
| ARG001       | 1              |
| ARG003       | 1              |
| B009         | 1              |
| B013         | 1              |
| B014         | 1              |
| B017         | 1              |
| B026         | 1              |
| B905         | 1              |
| C401         | 1              |
| C410         | 1              |
| C416         | 1              |
| D200         | 1              |
| D203         | 1              |
| D408         | 1              |
| D418         | 1              |
| DTZ004       | 1              |
| E202         | 1              |
| E226         | 1              |
| E231         | 1              |
| E302         | 1              |
| E303         | 1              |
| E401         | 1              |
| E402         | 1              |
| E711         | 1              |
| E714         | 1              |
| E731         | 1              |
| F502         | 1              |
| F523         | 1              |
| F541         | 1              |
| F704         | 1              |
| F722         | 1              |
| F901         | 1              |
| FURB101      | 1              |
| FURB110      | 1              |
| FURB161      | 1              |
| FURB168      | 1              |
| FURB169      | 1              |
| FURB180      | 1              |
| ICN002       | 1              |
| N804         | 1              |
| N807         | 1              |
| N818         | 1              |
| NPY003       | 1              |
| PD003        | 1              |
| PD004        | 1              |
| PIE810       | 1              |
| PLC0131      | 1              |
| PLC0208      | 1              |
| PLC0415      | 1              |
| PLC3002      | 1              |
| PLE0115      | 1              |
| PLE0117      | 1              |
| PLE1142      | 1              |
| PLR0124      | 1              |
| PLR0202      | 1              |
| PLR0913      | 1              |
| PLR1730      | 1              |
| PLR1733      | 1              |
| PLW0406      | 1              |
| PLW1501      | 1              |
| PT004        | 1              |
| PT009        | 1              |
| PT010        | 1              |
| PT020        | 1              |
| PTH108       | 1              |
| PTH113       | 1              |
| PTH201       | 1              |
| PYI006       | 1              |
| PYI029       | 1              |
| PYI053       | 1              |
| Q000         | 1              |
| Q004         | 1              |
| RUF028       | 1              |
| S104         | 1              |
| S304         | 1              |
| S316         | 1              |
| S508         | 1              |
| S605         | 1              |
| S606         | 1              |
| S612         | 1              |
| SIM201       | 1              |
| SIM211       | 1              |
| SIM300       | 1              |
| SIM401       | 1              |
| TCH001       | 1              |
| TCH005       | 1              |
| TID251       | 1              |
| TRY004       | 1              |
| UP012        | 1              |
| UP022        | 1              |
| UP027        | 1              |
| W605         | 1              |
| YTT202       | 1              |
+--------------+----------------+
```

---

_Comment by @qarmin on 2023-11-01 11:12_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select D300 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
def t_BEGIN_STRING(t):
    """('''|\"""|'|")"""
```

error
```
/home/rafal/test/tmp_folder/F_NAME_152486550958779227.py:2:5: D300 Use triple single quotes `'''`
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_152486550958779227.py` with rule codes D300: unexpected character after line continuation character at byte offset 34
---
def t_BEGIN_STRING(t):
    '''('''|\"""|'|")'''
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226273/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-01 11:12_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select D301 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
u'\s$'
```

error
```
/home/rafal/test/tmp_folder/F_NAME_2876580788712488819.py:1:1: D301 Use `r"""` if any backslashes in a docstring
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_2876580788712488819.py` with rule codes D301: invalid syntax. Got unexpected token "\s$" at byte offset 2
---
ru'\s$'
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226277/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-01 11:13_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select E223,E711 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class AutoFocusFrame():
	def __str__(self):
			+ "\t" + 'width: ' + str
```

error
```
/home/rafal/test/tmp_folder/F_NAME_7085034160936872701.py:3:1: E223 Tab before operator
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_7085034160936872701.py` with rule codes E223: unindent does not match any outer indentation level at byte offset 45
---
class AutoFocusFrame():
	def __str__(self):
 + "\t" + 'width: ' + str
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226281/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-01 11:13_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select FURB171 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
'count' in [*records]
```

error
```
/home/rafal/test/tmp_folder/F_NAME_15972918632259983480.py:1:1: FURB171 Membership test against single-item container
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_15972918632259983480.py` with rule codes FURB171: invalid syntax. Got unexpected token '*' at byte offset 11
---
'count' == *records
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226285/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-01 11:14_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select PIE804 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
func_positional_args(1, 2, c=24, d=32, **{'d': 32}) # [repeated-keyword]
```

error
```
/home/rafal/test/tmp_folder/F_NAME_539807517615219474.py:1:1: PIE804 Unnecessary `dict` kwargs
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_539807517615219474.py` with rule codes PIE804: keyword argument repeated: d at byte offset 39
---
func_positional_args(1, 2, c=24, d=32, d=32) # [repeated-keyword]
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226298/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-01 11:16_

Ruff 0.1.3 (latest changes from main branch)
```
ruff  *.py --select UP014 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
from typing import NamedTuple
S3File = NamedTuple(
    "S3File",
    [
        ("dataHPK",* str),
    ],
)
```

error
```
/home/rafal/test/tmp_folder/F_NAME_17639690495213172135.py:2:1: UP014 Convert `S3File` from `NamedTuple` functional to class syntax
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_17639690495213172135.py` with rule codes UP014: invalid syntax. Got unexpected token '*' at byte offset 69
---
from typing import NamedTuple
class S3File(NamedTuple):
    dataHPK: *str
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13226309/python_compressed.zip)


---

_Label `fuzzer` added by @dhruvmanila on 2023-11-01 12:47_

---

_Label `bug` added by @dhruvmanila on 2023-11-01 12:47_

---

_Comment by @qarmin on 2023-11-14 17:48_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select PLR1706 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
PERM = 'view' if (VERSION[0] == 2 and VERSION[1] > 0) or (VERSION[0] > 2) else 'change'
```

error
```
/home/rafal/test/tmp_folder/F_NAME_8080826207718179159.py:1:18: PLR1706 Consider using if-else expression
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_8080826207718179159.py` with rule codes PLR1706: invalid syntax. Got unexpected token 'if' at byte offset 32
---
PERM = 'view' if VERSION[1] > 0 if VERSION[0] == 2 else VERSION[0] > 2 else 'change'
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353537/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:49_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select PD002 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
r().__init__(inplace=True)
```

error
```
/home/rafal/test/tmp_folder/F_NAME_6076913071211757466.py:1:14: PD002 `inplace=True` should be avoided; it has inconsistent behavior
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_6076913071211757466.py` with rule codes PD002: invalid assignment target at byte offset 0
---
r() = r().__init__()
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353568/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:50_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select FURB171 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
t in [ConnectionState.INIT or
                              ConnectionState.DISCONNECTED]
```

error
```
/home/rafal/test/tmp_folder/F_NAME_15087088891922560551.py:1:1: FURB171 Membership test against single-item container
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_15087088891922560551.py` with rule codes FURB171: invalid syntax. Got unexpected token Newline at byte offset 28
---
t == ConnectionState.INIT or
                              ConnectionState.DISCONNECTED
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353564/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:51_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select E223,RUF015 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class ITracker(IAlgorithm):
	def reset(self):
		- """
		"""
```

error
```
/home/rafal/test/tmp_folder/F_NAME_9953448065317350986.py:3:1: E223 Tab before operator
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_9953448065317350986.py` with rule codes E223: unindent does not match any outer indentation level at byte offset 47
---
class ITracker(IAlgorithm):
	def reset(self):
 - """
		"""
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353577/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:54_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select RUF015 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
[x for x in range(10)][0] = 42
```

error
```
/home/rafal/test/tmp_folder/F_NAME_7189916765206520894.py:1:1: RUF015 Prefer `next(iter(range(10)))` over single element slice
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_7189916765206520894.py` with rule codes RUF015: invalid assignment target at byte offset 0
---
next(iter(range(10))) = 42
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353600/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:55_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select C405 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class TestDemand(unittest.TestCase):
        interface_a = Interface(
        )
        interface_b = Interface(
        )
        lsp_a_b = RSVP_LSP(
        )
        model = PerformanceModel(
            interface_objects=set
([interface_a, interface_b]),
        )
```

error
```
/home/rafal/test/tmp_folder/F_NAME_8513460905165022730.py:9:31: C405 Unnecessary `list` literal (rewrite as a `set` literal)
Found 1 error.

error: Failed to create fix for UnnecessaryLiteralSet: Failed to extract expression from source
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353609/python_compressed.zip)


---

_Comment by @qarmin on 2023-11-14 17:59_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select TRIO115 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
from trio import Event, sleep
class _RouteMethod(Enum):
    def __init__(self, token: str):
        self._headers = {
        }
    async def request(
    ):
            try:
                        if bool(resp.headers.get("X-RateLimit-Global")):
                            logger.warning(
                        )
                        await sleep(reset_after)
            except OSError as err:
                    await sleep(5)
```

error
```
/home/rafal/test/tmp_folder/F_NAME_9541977883458822252.py:12:31: TRIO115 Use `trio.lowlevel.checkpoint()` instead of `trio.sleep(0)`
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/home/rafal/test/tmp_folder/F_NAME_9541977883458822252.py` with rule codes TRIO115: invalid syntax. Got unexpected token '.' at byte offset 39
---
from trio import Event, sleep, lowlevel.checkpoint
class _RouteMethod(Enum):
    def __init__(self, token: str):
        self._headers = {
        }
    async def request(
    ):
            try:
                        if bool(resp.headers.get("X-RateLimit-Global")):
                            logger.warning(
                        )
                        await lowlevel.checkpoint
            except OSError as err:
                    await sleep(5)
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353656/python_compressed.zip)





---

_Comment by @qarmin on 2024-01-30 07:08_

Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select RUF022 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
__all__ = (
    "loads",
    "dumps",)
```

error
```
/opt/tmp_folder/F_NAME_5497816149756295069.py:1:11: RUF022 `__all__` is not sorted
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_5497816149756295069.py` with rule codes RUF022: invalid syntax. Got unexpected token ',' at byte offset 37
---
__all__ = (
    "dumps",
    "loads",,)
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094044/python_compressed.zip)


---

_Comment by @MichaReiser on 2024-01-30 07:11_

@AlexWaygood you might be interested in the above `__all__` edge case.

---

_Comment by @qarmin on 2024-01-30 07:17_

```
ruff  *.py --select RUF023 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class BezierBuilder:
    __slots__ = ('xp', 'yp',
                 'canvas',)
```

error
```
/opt/tmp_folder/F_NAME_2486365452265973811.py:2:17: RUF023 `BezierBuilder.__slots__` is not sorted
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_2486365452265973811.py` with rule codes RUF023: invalid syntax. Got unexpected token ',' at byte offset 84
---
class BezierBuilder:
    __slots__ = (
        'canvas',
        'xp',
        'yp',,)
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094102/python_compressed.zip)


---

_Comment by @qarmin on 2024-01-30 07:18_

```
ruff  *.py --select SIM114 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
if(x > 200): pass
elif(100 < x and x < 200 and 300 < y and y < 800):
	pass
```

error
```
/opt/tmp_folder/F_NAME_4487587048573256590.py:1:1: SIM114 Combine `if` branches using logical `or` operator
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_4487587048573256590.py` with rule codes SIM114: unexpected indent at byte offset 67
---
if(x > 200) or (100 < x and x < 200 and 300 < y and y < 800): pass
	pass
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094112/python_compressed.zip)


---

_Comment by @qarmin on 2024-01-30 07:21_

```
ruff  *.py --select PLR1706 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
PERM = 'view' if (VERSION[0] == 2 and VERSION[1] > 0) or (VERSION[0] > 2) else 'change'
```

error
```
/opt/tmp_folder/F_NAME_11180961617118992883.py:1:18: PLR1706 Consider using if-else expression
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_11180961617118992883.py` with rule codes PLR1706: invalid syntax. Got unexpected token 'if' at byte offset 32
---
PERM = 'view' if VERSION[1] > 0 if VERSION[0] == 2 else VERSION[0] > 2 else 'change'
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094141/python_compressed.zip)


---

_Comment by @qarmin on 2024-01-30 07:23_

```
ruff  *.py --select RET505 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
def fibo(n):
    if n<2:
        return n;
    else:
        last = 1;
        last2 = 0;
```

error
```
/opt/tmp_folder/F_NAME_10157061443194071948.py:4:5: RET505 Unnecessary `else` after `return` statement
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_10157061443194071948.py` with rule codes RET505: unexpected indent at byte offset 57
---
def fibo(n):
    if n<2:
        return n;
    last = 1;
        last2 = 0;
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094170/python_compressed.zip)


---

_Comment by @qarmin on 2024-01-30 07:25_

```
ruff  *.py --select C413 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class BNode (object) :
            self.by_gain = list \
                (sorted
                  ( self.candidates.keys ()
                  )
                )
```

error
```
/opt/tmp_folder/F_NAME_289572757642207108.py:2:28: C413 Unnecessary `list` call around `sorted()`
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_289572757642207108.py` with rule codes C413: unexpected indent at byte offset 57
---
class BNode (object) :
            self.by_gain = sorted
                  ( self.candidates.keys ()
                  )
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14094187/python_compressed.zip)


---

_Comment by @charliermarsh on 2024-01-30 13:56_

@diceroll123 - Are you interested in taking a look at some of the `RET` and `SIM` issues in the last few comments? They might be related to your recent fixes.

---

_Comment by @qarmin on 2024-02-21 09:45_

Ruff 0.2.2
 (latest changes from main branch)
```
ruff  *.py --select RET505 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
def findNearest( p, ps, rcut=1e+9 ):
	if rs[imin]<(rcut**2):
	    return imin
	else:
	    return -1
```

error
```
/opt/tmp_folder/F_NAME_1454123032375318835.py:4:2: RET505 Unnecessary `else` after `return` statement
Found 1 error.
[*] 1 fixable with the --fix option.

error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_1454123032375318835.py` with rule codes RET505: unindent does not match any outer indentation level at byte offset 79
---
def findNearest( p, ps, rcut=1e+9 ):
	if rs[imin]<(rcut**2):
	    return imin
 return -1
---
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14357469/python_compressed.zip)


---

_Comment by @qarmin on 2024-02-21 09:46_

ruff 0.2.2
 (latest changes from main branch)
```
ruff  *.py --select PLR0203 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
class IfrArray (object):
  def compile_options ():
    return _options;
  compile_options = staticmethod(compile_options);
```

error
```
/opt/tmp_folder/F_NAME_13846403599701326120.py:2:3: PLR0203 Static method defined without decorator
Found 1 error.
[*] 1 fixable with the --fix option.

warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `RUF011` has been remapped to `B035`.
warning: `TRY200` has been remapped to `B904`.
error: Fix introduced a syntax error in `/opt/tmp_folder/F_NAME_13846403599701326120.py` with rule codes PLR0203: invalid syntax. Got unexpected token ';' at byte offset 88
---
class IfrArray (object):
  @staticmethod
  def compile_options ():
    return _options;
;
---
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14357539/python_compressed.zip)


---

_Comment by @qarmin on 2024-04-06 19:49_

ruff 0.3.5
 (latest changes from main branch)
```
ruff  *.py --select UP032 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
for x in a:
        s += "{0.4f} ".format(x)
```

error
```
/home/rafal/test/tmp_folder/9238284119209442364.py:2:14: UP032 Use f-string instead of `format` call
Found 1 error.
[*] 1 fixable with the --fix option.


error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/9238284119209442364.py`, the rule codes UP032, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14894818/python_compressed.zip)


---

_Comment by @diceroll123 on 2024-04-14 03:47_

> ```
> for x in a:
>         s += "{0.4f} ".format(x)
> ```

this would be interesting to fix, because as-is, it's an AttributeError at runtime. Syntactically, it's fine though.

---

_Comment by @diceroll123 on 2024-04-14 04:02_

It seems https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810829481 `C405` was unceremoniously fixed and is not crossed off here, please confirm?

<img width="617" alt="image" src="https://github.com/astral-sh/ruff/assets/1243957/72cf8634-ae39-46be-83f2-8e744f3db50f">


---

_Comment by @dhruvmanila on 2024-04-15 06:47_

@diceroll123 Yeah, that's correct. I think it's https://github.com/astral-sh/ruff/pull/9821 which fixed it. Thank you!

---

_Closed by @qarmin on 2024-07-29 13:05_

---
