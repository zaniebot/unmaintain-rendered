```yaml
number: 15021
title: "Add `actionlint` as a pre-commit hook (with shellcheck integration)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/add-actionlint
created_at: 2024-12-16T14:25:51Z
updated_at: 2024-12-16T17:33:05Z
url: https://github.com/astral-sh/ruff/pull/15021
synced_at: 2026-01-12T15:55:49Z
```

# Add `actionlint` as a pre-commit hook (with shellcheck integration)

---

_@AlexWaygood_

## Summary

This PR adds [actionlint](https://github.com/rhysd/actionlint) as a pre-commit hook. actionlint is a tool that analyzes your GitHub Actions and verifies that you have correct syntax in your workflows.

One of actionlint's most useful features is a [shellcheck](https://github.com/koalaman/shellcheck) integration. Shellcheck is a tool that checks for errors in shell scripts; actionlint's shellcheck integration extracts bash scripts from `run:` steps in GitHub Actions workflows and passes them to shellcheck. All the changes to workflow files in this PR are due to issues shellcheck identified via actionlint's shellcheck integration.

The motivation behind this PR is that it would have caught the mistakes fixed in https://github.com/astral-sh/ruff/pull/14938 before they happened. I'd like to upgrade our zizmor pre-commit hook to the latest version (https://github.com/astral-sh/ruff/pull/15022), but doing so requires further changes to our GitHub workflows; I'm unwilling to do that until we have better linters for our GitHub workflows in CI.

## Test Plan

- `uvx pre-commit run -a` passes locally, and I verified that it is correctly running the shellcheck integration due to the `additional_dependencies` in the pre-commit entry
- Let's see if CI passes on this PR...


---

_Label `ci` added by @AlexWaygood on 2024-12-16 14:25_

---

_Comment by @github-actions[bot] on 2024-12-16 14:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @AlexWaygood on 2024-12-16 14:44_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-12-16 14:44_

---

_@dhruvmanila reviewed on 2024-12-16 17:07_

---

_Review comment by @dhruvmanila on `.github/workflows/build-docker.yml`:294 on 2024-12-16 17:07_

This _might_ actually be ok to quote but it's ok for now to disable this as we might need to test this a bit to verify.

But, the other one is probably correct to ignore because it contains glob `*` character.

---

_@dhruvmanila reviewed on 2024-12-16 17:10_

---

_Review comment by @dhruvmanila on `.github/workflows/daily_fuzz.yaml`:49 on 2024-12-16 17:10_

It might be ok to quote this:

```sh
"$(shuf ...)"
```

Although I don't really have a linux machine to test. I'd be fine to ignore this.

---

_@AlexWaygood reviewed on 2024-12-16 17:12_

---

_Review comment by @AlexWaygood on `.github/workflows/daily_fuzz.yaml`:49 on 2024-12-16 17:12_

I checked it on my Mac, and the command copied-and-pasted into my shell works as it currently is on `main`, but adding the quoting breaks it. It's possible it might be different on Linux, but it's also just quite nice to use commands in our GitHub workflows that I can test locally

---

_@dhruvmanila reviewed on 2024-12-16 17:15_

---

_Review comment by @dhruvmanila on `.github/workflows/publish-docs.yml`:115 on 2024-12-16 17:15_

Maybe we should just use quotes on the outer value?
```suggestion
          pull_request_title="Update ruff documentation for ${display_name}"
```

---

_Review comment by @dhruvmanila on `.github/workflows/publish-docs.yml`:131 on 2024-12-16 17:15_

Same as above
```suggestion
            --body="Automated documentation update for ${display_name}" \
```

---

_@dhruvmanila reviewed on 2024-12-16 17:15_

---

_@dhruvmanila reviewed on 2024-12-16 17:16_

---

_Review comment by @dhruvmanila on `.pre-commit-config.yaml`:115 on 2024-12-16 17:16_

Can you provide the case where it's giving false positive?

---

_@dhruvmanila approved on 2024-12-16 17:17_

Looks good, I've some minor comments but otherwise should be good to go. Thanks for taking this on!

---

_@AlexWaygood reviewed on 2024-12-16 17:19_

---

_Review comment by @AlexWaygood on `.github/workflows/publish-docs.yml`:115 on 2024-12-16 17:19_

Thanks! Done

---

_@AlexWaygood reviewed on 2024-12-16 17:21_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:115 on 2024-12-16 17:21_

This is the report:

```
Lint GitHub Actions workflow files.......................................Failed
- hook id: actionlint
- exit code: 1

.github/workflows/publish-docs.yml:113:9: shellcheck reported issue in this script: SC2016:info:7:43: Expressions don't expand in single quotes, use double quotes for that [shellcheck]
    |
113 |         run: |
    |         ^~~~
.github/workflows/pr-comment.yaml:52:9: shellcheck reported issue in this script: SC2016:info:13:6: Expressions don't expand in single quotes, use double quotes for that [shellcheck]
   |
52 |         run: |
   |         ^~~~
```

they _might_ be fixable? Not sure. I don't think there's an actual bug on those lines anyway.

We could just put inline ignore comments on those lines? But that specific check didn't seem to actually flag any real issues in our shell scripts

---

_@dhruvmanila reviewed on 2024-12-16 17:31_

---

_Review comment by @dhruvmanila on `.pre-commit-config.yaml`:115 on 2024-12-16 17:31_

Oh right, I think the one in `publish-docs` is a false positive because the `pull_request_title` needs to be quoted as the title contains multiple words separated by whitespace:

```console
$ gh pr list --state open --json title --jq ".[] | select(.title == ${pull_request_title}) | .number"
failed to parse jq expression (line 1, column 35)
    .[] | select(.title == [red-knot] Statically known branches) | .number
                                      ^  unexpected token "Statically"
```

Let's ignore this for now as we know it's already working.

---

_Merged by @AlexWaygood on 2024-12-16 17:32_

---

_Closed by @AlexWaygood on 2024-12-16 17:32_

---

_Branch deleted on 2024-12-16 17:32_

---

_@dhruvmanila reviewed on 2024-12-16 17:32_

---

_Review comment by @dhruvmanila on `.github/workflows/daily_fuzz.yaml`:49 on 2024-12-16 17:32_

I see, let's keep it disabled then. Thanks for testing it.

---

_Comment by @AlexWaygood on 2024-12-16 17:33_

Thanks for the review!

---
