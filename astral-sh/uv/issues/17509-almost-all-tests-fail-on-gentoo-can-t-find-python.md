```yaml
number: 17509
title: "Almost all tests fail on Gentoo (can't find Python) in 0.9.26"
type: issue
state: open
author: mgorny
labels:
  - bug
assignees: []
created_at: 2026-01-16T04:46:41Z
updated_at: 2026-01-21T05:10:19Z
url: https://github.com/astral-sh/uv/issues/17509
synced_at: 2026-01-21T05:55:48Z
```

# Almost all tests fail on Gentoo (can't find Python) in 0.9.26

---

_@mgorny_

### Summary

When running the test suite in 0.6.26, I'm getting:

```
test result: FAILED^O. 141 passed; 2263 failed; 1 ignored; 0 measured; 0 filtered out; finished in 309.87s
```

I haven't checked all 2263 errors, but they seem to mostly be:

```
thread 'workspace_metadata::workspace_metadata_simple' (35046) panicked at /rustc/ded5c06cf21d2b93bffd5d884aa6e96934ee4234/library/core
/src/ops/function.rs:250:5:
Unexpected failure.
code=2
stderr=``````
error: No interpreter found for Python 3.12 in search path or managed installations

hint: A managed Python download is available for Python 3.12, but Python downloads are set to \'manual\', use `uv python install 3.12` 
to install the required version
```
```
command=`cd "/var/tmp/portage/dev-python/uv-0.9.26/temp/uv/tests/.tmpsVE3pU/temp" && env -u ANDROID_API_LEVEL -u APPDATA -u AWS_ACCESS_
KEY_ID -u AWS_CONFIG_FILE -u AWS_DEFAULT_REGION -u AWS_PROFILE -u AWS_REGION -u AWS_SECRET_ACCESS_KEY -u AWS_SESSION_TOKEN -u AWS_SHARE
D_CREDENTIALS_FILE -u BASH_VERSION -u BLACK_PATH -u BUILD_BUILDID -u BUILD_ID -u CARGO_CFG_WINDOWS -u CARGO_MANIFEST_DIR -u CARGO_TARGE
T_DIR -u CI -u CLICOLOR_FORCE -u COLUMNS -u CONDA_DEFAULT_ENV -u CONDA_PREFIX -u DEPENDABOT -u EXPECTED_ANYIO_VERSION -u FILE_PATH -u F
ISH_VERSION -u FORCE_COLOR -u GITHUB_ACTIONS -u GITLAB_CI -u GIT_ALLOW_PROTOCOL -u GIT_ALTERNATE_OBJECT_DIRECTORIES -u GIT_CEILING_DIRE
CTORIES -u GIT_CONFIG_GLOBAL -u GIT_DIR -u GIT_INDEX_FILE -u GIT_LFS_SKIP_SMUDGE -u GIT_OBJECT_DIRECTORY -u GIT_SSH_COMMAND -u GIT_SSL_
NO_VERIFY -u GIT_TERMINAL_PROMPT -u GIT_WORK_TREE -u HATCH_PATH -u HF_TOKEN -u HOME -u HTTP_TIMEOUT -u IPHONEOS_DEPLOYMENT_TARGET -u JP
Y_SESSION_NAME -u KEYRING_TEST_CREDENTIALS -u KSH_VERSION -u LC_ALL -u LOCALAPPDATA -u MACOSX_DEPLOYMENT_TARGET -u NETRC -u NO_COLOR -u
 NU_VERSION -u OUT_DIR -u PAGER -u PATH -u PIP_IS_CI -u PROMPT -u PWD -u PYC_INVALIDATION_MODE -u PYPI_ID_TOKEN -u PYTHONHOME -u PYTHON
IOENCODING -u PYTHONPATH -u PYTHONUNBUFFERED -u PYTHONUTF8 -u PYX_API_KEY -u PYX_API_URL -u PYX_AUTH_TOKEN -u PYX_CDN_DOMAIN -u PYX_CRE
DENTIALS_DIR -u ROOT_PATH -u SHELL -u SSL_CLIENT_CERT -u TARGET -u TESTPYPI_ID_TOKEN -u TRACING_DURATIONS_FILE -u TRACING_DURATIONS_TES
T_ROOT -u URL -u USERPROFILE -u UV -u UV_AMD_GPU_ARCHITECTURE -u UV_API_KEY -u UV_AUTH_TOKEN -u UV_BREAK_SYSTEM_PACKAGES -u UV_BUILD_CO
NSTRAINT -u UV_CACHE_DIR -u UV_COMMIT_DATE -u UV_COMMIT_HASH -u UV_COMMIT_SHORT_HASH -u UV_COMPILE_BYTECODE -u UV_COMPILE_BYTECODE_TIMEOUT -u UV_CONCURRENT_BUILDS -u UV_CONCURRENT_DOWNLOADS -u UV_CONCURRENT_INSTALLS -u UV_CONFIG_FILE -u UV_CONSTRAINT -u UV_CREDENTIALS_DIR -u UV_CUDA_DRIVER_VERSION -u UV_CUSTOM_COMPILE_COMMAND -u UV_DEFAULT_INDEX -u UV_DEV -u UV_DOWNLOAD_URL -u UV_ENV_FILE -u UV_EXCLUDE -u UV_EXCLUDE_NEWER -u UV_EXTRA_INDEX_URL -u UV_FIND_LINKS -u UV_FORK_STRATEGY -u UV_FROZEN -u UV_GCS_ENDPOINT_URL -u UV_GITHUB_FAST_PATH_URL -u UV_GITHUB_TOKEN -u UV_GIT_LFS -u UV_HIDE_BUILD_OUTPUT -u UV_HTTP_RETRIES -u UV_HTTP_TIMEOUT -u UV_INDEX -u UV_INDEX_MY_INDEX_PASSWORD -u UV_INDEX_MY_INDEX_USERNAME -u UV_INDEX_STRATEGY -u UV_INDEX_URL -u UV_INIT_BUILD_BACKEND -u UV_INSECURE_HOST -u UV_INSECURE_NO_ZIP_VALIDATION -u UV_INSTALLER_GHE_BASE_URL -u UV_INSTALLER_GITHUB_BASE_URL -u UV_INSTALL_DIR -u UV_INTERNAL__PARENT_INTERPRETER -u UV_INTERNAL__SHOW_DERIVATION_TREE -u UV_INTERNAL__TEST_DIR -u UV_INTERNAL__TEST_LFS_DISABLED -u UV_INTERNAL__TEST_PYTHON_MANAGED -u UV_ISOLATED -u UV_KEYRING_PROVIDER -u UV_LAST_TAG -u UV_LAST_TAG_DISTANCE -u UV_LIBC -u UV_LINK_MODE -u UV_LOCKED -u UV_LOCK_TIMEOUT -u UV_LOG_CONTEXT -u UV_MANAGED_PYTHON -u UV_NO_BINARY -u UV_NO_BINARY_PACKAGE -u UV_NO_BUILD -u UV_NO_BUILD_ISOLATION -u UV_NO_BUILD_PACKAGE -u UV_NO_CACHE -u UV_NO_CONFIG -u UV_NO_DEFAULT_GROUPS -u UV_NO_DEV -u UV_NO_EDITABLE -u UV_NO_ENV_FILE -u UV_NO_GITHUB_FAST_PATH -u UV_NO_GROUP -u UV_NO_HF_TOKEN -u UV_NO_INSTALLER_METADATA -u UV_NO_MANAGED_PYTHON -u UV_NO_MODIFY_PATH -u UV_NO_PROGRESS -u UV_NO_SOURCES -u UV_NO_SOURCES_PACKAGE -u UV_NO_SYNC -u UV_NO_VERIFY_HASHES -u UV_NO_WRAP -u UV_OFFLINE -u UV_OVERRIDE -u UV_PRERELEASE -u UV_PREVIEW -u UV_PREVIEW_FEATURES -u UV_PROJECT -u UV_PROJECT_ENVIRONMENT -u UV_PUBLISH_CHECK_URL -u UV_PUBLISH_INDEX -u UV_PUBLISH_NO_ATTESTATIONS -u UV_PUBLISH_PASSWORD -u UV_PUBLISH_TOKEN -u UV_PUBLISH_URL -u UV_PUBLISH_USERNAME -u UV_PYPY_INSTALL_MIRROR -u UV_PYTHON -u UV_PYTHON_BIN_DIR -u UV_PYTHON_CACHE_DIR -u UV_PYTHON_CPYTHON_BUILD -u UV_PYTHON_DOWNLOADS -u UV_PYTHON_DOWNLOADS_JSON_URL -u UV_PYTHON_GRAALPY_BUILD -u UV_PYTHON_INSTALL_BIN -u UV_PYTHON_INSTALL_DIR -u UV_PYTHON_INSTALL_MIRROR -u UV_PYTHON_INSTALL_REGISTRY -u UV_PYTHON_PREFERENCE -u UV_PYTHON_PYODIDE_BUILD -u UV_PYTHON_PYPY_BUILD -u UV_REQUEST_TIMEOUT -u UV_REQUIRE_HASHES -u UV_RESOLUTION -u UV_RUN_MAX_RECURSION_DEPTH -u UV_RUN_RECURSION_DEPTH -u UV_S3_ENDPOINT_URL -u UV_SHOW_RESOLUTION -u UV_SKIP_WHEEL_FILENAME_CHECK -u UV_SYSTEM_PYTHON -u UV_TEST_CURRENT_TIMESTAMP -u UV_TEST_NO_CLI_PROGRESS -u UV_TEST_NO_HTTP_RETRY_DELAY -u UV_TEST_PACKSE_INDEX -u UV_TEST_PYTHON_PATH -u UV_TOOL_BIN_DIR -u UV_TOOL_DIR -u UV_TORCH_BACKEND -u UV_UNMANAGED_INSTALL -u UV_UPDATE_SCHEMA -u UV_UPLOAD_HTTP_TIMEOUT -u UV_VENV_CLEAR -u UV_VENV_SEED -u UV_WORKING_DIR -u UV_WORKING_DIRECTORY -u VIRTUAL_ENV -u VIRTUAL_ENV_DISABLE_PROMPT -u XDG_BIN_HOME -u XDG_CACHE_HOME -u XDG_CONFIG_DIRS -u XDG_CONFIG_HOME -u XDG_DATA_HOME -u ZDOTDIR -u ZSH_VERSION -u _CONDA_ROOT -u __PYVENV_LAUNCHER__ "/var/tmp/portage/dev-python/uv-0.9.26/work/uv-0.9.26/target/debug/uv" "venv" "/var/tmp/portage/dev-python/uv-0.9.26/temp/uv/tests/.tmpsVE3pU/temp/.venv" "--clear" "--cache-dir" "/var/tmp/portage/dev-python/uv-0.9.26/temp/uv/tests/.tmpsVE3pU/cache" "--python" "3.12"`
code=2
stdout=""
stderr=```
error: No interpreter found for Python 3.12 in search path or managed installations

hint: A managed Python download is available for Python 3.12, but Python downloads are set to \'manual\', use `uv python install 3.12` 
to install the required version
```
```

There obviously is a `/usr/bin/python3.12` in Gentoo.

### Platform

Gentoo Linux amd64

### Version

0.9.26

### Python version

3.12.12 (though `python{,3}` is 3.14.0)

---

_Label `bug` added by @mgorny on 2026-01-16 04:46_

---

_Comment by @konstin on 2026-01-16 10:32_

I assume that's https://github.com/astral-sh/uv/pull/14080, do you know which env var is missing?

---

_Assigned to @konstin by @konstin on 2026-01-16 10:33_

---

_Comment by @mgorny on 2026-01-16 12:40_

> I assume that's [#14080](https://github.com/astral-sh/uv/pull/14080), do you know which env var is missing?

`PATH` is, for a start.

---

_Comment by @mgorny on 2026-01-16 12:41_

(I'm going to test further to see if any failures remain after adding `PATH`.)

---

_Comment by @konstin on 2026-01-16 12:44_

https://github.com/astral-sh/uv/pull/17515 - If you have more env vars to share, I'll try to batch them in that PR.

---

_Comment by @mgorny on 2026-01-16 13:24_

Well, "most" of them are gone but still a lot remain.

So:

1. We override `HOME` with a temporary directory, but the test suite scrubs that and grabs it from passwd.
2. We override `XDG_CONFIG_DIRS` to hide the system-wide configuration file, but the test suite scrubs that and therefore gets affected by the system-wide config.

In both cases, passing the variable through would be sufficient for the test suite to pass for us *but* I think it would be better if the test suite weren't affected by system environment in the first place — possibly by overriding both envvars with a temporary directory internally, or otherwise mocking the relevant logic to use temporary directories.

<details>
<summary>Example failures</summary>

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: lock_requires_python-9
Source: crates/uv/tests/it/lock.rs:5867
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    2     2 │ exit_code: 2
    3     3 │ ----- stdout -----
    4     4 │ 
    5     5 │ ----- stderr -----
    6       │-error: No interpreter found for Python >=3.12 in [PYTHON SOURCES]
          6 │+error: No interpreter found for Python >=3.12 in [PYTHON SOURCES] or managed installations
    7     7 │ 
    8     8 │ hint: A managed Python download is available for Python >=3.12, but Python downloads are set to 'never'
────────────┴───────────────────────────────────────────────────────────────────
```

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: compile_fallback_interpreter-5
Source: crates/uv/tests/it/pip_compile.rs:1554
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    2     2 │ exit_code: 2
    3     3 │ ----- stdout -----
    4     4 │ 
    5     5 │ ----- stderr -----
    6       │-error: No interpreter found for PyPy in [PYTHON SOURCES]
          6 │+error: No interpreter found for PyPy in virtual environments, [PYTHON SOURCES], or managed installations
    7     7 │ 
    8     8 │ hint: A managed Python download is available for PyPy, but Python downloads are set to 'never'
────────────┴───────────────────────────────────────────────────────────────────
```

But also:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: read_index_credential_env_vars_for_check_url-2
Source: crates/uv/tests/it/publish.rs:481
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1       │-success: true
    2       │-exit_code: 0
          1 │+success: false
          2 │+exit_code: 2
    3     3 │ ----- stdout -----
    4     4 │ 
    5     5 │ ----- stderr -----
    6     6 │ Publishing 1 file to http://[LOCALHOST]/upload
    7       │-File astral_test_private-0.1.0-py3-none-any.whl already exists, skipping
          7 │+error: Failed to query check URL
          8 │+  Caused by: Failed to write to the client cache
          9 │+  Caused by: failed to create directory `/var/lib/portage/home/.cache/uv/simple-v18/index/1481bc36ab95888e`: Permission denied (os error 13)
────────────┴───────────────────────────────────────────────────────────────────
```

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list_downloads
Source: crates/uv/tests/it/python_list.rs:369
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ success: true
    2     2 │ exit_code: 0
    3     3 │ ----- stdout -----
    4       │-cpython-3.10.19-[PLATFORM]    <download available>
    5       │-pypy-3.10.16-[PLATFORM]       <download available>
    6       │-graalpy-3.10.0-[PLATFORM]     <download available>
    7     4 │ 
    8     5 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
```
</details>

---

_Comment by @zanieb on 2026-01-16 13:55_

Hm we do set `HOME`

https://github.com/astral-sh/uv/blob/681e8e060fdc4ca2d0a180159ce63a720d3b93cf/crates/uv/tests/it/common/mod.rs#L1017

and I'm supportive of expanding our `XDG_*` coverage

https://github.com/astral-sh/uv/blob/681e8e060fdc4ca2d0a180159ce63a720d3b93cf/crates/uv/tests/it/common/mod.rs#L1020-L1023

---

_Comment by @musicinmybrain on 2026-01-16 14:21_

Aha, I was struggling with this in Fedora as well, although I didn’t have nearly as many failing tests. Thank you for reporting it. It looks like passing through `PATH` (#17515) will be sufficient for the way we build packages in Fedora. I also needed #17520.

---

_Comment by @mgorny on 2026-01-16 16:15_

Uh, now I'm seeing failures again, even with all the variables I've listed. Either I've done something wrong over the way, or there are new regressions on main. Lemme rollback to the previous commit and test again.

---

_Comment by @konstin on 2026-01-16 16:46_

Is the test harness you're using locally runnable (e.g. a docker container)? That way I could reproduce the failures you are seeing.

---

_Comment by @mgorny on 2026-01-16 16:51_

Well, yes, but not that easily. You could start with the Gentoo docker image and use `emerge` to run the test suite; but it'd have to install all the test deps, and I'm not sure if we have binary package of them all.

That said, I can try to make a `Dockerfile` to give you a fully prepared image later today.

---

_Comment by @mgorny on 2026-01-16 17:46_

Okay, so with `PATH` and `XDG_CONFIG_DIRS` I'm down to 27 failures. `HOME` fixes one test, so I suppose we can just look into figuring out why the existing logic doesn't apply to it. I'll try the patch from #17515 now, and then try to figure out why I still have 26 failures.

---

_Comment by @mgorny on 2026-01-16 17:59_

With #17515, it's down to the one test failing over `HOME`:

```
---- publish::read_index_credential_env_vars_for_check_url stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Publishing 1 file to http://127.0.0.1:43805/upload
Uploading astral_test_private-0.1.0-py3-none-any.whl (1.1KiB)
error: Failed to publish `dist/astral_test_private-0.1.0-py3-none-any.whl` to http://127.0.0.1:43805/upload
  Caused by: Failed to send POST request
  Caused by: Missing credentials for http://127.0.0.1:43805/upload

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Publishing 1 file to http://127.0.0.1:43805/upload
error: Failed to query check URL
  Caused by: Failed to write to the client cache
  Caused by: failed to create directory `/var/lib/portage/home/.cache/uv/simple-v18/index/cbbfa8f4a87b6c73`: Permission denied (os erro
r 13)

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: read_index_credential_env_vars_for_check_url-2
Source: crates/uv/tests/it/publish.rs:481
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1       │-success: true
    2       │-exit_code: 0
          1 │+success: false
          2 │+exit_code: 2
    3     3 │ ----- stdout -----
    4     4 │ 
    5     5 │ ----- stderr -----
    6     6 │ Publishing 1 file to http://[LOCALHOST]/upload
    7       │-File astral_test_private-0.1.0-py3-none-any.whl already exists, skipping
          7 │+error: Failed to query check URL
          8 │+  Caused by: Failed to write to the client cache
          9 │+  Caused by: failed to create directory `/var/lib/portage/home/.cache/uv/simple-v18/index/cbbfa8f4a87b6c73`: Permission d
enied (os error 13)
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'publish::read_index_credential_env_vars_for_check_url' (66141) panicked at /var/tmp/portage/dev-python/uv-9999/work/cargo_home/
gentoo/insta/src/runtime.rs:708:13:
snapshot assertion for 'read_index_credential_env_vars_for_check_url-2' failed in line 481
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    publish::read_index_credential_env_vars_for_check_url

```

---

_Comment by @mgorny on 2026-01-16 19:54_

> Is the test harness you're using locally runnable (e.g. a docker container)? That way I could reproduce the failures you are seeing.

Okay, I've pushed an image, though it's not really optimal (as in quite big): https://hub.docker.com/repository/docker/mgorny/gentoo-uv-test/general

The Dockerfile's:

```
FROM gentoo/stage3:latest

RUN emerge-webrsync
RUN getuto
# use the git version
RUN echo 'dev-python/uv **' > /etc/portage/package.accept_keywords/uv
RUN emerge -1vg --jobs --onlydeps --with-test-deps=y dev-python/uv
#CMD FEATURES=test ALLOW_TEST=network emerge -1v dev-python/uv
```

I built it without `CMD`, so you can `docker run -it` and play with the shell. It's got full package repository ready to use.

To test and install latest uv git:

```
FEATURES=test ALLOW_TEST=network emerge -1v dev-python/uv
```

Note that some failures will only happen when `uv` is installed already (i.e. those caused by global config).

---

_Comment by @mgorny on 2026-01-21 05:10_

Thanks. Now we're down to `publish::read_index_credential_env_vars_for_check_url` as noted in https://github.com/astral-sh/uv/issues/17509#issuecomment-3761184268. Any chance you could look at that one?

---
