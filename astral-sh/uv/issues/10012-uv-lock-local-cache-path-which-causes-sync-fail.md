```yaml
number: 10012
title: uv.lock local cache path which causes sync fail when cache is empty
type: issue
state: closed
author: jhurlb
labels:
  - wontfix
assignees: []
created_at: 2024-12-18T23:50:18Z
updated_at: 2024-12-22T13:57:37Z
url: https://github.com/astral-sh/uv/issues/10012
synced_at: 2026-01-12T16:00:04Z
```

# uv.lock local cache path which causes sync fail when cache is empty

---

_@jhurlb_

# Environment
Platform: MacOS (M2)
Python: 3.13.1
UV: 0.5.10

# Commands Run

```
uv init

uv add git+https://github.com/vmware/vsphere-automation-sdk-python.git --tag v8.0.3.0

cat uv.lock | grep .cache
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-policy-python-sdk/nsx_policy_python_sdk-4.2.0-py2.py3-none-any.whl" }
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-python-sdk/nsx_python_sdk-4.2.0-py2.py3-none-any.whl" }
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-vmc-aws-integration-python-sdk/nsx_vmc_aws_integration_python_sdk-4.1.2.0.1-py2.py3-none-any.whl" }
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-vmc-policy-python-sdk/nsx_vmc_policy_python_sdk-4.1.2.0.1-py2.py3-none-any.whl" }
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/vmwarecloud-aws/vmwarecloud_aws-1.64.1-py2.py3-none-any.whl" }
  source = { path = "../../../.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/vmwarecloud-draas/vmwarecloud_draas-1.23.1-py2.py3-none-any.whl" }

rm -rf .venv

uv sync
Using CPython 3.13.1
Creating virtual environment at: .venv
Resolved 24 packages in 1ms
Installed 23 packages in 325ms
 + certifi==2024.12.14
 + cffi==1.17.1
 + charset-normalizer==3.4.0
 + cryptography==44.0.0
 + idna==3.10
 + lxml==5.3.0
 + nsx-policy-python-sdk==4.2.0 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-policy-python-sdk/nsx_policy_python_sdk-4.2.0-py2.py3-none-any.whl)
 + nsx-python-sdk==4.2.0 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-python-sdk/nsx_python_sdk-4.2.0-py2.py3-none-any.whl)
 + nsx-vmc-aws-integration-python-sdk==4.1.2.0.1 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-vmc-aws-integration-python-sdk/nsx_vmc_aws_integration_python_sdk-4.1.2.0.1-py2.py3-none-any.whl)
 + nsx-vmc-policy-python-sdk==4.1.2.0.1 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-vmc-policy-python-sdk/nsx_vmc_policy_python_sdk-4.1.2.0.1-py2.py3-none-any.whl)
 + pycparser==2.22
 + pyopenssl==24.3.0
 + pyvmomi==8.0.3.0.1
 + requests==2.32.3
 + setuptools==75.6.0
 + six==1.17.0
 + urllib3==2.2.3
 + vmware-vapi-common-client==2.52.0
 + vmware-vapi-runtime==2.52.0
 + vmware-vcenter==8.0.3.0
 + vmwarecloud-aws==1.64.1 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/vmwarecloud-aws/vmwarecloud_aws-1.64.1-py2.py3-none-any.whl)
 + vmwarecloud-draas==1.23.1 (from file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/vmwarecloud-draas/vmwarecloud_draas-1.23.1-py2.py3-none-any.whl)
 + vsphere-automation-sdk==1.87.0 (from git+https://github.com/vmware/vsphere-automation-sdk-python.git@0e0b9321122e404cb49f6812dcea5bea792c49b2)

rm -rf .venv

uv cache clean

uv sync
Using CPython 3.13.1
Creating virtual environment at: .venv
Resolved 24 packages in 0.80ms
error: Failed to determine installation plan
  Caused by: Distribution not found at: file:///Users/testuser/.cache/uv/git-v0/checkouts/3a7c78869eb8297a/0e0b932/lib/nsx-policy-python-sdk/nsx_policy_python_sdk-4.2.0-py2.py3-none-any.whl
```

---

_Renamed from "uv.lock has local cache for path which fails when cache is empty" to "uv.lock local cache path which causes sync fail when cache is empty" by @jhurlb on 2024-12-18 23:52_

---

_Comment by @charliermarsh on 2024-12-19 00:07_

The paths you're seeing are indicative of a bug. We should never be writing relative paths to the cache like that. Can you come up with a reproduction that we can use to debug?

---

_Label `bug` added by @charliermarsh on 2024-12-19 00:07_

---

_Label `needs-mre` added by @charliermarsh on 2024-12-19 00:07_

---

_Comment by @jhurlb on 2024-12-19 00:18_

```
uv init
uv add git+https://github.com/vmware/vsphere-automation-sdk-python.git --tag v8.0.3.0
uv cache clean
uv sync
```

---

_Comment by @charliermarsh on 2024-12-19 00:53_

Thanks, I'll take a look.

---

_Comment by @charliermarsh on 2024-12-19 01:52_

Ah yeah, this is known -- there's a TODO for it, but we don't have any way to represent "local wheel file within a Git repository" today. (The problem is the references to the wheels in `lib`.)

---

_Label `needs-mre` removed by @charliermarsh on 2024-12-19 01:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-19 02:06_

---

_Comment by @jhurlb on 2024-12-19 02:21_

Thank you! Is there an alternative installation method to work around this issue that you know of? 

---

_Comment by @charliermarsh on 2024-12-19 03:44_

Unfortunately I think we just need to fix it. A long-shot option is that you could use URL sources for your pre-built wheels instead of path sources... That would probably work, but it's pretty inconvenient.

---

_Comment by @charliermarsh on 2024-12-22 13:44_

I looked into this a bit more and unfortunately I misdiagnosed the issue. There _is_ a real limitation to relying on wheel paths in Git repositories, which I've fixed in #10072. But the repo you linked is doing something very specific that I doubt we'll be able to support, in that the metadata it generates on-the-fly encodes the current working directory: https://github.com/vmware/vsphere-automation-sdk-python/blob/145786b88fec3c0f59335d7dd683c1017a1fe626/setup.py#L20-L25 (e.g., `vmwarecloud-aws @ file://localhost/{}/lib/vmwarecloud-aws/vmwarecloud_aws-1.64.1-py2.py3-none-any.whl'.format(os.getcwd())`).

That will always encode the path to the build directory. In some sense pip suffers from the same problem, if you look at the `direct_url.json` -- it just doesn't appear in pip because they don't write a lockfile:

```
❯ cat bar/lib/python3.12/site-packages/vmwarecloud_aws-1.64.1.dist-info/direct_url.json
{"archive_info": {"hash": "sha256=91a153af25eb52cad950ecddf5b53caaf09777cc0ed4d313c9aa58635551b6e6", "hashes": {"sha256": "91a153af25eb52cad950ecddf5b53caaf09777cc0ed4d313c9aa58635551b6e6"}}, "url": "file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/vmwarecloud-aws/vmwarecloud_aws-1.64.1-py2.py3-none-any.whl"}
```

But it does show up in `pip freeze`:

```
❯ bar/bin/pip freeze
certifi==2024.12.14
cffi==1.17.1
charset-normalizer==3.4.0
cryptography==44.0.0
idna==3.10
lxml==5.3.0
nsx-vmc-aws-integration-python-sdk @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/nsx-vmc-aws-integration-python-sdk/nsx_vmc_aws_integration_python_sdk-4.1.2.0.1-py2.py3-none-any.whl#sha256=94db4bb6720c28a4fdfb7a55197b0dec97cb01a384bddcf51927f9694071d65d
nsx-vmc-policy-python-sdk @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/nsx-vmc-policy-python-sdk/nsx_vmc_policy_python_sdk-4.1.2.0.1-py2.py3-none-any.whl#sha256=a1f5a3153d4eb3282fb6b3b409343ce6e260cf4555c22f1e381a1bc8acfbc4b8
nsx_policy_python_sdk @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/nsx-policy-python-sdk/nsx_policy_python_sdk-4.2.0-py2.py3-none-any.whl#sha256=cb37240933b391d903ef83b5e0658661aba691ff05822f929f399ba7b5d27fe8
nsx_python_sdk @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/nsx-python-sdk/nsx_python_sdk-4.2.0-py2.py3-none-any.whl#sha256=c40cedaaad3637947625e858f81f305d048625cd4459ce970bf3ef69dc17bb5f
pycparser==2.22
pyOpenSSL==24.3.0
pyvmomi==8.0.3.0.1
requests==2.32.3
setuptools==75.6.0
six==1.17.0
urllib3==2.3.0
vmware-vapi-common-client==2.52.0
vmware-vapi-runtime==2.52.0
vmware_vcenter==8.0.3.0
vmwarecloud-aws @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/vmwarecloud-aws/vmwarecloud_aws-1.64.1-py2.py3-none-any.whl#sha256=91a153af25eb52cad950ecddf5b53caaf09777cc0ed4d313c9aa58635551b6e6
vmwarecloud-draas @ file://localhost//private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-uavdf4zk/lib/vmwarecloud-draas/vmwarecloud_draas-1.23.1-py2.py3-none-any.whl#sha256=d979e1a5335d1c5144591c916fc9404781456d669b92e494c31ab0975949f351
vsphere-automation-sdk @ git+https://github.com/vmware/vsphere-automation-sdk-python.git@145786b88fec3c0f59335d7dd683c1017a1fe626
```

So I think this is something that needs to change in the package itself, unfortunately.

---

_Label `bug` removed by @charliermarsh on 2024-12-22 13:44_

---

_Label `wontfix` added by @charliermarsh on 2024-12-22 13:44_

---

_Closed by @charliermarsh on 2024-12-22 13:57_

---
