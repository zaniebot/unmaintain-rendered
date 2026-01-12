```yaml
number: 7422
title: Print installation path
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
draft: true
base: main
head: print-installation-path
created_at: 2024-09-16T11:14:20Z
updated_at: 2024-10-01T15:49:46Z
url: https://github.com/astral-sh/uv/pull/7422
synced_at: 2026-01-12T16:07:49Z
```

# Print installation path

---

_@Aditya-PS-05_

closes #2155 
credits #4835 for reference

## Summary
Enhances logging to print environment and venv paths for better debugging and traceability and updates the test suite to ensure compatibility with recent code changes .
Test Plan

Validated by running the updated tests in my local terminal to confirm correct logging of paths and ensure compatibility with recent code changes.


---

_Comment by @zanieb on 2024-09-16 22:29_

Hi! It looks like this uses some of the code from https://github.com/astral-sh/uv/pull/4835 without crediting it? Did you try to address some of the open comments there?

Also note this makes a bunch of code changes unrelated to the goal which will prevent us from reviewing it.

---

_Converted to draft by @zanieb on 2024-09-16 22:29_

---

_Comment by @Aditya-PS-05 on 2024-09-18 14:05_

> Hi! It looks like this uses some of the code from #4835 without crediting it? Did you try to address some of the open comments there?
> 
> Also note this makes a bunch of code changes unrelated to the goal which will prevent us from reviewing it.

Sorry I didn't know about giving credits. 
Updated the message and gave credits.

I will update the code as soon as possible and remove the unrelated code changes. Well these changes I had to make so run the test cases, as I was not able to run them.

---

_Comment by @zanieb on 2024-10-01 15:49_

Unfortunately this still looks out of date and we have a lot of pull requests to manage so I'm going to close this.

---

_Closed by @zanieb on 2024-10-01 15:49_

---
