---
number: 4136
title: TRY400 flags log.error() + raise/sys.exit()
type: issue
state: open
author: jankatins
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-04-27T19:11:02Z
updated_at: 2025-12-23T04:42:40Z
url: https://github.com/astral-sh/ruff/issues/4136
synced_at: 2026-01-07T13:12:14-06:00
---

# TRY400 flags log.error() + raise/sys.exit()

---

_Issue opened by @jankatins on 2023-04-27 19:11_

In the following code snippet, a TRY400 violation is raised because I use log.error() and not log.exception() in an exception handler. In this case it's by design as I immediately re-raise the exception, I just want to add some message to the exception and I don't want the stacktrace shown twice.

```
import logging
log = logging.get_logger(__name__)
# The outer app loop...
try:
    # actually some function call which can't handle the error but wants to abort...
    try:
        raise Exception("bad")
    except Exception:
        # to get some info into the log to "enrich" it...
        # and I don't want to see the exception stacktrace twice
        log.error("That might mean ..." ) # noqa: TRY400: use log.exception()
        raise
except Exception:
    log.exception("Caught unhandled exception in outer context")
```

In my opinion the pattern "in except block, with a log.something + 'raise' in all cases` should be something TRY400 should ignore/be ok with.


---

_Renamed from "TRY400 is too eager to flag something" to "TRY400 flags log.error() + raise" by @jankatins on 2023-04-27 19:11_

---

_Comment by @tjkuson on 2023-04-27 20:38_

Is there a reason why you couldn't pass `exc_info=False` into the `log.exception` method to suppress the stacktrace?

---

_Comment by @jankatins on 2023-04-27 21:27_

Mostly because I didn't know of it :-) but also because I wanted a different log level than EXCEPTION.

---

_Comment by @dhruvmanila on 2023-04-28 05:12_

If you're using Python 3.11, then you should use the new [`add_notes` feature](https://docs.python.org/3/whatsnew/3.11.html#pep-678-exceptions-can-be-enriched-with-notes) to enhance the message.

> I wanted a different log level than EXCEPTION.

Is this a custom defined log level as it doesn't exists in the [`logging` module](https://docs.python.org/3/library/logging.html?highlight=logging#logging-levels)?

Correct me if I'm wrong as I might not be aware of the context, but I think you could get away with just using `logging.exception`. Whatever message you pass in `logging.exception` is logged at `ERROR` level and in addition to that the traceback is printed as well. You could provide the additional message and have the traceback message in the logs as well. Example:

```python
def foo():
	def bar():
		try:
			raise Exception("exception message")
		except:
			logging.exception("additional message")
	bar()


foo()
# ERROR:root:additional message
# Traceback (most recent call last):
#   File "<ipython-input-7-78c777a5f655>", line 4, in bar
#     raise Exception("exception message")
# Exception: exception message
```

---

_Comment by @jankatins on 2023-04-28 12:17_

> Whatever message you pass in logging.exception is logged at ERROR level 

Wow, thanks, I didn't know that. Always though that exception is it's own level.

re add_notes: Yes, I think that would be nice, but I haven't checked how this is then displayed in the logs. It's really nice that one has a extra log line with just the extra info instead of the "log lines explosion" of the stacktrace...

Anyway: I still think this pattern should be not flagged: it's perfectly good code to use e.g. `log.info("message")` or whatever else is printed... 

---

_Label `question` added by @charliermarsh on 2023-04-29 19:07_

---

_Comment by @saippuakauppias on 2023-06-15 12:29_

If you disable the sending of information about the exception with `exc_info=False` - there is another error: `TRY401 Redundant exception object included in logging.exception call`:

```python
import logging


logger = logging.getLogger('stream.logger')


def foo():
    try:
        raise ValueError('error message')
    except Exception as e:
        logger.error('Found handled error: %s', e)
        logger.error('Found handled error: %s', e, exc_info=False)
        logger.exception('Found handled error: %s', e, exc_info=False)
        logger.exception('Found handled error: %s', str(e), exc_info=False)
```


```
$ ruff "ruff_errors/TRY400.py" --select=TRY400,TRY401
ruff_errors/TRY400.py:11:9: TRY400 Use `logging.exception` instead of `logging.error`
ruff_errors/TRY400.py:12:9: TRY400 Use `logging.exception` instead of `logging.error`
ruff_errors/TRY400.py:13:53: TRY401 Redundant exception object included in `logging.exception` call
ruff_errors/TRY400.py:14:57: TRY401 Redundant exception object included in `logging.exception` call
Found 4 errors.
```

I agree that it is often necessary to log only text without traceback using the `.error()` method

---

_Comment by @saippuakauppias on 2023-06-15 13:39_

But in this case the log will also be traceback, and I do not need it

---

_Comment by @charliermarsh on 2023-06-15 13:43_

Hm, I guess if `exc_info` is set to `False`, we shouldn't raise "Redundant exception object included in call".

---

_Comment by @charliermarsh on 2023-06-15 13:45_

Similarly, "Redundant exception object included in call" should be enforced when we log `error` with `exc_info=True` (right now, it only logs on `exception`).

---

_Comment by @dhruvmanila on 2023-06-15 13:57_

> Similarly, "Redundant exception object included in call" should be enforced when we log `error` with `exc_info=True` (right now, it only logs on `exception`).

I believe this should be true for all other log levels as the `exc_info` parameter is common to all function calls. Additionally, the parameter can also accept exception instances along with a tuple returned by `sys.exc_info`:

```
In [4]: try:
   ...:     raise ValueError("some error")
   ...: except ValueError as exc:
   ...:     logging.warning("Error raised: %s", exc, exc_info=exc)
   ...: 
WARNING:root:Error raised: some error
Traceback (most recent call last):
  File "<ipython-input-4-1428f1d5d721>", line 2, in <module>
    raise ValueError("some error")
ValueError: some error

In [5]: try:
   ...:     raise ValueError("some error")
   ...: except ValueError as exc:
   ...:     logging.warning("Error raised: %s", exc, exc_info=True)
   ...: 
WARNING:root:Error raised: some error
Traceback (most recent call last):
  File "<ipython-input-5-998582a3cfed>", line 2, in <module>
    raise ValueError("some error")
ValueError: some error

In [7]: try:
   ...:     raise ValueError("some error")
   ...: except ValueError as exc:
   ...:     import sys
   ...:     logging.warning("Error raised: %s", exc, exc_info=sys.exc_info())
   ...: 
WARNING:root:Error raised: some error
Traceback (most recent call last):
  File "<ipython-input-7-7998fc845097>", line 2, in <module>
    raise ValueError("some error")
ValueError: some error
```

Reference: https://docs.python.org/3/library/logging.html?highlight=logging#logging.Logger.debug



---

_Comment by @zanieb on 2023-06-15 14:55_

For what it's worth, I agree `logger.exception` is not correct to suggest if you are going to raise the exception after and do not want to log the stack trace. In the same way that we suggest you should use `logger.exception` to set `exc_info=True`, you should use `logger.error` to set `exc_info=False`. Using `logger.exception` then opting back out of `exc_info` is weird.

In part, I may have qualms with `TRY400` itself. If I do `logger.error("foo", exc_info=True)` it should be corrected to `logger.exception("foo")` but it is pretty common to want to write error logs in an exception handler without including the stack trace. Is there another rule that just covers that narrower conversion?

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:15_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:15_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:15_

---

_Referenced in [astral-sh/ruff#7248](../../astral-sh/ruff/issues/7248.md) on 2023-09-15 16:34_

---

_Comment by @jankatins on 2023-12-18 11:14_

Another bad case: when the log is followed by a `sys.exit()`:

```py
# In an optional script
try:
    import ....
except ImportError:
    msg = "Dependencies are missing: run `poetry install --with <dependency group> --sync`"
    logger.error(msg) # noqa: TRY400
    sys.exit(1)
```

I specifically do NOT want the whole stacktrace here because it makes figuring out what to do harder than just seeing the specific instruction here.

---

_Renamed from "TRY400 flags log.error() + raise" to "TRY400 flags log.error() + raise/sys.exit()" by @jankatins on 2023-12-18 11:16_

---

_Comment by @bcyran on 2024-10-17 13:15_

I just want to say that I wholeheartedly agree with @zanieb's stance on this. `TRY400` should not be reported for exceptions which are immediately re-raised. It would be very nice to see this issue picked up again.

---

_Comment by @woutervh on 2025-01-13 10:19_

Personally I find this a weird rule that I strongly disagree with.

I always avoid `logger.exception`, as it is not a loglevel `logger.<loglevel>`

My best practice is to use `logger.error(..., exc_info=True|False)`.
It's more consistent, more explicit.
You can even use different loglevels .
And you can easily make showing the traceback conditional.

Most people use logger.exception because they don't know about `exc_info=True|False`.




---

_Referenced in [astral-sh/ruff#18044](../../astral-sh/ruff/issues/18044.md) on 2025-05-12 14:11_

---

_Comment by @thisisarko on 2025-12-22 21:04_

Noting this has been quiet for a while, but I think this rule could still be valuable for some users. The key divergence from the upstream tryceratops rule that would make it especially useful is adding a few more contextual checks when enforcing it. A custom rule could also work. From what I could gather, these should be good.

1. If the exception block contains a `raise`, pass the rule.
2. If the exception block contains a `return`?
3. If it calls `sys.exit()` or otherwise terminates the program: debatable. I understand @jankatins's point, but I’d still prefer a failure in this case. Opting out is always possible via `# noqa`, so I don’t think this should be a default exclusion.
4. Per https://github.com/astral-sh/ruff/issues/18070, allow the rule to pass if there is at least one `logger.exception()` call within the exception context.

All these could also possibly be made configurable in a `lint.try` section.

---
