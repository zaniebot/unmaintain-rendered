```yaml
number: 180
title: "calls to functions returning `Never`/`NoReturn` are terminal"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-03-13T08:39:42Z
updated_at: 2025-07-11T18:37:53Z
url: https://github.com/astral-sh/ty/issues/180
synced_at: 2026-01-10T02:07:35Z
```

# calls to functions returning `Never`/`NoReturn` are terminal

---

_Issue opened by @sharkdp on 2025-03-13 08:39_

### Summary

The following code emits a `invalid-return-type` diagnostic, but shouldn't:
```py
from typing import NoReturn
import sys

def f() -> NoReturn:
#          ^^^^^^^^ Function can implicitly return `None`, which is not assignable to return type `Never`
    sys.exit(1)
```

`sys.exit` is annotated with returning `NoReturn`, but the problem is that we don't have an explicit `return` statement here.

This is related to unreachable code analysis. We already have some special handling for detecting whether or not the end of scope is reachable (i.e. replacing `sys.exit(1)` with `while True: pass` works fine), but this does not take calls to functions returning `Never`/`NoReturn` into account.


---

_Label `bug` added by @sharkdp on 2025-03-13 08:39_

---

_Renamed from "[red-knot] Return type checking in presence of calls to `Never`/`NoReturn`" to "[red-knot] Return type checking for `Never`/`NoReturn`" by @sharkdp on 2025-03-13 08:40_

---

_Comment by @abhijeetbodas2001 on 2025-03-15 15:58_

Hi David, I would like to work on this. Would this be an OK first issue to pick up?

---

_Comment by @sharkdp on 2025-03-15 19:16_

You are certainly welcome to try! However, I do not think that this is a particularly beginner friendly task. We might have some issues labeled with "help wanted" that might be more suitable for a first contribution.

---

_Comment by @abhijeetbodas2001 on 2025-03-15 20:19_

Got it. I will try to find another issue, or another minor task in this area (I do see some TODOs in the tests, for example).

However, one thing is not clear to me. You mention in the issue description that replacing `sys.exit(1)` with `while True: pass` does not give an error. I tried that out, and indeed there is no error.

However, the following **also** does not give any error:

```py
def f() -> int:
  while True:
    pass
```

(Notice that I changed the return value to `int`.)

Here, if the "`while True` never returns anything" inference was being done correctly, the above should have errored out, right? 

Moreover, the following gives an error:

```py
# "Object of type `Literal["foo"]` is not assignable to return type `int`"
def f() -> int:
  while True:
    pass

  return "foo"
```

... which makes me wonder if `red-knot` does not correctly understand the `while True: pass` case at all. Am I missing something?

Edit: looking at https://github.com/astral-sh/ruff/pull/15676/, which introduced the unreachability-detection, I don't see a "`while True` with no `break`" condition.

---

_Renamed from "[red-knot] Return type checking for `Never`/`NoReturn`" to "[red-knot] calls to functions returning `Never`/`NoReturn` are terminal" by @carljm on 2025-03-26 20:12_

---

_Comment by @abhijeetbodas2001 on 2025-04-08 13:39_


> However, the following **also** does not give any error:
> 
> def f() -> int:
>   while True:
>     pass
> 
> (Notice that I changed the return value to `int`.)

Because there are no reachable `return` statements, and the end of function scope is not visible, it makes sense that there are no diagnostics emitted currently, because we aren't checking the return value at all.

However, in this case, the correct thing to do would be to enforce that the function's annotated return type is `Never`, isn't it?
@sharkdp does this seem fit for a separate issue?


---

_Comment by @sharkdp on 2025-04-09 07:01_

@abhijeetbodas2001 Sorry for responding so late, I must have missed the notification about your original message while I was out on vacation.

> Here, if the "`while True` never returns anything" inference was being done correctly, the above should have errored out, right?

I don't think so. A function annotated with `int` that loops endlessly is not incorrect. You can think of the endless loop as returning `Never`, but `Never` is assignable to the return type `int` (`Never` is assignable to any type), so nothing wrong there.

> Moreover, the following gives an error:

Yes, we don't silence *all* errors in unreachable code. And in this case, it's arguable if that would be an improvement. If the loop were not endless, that return statement *would* be wrong. So flagging it does not seem like unwanted behavior to me.

---

_Comment by @abhijeetbodas2001 on 2025-04-09 07:38_

> (Never is assignable to any type)

Ah! I missed this. That makes sense then. Thanks!

---

_Renamed from "[red-knot] calls to functions returning `Never`/`NoReturn` are terminal" to "calls to functions returning `Never`/`NoReturn` are terminal" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @WAKayser on 2025-05-08 05:12_

Currently NoReturn cannot be used to narrow a type.  Which is a relatively common pattern in for example Flask examples. Where abort() can be used to return an error code early. This can be used to make sure that the request contains valid data. 

For reference here is another example that currently doesnt work. 

```python 
from typing import NoReturn, reveal_type

def raise_error() -> NoReturn:
    raise Exception('test')


def test(x: str | None) -> None:
    if x is None:
        raise_error()
    
    reveal_type(x)
    # Should be "str" is "str | None"
```




---

_Label `control flow` added by @AlexWaygood on 2025-05-11 07:27_

---

_Comment by @Avasam on 2025-05-18 18:34_

Same issue, different symptoms. I have this simplified example that's causing `warning[possibly-unbound-attribute]: Attribute `ignore` on type `QCloseEvent | None` is possibly unbound` since ty doesn't see that 

```py
from PySide6 import QtGui
from PySide6.QtWidgets import QMainWindow

class MainWindow(QMainWindow):
    def closeEvent(self, event: QtGui.QCloseEvent | None = None):
        """Exit safely when closing the window."""

        if event is None or False:  # `or False` is actually some other condition, simplified for this example
            sys.exit(1)  # Here ty should see that `event` can't be `None` if this branch wasn't taken

        # ... some more checks and user prompts happened here

        # Fallthrough case: Prevent program from closing.
        event.ignore()

```

---

_Comment by @ilius on 2025-05-23 07:12_

Also for `os.execl`, `os.execle`, `os.execlp`, `os.execlpe`, `os.execvp`, `os.execvpe`.

Funcs in `os.py` that mention "replacing the current process" in their doc.

```py
def restartLow() -> typing.NoReturn:
	"""Will not return from the function."""
	os.execl(
		sys.executable,
		sys.executable,
		*sys.argv,
	)
```

```py
def execl(file, *args):
    """execl(file, *args)

    Execute the executable file with argument list args, replacing the
    current process. """
    execv(file, args)
```

---

_Assigned to @abhijeetbodas2001 by @carljm on 2025-06-17 16:13_

---

_Comment by @Eutropios on 2025-06-26 04:23_

This error arises in the context of the argparse module, where `argparse.ArgumentParser.error` calls `sys.exit`:
```py
    def exit(self, status=0, message=None):
        if message:
            self._print_message(message, _sys.stderr)
        _sys.exit(status)

    def error(self, message):
        """error(message: string)

        Prints a usage message incorporating the message to stderr and
        exits.

        If you override this in a subclass, it should not return -- it
        should either exit or raise an exception.
        """
        self.print_usage(_sys.stderr)
        args = {'prog': self.prog, 'message': message}
        self.exit(2, _('%(prog)s: error: %(message)s\n') % args)
```
This issue persists when subclassing:
```py
class FooParser(argparse.ArgumentParser):

    def error(self, message: str) -> NoReturn:
        self.exit(1, f"{self.usage}: error: {message}\n")
```
Ty emits:
`Function always implicitly returns `None`, which is not assignable to return type `Never`ty[invalid-return-type](https://ty.dev/rules#invalid-return-type)`.


---

_Closed by @carljm on 2025-07-04 18:52_

---

_Comment by @abhijeetbodas2001 on 2025-07-05 06:33_

Most of the false positives were fixed in https://github.com/astral-sh/ruff/pull/18333, but see also https://github.com/astral-sh/ty/issues/685 for an issue pertaining to type narrowing.

---

_Comment by @WAKayser on 2025-07-09 07:19_

This has not been fully closed yet. And is not only in nested if statements like in #685.  My previous test case isnt supported correctly still. https://github.com/astral-sh/ty/issues/180#issuecomment-2861795534




---

_Comment by @sharkdp on 2025-07-09 07:27_

> My previous test case isnt supported correctly still. [#180 (comment)](https://github.com/astral-sh/ty/issues/180#issuecomment-2861795534)

Unfortunately not, no. We do now consider the `raise_error()` call to be terminal (you can check that by [adding an unknown symbol after the `raise_error()` call](https://play.ty.dev/079489c9-108e-43b0-9495-c05cb1a9b54b), for which no error will be emitted, since it's unreachable), but we do not yet preserve the `x is not None` narrowing constraint from the (implicit) else branch in that conditional. This is already tracked in https://github.com/astral-sh/ty/issues/690.

---

_Comment by @Eutropios on 2025-07-09 20:31_

My [issue](https://github.com/astral-sh/ty/issues/180#issuecomment-3007000359) still persists even after the pull request. Should I open a new issue?

---

_Comment by @abhijeetbodas2001 on 2025-07-10 05:34_

> My [issue](https://github.com/astral-sh/ty/issues/180#issuecomment-3007000359) still persists even after the pull request. Should I open a new issue?

When subclassing, in your example, if you explicitly annotate `self` as `FooParser`, then the error vanishes, for example: https://play.ty.dev/631e5827-6f8a-4144-9261-428a127486f0.

Without this annotation, the type of `self` which ty infers is `Unknown` (which is also thus the inferred type of `self.exit`), which causes the error you see. Not inferring the type of `self` correctly seems to be tracked at https://github.com/astral-sh/ty/issues/159, though @sharkdp could confirm.

---

_Comment by @sharkdp on 2025-07-10 06:30_

> though [@sharkdp](https://github.com/sharkdp) could confirm.

Can confirm 😃. Thank you for the analysis.

---

_Comment by @Eutropios on 2025-07-11 18:37_

> > My [issue](https://github.com/astral-sh/ty/issues/180#issuecomment-3007000359) still persists even after the pull request. Should I open a new issue?
> 
> When subclassing, in your example, if you explicitly annotate `self` as `FooParser`, then the error vanishes, for example: https://play.ty.dev/631e5827-6f8a-4144-9261-428a127486f0.
> 
> Without this annotation, the type of `self` which ty infers is `Unknown` (which is also thus the inferred type of `self.exit`), which causes the error you see. Not inferring the type of `self` correctly seems to be tracked at [#159](https://github.com/astral-sh/ty/issues/159), though [@sharkdp](https://github.com/sharkdp) could confirm.

I see, thank you for the explanation 😄 

---
