```yaml
number: 10083
title: "B006 (unsafe fix) : does not fix correctly with @overload"
type: issue
state: closed
author: yoann9344
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2024-02-22T17:43:32Z
updated_at: 2024-02-28T19:22:48Z
url: https://github.com/astral-sh/ruff/issues/10083
synced_at: 2026-01-12T15:54:49Z
```

# B006 (unsafe fix) : does not fix correctly with @overload

---

_@yoann9344_

```diff
     @overload
     @classmethod
     def query(
         cls,
-        my_list: list[Whatever | int] = [],
+        my_list: list[Whatever | int] | None = None,
     ) -> Whatever:
-        ...
+        if my_list is None:
+            my_list = []

     @overload
     @classmethod
     def query(
         cls,
-        my_list: list[Whatever | int] = [],
+        my_list: list[Whatever | int] | None = None,
     ) -> Whatever:
-        ...
+        if my_list is None:
+            my_list = []

     @classmethod
-    def query(cls, session, my_list=[]):
+    def query(cls, session, my_list=None):
+        if my_list is None:
+            my_list = []
```

It should not touch overloaded functions except to change the right argument :
```diff
-        my_list: list[Whatever | int] = [],
+        my_list: list[Whatever | int] | None = None,
```
But must not add logic in it : 
```diff
-        ...
+        if my_list is None:
+            my_list = []
```

So the result should look like this for overloaded methods : 
```diff
     @overload
     @classmethod
     def query(
         cls,
-        my_list: list[Whatever | int] = [],
+        my_list: list[Whatever | int] | None = None,
     ) -> Whatever:
    ...
```

NB : this overloaded method has no sens, I removed the unnecessary. 

---

_Label `fixes` added by @MichaReiser on 2024-02-23 08:00_

---

_Comment by @MichaReiser on 2024-02-23 08:04_

Thanks for reporting this improvement. I summarised the issue again in my own words after I have played with the example for a bit.

The `B006` replaces the `[]` default value with `None` and inserts the 

```python
if <argument> is None:
	<argument> = []
```

as the first statement in the body which in itself is the semantically correct refactor. 

However, inserting the statement is undesired if the function has a stub body. I don't think we necessarily have to test if the function has the `@overload` signature because changing the body is also undesired in typing stub files. 

[Playground](https://play.ruff.rs/b39d8557-bce4-4506-a6b2-ebf2ed3b7c07)

---

_Label `help wanted` added by @MichaReiser on 2024-02-23 08:04_

---

_Comment by @yoann9344 on 2024-02-23 10:08_

> The `B006` replaces the `[]` default value with `None` and inserts the
> 
> ```python
> if <argument> is None:
> 	<argument> = []
> ```
> 
> as the first statement in the body which in itself is the semantically correct refactor.

Yes that's correct, it also replaces the typing by `<initial type> | None`

> However, inserting the statement is undesired if the function has a stub body. I don't think we necessarily have to test if the function has the `@overload` signature because changing the body is also undesired in typing stub files.

Well see ! I think it is the best way to handle it ! (to be clear : no statement added in stubs but method signature modified)

> [Playground](https://play.ruff.rs/b39d8557-bce4-4506-a6b2-ebf2ed3b7c07)

<details>
<summary>
Except at the end there's not a stub body.
</summary>

```python
class Test:
    @overload
    @classmethod
    def query(
        cls,
        my_list: list[Whatever | int] = [],
    ) -> Whatever:
        ...
    
    @overload
    @classmethod
    def query(
        cls,
        my_list: list[Whatever | int] = [],
    ) -> Whatever:
        ...
    
    @classmethod
    def query(cls, my_list: list[Whatever | int] = []) -> Whatever:
        raise NotImplementedError("")
```

</details>

---

_Comment by @Philipp-Thiel on 2024-02-28 19:01_

I added code that identifies all functions, where the body consists only out of a docstring, the ... literal and/or the pass keyword as a stub and doesn't modify the body then. But now that I think more about it, I also find the case where the function body only raises a NotImplementedError compelling. Should that maybe also be identified as a stub?

---

_Comment by @charliermarsh on 2024-02-28 19:22_

@Philipp-Thiel - Sure, that seems like a reasonable extension. You can use the `SemanticModel` to identify the exception type.

---

_Closed by @charliermarsh on 2024-02-28 19:22_

---
