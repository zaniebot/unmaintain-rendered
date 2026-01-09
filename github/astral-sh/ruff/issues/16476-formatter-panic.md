---
number: 16476
title: "[Formatter panic]"
type: issue
state: closed
author: xodiumx
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-03-03T17:01:07Z
updated_at: 2025-03-04T17:00:33Z
url: https://github.com/astral-sh/ruff/issues/16476
synced_at: 2026-01-07T13:12:16-06:00
---

# [Formatter panic]

---

_Issue opened by @xodiumx on 2025-03-03 17:01_

Hello, I am getting an error in the formatter
____
> pyproject.toml:

```
[tool.ruff]
line-length = 120
target-version = "py312"
```
____
> .pre-commit-config.yaml

```yaml
-   repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.9.9
    hooks:
      - id: ruff
        args: [ --fix ]
      - id: ruff-format
```
____
> stacktrace

```
panicked at
 crates/ruff_source_file/src/line_index.rs:117:28:
byte index 472 is not a char boundary; it is inside 'а' (bytes 471..473) of `import pandas as pd
import numpy as np

from sklearn.datasets import make_regression, fetch_california_housing
from sklearn.metrics import r2_score
from sklearn.model_selection import train_test_split
RANDOM_STATE = 123
TRAIN_SIZE = 0.75
np.random.RandomSt`[...]
Backtrace:    0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __pthread_deallocate
```



---

_Label `needs-mre` added by @MichaReiser on 2025-03-03 17:04_

---

_Comment by @MichaReiser on 2025-03-03 17:06_

Huh interesting. Would you mind uploading the header of the file on which this panics (e.g. as a gist?). It would help me reproduce the issue

Note: The file starts or at least contains:

```py
import pandas as pd
import numpy as np

from sklearn.datasets import make_regression, fetch_california_housing
from sklearn.metrics import r2_score
from sklearn.model_selection import train_test_split
RANDOM_STATE = 123
TRAIN_SIZE = 0.75
np.random.RandomSt
```



---

_Label `formatter` added by @MichaReiser on 2025-03-03 17:06_

---

_Comment by @xodiumx on 2025-03-03 17:20_

I found the problem. It's my mistake. The file contained code with incorrect syntax like this:

```py
x = # fill this in
```

Thank you for the quick response

---

_Comment by @MichaReiser on 2025-03-03 17:23_

Let me try this. The formatter shouldn't panic if a file contains invalid syntax and the error message suggests that the error was related to some import statement

---

_Comment by @ntBre on 2025-03-03 17:35_

Is it possible that multiple `pre-commit` steps were modifying the file in parallel? I found this [`require_serial`](https://pre-commit.com/#hooks-require_serial) option but otherwise I don't know much about how `pre-commit` works. That seems like one way invalid bytes could get into a file, though.

The `__pthread_deallocate` in the backtrace also points to some kind of threading.

---

_Comment by @xodiumx on 2025-03-04 14:19_

<details>
  <summary>test.ipynb JSON</summary>
  <pre>
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "id": "47ZRimlw6TEp"
   },
   "outputs": [],
   "source": [
    "RANDOM_STATE = 123\n",
    "TRAIN_SIZE = 0.75"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "id": "as1sEBKJt_Yb"
   },
   "outputs": [],
   "source": [
    "np.random.RandomState(RANDOM_STATE)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "id": "S6nTuMqGGqp2"
   },
   "outputs": [],
   "source": [
    "np.random.seed(RANDOM_STATE)\n",
    "\n",
    "X, y, _ = make_regression(\n",
    "    n_samples=100000,  # число объектов\n",
    "    n_features=10,  # число признаков\n",
    "    n_informative=8,  # число информативных признаков\n",
    "    noise=100,  # уровень шума в данных\n",
    "    coef=True,  # значение True используется при генерации данных\n",
    "    random_state=RANDOM_STATE,\n",
    ")\n",
    "\n",
    "X = pd.DataFrame(data=X, columns=np.arange(0, X.shape[1]))\n",
    "X[10] = X[6] + X[7] + np.random.random() * 0.01"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "id": "fTZWxz1zpb9R"
   },
   "outputs": [],
   "source": [
    "import random\n",
    "\n",
    "\n",
    "def stochastic_gradient_descent(X, y, learning_rate, iterations):\n",
    "    X = np.hstack((np.ones((X.shape[0], 1)), X))\n",
    "    params = np.random.rand(X.shape[1])\n",
    "\n",
    "    j = 0\n",
    "\n",
    "    cost_track = np.zeros((iterations, 1))\n",
    "\n",
    "    for i in range(iterations):\n",
    "        # выберите случайный индекс в диапазон от 0 до len(X)-1 включительно при помощи функции random.randint\n",
    "        j = # ваш код здесь\n",
    "\n",
    "        # обновите веса, используя сдвиг по градиенту только по объекту X[j] (делить на m в данном случае не нужно)\n",
    "        params = # ваш код здесь\n",
    "        cost_track[i] = compute_cost(X, y, params)\n",
    "\n",
    "    return cost_track, params"
   ]
  }
 ],
 "metadata": {
  "colab": {
   "provenance": []
  },
  "kernelspec": {
   "display_name": "ml",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "name": "python",
   "version": "3.12.9"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 0
}
  </pre>
</details>

*Run command:*

```sh
ruff format test.ipynb
```

*Get error:*

```
panicked at
 crates/ruff_source_file/src/line_index.rs:117:28:
byte index 472 is not a char boundary; it is inside 'н' (bytes 471..473) of `RANDOM_STATE = 123
TRAIN_SIZE = 0.75
np.random.RandomState(RANDOM_STATE)
np.random.seed(RANDOM_STATE)

X, y, _ = make_regression(
    n_samples=100000,  # число объектов
    n_features=10,  # число признаков
    n_informative=8, `[...]
Backtrace:    0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
```

*If I add or delete one character, I get:*
```
error: Failed to parse test.ipynb:4:5:14: Expected an expression
```

*And if I fix the syntax, it's fine:*

```
1 file reformatted
```

---

_Label `formatter` removed by @MichaReiser on 2025-03-04 14:42_

---

_Label `bug` added by @MichaReiser on 2025-03-04 14:42_

---

_Referenced in [astral-sh/ruff#16499](../../astral-sh/ruff/pulls/16499.md) on 2025-03-04 15:05_

---

_Closed by @MichaReiser on 2025-03-04 17:00_

---

_Closed by @MichaReiser on 2025-03-04 17:00_

---
