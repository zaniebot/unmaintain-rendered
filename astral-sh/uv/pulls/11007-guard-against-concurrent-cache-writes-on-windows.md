```yaml
number: 11007
title: Guard against concurrent cache writes on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/windows
created_at: 2025-01-27T22:59:57Z
updated_at: 2025-01-28T20:34:12Z
url: https://github.com/astral-sh/uv/pull/11007
synced_at: 2026-01-12T16:09:37Z
```

# Guard against concurrent cache writes on Windows

---

_@charliermarsh_

## Summary

On Windows, we have a lot of issues with atomic replacement and such. There are a bunch of different failure modes, but they generally involve: trying to persist a fail to a path at which the file already exists, trying to replace or remove a file while someone else is reading it, etc.

This PR adds locks to all of the relevant database paths. We already use these advisory locks when building source distributions; now we use them when unzipping wheels, storing metadata, etc.

Closes #11002.

## Test Plan

I ran the following script:

```shell
# Define the cache directory path
$cacheDir = "C:\Users\crmar\workspace\uv\cache"

# Clear the cache directory if it exists
if (Test-Path $cacheDir) {
    Remove-Item -Recurse -Force $cacheDir
}

# Create the cache directory again
New-Item -ItemType Directory -Force -Path $cacheDir

# Define the command to run with --cache-dir flag
$command = {
    param ($venvPath)

    # Create a virtual environment in the specified path with --python
    uv venv $venvPath

    # Run the pip install command with --cache-dir flag
    C:\Users\crmar\workspace\uv\target\profiling\uv.exe pip install flask==1.0.4 --no-binary flask --cache-dir C:\Users\crmar\workspace\uv\cache -v --python $venvPath
}

# Define the paths for the different virtual environments
$venv1 = "C:\Users\crmar\workspace\uv\venv1"
$venv2 = "C:\Users\crmar\workspace\uv\venv2"
$venv3 = "C:\Users\crmar\workspace\uv\venv3"
$venv4 = "C:\Users\crmar\workspace\uv\venv4"
$venv5 = "C:\Users\crmar\workspace\uv\venv5"

# Start the command in parallel five times using Start-Job, each with a different venv
$job1 = Start-Job -ScriptBlock $command -ArgumentList $venv1
$job2 = Start-Job -ScriptBlock $command -ArgumentList $venv2
$job3 = Start-Job -ScriptBlock $command -ArgumentList $venv3
$job4 = Start-Job -ScriptBlock $command -ArgumentList $venv4
$job5 = Start-Job -ScriptBlock $command -ArgumentList $venv5

# Wait for all jobs to complete
$jobs = @($job1, $job2, $job3, $job4, $job5)
$jobs | ForEach-Object { Wait-Job $_ }

# Retrieve the results (optional)
$jobs | ForEach-Object { Receive-Job -Job $_ }

# Clean up the jobs
$jobs | ForEach-Object { Remove-Job -Job $_ }
```

And ensured it succeeded in five straight invocations (whereas on `main`, it consistently fails with a variety of different traces).


---

_Review requested from @Gankra by @charliermarsh on 2025-01-27 23:00_

---

_Review requested from @konstin by @charliermarsh on 2025-01-27 23:00_

---

_Label `bug` added by @charliermarsh on 2025-01-27 23:00_

---

_Label `windows` added by @charliermarsh on 2025-01-27 23:00_

---

_Comment by @charliermarsh on 2025-01-27 23:59_

I'll fix up the tests on approval -- they're just count offsets due to writing more files to the cache (sadly).

---

_Comment by @konstin on 2025-01-28 09:24_

Can you add the script you used to provoke the error and check that it's test? Otherwise we don't have a regression test should we ever have to revisit this code.

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:339 on 2025-01-28 09:24_

Can you extend the comment explaining the `[cfg(windows)]`?

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:52 on 2025-01-28 09:26_

Do i read this comment correctly that there is no way for us to make this atomic?

---

_@konstin approved on 2025-01-28 09:52_

---

_@charliermarsh reviewed on 2025-01-28 14:03_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:52 on 2025-01-28 14:03_

I can try, but making it atomic doesn't solve the problem. We could still run into issues whereby we attempt to overwrite the file while it's open, right?

---

_Comment by @charliermarsh on 2025-01-28 14:03_

> Can you add the script you used to provoke the error and check that it's test? Otherwise we don't have a regression test should we ever have to revisit this code.

Sorry, do you mean, add a test to the codebase itself? The script I used is in the PR summary.

---

_@konstin reviewed on 2025-01-28 14:17_

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:52 on 2025-01-28 14:17_



That's good to know, i didn't think of that case.


---

_Comment by @konstin on 2025-01-28 14:22_

Sorry, totally overlooked that. Is it possible to port this to an integration test, or alternatively could we add that script to `scripts/`?

---

_@zanieb approved on 2025-01-28 14:23_

---

_@charliermarsh reviewed on 2025-01-28 14:26_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:52 on 2025-01-28 14:26_

It's somewhat tragic. I think we should still try to make this atomic though.

---

_Merged by @charliermarsh on 2025-01-28 20:33_

---

_Closed by @charliermarsh on 2025-01-28 20:33_

---

_Branch deleted on 2025-01-28 20:33_

---

_Comment by @charliermarsh on 2025-01-28 20:34_

@konstin -- I'm just gonna leave it in here for now. I think this is ~as good as putting it in `scripts` given that we'll never run it and it will likely bitrot there.

---
