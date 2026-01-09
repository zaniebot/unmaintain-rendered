---
number: 760
title: "Help text doesn't line up for options that accept multiple occurrences"
type: issue
state: closed
author: jimmycuadra
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-12-01T00:49:04Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/760
synced_at: 2026-01-07T13:12:19-06:00
---

# Help text doesn't line up for options that accept multiple occurrences

---

_Issue opened by @jimmycuadra on 2016-12-01 00:49_

Clap version: 2.19.0

Options that allow multiple occurrences have "..." appended to them, which results in the help text for those options not lining up with the others. Note the -i and -K options below:

```
OPTIONS:
    -a, --ami <ami>                                EC2 AMI ID to use for all CoreOS instances, e.g. "ami-1234"
        --availability-zone <availability-zone>    Availability Zone for etcd instances and EBS volumes, e.g. "us-east-1a"
    -A, --aws-account-id <aws-account-id>          The numeric ID of the AWS account, e.g. "123456789012"
    -d, --domain <domain>                          The base domain name for the cluster, e.g. "example.com"
    -i, --iam-user <iam-user>...                      An IAM user name who will have access to cluster PKI secrets, e.g. "alice"; this option can be specified more than once
    -v, --kubernetes-version <k8s-version>         Version of Kubernetes to use, e.g. "1.0.0"
        --masters-max-size <masters-max-size>      The maximum number of EC2 instances the Kubernetes masters may autoscale to
        --masters-min-size <masters-min-size>      The minimum number of EC2 instances the Kubernetes masters may autoscale to
        --nodes-max-size <nodes-max-size>          The maximum number of EC2 instances the Kubernetes nodes may autoscale to
        --nodes-min-size <nodes-min-size>          The minimum number of EC2 instances the Kubernetes nodes may autoscale to
        --rbac-super-user <rbac-super-user>        The Kubernetes username of an administrator who will set up initial RBAC policies, e.g. "jimmy"
    -r, --region <region>                          AWS Region to create the resources in, e.g. "us-east-1"
    -s, --instance-size <size>                     EC2 instance size to use for all instances, e.g. "m3.medium"
    -K, --ssh-key <ssh-key>...                        SSH public key to add to ~/.ssh/authorized_keys on each server; this option can be specified more than once
    -z, --zone-id <zone-id>                        Route 53 hosted zone ID
```

---

_Comment by @kbknapp on 2016-12-02 01:09_

Interesting, this a regression then and I'd thought I tested against it. Let me see if I can reproduce and I'll post back!

---

_Comment by @kbknapp on 2016-12-02 01:58_

@jimmycuadra I'm not able to reproduce this, could you like to the source that's causing it? If you look at `tests/help.rs` maybe you could also see if there's something you're doing differently than the tests?


---

_Comment by @jimmycuadra on 2016-12-02 02:16_

Here is the clap code that produces the help output I pasted: https://github.com/InQuicker/kaws/blob/9c8786aebaca68bff3637973a7c6c8bf54697dda/src/cli.rs

---

_Comment by @kbknapp on 2016-12-02 03:34_

Ah ok, interesting, so it appears it only happens when those args are part of a subcommand. I've reproduced it, and should have it fixed soon 😉 

---

_Label `C: help message` added by @kbknapp on 2016-12-02 03:35_

---

_Label `C: subcommands` added by @kbknapp on 2016-12-02 03:35_

---

_Label `D: easy` added by @kbknapp on 2016-12-02 03:35_

---

_Label `P2: need to have` added by @kbknapp on 2016-12-02 03:35_

---

_Label `T: bug` added by @kbknapp on 2016-12-02 03:35_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-02 03:35_

---

_Added to milestone `2.19.1` by @kbknapp on 2016-12-02 03:35_

---

_Comment by @kbknapp on 2016-12-02 04:13_

The pot thickens, it's not because of subcommands at all. It's because of the line `number_of_values(1)` Should have this fixed momentarily!

---

_Comment by @kbknapp on 2016-12-02 04:55_

This is fixed in #762 and once that merges I'll upload v2.19.1 to crates.io

---

_Comment by @jimmycuadra on 2016-12-02 08:27_

You're amazing, as always. ❤️ 

---

_Closed by @homu on 2016-12-02 14:00_

---
