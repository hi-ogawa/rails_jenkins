Assume target server is running on Ubuntu Server 14.04 (ami-a21529cc)

```
$ ansible --version
ansible 2.1.0 (devel 50dfd4b057) last updated 2016/02/07 13:39:55 (GMT +900)
  lib/ansible/modules/core: (detached HEAD e1ec52e365) last updated 2016/02/07 13:41:53 (GMT +900)
  lib/ansible/modules/extras: (detached HEAD 14a62fb5d6) last updated 2016/02/07 13:42:45 (GMT +900)
  config file =
  configured module search path = Default w/o overrides

$ ansible -i ansible/hosts -l odigo-jenkins all -m ping
$ ansible-playbook -i ansible/hosts -l odigo-jenkins ansible/playbook.yml
```

After running ansible, there're some manual work to setup CI for application.

__deploy key__

- make ssh key locally
- register its public key on github repository as deploy key
- go to `/credential-store/domain/_/newCredentials` and register its private key

__github web hook to jenkins__

- go to https://github.com/<username>/<reponame>/settings/hooks and add webhook to jenkins server

__slack integration__

- follow https://slack.com/apps/A0F7VRFKN-jenkins-ci


__project configuration on jenkins__

- click `New Item`
- input `Item name` and choose `Freestyle project`
- choose `GitHub project` and input `Project url`
- choose `Git` under `Source Code Management`, input `Repository URL`, and choose `Crendential` you made above
- choose `Build when a change is pushed to GitHub` under `Build Triggers`
- choose `Execute shell` under `Build` and input shell script to run tests
- choose `Slack Notifications` under `Post-build Actions`
