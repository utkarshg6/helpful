# SSH Keys

## Create New SSH Keys

### Check whether SSH Keys are already present on your Computer

```zsh
ls -a -l ~/.ssh
```

If `id_rsa` and `id_rsa.pub` are present, that means SSH keys are alerady present. If not then only add new SSH Keys.

### Add New SSH Keys

```zsh
ssh keygen -t rsa -b 4096 -C "{email-id}@gmail.com"
```

Press Enter every time.

## Authenticate SSH Connection to Github

### Print the Public Key inside terminal

```zsh
cat ~/.ssh/id_rsa.pub
```

Copy selected text and add it to Github. Follow these steps:

1. Click Profile Icon
2. Select _Settings_
3. Select _SSH and GPG Keys_
4. Click _New SSH key_
5. Add Title and Copy all the contents of `cat` command
6. Submit _Add SSH Key_

### Authenticate (or Test Old) New SSH Connections

```zsh
ssh -T git@github.com
```

## Add Key to a Git Repository

### Open Git Repository Locally

```zsh
cd {my-git-repository}
```

### Start the SSH agent in the background

```zsh
eval $(ssh-agent -s)
```

Note: Make Sure that the SSH Keys are alerady created.

### Add key to the opened repository

```zsh
ssh-add ~/.ssh/id_rsa
```

### Add Remote to Git Repository

```zsh
git remote add origin https://github.com/{username}/{my-git-repository}.git
```

### Test SSH Connections or Authenticate New Connections

```zsh
ssh -T git@github.com
```

Then type yes to confirm.

### Push Code to Github

```zsh
git push -u origin master
```
