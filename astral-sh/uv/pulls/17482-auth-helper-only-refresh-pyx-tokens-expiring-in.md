```yaml
number: 17482
title: "auth-helper: only refresh pyx tokens expiring in 30 minutes"
type: pull_request
state: open
author: zsol
labels:
  - performance
assignees: []
base: main
head: zsol/jj-purmswupkxso
created_at: 2026-01-15T10:27:47Z
updated_at: 2026-01-15T10:31:53Z
url: https://github.com/astral-sh/uv/pull/17482
synced_at: 2026-01-15T10:44:26Z
```

# auth-helper: only refresh pyx tokens expiring in 30 minutes

---

_@zsol_

## Summary

`uv auth helper` currently passes in a tolerance of zero seconds to `pyx_store.access_token()` which forces a refresh every time. That is excessive.

This PR changes that behavior to allow for a tolerance of 30 minutes. This is more than the default for `uv auth token` (5 minutes), because bazel builds might run for a long time, so it might take a while for the bazel-native downloader to get to actually fetching the file (although currently bazel seems to call the auth helper directly before attempting a fetch).

We might want to expose this as a CLI parameter in the future. 

## Test Plan

The following script exercises the credential helper concurrently:

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

Running this script with `-n 1000` takes about five minutes on my machine before this PR, and about two seconds after this PR.


---

_Label `performance` added by @zsol on 2026-01-15 10:28_

---

_Renamed from "auth-helper: don't force refresh the token" to "auth-helper: don't force refresh the pyx token" by @zsol on 2026-01-15 10:29_

---

_Renamed from "auth-helper: don't force refresh the pyx token" to "auth-helper: don't force refresh the pyx token unless it's about to expire in 30 minutes" by @zsol on 2026-01-15 10:30_

---

_Renamed from "auth-helper: don't force refresh the pyx token unless it's about to expire in 30 minutes" to "auth-helper: only refresh pyx tokens expiring in 30 minutes" by @zsol on 2026-01-15 10:30_

---

_Marked ready for review by @zsol on 2026-01-15 10:31_

---
