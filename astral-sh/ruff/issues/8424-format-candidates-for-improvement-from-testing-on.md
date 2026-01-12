```yaml
number: 8424
title: "[format] Candidates for improvement from testing on a large code repository"
type: issue
state: closed
author: saippuakauppias
labels:
  - formatter
assignees: []
created_at: 2023-11-01T21:59:44Z
updated_at: 2023-11-06T14:10:47Z
url: https://github.com/astral-sh/ruff/issues/8424
synced_at: 2026-01-12T15:54:48Z
```

# [format] Candidates for improvement from testing on a large code repository

---

_@saippuakauppias_

A few examples that could be improved. We ran `ruff==0.1.3` on a large codebase, I noticed some issues during the review, I don't want to describe each one separately, so I'll put it all together here. If you need - please, please, saw it into subtasks (I can't do it, because MR with changes is big and I'm already tired of looking into it).

1.
```diff
            raise TypeError(
+               "some: {}. " "bar".format(foobar),
-               'some: {}. ' "bar".format(foobar),
            )
```

2.
```diff
        pending = [
            task_id
            for task_id, task_data in response.items()
-           if task_id not in completed and
+           if task_id not in completed
+           and
            # foo
            len(task_id) == UUID_LEN and
        ]
```
better:
```diff
        pending = [
            task_id
            for task_id, task_data in response.items()
-           if task_id not in completed and
+           if task_id not in completed
            # foo
-           len(task_id) == UUID_LEN and
+           and len(task_id) == UUID_LEN and
        ]
```

3.
```diff
        if 'foo' in keys:
-           sql = '''`a` = `b` + VALUES(`a`),'''
+           sql = """
+               `a` = `b` + VALUES(`a`),
+           """ 
```
better (no quotation marks inside the string):
```diff
        if 'foo' in keys:
-           sql = '''`a` = `b` + VALUES(`a`),'''
+           sql = '`a` = `b` + VALUES(`a`),'
```
4.
```diff
-            foo = f'''date > toDate('{start}')'''
+            foo = f"""date > toDate('{start}')"""
```
better:
```diff
-            foo = f'''date > toDate('{start}')'''
+            foo = f"date > toDate('{start}')"
```
5.
```diff
-'''
+"""
some comment in multiline string
foo bar
-'''
+"""
```
better (use comment, if it comment):
```diff
-'''
-some comment in multiline string
+# some comment in multiline string
-foo bar
+# foo bar
-'''
```
6.
```diff
    foo = func(
-        '''
+        """
        SELECT a, b, c FROM bar
-   ''',
+   """,
    )
```
better:
```diff
    foo = func(
-        '''
+        """
        SELECT a, b, c FROM bar
-   ''',
+        """,
    )
```
7.
Why?
```diff
-        self.assertEqual('foo `;\' or `"\' bar', foo)
+        self.assertEqual("foo `;' or `\"' bar", foo)
```
8. ⚠️ 
```diff
-    SOME: str = (
-        "DELETE FROM {0} WHERE `id` = {1} AND `parent_id` = '{2}'"
-    )
+    SOME: (
+        str
+    ) = "DELETE FROM {0} WHERE `id` = {1} AND `parent_id` = '{2}'"
```
9.
```diff
return {
    v: (va[v], count)
    for v, count in vy.items()
    if v in va
    and (
        # comment
-        count < th
-        or v not in vt
+        count < th or v not in vt
    )
}
```


---

_Comment by @charliermarsh on 2023-11-01 22:03_

I believe (8) is already fixed in v0.1.3 -- if you remove the parentheses around the `(str)`, and re-run, it won't re-add them.

---

_Comment by @MichaReiser on 2023-11-01 23:22_

Hey @saippuakauppias. Thanks for listing all these examples that you find worth improving. Could yo provide some more information to help me triage the issue?

* did you use Black before and this is a list of examples where Ruff formatted code differently to Black?
* What's your ruff formatter configuration? 
* if you used black, what was your black configuration?

Out of interest if you don't mind sharing: How large is the repository (lines of code, number of files)? How long does it take ruff to format all files? How long did formatting take using black

---

_Label `formatter` added by @MichaReiser on 2023-11-01 23:22_

---

_Comment by @charliermarsh on 2023-11-06 14:10_

Will take these into advisement but going to close for now in the absence of more info. Thanks for filing!

---

_Closed by @charliermarsh on 2023-11-06 14:10_

---
