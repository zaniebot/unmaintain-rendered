```yaml
number: 19397
title: Empty line between condition and function
type: issue
state: open
author: gagaga67
labels:
  - formatter
  - style
assignees: []
created_at: 2025-07-17T13:09:37Z
updated_at: 2025-07-18T12:50:47Z
url: https://github.com/astral-sh/ruff/issues/19397
synced_at: 2026-01-12T15:54:56Z
```

# Empty line between condition and function

---

_@gagaga67_

### Question

Hello! I'm just starting to figure out ruff. And a question arose. As I understand it, ruff should not insert empty lines after the block https://docs.astral.sh/ruff/formatter/black/#blank-lines-at-the-start-of-a-block. Ruff behavior is perfect, unlike black behavior.

I'm checking different ways of writing code and how it will be formatted. And I found an example that seems to be different from the documentation. For me, this is not the best kind of formatting. From the description I thought that there would be no empty lines anywhere. If it is not, then correct me.

```python

#before
def dummy():
    if True:
        def function():
            pass


if True:
    def function():
        pass

if True:
    if True:
        def function():
            pass


if True:
    a = 1
    def function():
        pass
    b = 1


class A:
    def function(self):
        pass


def function2(self):
    def function3(self):
        pass

#after
def dummy():
    if True:

        def function():
            pass


if True:

    def function():
        pass


if True:
    if True:

        def function():
            pass


if True:
    a = 1

    def function():
        pass

    b = 1


class A:
    def function(self):
        pass


def function2(self):
    def function3(self):
        pass
```

### Version

_No response_

---

_Label `question` added by @gagaga67 on 2025-07-17 13:09_

---

_Comment by @ntBre on 2025-07-17 13:34_

I don't think this is related to that known deviation. Black produces the same result:

<details><summary>Output</summary>

```shell
> cat <<EOF | black --diff -
#before
def dummy():
    if True:
        def function():
            pass


if True:
    def function():
        pass

if True:
    if True:
        def function():
            pass


class A:
    def function(self):
        pass


def function2(self):
    def function3(self):
        pass
EOF
--- STDIN       2025-07-17 13:30:43.590201+00:00
+++ STDOUT      2025-07-17 13:30:43.597723+00:00
@@ -1,18 +1,22 @@
-#before
+# before
 def dummy():
     if True:
+
         def function():
             pass


 if True:
+
     def function():
         pass

+
 if True:
     if True:
+
         def function():
             pass


 class A:
would reformat -

All done! ✨ 🍰 ✨
1 file would be reformatted.
```

</details>

I believe these blank lines are inserted before the functions, not after the start of the enclosing block. I guess there's a special case for functions in classes and functions in functions, based on the last two examples.


---

_Label `formatter` added by @ntBre on 2025-07-17 13:34_

---

_Comment by @gagaga67 on 2025-07-17 13:45_

The fact that black does this is not a standard for me :). 

Maybe we should also make an exception for the function after the condition? In our example, such formatting is very ugly. Such examples often occur here. And we don't know yet how to block it either.

---

_Comment by @MichaReiser on 2025-07-18 12:50_

> I don't think this is related to that known deviation. Black produces the same result:

Yes, this is different. The linked deviation is about optional blank lines. The blank lines above functions aren't optional. 

I found this upstream discussion https://github.com/psf/black/issues/450 

I do agree that 

```py
def dummy():
    if True:

        def function():
            pass


if True:

    def function():
        pass


if True:
    if True:

        def function():
            pass
```

looks a bit funny and is something that could be considered for a future style guide, given that the diff isn't too large and it doesn't regress any other existing code. 

The other formattings do match my expectations.

---

_Label `question` removed by @MichaReiser on 2025-07-18 12:50_

---

_Label `style` added by @MichaReiser on 2025-07-18 12:50_

---
