```yaml
number: 18057
title: "[ty] Infer the Python version from the environment if feasible"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/python-env-version
created_at: 2025-05-12T21:51:50Z
updated_at: 2025-05-30T21:28:20Z
url: https://github.com/astral-sh/ruff/pull/18057
synced_at: 2026-01-12T15:56:11Z
```

# [ty] Infer the Python version from the environment if feasible

---

_@zanieb_

Posting the draft to preview the change here and decide if it's fine or if we need to do a more invasive refactor.

Before falling back to the default Python version, use the Python version in the target Python environment.

Only supports reading the version from the `pyvenv.cfg` right now. We can sniff target versions from the site packages folder name on Unix too? A bit messy though.

---

_Label `ty` added by @zanieb on 2025-05-12 21:51_

---

_Comment by @github-actions[bot] on 2025-05-12 21:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- error[unresolved-attribute] pyinstrument/vendor/decorator.py:57:34: Type `<module 'inspect'>` has no attribute `getargspec`
- Found 89 diagnostics
+ Found 88 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[unresolved-import] aioredis/connection.py:11:6: Cannot resolve imported module `distutils.version`
- error[duplicate-base] aioredis/exceptions.py:14:7: Duplicate base class `TimeoutError`
- Found 28 diagnostics
+ Found 26 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ warning[unused-ignore-comment] tests/pyutils/test_is_awaitable.py:88:29: Unused blanket `type: ignore` directive
- Found 407 diagnostics
+ Found 408 diagnostics

stone (https://github.com/dropbox/stone)
+ warning[unused-ignore-comment] stone/frontend/ir_generator.py:9:49: Unused blanket `type: ignore` directive
- Found 173 diagnostics
+ Found 174 diagnostics

vision (https://github.com/pytorch/vision)
- error[unresolved-import] setup.py:1:8: Cannot resolve imported module `distutils.command.clean`
- error[unresolved-import] setup.py:2:8: Cannot resolve imported module `distutils.spawn`
+ warning[possibly-unbound-attribute] torchvision/datasets/video_utils.py:280:12: Attribute `is_integer` on type `int | float` is possibly unbound
- Found 1546 diagnostics
+ Found 1545 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[unresolved-attribute] src/bandersnatch/mirror.py:58:42: Type `<module 'datetime'>` has no attribute `UTC`
+ error[unresolved-import] src/bandersnatch/simple.py:4:24: Module `enum` has no member `StrEnum`
+ error[not-iterable] src/bandersnatch/simple.py:66:49: Object of type `<class 'SimpleDigest'>` is not iterable
+ error[unresolved-attribute] src/bandersnatch/tests/plugins/test_storage_plugins.py:116:25: Type `<module 'datetime'>` has no attribute `UTC`
+ error[unresolved-attribute] src/bandersnatch/tests/plugins/test_storage_plugins.py:123:28: Type `<module 'datetime'>` has no attribute `UTC`
+ error[not-iterable] src/bandersnatch/tests/test_simple.py:52:59: Object of type `<class 'SimpleDigest'>` is not iterable
+ error[unresolved-attribute] src/bandersnatch_storage_plugins/filesystem.py:286:70: Type `<module 'datetime'>` has no attribute `UTC`
- error[unresolved-import] src/bandersnatch_storage_plugins/s3.py:24:10: Cannot resolve imported module `s3path.accessor`
+ error[unresolved-attribute] src/bandersnatch_storage_plugins/s3.py:437:52: Type `<module 'datetime'>` has no attribute `UTC`
+ error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:989:56: Type `<module 'datetime'>` has no attribute `UTC`
- Found 124 diagnostics
+ Found 132 diagnostics

rich (https://github.com/Textualize/rich)
+ error[unresolved-attribute] tests/test_traceback.py:371:9: Type `Exception` has no attribute `add_note`
+ error[unresolved-attribute] tests/test_traceback.py:372:9: Type `Exception` has no attribute `add_note`
- Found 391 diagnostics
+ Found 393 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[unresolved-import] psycopg_c/build_backend/psycopg_build_ext.py:13:6: Cannot resolve imported module `distutils`
- error[unresolved-import] psycopg_c/build_backend/psycopg_build_ext.py:14:6: Cannot resolve imported module `distutils.command.build_ext`
- Found 1114 diagnostics
+ Found 1112 diagnostics

paasta (https://github.com/yelp/paasta)
- error[unresolved-import] paasta_tools/run-paasta-api-in-dev-mode.py:3:6: Cannot resolve imported module `distutils.dir_util`
- error[invalid-argument-type] paasta_tools/utils.py:2839:35: Argument to function `split` is incorrect: Expected `str | _ShlexInstream`, found `(str & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
+ error[no-matching-overload] paasta_tools/utils.py:2839:23: No overload of function `split` matches arguments
- Found 942 diagnostics
+ Found 941 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[unresolved-attribute] comtypes/test/__init__.py:137:12: Type `<module 'unittest'>` has no attribute `makeSuite`
- error[unresolved-attribute] comtypes/test/__init__.py:207:15: Type `<module 'unittest'>` has no attribute `makeSuite`
- error[unresolved-import] comtypes/test/setup.py:3:6: Cannot resolve imported module `distutils.core`
- Found 598 diagnostics
+ Found 595 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ error[unresolved-attribute] static_frame/core/db_util.py:397:18: Type `<module 'typing'>` has no attribute `Self`
+ error[unresolved-attribute] static_frame/core/db_util.py:409:14: Type `<module 'typing'>` has no attribute `Self`
+ error[invalid-syntax] static_frame/test/unit_forward/test_type_clinic.py:16:71: Cannot use star expression in index on Python 3.10 (syntax was added in Python 3.11)
+ error[invalid-syntax] static_frame/test/unit_forward/test_type_clinic.py:23:74: Cannot use star expression in index on Python 3.10 (syntax was added in Python 3.11)
+ error[invalid-syntax] static_frame/test/unit_forward/test_type_clinic.py:29:55: Cannot use star expression in index on Python 3.10 (syntax was added in Python 3.11)
+ error[invalid-syntax] static_frame/test/unit_forward/test_type_clinic.py:29:91: Cannot use star expression in index on Python 3.10 (syntax was added in Python 3.11)
- Found 2065 diagnostics
+ Found 2071 diagnostics

meson (https://github.com/mesonbuild/meson)
+ warning[unused-ignore-comment] mesonbuild/scripts/python_info.py:5:1: Unused blanket `type: ignore` directive
- Found 1387 diagnostics
+ Found 1388 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ error[unknown-argument] aiohttp/client_reqrep.py:1386:59: Argument `eager_start` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] aiohttp/client_ws.py:149:55: Argument `eager_start` does not match any known parameter of bound method `__init__`
- error[invalid-argument-type] aiohttp/compression_utils.py:198:25: Argument to function `len` is incorrect: Expected `Sized`, found `Buffer`
- error[invalid-argument-type] aiohttp/compression_utils.py:239:21: Argument to function `len` is incorrect: Expected `Sized`, found `Buffer`
+ error[unknown-argument] aiohttp/connector.py:997:64: Argument `eager_start` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] aiohttp/web_protocol.py:379:58: Argument `eager_start` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] aiohttp/web_protocol.py:624:58: Argument `eager_start` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] aiohttp/web_ws.py:171:55: Argument `eager_start` does not match any known parameter of bound method `__init__`
- Found 173 diagnostics
+ Found 177 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[unresolved-import] cloudinit/distros/netbsd.py:15:12: Cannot resolve imported module `crypt`
- error[unresolved-import] cloudinit/sources/DataSourceAzure.py:52:12: Cannot resolve imported module `crypt`
+ error[unresolved-attribute] tests/unittests/helpers.py:609:21: Type `PackageMetadata` has no attribute `get`
+ warning[possibly-unbound-attribute] tests/unittests/sources/azure/test_imds.py:755:26: Attribute `is_integer` on type `int | float` is possibly unbound
+ warning[possibly-unbound-attribute] tests/unittests/sources/azure/test_imds.py:822:26: Attribute `is_integer` on type `int | float` is possibly unbound
+ warning[possibly-unbound-attribute] tests/unittests/sources/azure/test_imds.py:906:26: Attribute `is_integer` on type `int | float` is possibly unbound
- Found 705 diagnostics
+ Found 707 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[missing-argument] scripts/run_in_subdirectory.py:35:9: No argument provided for required parameter `other` of bound method `relative_to`
- Found 3270 diagnostics
+ Found 3269 diagnostics

sympy (https://github.com/sympy/sympy)
+ error[unresolved-attribute] sympy/core/numbers.py:3676:20: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1040:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1041:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1043:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1044:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1046:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1047:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1050:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_arit.py:1073:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/core/tests/test_assumptions.py:1227:12: Type `int` has no attribute `is_integer`
+ error[unresolved-attribute] sympy/functions/elementary/exponential.py:307:20: Type `int` has no attribute `is_integer`
- warning[unused-ignore-comment] sympy/polys/puiseux.py:56:32: Unused blanket `type: ignore` directive
+ error[unresolved-import] sympy/polys/puiseux.py:33:29: Module `typing` has no member `Unpack`
+ error[invalid-assignment] sympy/polys/tests/test_puiseux.py:31:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_puiseux.py:45:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_puiseux.py:199:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:245:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:258:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:336:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:585:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:598:5: Not enough values to unpack: Expected 3
+ error[invalid-assignment] sympy/polys/tests/test_ring_series.py:688:5: Not enough values to unpack: Expected 3
- Found 18555 diagnostics
+ Found 18575 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ error[unresolved-import] ci/cleanup/aws.py:10:22: Module `datetime` has no member `UTC`
- error[unresolved-import] ci/deploy/pypi.py:10:8: Cannot resolve imported module `distutils.core`
+ error[invalid-return-type] ci/deploy/pypi.py:54:12: Return type does not match returned value: expected `tuple[str, str]`, found `tuple[str | None, str | None]`
+ error[unresolved-attribute] ci/load/periodic.py:34:34: Type `<module 'datetime'>` has no attribute `UTC`
- error[unresolved-import] misc/dbt-materialize/setup.py:18:6: Cannot resolve imported module `distutils.core`
+ error[invalid-syntax] test/race-condition/mzcompose.py:489:113: Cannot reuse outer quote character in f-strings on Python 3.10 (syntax was added in Python 3.12)
+ error[invalid-syntax] test/race-condition/mzcompose.py:513:126: Cannot reuse outer quote character in f-strings on Python 3.10 (syntax was added in Python 3.12)
- Found 3216 diagnostics
+ Found 3219 diagnostics

```
</details>


---

_Review comment by @zanieb on `crates/ty_python_semantic/src/program.rs`:36 on 2025-05-12 21:57_

This is the crux of it, really.

The main implementation question is whether we want to store the `PythonVersion` on `SearchPaths`. It's a little weird, but that's the only place we have a `PythonEnvironment` right now and I'm hesitant to tear that out for this feature.

---

_@zanieb reviewed on 2025-05-12 21:57_

---

_@zanieb reviewed on 2025-05-13 00:32_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:296 on 2025-05-13 00:32_

This is the main functional change here (returning the version). I was confused by how this was implemented, it seemed non-idiomatic.

---

_Comment by @github-actions[bot] on 2025-05-13 00:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2025-05-13 04:31_

A second opinion seems helpful. Not in a rush to merge before the announcement though.

---

_Marked ready for review by @zanieb on 2025-05-13 04:31_

---

_Review requested from @carljm by @zanieb on 2025-05-13 04:31_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-13 04:31_

---

_Review requested from @sharkdp by @zanieb on 2025-05-13 04:31_

---

_Review requested from @dcreager by @zanieb on 2025-05-13 04:31_

---

_Review requested from @MichaReiser by @zanieb on 2025-05-13 04:31_

---

_@MichaReiser reviewed on 2025-05-13 06:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:68 on 2025-05-13 06:23_

I think this is wrong but you have to test it by running ty in `--watch` and then change from having a `python-version` in the `pyproject.toml` to removing it. I suspect that ty will not propagate the change and instead keeps using the old `python-version`.

What we need here is the same behavior as when initializing the settings the first time. 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:36 on 2025-05-13 06:26_

Hmm, I don't love it but it seems okay. It's a bit dangerous because it's very easy to take the `python_version` from `ProgramSettings` directly (did you review all read paths?). In fact, you did exactly that in `update_from_settings`, which, I think, is incorrect. 


The reason why I think it's fine is because the logic is encapsulated in here.



---

_@MichaReiser reviewed on 2025-05-13 06:27_

We should do some more testing for how this interacts in `--watch` mode, and, ideally, at some tests to `file_watching.rs`. 

Two cases that I'm not sure if they work as expected:

* Changing the settings from having a `python-version` to not having one (in which case the fallback behavior should apply)
* Changing the python version in the venv. I don't think ty would pick this up and it would keep running with the wrong version

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-13 11:57_

---

_@zanieb reviewed on 2025-05-13 12:25_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/program.rs`:68 on 2025-05-13 12:25_

Oh interesting. I can look into that.

---

_Comment by @zanieb on 2025-05-13 12:26_

Thanks! Those are good callouts. I'll follow up on this when I have time, otherwise, feel free.

---

_Review request for @carljm removed by @carljm on 2025-05-22 22:04_

---

_Comment by @carljm on 2025-05-22 22:04_

This would be great to have (especially since I accidentally said at the typing summit that we already had it 😆 )

---

_Comment by @AlexWaygood on 2025-05-27 13:05_

I'll try to pick this up this week

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-27 13:05_

---

_Comment by @MichaReiser on 2025-05-27 13:36_

IMO, not sure if it's worth prioritizing this over other typing features. 

---

_Comment by @AlexWaygood on 2025-05-27 13:41_

I disagree:
- I think it's causing a fair amount of user confusion that we don't do this already (people expect us to do it)
- I think it's a logical change, and I don't think it's too much work to implement it
- It's a feature already supported by rival type checkers that are not written in Python
- As Carl says, he mistakenly announced that we already have the feature at the typing summit 😆

The better we do at inferring the Python version from the user's environment, the less likely it is we have to fallback to an arbitrary default Python version, which is always going to lead to false positives and false negatives for some users. Improving usability in this area feels pretty high priority to me.

---

_Converted to draft by @carljm on 2025-05-27 17:44_

---

_Comment by @AlexWaygood on 2025-05-29 17:58_

Here's a screen recording that demonstrates that we correctly update the inferred Python version in `--watch` mode if a new virtual environment is added, modified or removed again:

https://github.com/user-attachments/assets/f5e9b48f-ce11-454e-ba87-2fccce7c6420

I checked, and we also get the correct behaviour if you delete a pyproject.toml file that contains a `project.requires-python` field: we switch to the version specified in the virtual environment's `pyvenv.cfg` file. Adding it back causes us to switch back to the version specified in the pyproject.toml file.


---

_Comment by @AlexWaygood on 2025-05-29 19:38_

Okay, I think this is ready for re-review now! In terms of tests, I added two CLI tests and recorded the video in https://github.com/astral-sh/ruff/pull/18057#issuecomment-2920157777 to demonstrate that the file-watching behaviour is working.

We have _fairly_ comprehensive tests already for `pyvenv.cfg` parsing, so I think we're okay on that front. I feel like I should be adding more tests of some kind somewhere, but I'm not sure how... I tried adding a file-watching test, but I couldn't get it to work and I wasn't fully sure I understood the setup in `file_watching.rs`.

---

_Marked ready for review by @AlexWaygood on 2025-05-29 19:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-29 19:38_

---

_Comment by @AlexWaygood on 2025-05-29 19:38_

(Cc. @zanieb -- would love a review from you of the changes I've pushed here 😄)

---

_Comment by @AlexWaygood on 2025-05-29 19:52_

Ah, the primer report is interesting. I guess an unanticipated consequence of this change is that we're now picking up the Python version from the virtual environments mypy_primer creates on the fly if primer is being run on a project that doesn't specify `project.requires-python` in its `pyproject.toml` file.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:395 on 2025-05-29 19:54_

The switch to a proper parser here is motivated by wanting to capture the `TextRange` of the Python version in the `pyvenv.cfg` file so that we can display it in diagnostics. Screenshot of what this looks like on the command line:

![image](https://github.com/user-attachments/assets/0aa68490-b9cc-4e85-a58c-64dff1f0e925)


---

_@AlexWaygood reviewed on 2025-05-29 19:54_

---

_Comment by @MichaReiser on 2025-05-29 20:06_

> t. I feel like I should be adding more tests of some kind somewhere, but I'm not sure how... I tried adding a file-watching test, but I couldn't get it to work and I wasn't fully sure I understood the setup in file_watching.rs

Can you say more about what you need and what you tried?

---

_Comment by @AlexWaygood on 2025-05-29 20:20_

> Can you say more about what you need and what you tried?

I wanted to add a test that showed that if you added a virtual environment, ty would switch to using the Python version from that virtual environment if no Python version was specified in any configuration files. I tried something like this:

```diff
diff --git a/crates/ty/tests/file_watching.rs b/crates/ty/tests/file_watching.rs
index 71c50ff6eb..d8a398f276 100644
--- a/crates/ty/tests/file_watching.rs
+++ b/crates/ty/tests/file_watching.rs
@@ -1170,6 +1170,42 @@ print(sys.last_exc, os.getegid())
     Ok(())
 }
 
+#[test]
+fn change_python_version_due_to_new_venv() -> anyhow::Result<()> {
+    let mut case = setup(|context: &mut SetupContext| {
+        // `sys.last_exc` is a Python 3.12 only feature;
+        // we default to the latest Python version.
+        context.write_project_file("bar.py", "import sys; print(sys.last_exc)")
+    })?;
+
+    let diagnostics = case.db.check();
+
+    assert!(diagnostics.is_empty());
+
+    let venv_dir = case.project_path(".venv").as_std_path().to_path_buf();
+    std::fs::create_dir_all(&venv_dir)?;
+    std::fs::write(venv_dir.join("pyvenv.cfg"), "version = 3.8")?;
+
+    // Register the venv as a search path:
+    // we should now pick up the Python version from the venv.
+    case.update_options(Options {
+        environment: Some(EnvironmentOptions {
+            extra_paths: Some(vec![RelativePathBuf::cli(".venv/site-packages")]),
+            ..EnvironmentOptions::default()
+        }),
+        ..Options::default()
+    })?;
+
+    let diagnostics = case.db.check();
+    assert_eq!(diagnostics.len(), 1);
+    assert_eq!(
+        diagnostics[0].primary_message(),
+        "Type `<module 'sys'>` has no attribute `last_exc`"
+    );
+
+    Ok(())
+}
+
```

but it did not pass, even though manual testing _appears_ to indicate that file watching is working as intended. (LMK if that's not the case!)

---

_Comment by @MichaReiser on 2025-05-30 06:14_

> Here's a screen recording that demonstrates that we correctly update the inferred Python version in --watch mode if a new virtual environment is added, modified or removed again:

This is so cool!

File watching tests always have the following structure:

1. setup call: Writes the "before" state
1. modifications: Write new files, change the environment that ty should observe
1. Waiting for the file watcher to observe the changes: `let changes = case.stop_watch(event_for_file("sub"))`
1. Applying the observed changes `case.apply_changes(changes, None);`
1. Assertions that ty correcty picked up the changes

Your code misses the steps 3 and 4.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:79 on 2025-05-30 06:23_

Could we call `Self::update_search_paths` here instead of duplicating the logic?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:188 on 2025-05-30 06:24_

```suggestion
enum PythonSourcePriority {
    Default,
    PyvenvCfgFile,
    ConfigFile,
    Cli,
}
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 06:30_

What's the reason that we need to canonicalize here. Could we use `absolute` instead?

Also, `as_std_path` won't work with our memory file system. Use `System::canonicalize` instead

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:421 on 2025-05-30 06:32_

I guess we don't support `\r` line endings. Seems reasonable. Does your code work correclty with `\r\n`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:442 on 2025-05-30 06:33_

This should never be `eof` here because we should always be at a `=`. I would use `bump` here and add a comment that we bump the `=`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:467 on 2025-05-30 06:39_


```suggestion
                let version_range = TextRange::at(value_offset, value.text_len());
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:490 on 2025-05-30 06:39_

I always find `bump` calls hard to understand. Which char are we reading here? I would use `cursor.eat('\n')` which gives you the eof check for free

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:412 on 2025-05-30 06:47_

Nit: What I like doing is to store the source in addition to the `Cursor` and then use the offsets to slice into the string. It reduces the cases where you need to use `find` and other string methods (untested):

```patch
Index: crates/ty_python_semantic/src/site_packages.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_python_semantic/src/site_packages.rs b/crates/ty_python_semantic/src/site_packages.rs
--- a/crates/ty_python_semantic/src/site_packages.rs	(revision 30c867ecfdc943501aa0fb1ab4802d43f51ad681)
+++ b/crates/ty_python_semantic/src/site_packages.rs	(date 1748587587888)
@@ -424,38 +424,29 @@
         let line_number = *line_number;
 
         cursor.eat_while(|c| c.is_whitespace() && c != '\n');
-
-        let remaining_file = cursor.chars().as_str();
-
-        let next_newline_position = remaining_file
-            .find('\n')
-            .unwrap_or(cursor.text_len().to_usize());
-
-        // The Python standard-library's `site` module parses these files by splitting each line on
-        // '=' characters, so that's what we should do as well.
-        let Some(eq_position) = remaining_file[..next_newline_position].find('=') else {
-            // Skip over any lines that do not contain '=' characters, same as the CPython stdlib
-            // <https://github.com/python/cpython/blob/e64395e8eb8d3a9e35e3e534e87d427ff27ab0a5/Lib/site.py#L625-L632>
-            cursor.skip_bytes(next_newline_position);
-            if !cursor.is_eof() {
-                cursor.bump();
-            }
+        
+        let key_start = cursor.offset();
+        
+        cursor.eat_while(|c| c != '\n' && c != '=');
+        if !cursor.eat_char('=') {
+            cursor.eat_char('\n');
             return Ok(());
-        };
-
-        let key = remaining_file[..eq_position].trim();
-
-        cursor.skip_bytes(eq_position + 1);
+        }
+        
+        let key_range = TextRange::new(key_start, cursor.offset());
+        let key = self.source[key_range];
+        
         cursor.eat_while(|c| c.is_whitespace() && c != '\n');
 
-        let value = remaining_file[eq_position + 1..next_newline_position].trim();
+        let value_start = cursor.offset();
+        cursor.eat_while(|c| c != '\n');
+        let value_range = TextRange::new(value_start, cursor.offset());
+        let value = &self.source[value_range];
 
         if value.is_empty() {
             return Err(PyvenvCfgParseErrorKind::MalformedKeyValuePair { line_number });
         }
 
-        let value_offset = cursor.offset();
-
         match key {
             "include-system-site-packages" => {
                 data.include_system_site_packages = value.eq_ignore_ascii_case("true");
@@ -464,8 +455,7 @@
             // `virtualenv` and `uv` call this key `version_info`,
             // but the stdlib venv module calls it `version`
             "version" | "version_info" => {
-                let version_range = TextRange::new(value_offset, value_offset + value.text_len());
-                data.version = Some((value, version_range));
+                data.version = Some((value, value_range));
             }
             "implementation" => {
                 data.implementation = match value.to_ascii_lowercase().as_str() {
```



---

_@MichaReiser approved on 2025-05-30 06:47_

Nice! 

---

_@AlexWaygood reviewed on 2025-05-30 13:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:188 on 2025-05-30 13:42_

I know they're not necessary, but I kinda prefer having them there so that it's explicit that `Cli` is definitely meant to have a higher priority than `ConfigFile` (for example)... I guess I should add a test for that as well!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:421 on 2025-05-30 13:43_

> I guess we don't support `\r` line endings.

That's correct, yeah. We didn't before this PR either.

> Does your code work correclty with `\r\n`?

I _think_ so... I'll see if I can add a test.

---

_@AlexWaygood reviewed on 2025-05-30 13:43_

---

_@AlexWaygood reviewed on 2025-05-30 13:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:79 on 2025-05-30 13:44_

I think it would lead to us doing a tiny bit of unnecessary work in some situations, but it's probably worth it to avoid the duplication of logic (especially since updating the search paths and changing the configured Python version are both very rare events)

---

_@AlexWaygood reviewed on 2025-05-30 13:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 13:49_

> What's the reason that we need to canonicalize here.

I found that without the `canonicalize()` call here, the `strip_prefix` call in the following function would fail on Windows if it was rendering a diagnostic that used an annotation showing the contents of the `pyvenv.cfg` file. This is because the `cwd` path would be a UNC path but the path to the `pyvenv.cfg` file would not be a UNC path. So the purpose of the `canonicalize()` call here is to convert the path to the `pyvenv.cfg` file to a UNC path.

https://github.com/astral-sh/ruff/blob/d65bd69963e8b6ec05e465b1ecbdd391883645ef/crates/ruff_db/src/diagnostic/render.rs#L700-L706

> Could we use `absolute` instead?

I don't know... does `Path::absolute()` convert non-UNC paths to UNC paths on Windows? 😄

> Also, `as_std_path` won't work with our memory file system. Use `System::canonicalize` instead

👍

---

_@AlexWaygood reviewed on 2025-05-30 13:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:442 on 2025-05-30 13:53_

Hmm, I think you might have misread the code here. We only hit this branch if there _isn't_ a `=` character on this line. But I'll switch this to `cursor.eat_char('\n');`

---

_@AlexWaygood reviewed on 2025-05-30 14:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 14:08_

Hmm, our docs for `System::canonicalize_path()` state:

>    /// ## Windows long-paths
>    ///
>    /// Unlike `std::fs::canonicalize`, this function does remove UNC prefixes if possible.
>    /// See [dunce::canonicalize] for more information.

But... I actually _need_ a UNC path here, since all other paths being passed to `relativize_path` appear to be UNC paths on Windows. What do you suggest?

---

_@AlexWaygood reviewed on 2025-05-30 15:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:79 on 2025-05-30 15:30_

Actually, no, we can't really call `Self::update_search_paths()` here :/ because in this method we need to combine the Python version we might have got from the config file with the Python version we might have got from the environment, whereas in `Self::update_search_paths()` we only care about the latter.

---

_@MichaReiser reviewed on 2025-05-30 15:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 15:34_

Where did you see this. Where does the `cwd` directory come from? We don't canonicalize it in the CLI

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 15:54_

You can see that on commit https://github.com/astral-sh/ruff/pull/18057/commits/634dc4306d68d0550d8c0bdda3501709e79835d0, my snapshot test was failing only on Windows: https://github.com/astral-sh/ruff/actions/runs/15323540471/job/43112455490?pr=18057. This is because on Windows, an absolute path to the `pyvenv.cfg` file was being included in the annotation, rather than a relative path, as was the case on all other platforms. Absolute paths are converted to relative paths in diagnostic annotations using the `relativize_path` function in `render.rs`.

In commit https://github.com/astral-sh/ruff/pull/18057/commits/634dc4306d68d0550d8c0bdda3501709e79835d0, I added some debug prints to debug why our diagnostic rendering infrastructure was failing to relativize the path to the `pyvenv.cfg` file on Windows. The CI run is here, and it revealed that this was because `cwd` is a UNC path but the `pyvenv.cfg` path was not: https://github.com/astral-sh/ruff/actions/runs/15325734010/job/43119701574?pr=18057

Adding the `canonicalize()` call in https://github.com/astral-sh/ruff/pull/18057/commits/3a8cb826970b5fe8ae97eba2b518cf60870e2927 fixed the snapshot test on Windows: `relativize_path` is now able to relativize the path to the `pyvenv.cfg` file on all platforms.

---

_@AlexWaygood reviewed on 2025-05-30 15:54_

---

_@MichaReiser reviewed on 2025-05-30 15:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 15:59_

I don't think this is the right fix. It will fail if `cwd` isn't canonicalized. 

---

_@MichaReiser reviewed on 2025-05-30 16:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 16:01_

The problem here is that we canonicalize the project directory in the CLI tests (but we don't on the CLI), which gives you a UNC path

---

_@AlexWaygood reviewed on 2025-05-30 16:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 16:03_

I think all our other paths must already be UNC paths on Windows (for some reason), so I think a lot of other things would start failing if `cwd` wasn't canonicalized. (But I'm also happy to look again and find a different fix!)

---

_@MichaReiser reviewed on 2025-05-30 16:04_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 16:04_

We do have to canonicalize in the CLI tests because of macos (thanks for that!) You could try to use `dunce::simplified` to simplify the path or use `System::canonicalize_path` although I'm not sure if the docs are accurate because I don't see the dunce::simplified :D

> Hmm, our docs for System::canonicalize_path() state:


---

_@MichaReiser reviewed on 2025-05-30 16:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 16:05_

Sorry, I'm wrong. we actually do... 

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_db/src/system/os.rs#L87-L91

We call simplified. You could try to add the simplified call in the CLI test

---

_@AlexWaygood reviewed on 2025-05-30 17:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 17:40_

Okay cool. I'll trust you that this is purely an issue with our test setup and not something that Windows users will actually run into ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:288 on 2025-05-30 18:03_

Fixed in https://github.com/astral-sh/ruff/pull/18057/commits/06bd1a58054ac2e53ddf2e566f8015b0f9a72a75 -- ty for pointing me towards the correct fix here!!

---

_@AlexWaygood reviewed on 2025-05-30 18:03_

---

_@AlexWaygood reviewed on 2025-05-30 19:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:421 on 2025-05-30 19:21_

Added a test in https://github.com/astral-sh/ruff/pull/18057/commits/45e067a80da64c57da98f0060ba870c48ce9b18f that verifies this

---

_@AlexWaygood reviewed on 2025-05-30 20:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:412 on 2025-05-30 20:20_

Oh, this is great. Thank you!

---

_Comment by @AlexWaygood on 2025-05-30 21:20_

I'm still struggling to write a file-watching test. I'm probably missing something, but I'm just going to land for now. Happy to follow up on this if there's an easy way to get it working!

---

_Merged by @AlexWaygood on 2025-05-30 21:22_

---

_Closed by @AlexWaygood on 2025-05-30 21:22_

---

_Branch deleted on 2025-05-30 21:22_

---
