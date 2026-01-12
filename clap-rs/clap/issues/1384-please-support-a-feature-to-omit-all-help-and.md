```yaml
number: 1384
title: Please support a feature to omit all help and messages
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2018-11-18T10:10:11Z
updated_at: 2022-09-19T19:16:38Z
url: https://github.com/clap-rs/clap/issues/1384
synced_at: 2026-01-12T16:14:10Z
```

# Please support a feature to omit all help and messages

---

_@joshtriplett_

For the purposes of reducing binary size, I'd love to have a new default feature flag for `clap` (that projects could then disable) that would omit generated `--help` and all friendly error/constraint messages, and minimize error reporting to just "Invalid arguments" or similar. That would help for embedded use cases that may want to reduce the sizes of binaries used in scripting, without regard to human invocations of those binaries.

---

_Comment by @CreepySkeleton on 2020-02-01 15:28_

Related to #1485 

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 15:29_

---

_Label `C: other` added by @CreepySkeleton on 2020-02-01 15:29_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 15:29_

---

_Comment by @CreepySkeleton on 2020-02-01 15:37_

Closing in favor of #1365 

---

_Closed by @CreepySkeleton on 2020-02-01 15:37_

---

_Comment by @Dylan-DPC-zz on 2020-02-01 15:41_

This may be a side effect of that but not directly related. Reopening 

---

_Reopened by @Dylan-DPC-zz on 2020-02-01 15:41_

---

_Label `C: other` removed by @pksunkara on 2020-02-01 19:09_

---

_Label `C: errors` added by @pksunkara on 2020-02-01 19:09_

---

_Label `W: 3.x` added by @pksunkara on 2020-02-01 19:09_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-01 19:09_

---

_Label `W: 3.x` removed by @pksunkara on 2020-10-10 19:24_

---

_Label `P1: urgent` added by @pksunkara on 2020-10-10 19:24_

---

_Removed from milestone `3.1` by @pksunkara on 2020-10-13 07:15_

---

_Added to milestone `3.0` by @pksunkara on 2020-10-13 07:15_

---

_Comment by @Rua on 2021-01-21 14:34_

I would like this as well, but supported as a runtime option instead of a feature flag, so that it can be enabled or disabled on a case-by-case basis.

---

_Comment by @ldm0 on 2021-01-28 17:25_

> I would like this as well, but supported as a runtime option instead of a feature flag, so that it can be enabled or disabled on a case-by-case basis.

@Rua This issue is mainly about reducing binary size. If you just wanna control the help message emission in the runtime, the right-now approach is using [try_get_matches](https://docs.rs/clap/3.0.0-beta.2/clap/struct.App.html#method.try_get_matches) to get a result and control the output by yourself. 

---

_Assigned to @ldm0 by @ldm0 on 2021-03-13 08:16_

---

_Removed from milestone `3.0` by @pksunkara on 2021-06-16 01:24_

---

_Unassigned @ldm0 by @pksunkara on 2021-06-16 01:25_

---

_Comment by @pksunkara on 2021-06-16 01:27_

Postponing this for later than 3.0 because we need design on how to divided the existing `help`, error messages, usage etc.. into separate feature flags.

---

_Referenced in [epage/clapng#109](../../epage/clapng/issues/109.md) on 2021-12-06 17:39_

---

_Label `C: errors` removed by @epage on 2021-12-08 20:25_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Referenced in [clap-rs/clap#2914](../../clap-rs/clap/issues/2914.md) on 2021-12-09 19:14_

---

_Comment by @epage on 2021-12-09 19:26_

https://github.com/clap-rs/clap/issues/2914 would unblock code-generating a display-agnostic(wrapping, styling) help message 

So breaking this down, parts we can trim down
- Simpler error messages
- Static help (`App::override_help`)
- Code-genned formatted help with re-rendering the help (built on #2914)
- Non-smart usage
- Static usage (`App::override_usage`)

The main question I have is how should we divide this up among feature flags without overwhelming people (with too many flags).  For example, a user could want to provide codegenned formatted help but still get all of the other nice features.  Next would be the bike shedding over the names.

To get the conversation started, I'll start with
- `helpgen`
  - Without `App::override_help`, no `--help` flag is generated
- `detailed-errors`
  - includes smart usage, people have to disable `helpgen` to get rid of smart usage

With that said, how we divide things up I think depends partly on the cost of each feature.  For example, if smart usage is more expensive, then maybe that should be broken out into its own feature flag.  Or if detailed errors isn't too expensive, maybe we don't provide it.  So I am assuming whatever decision we come up is a guideline and the implementer would need to measure the results and adapt accordingly.

---

_Label `P1: urgent` removed by @epage on 2021-12-09 19:27_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 19:27_

---

_Referenced in [clap-rs/clap#4111](../../clap-rs/clap/pulls/4111.md) on 2022-08-24 17:29_

---

_Referenced in [clap-rs/clap#4236](../../clap-rs/clap/pulls/4236.md) on 2022-09-19 18:32_

---

_Closed by @epage on 2022-09-19 19:16_

---
