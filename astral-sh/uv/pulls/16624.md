```yaml
number: 16624
title: Handle externally managed system Pythons when validating global installs
type: pull_request
state: closed
author: terror
labels: []
assignees: []
base: main
head: handle-externally-managed-pythons-ci
created_at: 2025-11-06T22:29:20Z
updated_at: 2025-11-07T16:11:01Z
url: https://github.com/astral-sh/uv/pull/16624
synced_at: 2026-01-10T06:28:12Z
```

# Handle externally managed system Pythons when validating global installs

---

_Pull request opened by @terror on 2025-11-06 22:29_

This PR auto-detects `EXTERNALLY-MANAGED` marker files before running the “validate global Python install” script and append `--break-system-packages` to every `uv pip ... --system` call when needed. This lets the CI step keep exercising the real system interpreter (including Homebrew’s Python 3.14 on macOS) while staying compliant with [PEP 668](https://peps.python.org/pep-0668/) protections, and it avoids requiring job-level flags or hard-coded platform checks.

This aims to fix the `check system | python on macos x86-64` / `Validate global Python install` CI step failing for new branches.

---

_@terror reviewed on 2025-11-06 22:47_

---

_Review comment by @terror on `scripts/check_system_python.py`:20 on 2025-11-06 22:47_

If other system Python's become externally managed (e.g. on other runners) this detection will detect that and auto-pass in `--break-system-packages` without us having to pass in `--allow-externally-managed` directly.

We _could_ have passed in `--externally-managed` for this specific case, but that seems a bit brittle to me. Open to hearing other opinions!

This change also means that the `--externally-managed` flag is now redundant (and only serves the purpose of being explicit when we pass it in).

---

_Comment by @zanieb on 2025-11-07 04:16_

Thanks for investigating this!

I like this idea as a solution for reducing maintenance, but I think we could miss changes to the intent of the test if we infer it, e.g., this marker is in a Homebrew Python install but the test explicitly is for GitHub's Python? (see https://github.com/astral-sh/uv/pull/16628#issuecomment-3500617033). It's unclear to me that it's worth inferring it since these are nearly always static, but I could be convinced.

---

_Comment by @terror on 2025-11-07 05:53_

> Thanks for investigating this!
> 
> I like this idea as a solution for reducing maintenance, but I think we could miss changes to the intent of the test if we infer it, e.g., this marker is in a Homebrew Python install but the test explicitly is for GitHub's Python? (see [#16628 (comment)](https://github.com/astral-sh/uv/pull/16628#issuecomment-3500617033)). It's unclear to me that it's worth inferring it since these are nearly always static, but I could be convinced.

I'm operating under the assumption that any externally managed interpreter will need `--break-system-packages` to run this check, so this removes a manual step if any of them change. 

> but I think we could miss changes to the intent of the test if we infer it

I'm a bit confused here, the detection doesn't change which Python we test, it only asks that Python whether it requires `--break-system-packages`. I could see a scenario where we may _not_ want detection, but for this specific check I'm not sure we ever would?

> since these are nearly always static

I'm not too familiar with how often these change, but if this is the case and the maintenance burden is tolerated then I could see why it may not be worth it.

---

_Comment by @zanieb on 2025-11-07 05:57_

> I'm a bit confused here, the detection doesn't change which Python we test, it only asks that Python whether it requires --break-system-packages. I could see a scenario where we may not want detection, but for this specific check I'm not sure we ever would?

I'll try to explain in a bit more detail.

The intent of the tests is to check that we properly detect and install into system Python versions in various contexts. If we're selecting a system Python version which has suddenly gained an `EXTERNALLY-MANAGED` marker, that _could_ mean we're no longer testing against the interpreter we thought we were — at the very least, something changed about the test environment.

I think it's probably _useful_ for us to know that GitHub's macos-15-intel runner now requires this flag for mutating system requirements whereas it didn't a week ago. If we silently infer that, then we lose that information. 

---

_Comment by @terror on 2025-11-07 06:14_

> > I'm a bit confused here, the detection doesn't change which Python we test, it only asks that Python whether it requires --break-system-packages. I could see a scenario where we may not want detection, but for this specific check I'm not sure we ever would?
> 
> I'll try to explain in a bit more detail.
> 
> The intent of the tests is to check that we properly detect and install into system Python versions in various contexts. If we're selecting a system Python version which has suddenly gained an `EXTERNALLY-MANAGED` marker, that _could_ mean we're no longer testing against the interpreter we thought we were — at the very least, something changed about the test environment.
> 
> I think it's probably _useful_ for us to know that GitHub's macos-15-intel runner now requires this flag for mutating system requirements whereas it didn't a week ago. If we silently infer that, then we lose that information.

Interesting I hadn't thought about it that way. There would still be _some_ [signal](https://github.com/astral-sh/uv/actions/runs/19151813166/job/54743833055?pr=16624#step:7:12) with the log, but failing is definitely more attention grabbing 😅.

---

_Comment by @terror on 2025-11-07 16:10_

Closing this in favour of supplying manual `--break-system-packages` when necessary. See above discussion for more context!

---

_Closed by @terror on 2025-11-07 16:10_

---

_Branch deleted on 2025-11-07 16:11_

---
