```yaml
number: 10844
title: "New Rule: Disallow uncaught and unchecked exceptions"
type: issue
state: closed
author: Starz0r
labels:
  - type-inference
assignees: []
created_at: 2024-04-09T03:16:28Z
updated_at: 2024-10-24T14:34:15Z
url: https://github.com/astral-sh/ruff/issues/10844
synced_at: 2026-01-10T11:09:53Z
```

# New Rule: Disallow uncaught and unchecked exceptions

---

_Issue opened by @Starz0r on 2024-04-09 03:16_

Typically, if I'm writing a Python program, I want to be sure to handle or at least catch many exceptions. A program like so should not pass without warnings:
```py
import sys


def foo():
	raise Exception("do not call foo()!")


def main() -> int:
	foo()
	return 0


if __name__ == "__main__":
	sys.exit(main())
```

This can be easy if the functions or methods you're pulling from are in the local source directory. However, it gets a bit more tricky when it's not, and without going into where the source files for the library is located, and inspecting it manually you may not know. Since this library likely parses the library source tree, `ruff` likely knows this information. Thus, it can notify us when the function we are calling into has an exception that we are not handling.
```py
import sys

from some_library import foo


def main() -> int:
	foo()
	return 0


if __name__ == "__main__":
	sys.exit(main())
```

In both cases, neither of the functions are wrapped in a try block, as so to make it easier for the program to fail at runtime. By adding this as a rule, `ruff` can notify the programmer that they are calling a function that can explicitly raise an exception, and signal that something should be done about it (or disable the rule for that line). As of now, calling `ruff check .\src\main.py --isolated` on the file passes all checks. I think this would be a very beneficial addition for those writing Python.

Main Inspiration: https://github.com/kisielk/errcheck

---

_Label `multifile-analysis` added by @MichaReiser on 2024-04-09 07:01_

---

_Comment by @MichaReiser on 2024-04-09 07:01_

Note: This requires multifile and control flow anlaysis.

---

_Comment by @NeilGirdhar on 2024-04-10 04:36_

This idea is the main motivation for the desire to annotate exceptions.  The latter idea has been discussed many times on the python-discuss boards, and I think it would be worth going over the discussion.

In general, exceptions are intended to propagate up the stack, so they shouldn't necessarily be handled by the caller.  Also, as noted, there's no way to figure out which exceptions might be raised.  And there are plenty of exceptions that can be raised even if they're not annoted or explicitly raised anywhere (I think there's an out-of-memory, and some kind of user-break, etc.).  Of course, not every function call should handle these.

I'm not saying that this a bad idea, but I think you should go over the discussions on discuss, and hash out how you would want this to work.

---

_Comment by @Starz0r on 2024-04-14 01:05_

Preface: Regardless of what discussions going on, I'd still like to see this implemented in some form or fashion, simply as a hold-over until we can get a proper idea of how exceptions should be communicated to the caller. I wouldn't mind if it was a non-default or even behind an experimental flag, or even a temporary one. I would just like to check these in general, as they're important and there is currently no visible way to have insight into what can be thrown except for documentation (if there is any, and if it's properly documented!). Something as simple as just telling me what exceptions can be thrown from a function or method would be helpful. Anything is better than nothing right now.

> In general, exceptions are intended to propagate up the stack, so they shouldn't necessarily be handled by the caller. 

In cases where they're supposed to propagate up the stack, you could re-raise them.
```py
def foo():
	try:
		write(...) as f:
			pass
	except IOError as e:
			raise e
```
Although I will concede, littering this around in library code would make it harder to read and is just doing the job the normal exception handler would have done already. I don't have a great way to solve this, but in the interim be handled by not having that lint enabled for that file or annotating the function/line with `noqa`.

> Also, as noted, there's no way to figure out which exceptions might be raised. And there are plenty of exceptions that can be raised even if they're not annoted or explicitly raised anywhere (I think there's an out-of-memory, and some kind of user-break, etc.). Of course, not every function call should handle these.

In code where this is the case, the programmer might know ahead of the time, but regardless, this is out of scope for the rule anyway (Perhaps I should re-title the issue with “explicit” exceptions). Any place where an exception is explicitly raised from the caller should be able to be handled, so out-of-memory exceptions as the result of a Python runtime heuristic (or system heuristic), would not apply here. Same with any other similar issues like bad/malformed syntax or divide by zero (although divide by zero is already handled by other lints).

> I'm not saying that this a bad idea, but I think you should go over the discussions on discuss, and hash out how you would want this to work.

Sure. I don't mind discussing this a little more if needed. Just for reference, did you mean the general `python-discuss` boards, or the GitHub Discussions page for ruff?



---

_Comment by @NeilGirdhar on 2024-04-14 07:39_

> Something as simple as just telling me what exceptions can be thrown from a function or method would be helpful.

That's not "simple".

> Although I will concede, littering this around in library code would make it harder to read and is just doing the job the normal exception handler would have done already.

Exactly.  I don't think this is a good idea.

> Any place where an exception is explicitly raised from the caller should be able to be handled,

I don't think that's true.  Where did you get this idea?

> Sure. I don't mind discussing this a little more if needed. Just for reference, did you mean…

[These ones.](https://discuss.python.org/c/ideas/6)  Please read [this](https://discuss.python.org/t/extend-type-hints-to-cover-exceptions/23788) and [this](https://discuss.python.org/t/pep-484-when-will-we-have-syntax-for-exceptions/26936).

---

_Comment by @Starz0r on 2024-04-14 09:32_

It's been awhile since I've been around the “[Checked Exceptions](https://www.artima.com/articles/the-trouble-with-checked-exceptions)” discourse, since I've totally forgotten about it. Didn't really realize at the time of writing what I was asking for, so, sorry for that. I think what I should do is reduce the scope of the issue so it's less controversial. What I'd actually need is just a diagnostic or report that tells me what a function raises and hasn't caught on its own. But as I understand it, this might actually be a little out of scope for the project, as I'm now realizing. However, if I'm wrong, feel free to re-open this, but I think I know in what direction I need to be heading now.

---

_Closed by @Starz0r on 2024-04-14 09:32_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
