# handbook
Living styleguide and development best practices for Blackbird Studios.

## Table of Contents

- [Workflow](#workflow)
- [Infrastructure](#infrastructure)
- [Useful Processes](#useful-processes)

## Workflow

- GitHub branches should be utilized as follows:
	- `master`: "Stable" code. Usually corresponds to ***production*** environments. Developers should not push new features to this branch. Ideally, this branch should only be pushed to after code has been thoroughly tested.
	- `dev`: The "working" branch for any given project. Developers can push small changes to this branch, but for larger features, a separate branch should be made and then merged in. Usually corresponds to ***development*** and/or ***staging*** environments depending on the project.
	- `staging`: This branch is useful for larger projects, but most likely not applicable to most of the prototypes we build. The ***staging*** environment can also be considered as ***pre-production***. This is a good branch to run tests on before deploying.

- For larger projects, we have **continuous integration** set up with **Jenkins**.

- External resources:
	- http://guides.beanstalkapp.com/deployments/best-practices.html
	- https://blog.codeship.com/continuously-deploying-single-page-apps/

## Infrastructure

- Blackbird uses Amazon Web Services for most of our infrastructure.
- A typical *EC2 instance*:
	- Ubuntu Server 14.04 LTS, 64-bit
	- t2.micro, 1GB memory

## Useful Processes

### Set Up GitHub Deploys Using Git Hooks

Inspired by this Digital Ocean guide: https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps

#### Remote Machine

1. SSH into your remote server and make sure that `git` is installed:
```
sudo apt-get update && sudo apt-get install git
```

1. If they don't exist already, create `/var/www` (for build files) and `/var/repo` (for source files) and `cd` into `/var/repo`. Create a folder within `/var/repo` for your code to live. Initialize that folder as a bare `git` repository.
```
mkdir {/var/www,/var/repo}
mkdir {/var/www/[PROJECT_NAME],/var/repo/[PROJECT_NAME].git}
cd /var/repo/[PROJECT_NAME].git
git init --bare
```

1. Now let's create our `post-receive` hook:
```
cd hooks
cat > post-receive
```

1. And configure our shell script (replace `[PROJECT_NAME]` with your repo or site name). You can also add any other shell commands you'd like to run on deploy to this script:
```
#!/bin/sh
git --work-tree=/var/www/[PROJECT_NAME] --git-dir=/var/repo/[PROJECT_NAME].git checkout -f
```

1. `ctrl+d` to save, and set up the proper permissions:
```
chmod +x post-receive
```

1. Also, make sure that both directories are owned by the `ubuntu` user:
```
sudo chown -R ubuntu /var/www/[PROJECT_NAME]
sudo chown -R ubuntu /var/repo/[PROJECT_NAME].git
```

#### Local Machine

1. Ok, now we need to switch over to our dev machine (the machine we'll be deploying from). `cd` to the git repo that you want to push to your remote server and add a new remote called `live`:
```
git remote add live ssh://ubuntu@[IP_ADDRESS]/var/repo/[PROJECT_NAME].git
```

1. And that's it! If all goes well, you can deploy your code to your server:
```
git push live master
```
