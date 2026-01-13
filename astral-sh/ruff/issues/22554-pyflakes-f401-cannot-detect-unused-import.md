```yaml
number: 22554
title: "[`pyflakes`] `F401` cannot detect unused import"
type: issue
state: open
author: chirizxc
labels: []
assignees: []
created_at: 2026-01-13T16:56:16Z
updated_at: 2026-01-13T17:29:50Z
url: https://github.com/astral-sh/ruff/issues/22554
synced_at: 2026-01-13T18:45:32Z
```

# [`pyflakes`] `F401` cannot detect unused import

---

_@chirizxc_

### Summary

I discovered this when I opened [`this*`](https://github.com/reagento/dishka/blob/6b36da4f15dc7e7eedf5ae5a4fc8469dfcfd78ff/src/dishka/integrations/faststream/faststream_06.py) in Pycharm

<img width="1002" height="752" alt="Image" src="https://github.com/user-attachments/assets/41527546-df91-4c3d-ac9b-d9471310d7c2" />

PyCharm:

<img width="1509" height="806" alt="Image" src="https://github.com/user-attachments/assets/d72e74c7-0aa3-4b01-8b94-aced36666293" />

Pyflakes:
```bash
❯ pyflakes xxx.py                                                                                                                              
xxx.py:43:5: redefinition of unused 'Context' from line 23     
```

Ruff:
```bash
❯ ruff check xxx.py --select F401 --preview                                                                                                    
All checks passed!
```


### Version

`ruff 0.14.11 (c920cf8cd 2026-01-08)`

---

_Comment by @chirizxc on 2026-01-13 17:00_

a shorter example:
```py
from faststream import Context, FastStream
from faststream.asgi import AsgiFastStream

Application = FastStream | AsgiFastStream

try:
    # import works only if fastapi is installed
    from faststream._internal.fastapi import Context, StreamRouter

except ImportError:
    from faststream import Context

else:
    Application |= StreamRouter


Context(name="t")
```

Locally, I quickly tried to change this behavior, and the tests even passed, but it seemed like the first start took quite a long time🤔

```bash
❯ myruff check xxx.py
F401 `faststream.Context` imported but unused; redefinition from previous import                                                               
 --> xxx.py:1:24
  |
1 | from faststream import Context, FastStream
  |                        ^^^^^^^
2 | from faststream.asgi import AsgiFastStream
  |
help: Remove unused import: `faststream.Context`

Found 1 error.
```

---

_Comment by @chirizxc on 2026-01-13 17:27_

Also in [`faststream`](https://github.com/ag2ai/faststream)
```bash
❯ pyflakes faststream                                                                                                                          
faststream\app.py:28:5: redefinition of unused 'Lifespan' from line 17                                                                         
faststream\app.py:28:5: redefinition of unused 'LoggerProto' from line 17
faststream\kafka\publisher\usecase.py:20:5: redefinition of unused 'KafkaMessage' from line 7
faststream\rabbit\helpers\declarer.py:9:5: redefinition of unused 'aio_pika' from line 3
faststream\rabbit\helpers\declarer.py:11:5: redefinition of unused 'Channel' from line 6
faststream\rabbit\helpers\declarer.py:11:5: redefinition of unused 'RabbitQueue' from line 6
faststream\specification\base\generator.py:9:5: redefinition of unused 'BrokerUsecase' from line 4

####################################
####################################

❯ ruff check faststream --select F401                                                                            
All checks passed!

```
From [`generator.py`](https://github.com/ag2ai/faststream/blob/9ff725fa4dc6c42f8d8281ee28c2ed73bc01b9db/faststream/specification/base/generator.py#L4-L9):

```py
from faststream._internal.broker import BrokerUsecase


from .specification import Specification


if TYPE_CHECKING:
    from faststream._internal.broker import BrokerUsecase
```

Here, too, we should propose removing imports first `from faststream._internal.broker import BrokerUsecase`

---
