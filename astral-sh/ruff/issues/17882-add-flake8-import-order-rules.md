```yaml
number: 17882
title: Add flake8-import-order rules
type: issue
state: open
author: Coacher
labels:
  - isort
assignees: []
created_at: 2025-05-06T08:08:16Z
updated_at: 2025-05-06T09:12:53Z
url: https://github.com/astral-sh/ruff/issues/17882
synced_at: 2026-01-12T15:54:56Z
```

# Add flake8-import-order rules

---

_@Coacher_

### Summary

Hello.

flake8-import-order plugin provides additional value in supporting various styles of import ordering. Namely rules I201 and I202 extend the default isort rules.

Consider the following file `example.py`:
```
import re
import sys
from collections import Counter, deque, namedtuple
from datetime import date, timedelta

import numpy as np
import pandas as pd
from requests import Request, Response, Session, request
```

Then `ruff check --select I example.py` shows no errors, but `flake8 --select I example.py` shows:
```
example.py:7:1: I201 Missing newline between import groups. 'import pandas' is identified as Third Party and 'import numpy' is identified as Third Party.
example.py:8:1: I201 Missing newline between import groups. 'from requests import Request, Response, Session, request' is identified as Third Party and 'import pandas' is identified as Third Party.
```

Could you please add support for these rules in ruff?

---

_Comment by @MichaReiser on 2025-05-06 08:50_

Can you help me understand what the expected formatting would be. Do you want an empty line between each third party import? 

---

_Comment by @Coacher on 2025-05-06 09:09_

> Do you want an empty line between each third party import?

An empty line between third party imports belonging to different modules.

Extending the previous example, here is ruff-accepted file:
```
import re
import sys
from collections import Counter, deque, namedtuple
from datetime import date, timedelta

import numpy as np
import pandas as pd
from numpy.typing import ArrayLike
from pandas.plotting import boxplot
from requests import Request, Response, Session, request
```

Here is flake8-import-order formatting:
```
import re
import sys
from collections import Counter, deque, namedtuple
from datetime import date, timedelta

import numpy as np
from numpy.typing import ArrayLike

import pandas as pd
from pandas.plotting import boxplot

from requests import Request, Response, Session, request
```

The latter visually separates dependencies from different modules

---

_Label `isort` added by @MichaReiser on 2025-05-06 09:12_

---
