---
number: 8655
title: Please add rdjson output support
type: issue
state: closed
author: keramblock
labels:
  - configuration
  - help wanted
assignees: []
created_at: 2023-11-13T16:04:56Z
updated_at: 2024-06-04T10:01:59Z
url: https://github.com/astral-sh/ruff/issues/8655
synced_at: 2026-01-10T01:22:48Z
---

# Please add rdjson output support

---

_Issue opened by @keramblock on 2023-11-13 16:04_

Hi, it would be nice, if ruff could support rdjson: https://github.com/reviewdog/reviewdog/tree/master/proto/rdf#rdjson both for linter and format mode(if possible)

Why? because it allows to provide problem description to PR and suggest fix at the same time using reviewdog: https://github.com/reviewdog/reviewdog

Here is an example of results of such pipeline(rdjson output by eslint):
<img width="648" alt="eslint rdjson parsed" src="https://github.com/astral-sh/ruff/assets/3233532/4cb7497d-096e-4811-bf41-650a1c1a2197">

Useful links by @eremeevfd: 
jsonschema: https://github.com/reviewdog/reviewdog/blob/master/proto/rdf/jsonschema/DiagnosticResult.jsonschema
actual jq parser for Hado-Lint: https://github.com/reviewdog/action-hadolint/blob/master/to-rdjson.jq




---

_Label `configuration` added by @charliermarsh on 2023-11-13 18:10_

---

_Label `help wanted` added by @charliermarsh on 2023-11-13 18:10_

---

_Comment by @charliermarsh on 2023-11-13 18:11_

No objection to including this, PR welcome if anyone is up for it!

---

_Referenced in [astral-sh/ruff#8253](../../astral-sh/ruff/issues/8253.md) on 2023-11-13 21:56_

---

_Comment by @eremeevfd on 2023-11-14 12:20_

Some info regarding RDJson:

Here is description from reviewdog: https://github.com/reviewdog/reviewdog/tree/master/proto/rdf#rdjson

And it's jsonschema: https://github.com/reviewdog/reviewdog/blob/master/proto/rdf/jsonschema/DiagnosticResult.jsonschema

Also I've found actual jq parser for Hado-Lint: https://github.com/reviewdog/action-hadolint/blob/master/to-rdjson.jq

---

_Comment by @eremeevfd on 2023-11-14 13:15_

```jq
# Convert hadolint JSON output to Reviewdog Diagnostic Format (rdjson)
# https://github.com/reviewdog/reviewdog/blob/f577bd4b56e5973796eb375b4205e89bce214bd9/proto/rdf/reviewdog.proto
{
  source: {
    name: "ruff",
    url: "https://github.com/astral-sh/ruff"
  },
  diagnostics: . | map({
    message: .message,
    code: {
      value: .code,
      url: .url,
    },
    location: {
      path: .filename,
      range: {
        start: {
          line: .location.row,
          column: .location.column
        },
        end: {
            line: .end_location.row,
            column: .end_location.column
        }
      }
    },
    suggestions: (.fix.edits // {}) | map(
        {
            range: {
                start: {
                    line: .location.row,
                    column: .location.column
                },
                end: {
                    line: .end_location.row,
                    column: .end_location.column
                }
            },
            text: .content
        }
    ),
    severity: "WARNING"
  })
}
```

I've managed to apply this JQ to ruff check output:
```bash
ruff check ./ --output-format=json | jq -f "./to-rdjson.jq
```

And get this:
```json
{
  "source": {
    "name": "ruff",
    "url": "https://github.com/astral-sh/ruff"
  },
  "diagnostics": [
    {
      "message": "Import block is un-sorted or un-formatted",
      "code": {
        "value": "I001",
        "url": "https://docs.astral.sh/ruff/rules/unsorted-imports"
      },
      "location": {
        "path": "/Users/f.eremeev/hautaiou/saas-backend-core/source/assessor/views/b2b/assessments.py",
        "range": {
          "start": {
            "line": 1,
            "column": 1
          },
          "end": {
            "line": 38,
            "column": 1
          }
        }
      },
      "suggestions": [
        {
          "range": {
            "start": {
              "line": 1,
              "column": 1
            },
            "end": {
              "line": 38,
              "column": 1
            }
          },
          "text": "import uuid\n\nfrom django.db.models.manager import BaseManager\nfrom django.http import StreamingHttpResponse\nfrom drf_spectacular.utils import (\n    OpenApiExample,\n    OpenApiResponse,\n    OpenApiTypes,\n    extend_schema,\n    extend_schema_view,\n)\nfrom rest_framework import exceptions, parsers, serializers, viewsets\nfrom rest_framework.decorators import action\nfrom rest_framework.request import Request\nfrom rest_framework.response import Response\nfrom rest_framework.settings import api_settings\nfrom rest_framework_csv.renderers import CSVRenderer\n\nfrom assessor.models import Assessment\nfrom assessor.serializers.assessments import (\n    AssessmentCreate,\n    AssessmentRead,\n    AssessmentUpdate,\n    UploadImageComparisonPairs,\n)\nfrom assessor.services.assessments import (\n    ExportCsv,\n    ImageComparisonPairs,\n    ImageComparisonPairsExample,\n    SetImageComparisonPairsFromCSV,\n)\nfrom core_v2.models import Company, Image\nfrom drf_rw_serializers import generics\n\n"
        }
      ],
      "severity": "WARNING"
    },
    {
      "message": "Missing return type annotation for public function `get_queryset`",
      "code": {
        "value": "ANN201",
        "url": "https://docs.astral.sh/ruff/rules/missing-return-type-undocumented-public-function"
      },
      "location": {
        "path": "/Users/f.eremeev/hautaiou/saas-backend-core/source/assessor/views/b2b/assessments.py",
        "range": {
          "start": {
            "line": 73,
            "column": 9
          },
          "end": {
            "line": 73,
            "column": 21
          }
        }
      },
      "suggestions": [],
      "severity": "WARNING"
    }
  ]
}
```

---

_Comment by @keramblock on 2023-11-14 13:31_

But this will not work with format, AFAIK

---

_Comment by @eremeevfd on 2023-11-14 22:13_

Yep, format doesn't have any output formats except main diff

---

_Referenced in [astral-sh/ruff#11682](../../astral-sh/ruff/pulls/11682.md) on 2024-06-01 16:46_

---

_Comment by @tobb10001 on 2024-06-03 20:01_

In #11682 I implemented RDJson formatting for `ruff check`. I didn't address anything regarding formatting.

---

_Comment by @charliermarsh on 2024-06-03 20:03_

👍 For me this is enough to close -- we don't support alternate output formats for `format` at all right now.

---

_Closed by @charliermarsh on 2024-06-03 20:03_

---

_Comment by @keramblock on 2024-06-04 10:01_

@tobb10001 thanks a lot!

---

_Referenced in [evrone/evrone-django-template#21](../../evrone/evrone-django-template/issues/21.md) on 2024-12-23 13:55_

---
