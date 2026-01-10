---
number: 16425
title: allow uv publish to upload doc zips
type: issue
state: closed
author: Ktlist
labels:
  - question
assignees: []
created_at: 2025-10-23T16:40:28Z
updated_at: 2025-10-24T10:11:21Z
url: https://github.com/astral-sh/uv/issues/16425
synced_at: 2026-01-10T01:26:06Z
---

# allow uv publish to upload doc zips

---

_Issue opened by @Ktlist on 2025-10-23 16:40_

### Question

I odnt know if this is explained somewhere but i have not found it

Currently im using a private pypi regidtry to store my own packages, its run with a devpi-server
I want to also upload the documentation that for devpi-server is just a zip with the html and files needed

the issue is that if i try to do a uv publish --index private_index (the index is defined in pyproject.toml)
it uploads the sdist and wheel but for the docs zip issues a warning that it has been skipped since looks like a distribution but it isn´t here is the log:

uv publish --index packages_staging
warning: Skipping file that looks like a distribution, but is not a valid distribution filename: dist\packagetemplate-0.1.1a1.doc.zip
Publishing 2 files to https://some-server/packages/staging/
File packagetemplate-0.1.4a10-py3-none-any.whl already exists, skipping
File packagetemplate-0.1.4a10.tar.gz already exists, skipping

the wheel and sdist were previously uploaded but thats normal to be skipped

Thanks in advance

### Platform

Windows 11

### Version

uv 0.9.2 (141369ce7 2025-10-10)

---

_Label `question` added by @Ktlist on 2025-10-23 16:40_

---

_Comment by @Ktlist on 2025-10-23 21:26_

I have been investigating the code a bit but i have not touched any of rust before so i might get things wrong:

first seems that files ended in xxxx.doc.zip are filtered out 
at this point since they are not a wheel nor a tar.gz dist file:
https://github.com/astral-sh/uv/blob/17181fef07326707ca6921aba28d1dbdd1e6ce83/crates/uv-publish/src/lib.rs#L270-L284

So I modified the file name to en in only zip as the code seem sto accept that kind of files, so the new file is called 
packagetemplate-0.1.1a1.zip
and this works perfectly... until we reach the point of actually uploading the file:
https://github.com/astral-sh/uv/blob/17181fef07326707ca6921aba28d1dbdd1e6ce83/crates/uv-publish/src/lib.rs#L679-L685

at this point is clear that there was never the intention of uploading any file ended in .zip

I dont know what are the limitations and if its possible to create some kind of bypass of these rules, also if someone comes up with a workarround after seeing the code ill be glad to hear

---

_Comment by @konstin on 2025-10-24 09:00_

`uv publish` is only for uploading the two standardized Python package formats, wheels and source distributions. It's not the right tool for uploading docs zip files to a server. I'm surprised devpi accepts this file, most package registries reject files that are not wheels or source distributions.

---

_Comment by @Ktlist on 2025-10-24 10:03_

thnaks @konstin for the response

i have a doubt left, how is usually people managing the documentation of the packages?
using completely dedicated services only for the documentation with its own calls and authentications?
I have always worked with devpi and used devpi-client to manage uploads, but seeing how powerfull uv is i wanted to fully use it
i would rather not need to use both tools at the same time since i would need to parse credentials to both and maintain 2 pipelines 


---

_Comment by @konstin on 2025-10-24 10:04_

> i have a doubt left, how is usually people managing the documentation of the packages?
> using completely dedicated services only for the documentation with its own calls and authentications?

Yes, this is generally not handled through package registries, I'm not aware of any other projects that use package registries for documentation.

---

_Comment by @Ktlist on 2025-10-24 10:11_

ok thanks for the info

---

_Closed by @Ktlist on 2025-10-24 10:11_

---
