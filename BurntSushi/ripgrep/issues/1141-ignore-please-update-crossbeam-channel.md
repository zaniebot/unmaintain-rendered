```yaml
number: 1141
title: "ignore: Please update crossbeam-channel dependencies to 0.3"
type: issue
state: closed
author: sylvestre
labels:
  - enhancement
assignees: []
created_at: 2018-12-14T14:44:52Z
updated_at: 2018-12-15T13:44:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1141
synced_at: 2026-01-12T16:13:23Z
```

# ignore: Please update crossbeam-channel dependencies to 0.3

---

_@sylvestre_

ignore uses crossbeam-channel 0.2 currently ( https://github.com/BurntSushi/ripgrep/tree/master/ignore )
it would make the life of distro if it could use the most recent upstream release (0.3.2) instead.
Currently, ignore fails with this version with:

> 
> error[E0308]: mismatched types                                                                                                                                                   
>     --> src/walk.rs:1449:17                                                                                                                                                      
>      |                                                                                                                                                                           
> 1449 |                 Some(Message::Work(work)) => {                                                                                                                            
>      |                 ^^^^^^^^^^^^^^^^^^^^^^^^^ expected enum `std::result::Result`, found enum `std::option::Option`                                                           
>      |                                                                                                                                                                           
>      = note: expected type `std::result::Result<walk::Message, channel::TryRecvError>`                                                                                           
>                 found type `std::option::Option<_>`                                                                                                                              
>                                                                                                                                                                                  
> error[E0308]: mismatched types                                                                                                                                                   
>     --> src/walk.rs:1454:17                                                                                                                                                      
>      |                                                                                                                                                                           
> 1454 |                 Some(Message::Quit) => {                                                                                                                                  
>      |                 ^^^^^^^^^^^^^^^^^^^ expected enum `std::result::Result`, found enum `std::option::Option`                                                                 
>      |                                                                                                                                                                           
>      = note: expected type `std::result::Result<walk::Message, channel::TryRecvError>`                                                                                           
>                 found type `std::option::Option<_>`                                                                                                                              
>                                                                                                                                                                                  
> error[E0308]: mismatched types                                                                                                                                                   
>     --> src/walk.rs:1485:17                                                                                                                                                      
>      |                                                                                                                                                                           
> 1485 |                 None => {                                                                                                                                                 
>      |                 ^^^^ expected enum `std::result::Result`, found enum `std::option::Option`                                                                                
>      |                                                                                                                                                                           
>      = note: expected type `std::result::Result<walk::Message, channel::TryRecvError>`                                                                                           
>                 found type `std::option::Option<_>`                                                                                                                              
>                                                                                                                                                                                  
> error: aborting due to 3 previous errors       


---

_Label `enhancement` added by @BurntSushi on 2018-12-14 14:47_

---

_Comment by @igor-raits on 2018-12-15 10:07_

We need this for Fedora asap too :)

---

_Comment by @BurntSushi on 2018-12-15 12:35_

Could someone help me understand why this is time sensitive? Getting this
fixed on master in a reasonable amount of time seems okay, but that doesn't
mean I'm going to also then put out a new release. Releases are a lot of
work, and I cannot be beholded to a release process whose schedule is
dictated by the new release of a dependency.

On Sat, Dec 15, 2018, 05:07 Igor Gnatenko <notifications@github.com wrote:

> We need this for Fedora asap too :)
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1141#issuecomment-447556307>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34p9NCrh4YrAKsbW_d65tLPdSC57Wks5u5MnMgaJpZM4ZTpzO>
> .
>


---

_Comment by @igor-raits on 2018-12-15 12:38_

Because we would like to avoid supporting multiple versions of dependencies.

Also I'm not asking for release -- I'll simply backport patch into Fedora.

---

_Comment by @BurntSushi on 2018-12-15 13:07_

@sylvestre - Is that true for you as well? You don't need a release?

---

_Comment by @sylvestre on 2018-12-15 13:33_

Correct, I already uploaded ignore with this pr

Le sam. 15 déc. 2018 à 14:08, Andrew Gallant <notifications@github.com> a
écrit :

> @sylvestre <https://github.com/sylvestre> - Is that true for you as well?
> You don't need a release?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1141#issuecomment-447567232>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAswjqEz-JTXJaitTbrpx-uSbt3wBwt0ks5u5PQxgaJpZM4ZTpzO>
> .
>


---

_Closed by @BurntSushi on 2018-12-15 13:44_

---

_Comment by @BurntSushi on 2018-12-15 13:44_

OK, this should be all set. I did the fix myself since this wasn't quite right and cleaned up a few other things. I've also put my changes to `ignore` on crates.io in `0.4.5`. Thanks!

---
