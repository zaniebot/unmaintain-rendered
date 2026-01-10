---
number: 15360
title: "`uv lock --upgrade-package` results into all package upgrades"
type: issue
state: closed
author: vivekfe
labels:
  - bug
assignees: []
created_at: 2025-08-18T20:34:02Z
updated_at: 2025-08-19T21:27:10Z
url: https://github.com/astral-sh/uv/issues/15360
synced_at: 2026-01-10T01:25:55Z
---

# `uv lock --upgrade-package` results into all package upgrades

---

_Issue opened by @vivekfe on 2025-08-18 20:34_

### Summary

Hi, I am trying to upgrade two packages from internal company index. I run the command
`uv lock --upgrade-package <package-A>` however this does not change anything in UV.lock even if we have a new version of the package available in nexus.

However if I use upgrade=true in UV settings within `pyproject.toml,` then specific package upgrades works fine however that results into whole environment upgrade. So in a way `uv lock ---upgrade-package <package-A>` acts same as `uv lock --upgrade`.

Seems to be a bug.

### Platform

Windows 11

### Version

UV 0.8.9

### Python version

Python 3.10.10

---

_Label `bug` added by @vivekfe on 2025-08-18 20:34_

---

_Comment by @zanieb on 2025-08-18 20:39_

Sorry, I don't understand. Why are you setting `upgrade = true` in the `pyproject.toml`? It's expected that all packages would upgrade in that case.

Are you saying that `--upgrade-package <name>` in isolation causes all packages to upgrade? That should not be true and we have pretty clear test coverage for it https://github.com/astral-sh/uv/blob/323aa8f3324a2faaf286ad3a6bb84113c2d69e52/crates/uv/tests/it/lock.rs#L10575-L10584

---

_Comment by @zanieb on 2025-08-18 20:40_

> uv lock --upgrade-package <package-A> however this does not change anything in UV.lock even if we have a new version of the package available in nexus.

This should also not be true. Have you tried `--upgrade-package <package-A> --refresh` too? Are you sure the registry is returning the new versions?

---

_Comment by @vivekfe on 2025-08-19 05:16_

*Case 1-*

[tool.uv]
constraint-dependencies = ["numpy==1.26"]
python-preference = "only-system"
reinstall = false
package = true
upgrade = false

C:\AnalyticsWorkSpace\Global-Solutions> uv lock --upgrade-package hsbc_anl

Resolved 354 packages in 44ms
(no changes in uv.lock)

hsbc-anl v4.0.264 (stays same, even though v4.0.265 available on nexus)
hsbc-anl-infra v4.0.365 (stays same, even though v4.0.366 available on
nexus)


*Case 2- *

[tool.uv]
constraint-dependencies = ["numpy==1.26"]
python-preference = "only-system"
reinstall = false
package = true
upgrade-package = ["hsbc_anl", "hsbc_anl_infra"]
upgrade = false

C:\AnalyticsWorkSpace\Global-Solutions> uv lock --upgrade-package hsbc_anl

Resolved 354 packages in 12.36s
Updated hsbc-anl v4.0.264 -> v4.0.265
Updated hsbc-anl-infra v4.0.365 -> v4.0.366

So in case 1 if I don't define the lo
Iist if packages in upgrade-package argument of pyproject.toml, in this
case `uv lock ---upgrade-package hsbc_anl` works fine but it actually does
not change the versions of packages. There is no change in `uv.lock`

In case 2, if I provide an explicit list in pyproject.toml, then it works.
But, then it upgraded many other packages which I did not ask. I checked
the dependencies of the package hsbc_anl but the other upgraded packages
were not in dependencies.



On Mon, 18 Aug 2025, 10:41 pm Zanie Blue, ***@***.***> wrote:

> *zanieb* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3198352984>
>
> uv lock --upgrade-package however this does not change anything in UV.lock
> even if we have a new version of the package available in nexus.
>
> This should also not be true. Have you tried --upgrade-package
> <package-A> --refresh too? Are you sure the registry is returning the new
> versions?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3198352984>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLIE4BZC3PJ3SVL5YES33OI26JAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTCOJYGM2TEOJYGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @vivekfe on 2025-08-19 05:19_

Hi @zanieb thanks for your response. I have provided two cases in above
comment which should clarify the case which I mentioned.

When I run `uv lock --upgrade`, then it upgraded the whole environment as
intended.



On Tue, 19 Aug 2025, 7:15 am Vivek Srivastava, ***@***.***> wrote:

> *Case 1-*
>
> [tool.uv]
> constraint-dependencies = ["numpy==1.26"]
> python-preference = "only-system"
> reinstall = false
> package = true
> upgrade = false
>
> C:\AnalyticsWorkSpace\Global-Solutions> uv lock --upgrade-package hsbc_anl
>
> Resolved 354 packages in 44ms
> (no changes in uv.lock)
>
> hsbc-anl v4.0.264 (stays same, even though v4.0.265 available on nexus)
> hsbc-anl-infra v4.0.365 (stays same, even though v4.0.366 available on
> nexus)
>
>
> *Case 2- *
>
> [tool.uv]
> constraint-dependencies = ["numpy==1.26"]
> python-preference = "only-system"
> reinstall = false
> package = true
> upgrade-package = ["hsbc_anl", "hsbc_anl_infra"]
> upgrade = false
>
> C:\AnalyticsWorkSpace\Global-Solutions> uv lock --upgrade-package hsbc_anl
>
> Resolved 354 packages in 12.36s
> Updated hsbc-anl v4.0.264 -> v4.0.265
> Updated hsbc-anl-infra v4.0.365 -> v4.0.366
>
> So in case 1 if I don't define the lo
> Iist if packages in upgrade-package argument of pyproject.toml, in this
> case `uv lock ---upgrade-package hsbc_anl` works fine but it actually does
> not change the versions of packages. There is no change in `uv.lock`
>
> In case 2, if I provide an explicit list in pyproject.toml, then it works.
> But, then it upgraded many other packages which I did not ask. I checked
> the dependencies of the package hsbc_anl but the other upgraded packages
> were not in dependencies.
>
>
>
> On Mon, 18 Aug 2025, 10:41 pm Zanie Blue, ***@***.***>
> wrote:
>
>> *zanieb* left a comment (astral-sh/uv#15360)
>> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3198352984>
>>
>> uv lock --upgrade-package however this does not change anything in
>> UV.lock even if we have a new version of the package available in nexus.
>>
>> This should also not be true. Have you tried --upgrade-package
>> <package-A> --refresh too? Are you sure the registry is returning the
>> new versions?
>>
>> —
>> Reply to this email directly, view it on GitHub
>> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3198352984>,
>> or unsubscribe
>> <https://github.com/notifications/unsubscribe-auth/AADKLIE4BZC3PJ3SVL5YES33OI26JAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTCOJYGM2TEOJYGQ>
>> .
>> You are receiving this because you authored the thread.Message ID:
>> ***@***.***>
>>
>


---

_Comment by @charliermarsh on 2025-08-19 09:18_

Okay, I think the bug here is: if you have `upgrade = false` set in your configuration file, then `--upgrade-package` on the command-line seems to be a no-op.

---

_Comment by @charliermarsh on 2025-08-19 09:19_

Given this `uv.lock`:

```toml
version = 1
revision = 2
requires-python = ">=3.11"

[[package]]
name = "foo"
version = "0.1.0"
source = { editable = "." }
dependencies = [
    { name = "numpy" },
]

[package.metadata]
requires-dist = [{ name = "numpy" }]

[[package]]
name = "numpy"
version = "1.26.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/55/b3/b13bce39ba82b7398c06d10446f5ffd5c07db39b09bd37370dc720c7951c/numpy-1.26.0.tar.gz", hash = "sha256:f93fc78fe8bf15afe2b8d6b6499f1c73953169fad1e9a8dd086cdff3190e7fdf", size = 15633455, upload-time = "2023-09-16T20:12:58.065Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/d2/2f/b42860931c1479714201495ffe47d74460a916ae426a21fc9b68c5e329aa/numpy-1.26.0-cp311-cp311-macosx_10_9_x86_64.whl", hash = "sha256:637c58b468a69869258b8ae26f4a4c6ff8abffd4a8334c830ffb63e0feefe99a", size = 20619338, upload-time = "2023-09-16T20:01:30.608Z" },
    { url = "https://files.pythonhosted.org/packages/35/21/9e150d654da358beb29fe216f339dc17f2b2ac13fff2a89669401a910550/numpy-1.26.0-cp311-cp311-macosx_11_0_arm64.whl", hash = "sha256:306545e234503a24fe9ae95ebf84d25cba1fdc27db971aa2d9f1ab6bba19a9dd", size = 13981953, upload-time = "2023-09-16T20:01:54.921Z" },
    { url = "https://files.pythonhosted.org/packages/a9/84/baf694be765d68c73f0f8a9d52151c339aed5f2d64205824a6f29021170c/numpy-1.26.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:8c6adc33561bd1d46f81131d5352348350fc23df4d742bb246cdfca606ea1208", size = 14167328, upload-time = "2023-09-16T20:02:20.922Z" },
    { url = "https://files.pythonhosted.org/packages/c4/36/161e2f8110f8c49e59f6107bd6da4257d30aff9f06373d0471811f73dcc5/numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:e062aa24638bb5018b7841977c360d2f5917268d125c833a686b7cbabbec496c", size = 18178118, upload-time = "2023-09-16T20:02:49.046Z" },
    { url = "https://files.pythonhosted.org/packages/37/41/63975634a93da2a384d3c8084eba467242cab68daab0cd8f4fd470dcee26/numpy-1.26.0-cp311-cp311-musllinux_1_1_x86_64.whl", hash = "sha256:546b7dd7e22f3c6861463bebb000646fa730e55df5ee4a0224408b5694cc6148", size = 18020808, upload-time = "2023-09-16T20:03:16.849Z" },
    { url = "https://files.pythonhosted.org/packages/58/d2/cbc329aa908cb963bd849f14e24f59c002a488e9055fab2c68887a6b5f1c/numpy-1.26.0-cp311-cp311-win32.whl", hash = "sha256:c0b45c8b65b79337dee5134d038346d30e109e9e2e9d43464a2970e5c0e93229", size = 20750149, upload-time = "2023-09-16T20:03:49.609Z" },
    { url = "https://files.pythonhosted.org/packages/93/fd/3f826c6d15d3bdcf65b8031e4835c52b7d9c45add25efa2314b53850e1a2/numpy-1.26.0-cp311-cp311-win_amd64.whl", hash = "sha256:eae430ecf5794cb7ae7fa3808740b015aa80747e5266153128ef055975a72b99", size = 15794407, upload-time = "2023-09-16T20:04:13.829Z" },
    { url = "https://files.pythonhosted.org/packages/e9/83/f8a62f08d38d831a2980427ffc465a4207fe600124b00cfb0ef8265594a7/numpy-1.26.0-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:166b36197e9debc4e384e9c652ba60c0bacc216d0fc89e78f973a9760b503388", size = 20325091, upload-time = "2023-09-16T20:04:44.267Z" },
    { url = "https://files.pythonhosted.org/packages/7a/72/6d1cbdf0d770016bc9485f9ef02e73d5cb4cf3c726f8e120b860a403d307/numpy-1.26.0-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:f042f66d0b4ae6d48e70e28d487376204d3cbf43b84c03bac57e28dac6151581", size = 13672867, upload-time = "2023-09-16T20:05:05.591Z" },
    { url = "https://files.pythonhosted.org/packages/2f/70/c071b2347e339f572f5aa61f649b70167e5dd218e3da3dc600c9b08154b9/numpy-1.26.0-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:e5e18e5b14a7560d8acf1c596688f4dfd19b4f2945b245a71e5af4ddb7422feb", size = 13872627, upload-time = "2023-09-16T20:05:28.488Z" },
    { url = "https://files.pythonhosted.org/packages/e3/e2/4ecfbc4a2e3f9d227b008c92a5d1f0370190a639b24fec3b226841eaaf19/numpy-1.26.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:7f6bad22a791226d0a5c7c27a80a20e11cfe09ad5ef9084d4d3fc4a299cca505", size = 17883864, upload-time = "2023-09-16T20:05:55.622Z" },
    { url = "https://files.pythonhosted.org/packages/45/08/025bb65dbe19749f1a67a80655670941982e5d0144a4e588ebbdbcfe7983/numpy-1.26.0-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:4acc65dd65da28060e206c8f27a573455ed724e6179941edb19f97e58161bb69", size = 17721550, upload-time = "2023-09-16T20:06:23.505Z" },
    { url = "https://files.pythonhosted.org/packages/98/66/f0a846751044d0b6db5156fb6304d0336861ed055c21053a0f447103939c/numpy-1.26.0-cp312-cp312-win32.whl", hash = "sha256:bb0d9a1aaf5f1cb7967320e80690a1d7ff69f1d47ebc5a9bea013e3a21faec95", size = 19951520, upload-time = "2023-09-16T20:06:53.976Z" },
    { url = "https://files.pythonhosted.org/packages/98/d7/1cc7a11118408ad21a5379ff2a4e0b0e27504c68ef6e808ebaa90ee95902/numpy-1.26.0-cp312-cp312-win_amd64.whl", hash = "sha256:ee84ca3c58fe48b8ddafdeb1db87388dce2c3c3f701bf447b05e4cfcc3679112", size = 15504471, upload-time = "2023-09-16T20:07:22.222Z" },
]
```

And this `pyproject.toml`:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = ["numpy"]

[tool.uv]
python-preference = "only-system"
reinstall = false
package = true
upgrade = false
```

Then `uv lock --upgrade-package numpy` has no effect (but removing `upgrade = false` fixes that).

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-19 09:22_

---

_Comment by @vivekfe on 2025-08-19 09:41_

Thanks Charlie,

I tried to isolate this behaviour

*Package Information Check(uvpy)*


 [Cmd]# uv pip show hsbc_anl
Using Python 3.10.10 at: /var/opt/uvpy/.venv
Name: targetPackage
Version: 4.0.263
Location: /var/opt/uvpy/.venv/lib/python3.10/site-packages
Requires: colour, dash, dash-ag-grid, dash-bootstrap-components, dash-dag,
dash-design-kit, dash-loading-spinners, pandas, plotly, pyclustering,
python-constraint, python-dateutil, scikit-learn, scipy, setuptools
Required-by:Package Lock and Upgrade

*Running Refresh or force reinstall *

 [cmd]# uv lock --upgrade-package targetpackage --refresh
Resolved 356 packages in 2ms

*Final Package Information Check (After Upgrade*)

[Cmd]# uv pip show hsbc_anl
Using Python 3.10.10 at: /var/opt/uvpy/.venv
Name: targetPackage
Version: 4.0.263
Location: /var/opt/uvpy/.venv/lib/python3.10/site-packages
Requires: colour, dash, dash-ag-grid, dash-bootstrap-components, dash-dag,
dash-design-kit, dash-loading-spinners, pandas, plotly, pyclustering,
python-constraint, python-dateutil, scikit-learn, scipy, setuptools
Required-by:

Summary: The version remained the same (4.0.263) both before and after
running the upgrade command.

- So if we put upgrade=false in `pyproject.toml`, nothing changes.

- if we put upgrade=true, then multiple other packages change which I
didn't want.

- if we put upgrade-package list then update happens as well, however many
other packages upgrade as well which were not really the actually
dependencies of myPackage

Note: I have changed the package name as it was an enterprise package


All this time newer versions of the package were available on Nexus.

On Tue, 19 Aug 2025, 11:19 am Charlie Marsh, ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3199939744>
>
> Okay, I think the bug here is: if you have upgrade = false set in your
> configuration file, then --upgrade-package on the command-line seems to
> be a no-op.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3199939744>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLIEELSWOTFQN22LETL33OLTZPAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTCOJZHEZTSNZUGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @charliermarsh on 2025-08-19 09:42_

I suggest just removing `upgrade = false`. It's the default behavior, but adding it explicitly is causing this bug.

---

_Comment by @vivekfe on 2025-08-19 09:47_

Thanks Charlie

If I remove upgrade=false, then it is working like uv lock ---upgrade.

Can we have the behaviour to update only the package and it's dependencies
when we run uv lock ---upgrade-package ?

Also could you advise on issue#15108 if possible ?

On Tue, 19 Aug 2025, 11:43 am Charlie Marsh, ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3200018087>
>
> I suggest just removing upgrade = false. It's the default behavior, but
> adding it explicitly is causing this bug.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3200018087>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLIAPPCX6IQWTOQTSFWL3OLWSTAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTEMBQGAYTQMBYG4>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @charliermarsh on 2025-08-19 09:49_

If you remove `upgrade = false`, I'm fairly sure that `uv lock --upgrade-package numpy` or whatever will do the right thing: upgrade only `numpy`, or any requisite dependencies that are forced to upgrade to support it.

---

_Comment by @vivekfe on 2025-08-19 14:35_

Yes that worked perfectly after removing upgrade=false, however I thought
false will be applied by default.

On Tue, 19 Aug 2025, 11:49 am Charlie Marsh, ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3200038631>
>
> If you remove upgrade = false, I'm fairly sure that uv lock
> --upgrade-package numpy or whatever will do the right thing: upgrade only
> numpy, or any requisite dependencies that are forced to upgrade to
> support it.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3200038631>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLIDFJD3BJF42VODC2ED3OLXLLAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTEMBQGAZTQNRTGE>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Renamed from "UV lock ---upgrade-package results into all package upgrades" to "`uv lock --upgrade-package` results into all package upgrades" by @charliermarsh on 2025-08-19 15:38_

---

_Comment by @charliermarsh on 2025-08-19 15:41_

> false will be applied by default.

Yes that's true, so you can just omit it. But in the code, we treat setting `upgrade = false` slightly differently than if it were omitted, just in terms of how the logic flows. It ends up overriding the `--upgrade-package` CLI flag, which is a bug.

---

_Comment by @vivekfe on 2025-08-19 20:29_

Thanks Charlie

So what's the intended behaviour of upgrade=false ?

On Tue, 19 Aug 2025, 5:42 pm Charlie Marsh, ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3201273814>
>
> false will be applied by default.
>
> Yes that's true, so you can just omit it. But in the code, we treat
> setting upgrade = false slightly differently than if it were omitted,
> just in terms of how the logic flows. It ends up overriding the
> --upgrade-package CLI flag, which is a bug.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3201273814>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLICOCYG7KALMM3HIQXT3ONAVPAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTEMBRGI3TGOBRGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @charliermarsh on 2025-08-19 21:05_

There isn't really a reason to ever put that in a configuration file. The flag exists because if you had `upgrade = true` in your configuration file, then passing `--no-upgrade` on the CLI would make sense.

---

_Comment by @vivekfe on 2025-08-19 21:15_

Ok that makes sense. Thanks Charlie.

This issue is good to be closed.

Many thanks for your help.

On Tue, 19 Aug 2025, 11:05 pm Charlie Marsh, ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15360)
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3202220674>
>
> There isn't really a reason to ever put that in a configuration file. The
> flag exists because if you had upgrade = true in your configuration file,
> then passing --no-upgrade on the CLI would make sense.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15360#issuecomment-3202220674>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADKLICU3KTDHUU64A4WD233OOGSPAVCNFSM6AAAAACEGGMNUKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTEMBSGIZDANRXGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Closed by @zanieb on 2025-08-19 21:27_

---
