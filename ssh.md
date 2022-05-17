# Make a dir to store your keys
And set the correct permissions
```bash
mkdir ~/.ssh && chmod 700 ~/.ssh
```

# Make keys for github
```bash
ssh-keygen -t ed25519 -C "your_github_email@example.com"
```

Press enter when prompted for file name to save the keys in sefault location (`~/.ssh`)

Create a passphrase

# ssh-agent

## Start the ssh agent
```bash
# start the ssh-agent in the background
eval "$(ssh-agent -s)"

# you should see something like 
# > Agent pid 59566
```

## Add your SSH private key to the ssh-agent
```bash
# use the name you created if it is not this one
ssh-add ~/.ssh/id_ed25519
```

[Add ssh key to github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

## Start SSH Agent on login

In your `~/.profile` or `~/.bash_profile`:
```bash
# start ssh-agent and load ssh keys on login
if [ -z "$SSH_AUTH_SOCK" ] ; then
  eval `ssh-agent -s`
  ssh-add
fi

# kill the ssh-agent on logout
trap 'test -n "$SSH_AUTH_SOCK" && eval `/usr/bin/ssh-agent -k`' 0
```

# Specify SSH key for github 

Create or modify ~/.ssh/config with vim/nano/etc. Enter the path to your private key as the `IdentityFile`
```bash
Host github.com
  User git
  Hostname github.com
  IdentityFile ~/.ssh/id_ed25519
```
