# git을 통한 배포

node기반 서비스를 git을 통해 배포하는 방법에 대해 설명한다.


## Target Server 설정

### git repository 생성

```
$> mkdir -p ~/mbuz_repos/mbuzzer_server.git
$> cd ~/mbuz_repos/mbuzzer_server.git
$> git init --bare
```

### Hook : post-receive

```bash
#!/usr/bin/env ruby
# post-receive
# 1. Read STDIN (Format: "from_commit to_commit branch_name")
from, to, branch = ARGF.read.split " "
# 2. Only deploy if master branch was pushed
if (branch =~ /master$/) == nil
    puts "Received branch #{branch}, not deploying."
    exit
end
# 3. Copy files to deploy directory
deploy_to_dir = '~/mbuzzer_projects/mbuzdeploy'
`GIT_WORK_TREE="#{deploy_to_dir}" git checkout -f master`
puts "DEPLOY: master(#{to}) copied to '#{deploy_to_dir}'"
# 4.TODO: Deployment Tasks
# i.e.: Run Puppet Apply, Restart Daemons, etc
# stop
# start
```

```chmod +x post-receive```

## Workspace 설정

### edit .ssh/config to register ssh key for git clone or push

```
Host ec2-54-178-164-177.ap-northeast-1.compute.amazonaws.com
   User ubuntu
   IdentityFile ~/.ssh/awsec2.pem
```

### push to repository

```
$> git remote add aws ubuntu@ec2-54-178-164-177.ap-northeast-1.compute.amazonaws.com:~/mbuz_repos/mbuzzer_server.git
$> git push aws master
```

### clone from repository
```
$> git clone ubuntu@ec2-54-178-164-177.ap-northeast-1.compute.amazonaws.com:~/mbuz_repos/mbuzzer_server.git
```

