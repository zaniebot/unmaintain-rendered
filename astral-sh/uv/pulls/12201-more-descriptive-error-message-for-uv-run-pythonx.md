```yaml
number: 12201
title: "More descriptive error message for `uv run pythonx.xx`"
type: pull_request
state: open
author: JWLee89
labels:
  - error messages
assignees: []
base: main
head: issue-11796
created_at: 2025-03-16T12:12:47Z
updated_at: 2025-08-19T03:13:23Z
url: https://github.com/astral-sh/uv/pull/12201
synced_at: 2026-01-12T16:10:10Z
```

# More descriptive error message for `uv run pythonx.xx`

---

_@JWLee89_

## Related Issue 

https://github.com/astral-sh/uv/issues/11796

## Summary

1. Add meaningful message when user invokes uv with `run python<valid-python-version>`. 

 ✅ "python", "python3", "python3.9", "python4", "python3.10", "python3.13.3", "python39" (yes, python39 might still be released in 100 years 😆 )
 ❌ "python3abc", "python3.12b3", "", "python-foo"
 
 2. Contextual information added depending on whether the command was run inside of a `uv project` or not. See: https://github.com/astral-sh/uv/pull/12201/files#diff-423c3a82aab6dba4068dd7675391fc9328a66682131dbf2043e7847ab4ba8661R1092
 
 ## Automated Tests

1. Added unit test for new functions created for parsing and validating python executable entered by user
2. TODO: Once specs are finalized through discussions in PR, will create integration tests if necessary
 
 ## Need to Discuss

1. Invoking python via its patch version - E.g. `python3.11.9` is invalid. However, uv supports `uv run -p 3.11.9 python`, which is why when we do the following: 

```python
uv run python3.11.9
```

We will get the following error:

```shell
error: Failed to spawn: `python3.11.9`
  Caused by: `python3.11.9` not available in the environment, which uses python `3.11.9`. Did you mean to search for a Python 3.11.9 environment with `uv run -p 3.11.9 python`?
```

Is this okay? If not, we need to define the specs in the PR here.

2. Supported python executable specs are defined as: 
- is stable version
- is not `post` version

See:https://github.com/astral-sh/uv/pull/12201/files#diff-423c3a82aab6dba4068dd7675391fc9328a66682131dbf2043e7847ab4ba8661R1572-R1574

Would this be okay?
We can change the specs if needed. Lets discuss.

## Manual Test

### Case 1: run outside of project

1. Case where no project is created. Look for a python executable version that is not available in my local machine (in my case, it was `python3.9`)
4. Run `cargo run -- run python3.9.3`

```Shell
error: Failed to spawn: `python3.9.3`
  Caused by: `python3.9.3` not available in the environment, which uses python `3.11.9`. Did you mean to search for a Python 3.9.3 environment with `uv run -p 3.9.3 python`?
```

5.  Run `cargo run -- run python3.9`

```shell
error: Failed to spawn: `python3.9`
  Caused by: `python3.9` not available in the environment, which uses python `3.11.9`. Did you mean to search for a Python 3.9 environment with `uv run -p 3.9 python`?
```

6. Run `cargo run -- run python3.11`

```Shell
Running `target/debug/uv run python3.11`
Python 3.11.9 (main, Apr  2 2024, 08:25:04) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

7. Run `cargo run -- run python3.11.9`

```Shell
 Running `target/debug/uv run python3.11.9`
error: Failed to spawn: `python3.11.9`
  Caused by: `python3.11.9` not available in the environment, which uses python `3.11.9`. Did you mean to search for a Python 3.11.9 environment with `uv run -p 3.11.9 python`?
```

**IMPORTANT**: Is this acceptable? Invoking python by its patch version is not supported, so this is the expected behavior, but we need to discuss whether this behavior is acceptable.

### Case 2: run inside of project

Create project using `uv init example-app`

1. `uv run python3.13` 

```shell
error: Failed to spawn: `python3.13`
  Caused by: `python3.13` not available in the project environment, which uses python `3.12.5`. Did you mean to change the environment to Python 3.13 with `uv run -p 3.13 python`?
```

2. `uv run python3` 

```shell
uv run python3
Python 3.12.5 (main, Aug  6 2024, 19:08:49) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```


---

_Assigned to @zanieb by @charliermarsh on 2025-03-16 17:40_

---

_Comment by @zanieb on 2025-04-16 15:36_

Hey @JWLee89, thanks for the contribution! This looks like a nice improvement. We can provide some feedback to unblock adding tests.

---

_@zanieb reviewed on 2025-04-16 15:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1110 on 2025-04-16 15:38_

Should `is_python_executable` be `python_executable_version -> Some(str)` instead? Then you can avoid the `unwrap_or`?

---

_Comment by @zanieb on 2025-04-16 15:40_

I think the semantics make sense here. I wonder if we can reuse anything from the `uv-python` crate, but nothing obvious comes to mind.

---

_Label `error messages` added by @zanieb on 2025-04-16 15:40_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-16 15:45_

---

_@jtfmumm reviewed on 2025-04-16 16:00_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1110 on 2025-04-16 16:00_

Maybe `specified_version` instead of `version_part`?

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1628 on 2025-04-16 16:04_

Since this is likely a typo and we probably don't have to worry about the distant future at the moment, should we detect cases over 37 and treat them as typos? Not necessary for this PR though

---

_@jtfmumm reviewed on 2025-04-16 16:04_

---

_@zanieb reviewed on 2025-04-16 16:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1585 on 2025-04-16 16:06_

Worth noting this isn't sufficient for Windows, where you'll have a `.exe` suffix (though versioned executables are not common there)

---

_Review comment by @JWLee89 on `crates/uv/src/commands/project/run.rs`:1585 on 2025-04-19 06:40_

@zanieb 

Thank you for the followup!

## Windows handling logic

Good point! I don't a windows machine available to me right now, but my guess is that the access pattern for invoking python will be something like the following: 

```Shell
# versioned executable
python3.11.exe
# General 
python.exe
```

Would this be correct? 

A simple approach could be also stripping suffix `.exe`. I don't think we should introduce complexity by checking whether the host is running windows unless this contextual information is already available without extra compute somewhere else.

## Additional Possible Cases for Windows.

From some quick ChatGPT search, I found the following: 

|   Command   |                                Notes                               |
|:-----------:|:------------------------------------------------------------------:|
| python      | After installing via the official installer, adds to PATH.         |
| py          | Python launcher for Windows. Supports version flags like py -3.11. |
| python.exe  | Explicit executable, useful in scripts or batch files.             |
| py -X.Y     | e.g., py -3.10 to launch a specific version via Python launcher.   |
| pythonw.exe | Launches Python without a console window (GUI apps).               |

Out of these items above, do we need to support the py launcher (E.g. `py`)? Just wanted to be sure :) 

## Edge cases

After thinking I just realized that people might invoke python using the following paths: 

```
# absolute path
/usr/bin/python3
# venv path 
/Users/username/repos/uv/venv/bin/python3.10
```

Should these kinds of absolute path specifications also be covered by this feature update? 

CC: @jtfmumm 

Thank you!

---

_@JWLee89 reviewed on 2025-04-19 06:40_

---

_@JWLee89 reviewed on 2025-04-19 06:53_

---

_Review comment by @JWLee89 on `crates/uv/src/commands/project/run.rs`:1110 on 2025-04-19 06:53_

Applied: https://github.com/astral-sh/uv/pull/12201/commits/e4a2fa1c1335af89e960036526d8456baef25fd9

---

_@JWLee89 reviewed on 2025-04-19 06:54_

---

_Review comment by @JWLee89 on `crates/uv/src/commands/project/run.rs`:1628 on 2025-04-19 06:54_

I think this can be a good idea, especially since python version `37` will not likely be released anytime in the future while we are still alive 😆 .

Still I think we should carefully discuss this and come up with a reasonable spec before making any feature changes.

Would it be okay if I create a separate PR for this once we discuss and decide on specifications here?

---

_@JWLee89 reviewed on 2025-04-20 07:31_

---

_Review comment by @JWLee89 on `crates/uv/src/commands/project/run.rs`:1110 on 2025-04-20 07:31_

Applied: https://github.com/astral-sh/uv/pull/12201/commits/0cb2d5e35dddaabf6eef93410643bbc58e8098fd

---

_@JWLee89 reviewed on 2025-04-20 13:21_

---

_Review comment by @JWLee89 on `crates/uv/src/commands/project/run.rs`:1585 on 2025-04-20 13:21_

Applied here: https://github.com/astral-sh/uv/pull/12201/commits/e6a801d0fe3deb62318b1e5aed5a18a3e350517b

Specs include: 
- strip `.exe` suffix
- Support absolute path python. E.g. `/Users/username/repos/uv/venv/bin/python3.10` 

Since spec was defined on the fly, might be good to review and discuss this together. 

Thank you! 🙇 

---

_Review requested from @zanieb by @JWLee89 on 2025-04-20 13:21_

---

_Review requested from @jtfmumm by @JWLee89 on 2025-04-20 13:21_

---

_Renamed from "Issue-11796: Provide more descriptive message for uv run pythonx.xx" to "Provide more descriptive message for uv run pythonx.xx" by @konstin on 2025-04-24 13:17_

---

_Renamed from "Provide more descriptive message for uv run pythonx.xx" to "More descriptive error message for `uv run pythonx.xx`" by @konstin on 2025-04-24 13:18_

---

_Comment by @jtfmumm on 2025-04-24 16:15_

I pushed some snapshot (integration) tests for a handful of cases. 

I think we might want to update the error messages for `uv run pythonX.X.X` (the case you called out in your description). At least when it matches the patch version in use, it's a bit confusing. Though it's nice to have the rest of the message when it's not matched. But it seems like an opportunity to indicate that this is not expected usage. The first snapshot test I added captures this behavior.

---

_Comment by @JWLee89 on 2025-04-26 08:43_

> I pushed some snapshot (integration) tests for a handful of cases.

Thank you so much for adding the snapshot (integration) tests! 🙇 
I will keep this in mind when working on future PRs to add integration tests when necessary. 

> I think we might want to update the error messages for `uv run pythonX.X.X` (the case you called out in your description). At least when it matches the patch version in use, it's a bit confusing. Though it's nice to have the rest of the message when it's not matched. But it seems like an opportunity to indicate that this is not expected usage. The first snapshot test I added captures this behavior.

I absolutely agree and thank you for bringing this up! 
Applied: https://github.com/astral-sh/uv/pull/12201/commits/99765005f3ef2e5e4da44a83a300980bdcd49b42

I should have clarified about this point properly in this PR. 
In my environment, I reproduced it with the following command: 

```bash
uv run python3.13.2
```

Results:

```bash
`python3.13.2` not available in the virtual environment, which uses python `3.13.2`. Did you mean to search for a Python 3.13.2 environment with `uv run -p 3.13.2 python`?
```

## Description of update

When users specify up to the patch version: 

```bash
uv run python3.3.3
```

I updated so that if users specify up to the patch version. E.g. `python3.12.x`, we get a error message that tells users to either use `uv run -p` or use `uv run` without the patch version (see below). 

```bash
error: Failed to spawn: `python3.3.3`
  Caused by: Please omit patch version. Try: `uv run python3.3`. Did you mean to search for a Python 3.3.3 environment with `uv run -p 3.3.3 python`?
```

However, if we omit patch version, we will get the following (same behavior as before): 

```bash
error: Failed to spawn: `python3.3`
  Caused by: `3.3` not available in the virtual environment, which uses python `3.13.2`. Did you mean to search for a Python 3.3 environment with `uv run -p 3.3 python`?
```

**Note**: message is subject to discussion and I think after iterating together, we can come up with the finalized message soon :). 
For now, I just want to emphasize that I implemented the logic to change the error message depending on whether patch version is omitted or not.

---

_Comment by @JWLee89 on 2025-05-22 01:34_

Hi @jtfmumm and @zanieb , 

Sorry for the ping ~ I know that you are both very busy. 
Just wanted to touch base and see if there are any changes / improvements that should be made to this PR 😄 . 

P.S. No rush, please respond only when you have time to review.

Thank you!

---

_Comment by @JWLee89 on 2025-06-09 04:32_

Hello @jtfmumm and @zanieb,
 
Just wanted to follow up and double-check if there is any other items I can add to this PR to improve it. 
No rush: just a gentle followup (I know that you are both very busy).

Thank you!

---

_Comment by @jtfmumm on 2025-06-09 11:50_

Hi @JWLee89. Thanks for your patience on this PR. We still need to make a decision on the messaging. I'll follow up with you.

---

_Comment by @JWLee89 on 2025-06-10 06:34_

> Hi @JWLee89. Thanks for your patience on this PR. We still need to make a decision on the messaging. I'll follow up with you.

Hi @jtfmumm ,

Thank you for the update! 👍 

---

_Comment by @JWLee89 on 2025-08-19 03:13_

Hello @jtfmumm @zanieb , 

Just wanted to check if there has been any updates on decision-making for this feature. 
No need to respond right away, I know that you both are very busy 😄 . 

Thank you!

---
