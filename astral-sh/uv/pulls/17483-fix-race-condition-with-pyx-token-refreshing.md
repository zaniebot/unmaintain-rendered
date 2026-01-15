```yaml
number: 17483
title: Fix race condition with pyx token refreshing
type: pull_request
state: open
author: zsol
labels: []
assignees: []
draft: true
base: main
head: zsol/jj-kryxnlurmpxt
created_at: 2026-01-15T14:38:57Z
updated_at: 2026-01-15T14:44:52Z
url: https://github.com/astral-sh/uv/pull/17483
synced_at: 2026-01-15T14:51:33Z
```

# Fix race condition with pyx token refreshing

---

_@zsol_

## Summary

This PR fixes a race condition when refreshing pyx tokens in uv.

It's mostly plagiarized from #17479, but drops the debouncing bits. The logic for refreshing now looks like:
1. if the passed token is fresh enough, return it
2. acquire a lock
3. read the token from the cache and use that if _that_'s fresh enough (releasing the lock and returning)
4. do the refresh dance
5. write back the refreshed token to the cache
6. release the lock and return the refreshed token 

## Test Plan

Using this script with concurrency=300 no longer produces failures:
```python
#!/usr/bin/env python3
"""Request pyx auth tokens from uv in concurrent attempts."""

import argparse
import asyncio


async def get_auth_token(
    index: int, uv_binary: str
) -> tuple[int, str | None, str | None]:
    """Request an auth token from uv."""
    proc = await asyncio.create_subprocess_exec(
        uv_binary,
        "auth",
        "helper",
        "--preview-features",
        "auth-helper",
        "--protocol",
        "bazel",
        "get",
        stdin=asyncio.subprocess.PIPE,
        stdout=asyncio.subprocess.PIPE,
        stderr=asyncio.subprocess.PIPE,
    )
    input_json = b'{"uri": "https://api.pyx.dev/"}'
    stdout, stderr = await proc.communicate(input=input_json)

    if proc.returncode == 0:
        return (index, stdout.decode().strip(), None)
    else:
        return (index, None, stderr.decode().strip())


async def main(invocations: int, uv_binary: str) -> None:
    """Run n concurrent auth token requests."""
    print(f"Starting {invocations} concurrent auth token requests...")

    tasks = [get_auth_token(i, uv_binary) for i in range(invocations)]
    results = await asyncio.gather(*tasks)

    successes = 0
    failures = 0

    for index, token, error in results:
        if token:
            successes += 1
            # Only print first few characters of token for privacy.
            print(f"[{index:3d}] Success: {token[:5]}...")
        else:
            failures += 1
            print(f"[{index:3d}] Failed: {error}")

    print(f"\nResults: {successes} successes, {failures} failures")


if __name__ == "__main__":
    parser = argparse.ArgumentParser(
        description="Request PyX auth tokens from uv in concurrent attempts"
    )
    parser.add_argument(
        "-n",
        "--invocations",
        type=int,
        default=100,
        help="Number of concurrent requests (default: 100)",
    )
    parser.add_argument(
        "--uv",
        default="uv",
        help="Path to uv binary (default: uv)",
    )
    args = parser.parse_args()
    asyncio.run(main(args.invocations, args.uv))
```

---

_Renamed from "Help rustfmt a little (#17004)" to "Fix race condition with pyx token refreshing" by @zsol on 2026-01-15 14:39_

---

_@zsol reviewed on 2026-01-15 14:44_

---

_Review comment by @zsol on `crates/uv-auth/src/pyx.rs`:466 on 2026-01-15 14:44_

This seems a bit weird. We're only locking for a specific token, but `read()` can read "outside of the scope" of that lock. Should we just lock the entire subdirectory?

---
