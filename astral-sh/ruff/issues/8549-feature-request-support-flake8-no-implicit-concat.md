```yaml
number: 8549
title: "[Feature request]: support `flake8-no-implicit-concat` (NIC)"
type: issue
state: closed
author: wtfzambo
labels:
  - question
assignees: []
created_at: 2023-11-07T21:43:55Z
updated_at: 2023-11-07T23:08:14Z
url: https://github.com/astral-sh/ruff/issues/8549
synced_at: 2026-01-12T15:54:48Z
```

# [Feature request]: support `flake8-no-implicit-concat` (NIC)

---

_@wtfzambo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

This is a feature request to introduce the aforementioned [flake8 plugin](https://pypi.org/project/flake8-no-implicit-concat/) as an optional rule in Ruff (mutually exclusive with ISC).

The reasoning behind this is that in certain occasions, separate strings must not be concatenated, but separated by a comma, which may be missing by accident; and in a sufficiently long module, is not easily spotted.

One practical (and personal) example is when writing IAM policies via SDKs such as Pulumi. Each "permission" is likely written on a separate line. Without a comma separator, 2 lines would be implicitly concatenated, resulting in a wrong (but programmatically valid) policy, which obviously breaks something downstream, leaving the developer wondering which permission they missed, and instead it was just a comma.

Also, 

```
Explicit is better than implicit.
```

<details>

<summary>Go ahead, try to spot the missing comma quickly</summary>

```python

def build(
    *, oidc_arn: Output[str], bucket: s3.Bucket, workgroup: athena.Workgroup
) -> None:
    bucket_arn: Output[str] = bucket.arn

    policy_document = iam.get_policy_document(
        statements=[
            Statement(
                sid="GlueAccessForDltPipelines",
                effect="Allow",
                actions=[
                    "glue:CreateDatabase",
                    "glue:DeleteDatabase",
                    "glue:GetDatabase",
                    "glue:GetDatabases",
                    "glue:UpdateDatabase",
                    "glue:CreateTable",
                    "glue:DeleteTable",
                    "glue:BatchDeleteTable",
                    "glue:UpdateTable",
                    "glue:GetTable",
                    "glue:GetTables",
                    "glue:BatchCreatePartition",
                    "glue:CreatePartition"
                    "glue:DeletePartition",
                    "glue:BatchDeletePartition",
                    "glue:UpdatePartition",
                    "glue:GetPartition",
                    "glue:GetPartitions",
                    "glue:BatchGetPartition",
                ],
                resources=[
                    f"arn:aws:glue:{AWS_REGION}:{AWS_ACCOUNT_ID}:catalog",
                    f"arn:aws:glue:{AWS_REGION}:{AWS_ACCOUNT_ID}:database/{TA_RAW}_*_{CI_ENVIRONMENT}",
                    f"arn:aws:glue:{AWS_REGION}:{AWS_ACCOUNT_ID}:database/{VH_RAW}_*_{CI_ENVIRONMENT}",
                    f"arn:aws:glue:{AWS_REGION}:{AWS_ACCOUNT_ID}:table/{TA_RAW}_*_{CI_ENVIRONMENT}/*",
                    f"arn:aws:glue:{AWS_REGION}:{AWS_ACCOUNT_ID}:table/{VH_RAW}_*_{CI_ENVIRONMENT}/*",
                ],
            ),
            Statement(
                sid="S3AccessForDataExportAndQueries",
                effect="Allow",
                actions=[
                    "s3:ListBucket",
                    "s3:ListBucketMultipartUploads",
                    "s3:ListMultipartUploadParts",
                    "s3:PutObject",
                    "s3:GetObject",
                    "s3:GetObjectAttributes",
                    "s3:DeleteObject",
                    "s3:GetBucketLocation",
                    "s3:AbortMultipartUpload",
                ],
                resources=[
                    f"arn:aws:s3:::{ATHENA_QUERY_RESULT_BUCKET}",
                    f"arn:aws:s3:::{ATHENA_QUERY_RESULT_BUCKET}/*",
                    bucket_arn,  # type: ignore
                    bucket_arn.apply(lambda arn: f"{arn}/*"),  # type: ignore
                ],
            ),
            Statement(
                sid="AthenaResourcesAccessForDltPipelines",
                effect="Allow",
                actions=[
                    "athena:ListQueryExecutions",
                    "athena:ListDatabases",
                    "athena:ListTableMetadata",
                    "athena:GetTableMetadata",
                    "athena:GetQueryResultsStream",
                    "athena:GetQueryResults",
                    "athena:GetDatabase",
                    "athena:GetDataCatalog",
                    "athena:GetWorkGroup",
                    "athena:BatchGetQueryExecution",
                    "athena:GetQueryExecution",
                    "athena:StartQueryExecution",
                    "athena:StopQueryExecution",
                ],
                resources=[
                    workgroup.arn,
                    f"arn:aws:athena:{AWS_REGION}:{AWS_ACCOUNT_ID}:datacatalog/awsdatacatalog",
                ],
            ),
        ]
    )
```
</details>

---

_Comment by @charliermarsh on 2023-11-07 22:41_

I think we may actually support what you're describing... Check out the playground here: https://play.ruff.rs/32b744cd-c7ba-4e3b-b876-284ad6473fa4. Is that right? Disallow all implicit concatenations?

(Locally, you'd need to set `allow-multiline` to `false` (https://docs.astral.sh/ruff/settings/#flake8-implicit-str-concat-allow-multiline.)

---

_Label `question` added by @charliermarsh on 2023-11-07 22:41_

---

_Comment by @wtfzambo on 2023-11-07 23:08_

@charliermarsh oh, it is! YES! Fantastic! Thanks, I searched for this in the docs but somehow had missed this setting. Neat!

---

_Closed by @wtfzambo on 2023-11-07 23:08_

---
