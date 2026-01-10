---
number: 3567
title: Default Subcommand
type: issue
state: closed
author: pvshvp-oss
labels: []
assignees: []
created_at: 2022-03-22T02:01:48Z
updated_at: 2022-03-25T01:22:46Z
url: https://github.com/clap-rs/clap/issues/3567
synced_at: 2026-01-10T01:27:43Z
---

# Default Subcommand

---

_Issue opened by @pvshvp-oss on 2022-03-22 02:01_

### Discussed in https://github.com/clap-rs/clap/discussions/3566

<div type='discussions-op-text'>

<sup>Originally posted by **shivanandvp** March 21, 2022</sup>
Is there a way for `clap` to have a _default subcommand_ that would be in effect if none are specified?
 
For example, I want to be able to use both
```
my_app create -t "some text" -p "some password"
```
and also
```
my_app -t "some text" -p "some password"
```
where "create" is assumed to be the default subcommand. However, when a different subcommand is specified, the default subcommand does not go into effect. For example:
```
my_app delete -id "1234"
```
Is there any stable or unstable item in the API to implement the above?</div>

---

_Comment by @epage on 2022-03-25 01:22_

For now, going to consider this resolved by #3570 

---

_Closed by @epage on 2022-03-25 01:22_

---
