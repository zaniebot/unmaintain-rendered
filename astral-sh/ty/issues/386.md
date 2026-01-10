```yaml
number: 386
title: "\"Too many iterations\" panic from `exported_names` query when analysing a `*` import"
type: issue
state: closed
author: rushiagr
labels:
  - needs-mre
  - fatal
  - imports
assignees: []
created_at: 2025-05-14T15:37:03Z
updated_at: 2025-11-12T16:01:17Z
url: https://github.com/astral-sh/ty/issues/386
synced_at: 2026-01-10T02:06:24Z
```

# "Too many iterations" panic from `exported_names` query when analysing a `*` import

---

_Issue opened by @rushiagr on 2025-05-14 15:37_

Attaching run of ty with `RUST_BACKTRACE=1`. Please let me know if you need more info.


[ty-issue.txt](https://github.com/user-attachments/files/20210471/ty-issue.txt)

---

_Label `fatal` added by @AlexWaygood on 2025-05-14 15:43_

---

_Label `imports` added by @AlexWaygood on 2025-05-14 15:43_

---

_Comment by @AlexWaygood on 2025-05-14 15:43_

Thank you. It looks like we're entering an infinite loop when trying to analyze a `*` import somewhere. If `/Users/rap/src/kite/sandesh-chacha/tickertrix/lib.py` has a `*` import somewhere in it, it would be useful to see the source code of the module it's trying to do a `*` import from.

---

_Comment by @rushiagr on 2025-05-14 16:04_

that file lib.py doesn't do a `*` import, but it imports newlib and newlib has (in `__init__.py`), `*` imports. lib.py's imports and `__init__.py` file of newlib pasted below:

```py
# import gevent.monkey  # noqa
#
# gevent.monkey.patch_all()  # noqa
import datetime
import functools
import gzip
import json
import math
import os
import pickle
import random
import threading
import zlib
from concurrent import futures
from typing import Callable, Dict, List, Union

import boto3
import pytest
import requests
from botocore import config
from dateutil.tz import gettz

import cu.fs
import newlib
from newlib import rushiforreal_access_key_id, rushiforreal_secret_access_key

```

```py
from .testutils import *  # noqa
from .aa import *  # noqa
from .creds import *  # noqa
from .dt import *  # noqa
from .simpy import *  # noqa
from .files import *  # noqa
from .market import *  # noqa
from .ntfy import *  # noqa
from .exc import *  # noqa
from .pledgeparser import *  # noqa
from .strings import *  # noqa
from .req import *  # noqa
from .otp import *  # noqa
from .ipc import *  # noqa
from .locking import *  # noqa

"""
isort:skip_file
"""
```

if sources of any or all of the imported files in newlib/__init__.py would help, i'll provide them in a few hours

---

_Renamed from "[panic] Encountered panic in my 60k loc codebase" to "[panic] "Too many iterations" from `exported_names` query when analysing a `*` import" by @AlexWaygood on 2025-05-14 16:06_

---

_Renamed from "[panic] "Too many iterations" from `exported_names` query when analysing a `*` import" to ""Too many iterations" panic from `exported_names` query when analysing a `*` import" by @AlexWaygood on 2025-05-14 16:07_

---

_Comment by @AlexWaygood on 2025-05-14 22:02_

Thank you! It would be great if you could see if it still reproduces with some of those `*` imports removed, so that we can identify which one(s) are causing the problem here. And e.g. if it's the `from .testutils import *` import that's causing the problem, we'll probably need to see the imports inside the  `newlib.testutils` module, too

---

_Comment by @rushiagr on 2025-05-16 01:04_

Couldn't get much time yesterday. I tried removing one each of the imports, and then everything from the above `__init__.py` file, which still caused the exception. Then I tried to remove one and count the number of times the error is printed. When I removed 'market' import, the number of times it is printed reduced from 17 to 12. I'll explore this further, but I also observed that sometimes when I do a command like below, it gets stuck and I basically had to kill the 'ty' process from another thread. Is there a bug reported for that already? If not, I can report, but not sure what diagnostics I'll be able to provide. 

```
 RUST_BACKTRACE=1 uvx ty check  | grep sandesh  | grep py | wc
```

---

_Label `needs-mre` added by @AlexWaygood on 2025-05-16 02:24_

---

_Comment by @sharkdp on 2025-05-19 12:58_

I'm closing this in favor of https://github.com/astral-sh/ty/issues/444 which has a MRE.

---

_Closed by @sharkdp on 2025-05-19 12:58_

---

_Closed by @sharkdp on 2025-05-19 12:58_

---

_Reopened by @carljm on 2025-11-12 16:00_

---

_Closed by @carljm on 2025-11-12 16:01_

---
