```yaml
number: 1568
title: "False positive: `UP024` Replace aliased errors with `OSError`"
type: issue
state: closed
author: woodruffw
labels: []
assignees: []
created_at: 2023-01-02T22:25:27Z
updated_at: 2023-03-09T19:12:34Z
url: https://github.com/astral-sh/ruff/issues/1568
synced_at: 2026-01-10T11:09:43Z
```

# False positive: `UP024` Replace aliased errors with `OSError`

---

_Issue opened by @woodruffw on 2023-01-02 22:25_

First of all, thanks for `ruff`! I've been trying it on a few of my projects, and it's been a pleasure to use.

This is a report for what looks (to me) like a false positive in `ruff`'s `pyupgrade`-style rules.

The code in question:

```python
        # Make sure the directory exists
        try:
            os.makedirs(os.path.dirname(name), self.dirmode)
        except (IOError, OSError):  # pragma: no cover
            pass
```

`ruff` flags `UP024` on this:

```
pip_audit/_cache.py:116:16: UP024 Replace aliased errors with `OSError`
Found 1 error(s).
1 potentially fixable with the --fix option.
```

...and wants to auto-fix it as this:

```python

        # Make sure the directory exists
        try:
            os.makedirs(os.path.dirname(name), self.dirmode)
        except OSError:  # pragma: no cover
            pass
```

However, as far as I can tell, [`IOError` is not a subclass of `OSError`](https://docs.python.org/3/library/exceptions.html#IOError). As such, this fix probably causes an unintended behavioral change: `IOError`s are no longer caught.

Running context:

```console
$ ruff --version
ruff 0.0.207

$ python -V
Python 3.11.0
```

---

_Comment by @charliermarsh on 2023-01-02 22:27_

Let me take a look!

---

_Comment by @woodruffw on 2023-01-02 22:28_

Hmm, maybe this is a documentation error in Python: `IOError` is indeed a subclass of `OSError`, according to the MRO:

```python
>>> IOError.__mro__
(<class 'OSError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)
```



---

_Comment by @charliermarsh on 2023-01-02 22:28_

Yeah, I'm seeing `IOError = OSError` in the `builtins.py` source.

---

_Comment by @woodruffw on 2023-01-02 22:29_

Yep! Sorry, this looks like an overeager report on my part :sweat_smile:. I'll follow up with CPython to get the docs fixed.

Thanks again for `ruff`!

---

_Closed by @woodruffw on 2023-01-02 22:29_

---

_Comment by @charliermarsh on 2023-01-02 22:30_

No prob at all, thanks for taking the time to file such a thorough Issue in the first place! Don't hesitate if you run into other problems etc.

---

_Comment by @deliro on 2023-03-09 18:05_

Hi guys! First of all, many thanks for this awesome tool!

Found a false positive one:

My code:

```python
import socket
...
from kombu import Connection, exceptions

...

try:
    conn = Connection(settings.CELERY_BROKER_URL)
    conn.ensure_connection(max_retries=2)
    conn._close()
except (socket.error, exceptions.OperationalError):
    return HttpResponseServerError("cache: cannot connect to broker.")
```

their MROs:

```python
exceptions.OperationalError.__mro__
(<class 'kombu.exceptions.OperationalError'>, <class 'kombu.exceptions.KombuError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)

socket.error.__mro__
(<class 'OSError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)
```


ruff suggests:

```python
try:
    conn = Connection(settings.CELERY_BROKER_URL)
    conn.ensure_connection(max_retries=2)
    conn._close()
except OSError:
    return HttpResponseServerError("cache: cannot connect to broker.")
```

Seems like ruff ignores the second one's MRO. Thank you!

---

_Comment by @charliermarsh on 2023-03-09 19:12_

Oh wow, yeah, looks like a bug!

---
