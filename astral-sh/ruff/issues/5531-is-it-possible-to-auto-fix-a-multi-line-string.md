```yaml
number: 5531
title: "Is it possible to auto-fix a multi-line string `format` call with a single-line string template?"
type: issue
state: closed
author: harupy
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-05T13:25:34Z
updated_at: 2023-07-10T13:49:14Z
url: https://github.com/astral-sh/ruff/issues/5531
synced_at: 2026-01-12T15:54:45Z
```

# Is it possible to auto-fix a multi-line string `format` call with a single-line string template?

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Not an issue, but a question. Is it possible to auto-fix a multi-line string `format` call with **_a single-line string template_** if the new code doesn't exceed the line length limit?

```python
# A multi-line string format call with a single-line string template
"a {} b".format(
    1
)

# Fixed code doesn't exceed the line length limit, can be auto-fixed 
"hello, I am a warning message: {}".format(
    1
)

# Fixed code exceeds the line length limit, can't be auto-fixed 
"{}{}{}".format(
    123123123123123123123123123123123123123,
    456456456456456456456456456456456456456,
    789789789789789789789789789789789789789,
)
```

It appears ruff currently ignores this case.


---

_Label `question` added by @charliermarsh on 2023-07-05 13:37_

---

_Comment by @harupy on 2023-07-05 13:45_

I'm testing this:

```diff
diff --git a/crates/ruff/src/checkers/ast/mod.rs b/crates/ruff/src/checkers/ast/mod.rs
index 1ed61170d..b6ffe6f32 100644
--- a/crates/ruff/src/checkers/ast/mod.rs
+++ b/crates/ruff/src/checkers/ast/mod.rs
@@ -2417,19 +2417,19 @@ where
                     if let Expr::Attribute(ast::ExprAttribute { value, attr, .. }) = func.as_ref() {
                         let attr = attr.as_str();
                         if let Expr::Constant(ast::ExprConstant {
-                            value: Constant::Str(value),
+                            value: Constant::Str(str),
                             ..
                         }) = value.as_ref()
                         {
                             if attr == "join" {
                                 // "...".join(...) call
                                 if self.enabled(Rule::StaticJoinToFString) {
-                                    flynt::rules::static_join_to_fstring(self, expr, value);
+                                    flynt::rules::static_join_to_fstring(self, expr, str);
                                 }
                             } else if attr == "format" {
                                 // "...".format(...) call
                                 let location = expr.range();
-                                match pyflakes::format::FormatSummary::try_from(value.as_ref()) {
+                                match pyflakes::format::FormatSummary::try_from(str.as_ref()) {
                                     Err(e) => {
                                         if self.enabled(Rule::StringDotFormatInvalidFormat) {
                                             self.diagnostics.push(Diagnostic::new(
@@ -2473,7 +2473,13 @@ where
                                         }
 
                                         if self.enabled(Rule::FString) {
-                                            pyupgrade::rules::f_strings(self, &summary, expr);
+                                            pyupgrade::rules::f_strings(
+                                                self,
+                                                &summary,
+                                                expr,
+                                                value,
+                                                self.settings.line_length,
+                                            );
                                         }
                                     }
                                 }
diff --git a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
index 942033505..dbdbd5f11 100644
--- a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
+++ b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
@@ -13,6 +13,7 @@ use ruff_python_ast::source_code::Locator;
 use ruff_python_ast::str::{is_implicit_concatenation, leading_quote, trailing_quote};
 
 use crate::checkers::ast::Checker;
+use crate::line_width::LineLength;
 use crate::registry::AsRule;
 use crate::rules::pyflakes::format::FormatSummary;
 use crate::rules::pyupgrade::helpers::curly_escape;
@@ -313,13 +314,19 @@ fn try_convert_to_f_string(expr: &Expr, locator: &Locator) -> Option<String> {
 }
 
 /// UP032
-pub(crate) fn f_strings(checker: &mut Checker, summary: &FormatSummary, expr: &Expr) {
+pub(crate) fn f_strings(
+    checker: &mut Checker,
+    summary: &FormatSummary,
+    expr: &Expr,
+    template: &Expr,
+    line_length: LineLength,
+) {
     if summary.has_nested_parts {
         return;
     }
 
     // Avoid refactoring multi-line strings.
-    if checker.locator.contains_line_break(expr.range()) {
+    if checker.locator.contains_line_break(template.range()) {
         return;
     }
 
@@ -329,9 +336,10 @@ pub(crate) fn f_strings(checker: &mut Checker, summary: &FormatSummary, expr: &E
         return;
     };
 
-    // Avoid refactors that increase the resulting string length.
-    let existing = checker.locator.slice(expr.range());
-    if contents.len() > existing.len() {
+    // Avoid refactors that exceed the line length limit.
+    if (expr.start() - checker.locator.line_start(expr.start())).to_usize() + contents.len()
+        > line_length.get()
+    {
         return;
     }
 

```

---

_Comment by @harupy on 2023-07-07 06:46_

@charliermarsh WDYT?

---

_Comment by @harupy on 2023-07-09 02:28_

Testing the change above in https://github.com/mlflow/mlflow. The results look correct:

```diff
diff --git a/mlflow/catboost.py b/mlflow/catboost.py
index 6a99aca83..93c0a3e22 100644
--- a/mlflow/catboost.py
+++ b/mlflow/catboost.py
@@ -264,9 +264,7 @@ def _init_model(model_type):
 
     if model_type not in model_types:
         raise TypeError(
-            "Invalid model type: '{}'. Must be one of {}".format(
-                model_type, list(model_types.keys())
-            )
+            f"Invalid model type: '{model_type}'. Must be one of {list(model_types.keys())}"
         )
 
     return model_types[model_type]()
diff --git a/mlflow/store/model_registry/dbmodels/models.py b/mlflow/store/model_registry/dbmodels/models.py
index a98fd1c8c..bdd01c3ab 100644
--- a/mlflow/store/model_registry/dbmodels/models.py
+++ b/mlflow/store/model_registry/dbmodels/models.py
@@ -167,9 +167,7 @@ class SqlModelVersionTag(Base):
     )
 
     def __repr__(self):
-        return "<SqlModelVersionTag ({}, {}, {}, {})>".format(
-            self.name, self.version, self.key, self.value
-        )
+        return f"<SqlModelVersionTag ({self.name}, {self.version}, {self.key}, {self.value})>"
 
     # entity mappers
     def to_mlflow_entity(self):
diff --git a/mlflow/store/tracking/dbmodels/models.py b/mlflow/store/tracking/dbmodels/models.py
index f19f82480..9a318d243 100644
--- a/mlflow/store/tracking/dbmodels/models.py
+++ b/mlflow/store/tracking/dbmodels/models.py
@@ -413,9 +413,7 @@ class SqlLatestMetric(Base):
     """
 
     def __repr__(self):
-        return "<SqlLatestMetric({}, {}, {}, {})>".format(
-            self.key, self.value, self.timestamp, self.step
-        )
+        return f"<SqlLatestMetric({self.key}, {self.value}, {self.timestamp}, {self.step})>"
 
     def to_mlflow_entity(self):
         """
diff --git a/mlflow/store/tracking/file_store.py b/mlflow/store/tracking/file_store.py
index 39e611d36..e488848da 100644
--- a/mlflow/store/tracking/file_store.py
+++ b/mlflow/store/tracking/file_store.py
@@ -1065,9 +1065,7 @@ class FileStore(AbstractStore):
 
         if not isinstance(mlflow_model, Model):
             raise TypeError(
-                "Argument 'mlflow_model' should be mlflow.models.Model, got '{}'".format(
-                    type(mlflow_model)
-                )
+                f"Argument 'mlflow_model' should be mlflow.models.Model, got '{type(mlflow_model)}'"
             )
         _validate_run_id(run_id)
         run_info = self._get_run_info(run_id)
diff --git a/mlflow/store/tracking/sqlalchemy_store.py b/mlflow/store/tracking/sqlalchemy_store.py
index c400080ca..bdf24b94c 100644
--- a/mlflow/store/tracking/sqlalchemy_store.py
+++ b/mlflow/store/tracking/sqlalchemy_store.py
@@ -1323,9 +1323,7 @@ class SqlAlchemyStore(AbstractStore):
 
         if not isinstance(mlflow_model, Model):
             raise TypeError(
-                "Argument 'mlflow_model' should be mlflow.models.Model, got '{}'".format(
-                    type(mlflow_model)
-                )
+                f"Argument 'mlflow_model' should be mlflow.models.Model, got '{type(mlflow_model)}'"
             )
         model_dict = mlflow_model.to_dict()
         with self.ManagedSessionMaker() as session:
diff --git a/mlflow/tracking/client.py b/mlflow/tracking/client.py
index fa77c32ca..eaf36ad48 100644
--- a/mlflow/tracking/client.py
+++ b/mlflow/tracking/client.py
@@ -1433,9 +1433,7 @@ class MlflowClient:
 
                 if (image.ndim == 3) and (image.shape[2] not in [1, 3, 4]):
                     raise ValueError(
-                        "Invalid channel length: {}. Must be one of [1, 3, 4]".format(
-                            image.shape[2]
-                        )
+                        f"Invalid channel length: {image.shape[2]}. Must be one of [1, 3, 4]"
                     )
 
                 # squeeze a 3D grayscale image since `Image.fromarray` doesn't accept it.
diff --git a/mlflow/types/utils.py b/mlflow/types/utils.py
index fe75fdd5a..d404b6947 100644
--- a/mlflow/types/utils.py
+++ b/mlflow/types/utils.py
@@ -58,9 +58,7 @@ def clean_tensor_type(dtype: np.dtype):
     """
     if not isinstance(dtype, np.dtype):
         raise TypeError(
-            "Expected `type` to be instance of `{}`, received `{}`".format(
-                np.dtype, dtype.__class__
-            )
+            f"Expected `type` to be instance of `{np.dtype}`, received `{dtype.__class__}`"
         )
 
     # Special casing for np.str_ and np.bytes_
@@ -383,9 +381,7 @@ def _validate_all_values_string(d):
 def _validate_keys_match(d, expected_keys):
     if d.keys() != expected_keys:
         raise MlflowException(
-            "Expected example to be dict with keys {}, got {}".format(
-                list(expected_keys), list(d.keys())
-            ),
+            f"Expected example to be dict with keys {list(expected_keys)}, got {list(d.keys())}",
             INVALID_PARAMETER_VALUE,
         )
 
diff --git a/mlflow/utils/__init__.py b/mlflow/utils/__init__.py
index b577dd194..5af08d489 100644
--- a/mlflow/utils/__init__.py
+++ b/mlflow/utils/__init__.py
@@ -11,9 +11,7 @@ import inspect
 _logger = logging.getLogger(__name__)
 
 
-PYTHON_VERSION = "{major}.{minor}.{micro}".format(
-    major=version_info.major, minor=version_info.minor, micro=version_info.micro
-)
+PYTHON_VERSION = f"{version_info.major}.{version_info.minor}.{version_info.micro}"
 
 
 _logger = logging.getLogger(__name__)
diff --git a/mlflow/utils/model_utils.py b/mlflow/utils/model_utils.py
index b955a895f..473db34b7 100644
--- a/mlflow/utils/model_utils.py
+++ b/mlflow/utils/model_utils.py
@@ -164,9 +164,7 @@ def _validate_onnx_session_options(onnx_session_options):
     if onnx_session_options is not None:
         if not isinstance(onnx_session_options, dict):
             raise TypeError(
-                "Argument onnx_session_options should be a dict, not {}".format(
-                    type(onnx_session_options)
-                )
+                f"Argument onnx_session_options should be a dict, not {type(onnx_session_options)}"
             )
         for key, value in onnx_session_options.items():
             if key != "extra_session_config" and not hasattr(ort.SessionOptions, key):
diff --git a/tests/pyfunc/test_model_export_with_class_and_artifacts.py b/tests/pyfunc/test_model_export_with_class_and_artifacts.py
index 322e571c4..fdc212ea6 100644
--- a/tests/pyfunc/test_model_export_with_class_and_artifacts.py
+++ b/tests/pyfunc/test_model_export_with_class_and_artifacts.py
@@ -226,9 +226,7 @@ def test_python_model_predict_compatible_without_params(sklearn_knn_model, iris_
     sklearn_artifact_path = "sk_model"
     with mlflow.start_run():
         mlflow.sklearn.log_model(sk_model=sklearn_knn_model, artifact_path=sklearn_artifact_path)
-        sklearn_model_uri = "runs:/{run_id}/{artifact_path}".format(
-            run_id=mlflow.active_run().info.run_id, artifact_path=sklearn_artifact_path
-        )
+        sklearn_model_uri = f"runs:/{mlflow.active_run().info.run_id}/{sklearn_artifact_path}"
 
     def test_predict(sk_model, model_input):
         return sk_model.predict(model_input) * 2
@@ -240,14 +238,10 @@ def test_python_model_predict_compatible_without_params(sklearn_knn_model, iris_
             artifacts={"sk_model": sklearn_model_uri},
             python_model=CustomSklearnModelWithoutParams(test_predict),
         )
-        pyfunc_model_uri = "runs:/{run_id}/{artifact_path}".format(
-            run_id=mlflow.active_run().info.run_id, artifact_path=pyfunc_artifact_path
-        )
+        pyfunc_model_uri = f"runs:/{mlflow.active_run().info.run_id}/{pyfunc_artifact_path}"
         assert model_info.model_uri == pyfunc_model_uri
         pyfunc_model_path = _download_artifact_from_uri(
-            "runs:/{run_id}/{artifact_path}".format(
-                run_id=mlflow.active_run().info.run_id, artifact_path=pyfunc_artifact_path
-            )
+            f"runs:/{mlflow.active_run().info.run_id}/{pyfunc_artifact_path}"
         )
         model_config = Model.load(os.path.join(pyfunc_model_path, "MLmodel"))
 
diff --git a/tests/store/tracking/test_sqlalchemy_store.py b/tests/store/tracking/test_sqlalchemy_store.py
index 1f002e0cc..933804bdf 100644
--- a/tests/store/tracking/test_sqlalchemy_store.py
+++ b/tests/store/tracking/test_sqlalchemy_store.py
@@ -1852,14 +1852,10 @@ class TestSqlAlchemyStore(unittest.TestCase, AbstractStoreTest):
             filter_string = f"attr.artifact_uri = '{expected_artifact_uri}'"
         assert self._search([e1, e2], filter_string) == [r1]
 
-        filter_string = "attr.artifact_uri = '{}/{}/{}/artifacts'".format(
-            ARTIFACT_URI, e1.upper(), r1.upper()
-        )
+        filter_string = f"attr.artifact_uri = '{ARTIFACT_URI}/{e1.upper()}/{r1.upper()}/artifacts'"
         assert self._search([e1, e2], filter_string) == []
 
-        filter_string = "attr.artifact_uri != '{}/{}/{}/artifacts'".format(
-            ARTIFACT_URI, e1.upper(), r1.upper()
-        )
+        filter_string = f"attr.artifact_uri != '{ARTIFACT_URI}/{e1.upper()}/{r1.upper()}/artifacts'"
         assert sorted(
             [r1, r2],
         ) == sorted(self._search([e1, e2], filter_string))

```

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:07_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:07_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:07_

---

_Comment by @charliermarsh on 2023-07-10 01:07_

@harupy - Are you up for submitting a PR? I agree that it should be supported.

---

_Label `needs-decision` removed by @charliermarsh on 2023-07-10 01:07_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:07_

---

_Comment by @harupy on 2023-07-10 02:27_

@charliermarsh Thanks, I'll file a PR!

---

_Closed by @charliermarsh on 2023-07-10 13:49_

---
