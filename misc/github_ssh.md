# Github SSH

Create a new ssh key pair:

```
> ssh-keygen -t ed25519 -C "comment"
> Enter a file in which to save the key (/home/YOU/.ssh/ALGORITHM): /home/YOU/.ssh/id_ALGORITHM-username
```

Use multiple ssh keys at the same time:

in ~/.ssh/config

```
Host name.github.com   
HostName github.com
User git
IdentityFile ~/.ssh/id_ALGORITHM-username
```
then

```
git clone git@github.com-username:username/some_repo.git
```
