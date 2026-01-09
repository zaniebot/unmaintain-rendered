---
number: 12257
title: "Publish official `UvPythonFunction` CDK construct"
type: issue
state: open
author: garysassano
labels:
  - enhancement
assignees: []
created_at: 2025-03-18T02:03:46Z
updated_at: 2025-03-18T02:03:46Z
url: https://github.com/astral-sh/uv/issues/12257
synced_at: 2026-01-07T13:12:18-06:00
---

# Publish official `UvPythonFunction` CDK construct

---

_Issue opened by @garysassano on 2025-03-18 02:03_

### Summary

I've come across the new [AWS Lambda docs](https://docs.astral.sh/uv/guides/integration/aws-lambda/), introduced by a recent PR (#10278).  

To avoid repetition, here are some discussions related to this issue:  

- https://github.com/astral-sh/uv/issues/9350
- https://github.com/aws/aws-cdk/issues/31238
- https://github.com/aws/aws-cdk/issues/32413

I believe it could be beneficial for users if the uv team provided and maintained this CDK construct, as they have the deepest understanding of the tool's intricacies and are best positioned to optimize the final output, as demonstrated in the AWS Lambda docs linked above.






### Example

### Unofficial CDK Construct for uv:
- [uv-python-lambda](https://github.com/fourTheorem/uv-python-lambda)

### Official CDK Constructs by 3rd-party (not included in `aws-cdk-lib`):
- [RustFunction](https://github.com/cargo-lambda/cargo-lambda-cdk)
- [PhpFpmFunction](https://github.com/brefphp/constructs)


---

_Label `enhancement` added by @garysassano on 2025-03-18 02:03_

---
