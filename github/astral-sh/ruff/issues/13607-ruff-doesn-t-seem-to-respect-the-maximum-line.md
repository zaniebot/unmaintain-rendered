---
number: 13607
title: "Ruff doesn't seem to respect the maximum line length"
type: issue
state: closed
author: mbridon
labels:
  - question
  - configuration
assignees: []
created_at: 2024-10-02T13:32:06Z
updated_at: 2024-10-03T13:41:03Z
url: https://github.com/astral-sh/ruff/issues/13607
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff doesn't seem to respect the maximum line length

---

_Issue opened by @mbridon on 2024-10-02 13:32_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"

I searched for "line_length" and couldn't find anything interesting :shrug: 

* A minimal code snippet that reproduces the bug.

I had written the following : 

```
        pretty_print = lambda f: "\n".join(
            [
                line
                for line in md.parseString(xml_string)
                .childNodes[0]
                .toprettyxml(indent=" " * 2, standalone=False)
                .split("\n")
                if line.strip()
            ]
        )
        return pretty_print(xml_string)
```

And then ran : 
```
$ ruff check . --fix --unsafe-fixes
```

This is the changes I got:

```
diff --git a/src/engine_info.py b/src/engine_info.py
index 5a5775d..f86d14c 100644
--- a/src/engine_info.py
+++ b/src/engine_info.py
@@ -194,14 +194,6 @@ class EngineInfoManager(object):
         for engine in engines:
             xml_string += engine.xml()
         xml_string += "</engines>"
-        pretty_print = lambda f: "\n".join(
-            [
-                line
-                for line in md.parseString(xml_string)
-                .childNodes[0]
-                .toprettyxml(indent=" " * 2, standalone=False)
-                .split("\n")
-                if line.strip()
-            ]
-        )
+        def pretty_print(f):
+            return "\n".join([line for line in md.parseString(xml_string).childNodes[0].toprettyxml(indent=" " * 2, standalone=False).split("\n") if line.strip()])
         return pretty_print(xml_string)

```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

Ah, so I didn't know about the `--isolate` flag until opening this issue, so I ran the same command with it, but this gave the same result. :shrug: 

```
$ ruff check . --fix --unsafe-fixes --isolate
```


* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```
$ cat pyproject.toml 
[tool.ruff]
# Extend the `pyproject.toml` file in the parent directory.
extend = "../pyproject.toml"
# But use a different line length.
line-length = 88
```

As this didn't work, I tried also adding this:

```
$ cat ruff.toml 
# Extend the `pyproject.toml` file in the parent directory.
extend = "../pyproject.toml"
# But use a different line length.
line-length = 88
```

And this didn't work either. :confused: 

What did I do wrong?

* The current Ruff version (`ruff --version`).

ruff 0.6.8

---

_Comment by @AlexWaygood on 2024-10-02 16:32_

Hiya! So, our linter rules don't _generally_ guarantee that they'll produce well-formatted code or code that necessarily fits within the specified line length, unfortunately. In general, I think we'd recommend using autofixes alongside an autoformatter such as [Ruff's formatter](https://docs.astral.sh/ruff/formatter/) or Black.

There are a _couple_ of exceptions to this (some rules propose simplifications that wouldn't really make sense if the "simpler" version of the code ended up exceeding the specified line length). But that's the general principle.

---

_Label `question` added by @AlexWaygood on 2024-10-02 16:32_

---

_Label `configuration` added by @AlexWaygood on 2024-10-02 16:32_

---

_Comment by @mbridon on 2024-10-03 07:32_

Indeed, I first passed the code through `black` and then `ruff` kept the exact same code, no overly long line in either case. A bit silly that ruff first needs blacked code, so it's not really a full replacement as I had hoped... :disappointed:

---

_Comment by @AlexWaygood on 2024-10-03 07:37_

Running `ruff check` and _then_ `ruff format` is the order in which we recommend to do things, if you want to run both. If you do it like that, it should hopefully be a full replacement :-) neither the linter nor the formatter should require the code to already be blackened in order to run properly

---

_Comment by @mbridon on 2024-10-03 07:43_

In my case I do require the code to be blackened first, otherwise `ruff check` will add this very long line and remove an import I need to keep for the tests to work...

I added the `fmt: skip` comment so black ignores that block and keeps the import, is there a way to tell ruff to do the same?

Then the only problem remains the overly long line for which I need to blacken the code  :wink: 

---

_Comment by @AlexWaygood on 2024-10-03 07:59_

Yes, we support suppression comments for our formatter in the same way as black: https://docs.astral.sh/ruff/formatter/#format-suppression

---

_Comment by @mbridon on 2024-10-03 13:06_

Ok, so I tried the following:

```
import ibus_cangjie.sounds  # type: ignore # fmt: skip
```

And ruff deleted this line, because the import is unused directly, but it is needed for the unit tests to work at all, because even if the import is not used as is, it is used through mocking:
```
sys.modules["ibus_cangjie.sounds"].ErrorSound = MockErrorSound  # type: ignore
```

So ruff not only deleted the line, ignoring the format suppression comment telling it to skip that line, it also deleted the skipping comment :joy: 

Black at least ignores the line with the same `fmt: skip` comment. :wink: 

---

_Comment by @dhruvmanila on 2024-10-03 13:09_

I think that's because it's the linter that's removing the import and not the formatter. So, `ruff check --fix` will run the linter and fix the diagnostics that can be fixable. You can find more information on the rules that provide fixes at https://docs.astral.sh/ruff/rules/. The way to suppress that is using `noqa` comments, so in this case it should be `# noqa: F401` (https://docs.astral.sh/ruff/rules/unused-import/).

---

_Comment by @mbridon on 2024-10-03 13:35_

Indeed, that works, thank you @dhruvmanila :tada:

So ruff is basically all I want now, just an insanely fast black :grin: 

---

_Comment by @AlexWaygood on 2024-10-03 13:41_

Great! Sorry for the confusion, glad we could help here 😊

---

_Closed by @AlexWaygood on 2024-10-03 13:41_

---
