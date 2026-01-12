```yaml
number: 13268
title: Only include rules with diagnostics in SARIF metadata
type: pull_request
state: merged
author: RussellLuo
labels:
  - cli
assignees: []
merged: true
base: main
head: remove-unnecessary-sarif-rules
created_at: 2024-09-06T12:02:38Z
updated_at: 2024-09-10T00:36:31Z
url: https://github.com/astral-sh/ruff/pull/13268
synced_at: 2026-01-12T15:55:43Z
```

# Only include rules with diagnostics in SARIF metadata

---

_@RussellLuo_

This PR can be considered as a follow-up of #13180.

## Motivation

The current Ruff's SARIF output contains all the 800+ rules, the vast majority of which have no relevance to the errors in results. IMO, this results in a large output and a lot of noise.

## Proposal

Per [the SARIF example](https://github.com/microsoft/sarif-tutorials/blob/main/docs/1-Introduction.md#a-simple-example), other linters such as ESLint only emits rules relevant to the errors in results. I think this could be an approach worth considering.

```json
{
  "version": "2.1.0",
  "$schema": "http://json.schemastore.org/sarif-2.1.0-rtm.4",
  "runs": [
    {
      "tool": {
        "driver": {
          "name": "ESLint",
          "informationUri": "https://eslint.org",
          "rules": [
            {
              "id": "no-unused-vars",
              "shortDescription": {
                "text": "disallow unused variables"
              },
              "helpUri": "https://eslint.org/docs/rules/no-unused-vars",
              "properties": {
                "category": "Variables"
              }
            }
          ]
        }
      },
      "artifacts": [
        {
          "location": {
            "uri": "file:///C:/dev/sarif/sarif-tutorials/samples/Introduction/simple-example.js"
          }
        }
      ],
      "results": [
        {
          "level": "error",
          "message": {
            "text": "'x' is assigned a value but never used."
          },
          "locations": [
            {
              "physicalLocation": {
                "artifactLocation": {
                  "uri": "file:///C:/dev/sarif/sarif-tutorials/samples/Introduction/simple-example.js",
                  "index": 0
                },
                "region": {
                  "startLine": 1,
                  "startColumn": 5
                }
              }
            }
          ],
          "ruleId": "no-unused-vars",
          "ruleIndex": 0
        }
      ]
    }
  ]
}
```

---

_Comment by @github-actions[bot] on 2024-09-06 12:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-08 18:26_

Thanks for the follow up. 

I'm not sure if reducing the rules to the reported tools is the right behavior. According to the [spec](https://docs.oasis-open.org/sarif/sarif/v2.1.0/cs01/sarif-v2.1.0-cs01.html#_Toc16012512)

> A toolComponent object MAY contain a property named rules whose value is an array of zero or more unique (§3.7.3) reportingDescriptor objects (§3.49) each of which provides information about an analysis rule supported by the tool component.

The way I understand this is that the `rules` table should list all rules supported by the tool and not just rules seen in this analysis run. Do you have a resource that states if it is okay for the rules table to only include the reported rules?

---

_Label `cli` added by @MichaReiser on 2024-09-08 18:27_

---

_Comment by @RussellLuo on 2024-09-09 00:00_

@MichaReiser Thanks for the reply!

I have found two resources that might be relevant:

1. [Rule metadata](https://github.com/microsoft/sarif-tutorials/blob/819b0f62f47ecde9a8f24dfc387c41926f5edabe/docs/2-Basics.md#rule-metadata)

    > Rule metadata is optional. An analysis tool can choose not to include it at all, to include metadata for only those rules that are relevant to the results, or to include metadata for all rules known to the tool.[13](https://github.com/microsoft/sarif-tutorials/blob/819b0f62f47ecde9a8f24dfc387c41926f5edabe/docs/2-Basics.md#note-13)

2. [Appendix E. (Informative) Locating rule and notification metadata](https://docs.oasis-open.org/sarif/sarif/v2.1.0/cs01/sarif-v2.1.0-cs01.html#_Toc16012891)

    > The SARIF format allows rule and notification metadata to be included in a SARIF log file (see §3.19.23 and §3.19.24). A SARIF log file does not need to include any metadata. This raises the questions of when metadata should be included in a log file, and how to locate the metadata if it is not included in the log file.
    >
    > Metadata should be included in a log file in the following circumstances:
    >
    > · The log file is intended to be viewed in a tool such as a log file viewer that needs to display metadata related to each result or notification even when the tool is not connected to a network.
    >
    > · The log file is intended to be uploaded to a result management system which requires information about every rule specified by every result, and which might not have prior knowledge of the rules specified by the results in this log file.
    >
    > · Neither of the above applies, but the increased log file size due to the metadata is not considered significant.

To summarize, including only the rules relevant to the results or including all rules is okay, but IMO the latter would result in a large output.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:31 on 2024-09-09 11:26_

We can simplify this to 

```suggestion
				let unique_rules: HashSet<_> = results.iter().filter_map(|result| result.rule).collect();
        let mut rules: Vec<SarifRule> = unique_rules.into_iter().map(SarifRule::from).collect();
        rules.sort_by(|a, b| a.code.cmp(&b.code));
```

and then use `rules` on line 46. This avoids allocating unnecessary `String`s 

---

_@MichaReiser approved on 2024-09-09 11:27_

Thanks. These two sections are extremely helpful. 

From the Appendix E:

> Metadata should be included in a log file in the following circumstances:

Our challenge is that we can't make assumptions about the use case and I don't fully understand the following scenario:

> The log file is intended to be uploaded to a result management system which requires information about every rule specified by every result, and which might not have prior knowledge of the rules specified by the results in this log file.

What do you make of this scenario? 

Either way. I'm slightly leaning towards moving forward with this until someone comes up with a use case where they need all metadata.

---

_Review comment by @RussellLuo on `crates/ruff_linter/src/message/sarif.rs`:31 on 2024-09-09 12:09_

Wow, this is fantastic!

(This PR is my second time writing Rust code. Learned a lot!)

---

_@RussellLuo reviewed on 2024-09-09 12:09_

---

_Comment by @RussellLuo on 2024-09-09 12:42_

> What do you make of this scenario?

To answer this question, let me analyze the second section (from spec) step by step.

> The log file is intended to be viewed in a tool such as a log file viewer that needs to display metadata related to each result or notification even when the tool is not connected to a network.

Regarding "display metadata related to each result", my understanding is that rules are primarily prepared for the results.

> The log file is intended to be uploaded to a result management system which requires information about every rule specified by every result, and which might not have prior knowledge of the rules specified by the results in this log file.

From "requires information about every rule specified by every result", again, I think rules are mainly used as additional details about the corresponding results.

> Neither of the above applies, but the increased log file size due to the metadata is not considered significant.

I'm currently using Ruff through [MegaLinter](https://megalinter.io/), which provides a webhook mechanism to send the SARIF output. Therefore, I'm concerned about the size of the HTTP request body (of the webhook).

---

_Comment by @MichaReiser on 2024-09-09 13:39_

> I'm currently using Ruff through [MegaLinter](https://megalinter.io/), which provides a webhook mechanism to send the SARIF output. Therefore, I'm concerned about the size of the HTTP request body (of the webhook).

I wouldn't be too concerned about that. I would suspect that the JSON file, even with all rule metadata, is still less than 1 MB, and HTTP compression should bring that down to 100 KB. 

---

_Comment by @RussellLuo on 2024-09-09 14:13_

Given this code snippet:

```python
# test.py
import os

def greet():
    print('hello {name}'.format())
```

I just performed a quick test:

```bash
# Using the official Ruff
$ ruff check --output-format=sarif test.py > official_result.json
$ du -sh official_result.json 
2.0M	official_result.json
# Using my local Ruff
$ ./target/debug/ruff check --output-format=sarif test.py > local_result.json
$ du -sh local_result.json   
8.0K	local_result.json
```

Overall, I agree with you that the size of the request body is not a big deal (although smaller is perhaps better). To my understanding of the [spec](https://docs.oasis-open.org/sarif/sarif/v2.1.0/cs01/sarif-v2.1.0-cs01.html#_Toc16012891), as described above, I think the rules are primarily used in conjunction with results. I'm not sure if there are scenarios where all rules are needed.

---

_Renamed from "Remove unnecessary SARIF rules" to "Only include rules with diagnostics in SARIF metadata" by @MichaReiser on 2024-09-09 21:23_

---

_Merged by @MichaReiser on 2024-09-09 21:23_

---

_Closed by @MichaReiser on 2024-09-09 21:23_

---

_Comment by @MichaReiser on 2024-09-09 21:24_

Thanks for making this contribution. This does sound reasonable to me and we can always revert back to the previous behavior if there's an actual use case for it.

---

_Branch deleted on 2024-09-10 00:36_

---
