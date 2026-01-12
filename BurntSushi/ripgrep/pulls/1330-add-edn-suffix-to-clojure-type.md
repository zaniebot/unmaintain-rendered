```yaml
number: 1330
title: Add .edn suffix to Clojure type
type: pull_request
state: merged
author: KingMob
labels: []
assignees: []
merged: true
base: master
head: feature/add-edn-to-clojure-type
created_at: 2019-07-27T01:27:21Z
updated_at: 2019-07-29T21:40:26Z
url: https://github.com/BurntSushi/ripgrep/pull/1330
synced_at: 2026-01-12T18:23:13Z
```

# Add .edn suffix to Clojure type

---

_@KingMob_

EDN is Clojure's JSON equivalent, and used heavily for Clojure configuration files these days.

In response to #1329 

---

_@BurntSushi requested changes on 2019-07-28 18:50_

@KingMob In #1329, you expressed doubt over whether this was desired or not, and I asked you to get some other folks in the Clojure community to give their opinions. Could you please do that?

---

_Comment by @KingMob on 2019-07-29 19:12_

Well, I totally guessed wrong on that one. I asked around, and by a vote of 6-1, more people prefer to keep them separate. I'll update the PR.

---

_Comment by @BurntSushi on 2019-07-29 19:27_

@KingMob Sounds good. Do you perhaps have a link to that vote? If not that's okay, but if so it would be good to include it here for provenance sake.

---

_Comment by @KingMob on 2019-07-29 20:33_

Hmm, I can add some Slack links, but two are private, and the other is on an unpaid plan, so the links will eventually decay, I assume. Want screenshots?

This was nothing so formal as a website and a poll; I asked at the public Clojurians Slack channels #edn and #off-topic, my coworking group's Slack (we have a few Clojurists), and the internal Cognitect Slack.

While not a super-high turnout, people like Alex Miller (a lead dev of Clojure), Billy Meier (author of the clj Liberator web framework), and Michiel Borkent (author of jet, a JSON/EDN/Transit conversion tool) all weighed in, FWIW.

Here's a link to the discussion in the public Clojurians #off-topic channel: https://clojurians.slack.com/archives/C03RZGPG3/p1564419595224700

---

_@BurntSushi approved on 2019-07-29 20:43_

Thanks! That's good enough for me. I'm not involved at all with the Clojure ecosystem, and I'd prefer not changing these mappings once added (but can if necessary), so I just try to do some CYA in less obvious cases. :-)

---

_Merged by @BurntSushi on 2019-07-29 20:43_

---

_Closed by @BurntSushi on 2019-07-29 20:43_

---

_Branch deleted on 2019-07-29 21:40_

---
