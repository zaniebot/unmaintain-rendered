```yaml
number: 1256
title: "Feature request: Support a \"warning\" level for linting"
type: issue
state: open
author: lengau
labels:
  - configuration
  - core
assignees: []
created_at: 2022-12-16T01:47:57Z
updated_at: 2025-10-04T13:09:12Z
url: https://github.com/astral-sh/ruff/issues/1256
synced_at: 2026-01-12T15:54:41Z
```

# Feature request: Support a "warning" level for linting

---

_@lengau_

It would be really nice if, like `ignore`, we could select specific issues to turn into warnings rather than errors. That is, it would still output a warning to `stdout`, (perhaps separating errors and warnings), but it would still return with exit status 0. 

### Use case:

A large, older codebase that contains a lot of linting issues regarding specific rules. (For example, a codebase that predates type annotations.) By allowing warnings, we can make these issues visible to developers for fixing as they lint their changes, but we don't have to have a separate configuration for our pre-commit hooks, CI tools, etc. to ensure they ignore these issues.

---

_Renamed from "Support a "warning" level for linting" to "Feature request: Support a "warning" level for linting" by @lengau on 2022-12-16 01:48_

---

_Label `enhancement` added by @charliermarsh on 2022-12-16 02:46_

---

_Label `configuration` added by @charliermarsh on 2022-12-16 02:46_

---

_Comment by @charliermarsh on 2022-12-16 02:47_

Yeah, this would be nice to have. It would also be helpful for editor integrations, since the LSP can differentiate between warnings and errors.


---

_Comment by @charliermarsh on 2022-12-16 04:57_

Any thoughts on what the API should look like?

ESLint lets you map from rule name to level, like:

```
...
"eqeqeq": "error",
"curly": "warning",
...
```

Which makes sense, though they also use that system to turn errors on and off -- and we instead have the Flake8-like `ignore` and `select` system.

The most basic thing would be a map from check code prefix to level, like:

```toml
[tool.ruff.severity]
"F401" = "warning"
"F481" = "error"
# Probably also want to support prefixes.
"E" = "warning"
```

---

_Comment by @lengau on 2022-12-17 01:45_

Not really sure... Naively, it appears to me that the most consistent way to configure it would be to have `warn` and `extend-warn` (or `warning` & `extend-warning`).

[This file](https://github.com/canonical/craft-parts/pull/337/files#diff-50c86b7ed8ac2cf95bd48334961bf0530cdc77b5a56f852c5c61b89d735fd711) contains what I'm doing currently for the project that inspired this request.

I enabled more linting than we previously had (everything I viewed as potentially appropriate for this app), but I put everything that raised an error into the ignore field. With this feature, lines 78-102 would've been in a `warn` section instead of inside `ignore`, with the goal of eventually emptying the `warn` section by consensus. 

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:14_

---

_Label `configuration` removed by @charliermarsh on 2022-12-31 18:14_

---

_Label `core` added by @charliermarsh on 2022-12-31 18:14_

---

_Label `configuration` added by @charliermarsh on 2022-12-31 18:14_

---

_Comment by @edgarrmondragon on 2023-01-04 06:17_

fwiw `bandit` has four severity levels: undefined, low, medium and high.

https://github.com/PyCQA/bandit/blob/af9f8dc09f7239f3c3481177a24a72dcb41ed450/bandit/core/constants.py#L8

---

_Comment by @jfmengels on 2023-01-17 21:31_

I would recommend against the implementation of warning (or severity levels). I wrote about why [in this blog post](https://jfmengels.net/disable-comments/) (and also in a ~as-of-yet-unreleased~ talk [Why you don't trust your linter](https://www.youtube.com/watch?v=XjwJeHRa53A)).

In my opinion, it's one of those features (along with disable comments) that gets copied over and over mostly because other linters have them. The fact that no linter docs ever recommend when to use these features is a sign of that in my opinion.

I can potentially see a benefit for this while seeing the errors in an editor, but it shouldn't IMO impact the behavior of the linter.

In [`elm-review`](https://elm-review.com) there are no severity levels (nor disable comments): rules are enforced or they are not. Though to be taken with a grain of salt: the linter also has a lot less false positives than is probably possible for Python because of the target language.

> A large, older codebase that contains a lot of linting issues regarding specific rules

This is exactly the kind of problem that the proposal in #1149 aims to solve, and is a much better solution.

---

_Comment by @jacobmischka on 2023-01-17 21:38_

> I can potentially see a benefit for this while seeing the errors in an editor, but it shouldn't IMO impact the behavior of the linter.

How would the editor show the proper type of diagnostic to the user if the linter doesn't provide that information?

---

_Comment by @jfmengels on 2023-01-17 21:43_

If by "type of diagnostic" you mean warning or error, it wouldn't be able to. I meant to say that this feature request could be useful for editors for that purpose. But that otherwise severity levels is not useful or recommended.

---

_Comment by @lengau on 2023-01-30 02:10_

@jfmengels while what you mention would be ideal, I don't believe it's practical for ruff.

### Disable comments

Let's take for example a project that has its own [`Formatter`](https://docs.python.org/3/library/logging.html?highlight=logging#logging.Formatter) subclass for logging. Perhaps it needs to implement its own `formatException` method. Now here I think the issue should start to become clear. The Python community as a whole have generally embraced most of [PEP8](https://peps.python.org/pep-0008/) for code formatting. If this codebase otherwise uses [pep8-naming](https://github.com/charliermarsh/ruff#pep8-naming-n), linting will fail for the newly-written `formatException` override.

With disable comments, this is straightforward. Simply write a comment like `# noqa: N815 - non-pep8 method name is needed for overriding parent method.` This is simple, straightforward, and reviewable. If a developer adds a disable comment without a proper comment, there's also a straightforward way to address this — at review time. It would be nice to automate this, perhaps requiring a certain amount of justification for any `noqa` comment.

Without disable comments, a developer is left with a host of options, none of them good. Do we disable the rule? That seems drastic, especially since it's a rule that's widely accepted, so we may well want to apply it to the rest of our code. Even with [fine-grained rule configuration](https://jfmengels.net/disable-comments/#fine-grained-rule-configuration), we're left with only the inferior choice of having to disable it for the whole file.

Now, it's quite a reasonable response to say that the linter should handle this case. And sure, I agree in principle. It would be a better outcome if the linter understood that I'm overriding a parent method and didn't yell at me about it. Of course, it's not like this is the only example of a rule that might need changing - or indeed of one that we might want to disable or modify in specific cases. Perhaps we need to `catch Exception` somewhere, which is normally bad practice but can be appropriate in, for example, a web framework, to ensure that an error in handling one request doesn't break the whole service.

Perhaps elm as a language doesn't have this. I'm not familiar enough with it to say. But that's a fact of life in the Python world, and a linting tool isn't going to have a way around it in all cases. In the end, that's something we have to deal with. 

## Severity levels

`elm-review` also seems to fit in a pretty different niche from what `ruff` appears to be intended to operate in, and this is where @jacobmischka's point comes in. If `ruff` were only for use in CI, I might have different opinions about severity levels. But as it advertises [editor integrations](https://github.com/charliermarsh/ruff#editor-integrations), includes autofix support and includes some [aggressive rules](https://jfmengels.net/disable-comments/#aggressive-and-relaxed-rules), a variety of levels would be very useful. I do agree that baselines are a better way to implement the specific use case I outlined, but that's an example of an issue with the user story, not with the underlying request.

## Thank you

That's not to say that your post wasn't without inspiration for me. To start, I think having certain rules [ignore noqa commets](https://jfmengels.net/disable-comments/#non-disableable-rules-by-default) is a great idea, and this could be implemented with only a small tweak to my feature request. A "critical" level would be great for allowing a project maintainer to prevent certain rules from being disabled. I'd imagine rules like F401 (unused import), F631/631 (assert/if is always true) and E999 (syntax error) could even be made critical by default. With that, explanations for noqa comments and the already-extant `RUF100` rule to alert about unused `noqa` comments (allowing an update to ruff to alert the user of fixes to bugs in the linter).

---

_Comment by @charliermarsh on 2023-02-02 15:57_

@not-my-profile - I'm considering adding this, but since you've been working on the violation API a lot, I wanted to get your feedback first. Here's a proposal.

- Add a `Severity` enum, with `Warning` and `Error` for now (which map to the LSP's `DiagnosticSeverity`).
- Add a const on the `Violation` trait that allows rules to define their default severity, like `AUTOFIX`. All rules will be `Error` for now, so we could consider omitting this at the moment.
- Add a `warnings = [...]` setting in `pyproject.toml`, that's functionally similar to `fixable`. All rules are errors by default, so users can downgrade them to `warnings` like so: `warnings = ["B"]`.
- In the CLI output, and in `ruff-lsp`, differentiate errors from warnings.

This wouldn't include making it possible to run Ruff and _only_ show warnings, or _only_ show errors (e.g., `ruff --select warnings`), but we could do that in the future.


---

_Comment by @not-my-profile on 2023-02-02 17:24_

Thanks for consulting with me :) I would hold off on adding any such constants to the `Violation` trait till we have implemented rule categories #1774 ... I am not sure if adding constants to `Violation` is the best way to model non-autofix-related rule metadata ... I'll have to think about it.

> Add a Severity enum, with Warning and Error for now (which map to the LSP's DiagnosticSeverity).

LSP also supports [other levels](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticSeverity):

* error
* warning
* information
* hint

I think it might make more sense to support all of them via a configuration like:

```toml
[tool.ruff.severities]
B = "information"
```

> e.g., `ruff --select warnings`

I don't think we should overload rule selectors for this. I think I'd prefer a dedicated CLI option like `--severity` to specify the minimum level that is reported.

---

_Comment by @charliermarsh on 2023-02-02 17:40_

> Thanks for consulting with me :)

Of course! You're driving a lot of the work on the violations API. Definitely want your help figuring these parts out :)

> LSP also supports other levels...

Yeah I've considered this... I wasn't sure if they would really be relevant to us. Like Clippy only has allow, warn, deny, and none. I'm not sold either way.

> I don't think we should overload rule selectors for this. I think I'd prefer a dedicated CLI option like --severity to specify the minimum level that is reported.

Makes sense -- just an example to articulate what I _wasn't_ planning to do on the first pass. I agree that the API you're suggesting is better.


---

_Comment by @not-my-profile on 2023-02-02 18:14_

> I wasn't sure if they would really be relevant to us.

I am not sure either. I still think a config like:

```toml
[tool.ruff.severities]
B = "warning"
```

is better than

```toml
[tool.ruff]
warning = ["B"]
```

because the former is more expressive, e.g.

```toml
[tool.ruff.severities]
B = "warning"
B007 = "error"
```

> Like Clippy only has allow, warn, deny, and none.

These are not defined by clippy ... these are the [rustc lint levels](https://doc.rust-lang.org/rustc/lints/levels.html):

1. allow
2. warn
3. force-warn
4. deny
5. forbid

But what you're saying makes sense ... I think it makes sense to start out with just supporting "warning" & "error".

Actually I think we probably should rather model this after clippy since I think it would probably make sense for us to also support "force-warn" and "forbid" (which basically is "force-deny").

So we could have:

```toml
[tool.ruff.levels]
B = "warn"
B007 = "deny"
```

We could actually also deprecate our `ignore` & `select` settings in favor of e.g.

```toml
[tool.ruff.levels]
E501 = "allow"
```

The nice thing about the Rust terminology is that "allow" and "deny" are semantic opposites (while "ignore" and "error" aren't).

---

_Comment by @charliermarsh on 2023-02-02 19:07_

Yeah I generally agree.

My only hesitation around modeling this after Clippy is that the API will be new to most users, whereas `warning` and `error` are more common (e.g. they also map 1:1 to the LSP).

But... it'd be nice to have a unified API for turning errors "on" and "off", _and_ for setting their severities, as with Clippy. (E.g., if we add `[tool.ruff.severities]` but use it in conjunction with `select` and `ignore`, you could turn the B rules "off" _and_ set their severity to "warning", which is strange.)


---

_Comment by @Avasam on 2023-03-07 16:27_

I have two main uses for setting a linter rule as warning:
1. Waterleak principle: When introducing linting to an existing project, I can turn rules that don't pass to warning. Stop new violations from being introduced, while gradually fixing existing ones.
  1.1 Similarly, a rule that flags special comments and has a "warning" severity lets "TODO/FIXME/XXX" comments appear in the CLI, CI, "problems" tab of VSCode and be annotated in PRs/CI, whilst not blocking. 
2. Some rules are too context-specific, but should still be brought to the dev's attention. It has a semantic meaning for my teams where those violations are more acceptable to be ignored / suppressed, whilst suppressing "linting errors" is generally frowned upon unless you have a really good reason to. Especially if that rule comes from a preset.

---

_Comment by @Agalin on 2023-03-10 14:49_

I'd really like this to support more than just "error" and "warning".

While discussion in this issue is mostly about editor, ruff is also used in CI and CI apps also use severities. [Gitlab defines 5](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implement-a-custom-tool) and none of them is `warning` nor `error`...

Approach proposed by @not-my-profile would make it possible to handle - if severity list is extended or free form supported.

Why such granularity may be useful in CI?
Because ruff includes rules of really varying importance. If you have an important fix to deploy it may pass review with imports not sorted properly (isort rule) but not with a serious security issue detected by a bandit rule.

---

_Comment by @teucer on 2023-03-24 23:48_

Having configurable severity levels is important for pipelines. Having gitlab levels (potentially preconfigured) would add a lot of value. 

Right now everything is major. I have added a script to postprocess the generated json file, but it is very hard to decide on the levels.

---

_Comment by @JSpenced on 2023-06-09 18:27_

Any progress here or when we think this will go into a version? 

---

_Comment by @tgross35 on 2023-06-14 21:38_

If user configuration of lint levels is desired, I think it could be reasonable to follow the design specified in Rust's [`manifest-lint` RFC](https://rust-lang.github.io/rfcs/3389-manifest-lint.html), since it gives pretty good reasoning for design choices and was thought through pretty thoroughly. Something like:

```toml
select = ["E", "F", "B"]

[lints]
E123 = "warn"
F456 = { level = "error", priority = 100 }
```

(priority probably isn't really required). Naming the table something like `lints` instead of `severity` has the benefit that it makes sense if you want to add per-lint config that isn't only severity level, e.g.

```toml
[lints.E123]
option-a = "syrup"
ignore = "**/pancakes.py"
# would fix https://github.com/astral-sh/ruff/issues/1992#issuecomment-1405497643
autofix = "never"
```

---

_Comment by @kaddkaka on 2024-04-09 08:53_

Jenkins CI has a plugin called warnings-ng for reporting issues from various tools. see https://github.com/jenkinsci/warnings-ng-plugin/blob/main/doc/Documentation.md#issues-history-new-fixed-and-outstanding-issues

Unfortunately ruff format is not supported yet. If we could have that it might be able to detect which warnings are new and which are old. This would make it easier to start using ruff in a repo with a lot of warnings by using a baseline.

see https://github.com/jenkinsci/warnings-ng-plugin/blob/main/SUPPORTED-FORMATS.md

---

_Comment by @Matthieu-LAURENT39 on 2024-07-03 07:58_

I think having a "info" level in addition to "warning" and "error" could be nice for some lints like PTH (`flake8-use-pathlib`), which i usually enable to remind me and others when we could be using pathlib, but we don't want to strictly enfore that.
I'm not sure about other IDEs, but VSCode also supports displaying with an info level.

---

_Comment by @MusicalNinjaDad on 2024-07-04 09:20_

> I think having a "info" level in addition to "warning" and "error" could be nice for some lints like PTH (`flake8-use-pathlib`), which i usually enable to remind me and others when we could be using pathlib, but we don't want to strictly enfore that. I'm not sure about other IDEs, but VSCode also supports displaying with an info level.

Personally, I use `--add-noqa` for these kind of info lints. Then I get the info however I look at the codebase, can search for them if e.g. I want to look at migrating to using pathlib, and get in-IDE popups (in VSCode) with the rule details.

Maybe having customisability of --add-noqa would be a solution to this use case? So for example you can specify a rule(set) e.g.:
`ruff --add-noqa PTH` or `ruff --add-noqa D4` etc...? 

---

_Comment by @dhruvmanila on 2024-07-05 02:10_

> Maybe having customisability of --add-noqa would be a solution to this use case? So for example you can specify a rule(set) e.g.:
> `ruff --add-noqa PTH` or `ruff --add-noqa D4` etc...?

This is possible by just combining it with `--select`. So, your examples would become
```
ruff check --select PTH --add-noqa
ruff check --select D4 --add-noqa
```
This should only add `noqa` comments for the selected rules.

---

_Comment by @FichteFoll on 2024-08-21 09:32_

Since it hasn't been explicitly mentioned yet, I'd also like to point one use case of autofixing. 

For some eslint project, I have turned the most common autofixable lint errors into warnings so that I am aware of them while editing. Since I personally am auto-formatting on save (and fixing all these autofixables at once) I don't need them to be marked as warnings, but not everyone else on the team might be, I still want these to cause the CI lint pass to fail.

Thus, perhaps a way of specifying the level/severity of all autofixable rules at once on a machine-by-machine basis, e.g. by a parameter to the language server, would make sense here.

---

_Comment by @AmeerArsala on 2024-11-19 23:42_

pls do this. I just want to put a warning on unused imports, nothing else

---

_Comment by @kaddkaka on 2024-11-20 04:53_

> Jenkins CI has a plugin called warnings-ng for reporting issues from various tools. see https://github.com/jenkinsci/warnings-ng-plugin/blob/main/doc/Documentation.md#issues-history-new-fixed-and-outstanding-issues
> 
> Unfortunately ruff format is not supported yet. If we could have that it might be able to detect which warnings are new and which are old. This would make it easier to start using ruff in a repo with a lot of warnings by using a baseline.
> 
> see https://github.com/jenkinsci/warnings-ng-plugin/blob/main/SUPPORTED-FORMATS.md

Junit support was actually added in #968

---

_Comment by @ShravanSunder on 2025-07-25 11:45_

It would be great to have severity levels and have this be integrated into ruff vscode plugin

---

_Comment by @Sillocan on 2025-08-15 23:50_

I would absolutely love warnings in codeclimate rather than errors for some errors such as TODOs

---

_Comment by @beauxq on 2025-10-04 13:00_

I would like configurable severity levels.

Please don't design it in a way that only works for "error" and "warning". Other levels, like "hint" are also useful.

Other tools that I like that use "hint", don't emit anything at all for "hint" in the CLI output, and only show the "hint" diagnostic in the editor.

Also, I'm worried about how it will be to specify large groups of rules efficiently.
For example, I'd like to be able to say something like:
All of `B` is "warning", except for `B016` which is "error".
And all of `E` is "error", except for `E115` which is "warning".
And the rest of `ALL` is all "hint", except for `PLC0208` which is "warning" and `A001` which is "error".

It would be bad to have to specify hundreds of "hint" lines in `ruff.toml` for something like that.

---
