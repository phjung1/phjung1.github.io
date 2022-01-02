---
layout: post
title: Set up GitHub push with SSH keys.md
date: 2021-12-18 22:51:00
author: "phjung1"
header-img: "#"
tags:

- gitgub
- ssh

---

## SSH keypair setup for GitHub (or GitHub/GitLab/BitBucket, etc, etc)


### Create a repo.
Make sure there is at least one file in it (even just the README.md)


### Generate a SSH key pair (private/public):
```
ssh-keygen -t rsa -C "your_email@example.com"
```

or even better:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```


### Copy the contents of the public SSH key

macOS:
```
pbcopy < ~/.ssh/id_rsa.pub
```
GNU/Linux (requires the xclip package):
```
xclip -sel clip < ~/.ssh/id_rsa.pub
```
Windows Command Line:
```
type %userprofile%\.ssh\id_rsa.pub | clip
```
Git Bash on Windows / Windows PowerShell:
```
cat ~/.ssh/id_rsa.pub | clip
```

or ofcourse copy it via your favorite editor, cat, or whatever suits your needs :)


### Copy the public SSH key to GitHub
Copy the contents of the to your SSH keys to your GitHub account settings (https://github.com/settings/keys).


### Test the SSH key:
```
ssh -T git@github.com
```

Change directory into the local clone of your repository (if you're not already there) and run:
```
git remote set-url origin git@github.com:username/your-repository.git
```

Now try editing a file (try the README) and then do:
```
git add -A
git commit -am "Update README.md"
git push
```

You should not be asked for a username or password. If it works, your SSH key is correctly configured.
