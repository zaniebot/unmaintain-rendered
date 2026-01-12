```yaml
number: 13746
title: "Failure to connect with \"No route to host\" in \"Test in Firecracker VM\" on `build binary | freebsd` CI test"
type: issue
state: closed
author: jtfmumm
labels:
  - ci-flake
assignees: []
created_at: 2025-05-30T22:10:47Z
updated_at: 2025-06-03T13:12:04Z
url: https://github.com/astral-sh/uv/issues/13746
synced_at: 2026-01-12T16:01:36Z
```

# Failure to connect with "No route to host" in "Test in Firecracker VM" on `build binary | freebsd` CI test

---

_@jtfmumm_

```
iptables: Bad rule (does a matching rule exist in that chain?).
iptables: Bad rule (does a matching rule exist in that chain?).
iptables: Bad rule (does a matching rule exist in that chain?).
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host [172](https://github.com/astral-sh/uv/actions/runs/15356286879/job/43215938691?pr=13595#step:5:174).16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
ssh: connect to host 172.16.0.2 port 22: No route to host
Run firecracker_pid=$(cat /tmp/firecracker.pid)
ssh: connect to host 172.16.0.2 port 22: No route to host
scp: Connection closed
Error: Process completed with exit code 255.
```

For example: https://github.com/astral-sh/uv/actions/runs/15356286879/job/43215938691?pr=13595

---

_Label `ci-flake` added by @jtfmumm on 2025-05-30 22:10_

---

_Comment by @zanieb on 2025-05-31 04:51_

Oh weird, I don't know if I've seen this before? Should we open an issue on the firecracker action?

---

_Comment by @konstin on 2025-06-02 12:11_

Reported upstream: https://github.com/acj/freebsd-firecracker-action/issues/3

---

_Closed by @konstin on 2025-06-03 13:12_

---
