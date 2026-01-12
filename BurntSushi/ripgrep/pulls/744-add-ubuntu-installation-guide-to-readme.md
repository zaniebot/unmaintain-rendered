```yaml
number: 744
title: Add Ubuntu installation guide to README
type: pull_request
state: merged
author: cstorres
labels: []
assignees: []
merged: true
base: master
head: readme-improvements
created_at: 2018-01-12T15:11:20Z
updated_at: 2018-01-13T13:37:17Z
url: https://github.com/BurntSushi/ripgrep/pull/744
synced_at: 2026-01-12T18:23:13Z
```

# Add Ubuntu installation guide to README

---

_@cstorres_

I added documentation for Ubuntu users who want to install `ripgrep` using the snap tool

---

_Review comment by @BurntSushi on `README.md`:228 on 2018-01-12 15:13_

Could you please wrap this to 79 columns inclusive like the rest of the README?

Also, do Ubuntu users need to install `snap` themselves, or is it on 16.04 by default?

Finally, could you remove the reference to Ubuntu above? (Since we know show how to install it.)

---

_@BurntSushi requested changes on 2018-01-12 15:13_

Thanks! I have a few nits.

---

_@cstorres reviewed on 2018-01-12 15:39_

---

_Review comment by @cstorres on `README.md`:228 on 2018-01-12 15:39_

Yes of course. I wrapped the lines to 79 and add some clarifications for the Ubuntu versions. `snap` is installed by default in Ubuntu 16.04 and later versions.

What I didn't understand is what do you mean with this:
> Finally, could you remove the reference to Ubuntu above? (Since we know show how to install it.)

Thanks for the feedback! :)

---

_@BurntSushi reviewed on 2018-01-12 15:53_

---

_Review comment by @BurntSushi on `README.md`:228 on 2018-01-12 15:53_

Just search for mentions of `Ubuntu`. The README says that Ubuntu isn't mentioned below, but this PR adds instructions for Ubuntu, so that section is now outdated. :-)

---

_@BurntSushi reviewed on 2018-01-12 18:11_

---

_Review comment by @BurntSushi on `README.md`:231 on 2018-01-12 18:11_

Could you please add a period to the end of this line? Thanks.

---

_@cstorres reviewed on 2018-01-12 19:05_

---

_Review comment by @cstorres on `README.md`:231 on 2018-01-12 19:05_

Of course. Added :)

---

_Merged by @BurntSushi on 2018-01-12 23:44_

---

_Closed by @BurntSushi on 2018-01-12 23:44_

---

_Comment by @BurntSushi on 2018-01-12 23:44_

@cstorres Thanks! And thanks for your patience with my nits. :)

---

_Comment by @cstorres on 2018-01-13 13:37_

I should thank you for letting me contribute with your project :) your nits
were perfectly reasonable, so I can't complain :)

On Jan 12, 2018 20:44, "Andrew Gallant" <notifications@github.com> wrote:

> @cstorres <https://github.com/cstorres> Thanks! And thanks for your
> patience with my nits. :)
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/pull/744#issuecomment-357385141>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AFYTMOyX7T4mMg6r1kTYhPhnZ4xz9sVUks5tJ-5vgaJpZM4Rca8A>
> .
>


---
