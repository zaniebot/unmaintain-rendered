---
number: 13011
title: "[red-knot] packages installed by uv fail resolving to a module"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
created_at: 2024-08-20T15:12:00Z
updated_at: 2025-04-04T20:44:44Z
url: https://github.com/astral-sh/ruff/issues/13011
synced_at: 2026-01-07T13:12:15-06:00
---

# [red-knot] packages installed by uv fail resolving to a module

---

_Issue opened by @MichaReiser on 2024-08-20 15:12_

I used black to test this:

* Clone black
* Install the dependencies listed by mypy-primer: `uv pip install aiohttp click pathspec tomli  platformdirs packaging`
* Run Red Knot from the ruff directory with `cargo run --bin red_knot -- --current-directory=../black -vv --venv-path ../black/.venv/
`


```
2024-08-20 16:59:18.065251480 DEBUG Searching for workspace in '/home/micha/astral/black'
2024-08-20 16:59:18.065424219 DEBUG Attempting to parse virtual environment metadata at /home/micha/astral/black/.venv/pyvenv.cfg
2024-08-20 16:59:18.065507259 DEBUG Searching for site-packages directory in `sys.prefix` path /home/micha/astral/black/.venv
2024-08-20 16:59:18.065548759 DEBUG Resolved site-packages directories for this virtual environment are: ["/home/micha/astral/black/.venv/lib/python3.12/site-packages"]
2024-08-20 16:59:18.065812788 INFO Target version: 3.8
2024-08-20 16:59:18.065858488 DEBUG Adding static search path '/home/micha/astral/black'
2024-08-20 16:59:18.065932048 DEBUG Adding site-package path '/home/micha/astral/black/.venv/lib/python3.12/site-packages'
2024-08-20 16:59:18.066177617 DEBUG Starting main loop
2024-08-20 16:59:18.067171654 DEBUG Waiting for next main loop message.
2024-08-20 16:59:18.067282073 DEBUG Checking workspace
2024-08-20 16:59:18.067466473 DEBUG Checking package /home/micha/astral/black
2024-08-20 16:59:18.079524922 INFO Indexed 277 files for package 'black'
2024-08-20 16:59:18.079658642 DEBUG Checking file /home/micha/astral/black/src/black/lines.py
2024-08-20 16:59:18.094895321 DEBUG Resolved module 'itertools' to 'vendored://stdlib/itertools.pyi'.
2024-08-20 16:59:18.095079480 DEBUG Resolved module 'math' to 'vendored://stdlib/math.pyi'.
2024-08-20 16:59:18.095239220 DEBUG Resolved module 'dataclasses' to 'vendored://stdlib/dataclasses.pyi'.
2024-08-20 16:59:18.100269214 DEBUG Resolved module 'typing' to 'vendored://stdlib/typing.pyi'.
2024-08-20 16:59:18.120247046 DEBUG Resolved module 'builtins' to 'vendored://stdlib/builtins.pyi'.
2024-08-20 16:59:18.173641168 DEBUG Resolved module 'collections.abc' to 'vendored://stdlib/collections/abc.pyi'.
2024-08-20 16:59:18.176923056 DEBUG Resolving dynamic module resolution paths
2024-08-20 16:59:18.177091826 DEBUG Adding editable installation to module resolution path /home/micha/astral/black/src
2024-08-20 16:59:18.177234815 DEBUG Resolved module 'black.brackets' to '/home/micha/astral/black/src/black/brackets.py'.
2024-08-20 16:59:18.181370692 DEBUG Resolved module 'black.mode' to '/home/micha/astral/black/src/black/mode.py'.
2024-08-20 16:59:18.184211353 DEBUG Resolved module 'enum' to 'vendored://stdlib/enum.pyi'.
2024-08-20 16:59:18.190560312 DEBUG Resolved module 'black.nodes' to '/home/micha/astral/black/src/black/nodes.py'.
2024-08-20 16:59:18.201385585 DEBUG Resolved module 'blib2to3.pgen2' to '/home/micha/astral/black/src/blib2to3/pgen2/__init__.py'.
2024-08-20 16:59:18.203056430 DEBUG Resolved module 'blib2to3' to '/home/micha/astral/black/src/blib2to3/__init__.py'.
2024-08-20 16:59:18.203674348 DEBUG Resolved module 'blib2to3.pytree' to '/home/micha/astral/black/src/blib2to3/pytree.py'.
2024-08-20 16:59:18.216170146 DEBUG Resolved module 'black.strings' to '/home/micha/astral/black/src/black/strings.py'.
2024-08-20 16:59:18.243074876 DEBUG Resolved module '_typeshed' to 'vendored://stdlib/_typeshed/__init__.pyi'.
2024-08-20 16:59:18.251812406 DEBUG Resolved module 'types' to 'vendored://stdlib/types.pyi'.
2024-08-20 16:59:18.264188346 DEBUG Resolved module 'typing_extensions' to 'vendored://stdlib/typing_extensions.pyi'.
2024-08-20 16:59:18.306658834 DEBUG Checking file /home/micha/astral/black/tests/data/cases/preview_hug_parens_with_braces_and_square_brackets_no_ll1.py
2024-08-20 16:59:18.310508060 DEBUG Checking file /home/micha/astral/black/tests/data/cases/bracketmatch.py
2024-08-20 16:59:18.312287704 DEBUG Checking file /home/micha/astral/black/profiling/dict_huge.py
2024-08-20 16:59:19.032030798 DEBUG Checking file /home/micha/astral/black/tests/data/jupyter/notebook_no_trailing_newline.ipynb
2024-08-20 16:59:19.033466554 DEBUG Checking file /home/micha/astral/black/tests/data/cases/linelength6.py
2024-08-20 16:59:19.033866562 DEBUG Checking file /home/micha/astral/black/tests/data/cases/prefer_rhs_split_reformatted.py
2024-08-20 16:59:19.034952929 DEBUG Checking file /home/micha/astral/black/profiling/mix_big.py
2024-08-20 16:59:19.161122956 DEBUG Checking file /home/micha/astral/black/tests/data/jupyter/notebook_empty_metadata.ipynb
2024-08-20 16:59:19.161707584 DEBUG Checking file /home/micha/astral/black/tests/data/cases/import_spacing.py
2024-08-20 16:59:19.162747351 DEBUG Resolved module 'logging' to 'vendored://stdlib/logging/__init__.pyi'.
2024-08-20 16:59:19.174090333 DEBUG Resolved module 'sys' to 'vendored://stdlib/sys/__init__.pyi'.
2024-08-20 16:59:19.174362742 DEBUG Resolved module 'tests.data.cases.import_spacing' to '/home/micha/astral/black/tests/data/cases/import_spacing.py'.
2024-08-20 16:59:19.174613431 DEBUG Module 'tests.data.cases.base_events' not found in the search paths.
2024-08-20 16:59:19.174865420 DEBUG Module 'tests.data.cases.coroutines' not found in the search paths.
2024-08-20 16:59:19.175107970 DEBUG Module 'tests.data.cases.events' not found in the search paths.
2024-08-20 16:59:19.175338769 DEBUG Module 'tests.data.cases.futures' not found in the search paths.
2024-08-20 16:59:19.175571188 DEBUG Module 'tests.data.cases.locks' not found in the search paths.
2024-08-20 16:59:19.175806187 DEBUG Module 'tests.data.cases.protocols' not found in the search paths.
2024-08-20 16:59:19.176023947 DEBUG Module 'tests.data.runners' not found in the search paths.
2024-08-20 16:59:19.176239326 DEBUG Module 'tests.data.queues' not found in the search paths.
2024-08-20 16:59:19.176461505 DEBUG Module 'tests.data.streams' not found in the search paths.
2024-08-20 16:59:19.176710424 DEBUG Module 'some_library' not found in the search paths.
2024-08-20 16:59:19.177577222 DEBUG Module 'name_of_a_company.extremely_long_project_name.component.ttypes' not found in the search paths.
2024-08-20 16:59:19.177748271 DEBUG Module 'name_of_a_company.extremely_long_project_name.extremely_long_component_name.ttypes' not found in the search paths.
2024-08-20 16:59:19.177969320 DEBUG Module 'tests.data.cases.a.b.c.subprocess' not found in the search paths.
2024-08-20 16:59:19.178175870 DEBUG Module 'tests.data.cases' not found in the search paths.
2024-08-20 16:59:19.181821398 DEBUG Checking file /home/micha/astral/black/tests/data/cases/remove_for_brackets.py
2024-08-20 16:59:19.183914501 DEBUG Checking file /home/micha/astral/black/tests/test_no_ipynb.py
2024-08-20 16:59:19.184656698 DEBUG Resolved module 'pathlib' to 'vendored://stdlib/pathlib.pyi'.
2024-08-20 16:59:19.184952697 DEBUG Resolved module 'pytest' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/pytest/__init__.py'.
2024-08-20 16:59:19.185164156 DEBUG Resolved module 'click.testing' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/testing.py'.
2024-08-20 16:59:19.190536508 DEBUG Resolved module 'black' to '/home/micha/astral/black/src/black/__init__.py'.
2024-08-20 16:59:19.206788334 DEBUG Resolved module 'black.handle_ipynb_magics' to '/home/micha/astral/black/src/black/handle_ipynb_magics.py'.
2024-08-20 16:59:19.210888981 DEBUG Resolved module 'functools' to 'vendored://stdlib/functools.pyi'.
2024-08-20 16:59:19.216441541 DEBUG Resolved module 'click' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py'.
2024-08-20 16:59:19.217243859 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.217386688 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.217605007 DEBUG Resolved module 'black.const' to '/home/micha/astral/black/src/black/const.py'.
2024-08-20 16:59:19.218122556 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.218245345 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.218363705 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.218460505 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.220783447 DEBUG Resolved module 're' to 'vendored://stdlib/re.pyi'.
2024-08-20 16:59:19.228374032 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.228716351 DEBUG Resolved module '_black_version' to '/home/micha/astral/black/src/_black_version.py'.
2024-08-20 16:59:19.236492925 DEBUG Resolved module 'os' to 'vendored://stdlib/os/__init__.pyi'.
2024-08-20 16:59:19.271307288 DEBUG Resolved module 'platform' to 'vendored://stdlib/platform.pyi'.
2024-08-20 16:59:19.273239241 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.273386701 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.273491920 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.273930889 DEBUG Failed to resolve file File { [salsa id]: Id(408), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/click/__init__.py"), permissions: Some(33188), revision: FileRevision(31776459385816825380651368099), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.275675123 DEBUG Resolved module 'tests.util' to '/home/micha/astral/black/tests/util.py'.
2024-08-20 16:59:19.281222065 DEBUG Resolved module '_pytest.mark' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/mark/__init__.py'.
2024-08-20 16:59:19.284353215 DEBUG Failed to resolve file File { [salsa id]: Id(458), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/mark/__init__.py"), permissions: Some(33188), revision: FileRevision(31805140660203790096836446635), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module
2024-08-20 16:59:19.286279339 DEBUG Checking file /home/micha/astral/black/tests/data/nested_gitignore_tests/x.py
2024-08-20 16:59:19.286519818 DEBUG Checking file /home/micha/astral/black/tests/data/cases/module_docstring_1.py
2024-08-20 16:59:19.287103536 DEBUG Checking file /home/micha/astral/black/tests/data/cases/no_blank_line_before_docstring.py
2024-08-20 16:59:19.288637880 DEBUG Checking file /home/micha/astral/black/tests/optional.py
2024-08-20 16:59:19.290911622 DEBUG Resolved module '_pytest.stash' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/stash.py'.
2024-08-20 16:59:19.292303628 DEBUG Module '_pytest.store' not found in the search paths.
2024-08-20 16:59:19.292823396 DEBUG Resolved module '_pytest.config' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/config/__init__.py'.
2024-08-20 16:59:19.310887056 DEBUG Resolved module '_pytest.config.argparsing' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/config/argparsing.py'.
2024-08-20 16:59:19.317633624 DEBUG Resolved module '_pytest.mark.structures' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/mark/structures.py'.
2024-08-20 16:59:19.324712560 DEBUG Resolved module '_pytest.nodes' to '/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/nodes.py'.
2024-08-20 16:59:19.332191135 DEBUG Resolved module 'abc' to 'vendored://stdlib/abc.pyi'.
2024-08-20 16:59:19.339355691 DEBUG Checking file /home/micha/astral/black/tests/data/cases/pep_604.py
2024-08-20 16:59:19.340468787 DEBUG Checking file /home/micha/astral/black/tests/data/cases/pattern_matching_complex.py
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:143:25:
no entry found for key
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting
fish: Job 1, 'cargo run --bin red_knot -- --c…' terminated by signal SIGABRT (Abort)

```

We should also improve the tracing log

---

_Label `bug` added by @MichaReiser on 2024-08-20 15:12_

---

_Label `red-knot` added by @MichaReiser on 2024-08-20 15:12_

---

_Renamed from "Packages installed by uv fail to resolve with: *Failed to resolve file*" to "Packages installed by uv fail resolving to a module" by @MichaReiser on 2024-08-20 15:12_

---

_Referenced in [astral-sh/ruff#13015](../../astral-sh/ruff/pulls/13015.md) on 2024-08-20 16:59_

---

_Comment by @MichaReiser on 2024-08-20 18:26_

I'm actually not sure if this is a bug. It's just something we should look into

---

_Comment by @AlexWaygood on 2024-08-20 18:31_

> I'm actually not sure if this is a bug. It's just something we should look into

These look suspicious to me:

> 2024-08-20 16:59:19.284353215 DEBUG Failed to resolve file File { [salsa id]: Id(458), path: System("/home/micha/astral/black/.venv/lib/python3.12/site-packages/_pytest/mark/__init__.py"), permissions: Some(33188), revision: FileRevision(31805140660203790096836446635), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } } to a module



---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Renamed from "Packages installed by uv fail resolving to a module" to "[red-knot] packages installed by uv fail resolving to a module" by @carljm on 2025-03-27 19:03_

---

_Comment by @MichaReiser on 2025-04-04 11:32_

Running this on the latest version gives:

```
../ruff/target/release/red_knot check -vv --python .venv/ src --python-version 3.13 | tail
2025-04-04 13:31:34.461356000 DEBUG Version: 0.0.0+2875 (ffa824e03 2025-04-04)
2025-04-04 13:31:34.461618000 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-04-04 13:31:34.461663000 DEBUG Searching for a project in '/Users/micha/astral/black'
2025-04-04 13:31:34.462095000 DEBUG Resolving requires-python constraint: `>=3.9`
2025-04-04 13:31:34.462122000 DEBUG Resolved requires-python constraint to: 3.9
2025-04-04 13:31:34.462145000 DEBUG Project without `tool.knot` section: '/Users/micha/astral/black'
2025-04-04 13:31:34.462167000 DEBUG Searching for a user-level configuration at `/Users/micha/.config/knot/knot.toml`
2025-04-04 13:31:34.462213000 INFO Python version: Python 3.13, platform: all
2025-04-04 13:31:34.462237000 DEBUG Adding first-party search path '/Users/micha/astral/black'
2025-04-04 13:31:34.462258000 DEBUG Adding first-party search path '/Users/micha/astral/black/src'
2025-04-04 13:31:34.462263000 DEBUG Using vendored stdlib
2025-04-04 13:31:34.462713000 DEBUG Discovering site-packages paths from sys-prefix `/Users/micha/astral/black/.venv` (`--python` argument')
2025-04-04 13:31:34.462764000 DEBUG Attempting to parse virtual environment metadata at '/Users/micha/astral/black/.venv/pyvenv.cfg'
2025-04-04 13:31:34.462829000 DEBUG Searching for site-packages directory in `sys.prefix` path `/Users/micha/astral/black/.venv`
2025-04-04 13:31:34.462840000 DEBUG Resolved site-packages directories for this virtual environment are: ["/Users/micha/astral/black/.venv/lib/python3.13/site-packages"]
2025-04-04 13:31:34.462850000 DEBUG Adding site-packages search path '/Users/micha/astral/black/.venv/lib/python3.13/site-packages'
2025-04-04 13:31:34.463026000 DEBUG Setting included paths: 1
2025-04-04 13:31:34.463048000 DEBUG Reloading files for project `black`
2025-04-04 13:31:34.463108000 DEBUG Starting main loop
2025-04-04 13:31:34.463463000 DEBUG Waiting for next main loop message.
2025-04-04 13:31:34.463752000 DEBUG Checking project 'black'
2025-04-04 13:31:34.468949000 INFO Indexed 40 file(s)
2025-04-04 13:31:34.469364000 DEBUG Checking file '/Users/micha/astral/black/src/black/resources/__init__.py'
2025-04-04 13:31:34.469378000 DEBUG Checking file '/Users/micha/astral/black/src/black/const.py'
2025-04-04 13:31:34.469365000 DEBUG Checking file '/Users/micha/astral/black/src/black/__main__.py'
2025-04-04 13:31:34.469370000 DEBUG Checking file '/Users/micha/astral/black/src/blackd/middlewares.py'
2025-04-04 13:31:34.469368000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/__init__.py'
2025-04-04 13:31:34.469551000 DEBUG Checking file '/Users/micha/astral/black/src/black/concurrency.py'
2025-04-04 13:31:34.469593000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pygram.py'
2025-04-04 13:31:34.469585000 DEBUG Checking file '/Users/micha/astral/black/src/black/parsing.py'
2025-04-04 13:31:34.469652000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/conv.py'
2025-04-04 13:31:34.469765000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/tokenize.py'
2025-04-04 13:31:34.469888000 DEBUG Checking file '/Users/micha/astral/black/src/black/schema.py'
2025-04-04 13:31:34.469843000 DEBUG Checking file '/Users/micha/astral/black/src/black/brackets.py'
2025-04-04 13:31:34.470065000 DEBUG Checking file '/Users/micha/astral/black/src/black/comments.py'
2025-04-04 13:31:34.469847000 DEBUG Checking file '/Users/micha/astral/black/src/black/_width_table.py'
2025-04-04 13:31:34.469915000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/__init__.py'
2025-04-04 13:31:34.470451000 DEBUG Checking file '/Users/micha/astral/black/src/black/handle_ipynb_magics.py'
2025-04-04 13:31:34.470727000 DEBUG Resolving dynamic module resolution paths
2025-04-04 13:31:34.470874000 DEBUG Checking file '/Users/micha/astral/black/src/black/__init__.py'
2025-04-04 13:31:34.471106000 DEBUG Module `pgen2` not found in search paths
2025-04-04 13:31:34.471326000 DEBUG Checking file '/Users/micha/astral/black/src/black/trans.py'
2025-04-04 13:31:34.476588000 DEBUG Checking file '/Users/micha/astral/black/src/black/output.py'
2025-04-04 13:31:34.476756000 DEBUG Checking file '/Users/micha/astral/black/src/blackd/__main__.py'
2025-04-04 13:31:34.477453000 DEBUG Checking file '/Users/micha/astral/black/src/black/report.py'
2025-04-04 13:31:34.478468000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pytree.py'
2025-04-04 13:31:34.479148000 DEBUG Checking file '/Users/micha/astral/black/src/black/nodes.py'
2025-04-04 13:31:34.483696000 DEBUG Module `collections.abc.Set` not found in search paths
2025-04-04 13:31:34.484750000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/parse.py'
2025-04-04 13:31:34.485074000 DEBUG Checking file '/Users/micha/astral/black/src/black/ranges.py'
2025-04-04 13:31:34.486055000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/literals.py'
2025-04-04 13:31:34.486418000 DEBUG Checking file '/Users/micha/astral/black/src/black/lines.py'
2025-04-04 13:31:34.487014000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/token.py'
2025-04-04 13:31:34.487131000 DEBUG Checking file '/Users/micha/astral/black/src/black/cache.py'
2025-04-04 13:31:34.489596000 DEBUG Module `IPython.core.inputtransformer2` not found in search paths
2025-04-04 13:31:34.489802000 DEBUG Checking file '/Users/micha/astral/black/src/black/rusty.py'
2025-04-04 13:31:34.489923000 DEBUG Checking file '/Users/micha/astral/black/src/black/numerics.py'
2025-04-04 13:31:34.490108000 DEBUG Checking file '/Users/micha/astral/black/src/black/files.py'
2025-04-04 13:31:34.490235000 DEBUG Checking file '/Users/micha/astral/black/src/black/strings.py'
2025-04-04 13:31:34.490264000 DEBUG Checking file '/Users/micha/astral/black/src/blackd/__init__.py'
2025-04-04 13:31:34.490380000 DEBUG Checking file '/Users/micha/astral/black/src/black/linegen.py'
2025-04-04 13:31:34.490733000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/driver.py'
2025-04-04 13:31:34.491160000 DEBUG Checking file '/Users/micha/astral/black/src/black/debug.py'
2025-04-04 13:31:34.491558000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/grammar.py'
2025-04-04 13:31:34.492003000 DEBUG Checking file '/Users/micha/astral/black/src/black/mode.py'
2025-04-04 13:31:34.492049000 DEBUG Module `hashlib.sha256` not found in search paths
2025-04-04 13:31:34.492669000 DEBUG Checking file '/Users/micha/astral/black/src/blib2to3/pgen2/pgen.py'
2025-04-04 13:31:34.519215000 DEBUG Exiting main loop
```

which mostly looks good 


---

_Comment by @MichaReiser on 2025-04-04 16:33_

The `hashlib` import fails because `sha256` is only listed in `__all__` and we don't support all yet. 

https://github.com/python/typeshed/blob/1c17cd429c2f91b0066547deb99c537af3e54d39/stdlib/hashlib.pyi#L25-L68

---

_Comment by @carljm on 2025-04-04 16:36_

I don't understand the `hashlib.sha256` import failure. Even without any `__all__` support, it is imported via `from _hashlib import openssl_sha256 as sha256` which we should understand as a re-export.

---

_Comment by @MichaReiser on 2025-04-04 16:38_

Oh right. The `collections.abc.Set` looks similar. It should be re-exported (by a star import), but we somehow miss it. 

---

_Comment by @carljm on 2025-04-04 16:43_

I don't think we consider star imports to be re-exports (and we shouldn't), so that is a case where we would need `__all__` support in order to understand that it is re-exported. But `from .. import ... as ...` should be considered a re-export even without `__all__`.

---

_Comment by @carljm on 2025-04-04 16:45_

@AlexWaygood didn't the star-import support you landed fix a bunch of `collections.abc` false positives? How did that even work without `__all__` support? Doesn't `collections.abc` rely on `__all__` to consider any of the star-imported names as re-exported?

---

_Comment by @AlexWaygood on 2025-04-04 16:47_

`*` imports do constitute re-exports in stubs even without `__all__`, yes, [this is specified](https://typing.python.org/en/latest/spec/distributing.html#import-conventions)

---

_Comment by @carljm on 2025-04-04 16:51_

Oh! My bad. I wonder why the `collections.abc` stub bothers to also import `__all__` then...

---

_Comment by @AlexWaygood on 2025-04-04 17:38_

> Oh! My bad. I wonder why the `collections.abc` stub bothers to also import `__all__` then...

if it didn't, you wouldn't be able to access `__all__` itself from `collections.abc`! I.e., it's fine to do this at runtime, but if the `collections.abc` stub did not import `__all__`, type checkers would complain about it:

```py
import collections.abc

for item in collections.abc.__all__:
    print(item)
```

I agree that the `collections.abc.Set` thing looks like it's caused by missing support for `__all__`, though -- but it's specifically missing support for the `__all__` re-export convention rather than missing support for `__all__` in `*` imports. Whereas all other members of `_collections_abc` are re-exported from that module using both the "redundant alias" convention _and_ inclusion in `__all__`, `_collections_abc.pyi` does not use the "redundant alias" convention for the `Set` symbol (it cannot). For the `Set` symbol, the only way to tell it's re-exported from `_collections_abc` is due to its inclusion in `_collections_abc.__all__`.

Because we don't understand `__all__` yet, we don't consider it re-exported by `_collections_abc`, and therefore we don't consider it an available name in `collections.abc`, because `collections.abc` does `from _collections_abc import *`, and we only consider that `*` import as importing all re-exported names from `_collections_abc` (which doesn't include `Set`).

---

_Comment by @AlexWaygood on 2025-04-04 17:44_

> I don't understand the `hashlib.sha256` import failure. Even without any `__all__` support, it is imported via `from _hashlib import openssl_sha256 as sha256` which we should understand as a re-export.

@carljm that's not correct. Imports in stubs are only re-exported to other modules if the alias name is exactly the same as the original symbol. In order for this to be a re-export, it would have to be `from _hashlib import openssl_sha256 as openssl_sha256`. Because it aliases the name to a different symbol, it does not constitute a re-export. So it looks like this is also something that requires an understanding of the `__all__` impact on re-export conventions for us to understand.

This is clearer in the original [PEP-484 text](https://peps.python.org/pep-0484/#stub-files) than the [typing spec](https://typing.python.org/en/latest/spec/distributing.html#import-conventions) -- PEP 484 says:

> Modules and variables imported into the stub are not considered exported from the stub unless the import uses the `import ... as ...` form or the equivalent `from ... import ... as ...` form. (UPDATE: To clarify, the intention here is that only names imported using the form `X as X` will be exported, i.e. the name before and after `as` must be the same.)

---

_Comment by @carljm on 2025-04-04 17:59_

Ugh sorry for the noise, clearly I needed to re-read the re-export spec 😆 

So in that case, does it seem like apart from the known issue of `__all__` support, this issue can be closed now?

---

_Comment by @AlexWaygood on 2025-04-04 18:01_

> 2025-04-04 13:31:34.470874000 DEBUG Checking file '/Users/micha/astral/black/src/black/__init__.py'
> 2025-04-04 13:31:34.471106000 DEBUG Module `pgen2` not found in search paths

I'm still not sure why we don't manage to resolve [the `pgen2` import in `black/__init__.py`](https://github.com/psf/black/blob/950ec38c119916868833cb9896dab324c5d8eadd/src/black/__init__.py#L82). That doesn't look to me like it's an `__all__`-related issue? 

---

_Comment by @sharkdp on 2025-04-04 18:32_

> I'm still not sure why we don't manage to resolve [the `pgen2` import in `black/__init__.py`](https://github.com/psf/black/blob/950ec38c119916868833cb9896dab324c5d8eadd/src/black/__init__.py#L82). That doesn't look to me like it's an `__all__`-related issue?

This line doesn't raise a diagnostic. The log message comes from here:

https://github.com/psf/black/blob/950ec38c119916868833cb9896dab324c5d8eadd/src/blib2to3/pgen2/conv.py#L35

---

_Comment by @AlexWaygood on 2025-04-04 18:40_

Aha! And if I remove this line locally:

```diff
diff --git a/src/blib2to3/pgen2/conv.py b/src/blib2to3/pgen2/conv.py
index 04eccfa..aee01c7 100644
--- a/src/blib2to3/pgen2/conv.py
+++ b/src/blib2to3/pgen2/conv.py
@@ -1,8 +1,6 @@
 # Copyright 2004-2005 Elemental Security, Inc. All Rights Reserved.
 # Licensed to PSF under a Contributor Agreement.
 
-# mypy: ignore-errors
-
 """Convert graminit.[ch] spit out by pgen to Python code.
 
 Pgen is the Python parser generator.  It is useful to quickly create a
```

then it turns out mypy can't resolve it either when I run mypy from the root of the Black repo:

```
src/blib2to3/pgen2/conv.py:33:1: error: Cannot find implementation or library stub for module named "pgen2"  [import-not-found]
src/blib2to3/pgen2/conv.py:33:1: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
```

It looks like this script can only be run from the `blib2to3` directory, not from the project root? I think both type checkers are correct in not resolving the import when the tool is run from the project root.

So I think we're done here 🥳

---

_Closed by @AlexWaygood on 2025-04-04 18:40_

---

_Comment by @MichaReiser on 2025-04-04 20:44_

Wow, thanks @AlexWaygood for this very thoughtful investigation. This would have taken me while to figure this all out 

---
