```yaml
number: 2626
title: various rollup + move off of Clap to lexopt
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/more-work
created_at: 2023-10-12T15:22:03Z
updated_at: 2024-06-23T17:16:03Z
url: https://github.com/BurntSushi/ripgrep/pull/2626
synced_at: 2026-01-12T18:23:14Z
```

# various rollup + move off of Clap to lexopt

---

_@BurntSushi_

cli: replace clap with lexopt and supporting code

ripgrep began it's life with docopt for argument parsing. Then it moved
to Clap and stayed there for a number of years. Clap has served ripgrep
well, and it probably could continue to serve ripgrep well, but I ended
up deciding to move off of it.

Why?

The first time I had the thought of moving off of Clap was during the
2->3->4 transition. I thought the 3.x and 4.x releases were great, but
for me, it ended up moving a little too quickly. Since the release of
4.x was telegraphed around when 3.x came out, I decided to just hold off
and wait to migrate to 4.x instead of doing a 3.x migration followed
shortly by another 4.x migration. Of course, I just never ended up doing
the migration at all. I never got around to it and there just wasn't a
compelling reason for me to upgrade. While I never investigated it, I
saw an upgrade as a non-trivial amount of work in part because I didn't
encapsulate the usage of Clap enough.

The above is just what got me started thinking about it. It wasn't
enough to get me to move off of it on its own. What ended up pushing me
over the edge was a combination of factors:

* As mentioned above, I didn't want to run on the migration treadmill.
This has proven to not be much of an issue, but at the time of the
2->3->4 releases, I didn't know how long Clap 4.x would be out before a
5.x would come out.
* The release of lexopt[1] caught my eye. IMO, that crate demonstrates
exactly how something new can arrive on the scene and just thoroughly
solve a problem minimalistically. It has the docs, the reasoning, the
simple API, the tests and good judgment. It gets all the weird corner
cases right that Clap also gets right (and is part of why I was
originally attracted to Clap).
* I have an overall desire to reduce the size of my dependency tree. In
part because a smaller dependency tree tends to correlate with better
compile times, but also in part because it reduces my reliance and trust
on others. It lets me be the "master" of ripgrep's destiny by reducing
the amount of behavior that is the result of someone else's decision
(whether good or bad).
* I perceived that Clap solves a more general problem than what I
actually need solved. Despite the vast number of flags that ripgrep has,
its requirements are actually pretty simple. We just need simple
switches and flags that support one value. No multi-value flags. No
sub-commands. And probably a lot of other functionality that Clap has
that makes it so flexible for so many different use cases. (I'm being
hand wavy on the last point.)

With all that said, perhaps most importantly, the future of ripgrep
possibly demands a more flexible CLI argument parser. In today's world,
I would really like, for example, flags like `--type` and `--type-not`
to be able to accumulate their repeated values into a single sequence
while respecting the order they appear on the CLI. For example, prior
to this migration, `rg regex-automata -Tlock -ttoml` would not return
results in `Cargo.lock` in this repository because the `-Tlock` always
took priority even though `-ttoml` appeared after it. But with this
migration, `-ttoml` now correctly overrides `-Tlock`. We would like to
do similar things for `-g/--glob` and `--iglob` and potentially even
now introduce a `-G/--glob-not` flag instead of requiring users to use
`!` to negate a glob. (Which I had done originally to work-around this
problem.) And some day, I'd like to add some kind of boolean matching to
ripgrep perhaps similar to how `git grep` does it. (Although I haven't
thought too carefully on a design yet.) In order to do that, I perceive
it would be difficult to implement correctly in Clap.

I believe that this last point is possible to implement correctly in
Clap 2.x, although it is awkward to do so. I have not looked closely
enough at the Clap 4.x API to know whether it's still possible there. In
any case, these were enough reasons to move off of Clap and own more of
the argument parsing process myself.

This did require a few things:

* I had to write my own logic for how arguments are combined into one
single state object. Of course, I wanted this. This was part of the
upside. But it's still code I didn't have to write for Clap.
* I had to write my own shell completion generator.
* I had to write my own `-h/--help` output generator.
* I also had to write my own man page generator. Well, I had to do this
with Clap 2.x too, although my understanding is that Clap 4.x supports
this. With that said, without having tried it, my guess is that I
probably wouldn't have liked the output it generated because I
ultimately had to write most of the roff by hand myself to get the man
page I wanted. (This also had the benefit of dropping the build
dependency on asciidoc/asciidoctor.)

While this is definitely a fair bit of extra work, it overall only cost
me a couple days. IMO, that's a good trade off given that this code is
unlikely to change again in any substantial way. And it should also
allow for more flexible semantics going forward.

Fixes #884, Fixes #1648, Fixes #1701, Fixes #1814, Fixes #1966

[1]: https://docs.rs/lexopt/0.3.0/lexopt/index.html

---

_Merged by @BurntSushi on 2023-11-21 04:51_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---

_Branch deleted on 2023-11-21 04:51_

---

_Renamed from "WIP" to "various rollup + move off of Clap" by @BurntSushi on 2023-11-21 04:52_

---

_Renamed from "various rollup + move off of Clap" to "various rollup + move off of Clap to lexopt" by @BurntSushi on 2023-11-21 04:52_

---

_Comment by @ruuda on 2024-06-23 16:00_

[This change bumped the minimum Rust version tested on CI from 1.72.1 to 1.74](https://github.com/BurntSushi/ripgrep/commit/082245dadb3854238e62b91aa95a46018cf16ef1#diff-b803fcb7f17ed9235f1e5cb1fcd2f5d3b2838429d4368ae4c57ce4436577f03fR56), but it did not update [the MSRV in Cargo.toml](https://github.com/BurntSushi/ripgrep/blob/c9ebcbd8abe48c8336fb4826df7e9b6fb179de03/Cargo.toml#L27) and the version mentioned in the readme. Current `master` does still compile with 1.72.1. Was it intentional to bump the version pinned on CI?

---

_Comment by @BurntSushi on 2024-06-23 16:04_

The version in CI is ultimately what is tested, so is probably my intent. Otherwise, I don't remember. In general, ripgrep tracks the latest version of Rust and relying on `master` building on anything other than latest stable Rust is a precarious position.

---

_Comment by @ruuda on 2024-06-23 17:09_

So, would you accept a PR to bump the MSRV to match what is tested on CI? It seems dangerous to specify an older version than what is tested. (Or alternatively, test with 1.72.1 again on CI. I thought that maybe the Cargo sparse registry format was the reason to update because it’s a lot faster, but that has been the default since 1.70 so that can’t have been the reason.)

---

_Comment by @BurntSushi on 2024-06-23 17:16_

I guess so? Like I said, in practice, I track the latest version of stable Rust.

I worry about MSRV a lot for ecosystem libraries. I very intentionally worry about it a lot less for CLI programs. It's mostly just a distraction and I'm honestly burnt out from talking about it.

---
