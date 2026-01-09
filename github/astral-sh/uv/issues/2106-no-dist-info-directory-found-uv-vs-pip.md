---
number: 2106
title: No .dist-info directory found - uv vs. pip
type: issue
state: closed
author: TheoBabilon
labels:
  - compatibility
assignees: []
created_at: 2024-03-01T10:08:30Z
updated_at: 2024-03-04T09:00:36Z
url: https://github.com/astral-sh/uv/issues/2106
synced_at: 2026-01-07T13:12:17-06:00
---

# No .dist-info directory found - uv vs. pip

---

_Issue opened by @TheoBabilon on 2024-03-01 10:08_

I noticed a difference in behavior between `uv` and `pip` (`uv` fails where `pip` succeeds)
Note that because the underlying reason might be an incorrectly-built wheel, please feel free to close as "won't fixed"; I just thought it would be great to report here in case it is something you want to handle the same way as `pip` does currently.

uv version used is `0.1.13`.

I was given a company-internal package wheel file (I'm not aware of how it was built); let's call it `mypackage-0.10.3-cp311-cp311-linux_x86_64.whl`
when trying to install it using 
`uv pip install "mypackage @ mypackage-0.10.3-cp311-cp311-linux_x86_64.whl"`
I get
```
error: Failed to read: mypackage @ file:///path/to/my/wheel/mypackage-0.10.3-cp311-cp311-linux_x86_64.whl
  Caused by: No .dist-info directory found
```

Note that it's working fine when using pip:
`pip install mypackage-0.10.3-cp311-cp311-linux_x86_64.whl`
package gets installed without errors.

The underlying reason seems to be coming from this strict version check, which also depends on the filename: https://github.com/astral-sh/uv/blob/d0ffabd1f2aaa7788d40f1f3b9d253cc9cacdb2e/crates/install-wheel-rs/src/lib.rs#L166

I came to this conclusion doing the following:
I extracted the wheel (`unzip mypackage-0.10.3-cp311-cp311-linux_x86_64.whl`); omitting non-dist-info files
```
...
  inflating: mypackage-0.10.3+123.ab1cd2e.dist-info/LICENSE
  inflating: mypackage-0.10.3+123.ab1cd2e.dist-info/METADATA
  inflating: mypackage-0.10.3+123.ab1cd2e.dist-info/WHEEL
  inflating: mypackage-0.10.3+123.ab1cd2e.dist-info/top_level.txt
  inflating: mypackage-0.10.3+123.ab1cd2e.dist-info/RECORD
```
I noticed the added `+123.ab1cd2e` - not sure what it is, but this is why I initially said that this might be "an incorrectly-built wheel", so I simply changed the wheel filename 

From `mypackage-0.10.3-cp311-cp311-linux_x86_64.whl` to `mypackage-0.10.3+123.ab1cd2e-cp311-cp311-linux_x86_64.whl` 

and it fixes the issue (the above version check passes, so I don't get any error), the package gets installed without any problem using `uv pip install "mypackage @ mypackage-0.10.3+123.ab1cd2e-cp311-cp311-linux_x86_64.whl"`





---

_Label `compatibility` added by @konstin on 2024-03-01 10:30_

---

_Comment by @konstin on 2024-03-01 10:35_

This is a bug in the tool used to create `mypackage`, according to [PEP 491](https://peps.python.org/pep-0491/#the-dist-info-directory) the wheel filename is `{distribution}-{version}(-{build tag})?-{python tag}-{abi tag}-{platform tag}.whl` while the dist info directory is `{distribution}-{version}.dist-info/`. I suspect this accidentally passes with pip because versions the only differ in the local segments (the part after the `+`) are considered equal in most circumstances.

---

_Comment by @charliermarsh on 2024-03-02 03:56_

@konstin - What do you think is the right course of action here?

---

_Comment by @konstin on 2024-03-03 17:44_

I think this needs to be fixed on this side of the package authors; I wouldn't add a fixup unless it affects a bigger number of packages

---

_Comment by @TheoBabilon on 2024-03-03 17:53_

> I think this needs to be fixed on this side of the package authors; I wouldn't add a fixup unless it affects a bigger number of packages

As mentioned in my original post, I knew already that it was potentially an incorrectly-built package; I was merely reporting the issue as a "heads up" considering it was working fine using pip, and that you might (or might not) have the same exact thing reported in the near of far future; but if it's indeed just "incorrectly" working "by chance" in pip, then better to just close the issue! I'm just surprised (in principle) that a simple filename for a wheel is that important for correct or failing installation; but that makes sense considering the PEP you linked 🙂

Maybe an alternative error message might save some (investigation) time to the next person facing the exact same issue when the solution is simply related to the filename; but that might not be that easy to do in practice.

---

_Comment by @charliermarsh on 2024-03-03 17:55_

(To be clear: thanks @TheoBabilon, I appreciate you filing the issue regardless of whether we add handling for it or not!)

---

_Comment by @TheoBabilon on 2024-03-04 09:00_

Thanks guys, closing the issue as it served its "heads up" purpose, thanks for the explanations!

---

_Closed by @TheoBabilon on 2024-03-04 09:00_

---
