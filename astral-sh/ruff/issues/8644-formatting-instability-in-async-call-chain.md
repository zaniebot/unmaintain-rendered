```yaml
number: 8644
title: "Formatting instability in `async` call chain"
type: issue
state: closed
author: roshanjrajan-zip
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-11-13T05:03:19Z
updated_at: 2023-11-17T17:24:41Z
url: https://github.com/astral-sh/ruff/issues/8644
synced_at: 2026-01-12T15:54:48Z
```

# Formatting instability in `async` call chain

---

_@roshanjrajan-zip_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I don't know if this is expected but I am getting an error when formatting the following code. I would expect Ruff to come to a solution and not have differences when running with --no-cache or not.

Ruff Settings: Nothing specific for format but have line-length set to 88.
Ruff Version: v0.1.4/0.1.5

Steps to reproduce:
1) Create file called test.py with the following code
```python
# test.py
def func():
    test_data = await (
        Stream.from_async(async_data.data())
        .flat_map_async(lambda data: data.async_data())
        .map(lambda data: data.data())
        .filter_async(is_valid_data)
        .to_list()
    )
```

2) Call `ruff format test.py --isolated `. Can call this repeatedly and it stays the same as the following.
```python
def func():
    test_data = await Stream.from_async(async_data.data()).flat_map_async(
        lambda data: data.async_data()
    ).map(lambda data: data.data()).filter_async(is_valid_data).to_list()
```

3) Now if I run this code in CI where it runs the github action on v0.1.4 with `format check` it fails. When I call `ruff format test.py --isolated --no-cache` it formats the code to the following.
```python
def func():
    test_data = (
        await Stream.from_async(async_data.data())
        .flat_map_async(lambda data: data.async_data())
        .map(lambda data: data.data())
        .filter_async(is_valid_data)
        .to_list()
    )
```

This is stable and all calls to ruff format w/o the --no-cache flag do not reformat the file. This also allows it to pass in CI with the format check. Any help/pointers on how to make sure this is stable is greatly appreciated!


---

_Renamed from "Formatter: Using no_cache formats code differently which impacts formatting on other machines/CI" to "Formatter: Using --no-cache formats code differently which impacts formatting on other machines/CI" by @roshanjrajan-zip on 2023-11-13 05:08_

---

_Comment by @charliermarsh on 2023-11-13 05:11_

I think the issue is that there's an instability in the formatting:

```python
# test.py
def func():
    test_data = await (
        Stream.from_async(async_data.data())
        .flat_map_async(lambda data: data.async_data())
        .map(lambda data: data.data())
        .filter_async(is_valid_data)
        .to_list()
    )

# Format once...
def func():
    test_data = await Stream.from_async(async_data.data()).flat_map_async(
        lambda data: data.async_data()
    ).map(lambda data: data.data()).filter_async(is_valid_data).to_list()

# Format twice...
def func():
    test_data = (
        await Stream.from_async(async_data.data())
        .flat_map_async(lambda data: data.async_data())
        .map(lambda data: data.data())
        .filter_async(is_valid_data)
        .to_list()
    )
```

It's just a bug in the formatter. Will need to ensure it's fixed for the next release. Thanks!

(If you want a workaround, I'd suggest just applying that final formatting, which Ruff will retain.)

---

_Label `bug` added by @charliermarsh on 2023-11-13 05:11_

---

_Label `formatter` added by @charliermarsh on 2023-11-13 05:11_

---

_Comment by @roshanjrajan-zip on 2023-11-13 06:03_

Will do and thanks for looking into this! Really appreciate the great work you are doing! 🔥 

---

_Renamed from "Formatter: Using --no-cache formats code differently which impacts formatting on other machines/CI" to "Formatting instability in async call chain" by @charliermarsh on 2023-11-13 15:04_

---

_Renamed from "Formatting instability in async call chain" to "Formatting instability in `async` call chain" by @charliermarsh on 2023-11-13 15:04_

---

_Comment by @charliermarsh on 2023-11-17 17:24_

Closed by https://github.com/astral-sh/ruff/pull/8676.

---

_Closed by @charliermarsh on 2023-11-17 17:24_

---
