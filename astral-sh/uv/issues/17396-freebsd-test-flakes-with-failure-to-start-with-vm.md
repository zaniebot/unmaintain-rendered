```yaml
number: 17396
title: FreeBSD test flakes with failure to start with VM
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2026-01-09T21:59:00Z
updated_at: 2026-01-09T21:59:07Z
url: https://github.com/astral-sh/uv/issues/17396
synced_at: 2026-01-12T16:02:50Z
```

# FreeBSD test flakes with failure to start with VM

---

_@zanieb_

```
Run acj/freebsd-firecracker-action@a5a3fc1709c5b5368141a5699f10259aca3cd965
Run kernel_path="$(mktemp)"
iptables: Bad rule (does a matching rule exist in that chain?).
iptables: Bad rule (does a matching rule exist in that chain?).
iptables: Bad rule (does a matching rule exist in that chain?).
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
/home/runner/work/_temp/9ae60a13-0ce9-44d5-ac36-7bd30b2be349: line 123: kill: (3713) - No such process
Attempt 1: Firecracker VM did not start, retrying...
ℹ️ Firecracker VM log (attempt 1)
Removing stale API socket from previous run
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
Attempt 2: Firecracker VM did not start, retrying...
ℹ️ Firecracker VM log (attempt 2)
Removing stale API socket from previous run
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
/home/runner/work/_temp/9ae60a13-0ce9-44d5-ac36-7bd30b2be349: line 123: kill: (3772) - No such process
Attempt 3: Firecracker VM did not start, retrying...
ℹ️ Firecracker VM log (attempt 3)
❌ Failed to start Firecracker VM after multiple attempts. Please report this issue and include the Firecracker VM log below.
Error: Process completed with exit code 1.
Run echo "::group::ℹ️ Firecracker VM log (please link to this or attach the log contents when reporting a bug in this action)"
ℹ️ Firecracker VM log (please link to this or attach the log contents when reporting a bug in this action)
  cat: /tmp/firecracker.log: No such file or directory
  Error: Process completed with exit code 1.
```

---

_Label `ci-flake` added by @zanieb on 2026-01-09 21:59_

---
