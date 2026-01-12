```yaml
number: 10251
title: Failed to download and build package from S3
type: issue
state: closed
author: duducp
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-12-31T12:33:38Z
updated_at: 2024-12-31T17:59:27Z
url: https://github.com/astral-sh/uv/issues/10251
synced_at: 2026-01-12T16:00:09Z
```

# Failed to download and build package from S3

---

_@duducp_

I have some packages that are hosted on S3 but when I try to add them to the project I get an authentication error. This only occurs on UV, via Poetry and PIP itself works normally. This seems to be a bug since UV is removing the query param `AWSAccessKeyId` from the URL when trying to download the package.

- Command: `uv add my-package --index mb=https://pypi.dev.my-domain.com.br/simple`

<img width="1327" alt="Image" src="https://github.com/user-attachments/assets/5a0c77ea-903c-498c-a5c1-31a3b8caa710" />


- Details: 
```bash
>>> uv version    
uv 0.5.13 (c456bae5e 2024-12-27)

>>> python --version
Python 3.12.8

>>> sw_vers
ProductName:            macOS
ProductVersion:         15.1.1
BuildVersion:           24B91
```



---

_Label `bug` added by @charliermarsh on 2024-12-31 14:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-31 14:29_

---

_Comment by @charliermarsh on 2024-12-31 14:33_

Are you able to test this branch? https://github.com/astral-sh/uv/pull/10253

I can't easily reproduce this since it's specific to your repository.

---

_Comment by @carlosdorneles-mb on 2024-12-31 16:21_

> Are you able to test this branch? [#10253](https://github.com/astral-sh/uv/pull/10253)
> 
> I can't easily reproduce this since it's specific to your repository.

I ran it here, but it still gives me the same error.

<img width="1329" alt="Image" src="https://github.com/user-attachments/assets/044dc9e2-196e-4550-99dc-cae768061f2b" />


---

_Comment by @charliermarsh on 2024-12-31 17:08_

Did you remove your lockfile before running that command?

Is there any way I can test this myself? I don't really know where else we would be stripping the credentials.

As far as I can tell from testing, that branch is preserving the query parameter everywhere:

```
cargo run add "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl?foo=1"
```

---

_Label `needs-mre` added by @charliermarsh on 2024-12-31 17:08_

---

_Comment by @charliermarsh on 2024-12-31 17:23_

I think I see another possible spot in the code where this could be happening.

---

_Comment by @carlosdorneles-mb on 2024-12-31 17:35_

> Did you remove your lockfile before running that command?
> 
> Is there any way I can test this myself? I don't really know where else we would be stripping the credentials.
> 
> As far as I can tell from testing, that branch is preserving the query parameter everywhere:
> 
> ```
> cargo run add "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl?foo=1"
> ```

I have UV installed globally. To test the specific branch, I installed Cargo and ran the command `pip install -U git+https://github.com/astral-sh/uv@charlie/q` in my virtualenv. 

As shown in the attached image, when I ran the add command it was in the informed branch version. When running the command, it doesn't even write to lock or pyproject because the error occurs when downloading the package from S3.



---

_Comment by @charliermarsh on 2024-12-31 17:35_

Do you mind trying with the latest updates to that branch?

---

_Comment by @charliermarsh on 2024-12-31 17:36_

(Honestly, it's arguably incorrect to write these credentials to the lockfile in plaintext, but I don't see other alternatives for you if you plan to use query parameters.)

---

_Comment by @carlosdorneles-mb on 2024-12-31 17:58_

> Do you mind trying with the latest updates to that branch?

Now yes, it worked!!!

<img width="754" alt="Image" src="https://github.com/user-attachments/assets/3209f9af-ae23-4db3-adb5-7415d270988b" />

The pyproject and uv.lock files have been updated correctly.


---

_Comment by @charliermarsh on 2024-12-31 17:59_

Awesome, thanks for testing! Also worked for me locally.

---

_Closed by @charliermarsh on 2024-12-31 17:59_

---

_Closed by @charliermarsh on 2024-12-31 17:59_

---
