```yaml
number: 21035
title: Document when a rule was added
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/rule-versions
created_at: 2025-10-22T19:36:41Z
updated_at: 2025-10-23T18:48:43Z
url: https://github.com/astral-sh/ruff/pull/21035
synced_at: 2026-01-10T17:34:34Z
```

# Document when a rule was added

---

_Pull request opened by @ntBre on 2025-10-22 19:36_

Summary
--

Inspired by #20859, this PR adds the version a rule was added, and the file and line where it was defined, to `ViolationMetadata`. The file and line just use the standard `file!` and `line!` macros, while the more interesting version field uses a new `violation_metadata` attribute parsed by our `ViolationMetadata` derive macro.

I moved the commit modifying all of the rule files to the end, so it should be a lot easier to review by omitting that one.

As a curiosity and a bit of a sanity check, I also plotted the rule numbers over time:

<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/75b0b5cc-3521-4d40-a395-8807e6f4925f" />

I think this looks pretty reasonable and avoids some of the artifacts the earlier versions of the script ran into, such as the `rule` sub-command not being available or `--explain` requiring a file argument.

<details><summary>Script and summary data</summary>

```shell
gawk --csv '
NR > 1 {
    split($2, a, ".")
    major = a[1]; minor = a[2]; micro = a[3]
    # sum the number of rules added per minor version
    versions[minor] += 1
}
END {
    tot = 0
    for (i = 0; i <= 14; i++) {
        tot += versions[i]
        print i, tot
    }
}
' ruff_rules_metadata.csv > summary.dat
```

```
0 696
1 768
2 778
3 803
4 822
5 848
6 855
7 865
8 893
9 915
10 916
11 924
12 929
13 932
14 933
```

</details>

Test Plan
--

I built and viewed the documentation locally, and it looks pretty good!

<img width="1466" height="676" alt="image" src="https://github.com/user-attachments/assets/5e227df4-7294-4d12-bdaa-31cac4e9ad5c" />

The spacing seems a bit awkward following the `h1` at the top, so I'm wondering if this might look nicer as a footer in Ruff. The links work well too:
- [v0.0.271](https://github.com/astral-sh/ruff/releases/tag/v0.0.271)
- [Related issues](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20airflow-variable-name-task-id-mismatch)
- [View source](https://github.com/astral-sh/ruff/blob/main/crates%2Fruff_linter%2Fsrc%2Frules%2Fairflow%2Frules%2Ftask_variable_name.rs#L34)

The last one even works on `main` now since it points to the `derive(ViolationMetadata)` line.

In terms of binary size, this branch is a bit bigger than main with 38,654,520 bytes compared to 38,635,728 (+20 KB). I guess that's not _too_ much of an increase, but I wanted to check since we're generating a lot more code with macros.

---

_Label `documentation` added by @ntBre on 2025-10-22 19:36_

---

_Comment by @github-actions[bot] on 2025-10-22 19:56_

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

_Marked ready for review by @ntBre on 2025-10-22 20:12_

---

_Review requested from @AlexWaygood by @ntBre on 2025-10-22 20:12_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-22 20:13_

---

_Comment by @MeGaGiGaGon on 2025-10-22 22:45_

That's fun, I wasn't aware `ruff rule` existed, that would have made my other two previous adventures with iterating over all the rules much easier. I'll have to go back at some point and update my scripts for those, since I may have missed a couple rules while regexing my way through the problems.

Related to that, this may be a stupid question, but will the changes in this PR be reflected in `ruff rule`? (As in, after this will `ruff rule --all --output-format json` also include items for version, file, and line?) That would be even more convenient for my purposes, since I wouldn't need to do any of the regex spaghetti to get rule definition location info in the first place.

---

_@MeGaGiGaGon reviewed on 2025-10-23 01:01_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_dev/src/generate_docs.rs`:52 on 2025-10-23 01:01_

Apply with caution, completely untested.

Two suggestions on the issues link:
- Surround the rule name with quotes (`%27` is `'`), since otherwise GitHub doesn't treat it as a single word, and a lot of extra results appear
- Use the new-ish `OR` operator + parenthesis to search for both the rule name and code at the same time, since some issues include only one of the two.

As an example with a rule that has less, more common words: The [generated link](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20non-self-return-type) for `non-self-return-type` gives 16 open and 22 closed, many of which are not related, and do not mention the rule name at all. This [new link](https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20(%27non-self-return-type%27%20OR%20PYI034)) only gives 3 open and 8 closed, all of which at least mention the rule.

```suggestion
            let _ = writeln!(
                &mut output,
                r#"<small>
Added in <a href="https://github.com/astral-sh/ruff/releases/tag/{version}">{version}</a> ·
<a href="https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20(%27{encoded_name}%27%20OR%20{rule_code})" target="_blank">Related issues</a> ·
<a href="https://github.com/astral-sh/ruff/blob/main/{file}#L{line}" target="_blank">View source</a>
</small>

"#,
                version = rule.version().unwrap_or(env!("CARGO_PKG_VERSION")),
                encoded_name =
                    url::form_urlencoded::byte_serialize(rule.name().as_str().as_bytes())
                        .collect::<String>(),
                rule_code = rule.noqa_code(),
                file =
                    url::form_urlencoded::byte_serialize(rule.file().replace('\\', "/").as_bytes())
                        .collect::<String>(),
                line = rule.line(),
            );
```

---

_@MichaReiser reviewed on 2025-10-23 06:12_

Nice! That's what you were up to yesterday :)

`ViolationMetadata` seems the right spot for `file` and `location` but I'm less sure about the version. Mainly because the rule status is now spread over two places:

* `code_to_rule`: Defines the status of the rule (preview, removed, stable)
* `violation_metadata`: Defines the version. 

This has the downside that It's unclear what the version represents. Is it when the rule was added, deprecated or removed?

That's why I'm inclined to combine the version and the `RuleStatus`, similar to `LintStatus` in ty. Whether we simply extend the `RuleStatus` variants to have extra fields or move `RuleStatus` to `ViolationMetadata` (that's probably what I'd do if I would do it anew) isn't important and I'd probably go with what's easier. 



---

_@ntBre reviewed on 2025-10-23 12:47_

---

_Review comment by @ntBre on `crates/ruff_dev/src/generate_docs.rs`:52 on 2025-10-23 12:47_

That seems like a good idea, thank you!

---

_Comment by @ntBre on 2025-10-23 12:56_

> Related to that, this may be a stupid question, but will the changes in this PR be reflected in `ruff rule`? (As in, after this will `ruff rule --all --output-format json` also include items for version, file, and line?) That would be even more convenient for my purposes, since I wouldn't need to do any of the regex spaghetti to get rule definition location info in the first place.

@MeGaGiGaGon I think that's a great question and a good idea to add. It crossed my mind early on but then I kind of forgot about it. I think it should be straightforward to add here.

@MichaReiser that makes a lot of sense. I did like how `LintStatus` looked in ty. I'll try to see how it looks to move `RuleGroup` into the new macro attribute this morning, but I think adding the version as a field is probably easier.


---

_Comment by @ntBre on 2025-10-23 15:33_

As Micha and I discussed on Discord, the `RuleGroup` and version information have now been combined, more like `LintStatus` in ty. This is an example usage of the new attribute:

```rust
#[derive(ViolationMetadata)]
#[violation_metadata(stable_since = "v0.0.155")]
pub(crate) struct UnusedNOQA {
    pub codes: Option<UnusedCodes>,
}
```

One interesting and somewhat intentional aspect of the attribute is that the macro will preserve the last entry, so we could theoretically stack versions if the status of a rule changes over time. For example, this would be accepted by the current macro implementation but only use the `removed_since` field:

```rust
#[derive(ViolationMetadata)]
#[violation_metadata(
    preview_since = "v0.0.123", 
    stable_since = "v0.0.155", 
    deprecated_since = "v1.2.3", 
    removed_since = "v4.5.6",
)]
pub(crate) struct UnusedNOQA {
    pub codes: Option<UnusedCodes>,
}
```

Another alternative we considered was just using the `RuleGroup` variants directly, like:

```rust
#[derive(ViolationMetadata)]
#[violation_metadata(status = RuleGroup::Stable { since: "v0.0.155" })]
pub(crate) struct UnusedNOQA {
    pub codes: Option<UnusedCodes>,
}
```

which is also pretty nice. If we ever decide to  add a `reason` field like ty has for the deprecated and removed variants this might be even more compelling. Or we could just add a `reason` field to the macro too.

<hr>

I also updated the JSON output format for `ruff rule` to include the new file and line information (the version was added automatically by including it in `RuleGroup`).

And I rebased a bit more to keep the big commits at the end. The last commit is still `add metadata to rules` and the second-to-last is the big edit to remove the `RuleGroup` entries from the `map_codes` table.

---

_@MichaReiser approved on 2025-10-23 15:51_

This works for me. I think having `status = ` might be slightly easier to discover as you can type `RuleStatus::` to see all the variants that are supported. But I don't feel strongly about the syntax (having all versions stacked looks a bit nosiy and having multiple fields might be easier to parse). 

---

_Comment by @ntBre on 2025-10-23 16:02_

Sounds good! I guess I'll go ahead and land this once I fix the doctests and remove the scripts and CSV since it should be easy to iterate on the syntax now that we have the infrastructure in place.

Maybe this is just my editor, but I wasn't getting completions when I tried typing `RuleGroup::` in the attribute. But otherwise I like that fine too, no strong feelings on the exact syntax.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-23 16:09_

---

_Comment by @ntBre on 2025-10-23 16:24_

One last check to make sure my scripts didn't change any of the rule statuses:

```shell
diff \
    <(./target/debug/ruff rule --all --output-format json | jq '[ .[] | { code, status: ( .status | keys[0] ) } ]') \
    <(../worktrees/ruff1/target/debug/ruff rule --all --output-format json | jq '[ .[] | { code, status} ]')
```

passes cleanly with main checked out in my other worktree.

---

_Comment by @ntBre on 2025-10-23 16:30_

Oh, I just realized that many of the versions are actually wrong after the `RuleGroup` change... I collected all of the `Added in` versions but now they need to be when they were stabilized or removed for many rules instead.

---

_Comment by @ntBre on 2025-10-23 18:37_

All of the `removed_since` and `stable_since` versions are updated. Preview should have already been correct, and we don't currently have any deprecated rules. I'll merge once CI finishes and then start the release for the day. Thanks again!

---

_Merged by @ntBre on 2025-10-23 18:48_

---

_Closed by @ntBre on 2025-10-23 18:48_

---

_Branch deleted on 2025-10-23 18:48_

---
