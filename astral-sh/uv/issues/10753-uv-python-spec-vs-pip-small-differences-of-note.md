```yaml
number: 10753
title: uv python spec vs pip - small differences of note?
type: issue
state: closed
author: EdmundsEcho
labels:
  - question
assignees: []
created_at: 2025-01-19T15:55:36Z
updated_at: 2025-01-22T21:56:07Z
url: https://github.com/astral-sh/uv/issues/10753
synced_at: 2026-01-12T16:00:20Z
```

# uv python spec vs pip - small differences of note?

---

_@EdmundsEcho_

I now use `uv` as my build tool for python.  A team member needed to have my code base operate on Render, an online web hosting service that only supports `pip`.  To coordinate the two, I ran `uv pip freeze > requirements.txt` to enable an install using `pip install -r requirements.txt`.  Surprisingly, there were a few "errors" (strictly speaking), that `pip` raised where my `uv run...` workflow worked just fine.

So this is a FYI. Here is a list of places where `uv` worked, and `pip` failed.  They all have to do with how string literals can and cannot be specified on multiple lines.

Note, I'm not commenting on what is right or wrong (e.g., there is a place where I used quotes inside quotes that `uv` figured out how to interpret without raising an error, but `pip` raised an error).  Here is a list of changes that needed to be made to the `uv` version in order to get the `pip` version to work:

```
-        logger.debug(f"✅ top left start: { json.dumps(
-            { "top": y_start_override or y_start,
-              "left": left_margin },
-            indent=3)}."
-        )
+        logger.debug(f"✅ top left start: {json.dumps({'top': y_start_override or y_start, 'left': left_margin}, indent=3)}")

-  text = f"{text} {element["Text"]}"
+  text = f"{text} {element['Text']}"


-    logger.debug(f"💫 {page_:02} data_elements_iterator { json.dumps(
-        {
-            'x_start': x_start,
-            'x_end': x_end,
-            'y_start': data_y_start,
-            'y_end': data_y_end,
-        }, indent=3)
-    }")
+    debug_info = {
+        'x_start': x_start,
+        'x_end': x_end,
+        'y_start': data_y_start,
+        'y_end': data_y_end,
+    }
+    logger.debug(f"💫 {page_:02}  data_elements_iterator {json.dumps(debug_info, indent=3)}")

-        logger.debug(
-            f"Instantiated Transactions type: {self.amount_row_type}, "
-            f"with continuation date: {self.date_value_buffer.get_text() if self.date_value_buffer else 'None'}"
-        )
+        logger.debug(f"Instantiated Transactions type: {self.amount_row_type}, with continuation date: {self.date_value_buffer.get_text() if self.date_value_buffer else 'None'}")
```

IMO: While I value `uv` perhaps being more flexible, I suspect in the long run it makes sense to keep `uv` in-line with the pip spec.  Finally, this might all be moot if you tell me that this only happens in "older" versions of pip.  I cannot report on the version in question.

---

_Comment by @charliermarsh on 2025-01-19 16:10_

I believe this is just a difference in the Python version that you're using? Neither uv nor pip actually execute Python code, but the Python spec changed in the past few minor versions to be more permissive with respect to the contents of f-strings. My guess is that with whatever workflow you're using uv, you're using a greater Python version than wherever you're using pip.

---

_Label `question` added by @charliermarsh on 2025-01-20 03:38_

---

_Comment by @EdmundsEcho on 2025-01-20 03:59_

I don't know what it is about python that keeps me in a seemingly permanent state of confusion re build vs runtime. Notwithstanding, I don't know if we are dealing with a build or runtime issue. `uv run` is both building and referencing the python version specified in the pyproject.toml file to execute. I believe your explanation makes sense if it's a runtime issue. Yes?

---

_Comment by @konstin on 2025-01-20 15:33_

My best guess is that the problem is that Render is using a different Python version than your `uv run` (at runtime). For example `text = f"{text} {element["Text"]}"`, was introduced in 3.12, while `text = f"{text} {element['Text']}"` works in earlier versions too. Can you check the Python version Render and you are using (`uv run -v`)?

---

_Comment by @EdmundsEcho on 2025-01-22 21:56_

I think that you are correct. The version that required these changes was 3.11 (on Render).  My local build may have been using 3.12.  Thank you for the follow-up.  

---

_Closed by @EdmundsEcho on 2025-01-22 21:56_

---
