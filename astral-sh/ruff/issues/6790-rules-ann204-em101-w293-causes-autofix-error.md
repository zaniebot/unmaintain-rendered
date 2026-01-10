```yaml
number: 6790
title: Rules ANN204, EM101, W293 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-08-22T18:44:28Z
updated_at: 2023-08-23T03:35:47Z
url: https://github.com/astral-sh/ruff/issues/6790
synced_at: 2026-01-10T11:09:49Z
```

# Rules ANN204, EM101, W293 causes autofix error

---

_Issue opened by @qarmin on 2023-08-22 18:44_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
class IXXATBus(BusABC):

    @deprecated_args_alias(
        UniqueHardwareId="unique_hardware_id",
        rxFifoSize="rx_fifo_size",
        txFifoSize="tx_fifo_size",
    )
    def __init__(
        self,
        channel: int,
        can_filters=None,
        receive_own_messages: bool = False,
        unique_hardware_id: Optional[int] = None,
        extended: bool = True,
        rx_fifo_size: int = 16,
        tx_fifo_size: int = 16,
        bitrate: int = 500000,
        **kwargs,
    ):
        
        if _canlib is None:
            raise CanInterfaceNotImplementedError(
                "The IXXAT VCI library has not been initialized. Check the logs for more details."
            )

```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_8101205674.py`, the rule codes ANN204, EM101, W293, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[PY_FILE_TEST_8101205674.py.zip](https://github.com/astral-sh/ruff/files/12411956/PY_FILE_TEST_8101205674.py.zip)



---

_Comment by @dhruvmanila on 2023-08-22 19:25_

Thanks for reporting!

It seems that we're adding the return annotation in the wrong place because of the way we've defined the lexing rules to find the position.

Using a minimal repro:
```python
class Foo:
    @decorator()
    def __init__(self, channel: int):
        pass
```

And the reason is that the decorator is considered as part of the statement range which means the rules (lpar, rpar, colon, now add return annotation) doesn't work.

---

_Label `bug` added by @dhruvmanila on 2023-08-22 19:25_

---

_Label `autofix` added by @dhruvmanila on 2023-08-22 19:25_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-08-22 19:52_

---

_Closed by @dhruvmanila on 2023-08-23 03:35_

---
