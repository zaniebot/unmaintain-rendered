```yaml
number: 1807
title: Dependency resolution output
type: issue
state: open
author: heliocastro
labels:
  - enhancement
assignees: []
created_at: 2024-02-21T13:48:07Z
updated_at: 2024-02-22T13:10:44Z
url: https://github.com/astral-sh/uv/issues/1807
synced_at: 2026-01-12T15:58:32Z
```

# Dependency resolution output

---

_@heliocastro_

Hello guys
This is a possible questioning/enhancement issue.

I come from the compliance tooling space, and among our usual scanning tools, we depend for direct python resolution dependency.
Today we do collection in two steps:
1 - Get requirements.txt out of pip/poetry/etc.
2 - Use a metadata extractor like https://github.com/nexB/python-inspector

The resulting metadata file from the transient dependencies is used towards the forward process to scan ( for licenses, vulnerabilites, etc. ).
Here's an example generated from [requirements.txt](https://github.com/astral-sh/uv/files/14360230/requirements.txt)

[inspector.json](https://github.com/astral-sh/uv/files/14360221/inspector.json)

Come to a question that:
- Is this functionality belongs to uv ?
- Could be implemented ?

I'm up to get more details over it if you think is feasible and i'm looking in the wrong place.

Thanks for the great work btw.

---

_Label `enhancement` added by @zanieb on 2024-02-21 17:01_

---

_Comment by @zanieb on 2024-02-21 17:03_

Would this roughly match the `pip install --report` flag?

I think this makes sense for us to do eventually.

Thanks for participating in the project!

---

_Comment by @heliocastro on 2024-02-21 17:33_

Hi back.
Correct, almost like --report, except that python inspector dig in the transient dependencies as well.

---

_Comment by @sbidoul on 2024-02-21 17:47_

> transient dependencies as well

Assuming this is build dependencies, that is something I would like to eventually add to the pip install report.

@charliermarsh @zanieb feel free to involve me in the design if you work on that at some point, or if you have questions about the install report spec.

---

_Comment by @heliocastro on 2024-02-22 13:10_

I'm open to give any info needed and test results.

For reference where we use this information, i'm one dev over [Oss Review Toolkit](https://github.com/oss-review-toolkit/ort) and we use every single package manager possible to do parsing of multiple-languages and extract dependencies, in specific python case what is happening today:
* Extract the requirements over poetry/pip lock files to a requirements file if exists or if allowed dynamic version direct from pyproject
* Use the mentioned tool, python-inspector to extract the full meta data info, and then from there we use it to download and scan the source files to generate Ort  meta model.
* From there we go to the vulnerability analysis to license and copyright extraction and curation, etc., that's why this initial info is needed and valuable.

Again, thanks for the quick answer

---
