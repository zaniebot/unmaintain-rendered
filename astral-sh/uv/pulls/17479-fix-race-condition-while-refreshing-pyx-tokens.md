```yaml
number: 17479
title: Fix race condition while refreshing pyx tokens
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
base: main
head: claude/credential-helper-token-refresh-oq5GB
created_at: 2026-01-14T23:39:06Z
updated_at: 2026-01-15T15:41:18Z
url: https://github.com/astral-sh/uv/pull/17479
synced_at: 2026-01-15T15:50:34Z
```

# Fix race condition while refreshing pyx tokens

---

_@zanieb_

Prevent concurrent processes from all refreshing the token simultaneously by using a file lock. When a refresh is needed:

1. Acquire an exclusive file lock (which will block if another process is refreshing)
3. Re-read the tokens from disk and check if they are fresh enough, instead of making another request
4. Otherwise, perform the refresh and update the cached tokens

This prevents thundering herd issues when multiple concurrent processes are invoking `uv auth token` or `uv auth helper` at the same time.

Also, set the tolerance for `uv auth helper` to the same as `uv auth token` (5 minutes).

---

_Comment by @zanieb on 2026-01-15 00:12_

cc @charliermarsh I think this is probably sound even if it's not causing the Bazel issue @zsol is going to look into?

---

_Label `bug` added by @zanieb on 2026-01-15 05:45_

---

_Review comment by @zsol on `crates/uv-auth/src/pyx.rs`:414 on 2026-01-15 09:54_

I would version the contents of this lockfile, just in case we need to change its contents in future releases. It could be as simple as the first 2 bytes containing a version marker, followed by content

---

_@zsol reviewed on 2026-01-15 09:54_

---

_Comment by @zsol on 2026-01-15 09:58_

This seems to resolve the issue I'm seeing, verified with the following simple script:
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

On my machine, calling this with `uv run $script -n 300` would consistently fail on uv 0.9.25, but never with this PR applied.

However, with `-n 1000` I'm now running into the default `UV_LOCK_TIMEOUT` of five minutes

---

_Review comment by @zsol on `crates/uv/src/commands/auth/helper.rs`:95 on 2026-01-15 15:21_

let's set this to `false` and ship :)

---

_@zsol approved on 2026-01-15 15:21_

---

_Renamed from "Add debounce to pyx token refresh" to "Fix race condition while refreshing pyx tokens" by @zsol on 2026-01-15 15:34_

---

_Marked ready for review by @zsol on 2026-01-15 15:41_

---
