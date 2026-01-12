```yaml
number: 1820
title: I believe the Windows release workflow is broken
type: issue
state: closed
author: commonquail
labels:
  - question
assignees: []
created_at: 2021-03-14T20:22:29Z
updated_at: 2021-06-01T01:51:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1820
synced_at: 2026-01-12T16:13:24Z
```

# I believe the Windows release workflow is broken

---

_@commonquail_

#### Describe your bug.

I'm reasonably convinced the release workflow is broken for the `windows-2019` platform, probably since commit 13d77ab6463a55ac4ca604076c1da74661b26d69 which switched to [the new environment files](https://docs.github.com/en/actions/reference/workflow-commands-for-github-actions#environment-files). There has been no release since then to prove or disprove my theory.

When updating the environment, the syntax appears to be sensitive to the executing shell, and the default shell on `windows-2019` is `pwsh`. Therefore, either the filename should be `$env:GITHUB_ENV` or the `shell` should be overridden.

The retrieval syntax appears to be platform agnostic.

[When this release workflow activates Cross](https://github.com/BurntSushi/ripgrep/blob/c7730d1f3a366e42fdd497a1e0db4bf090de415c/.github/workflows/release.yml#L131-L137) the script uses the default shell with the Unix syntax so the `CROSS` and `TARGET_` variables do not get updated on `windows-2019`.

#### What are the steps to reproduce the behavior?

The job definition

```yaml
  set-env-on-windows:
    name: set-env-on-windows
    runs-on: windows-2019
    env:
      T_SET_ENV_WITH_GLOBAL: "yes"
    steps:
      - name: set-default
        run: printf 'T_SET_ENV_WITH_DEFAULT=%s\n' "yes" >> $GITHUB_ENV
      - name: set-bash
        shell: bash
        run: printf 'T_SET_ENV_WITH_BASH=%s\n' "yes" >> $GITHUB_ENV
      - name: set-ps
        shell: pwsh
        run: printf 'T_SET_ENV_WITH_PWSH=%s\n' "yes" >> $env:GITHUB_ENV

      - name: get-default
        run: printenv | grep T_SET_
      - name: get-bash
        shell: bash
        run: printenv | grep T_SET_
      - name: get-ps
        shell: bash
        run: printenv | grep T_SET_
```

produces the output

```
2021-03-14T20:05:58.5118148Z ##[section]Starting: Request a runner to run this job
2021-03-14T20:05:58.6883178Z Can't find any online and idle self-hosted runner in current repository that matches the required labels: 'windows-2019'
2021-03-14T20:05:58.6883259Z Can't find any online and idle self-hosted runner in current repository's account/organization that matches the required labels: 'windows-2019'
2021-03-14T20:05:58.6883470Z Found online and idle hosted runner in current repository's account/organization that matches the required labels: 'windows-2019'
2021-03-14T20:05:58.8104618Z ##[section]Finishing: Request a runner to run this job
2021-03-14T20:06:04.5087304Z Current runner version: '2.277.1'
2021-03-14T20:06:04.5294640Z ##[group]Operating System
2021-03-14T20:06:04.5295547Z Microsoft Windows Server 2019
2021-03-14T20:06:04.5295959Z 10.0.17763
2021-03-14T20:06:04.5296276Z Datacenter
2021-03-14T20:06:04.5296618Z ##[endgroup]
2021-03-14T20:06:04.5297143Z ##[group]Virtual Environment
2021-03-14T20:06:04.5297599Z Environment: windows-2019
2021-03-14T20:06:04.5298021Z Version: 20210219.1
2021-03-14T20:06:04.5298744Z Included Software: https://github.com/actions/virtual-environments/blob/win19/20210219.1/images/win/Windows2019-Readme.md
2021-03-14T20:06:04.5299933Z ##[endgroup]
2021-03-14T20:06:04.5301655Z ##[group]GITHUB_TOKEN Permissions
2021-03-14T20:06:04.5302852Z Actions: write
2021-03-14T20:06:04.5303237Z Checks: write
2021-03-14T20:06:04.5303623Z Contents: write
2021-03-14T20:06:04.5303995Z Deployments: write
2021-03-14T20:06:04.5304425Z Issues: write
2021-03-14T20:06:04.5304788Z Metadata: read
2021-03-14T20:06:04.5305341Z OrganizationPackages: write
2021-03-14T20:06:04.5305842Z Packages: write
2021-03-14T20:06:04.5306223Z PullRequests: write
2021-03-14T20:06:04.5306725Z RepositoryProjects: write
2021-03-14T20:06:04.5307178Z SecurityEvents: write
2021-03-14T20:06:04.5307620Z Statuses: write
2021-03-14T20:06:04.5308072Z ##[endgroup]
2021-03-14T20:06:04.5310571Z Prepare workflow directory
2021-03-14T20:06:04.6121512Z Prepare all required actions
2021-03-14T20:06:05.6673134Z ##[group]Run printf 'T_SET_ENV_WITH_DEFAULT=%s\n' "yes" >> $GITHUB_ENV
2021-03-14T20:06:05.6675261Z [36;1mprintf 'T_SET_ENV_WITH_DEFAULT=%s\n' "yes" >> $GITHUB_ENV[0m
2021-03-14T20:06:05.6754739Z shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
2021-03-14T20:06:05.6755473Z env:
2021-03-14T20:06:05.6756336Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:05.6756779Z ##[endgroup]
2021-03-14T20:06:07.3110439Z ##[group]Run printf 'T_SET_ENV_WITH_BASH=%s\n' "yes" >> $GITHUB_ENV
2021-03-14T20:06:07.3111331Z [36;1mprintf 'T_SET_ENV_WITH_BASH=%s\n' "yes" >> $GITHUB_ENV[0m
2021-03-14T20:06:07.3127574Z shell: C:\Program Files\Git\bin\bash.EXE --noprofile --norc -e -o pipefail {0}
2021-03-14T20:06:07.3128190Z env:
2021-03-14T20:06:07.3128536Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:07.3128940Z ##[endgroup]
2021-03-14T20:06:07.6269265Z ##[group]Run printf 'T_SET_ENV_WITH_PWSH=%s\n' "yes" >> $env:GITHUB_ENV
2021-03-14T20:06:07.6270033Z [36;1mprintf 'T_SET_ENV_WITH_PWSH=%s\n' "yes" >> $env:GITHUB_ENV[0m
2021-03-14T20:06:07.6327201Z shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
2021-03-14T20:06:07.6327677Z env:
2021-03-14T20:06:07.6328268Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:07.6328720Z   T_SET_ENV_WITH_BASH: yes
2021-03-14T20:06:07.6329114Z ##[endgroup]
2021-03-14T20:06:08.2681951Z ##[group]Run printenv | grep T_SET_
2021-03-14T20:06:08.2682625Z [36;1mprintenv | grep T_SET_[0m
2021-03-14T20:06:08.2742327Z shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
2021-03-14T20:06:08.2742959Z env:
2021-03-14T20:06:08.2743362Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:08.2743819Z   T_SET_ENV_WITH_BASH: yes
2021-03-14T20:06:08.2744224Z   T_SET_ENV_WITH_PWSH: yes
2021-03-14T20:06:08.2744610Z ##[endgroup]
2021-03-14T20:06:08.9118350Z T_SET_ENV_WITH_BASH=yes
2021-03-14T20:06:08.9119331Z T_SET_ENV_WITH_GLOBAL=yes
2021-03-14T20:06:08.9120268Z T_SET_ENV_WITH_PWSH=yes
2021-03-14T20:06:08.9378651Z ##[group]Run printenv | grep T_SET_
2021-03-14T20:06:08.9379560Z [36;1mprintenv | grep T_SET_[0m
2021-03-14T20:06:08.9401086Z shell: C:\Program Files\Git\bin\bash.EXE --noprofile --norc -e -o pipefail {0}
2021-03-14T20:06:08.9401844Z env:
2021-03-14T20:06:08.9402675Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:08.9403101Z   T_SET_ENV_WITH_BASH: yes
2021-03-14T20:06:08.9403515Z   T_SET_ENV_WITH_PWSH: yes
2021-03-14T20:06:08.9404337Z ##[endgroup]
2021-03-14T20:06:09.0096778Z T_SET_ENV_WITH_BASH=yes
2021-03-14T20:06:09.0097376Z T_SET_ENV_WITH_GLOBAL=yes
2021-03-14T20:06:09.0098024Z T_SET_ENV_WITH_PWSH=yes
2021-03-14T20:06:09.0292141Z ##[group]Run printenv | grep T_SET_
2021-03-14T20:06:09.0292833Z [36;1mprintenv | grep T_SET_[0m
2021-03-14T20:06:09.0308375Z shell: C:\Program Files\Git\bin\bash.EXE --noprofile --norc -e -o pipefail {0}
2021-03-14T20:06:09.0309205Z env:
2021-03-14T20:06:09.0309594Z   T_SET_ENV_WITH_GLOBAL: yes
2021-03-14T20:06:09.0310051Z   T_SET_ENV_WITH_BASH: yes
2021-03-14T20:06:09.0310457Z   T_SET_ENV_WITH_PWSH: yes
2021-03-14T20:06:09.0310897Z ##[endgroup]
2021-03-14T20:06:09.0981363Z T_SET_ENV_WITH_BASH=yes
2021-03-14T20:06:09.0981952Z T_SET_ENV_WITH_GLOBAL=yes
2021-03-14T20:06:09.0982898Z T_SET_ENV_WITH_PWSH=yes
2021-03-14T20:06:09.1012180Z Cleaning up orphan processes
```

Observe that `T_SET_ENV_WITH_DEFAULT` never appears in the environment.

---

_Comment by @BurntSushi on 2021-03-14 20:47_

Aye thanks. I'll keep this in mind next time I do a release.

---

_Label `question` added by @BurntSushi on 2021-03-14 20:47_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
