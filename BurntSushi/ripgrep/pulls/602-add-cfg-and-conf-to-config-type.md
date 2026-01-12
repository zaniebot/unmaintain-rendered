```yaml
number: 602
title: "Add `*.cfg` and `*.conf` to `config` type"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/type/config
created_at: 2017-09-08T21:18:14Z
updated_at: 2017-10-08T12:05:29Z
url: https://github.com/BurntSushi/ripgrep/pull/602
synced_at: 2026-01-12T18:23:13Z
```

# Add `*.cfg` and `*.conf` to `config` type

---

_@okdana_

Unless i'm misunderstanding the purpose/history of the `config` type, the lack of `*.conf` seems like a pretty big miss here, since it is by far the most common extension for config files on UNIX systems — see just about anything in `/etc` for example. `*.cfg` is less common, but also used a lot (`grub.cfg` and so on).

## Other musings

* Are you happy with the structure/formatting of the type list? Some of the lines exceed 80 cols, for example. And then some of the types are broken into multiple lines even though there are only like three entries. Is there any formatting work i can do on this for you? Would it be easier to read if each pattern was on its own line? Or is that too much vertical space?

* Should i add an `ini` (e.g., `php.ini`)?

* Given that `zsh` has zsh profile/`rc` files, should i also add `.profile`, `.bash_profile`, `.bashrc`, &c., for `sh`?

* On the subject of shell types, it's kind of weird: `fish` gets its own type, which makes sense to me, since it's out doing its own thing. But `zsh` having its own type when it's a more-or-less Bourne/ksh-descended shell just like bash is a little strange, especially when you consider that bizarro incompatible shells like tcsh are included with bash.

* Should i add `*.psql` (PostgreSQL) for `sql`?

* Should i add a `systemd`? systemd has loads of different extensions: `*.conf`, `*.service`, `*.target`, &c.

* Should i add a `license` or similar for `LICEN[CS]E`, `COPYING`, &c.?

* I'm thinking `*.xsl` belongs with `xml`?

* Might consider `*.xml.dist` for `xml` too — this is common in the PHP world.

* If we're considering lock files (`Cargo.lock`) i suppose we should probably also add the ones for Composer, Yarn, &c.?

Let me know if i'm getting all over-engineer-y 😦 

---

_Comment by @BurntSushi on 2017-09-18 16:27_

> Unless i'm misunderstanding the purpose/history of the config type, the lack of *.conf seems like a pretty big miss here, since it is by far the most common extension for config files on UNIX systems 

Agreed! For the most part, I tend to just rely on folks to come up with the right types/globs. My only real requirement is that there's some semblance of ubiquity to the type.

> Are you happy with the structure/formatting of the type list? Some of the lines exceed 80 cols, for example. And then some of the types are broken into multiple lines even though there are only like three entries. Is there any formatting work i can do on this for you? Would it be easier to read if each pattern was on its own line? Or is that too much vertical space?

I definitely prefer 79 columns for everything. I think the ideal case is that CI will run a column checker because it's hard to see line length violations in PR diffs. The problem is that applying a column limit to literally every line isn't feasible since some lines are OK going over 79 columns. e.g., Lines that contain URLs. So you wind up needing a whitelist of some sort, and I've just never gotten around to doing that.

So in that sense, I've mostly just let it drift. If I ever touch that file, then I try to clean up the formatting. I'd probably try to fit most types on one line, but if they don't fit, I'd just insert manual breaks probably similar to [this](https://github.com/BurntSushi/ripgrep/blob/1136f8adabb694634e6bc0c5ca3842e9403fe697/ignore/src/types.rs#L131-L134). Although, I'd try to fill up the first line first.

> Should i add an ini (e.g., php.ini)?

Yeah that seems fine. `ini` files are generally config files in my experience.

> Given that zsh has zsh profile/rc files, should i also add .profile, .bash_profile, .bashrc, &c., for sh?

Hmm. I'm not actually sure. If I search for `-tsh`, should it search a bash specific file? I kind of feel like there's no obvious answer here. Happy to defer to your opinion.

> On the subject of shell types, it's kind of weird: fish gets its own type, which makes sense to me, since it's out doing its own thing. But zsh having its own type when it's a more-or-less Bourne/ksh-descended shell just like bash is a little strange, especially when you consider that bizarro incompatible shells like tcsh are included with bash.

Hmm yeah. The problem is that I've intentionally let this list grow organically because I knew I personally wouldn't be familiar with every extension and therefore would not be able to sort them all correctly anyway. I'd generally prefer not *removing* extensions from types, but I think I'm generally OK with adding extensions to types. So I'd be OK leaving the `zsh` type for example but also adding it to the `sh` type.

> Should i add `*.psql` (PostgreSQL) for `sql`?

Oh hmm I've never seen the `psql` extension used, but if that's a thing specifically for PostgreSQL then yeah sure that sounds fine.

> Should i add a systemd? systemd has loads of different extensions: *.conf, *.service, *.target, &c.

Yeah. I'd actually find that pretty useful!

> Should i add a license or similar for LICEN[CS]E, COPYING, &c.?

Sounds good.

> I'm thinking `*.xsl` belongs with `xml`?

Isn't that kind of like saying `*.css` belongs with `html`?

> Might consider `*.xml.dist` for `xml` too — this is common in the PHP world.

Sounds good.

> If we're considering lock files (`Cargo.lock`) i suppose we should probably also add the ones for Composer, Yarn, &c.?

That's an interesting one. Sure!

---

_Comment by @okdana on 2017-09-18 16:54_

>Yeah that seems fine. ini files are generally config files in my experience.

Ah, just add to `config` then?

>Hmm. I'm not actually sure. If I search for -tsh, should it search a bash specific file? I kind of feel like there's no obvious answer here. Happy to defer to your opinion.

I'm not sure either tbh. I could see the `sh` type being useful both as an umbrella type including bash- and zsh-specific files, and as a more particular type including only the 'portable' stuff. It *already* includes a bunch of non-portable shell-specific extensions, though, so maybe the decision's already been made? idk.

I think that adding `.profile` should be pretty unobjectionable, at the very least — it's not shell-specific.

>Oh hmm I've never seen the psql extension used, but if that's a thing specifically for PostgreSQL then yeah sure that sounds fine.

[Examples here](https://github.com/search?utf8=%E2%9C%93&q=from+extension%3Apsql&type=Code). On balance it doesn't seem *super* common, though. Could probably live without it if it seems weird 🤷‍♀️

 >Isn't that kind of like saying *.css belongs with html?

Well, CSS is a completely different language from HTML. XSLT is itself expressed in XML. I don't do much with XML/XSL stuff though so i'm not sure how it would fit into people's work-flows. Was just a random thought i had whilst scanning through the file.

---

_Comment by @BurntSushi on 2017-09-18 17:36_

> Ah, just add to config then?

Ya.

> I'm not sure either tbh. I could see the sh type being useful both as an umbrella type including bash- and zsh-specific files, and as a more particular type including only the 'portable' stuff. It already includes a bunch of non-portable shell-specific extensions, though, so maybe the decision's already been made? idk.

> I think that adding .profile should be pretty unobjectionable, at the very least — it's not shell-specific.

Sounds good. I think adding zsh extensions to sh, given what's already there, is fine.

> Examples here. On balance it doesn't seem super common, though. Could probably live without it if it seems weird 🤷‍♀️

Ah yeah that's good enough for me!

> Well, CSS is a completely different language from HTML. XSLT is itself expressed in XML. I don't do much with XML/XSL stuff though so i'm not sure how it would fit into people's work-flows. Was just a random thought i had whilst scanning through the file.

Hmm, right. I guess I'd rather hold off on this one until someone who works with XSLT/XML frequently says they make sense to be grouped. :-)

---

_Comment by @okdana on 2017-09-18 18:09_

K, updated and rebased. In addition to what we already discussed i also made some very minor formatting changes and i added `*.j2` for `jinja` (this is used by Ansible and i think Django).

I'll admit the `license` type i added is rather complex, but i wanted to accurately catch everything that i could find in GitHub's project show-case. I think i managed it, but let me know if you think it's too much. Here's the result of running it on the repo anyway:

```
% target/release/rg -tlicense --files
COPYING
LICENSE-MIT
UNLICENSE
globset/COPYING
globset/LICENSE-MIT
globset/UNLICENSE
grep/COPYING
ignore/COPYING
grep/LICENSE-MIT
ignore/LICENSE-MIT
termcolor/COPYING
termcolor/LICENSE-MIT
termcolor/UNLICENSE
grep/UNLICENSE
ignore/UNLICENSE
wincolor/COPYING
wincolor/LICENSE-MIT
wincolor/UNLICENSE
```

I didn't mess with any tests because it doesn't seem like we test specific types — let me know if i missed something though.

---

_@BurntSushi approved on 2017-10-08 12:05_

This LGTM! Those license globs are crazy. :-)

*idly wonders when we'll start noticing startup lag from compiling file type filtering rules*

---

_Merged by @BurntSushi on 2017-10-08 12:05_

---

_Closed by @BurntSushi on 2017-10-08 12:05_

---
