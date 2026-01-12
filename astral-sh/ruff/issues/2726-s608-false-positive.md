```yaml
number: 2726
title: "S608: false positive"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-02-10T18:19:59Z
updated_at: 2023-02-10T18:48:34Z
url: https://github.com/astral-sh/ruff/issues/2726
synced_at: 2026-01-12T15:54:43Z
```

# S608: false positive

---

_@spaceone_

```python
        print(dedent(
            r"""
            The local repository has been prepared. The repository can be updated using:

              univention-repository-update net

            The local host has been modified to use this local repository.  Other hosts
            must be re-configured by setting the Univention Configuration Registry (UCR)
            variable 'repository/online/server' to the FQDN of this host.

              ucr set repository/online/server="%(hostname)s.%(domainname)s"

            The setting is best set in a domain by defining UCR Policies, which
            set this variable on all hosts using this repository server. For example:

              udm policies/repositoryserver create \
                --position "cn=repository,cn=update,cn=policies,%(ldap/base)s" \
                --set name="%(hostname)s repository" \
                --set repositoryServer="%(hostname)s.%(domainname)s"
              udm container/dc modify \
                --dn "%(ldap/base)s" \
                --policy-reference "cn=%(hostname)s repository,cn=repository,cn=update,cn=policies,%(ldap/base)s"
            """ % configRegistry))

```

is detected as `S608`

---

_Comment by @charliermarsh on 2023-02-10 18:25_

Unfortunately this is also flagged by Bandit so I consider it "up to spec".

---

_Closed by @charliermarsh on 2023-02-10 18:25_

---

_Comment by @spaceone on 2023-02-10 18:32_

→ reported upstream: https://github.com/PyCQA/bandit/issues/984

---

_Comment by @charliermarsh on 2023-02-10 18:48_

Thank you very much :)

---
