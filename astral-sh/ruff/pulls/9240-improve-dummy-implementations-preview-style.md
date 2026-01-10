```yaml
number: 9240
title: "Improve `dummy_implementations` preview style formatting"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dummy-implementations
created_at: 2023-12-22T03:12:21Z
updated_at: 2023-12-22T03:44:16Z
url: https://github.com/astral-sh/ruff/pull/9240
synced_at: 2026-01-10T23:07:18Z
```

# Improve `dummy_implementations` preview style formatting

---

_Pull request opened by @MichaReiser on 2023-12-22 03:12_

## Summary

Improve `dummy_implementations` preview style

* Only collapse dummy blocks if the parent is a function definition or class definition
* Don't insert empty lines between two function definitions if the preceding statement is a dummy definition and the two definitions aren't separated by an empty line. Not enforcing an empty lines allows grouping overloaded function definitions.


## Test Plan
* `cargo test`
* Verified that the similarity index are unchanged
* Manually reviewed the ecosystem changes


---

_Label `formatter` added by @MichaReiser on 2023-12-22 03:14_

---

_Label `preview` added by @MichaReiser on 2023-12-22 03:14_

---

_Comment by @github-actions[bot] on 2023-12-22 03:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+38 -68 lines in 23 files in 7 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -8 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/airflow/models/dag.py#L3971'>airflow/models/dag.py~L3971</a>
```diff
             default_args=default_args,
             schedule="0 0 * * *",
             dagrun_timeout=timedelta(minutes=60),
-        ) as dag: ...
+        ) as dag:
+            ...
 
     If you do this the context stores the DAG and whenever new task is created, it will use
     such stored DAG as the parent DAG.
```
<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/airflow/utils/log/secrets_masker.py#L359'>airflow/utils/log/secrets_masker.py~L359</a>
```diff
 
     Expected usage::
 
-        with contextlib.redirect_stdout(RedactedIO()): ...  # Writes to stdout will be redacted.
+        with contextlib.redirect_stdout(RedactedIO()):
+            ...  # Writes to stdout will be redacted.
     """
 
     def __init__(self):
```
<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/tests/decorators/test_bash.py#L39'>tests/decorators/test_bash.py~L39</a>
```diff
     def setup(self, dag_maker):
         self.dag_maker = dag_maker
 
-        with dag_maker(dag_id="bash_deco_dag") as dag: ...
+        with dag_maker(dag_id="bash_deco_dag") as dag:
+            ...
 
         self.dag = dag
 
```
<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/tests/models/test_dagrun.py#L2599'>tests/models/test_dagrun.py~L2599</a>
```diff
 )
 def test_dag_run_id_config(session, dag_maker, pattern, run_id, result):
     with conf_vars({("scheduler", "allowed_run_id_pattern"): pattern}):
-        with dag_maker(): ...
+        with dag_maker():
+            ...
         if result:
             dag_maker.create_dagrun(run_id=run_id)
         else:
```
<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/tests/providers/amazon/aws/hooks/test_hooks_signature.py#L159'>tests/providers/amazon/aws/hooks/test_hooks_signature.py~L159</a>
```diff
                 self.spam = spam
 
             def method1(self):
-                if self.foo == "bar": ...
+                if self.foo == "bar":
+                    ...
 
             def method2(self):
-                if self.spam == "egg": ...
+                if self.spam == "egg":
+                    ...
 
     .. code-block:: python
 
```
<a href='https://github.com/apache/airflow/blob/1e6fa735752d61125903f0709b12cc1338789c5d/tests/providers/amazon/aws/hooks/test_hooks_signature.py#L173'>tests/providers/amazon/aws/hooks/test_hooks_signature.py~L173</a>
```diff
                 super().__init__(*args, **kwargs)
 
             def method1(self, foo: str):
-                if foo == "bar": ...
+                if foo == "bar":
+                    ...
 
             def method2(self, spam: str):
-                if spam == "egg": ...
+                if spam == "egg":
+                    ...
 
     """
     hooks = get_aws_hooks_from_module(hook_module)
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -36 lines across 9 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/enum.py#L55'>src/bokeh/core/property/enum.py~L55</a>
```diff
 
     @overload
     def __init__(self, enum: enums.Enumeration, *, default: Init[str] = ..., help: str | None = ...) -> None: ...
-
     @overload
     def __init__(self, enum: str, *values: str, default: Init[str] = ..., help: str | None = ...) -> None: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/standalone.py#L143'>src/bokeh/embed/standalone.py~L143</a>
```diff
     wrap_plot_info: Literal[True] = ...,
     theme: ThemeLike = ...,
 ) -> tuple[str, str]: ...
-
-
 @overload
 def components(models: Model, wrap_script: bool = ..., wrap_plot_info: Literal[False] = ..., theme: ThemeLike = ...) -> tuple[str, RenderRoot]: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/standalone.py#L156'>src/bokeh/embed/standalone.py~L156</a>
```diff
     wrap_plot_info: Literal[True] = ...,
     theme: ThemeLike = ...,
 ) -> tuple[str, Sequence[str]]: ...
-
-
 @overload
 def components(
     models: Sequence[Model], wrap_script: bool = ..., wrap_plot_info: Literal[False] = ..., theme: ThemeLike = ...
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/standalone.py#L171'>src/bokeh/embed/standalone.py~L171</a>
```diff
     wrap_plot_info: Literal[True] = ...,
     theme: ThemeLike = ...,
 ) -> tuple[str, dict[str, str]]: ...
-
-
 @overload
 def components(
     models: dict[str, Model], wrap_script: bool = ..., wrap_plot_info: Literal[False] = ..., theme: ThemeLike = ...
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/io/notebook.py#L609'>src/bokeh/io/notebook.py~L609</a>
```diff
 
 @overload
 def show_doc(obj: Model, state: State) -> None: ...
-
-
 @overload
 def show_doc(obj: Model, state: State, notebook_handle: CommsHandle) -> CommsHandle: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L87'>src/bokeh/layouts.py~L87</a>
```diff
 
 @overload
 def row(children: list[UIElement], *, sizing_mode: SizingModeType | None = None, **kwargs: Any) -> Row: ...
-
-
 @overload
 def row(*children: UIElement, sizing_mode: SizingModeType | None = None, **kwargs: Any) -> Row: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L126'>src/bokeh/layouts.py~L126</a>
```diff
 
 @overload
 def column(children: list[UIElement], *, sizing_mode: SizingModeType | None = None, **kwargs: Any) -> Column: ...
-
-
 @overload
 def column(*children: UIElement, sizing_mode: SizingModeType | None = None, **kwargs: Any) -> Column: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L375'>src/bokeh/layouts.py~L375</a>
```diff
 # XXX https://github.com/python/mypy/issues/731
 @overload
 def grid(children: list[UIElement | list[UIElement | list[Any]]], *, sizing_mode: SizingModeType | None = ...) -> GridBox: ...
-
-
 @overload
 def grid(children: Row | Column, *, sizing_mode: SizingModeType | None = ...) -> GridBox: ...
-
-
 @overload
 def grid(children: list[UIElement | None], *, sizing_mode: SizingModeType | None = ..., nrows: int) -> GridBox: ...
-
-
 @overload
 def grid(children: list[UIElement | None], *, sizing_mode: SizingModeType | None = ..., ncols: int) -> GridBox: ...
-
-
 @overload
 def grid(children: list[UIElement | None], *, sizing_mode: SizingModeType | None = ..., nrows: int, ncols: int) -> GridBox: ...
-
-
 @overload
 def grid(children: str, *, sizing_mode: SizingModeType | None = ...) -> GridBox: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/plots.py#L326'>src/bokeh/models/plots.py~L326</a>
```diff
 
     @overload
     def add_glyph(self, glyph: Glyph, **kwargs: Any) -> GlyphRenderer: ...
-
     @overload
     def add_glyph(self, source: ColumnarDataSource, glyph: Glyph, **kwargs: Any) -> GlyphRenderer: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/sources.py#L234'>src/bokeh/models/sources.py~L234</a>
```diff
 
     @overload
     def __init__(self, data: DataDict | pd.DataFrame | pd.core.groupby.GroupBy, **kwargs: TAny) -> None: ...
-
     @overload
     def __init__(self, **kwargs: TAny) -> None: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/protocol/__init__.py#L101'>src/bokeh/protocol/__init__.py~L101</a>
```diff
 
     @overload
     def create(self, msgtype: Literal["ACK"], **metadata: Any) -> ack: ...
-
     @overload
     def create(self, msgtype: Literal["ERROR"], request_id: ID, text: str, **metadata: Any) -> error: ...
-
     @overload
     def create(self, msgtype: Literal["OK"], request_id: ID, **metadata: Any) -> ok: ...
-
     @overload
     def create(self, msgtype: Literal["PATCH-DOC"], events: list[DocumentPatchedEvent], **metadata: Any) -> patch_doc: ...
-
     @overload
     def create(self, msgtype: Literal["PULL-DOC-REPLY"], request_id: ID, document: Document, **metadata: Any) -> pull_doc_reply: ...
-
     @overload
     def create(self, msgtype: Literal["PULL-DOC-REQ"], **metadata: Any) -> pull_doc_req: ...
-
     @overload
     def create(self, msgtype: Literal["PUSH-DOC"], document: Document, **metadata: Any) -> push_doc: ...
-
     @overload
     def create(self, msgtype: Literal["SERVER-INFO-REPLY"], request_id: ID, **metadata: Any) -> server_info_reply: ...
-
     @overload
     def create(self, msgtype: Literal["SERVER-INFO-REQ"], **metadata: Any) -> server_info_req: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/themes/theme.py#L151'>src/bokeh/themes/theme.py~L151</a>
```diff
 
     @overload
     def __init__(self, filename: PathLike) -> None: ...
-
     @overload
     def __init__(self, json: dict[str, Any]) -> None: ...
 
```
<a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/util/strings.py#L114'>src/bokeh/util/strings.py~L114</a>
```diff
 
 @overload
 def format_docstring(docstring: None, *args: Any, **kwargs: Any) -> None: ...
-
-
 @overload
 def format_docstring(docstring: str, *args: Any, **kwargs: Any) -> str: ...
 
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/effd4610dcd1ee80e522702731f34756fdd933b5/ibis/backends/base/sql/__init__.py#L160'>ibis/backends/base/sql/__init__.py~L160</a>
```diff
         Or you can use a context manager:
 
         ```python
-        with con.raw_sql("SELECT ...") as cursor: ...
+        with con.raw_sql("SELECT ...") as cursor:
+            ...
         ```
         :::
 
```
<a href='https://github.com/ibis-project/ibis/blob/effd4610dcd1ee80e522702731f34756fdd933b5/ibis/backends/base/sql/alchemy/__init__.py#L628'>ibis/backends/base/sql/alchemy/__init__.py~L628</a>
```diff
         Or you can use a context manager:
 
         ```python
-        with con.raw_sql("SELECT ...") as cursor: ...
+        with con.raw_sql("SELECT ...") as cursor:
+            ...
         ```
         :::
 
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+14 -7 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/docker_utils/__init__.py#L252'>latch_cli/docker_utils/__init__.py~L252</a>
```diff
 
                 has_buildable_pyproject = True
                 break
-    except FileNotFoundError: ...
+    except FileNotFoundError:
+        ...
 
     # from https://peps.python.org/pep-0518/ and https://peps.python.org/pep-0621/
     if has_setup_py or has_buildable_pyproject:
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/services/get_executions.py#L221'>latch_cli/services/get_executions.py~L221</a>
```diff
                 prev = (curr_selected, hor_index, term_width, term_height)
                 menus.clear_screen()
                 max_row_len = render(curr_selected, hor_index, term_width, term_height)
-    except KeyboardInterrupt: ...
+    except KeyboardInterrupt:
+        ...
     finally:
         menus.clear_screen()
         menus.reveal_cursor()
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/services/get_executions.py#L314'>latch_cli/services/get_executions.py~L314</a>
```diff
                 menus.clear_screen()
                 prev = (curr_selected, term_width, term_height)
                 render(curr_selected, term_width, term_height)
-    except KeyboardInterrupt: ...
+    except KeyboardInterrupt:
+        ...
     finally:
         menus.clear_screen()
         menus.move_cursor((0, 0))
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/services/get_executions.py#L450'>latch_cli/services/get_executions.py~L450</a>
```diff
                 menus.clear_screen()
                 prev_term_dims = (vert_index, hor_index, term_width, term_height)
                 render(vert_index, hor_index, term_width, term_height)
-    except KeyboardInterrupt: ...
+    except KeyboardInterrupt:
+        ...
     finally:
         log_sched.shutdown()
         log_file.unlink(missing_ok=True)
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/services/get_executions.py#L511'>latch_cli/services/get_executions.py~L511</a>
```diff
             if prev_term_dims != (term_width, term_height):
                 prev_term_dims = (term_width, term_height)
                 render(term_width, term_height)
-    except KeyboardInterrupt: ...
+    except KeyboardInterrupt:
+        ...
     finally:
         menus.clear_screen()
         menus.move_cursor((0, 0))
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/services/local_dev_old.py#L313'>latch_cli/services/local_dev_old.py~L313</a>
```diff
     try:
         io_task = asyncio.gather(input_task(), output_task(), resize_task())
         await io_task
-    except asyncio.CancelledError: ...
+    except asyncio.CancelledError:
+        ...
     finally:
         termios.tcsetattr(sys.stdin.fileno(), termios.TCSANOW, old_settings_stdin)
         signal.signal(signal.SIGWINCH, old_sigwinch_handler)
```
<a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/utils/__init__.py#L202'>latch_cli/utils/__init__.py~L202</a>
```diff
                     continue
 
                 exclude.append(l)
-    except FileNotFoundError: ...
+    except FileNotFoundError:
+        ...
 
     from docker.utils import exclude_paths
 
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/16933b838eef7be92ee02f66b87aa1a7532cee63/test_cases/stdlib/check_pathlib.py#L2'>test_cases/stdlib/check_pathlib.py~L2</a>
```diff
 
 from pathlib import Path, PureWindowsPath
 
-if Path("asdf") == Path("asdf"): ...
+if Path("asdf") == Path("asdf"):
+    ...
 
 # https://github.com/python/typeshed/issues/10661
 # Provide a true positive error when comparing Path to str
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/db/test_savepoints.py#L44'>rotkehlchen/tests/db/test_savepoints.py~L44</a>
```diff
         conn.release_savepoint()
 
     conn._enter_savepoint("point")
-    with pytest.raises(ContextError), conn._enter_savepoint("point"): ...
+    with pytest.raises(ContextError), conn._enter_savepoint("point"):
+        ...
 
     with pytest.raises(ContextError):
         conn.rollback_savepoint("abc")
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+0 -13 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/environment/__init__.py#L101'>sphinx/environment/__init__.py~L101</a>
```diff
     class _DomainsType(MutableMapping[str, Domain]):
         @overload
         def __getitem__(self, key: Literal["c"]) -> CDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["cpp"]) -> CPPDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["changeset"]) -> ChangeSetDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["citation"]) -> CitationDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["index"]) -> IndexDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["js"]) -> JavaScriptDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["math"]) -> MathDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["py"]) -> PythonDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["rst"]) -> ReSTDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["std"]) -> StandardDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["duration"]) -> DurationDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: Literal["todo"]) -> TodoDomain: ...  # NoQA: E704
-
         @overload
         def __getitem__(self, key: str) -> Domain: ...  # NoQA: E704
-
         def __getitem__(self, key):
             raise NotImplementedError  # NoQA: E704
 
```

</p>
</details>




---

_Review requested from @konstin by @MichaReiser on 2023-12-22 03:31_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-22 03:31_

---

_Marked ready for review by @MichaReiser on 2023-12-22 03:31_

---

_@charliermarsh approved on 2023-12-22 03:34_

---

_Merged by @MichaReiser on 2023-12-22 03:44_

---

_Closed by @MichaReiser on 2023-12-22 03:44_

---

_Branch deleted on 2023-12-22 03:44_

---
