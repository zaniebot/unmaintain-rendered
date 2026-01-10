```yaml
number: 18437
title: ruff format adds trailing comma after function call, silently turning return value into tuple
type: issue
state: closed
author: keita00
labels:
  - formatter
  - needs-mre
assignees: []
created_at: 2025-06-03T08:09:18Z
updated_at: 2025-06-05T12:31:06Z
url: https://github.com/astral-sh/ruff/issues/18437
synced_at: 2026-01-10T11:09:58Z
```

# ruff format adds trailing comma after function call, silently turning return value into tuple

---

_Issue opened by @keita00 on 2025-06-03 08:09_


Ruff's formatter currently adds a trailing comma after a closing parenthesis ) of a nested function call — even when the result is not part of an argument list, which silently converts the return value into a tuple.

This behavior can change the semantics of the code without warning and differs from how black handles similar cases.


---

❌ Example of broken behavior:
```py
result = os.path.exists(
    os.path.join(
        tmp_repo_path,
        "models",
        self.model_name,
        self.model_version,
        "model.tar.gz",
    ),  # ← Ruff adds this trailing comma
)
```

This turns os.path.join(...) into a tuple, and os.path.exists(...) receives a tuple instead of a string — which breaks the logic.


---

✅ Expected (and safe) formatting:

```
result = os.path.exists(
    os.path.join(
        tmp_repo_path,
        "models",
        self.model_name,
        self.model_version,
        "model.tar.gz",
    )
)
```

Trailing commas inside containers are fine, but not after closing a function call when the result is not part of an argument list.


---

🔍 Affected cases:

Ruff will apply this formatting even when:

The return value is assigned to a variable

The expression is used directly as a single argument to another call

The code is not inside a container



---

💥 Why this is critical:

Adds semantic-breaking trailing commas silently

Converts values to single-element tuples ((<value>,))

Breaks common patterns like:

```py

result = json.load(
    open(
        os.path.join(...),
    ),  # ← breaks: open receives a tuple
)
```


---

💻 Environment:

Ruff version: 0.11.12

Installed via: pre-commit with ruff-pre-commit hook

Config: # ruff: format = off/on tested but does not block the formatter

Formatter used: ruff format via pre-commit

Python version: 3.10.14

---

🧪 Suggested resolution:

Suppress trailing comma after closing ) or ] if the value is not in a container or call argument list

Follow Black’s behavior (which avoids this case)

Or provide a configuration option to opt out of this behavior in pyproject.toml

---

_Label `needs-info` added by @MichaReiser on 2025-06-03 08:17_

---

_Comment by @MichaReiser on 2025-06-03 08:19_

Do you have an example where the trailing comma changes the runtime semantics?

A trailing comma in a call expression doesn't convert the expression to a tuple. In fact, it has no runtime semantics. You can see this with:

```py
def my_call(a): print(a)

my_call(1)
my_call(1,)
```

Running this program prints 

```
1
1
```



---

_Comment by @keita00 on 2025-06-03 08:28_

```result = os.path.exists(
    os.path.join(
        tmp_repo_path,
        "models",
        self.model_name,
        self.model_version,
        "model.tar.gz",  # ← trailing comma OK here
    )  # ← line : should **NOT** have a comma
)
```

That's the problem. Ruff tries to insert comma there.
That breaks the logic — os.path.exists() expects a str, not a tuple.

I will upload full log from execution with diff to give more info in few minutes

---

_Comment by @MichaReiser on 2025-06-03 08:39_

I ran your example through the python parser (using `python -m ast < file.py`) where the left is the example with a trailing comma and the right without. Both produce the exact same parsed AST (which is what python then uses to interpret your code). ([diff](https://www.diffchecker.com/YSYL5iuS/))


I think you're issue is with something else. Can you share your exact code?

---

_Comment by @keita00 on 2025-06-03 09:06_

Diff
```
+++ tests/monitoring/test_unit_monitoring.py
@@ -27,7 +27,7 @@
             (False, model_id, None),
             (True, model_id, None),
             (True, "test_model_abc", ValueError),
-        ]
+        ],
     )
     def test_get_model_metadata(self, monkeypatch, spark, prod_run, model_id, exception):
         model_registry_db = "discovery_ard368_mf"
@@ -59,7 +59,7 @@
             (datetime(2024, 5, 20), ["2024-01-24", "2024-02-01", "2024-02-20", "2024-03-20"], 0, ValueError, None),
             (datetime(2024, 1, 20), ["2023-10-20", "2023-11-20", "2023-12-20"], 0, None, "2023-10-20"),
             (datetime(2024, 1, 20), ["2023-09-20", "2023-10-20", "2023-11-20", "2023-12-20"], 1, None, "2023-09-20"),
-        ]
+        ],
     )
     def test_get_prediction_date_from_given_date(self, monkeypatch, spark, monitoring, date, inference_dates, lag, exception, expected):
         monkeypatch.setattr("mddk.monitoring.monitoring.get_distinct_load_dates", lambda x, y: inference_dates)
@@ -77,7 +77,7 @@
         [
             True,
             False,
-        ]
+        ],
     )
     def test_get_prediction_results(self, monkeypatch, spark, prod_run):
         database = "app_ard368_output" if prod_run else "discovery_ard368_output"
@@ -106,7 +106,7 @@
         [
             True,
             False,
-        ]
+        ],
     )
     def test_default_classification_evaluation(self, monkeypatch, spark, prod_run):
         threshold = 0.5
@@ -155,7 +155,7 @@
             ([202402, 202403, 202404], TestDataMonitoring.metrics_data, TestDataMonitoring.metrics_result),
             (None, TestDataMonitoring.metrics_data_null_truth_dates, TestDataMonitoring.metrics_result),
             ([202402, 202403, 202404], TestDataMonitoring.metrics_data_null_values, TestDataMonitoring.metrics_result_null),
-        ]
+        ],
     )
     def test_metrics_result_to_df(self, spark, monitoring, truth_dates, expected_data, result_metrics):
 
@@ -206,7 +206,7 @@
             (True, True),
             (False, True),
             (False, False),
-        ]
+        ],
     )
     def test_run(self, monkeypatch, spark, prod_run, otv_probability):
         output_database = "app_ard368_output" if prod_run else "discovery_ard368_output"

```
Test results after suggested change.
```
=========================== short test summary info ============================
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_get_prediction_results[True] - AttributeError: 'tuple' object has no attribute 'schema'
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_get_prediction_results[False] - AttributeError: 'tuple' object has no attribute 'schema'
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_join_prediction_with_truth - AttributeError: 'tuple' object has no attribute 'join'
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_add_if_not_exist_itv_otv_metrics[input_data0-input_schema0-output_data0] - AttributeError: 'tuple' object has no attribute 'filter'
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_add_if_not_exist_itv_otv_metrics[input_data1-input_schema1-output_data1] - AttributeError: 'tuple' object has no attribute 'filter'
FAILED tests/monitoring/test_unit_monitoring.py::TestMonitoring::test_add_if_not_exist_itv_otv_metrics[input_data2-input_schema2-output_data2] - AttributeError: 'tuple' object has no attribute 'filter'
====== 6 failed, 154 passed, 20 skipped, 10 warnings in 62.41s (0:01:02) =======
```
Code attached

[test_unit_monitoring.txt](https://github.com/user-attachments/files/20565640/test_unit_monitoring.txt)

---

_Comment by @MichaReiser on 2025-06-03 09:14_

Interesting. Are you sure there are no other changes? 

I'm asking because I did the same as before. I took your example and ran both the version with and without trailing comma through the python parser and they prodcue the same AST.

https://www.diffchecker.com/eVPznTGs/

The only other thing I could think of is if pytest does source text parsing

---

_Comment by @MichaReiser on 2025-06-03 09:18_

@AlexWaygood mentioned that this could be a bug in pytest. Let me see if I can come up with a smaller reproduction. What the formatter does here is safe under python semantics

---

_Comment by @keita00 on 2025-06-03 09:20_

That's all for this file.
Happened not only in tests, but in apllication as well. Usually changing tuple to tuple of tuples, or list/dist to tuple.
I've also tried to disable formatting (fmt: off/on, fmt: skip, ruff: format = off/on). None worked and it still tried to add comma in some places.
While I agree with most of suggested fixes, this actually affected type, which is not ideal.

---

_Comment by @AlexWaygood on 2025-06-03 09:31_

@keita00, could you tell us:
- Exactly what command you're using to run your tests?
- What version of pytest you have installed?
- If you have any pytest plugins installed and, if so, what versions of those plugins you have installed?

---

_Comment by @keita00 on 2025-06-03 09:53_

Of course:
pytest-8.2.2 pytest-metadata-3.1.1 pytest_html-4.1.1 pytest_mock-3.14.0 
Tests are run with this command:
```
+ : 'Run tests with coverage'
+ .venv/bin/python -m coverage run --source=src -m pytest -o log_cli=true --log-level=INFO --show-capture=log -W ignore:DeprecationWarning -vv
```

---

_Comment by @MichaReiser on 2025-06-04 08:26_

I created an upstream issue in pytest to figure out the next steps https://github.com/pytest-dev/pytest/issues/13489

---

_Comment by @RonnyPfannschmidt on 2025-06-04 10:47_

Based on the provided information this is not a pytest issue 



---

_Comment by @MichaReiser on 2025-06-04 11:21_

@keita00 could you try to narrow down the problem into a smaller reproducible that doesn't depend on any of your code. Ideally, a single test that can be run without any third party dependencies and demonstrate that pytest once passes a tuple after ruff added the trailing comma

---

_Label `needs-info` removed by @MichaReiser on 2025-06-04 11:21_

---

_Label `needs-mre` added by @MichaReiser on 2025-06-04 11:21_

---

_Comment by @keita00 on 2025-06-04 13:02_

I'll try to do it tomorrow.

---

_Comment by @keita00 on 2025-06-05 11:35_

I was able to reproduce it using only one import:
from pyspark import SparkConf
```
        config = SparkConf().setAll(
            [
                ("spark.sql.execution.arrow.enabled", False),
                ("spark.sql.repl.eagerEval.enabled", True),
                ("spark.dynamicAllocation.enabled", True),
            ]
        )
```

Ruff tried to add comma:
```
                 ("spark.sql.execution.arrow.enabled", False),
                 ("spark.sql.repl.eagerEval.enabled", True),
                 ("spark.dynamicAllocation.enabled", True),
-            ]
+            ],
```

Using # ruff: format off/on didn't affect outcome and comma was still being added. This changes return value.

---

_Comment by @MichaReiser on 2025-06-05 11:48_

Can you share a few more lines. It's hard to tell what's happening given only the few lines

---

_Comment by @AlexWaygood on 2025-06-05 12:01_

@keita00, you can use Python's `ast` module in the standard library to see that the AST that Python produces is exactly the same for the two calls -- the list is not turned into a tuple because of the trailing comma being added:

```pycon
>>> import ast
>>> x = ast.parse("""\
... config = SparkConf().setAll(
...             [
...                 ("spark.sql.execution.arrow.enabled", False),
...                 ("spark.sql.repl.eagerEval.enabled", True),
...                 ("spark.dynamicAllocation.enabled", True),
...             ]
...         )""")
>>> y = ast.parse("""\
... config = SparkConf().setAll(
...             [
...                 ("spark.sql.execution.arrow.enabled", False),
...                 ("spark.sql.repl.eagerEval.enabled", True),
...                 ("spark.dynamicAllocation.enabled", True),
...             ],
...         )""")
>>> ast.dump(x) == ast.dump(y)
True
>>> print(ast.dump(y, indent=2))
Module(
  body=[
    Assign(
      targets=[
        Name(id='config', ctx=Store())],
      value=Call(
        func=Attribute(
          value=Call(
            func=Name(id='SparkConf', ctx=Load())),
          attr='setAll',
          ctx=Load()),
        args=[
          List(
            elts=[
              Tuple(
                elts=[
                  Constant(value='spark.sql.execution.arrow.enabled'),
                  Constant(value=False)],
                ctx=Load()),
              Tuple(
                elts=[
                  Constant(value='spark.sql.repl.eagerEval.enabled'),
                  Constant(value=True)],
                ctx=Load()),
              Tuple(
                elts=[
                  Constant(value='spark.dynamicAllocation.enabled'),
                  Constant(value=True)],
                ctx=Load())],
            ctx=Load())]))])
```

The same applies for your original snippet in this issue:

```pycon
>>> a = ast.parse("""\
... result = os.path.exists(
...     os.path.join(
...         tmp_repo_path,
...         "models",
...         self.model_name,
...         self.model_version,
...         "model.tar.gz",
...     ),  # ← Ruff adds this trailing comma
... )""")
>>> b = ast.parse("""\
... result = os.path.exists(
...     os.path.join(
...         tmp_repo_path,
...         "models",
...         self.model_name,
...         self.model_version,
...         "model.tar.gz",
...     )
... )""")
>>> ast.dump(a) == ast.dump(b)
True
>>> print(ast.dump(b, indent=2))
Module(
  body=[
    Assign(
      targets=[
        Name(id='result', ctx=Store())],
      value=Call(
        func=Attribute(
          value=Attribute(
            value=Name(id='os', ctx=Load()),
            attr='path',
            ctx=Load()),
          attr='exists',
          ctx=Load()),
        args=[
          Call(
            func=Attribute(
              value=Attribute(
                value=Name(id='os', ctx=Load()),
                attr='path',
                ctx=Load()),
              attr='join',
              ctx=Load()),
            args=[
              Name(id='tmp_repo_path', ctx=Load()),
              Constant(value='models'),
              Attribute(
                value=Name(id='self', ctx=Load()),
                attr='model_name',
                ctx=Load()),
              Attribute(
                value=Name(id='self', ctx=Load()),
                attr='model_version',
                ctx=Load()),
              Constant(value='model.tar.gz')])]))])
```

Unless some other tool or library is somehow (inaccurately) rewriting your source code or AST before your code is executed, I don't see how the change Ruff is making here could alter the semantics of your code, unfortunately :-(

At runtime, both of these appear to work fine on my machine:

```pycon
 % uv run --no-project --with=pyspark python
⠋ pyspark==4.0.0                                                                                                                                                                                                        Built pyspark==4.0.0
Installed 2 packages in 11ms
Python 3.13.2 (main, Mar 17 2025, 21:26:38) [Clang 20.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from pyspark import SparkConf
>>> config = SparkConf().setAll(
...             [
...                 ("spark.sql.execution.arrow.enabled", False),
...                 ("spark.sql.repl.eagerEval.enabled", True),
...                 ("spark.dynamicAllocation.enabled", True),
...             ]
...         )
>>> config = SparkConf().setAll(
...             [
...                 ("spark.sql.execution.arrow.enabled", False),
...                 ("spark.sql.repl.eagerEval.enabled", True),
...                 ("spark.dynamicAllocation.enabled", True),
...             ],
...         )
>>>
```

You'll need to give us a _complete_ snippet that shows either different AST being produced or different behaviour at runtime.

---

_Comment by @keita00 on 2025-06-05 12:09_

How about this:
```
    def conf(self, params: Optional[List[Tuple]] = None) -> None:
        """User-defined spark configuration interface.

        Args:
          params:
            Iterable of tuples. Other user-defined spark configuration variables, usually passed
            as a list of key-value pairs to set. Default value is None

        """

        config = SparkConf().setAll(
            [
                ("spark.sql.execution.arrow.enabled", False),
                ("spark.sql.repl.eagerEval.enabled", True),
                ("spark.dynamicAllocation.enabled", True),
            ]
        )

        if params:
            config.setAll(params)
        self.config = config
```

---

_Label `formatter` added by @AlexWaygood on 2025-06-05 12:12_

---

_Comment by @RonnyPfannschmidt on 2025-06-05 12:13_

At this point im of the impression that by deliberate censorship of the code op is accidentally hiding the real issue


Either tell the whole story or the issue needs to close as invalid 

---

_Comment by @keita00 on 2025-06-05 12:29_

I've found out what the issue was. It's not ruff, but one of the tests was changing function attributes.
Issue was on my side. Thank you for help and advice.

---

_Comment by @AlexWaygood on 2025-06-05 12:31_

Glad that you fixed your issue!!

---

_Closed by @AlexWaygood on 2025-06-05 12:31_

---
